# Write-up Template

### Analyze, choose, and justify the appropriate resource option for deploying the app.

I've chosen App Services and decided against a Virtual Machine, because

* the application is a simple web application whichi for now does not need much resources.
* using App Services, there is no need to manage the underlying infrastructure. VM's require much more configuration efforts and are less easier to manage.
* the deloyment is very simple and fast. Integration with GitHub is very easy.
* the required runtime stack (Python) is available out-of-the-box.
* App Services have autoscaling and load-balancing built-in.
* App Services have many availability zones out-of-the-box.
* a rough cost comparison shows that it is cheaper (but I'm not super sure).

### Assess app changes that would change your decision.

* The app requires technologies and software which are not available in Azure as a service.
* The runtime stack is not supported by App Services.
* The application requires multiple machines, e.g. for big-data processing.
* The app deployment requires more control and debugging capabilities.

### Sourcecode

My forked public GitHub repository is this: `https://github.com/thduerr/nd081-c1-provisioning-microsoft-azure-vms-project-starter`

My webapp is available online for some days using this link: `http://thduerr-cms.azurewebsites.net/`

