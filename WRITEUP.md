# Write-up Template

### Analyze, choose, and justify the appropriate resource option for deploying the app.

The application is a simple web application which does not need much resources.

I've chosen App Services and decided against a VM, because

* using App Services, there is no need to manage the underlying infrastructure. VM's require much more configuration efforts and are less easier to manage.
* the deloyment is very simple and fast.
* the required runtime stack (Python) is available out-of-the-box.
* App Servies has autoscaling and load-balancing built-in.
* App Services has more availability zones.
* a rough cost comparison shows that it is cheaper

### Assess app changes that would change your decision.

* The app requires technologies which are not available in Azure as a service.
* The runtime stack is not supported by App Services.
* The application requires multiple machines, e.g. for big-data processing.
* The app deployment requires more control and debugging capabilities.

### Online Webapp

The forked GitHub repository is: `https://github.com/thduerr/nd081-c1-provisioning-microsoft-azure-vms-project-starter`

My webapp is available online for one day using this link: `http://thduerr-cms.azurewebsites.net/`

