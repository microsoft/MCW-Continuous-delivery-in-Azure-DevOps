# Continuous delivery in VSTS and Azure

Tailspin Toys saw the positive potential of the cloud and moved its IT infrastructure, with no significant re-architecture, into Microsoft Azure around six months ago. Now that its business is running successfully in the cloud, it has started a series of process improvements to become a more agile company with a specific focus on delivering frequent feature updates and fixes to its public website. Alex Montgomery, VP of sales and the head of its online business team, says that, "Even though our products are better, our competitors are generating more online sales than we are. For every feature that we deliver to our website, they have delivered 2 or 3. Our development processes are too cumbersome and slow for us to build quality code at that pace."

When it moved its existing systems into Microsoft Azure, Tailspin Toys decided to use the Azure App Service to host its public website which is written as an ASP.NET Model View Controller MVC application. For the database (back-end) tier, they chose the Azure SQL Database service for full Platform-as-a-Service agility.

Tailspin wants to improve the turnaround time for fixing these bugs, and they need better logs for the developers. Tailspin needs a solution to gather new types of logs including browser errors and application dependency errors such as timeouts. Ideally, they want to make the application logs searchable as well as implement an automated warning system that emails alerts when application behavior is unusually slow or problematic.


## Target audience

- Application Developer

## Abstract

### Workshop

In this workshop, you will learn how to setup and configure continuous delivery within Azure using a combination of Azure Resource Manager templates and Visual Studio Team Services (VSTS). You will do this throughout the use of a new VSTS project, Git repository for source control, and an Azure Resource Manager template for Azure resource deployment and configuration management.

At the end of this workshop, you will be able to build templates to automate cloud infrastructure and reduce error-prone manual processes. In addition,  you'll create an Azure Resource Manager (ARM) template to provision Azure resources, configure continuous delivery with Visual Studio Team Services (VSTS), configure Application Insights into an application, and create a Visual Studio Team Services project and Git repository.

### Whiteboard design session

In this whiteboard design session, you will learn how to design a solution with a combination of Azure Resource Manager templates and Visual Studio Team Services (VSTS) to enable continuous delivery with several Azure PaaS services.

At the end of this workshop, you will be better able to build templates to automate cloud infrastructure and reduce error-prone manual processes. In addition, you'll create an Azure Resource Manager (ARM) template to provision Azure resources, configure continuous delivery with VSTS, configure Application Insights into an application, and create a Visual Studio Team Services project and Git repository.

### Hands-on Lab

In this hands-on lab, you will learn how to implement a solution with a combination of Azure Resource Manager templates and Visual Studio Team Services (VSTS) to enable continuous delivery with several Azure PaaS services.

At the end of this workshop, you will be better able to implement solutions for continuous delivery with VSTS in Azure, as well create an Azure Resource Manager (ARM) template to provision Azure resources, configure continuous delivery with VSTS, configure Application Insights into an application, and create a Visual Studio Team Services project and Git repository.

## Azure services and related products
- Azure App Service 
- Azure SQL Database
- Application Insights
- Azure Resource Manager
- Visual Studio Team Services 

## Azure solutions
DevOps

## Related references
- [MCW](https://github.com/Microsoft/MCW)
- [DevOps Checklist](https://docs.microsoft.com/en-us/azure/architecture/checklist/dev-ops)
- [Reference Architecture for Basic Web Application](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/app-service-web-app/basic-web-app)
