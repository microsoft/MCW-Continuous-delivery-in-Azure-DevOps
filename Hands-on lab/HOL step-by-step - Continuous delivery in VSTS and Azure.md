![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Continuous delivery in VSTS and Azure
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
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

- [Continuous delivery with VSTS and Azure hands-on lab step-by-step](#continuous-delivery-with-vsts-and-azure-hands-on-lab-step-by-step)
    - [Abstract and learning objectives](#abstract-and-learning-objectives)
    - [Overview](#overview)
    - [Solution architecture](#solution-architecture)
    - [Requirements](#requirements)
    - [Exercise 1: Create an Azure Resource Manager (ARM) template that can provision the web application, SQL database, and deployment slots in a single automated process](#exercise-1-create-an-azure-resource-manager-arm-template-that-can-provision-the-web-application-sql-database-and-deployment-slots-in-a-single-automated-process)
        - [Task 1: Create an Azure Resource Manager (ARM) template using Visual Studio](#task-1-create-an-azure-resource-manager-arm-template-using-visual-studio)
        - [Task 2: Add an Azure SQL database and server to the template](#task-2-add-an-azure-sql-database-and-server-to-the-template)
        - [Task 3: Add a web hosting plan and web app to the template](#task-3-add-a-web-hosting-plan-and-web-app-to-the-template)
        - [Task 4: Add Application Insights to the template](#task-4-add-application-insights-to-the-template)
        - [Task 5: Configure automatic scale for the web app in the template](#task-5-configure-automatic-scale-for-the-web-app-in-the-template)
        - [Task 6: Configure the list of release environments parameters](#task-6-configure-the-list-of-release-environments-parameters)
        - [Task 7: Configure the name of the web app using the environments parameters](#task-7-configure-the-name-of-the-web-app-using-the-environments-parameters)
        - [Task 8: Add a deployment slot for the "staging" version of the site](#task-8-add-a-deployment-slot-for-the-staging-version-of-the-site)
        - [Task 9: Create the dev environment and deploy the template to Azure](#task-9-create-the-dev-environment-and-deploy-the-template-to-azure)
        - [Task 10: Create the test environment and deploy the template to Azure](#task-10-create-the-test-environment-and-deploy-the-template-to-azure)
        - [Task 11: Create the production environment and deploy the template to Azure](#task-11-create-the-production-environment-and-deploy-the-template-to-azure)
    - [Exercise 2: Create Visual Studio Team Services team project and Git Repository](#exercise-2-create-visual-studio-team-services-team-project-and-git-repository)
        - [Task 1: Create Visual Studio Team Services Account](#task-1-create-visual-studio-team-services-account)
        - [Task 2: Add the Tailspin Toys source code repository to Visual Studio Team Services](#task-2-add-the-tailspin-toys-source-code-repository-to-visual-studio-team-services)
    - [Exercise 3: Create Visual Studio Team Services build definition](#exercise-3-create-visual-studio-team-services-build-definition)
        - [Task 1: Create a build definition](#task-1-create-a-build-definition)
        - [Task 2: Enable continuous integration](#task-2-enable-continuous-integration)
    - [Exercise 4: Create Visual Studio Team Services release pipeline](#exercise-4-create-visual-studio-team-services-release-pipeline)
        - [Task 1: Create a release definition](#task-1-create-a-release-definition)
        - [Task 2: Add test and production environments to release definition](#task-2-add-test-and-production-environments-to-release-definition)
    - [Exercise 5: Trigger a build and release](#exercise-5-trigger-a-build-and-release)
        - [Task 1: Manually queue a new build and follow it through the release pipeline](#task-1-manually-queue-a-new-build-and-follow-it-through-the-release-pipeline)
    - [Exercise 6: Create a feature branch and submit a pull request](#exercise-6-create-a-feature-branch-and-submit-a-pull-request)
        - [Task 1: Create a new branch](#task-1-create-a-new-branch)
        - [Task 2: Make a code change to the feature branch](#task-2-make-a-code-change-to-the-feature-branch)
        - [Task 3: Submit a pull request](#task-3-submit-a-pull-request)
        - [Task 4: Approve and complete a pull request](#task-4-approve-and-complete-a-pull-request)
    - [After the hands-on lab](#after-the-hands-on-lab)
        - [Task 1: Delete Resources](#task-1-delete-resources)

<!-- /TOC -->

# Continuous delivery with VSTS and Azure hands-on lab step-by-step

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

![Image that shows the pipeline for checking in code to Visual Studio tTeam Services that goes through automated build and testing with release management to production.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image2.png "Solution architecture")

## Requirements

1.  Microsoft Azure subscription

2.  Local machine or a virtual machine configured with:

    -   Visual Studio Community 2017

    -   Git command-line interface (CLI)



## Exercise 1: Create an Azure Resource Manager (ARM) template that can provision the web application, SQL database, and deployment slots in a single automated process

Duration: 45 Minutes

Tailspin Toys has requested three Azure environments (dev, test, production), each consisting of the following resources:

-   App Service

    -   Web App

        -   Auto-scale rule

    -   Deployment slots (for zero-downtime deployments)

-   SQL Server

    -   SQL Database

-   Application Insights

Since this solution is based on Azure Platform-as-a-Service (PaaS) technology, it should take advantage of that platform by utilizing automatic scale for the web app and the SQL Database PaaS service instead of SQL Server virtual machines.

### Task 1: Create an Azure Resource Manager (ARM) template using Visual Studio

1.  Open Visual Studio and create a new project of the type Cloud -- Azure Resource Group. Name the new project "TailspinToys.AzureResourceTemplate" and save it to **C:\\Hackathon**. Also, make sure that both check boxes are checked on the lower right, as in the screen shot below. When finished, select **OK**.
    
    ![In this screenshot of the New Project dialog box, all the options to create the TailspinToys.AzureResourceTemplate project are highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image22.png "Visual Studio New Project dialog box")

2.  On the next window, click **Blank Template**, and select **OK**
    
    ![In the Select Azure Template window, Visual Studio Templates has been selected in the Show templates from this location drop-down list, the Blank Template option is selected and highlighted, and Next is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image23.png "Blank Template selection")

3.  In the Solution Explorer window, open the azuredeploy.json file by double-clicking it
    
    ![In the Solution Explorer window, azuredeploy.json is highlighted under TailspinToys.AzureResourceTemplate.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image24.png "Selecting the azuredeploy.json file")

4.  Then, probably on the left side of the Visual Studio window, open the window called JSON Outline. It will look like this screen shot.
    
    ![The screenshot of the JSON Outline window depicts the following: parameters (0), variables (0), resources (0), and outputs (0).](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image25.png "JSON Outline window")

5.  Save your files

### Task 2: Add an Azure SQL database and server to the template

1.  Right-click on the **resources** item in the **JSON Outline** and select **Add New Resource**
    
    ![In JSON Outline, resources (0) is selected, and a submenu displays a selected and highlighted Add New Resource option.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image26.png "Add New Resource selection")

2.  Select **SQL Server** and give it a name like "tailspinsql", then select **Add**
    \
    ![In the Add Resource window, SQL Server is highlighted on the left, tailspinsql is highlighted next to that in the Name box, and Add is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image27.png "Add Resource window")

3.  Now that the SQL Server has been created as a resource, right-click that SQL Server resource and choose **Add New Resource** so that you can add a database
    
    ![In JSON Outline, under resources (1), tailspinsql is selected, and a submenu displays a selected and highlighted Add New Resource option.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image28.png "Add New Resource selection")

4.  Choose SQL Database, and call it "TailspinData." Make sure that your server is selected in the drop-down list below, and select **Add**.
    
    ![In the Add Resource window, SQL Database is highlighted on the left, TailspinData is highlighted next to that in the Name box, and Add is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image29.png "Add Resource window")

### Task 3: Add a web hosting plan and web app to the template

1.  Add another resource; this time choose **App Service Plan** and call it "TailspinToysHostingPlan". Then select **Add**.
    
    ![In the Add Resource window, App Service Plan (Server Farm) is highlighted on the left, TailspinToysHostingPlan is highlighted next to that in the Name box, and Add is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image30.png "Add Resource window")

2.  Right-click the hosting plan resource and add a new resource underneath it
    
    ![In JSON Outline, under resources (2), TailspinToysHostingPlan is selected, and a submenu displays a selected and highlighted Add New Resource option.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image31.png "Add New Resource selection")

3.  Choose **Web App** and name it "TailspinToysWeb"; make sure your hosting plan is selected in the drop-down list. Then select **Add**.
    
    ![In the Add Resource window, Web App is highlighted on the left, TailspinToysWeb is highlighted next to that in the Name box, and Add is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image32.png "Add Resource window")

### Task 4: Add Application Insights to the template

1.  Add a new resource to the template; this time choose **Application Insights for Web Apps**. Make sure your correct hosting plan and web app are selected in the boxes. Name the Application Insights resource "TailspinToysWeb" and then select **Add**.
    
    ![In the Add Resource window, Application Insights for Web App is highlighted on the left, TailspinToysWeb is highlighted next to that in the Name box, and Add is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image33.png "Add Resource window")

2.  Next, you need to add the Application Insights extension to the App Service so that it will be running automatically once the site is deployed. This is going to require some manual code because there is not a wizard for this resource type. Select the TailspinToysWeb web app resource to locate its JSON code. Then, just below the "properties" property, paste or type in this block of JSON code:
    ```
    "resources": [
        {
        "apiVersion": "2015-08-01",
        "name": "Microsoft.ApplicationInsights.AzureWebSites",
        "type": "siteextensions",
        "tags": {
            "displayName": "Application Insights Extension"
        },
        "dependsOn": [
            "[resourceId('Microsoft.Web/Sites/', variables('TailspinToysWebName'))]",
            "[resourceId('Microsoft.Insights/components/', 'TailspinToysWeb')]"
        ],
        "properties": {
        }
        }
    ]
    ```

    It will look something like this screen shot.

    ![This is a screenshot of the code pasted below the "properties" property.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image34.png "Pasted block of JSON code")

### Task 5: Configure automatic scale for the web app in the template

1.  Select the resource called "TailspinToysWeb AutoScale" to see its JSON value
    
    ![TailspinToysWeb AutoScale is selected and highlighted in the template.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image35.png "TailspinToysWeb AutoScale selection")

2.  In the main window, scroll down a little to find the "enabled" property of the auto scale rule. Change it from "false" to "true." You can examine the other settings in this JSON value to understand the setting. It defaults to increasing the instance count if the CPU goes above 80% for a while, and then it reduces the instance count if the CPU falls below 60% for a while.
    
    ![This is a screenshot of the "enabled" property of the auto scale rule set to "true."](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image36.png "Screenshot of the ???enabled??? property ")

### Task 6: Configure the list of release environments parameters

1.  Next, you need to configure the list of release environments we'll be deploying to. Our scenario calls for adding three environments: dev, test, and production. This is going to require some manual code because there is not a wizard for this resource type. Click on the parameters item in the JSON Outline window to locate its JSON code. Then, add this code as the first element inside the of the "parameters" object.
    ```
    "environment": {
          "type": "string",
          "defaultValue": "dev",
          "allowedValues": [
            "dev",
            "test",
            "production"
          ]
    },
    ```

After adding the code, it will look like this.

![This is a screenshot of the code pasted inside the of the "parameters" object.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image37.png "Pasted block of JSON code")

### Task 7: Configure the name of the web app using the environments parameters

1.  Next, you need to configure the template so that it dynamically generates the name of the web application based on the environment it is being deployed to. Select the parameters item in the JSON Outline window to locate its JSON code. Then, locate the "variables" section and replace the corresponding TailspinToysWebName value with the following code:

    "TailspinToysWebName": "[concat('TailspinToysWeb', '-', parameters('environment'), '-', uniqueString(resourceGroup().id))]"},

After adding the code, it will look like this.

![This is a screenshot of the code replacement of the TailspinToysWebName value in the "variables" section.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image38.png "Pasted block of JSON code")

### Task 8: Add a deployment slot for the "staging" version of the site

2.  Next, you need to add the "staging" deployment slot to the web app. This is used during a deployment to stage the new version of the web app. This is going to require some manual code because there is not a wizard for this resource type. Select the TailspinToysWeb web app resource to locate its JSON code. Then, add this code to the "resources" array, just below the element for the application insights extension.
    ```
    {
        "apiVersion": "2015-08-01",
        "name": "staging",
        "type": "slots",
        "tags": {
        "displayName": "Deployment Slot: staging"
        },
        "location": "[resourceGroup().location]",
        "dependsOn": [
        "[resourceId('Microsoft.Web/Sites/', variables('TailspinToysWebName'))]"
        ],
        "properties": {
        },
        "resources": []
    }
    ```
It will look something like this screen shot.

![This is a screenshot of the code pasted just below the element for the application insights extension in the "resources" array.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image39.png "Pasted block of JSON code")

### Task 9: Create the dev environment and deploy the template to Azure

1.  First, before you deploy the template, you need to make sure that you can get the instrumentation key from the Application Insights resource because you will need it later. To do this, you can add an output property to the template. Go the "outputs" area of the template and paste or type in this JSON code:

    ```
        "MyAppInsightsInstrumentationKey": {
            "value": "[reference(resourceId('Microsoft.Insights/components', 'TailspinToysWeb'), '2014-04-01').InstrumentationKey]",
            "type": "string"
            }
    ```
2.  Now, save all your files

3.  Right-click the project in Solution Explorer and choose "Deploy" and then "New...."\
    ![In Solution Explorer, TailspinToysAzureResourceTemplate is selected, a submenu has New selected and highlighted, and another submenu has Deploy highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image40.png "Choosing New and Deploy")

4.  Sign in to your Azure account if necessary, and then choose your correct subscription. Under Resource group, choose "Create New..." and create a new resource group for this deployment. Since we are creating a dev environment, let us name it "TailspinToys-dev." Choose a location near you.
  
    ![In the Deploy to Resource Group window, Create New is selected in the Resource group drop-down list.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image41.png "Deploy to Resource Group window")\
    \
    ![In the Create Resource Group window, Tailspintoys-dev is highlighted in the Resource group name box, and Create is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image42.png "Create Resource Group window")

5.  Once you have the resource group created, select the **Edit Parameters** button

    ![In the Deploy to Resource Group window, the resource group has been created, and the Edit Parameters button is highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image43.png "Deploy to Resource Group window")

6.  In the next window, select "dev" from the list of environments. Then, pick an admin username and password for the database; it does not matter what you choose. Then use "TailspinData" for the TailspinDataName value. Call the hosting plan "TailspinHostingPlan1" and choose "S1" for the Sku. Finally, be sure to check the "Save passwords..." option at the bottom See this screen shot for help. When finished, select Save.
    
    ![In the Edit Parameters window, the dev value is highlighted along with the values for the admin username and the database password. The TailspinData, TailspinHostingPlan1, and S1 values are also highlighted, and the Save passwords as plain text in the parameters file check box is selected.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image44.png "Edit Parameters window")

7.  Then, select the **Deploy** button on the deployment window
    
    ![In the Deploy to Resource Group window, Deploy is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image45.png "Deploy to Resource Group window")

8.  If we have done everything correct, the deployment will begin. You can watch the output window inside Visual Studio to follow along. This deployment typically takes a few minutes. Upon completion, you should see success and you should see an instrumentation key be written out in the output window. Copy this down for a future step in this process. Note that your key will be different from the one shown in this screen shot.
    
    ![This is a screenshot of a highlighted instrumentation key in the output window.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image46.png "instrumentation key")

### Task 10: Create the test environment and deploy the template to Azure

The following steps are very similar to what was done in the previous task with the exception that you are now creating the test environment.

1.  Right-click the project in Solution Explorer and choose "Deploy" and then "New...."\
    ![In Solution Explorer, TailspinToysAzureResourceTemplate is selected, a submenu has New selected and highlighted, and another submenu has Deploy highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image40.png "Choosing New and Deploy")

2.  Sign in to your Azure account if necessary, and then choose your correct subscription. Under Resource group, choose "Create New..." and create a new resource group for this deployment. Since we are creating a test environment, let us name it "TailspinToys-test." Choose a location near you.
    ![In the Deploy to Resource Group window, Create New is selected in the Resource group drop-down list.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image41.png "Deploy to Resource Group window")\
    \
    ![In the Create Resource Group window, Tailspintoys-test is in the Resource group name box, and South Central US is in the Resource group location box.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image47.png "Create Resource Group window")

3.  Once you have the resource group created, select the **Edit Parameters** button

    ![In the Deploy to Resource Group window, the resource group has been created, and the Edit Parameters button is highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image48.png "Deploy to Resource Group window")

4.  In the next window, select "test" from the list of environments. Then, pick an admin username, and password for the database, it does not matter what you choose. Then use "TailspinData" for the TailspinDataName value. Call the hosting plan "TailspinHostingPlan1" and choose "S1" for the Sku. Finally, be sure to check the "Save passwords..." option at the bottom See this screen shot for help. When finished, select Save.
    ![In the Edit Parameters window, the test value is highlighted along with the values for the admin username and the database password. The TailspinData, TailspinHostingPlan1, and S1 values are also highlighted, and the Save passwords as plain text in the parameters file check box is selected.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image49.png "Edit Parameters window")

5.  Then, select the **Deploy** button on the deployment window
    ![In the Deploy to Resource Group window, Deploy is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image50.png "Deploy to Resource Group window")

6.  If we have done everything correct, the deployment will begin. You can watch the output window inside Visual Studio to follow along. This deployment typically takes a few minutes. Upon completion, you should see success and you should see an instrumentation key be written out in the output window. Copy this down for a future step in this process. Note that your key will be different from the one shown in this screen shot.
    ![This is a screenshot of a highlighted instrumentation key in the output window.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image46.png "Instrumentation key")

### Task 11: Create the production environment and deploy the template to Azure

The following steps are very similar to what was done in the previous task with the exception that you are now creating the production environment.

1.  Right-click the project in Solution Explorer and choose "Deploy" and then "New...."\
    ![In Solution Explorer, TailspinToysAzureResourceTemplate is selected, a submenu has New selected and highlighted, and another submenu has Deploy highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image40.png "Choosing New and Deploy")

2.  Sign in to your Azure account if necessary, and then choose your correct subscription. Under Resource group, choose "Create New..." and create a new resource group for this deployment. Since we are creating a production environment, let us name it "TailspinToys-prod." Choose a location near you.
    ![In the Deploy to Resource Group window, Create New is selected in the Resource group drop-down list.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image41.png "Deploy to Resource Group window")\
    \
    ![In the Create Resource Group window, TailspinToys-production is in the Resource group name box, and South Central US is in the Resource group location box.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image51.png "Create Resource Group window")

3.  Once you have the resource group created, select the **Edit Parameters** button

    ![In the Deploy to Resource Group window, the resource group has been created, and the Edit Parameters button is highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image52.png "Deploy to Resource Group window")

4.  In the next window, select "production" from the list of environments. Then, pick an admin username, and password for the database; it does not matter what you choose. Then use "TailspinData" for the TailspinDataName value. Call the hosting plan "TailspinHostingPlan1" and choose "S1" for the Sku. Finally, be sure to check the "Save passwords..." option at the bottom See this screen shot for help. When finished, select Save.

    ![](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image53.png)

5.  Then, select the **Deploy** button on the deployment window
    
    ![In the Deploy to Resource Group window, Deploy is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image54.png "Deploy to Resource Group window")

6.  If we have done everything correct, the deployment will begin. You can watch the output window inside Visual Studio to follow along. This deployment typically takes a few minutes. Upon completion, you should see success and you should see an instrumentation key be written out in the output window. Copy this down for a future step in this process. Note that your key will be different from the one shown in this screen shot.
    
    ![This is a screenshot of a highlighted instrumentation key in the output window.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image46.png "Instrumentation key")

7.  If you visit the Azure Portal for your Azure subscription, you should now see the three newly created resource groups

    ![In this screenshot, TailspinToys-dev, TailspinToys-production, and TailspinToys-test appear under Resource groups in the Azure Portal.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image55.png "Resource groups screenshot")

## Exercise 2: Create Visual Studio Team Services team project and Git Repository

Duration: 15 Minutes

In this exercise, you will create and configure a Visual Studio Team Services account along with an Agile team project.

### Task 1: Create Visual Studio Team Services Account

1.  Browse to the Visual Studio site at <http://visualstudio.com>

2.  If you do not already have an account, select **Get started for free**
    
    ![In this screenshot, Get started for free is selected under Visual Studio Team Services.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image56.png "Visual Studio Team Services screenshot")

3.  Authenticate with a Microsoft account

4.  Choose a name for your visualstudio.com account. For the purposes of this scenario, we will use "TailspinToys." Choose **Git** for the source code and then select Create.
    
    ![In the Create new project window, TailspinToys is highlighted in the Project name box, Git is highlighted in the Version control box, and Create is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image57.png "Create new project window")

5.  Once the Project is created, select the **Code** menu option in the header navigation\
    \
    ![In the TailspinToys window, Code is highlighted in the ribbon.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image58.png "TailspinToys window")

6.  On the **Code** -\> **File** page for the **TailspinToys** repository, scroll down to the bottom of the page, then select the **Initialize** button to get the "master" branch created
    
    ![In the TailspinToys is empty. Add some code! window, URLs appear in the Clone to your computer or push an existing repository from command line boxes, and Initialize is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image59.png "TailspinToys is empty. Add some code! window")

### Task 2: Add the Tailspin Toys source code repository to Visual Studio Team Services

In this Task, you will configure the Visual Studio Team Services Git repository. You will configure the remote repository using Git and then push the source code up to Visual Studio Team Services through the command line tools.

1.  In the support files, open a command prompt in the **C:\\Hackathon** folder

2.  Initialize a local Git repository by running the following commands at the command prompt:
    ```
    git init
    git add *
    git commit -m "adding files"
    ```
    
If a ."git" folder and local repository already exists in the TailspinToys.Web folder, then you will need to delete the ."git" folder first before running the above commands to initialize the Git repository.

3.  Leave that command prompt window open, and switch back to the web browser window for Visual Studio Team Services from the previous Task

4.  Within the list of Branches, select the **master** branch name
    
    ![In the Visual Studio Team Services window, the master branch name is highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image60.png "Visual Studio Team Services window")

5.  Select the **Clone** link in the upper-right
    
    ![In the upper-right corner of the window, the Clone link is highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image61.png "Clone link")

6.  Copy the **HTTPS** URL for the Git repository for use in the following step![The HTTPS URL is highlighted under Clone Repo.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image62.png "HTTPS URL")

7.  Go back to the **Command Prompt** and run the following command with including the HTTPS URL for your Git repository:
    ```
    git remote add origin https://<vsts account name>.visualstudio.com/_git/TailspinToys
    ```

8.  Next, run the following command:
    ```
    git push -u origin --all
    ```

9.  When prompted, login with your Microsoft Account for your VSTS Account
    
    ![This is a screenshot of the Enter password dialog box.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image63.png "Enter password dialog box")

10. You will get an error message similar to the following, since the local repository was created before the clone of the VSTS Git repository
    
    ![In this screenshot of the Command Prompt window, an error message appears because the local repository was created before the clone of the VSTS Git repository. At this time, we are unable to capture all of the information in the window. Future versions of this course should address this.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image64.png "Command Prompt window")

11. To fix this error, you will need to pull down the latest from the VSTS Git repository and force it to merge even though the histories do not match. You can do this by running the following command:
    ```
    git pull origin master --allow-unrelated-histories
    ```

12. Now run the "git push" command again:
    ```
    git push -u origin --all
    ```

13. Once the local changes are pushed up to the VSTS Git repository, you should see command-line output similar to the following:\
    ![In this screenshot of the Command Prompt window is command-line output that results from local changes pushing to the VSTS Git repository. At this time, we are unable to capture all of the information in the window. Future versions of this course should address this.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image65.png "Command Prompt window")

14. Go back to the web browser window for Visual Studio Team Services and select the **Files** link
    
    ![In the Visual Studio Team Services window, the Files link is highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image66.png "Visual Studio Team Services ")

15. You should see your source code now appearing inside of Visual Studio Team Services
    
    ![In the Visual Studio Team Services window, your source code now appears.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image67.png "Visual Studio Team Services")

## Exercise 3: Create Visual Studio Team Services build definition

Duration: 13 Minutes

In this exercise, you will create a build definition in Visual Studio Team Services (VSTS) that automatically builds the web application with every commit of source code. This will lay the groundwork for us to then create a release pipeline for publishing the code to our Azure environments.

### Task 1: Create a build definition

1.  Select the Build and Release hub in your VSTS project, and then select the Builds link

    ![In the Visual Studio Team Services window, Build and Release is highlighted in the ribbon. Below that, Builds is highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image68.png "Visual Studio Team Services window")

2.  Create a new definition and select continue on the following selection screen below

    ![In Builds, +New definition is highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image69.png "Create a new definition")

    ![A screen that shows choosing the VSTS Git option for the TailspinToys project.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image70.png "Select a source")

3.  Select the "Empty process" link

    ![Under Select a template, Empty process is highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image71.png "Select the Empty process link")

4.  Then, you are presented with the build definition process editor. Because we selected "Empty process" in the previous step, this process template is empty at the moment. This is where we will add and configure Tasks that define our build process. Notice the Name field on the right side of the screen is already filled in for us with "TailspinToys-CI." You can change this name for your builds, but for now, we will leave it as is.

    ![In the TailspinToys-CI build window, the Agent queue drop-down box is empty, below which is a message that states, "This setting is required."](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image72.png "Empty Agent queue drop-down box")

5.  Use the dropdown menu to set Agent queue to "Hosted." This tells VSTS that you want to use their provided build server to build your application. 

    ![In the Agent queue drop-down box, Hosted is highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image73.png "Agent queue drop-down box")

6.  On the left side of the screen, select the "Get sources" task. Notice the configuration options that appear on the right side of the screen. This is a consistent experience when navigating through the task list. This task is already configured to point to our "TailspinToys" repository and master branch that we configured earlier. No changes are needed to this task.

    ![In the TailspinToys-CI build window, the Get sources task is highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image74.png "Get sources task")

7.  Next, it is time to add the tasks the perform the build process. Note the default "Phase 1" section on the left side of the screen. This section will hold all of the upcoming tasks we add for our build process. It is simply a way of logically grouping tasks. You can change the display name of "Phase 1", but we will leave it as is for this scenario. Select the "+" plus sign to the right of the "Phase 1" section header. This will bring up the Add tasks list on the right.

    ![In the TailspinToys-CI build window, the plus sign (+) is highlighted next to Phase 1. To the right of that is the Add tasks list.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image75.png "Add tasks list")

8.  We will add four new tasks to Phase 1. In the Search box in the top right of the Add tasks screen, type "Nuget" to filter down the list. Several will be listed and your list may differ slightly from the list below. Select the "NuGet" task and then select the "Add" button. 

    ![In the Add tasks screen, the Nuget search box and Add button are highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image76.png "Add tasks screen")

9.  This adds a new item to the Phase 1 list on the left side of the screen labeled "NuGet restore." Next, we will add the task for actually building the solution. Replace the "Nuget" text in the search box with "Visual Studio Build" and select the Visual Studio Build task by clicking the "Add" button.
 
    ![In the TailspinToys-CI build window, NuGet restore is highlighted on the left, Visual Studio Build is highlighted in the search box, and the Add button is highlighted below that.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image77.png "NuGet restore item")

10. This also added a new item to Phase 1 list on the left side of the screen labeled "Build solution \*\*\\\*.sln." Next, add the task for executing unit tests. Replace the "Visual Studio Build" text in the search box with "Visual Studio Test" and select the task by selecting the "Add" button.

    ![In the TailspinToys-CI build window, Build solution \*\*\\\*.sln is highlighted on the left, Visual Studio Test is highlighted in the search box, and the Add button is highlighted below that.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image78.png "Build solution **\*.sln item")

11. The last task you add will be to package up the build and prepare it for deployment. Replace the "Visual Studio Test" text in the search box with "Publish Build Artifacts" and choose the task by selecting the "Add" button.

    ![In the TailspinToys-CI build window, VsTest - testAssemblies is highlighted on the left, Publish Build Artifacts is highlighted in the search box, and the Add button is highlighted below that.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image79.png "Package up the build and prepare it for deployment")

12. Now that we have added the final task, we will need to go back and configure several of the tasks

13. Select the "Build solution \*\*\\\*.sln" task that you added and copy the following text into the "MSBuild Arguments" field as shown in the screen shot below.

    ```
    /p:DeployOnBuild=true /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:SkipInvalidConfigurations=true /p:PackageLocation="$(build.artifactstagingdirectory)\\"
    ```

    ![In the TailspinToys-CI build window, Build solution \*\*\\\*.sln is highlighted on the left, and the above text is highlighted in the MSBuild Arguments box.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image80.png "MSBuild Arguments box")

14. Select the "Publish Artifact" task that you added and enter "\$(Build.ArtifactStagingDirectory)" into the Path to publish field and enter "TailspinToys-CI" into the Artifact name field as shown in the screen shot below.

    ![In the TailspinToys-CI build window, Publish Artifact: TailspinToys-CI is highlighted on the left, and on the right, \$(Build.ArtifactStagingDirectory) in the Path to publish box and TailspinToys-CI in the Artifact name box are highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image81.png "Path to publish and Artifact name boxes")

15. As a last step, select the dropdown arrow next to the "Save & queue" button near the top of the screen. You will be asked to confirm the save request. Select "Save" again.

    ![In the TailspinToys-CI build window, Save is highlighted in the Save & queue drop-down list.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image82.png "Save")

16. Congratulations! You have just created your first build definition. In the next exercise, we will create a release pipeline that deploys your builds.

### Task 2: Enable continuous integration

1.  For this scenario, we want the build definition to automatically trigger a build when any code is committed to the master repository. This is known as continuous integration. To configure a build definition for continuous integration, select the "Triggers" menu item and check the "Enable continuous integration" option. Be sure to set the Branch filters fields to match the screen shot below.

    ![In the TailspinToys-CI build window, the Triggers menu item is highlighted, and to the right, the Enable continuous integration option, Include Type, and master Branch specification are also highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image83.png "Automatically triggering a build")

2.  Save the changes

## Exercise 4: Create Visual Studio Team Services release pipeline

Duration: 30 Minutes

In this exercise, you will create a release pipeline in Visual Studio Team Services that performs automated deployment of build artifacts to Microsoft Azure. The release pipeline will deploy to three environments: dev, test, and production.

### Task 1: Create a release definition

1.  Navigate to the "Build and Release" hub and select "Releases" from the menu. This will bring up the Release Management screen. Click on the "+ New definition" button to begin the creation of a new release definition.

    ![On the Release Management screen, the Build and Release hub is highlighted in the menu, Releases is highlighted in the submenu, and the + New definition button is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image84.png "Release Management screen")

2.  You are then presented with several release templates to choose from. Select the "Empty process" link at the top of the screen.

    ![Under Select a Template, the Empty process link is highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image85.png "Select a template")

3.  This will present you with the Release Management editor which allows you to manage your release environments. Let us start by giving this release definition a name. Change the value "New Release Definition" at the top of the editor to "TailspinToys-Release" by selecing name to begin editing.

4.  Then, let us name our first environment by changing the Environment name field from "Environment 1" to "dev." In a future step, we will add additional environments.

    ![At the top of the window, TailspinToys-Release is highlighted, and to the right, dev is highlighted in the Environment name box.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image86.png "Changing the environment name")

5.  In this step, we will connect the artifacts from our build definition to this newly created release definition. Select the "+ Add" button or the "+ Add artifact" icon on the left side of the screen to begin this process.

    ![+ Add and + Add artifact are highlighted in this step.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image87.png "Connect the artifacts")

6.  The Add artifact screen will display and many of the fields will already be populated. In the Source (Build definition) field, select the "TailspinToys-CI" build definition you created in a previous exercise and then select the "Add" button.

    ![On the Add artifact screen, TailspinToys-CI is highlighted in the Source (Build definition) box, and the Add button is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image88.png "Add artifact screen")

7.  Now, it is time to begin adding specific tasks to perform a deployment to the dev environment. To navigate to the task editor, select the "Task" menu drop down and then select the "dev" environment.

    ![In the menu, dev is highlighted in the Tasks drop-down list.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image89.png "Adding specific tasks to perform")

8.  This brings up the task editor. This interface will be familiar to you as it is very similar to the task editor you used when creating the build definition. For this release definition we will need to add two tasks.

9.  Select the "+" button to bring up the Add tasks window

10. Enter "Azure App Service" into the search box

11. Select Azure App Services Deploy, and then select the "Add" button

    ![In the task editor, the plus sign (+) is highlighted next to Agent phase, Azure App Service is highlighted in the search box, and below that, Add is highlighted next to Azure App Service Deploy.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image90.png "Add Azure App Service Deploy")

12. We also need to add the second task on that search list "Azure App Service Manage." This task will assist us with the deployment slot swap after deployment. Select the "Add" button next to that task to also add it.

    ![In the task editor, under Add tasks, Add is highlighted next to Azure App Service Manage.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image91.png "Add Azure App Service Manage")

13. Both tasks need some configuration before they will work. On the left side of the screen, select the Azure App Service Deploy task to bring up the configuration window. At the start of this task, your list of fields may not look the same as the screen shot below, but the display will change as you begin to configure the fields from top to bottom.

14. In the Azure subscription field, select your subscription. You may see an "Authorize" button which will require you to authenticate Azure before populating this drop-down list.

15. In the App Service name field, select the item that begins with "TailspinToysWeb-dev." There will be series of numbers and letters after which will not match the screen shot below. This is due to the uniqueness requirement of resource names.

16. Check the Deploy to slot checkbox

17. Select "TailspinToys-dev" from the Resource group drop down list

18. Finally, select the "staging" value from the Slot drop down list. This tells the deployment to deploy to the slot first.

    ![After selecting the Azure App Service Deploy task, fill in various boxes in the configuration window, including Azure subscription, App Service name (select Deploy to slot), Resource group, and Slot. These values are highlighted in the screenshot of the configuration window.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image92.png "Configure the Azure App Service Deploy task")

19. Scroll down the configuration screen to see additional configuration fields

20. In the Package or folder option enter the following text:
    
    ```
    $(System.DefaultWorkingDirectory)/**/*.zip
    ```
    
    ![The above value is highlighted in the Package or folder box in the screenshot of the configuration window.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image93.png "Package or folder box ")

21. That completes the configuration for the Azure App Service Deploy task. Now, select the Swap Slots task to bring up the configuration screen.

22. In the Azure subscription field, select your subscription. You may see an "Authorize" button, which will require you to authentication to Azure before populating this drop-down list.

23. In the App Service name field, select the item that begins with "TailspinToysWeb-dev." There will be series of numbers and letters after which will not match the screen shot below. This is due to the uniqueness requirement of resource names.

24. Select "TailspinToys-dev" from the Resource group drop down list

25. Finally, select the "staging" value from the Source Slot drop down list. This tells the deployment to swap the staging slot with production.

    ![After selecting the Swap Slots task, fill in various boxes in the configuration window, including Azure subscription, App Service name, Resource group, and Source Slot (select Swap with Production). These values are highlighted in the screenshot of the configuration window.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image94.png "Configure the Swap Slots task")

26. Select the "Save" button at the top of the screen and confirm by selecting the "OK" button

27. Congratulations! You have just created your first release pipeline

### Task 2: Add test and production environments to release definition

1.  Navigate to the "Releases" menu and click "..." next to the TailspinToys-Release definition. Then, select "Edit" to bring up the Release Management editor.

    ![The Release menu item is highlighted, and below that, Edit is highlighted in the submenu for the TailspinToys-Release definition.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image95.png "Release Management editor")

2.  Move your mouse over the dev environment and select the "Clone" button to create a copy of the deployment tasks from the dev environment. We will use these same steps to deploy to test with a few configuration changes.

    ![The Clone button is highlighted under the dev environment.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image96.png "Copy the deployment tasks")

3.  Select the new environment "Copy of dev" to bring up the configuration screen

4.  Change the Environment name to "test"

5.  Now, we will begin editing the configuration for the test environment. Select the "1 phase, 2 tasks" link for the test environment.

    ![On the left, 1 phase, 2 tasks is highlighted under the test environment, and on the right, test is highlighted in the Environment name box.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image97.png "Begin configuring the test environment")

6.  Select the "Azure App Service Deploy" task to bring up the task configuration panel. Notice the settings are the same as when we configured it for the dev environment because we cloned the dev environment to create the test environment.

7.  For the App Service name field, select the item that begins with "TailspinToysWeb-test"

8.  For the Resource group field, select "TailspinToys-test"

9.  For the Slot field, select "staging"

    ![After selecting the Azure App Service Deploy task, fill in various boxes in the configuration window, including App Service name (select Deploy to slot), Resource group, and Slot. These values are highlighted in the screenshot of the configuration window.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image98.png "Configure the Azure App Service Deploy task")

10. We have completed configuration of the first task. Now, select the "Swap Slots" task to bring up the configuration panel.

11. For the App Service name field, select the item that begins with "TailspinToysWeb-test"

12. For the Resource group field, select "TailspinToys-test"

13. For the Source Slot field, select "staging"

    ![After selecting the Swap Slots task, fill in various boxes in the configuration window, including App Service name, Resource group, and Source Slot (select Swap with Production). These values are highlighted in the screenshot of the configuration window.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image99.png "Configure the Swap Slots task")

14. Select the "Save" button at the top of the screen, and confirm by seelcting the "OK" button

15. Congratulations! You have just created a test environment and added it to your pipeline

16. Repeat all of the steps in Task 2 to create a production environment, being careful to enter "production" as a replacement for "test" and selecting "TailspinWeb-production" instead of "TailspinWeb-test" where applicable. Do not forget to configure to individual steps in the newly cloned production environment.

17. The final release pipeline should look like the screen shot below.

    ![On the left is the TailspinToys-CI artifact, which is connected on the right by a line through the following environments (left to right): dev, test, and production (selected).](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image100.png "The final release pipeline")

18. Now you will enable the continuous deployment trigger so the release process automatically begins as soon as a build successfully completes. To do this, select the lightning bolt icon in the Artifacts window.

19. This will bring up the Continuous deployment trigger panel. Change the setting to "Enabled."

    ![The lightning bolt icon next to the TailspinToys-CI artifact is highlighted, and to the right, Enabled is selected and highlighted in the Continuous deployment trigger panel.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image101.png "Enable the continuous deployment trigger")

20. Select Save, and confirm your changes

21. Congratulations! You have completed the creation of a release pipeline with three environments

## Exercise 5: Trigger a build and release

Duration: 10 Minutes

In this exercise, you will trigger an automated build and release of the web application using the build definition and release pipeline you created in earlier exercises. The release pipeline will deploy to three environments: dev, test, and production.

Any commit of new or modified code to the master branch will automatically trigger a build. The task below is for when you want to manually trigger a build without a code change.

### Task 1: Manually queue a new build and follow it through the release pipeline

1.  Navigate to the "Builds" menu and click "..." next to the TailspinToys-CI build definition. Then, select "Queue new build..." menu option.

    ![In the Build menu, the TailspinToys-CI build definition is selected, the ellipsis next to it is highlighted, and Queue new build is highlighted in the shortcut menu.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image102.png "Queue a new build")

2.  This will present a popup titled "Queue build for TailspinToys-CI." Select the "Queue" button at the bottom of the popup.

    ![The Queue button is selected at the bottom of the popup dialog box.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image103.png "Queue button")

3.  A notification banner will indicate your build has been queued. You can follow the build process by clicking on the build notification banner. If the build is successful, it will resemble the screen shot below.

    ![The screenshot depicts a green notification banner that reads, "Build succeeded." The banner appears above a summary screen of the build details.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image104.png "Successful notification banner")

4.  Because we configured continuous deployment, the deployment to dev environment will then be triggered immediately. It will continue through on to the test and production environments. A successful release through all three environments will look like the screen shot below.

    ![The screenshot depicts a successful release through all three environments for the TailspinToys build.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image105.png "Screenshot of a successful release")

## Exercise 6: Create a feature branch and submit a pull request

Duration: 20 Minutes

In this exercise, you will create a short-lived feature branch, make a small code change, commit the code, and submit a pull request. You'll then merge the pull request into the master branch, which will trigger an automated build and release of the application.

In the tasks below, you will make changes directly through the Visual Studio Team Services web interface. These steps could also be performed through the Visual Studio IDE.

### Task 1: Create a new branch

1.  Select the "Code" hub. Then, select "Branches".

    ![In the Visual Studio Team Services web interface for TailspinToys, Code, Branches, and the New Branch button are highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image106.png "Visual Studio Team Services window")

2.  Select the "New branch" button in the upper right corner of the page

3.  In the "Create a branch" dialog, enter a name for the new branch. In this scenario, name it "new-heading". In the "Based on" field, be sure master is selected.

    ![In the Create a branch dialog box, new-heading is highlighted in the Name box, master is highlighted in the Based on box, and Create branch is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image107.png "Create a branch dialog box")

4. Select "Create branch"

### Task 2: Make a code change to the feature branch

1.  Select the name of the newly created branch. This will present the "Files" window.

    ![This is a screenshot of the Files window for Tailspin Toys.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image108.png "Files window")

2.  Next, you'll make a change to a page in the web application in the web browser

3.  Select the "TailspinToys.Web" folder

4.  Then, select the "Views" folder

5.  Then, select the "Home" folder

6.  Locate and select the "Index.cshtml" file. You will now see the contents of the file.

7.  Select the "Edit" button on the top right of the screen to begin editing the page

    ![The screenshot depicts the editing page for Index.cshtml. Index.cshtml is selected in the navigation pane, Index.cshtml is highlighted in the address bar, and Edit is highlighted at the top right.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image109.png "Editing page")

8.  Replace the code on line 6 with the following:

    ```
    <h1>Tailspin Toys</h1>
    ```
    
9.  Now that you've completed the code change, select the "Commit..." button on the top right side of the screen

    ![The code change above is highlighted on the editing page, and Commit is highlighted at the top right.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image110.png "Completing the code change")

10. This will present the Commit popup where you can enter a comment. Select the "Commit" button.

    ![The Commit button is highlighted at the bottom of the Commit dialog box.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image111.png "Commit dialog box")

### Task 3: Submit a pull request

1.  Select the "Branches" menu. Then, select the "..." button next to the branch you created earlier. Then, select the "New pull request" option.

    ![In the Visual Studio Team Services web interface for TailspinToys, Branches is highlighted in the menu, the ellipsis highlighted next to new-heading (selected), and New pull request is selected in the submenu.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image112.png "New pull request")

2.  This brings up the New Pull Request page. It shows we are submitting a request to merge code from our new-heading branch into the master branch. You have the option to change the Title and Description fields. The TailspinToys Team has been selected to review this pull request before it will be merged. The details of the code change are at the bottom of the page.

    ![This is a screenshot of the New Pull Request page with details of the code change at the bottom of the page.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image113.png "New Pull Request page")

3.  Select the "Create" button to submit the pull request

### Task 4: Approve and complete a pull request

Typically, the next few steps would be performed by another team member to allow for the code to be peer reviewed. However, in this scenario, you will continue as if you are the only developer on the project.

1.  After submitting the pull request, you are presented with Pull Requests review screen

2.  First, select the "Approve" button, assuming you approve of the code that was modified

3.  This will note that you approved the pull request. Then, select the "Complete" button to finish and merge the pull request into the master branch.

    ![On the New Pull Request page, Approve and Complete are highlighted.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image114.png "Finish merging the pull request")

4.  After selecting the Complete button in the previous step, you will be presented with the Complete pull request popup. You can add additional comments for the merge activity. Select the "Delete new-heading after merging" option so that our branch will be deleted after the merge has been completed. This keeps our repository clean of old and abandoned branches and eliminates the possibility of confusion.

    ![In the Complete pull request dialog box, Delete new-heading after merging is selected and highlighted, and Complete merge is highlighted at the bottom.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image115.png "Complete pull request dialog box")

5.  Select the "Complete merge" button

6.  You will then see a confirmation of the completed pull request

    ![This is a screenshot of the confirmation message.](images/Hands-onlabstep-by-step-ContinuousdeliverywithVSTSandAzureimages/media/image116.png "Confirmation message")

7.  Congratulations! You just created a branch, made a code change, submitted a pull request, approved the pull request, and merged the code

8.  Because we configured continuous integration and continuous deployment, an automated build will be triggered and then the deployment to dev environment will begin immediately after a successful build. It will continue through on to the test and production environments.

## After the hands-on lab 

Duration: 10 Minutes

These steps should be followed only *after* completing the hands-on lab.

### Task 1: Delete Resources

1.  Now since the hands-on lab is complete, go ahead and delete all of the Resource Groups that were created for this hands-on lab. You will no longer need those resources and it will be beneficial to clean up your Azure Subscription.

