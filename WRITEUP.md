Analyze, choose, and justify the appropriate resource option for deploying the app
----------------------------------------------------------------------------------

# STARTING POINT

What do I have?

* a simple Flask web-application (CMS) to be run in a Python runtime environment
* a relational database for user- and content-data ( --> is already running as Azure SQL database)
* a file storage for content assets and logfiles ( --> is already running as Azure Blob storage)
* an external identity provider ( --> is already available as Azure Active Directory)

Problem:

How to deploy and run the given web-application on a server?

I have two options:
1. Azure App Services
2. Azure Virtual Machine



# ANALYSIS

The following analysis compares the pros and cons of each option and explores the cost aspect in particular.


## Azure App Service

App Service is a PaaS solution that allows a developer to focus on the application while Azure takes care of the infrastructure

PRO:
* no need to setup and manage the underlying infrastructure
* fast provisioning of a Python runtime
* easy deployment workflow and integration to external SCM hosting
* deployment slots
* monitoring and diagnostics out-of-the-box 
* load balancing is integrated
* auto-scaling is build-in
* high availability as services managed and monitored

CONTRA:
* limited configuration and optimization options
* limited possibilities for debugging and finding root-causes if something does not work as expected
* only pre-defined workflows
* only pre-defined runtime stacks


## Azure Virtual Machine

Azure Virtual Machines is a IaaS service by allowing you to create and use virtual machines in the cloud.

PRO:
* full control on the server infrastructure
* being able to install any software and technology stack
* allows to run legacy software and services which are not available as a Azure service
* run background processes (e.g. automatic renewal of Let's-Encrypt SSL certificates)
* easier debugging and troubleshooting
* workflow agnostic

CONTRA:
* more control comes with more responsibility (e.g. server security)
* more operational efforts
* no web server out-of-the-box
* manual setup of a deployment workflow
* requires knowledge of server management and OS systems
* auto-scaling is not build-in
* load balancing is not build-in



## Cost comparison

Regarding server performance, let's have the following assumptions:

* no heavy computation
* medium-sized database
* mid volume server traffic

As database and storage costs are the same for both options, I only compare prices for VM vs. App-Service


  Component        | Azure App Service          | Azure Virtual Machine
 ------------------+----------------------------+--------------------------
  Server           | P1V2: ~ 60 EUR / month     | DS1 v2: ~ 45 EUR / month
  Database         | +                          | +
  Storage          | +                          | +
  Load Balancer    | -                          | +
  Development      | F1 Free tier: 0 EUR        | B1LS: ~ 3 EUR / month
  Operations       | -                          | +



# CONCLUSION

With the current state of the web-app, I definitely start with the fully managed Azure App Services, and 
switch to VM as soon as I push the limits.

From a cost perspective, using App Services allows to develop an application without starting costs using the F1 Free
tier. Also there are no operational costs and no costs for setting up an infrastructure.

App Services allow to have an agile approach: start early and get fast results without the need to have done the
right decisions upfront.

App Services have build-in all aspects which are important when going to production: scaling, availability and security.





Assess app changes that would change your decision
--------------------------------------------------

I would switch to using Azure Virtual Machine, if

* the application is a legacy application and requires technologies and software which are not available 
  in the App Service runtime stack.
* the application is more complex and consists of different services and processes which have different requirements on
  computation, storage and traffic. Maybe I would switch to microservices then, instead of VMs.
* the application has technical problems which can't be solved without having full access and full control of
  the infrastructure.
* if the costs of App Services grow faster compared to VMs in case of increasing traffic, storage and
  computation.
* the development team is large and requires more flexible and individual workflows and deployment processes.

