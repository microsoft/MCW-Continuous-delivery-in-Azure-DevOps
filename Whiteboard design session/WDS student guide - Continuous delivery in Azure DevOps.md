![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/main/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
Continuous delivery in Azure DevOps
</div>

<div class="MCWHeader2">
Whiteboard design session student guide
</div>

<div class="MCWHeader3">
November 2021
</div>

Information in this document, including URL and other Internet website references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2021 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [Continuous delivery in Azure DevOps whiteboard design session student guide](#continuous-delivery-in-azure-devops-whiteboard-design-session-student-guide)
  - [Abstract and learning objectives](#abstract-and-learning-objectives)
  - [Step 1: Review the customer case study](#step-1-review-the-customer-case-study)
    - [Customer situation](#customer-situation)
    - [Customer needs](#customer-needs)
    - [Customer objections](#customer-objections)
    - [Infographic for common scenarios](#infographic-for-common-scenarios)
  - [Step 2: Design a proof of concept solution](#step-2-design-a-proof-of-concept-solution)
  - [Step 3: Present the solution](#step-3-present-the-solution)
  - [Wrap-up](#wrap-up)
  - [Additional references](#additional-references)

<!-- /TOC -->

# Continuous delivery in Azure DevOps whiteboard design session student guide

## Abstract and learning objectives

In this whiteboard design session, you will learn how to design a solution with a combination of Bicep templates, Azure DevOps, and GitHub actions to enable continuous delivery with several Azure PaaS services.

At the end of this workshop, you will be better able to build templates to automate cloud infrastructure and reduce error-prone manual processes. In addition, you'll learn how to design a deployment and monitoring architecture using Bicep templates to provision Azure resources, Application Insights for deep application monitoring, and GitHub as a source code repository and build/deploy pipeline.

## Step 1: Review the customer case study

**Outcome**

Analyze your customer's needs.

Timeframe: 15 minutes

Directions: With all participants in the session, the facilitator/SME presents an overview of the customer case study along with technical tips.

1. Meet your team members and trainer.

2. Read all directions for steps 1-3 in the student guide.

3. As a team, review the following customer case study.

### Customer situation

Fabrikam Medical Conferences provides conference website services tailored to the medical community. They started 10 years ago building a few conference sites for a small conference organizer. Since then, word of mouth has spread, and Fabrikam Medical Conferences is now a well-known industry brand. They currently handle over 100 conferences per year and growing.

Websites for medical conferences are typically low-budget websites because the conferences usually have between 100 to 1500 attendees. At the same time, the conference owners have significant customization and change demands that require turnaround on a dime to the live sites. These changes can impact various aspects of the system from UI through to the back end, including conference registration and payment terms.

**Public website and data layer**

The VP of Engineering at Fabrikam, Susan Withers, has a team of 12 developers who handle all aspects of development, testing, deployment, and operational management of their customer sites. Due to customer demands, they have issues with the efficiency and reliability of the conference websites. This is mainly caused by an inefficient development and operations workflow.

In the current situation, the conference sites are hosted on-premises with the following topology and platform implementation:

- The conference websites are built with the MEAN stack (Mongo, Express, Angular, Node.js).
- websites and APIs are hosted on Linux machines.
- MongoDB is also running on a separate cluster of Linux machines.

### Customer needs

1. Be able to automatically and continuously deploy new software builds to the Azure App Service web app.

2. Ensure that continuously deployed builds to the cloud do not interfere with the production copy of the solution.

3. Identify an automated way of deploying to different environments for "development," "test," and "production" so that changes or deployments to one environment do not affect the others.

4. Configure the automated builds to first require that a full series of unit tests pass before a deployment is started.

5. Provide a search feature and visual dashboard for the application logs so the developers can more quickly resolve help desk tickets.

6. Enhance the logged data from the front-end website to give the developers a more complete picture of the application's performance and behavior.

   - Browser information such as browser page load time and user activity per page.

   - Application dependency metrics such as request times and request failures for communication with the database or other services.

7. Implement proactive diagnostics to generate automatic alerts for unusual application behavior including aberrant request response time, dependency response time, and page load time.

### Customer objections

1. We do not want to be locked into a specific source control repository. We are evaluating GitHub and Azure DevOps and need to be able to change between them without frustrating rework.

2. We do not want the developers to be able to make changes to the Azure resources even though they will have access to make source code changes.

3. If developers can deploy directly to the cloud, will that expose us to the same quality problems we had before when untested code was promoted to production?

4. How much of an impact will these process changes have on our development cadence? Will learning this place a new burden on the developers?

5. Our developers are already having a challenge learning how to use Git; will adding a continuous deployment system on top of that slow them down and confuse them even more?

### Infographic for common scenarios

![Common Scenarios for building a continuous deployment pipeline with Azure DevOps. Images include the icon for Azure DevOps, a Git repo, and a pipeline.](media/commonscenarios.png "Common Scenarios for building a continuous deployment pipeline with Azure DevOps")

## Step 2: Design a proof of concept solution

**Outcome**

Design a solution and prepare to present the solution to the target customer audience in a 15-minute chalk-talk format.

Timeframe: 60 minutes

**Business needs**

Directions: With your team, answer the following questions and be prepared to present your solution to others:

1. Who will you present this solution to? Who is your target customer audience? Who are the decision makers?

2. What customer business needs do you need to address with your solution?

**Design**

Directions: With your team, respond to the following questions:

_Continuous Integration and Deployment_

1. What available system should you use to automate software builds and deployments of the application?

2. Explain how you can continuously deploy new builds directly to the cloud without interfering with the production site.

3. Document how to integrate unit tests into the continuous delivery process so that when a test fails to pass, the deployment process is flagged and stopped.

4. Explain how you can test a new build simultaneously with an existing build, like an A/B test.

5. Why shouldn't we have multiple long-lived branches in source control?

6. Create a plan on how to switch the source control location from Azure DevOps to GitHub.

_Enhance system logging functionality_

1. Implement a solution that will enable the logs to be searchable and visible in an online dashboard.

2. Implement a solution to enhance the application logs to provide more useful performance and application behavior details, specifically around browser metrics and application dependencies. Discuss which visualization, or dashboard, options exist for the log results. Existing App Service logs already cover these topics:

   - **Detailed Error Logging**---detailed error information for HTTP status codes that indicate a failure (status code 400 or greater).

   - **Failed Request Tracing**---detailed information on failed requests, including a trace of the Internet Information Server IIS components used to process the request and the time taken in each component.

   - **Web Server Logging**---information about HTTP transactions using the W3C extended log file format.

   - **Application Diagnostics**---trace messages as defined in the source code.

   - **Deployment Logs**

**Prepare**

Directions: As a team:

1. Identify any customer needs that are not addressed with the proposed solution.

2. Identify the benefits of your solution.

3. Determine how you will respond to the customer's objections.

Prepare a 15-minute chalk-talk style presentation to the customer.

## Step 3: Present the solution

**Outcome**

Present a solution to the target customer audience in a 15-minute chalk-talk format.

Timeframe: 30 minutes

**Presentation**

Directions:

1. Pair with another team.

2. One group is the Microsoft team, the other is the customer.

3. The Microsoft team presents their proposed solution to the customer.

4. The customer makes one of the objections from the list of objections.

5. The Microsoft team responds to the objection.

6. The customer team gives feedback to the Microsoft team.

7. Switch roles and repeat Steps 2-6.

## Wrap-up

Timeframe: 15 minutes

Directions: Reconvene with the larger group to hear the facilitator/SME share the preferred solution for the case study.

## Additional references

|                                           |                                                                                                                        |
| ----------------------------------------- | :--------------------------------------------------------------------------------------------------------------------: |
| **Description**                           |                                                       **Links**                                                        |
| Enable diagnostics logging for web apps   |              <https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/>               |
| Monitor web app performance               |                  <https://azure.microsoft.com/en-us/documentation/articles/insights-perf-analytics/>                   |
| Azure Pipelines                           |                             <https://azure.microsoft.com/en-us/services/devops/pipelines/>                             |
| Switch deployment slots in Azure web apps | <https://blogs.msdn.microsoft.com/devops/2017/04/10/considerations-on-using-deployment-slots-in-your-devops-pipeline/> |
| App service continuous deployment         |                   <https://docs.microsoft.com/en-us/azure/app-service/deploy-continuous-deployment/>                   |
| App service staging environments          |                <https://azure.microsoft.com/en-us/documentation/articles/web-sites-staged-publishing/>                 |
| Application Insights                      |                   <https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview/>                    |
