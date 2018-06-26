![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Continuous delivery in VSTS and Azure
</div>

<div class="MCWHeader2">
Hands-on lab unguided
</div>

<div class="MCWHeader3">
April 2018
</div>



Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.
© 2018 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents** 

<!-- TOC -->

- [Abstract and learning objectives](#abstract-and-learning-objectives)
- [Overview](#overview)
- [Solution architecture](#solution-architecture)
- [Requirements](#requirements)
- [Exercise 1: Create an Azure Resource Manager (ARM) template that can provision the web application, SQL database, and deployment slots in a single automated process.](#exercise-1-create-an-azure-resource-manager-arm-template-that-can-provision-the-web-application-sql-database-and-deployment-slots-in-a-single-automated-process)
- [Exercise 2: Create a Visual Studio Team Services team project and Git Repository](#exercise-2-create-a-visual-studio-team-services-team-project-and-git-repository)
- [Exercise 3: Create Visual Studio Team Services build definition](#exercise-3-create-visual-studio-team-services-build-definition)
    - [Tasks to complete](#tasks-to-complete)
    - [Exit criteria](#exit-criteria)
- [Exercise 4: Create Visual Studio Team Services release pipeline](#exercise-4-create-visual-studio-team-services-release-pipeline)
- [Exercise 5: Trigger a build and release](#exercise-5-trigger-a-build-and-release)
- [Exercise 6: Create a feature branch and submit a pull request](#exercise-6-create-a-feature-branch-and-submit-a-pull-request)
- [After the hands-on lab](#after-the-hands-on-lab)
    - [Task 1: Delete Resources](#task-1-delete-resources)

<!-- /TOC -->

## Abstract and learning objectives 

In this workshop, attendees will learn how to setup and configure continuous delivery within Azure using a combination of Azure Resource Manager (ARM) templates and Visual Studio Team Services (VSTS). Attendees will do this throughout the use of a new VSTS project, Git repository for source control, and an ARM template for Azure resource deployment and configuration management.

Attendees will be better able to build templates to automate cloud infrastructure and reduce error-prone manual processes. In addition,

-   Create an Azure Resource Manager (ARM) template to provision Azure resources

-   Configure continuous delivery with Visual Studio Team Services (VSTS)

-   Configure Application Insights into an application

-   Create a Visual Studio Team Services project and Git repository

## Overview

Tailspin Toys has asked you to automate their development process in two specific ways. First, they want you to define an Azure Resource Manager template that can deploy their application into the Microsoft Azure cloud using Platform-as-a-Service technology for their web application and their SQL database. Second, they want you to implement a continuous delivery process that will connect their source code repository into the cloud, automatically run their code changes through unit tests, and then automatically create new software builds and deploy them onto environment-specific deployment slots so that each branch of code can be tested and accessed independently.

## Solution architecture

![Image that shows the pipeline for checking in code to Visual Studio tTeam Services that goes through automated build and testing with release management to production.](images/Hands-onlabunguided-ContinuousdeliverywithVSTSandAzureimages/media/image2.png "Solution architecture")

## Requirements

1.  Microsoft Azure subscription

2.  Local machine or a virtual machine configured with:

    -   Visual Studio Community 2017

    -   Git command-line interface (CLI)


## Exercise 1: Create an Azure Resource Manager (ARM) template that can provision the web application, SQL database, and deployment slots in a single automated process.

Duration: 45 Minutes

Tailspin Toys has requested an Azure environment consisting of the following resources:

-   App Service

    -   Web App

        -   Auto-scale rule

    -   Deployment slots (for zero-downtime deployments)

-   SQL Server

    -   SQL Database

-   Application Insights

Since this solution is based on Azure Platform-as-a-Service (PaaS) technology, it should take advantage of that platform by utilizing automatic scale for the web app and the SQL Database PaaS service instead of SQL Server virtual machines.

Tasks to complete

-   Create an Azure Resource Manager (ARM) template file using Visual Studio

-   Add an Azure SQL Database and Server to the template

-   Add a web hosting plan and web app to the template

-   Add Application Insights as to the template

-   Configure automatic scale for the web app in the template

-   Configure the list of release environments parameters

-   Configure the name of the web app using the environments parameters

-   Add a deployment slot for the "staging" version of the site

-   Create the dev environment and deploy the template to Azure

-   Create the test environment and deploy the template to Azure

-   Create the production environment and deploy the template to Azure

Exit criteria

-   You have a completed ARM template and parameters file saved and committed to source control

-   You have deployed the completed template to Azure that creates the three environments: dev, test, and production.

## Exercise 2: Create a Visual Studio Team Services team project and Git Repository

Duration: 15 Minutes

Tailspin Toys has asked you to create a continuous delivery process for their development team. To do that without disrupting their current production application, you are tasked with creating an environment in Azure where this new process and workflow can be proven. Your first task is to create a Visual Studio Team Services Team Project with Git source control where their source code can be maintained.

Tasks to complete

-   Create Visual Studio Team Services account

-   Add the Tailspin Toys source code repository to Visual Studio Team Services

Exit criteria

-   You can connect Visual Studio to the Visual Studio Team Services account, clone and view the web app repository from the master branch

## Exercise 3: Create Visual Studio Team Services build definition

Duration: 15 Minutes

In this exercise, you will create a build definition in Visual Studio Team Services (VSTS) that automatically builds the web application with every commit of source code. This will lay the groundwork for us to then create a release pipeline for publishing the code to our Azure environments.

### Tasks to complete

-   Create a build definition

    -   Build the web app

    -   Execute unit tests

    -   Publish artifacts

-   Enable continuous integration

### Exit criteria

-   An automated build is triggered with each commit of code

## Exercise 4: Create Visual Studio Team Services release pipeline

Duration: 30 Minutes

In this exercise you will create a release pipeline in Visual Studio Team Services that performs automated deployment of build artifacts to Microsoft Azure. The release pipeline will deploy to three environments: dev, test, and production.

Tasks to complete

-   Create a release definition

    -   Link the build artifacts to the release

    -   Sequential automated flow to three environments: dev, test, and production

    -   Deploy to staging slot and then switch the slots after a successful deployment

-   Add test and production environments to release definition

    -   Deploy to staging slot and then switch the slots after a successful deployment

Exit criteria

-   You have created a release pipeline that includes three sequential environments: dev, test, and production. The deployments to dev are automatically triggered upon a successful build. The deployment to test is automatically triggered upon a successful deployment to dev. The deployment to production is automatically triggered upon a successful deployment to production.

## Exercise 5: Trigger a build and release

Duration: 10 Minutes

In this exercise, you will trigger an automated build and release of the web application using the build definition and release pipeline you created in earlier exercises. The release pipeline will deploy to three environments: dev, test, and production.

Tasks to complete

-   Manually queue a new build and follow it through the build and release pipeline

Exit criteria

-   You have successfully queued a new build and released it through the three environments: dev, test, and production

## Exercise 6: Create a feature branch and submit a pull request

Duration: 20 Minutes

In this exercise you will create a short-lived feature branch, make a small code change, and submit a pull request. You'll then merge the pull request into the master branch, which will trigger an automated build and release of the application.

Tasks to complete

-   Create a new feature branch

-   Make a code change to the feature branch

-   Commit the code change

-   Submit a pull request

-   Approve and complete a pull request

Exit criteria

-   You have successfully created a branch, made a code change, submitted a pull request, approved the pull request, and merged the code

## After the hands-on lab 

You should follow all steps provided *after* attending the hands-on lab

### Task 1: Delete Resources

1.  Now since the hands-on lab is complete, go ahead and delete all of the Resource Groups that were created for this Hackathon. You will no longer need those resources and it will be beneficial to clean up your Azure Subscription.

