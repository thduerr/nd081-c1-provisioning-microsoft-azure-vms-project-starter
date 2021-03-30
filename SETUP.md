# Setup

## Python Environment

    python -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt
    pip install python-dotenv
    pip install --upgrade setuptools
    pip install --upgrade pip
    pip install --upgrade distlib
    pip install mssql-cli

## Resource Group

Create a Resource Group in Azure

    az account list-locations -o table
    az group create -n <group> -l <location>

## SQL Database

Create an SQL Server Database in Azure that contains a user table, an article table, and data in each table

    az sql server create -u <user> -p <password> -n <server> -g <group> -e true
    az sql server firewall-rule create -g <group> -s <server> -n azure-access --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
    myip=$(curl -s ipinfo.io/ip)
    az sql server firewall-rule create -g <group> -s <server> -n public-access --start-ip-address $myip --end-ip-address $myip
    az sql server firewall-rule list -g <group> -s <server> -o table
    az sql db create -g <group> -s <server> -n <db> --tier Basic
    az resource list -g <group> -o table

Import Data

    mssql-cli -S <server>.database.windows.net -d <db> -U <user> -P <password> -i sql_scripts/users-table-init.sql
    mssql-cli -S <server>.database.windows.net -d <db> -U <user> -P <password> -i sql_scripts/posts-table-init.sql

Export credentials via `.env`:

    export SQL_SERVER="<server>"
    export SQL_DATABASE="<db>"
    export SQL_USER_NAME="<user>"
    export SQL_PASSWORD="<password>"

## Blob Storage

Create a Storage Container in Azure for `images` to be stored in a container.

    az storage account create -n <account> -g <group> --sku Standard_LRS
    export AZURE_STORAGE_ACCOUNT="<account>"
    export AZURE_STORAGE_KEY=$(az storage account keys list -g <group> -n <account> --query [0].value)
    az storage container create -n <container> --public-access blob

Export credentials via `.env`:

    export BLOB_ACCOUNT="<account>"
    export BLOB_STORAGE_KEY="$AZURE_STORAGE_KEY"
    export BLOB_CONTAINER="<container>"

Test locally

    python application.py
    open https://localhost:5555

  Login with `admin:pass`

In case of Chrome browser SSL warning, enter `thisisunsafe`

## Azure Active Directory

    az ad app create --display-name <app-name>
                     --reply-urls https://localhost:5555/getAToken
                     --required-resource-accesses manifest.json
    USERID=$(az ad signed-in-user show --query objectId -o tsv)
    APPID=$(az ad app list --query "[?displayName=='<app-name>'].appId" -o tsv)
    az ad app owner add --id $APPID --owner-object-id $USERID

  Create new app secret. Make sure you copy this value - it can't be retrieved.

    CLIENT_SECRET=$(az ad app credential reset --id $appid --credential-description "Default" --query password -o tsv)

Export credentials via `.env`:

    export AD_CLIENT_SECRET=$CLIENT_SECRET
    export AD_CLIENT_ID=$APPID

Test locally with Microsoft Account

    python application.py
    open https://localhost:5555

  Login with "Sign in with Microsoft"

## App Service

    az appservice plan create -g <group> -n <plan> --is-linux --sku F1
    az webapp create -n <name> -g <group> -p <plan> -r "PYTHON|3.8"
    while read env; do az webapp config appsettings set -g <group> -n <name> --settings $(echo $env | awk '{print $2}'); done < <(cat .env | sed /^$/d)
    az webapp deployment source config -n <name> -g <group> -u <github-repo-url> --branch master --manual-integration


TODO: update `getAToken` and `login` AD-URLs 

# Q & A

What’s this **SECRET_KEY** in config.py anyway?

The secret key is used by the Flask framework for signing cookies (and hence session data) kind of similar to a “salt”. You can set it to whatever you like, so for non-production code it is totally fine to keep the value as is.

