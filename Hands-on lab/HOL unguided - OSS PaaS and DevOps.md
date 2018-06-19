![](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")


<div class="MCWHeader1">
OSS PaaS and DevOps
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

- [Overview](#overview)
- [Solution Architecture](#solution-architecture)
- [Requirements](#requirements)
- [Exercise 1: Run starter application](#exercise-1-run-starter-application)
    - [Task 1: Connect to your Lab VM](#task-1-connect-to-your-lab-vm)
        - [Task to complete](#task-to-complete)
    - [Task 2: Connect to your Lab VM](#task-2-connect-to-your-lab-vm)
        - [Task to complete](#task-to-complete-1)
    - [Task 3: Integrate GitHub into VS Code](#task-3-integrate-github-into-vs-code)
        - [Task to complete](#task-to-complete-2)
    - [Task 4: Clone the starter application](#task-4-clone-the-starter-application)
        - [Task to complete](#task-to-complete-3)
    - [Task 5: Launch the starter application](#task-5-launch-the-starter-application)
        - [Task to complete](#task-to-complete-4)
- [Exercise 2: Migrate the database to Cosmos DB](#exercise-2-migrate-the-database-to-cosmos-db)
    - [Task 1: Provision Cosmos DB using the MongoDB API](#task-1-provision-cosmos-db-using-the-mongodb-api)
        - [Task to complete](#task-to-complete-5)
    - [Task 2: Update database connection string](#task-2-update-database-connection-string)
        - [Task to complete](#task-to-complete-6)
    - [Task 3: Pre-create and scale collections](#task-3-pre-create-and-scale-collections)
        - [Task to complete](#task-to-complete-7)
    - [Task 4: Import data to the API for MongoDB using mongoimport](#task-4-import-data-to-the-api-for-mongodb-using-mongoimport)
        - [Task to complete](#task-to-complete-8)
    - [Task 5: Install Azure Cosmos DB extension for VS Code](#task-5-install-azure-cosmos-db-extension-for-vs-code)
        - [Task to complete](#task-to-complete-9)
    - [Task 6: Decrease collection throughput](#task-6-decrease-collection-throughput)
        - [Task to complete](#task-to-complete-10)
- [Exercise 3: Containerize the app](#exercise-3-containerize-the-app)
    - [Task 1: Create an Azure Container Registry](#task-1-create-an-azure-container-registry)
        - [Task to complete](#task-to-complete-11)
    - [Task 2: Install Docker extension in VS Code](#task-2-install-docker-extension-in-vs-code)
        - [Task to complete](#task-to-complete-12)
    - [Task 3: Create Docker image and run the app](#task-3-create-docker-image-and-run-the-app)
        - [Task to complete](#task-to-complete-13)
    - [Task 4: Run the containerized App](#task-4-run-the-containerized-app)
        - [Task to complete](#task-to-complete-14)
    - [Task 5: Push image to Azure Container Registry](#task-5-push-image-to-azure-container-registry)
        - [Task to complete](#task-to-complete-15)
- [Exercise 4: Set up Web App for Containers](#exercise-4-set-up-web-app-for-containers)
    - [Task 1: Provision Web App for Containers](#task-1-provision-web-app-for-containers)
        - [Task to complete](#task-to-complete-16)
    - [Task 2: Navigate to the deployed app](#task-2-navigate-to-the-deployed-app)
        - [Task to complete](#task-to-complete-17)
- [Exercise 5: Configure CI/CD pipeline](#exercise-5-configure-cicd-pipeline)
    - [Task 1: Prepare GitHub account for service integrations](#task-1-prepare-github-account-for-service-integrations)
        - [Task to complete](#task-to-complete-18)
    - [Task 2: Configure Continuous Integration (CI) with Jenkins](#task-2-configure-continuous-integration-ci-with-jenkins)
        - [Task to complete](#task-to-complete-19)
    - [Task 3: Trigger CI build](#task-3-trigger-ci-build)
        - [Task to complete](#task-to-complete-20)
    - [Task 4: Create Free Visual Studio Team Services Account](#task-4-create-free-visual-studio-team-services-account)
        - [Task to complete](#task-to-complete-21)
    - [Task 5: Create a VSTS personal access token](#task-5-create-a-vsts-personal-access-token)
        - [Task to complete](#task-to-complete-22)
    - [Task 6: Configure Jenkins for Team Services integration](#task-6-configure-jenkins-for-team-services-integration)
        - [Task to complete](#task-to-complete-23)
    - [Task 7: Create a Jenkins service endpoint is VSTS](#task-7-create-a-jenkins-service-endpoint-is-vsts)
        - [Task to complete](#task-to-complete-24)
    - [Task 8: Create a Team Services release definition](#task-8-create-a-team-services-release-definition)
        - [Task to complete](#task-to-complete-25)
    - [Task 9: Trigger CI/CD pipeline](#task-9-trigger-cicd-pipeline)
        - [Task to complete](#task-to-complete-26)
- [Exercise 6: Create Azure Function for order processing](#exercise-6-create-azure-function-for-order-processing)
    - [Task 1: Provision a Function App](#task-1-provision-a-function-app)
        - [Task to complete](#task-to-complete-27)
    - [Task 2: Configure storage queues](#task-2-configure-storage-queues)
        - [Task to complete](#task-to-complete-28)
    - [Task 3: Create Cosmos DB trigger function](#task-3-create-cosmos-db-trigger-function)
        - [Task to complete](#task-to-complete-29)
    - [Task 4: Create Queue function](#task-4-create-queue-function)
        - [Task to complete](#task-to-complete-30)
- [Exercise 7: Create Logic App for sending SMS notifications](#exercise-7-create-logic-app-for-sending-sms-notifications)
    - [Task 1: Create Free Twilio account](#task-1-create-free-twilio-account)
        - [Task to complete](#task-to-complete-31)
    - [Task 2: Create Logic App](#task-2-create-logic-app)
        - [Task to complete](#task-to-complete-32)
- [After the hands-on lab](#after-the-hands-on-lab)
    - [Task 1: Delete Azure resource groups](#task-1-delete-azure-resource-groups)
    - [Task 2: Delete WebHooks and Service Integrations](#task-2-delete-webhooks-and-service-integrations)
- [Appendix A: Lab VM setup](#appendix-a-lab-vm-setup)
    - [Task 1: Create VM config script](#task-1-create-vm-config-script)
    - [Task 2: Create a Linux virtual machine](#task-2-create-a-linux-virtual-machine)

<!-- /TOC -->


## Overview

Best For You Organics Company is one of the leading health food suppliers in North America, serving customers in Canada, Mexico, and the United States. They have a MERN stack web application which they host on-premises, and are looking to migrate their OSS application into Azure. They will be creating a custom Docker image for their application, and using a Jenkins and Visual Studio Team Services (VSTS) continuous integration/continuous delivery (CI/CD) pipeline to deploy the application into a Web App for Containers instance. In addition, they will be setting up Azure Cosmos DB, and using the MongoDB APIs, so they don't have to make application code changes to leverage the power of Cosmos DB.

In this hands-on lab, you will assist with completing the OSS application and database migrations into Azure. You will create a custom Docker image, provision an Azure Container Registry, push the image to the registry, and configure the CI/CD pipeline to deploy the application to a Web App for Containers instance. You will also help them implement functionality enhancements using serverless architecture.

## Solution Architecture

Below is a diagram of the solution architecture you will build in this lab. Please study this carefully, so you understand the whole of the solution as you are working on the various components.

![This diagram consists of icons that are connected by arrows. On the left, the Developer icon (VS Code) points in a linear fashion to GitHub Repo, Jenkins, and the VSTS icons. The previous three icons are enclosed in a box labeled CI/CD Pipeline. VSTS points to Web App for Containers on the right. Various arrows point from Web App for Container to: Azure Container Registry (a double-sided arrow); Logic Apps (a linear arrow that also points from Logic Apps to Customers); Customers (a linear arrow); and Azure Cosmos DB (a double-sided arrow that also points from Azure Cosmos DB to Azure Functions).](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image2.png "Solution architecture diagram")

The solution begins with developers using Visual Studio Code (VS Code) as their code editor, so they can leverage its rich integration with GitHub, Docker, and Azure. From a high level, developers will package the entire OSS application inside a custom Docker container using the Docker extension in VS Code. The image will be pushed to an Azure Container Registry as part of a continuous integration/continuous delivery (CI/CD) pipeline using GitHub, Jenkins, and Visual Studio Team Services (VSTS). This Docker image will then be deployed to a Web App for Containers instance, as part of their continuous delivery process using Release Management in VSTS.

The MongoDB database will be imported into Azure Cosmos DB, using mongoimport.exe, and access the database from the application will continue to use the MongoDB APIs. The database connection string in the application will be updated to point to the new Cosmos DB.

Serverless architecture will be applied to order processing and customer notifications. Azure Functions will be used to automate the processing of orders. Logic Apps will be applied to send SMS notifications, via a Twilio connector, to customers informing them that their order has been processed and shipped.

## Requirements

1.  Microsoft Azure subscription must be pay-as-you-go or MSDN

    -   Trial subscriptions will *not* work

2.  Linux virtual machine configured with:

    -   Visual Studio Code

    -   Azure CLI

    -   Docker

    -   Node.js and npm

    -   MongoDB Community Edition



## Exercise 1: Run starter application

Duration: 30 minutes

In this exercise, you will create a local copy of the starter application on your Lab VM, add some sample data to the local MongoDB database, and run the application.

### Task 1: Connect to your Lab VM

In this task, you will create an RDP connection to your Lab VM.

#### Task to complete

-   Use a remote desktop client (RDP) application to connect to your Lab VM.

*Exit criteria*

-   You have an open RDP connection to your Lab VM.

### Task 2: Connect to your Lab VM

In this task, you will grant permissions to the demouser account to access the Unix socket needed to communicate with the Docker engine.

#### Task to complete

-   Execute a command to grant permissions to Docker for the current user:

    -   sudo usermod -a -G docker $USER

*Exit criteria*

-   You can successfully run the following:

    -   docker run hello-world

### Task 3: Integrate GitHub into VS Code

In this task, you will install the GitHub extension in VS Code, and configure a service integration with your GitHub account. This integration will allow you to push your code changes to GitHub directly from VS Code.

#### Task to complete

-   Install the GitHub extension in VS Code

-   Generate a Personal access token in GitHub, and add it to VS Code

*Exit criteria*

-   Your GitHub account is successfully connected within VS Code

### Task 4: Clone the starter application

In this task, you will add the Containers and Hyper-V features to your Windows installation, and install Docker.

#### Task to complete

-   Clone the mcw-oss-paas-devops starter application on your Lab VM

-   Register your GitHub email address with the local repository

-   Open the mcw-oss-paas-devops project in VS Code

*Exit criteria*

-   You have a local copy of the mcw-oss-paas-devops starter project open on your Lab VM in VS Code

### Task 5: Launch the starter application

In this task, you will seed the MongoDB with sample data, then run the application locally, connected to your MongoDB instance. This task is to verify the connection to MongoDB and that it contains the seeded plan data, before we migrate the application and data to Azure Cosmos DB.

#### Task to complete

-   Run npm install to set up your local copy of the starter application

-   Use node to execute the database seeding script (located in the project at data/Seed.js

-   Start the application

*Exit criteria*

-   You can view the starter application by navigating to http://localhost:3000![Two Person Plan, Four Person Plan, and High-Pro Plan boxes are visible in this screenshot of the starter application.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image17.png "View the starter application")

## Exercise 2: Migrate the database to Cosmos DB

Duration: 30 minutes

In this exercise, you will provision an Azure Cosmos DB account, and then update the starter application's database connection string to point to your new Azure Cosmos DB account. You will then, use *mongoimport.exe* to migrate the data in your MongoDB database into Cosmos DB collections, and verify with the application that you are connected to your Cosmos DB database.

### Task 1: Provision Cosmos DB using the MongoDB API

In this task, you will provision a new Azure Cosmos DB account using the MongoDB API.

#### Task to complete

-   Create an Azure Cosmos DB account, named best-for-you-db, using the MongoDB API

*Exit criteria*

-   You can navigate to your Cosmos DB account in the Azure portal

### Task 2: Update database connection string

In this task, you will retrieve the connection string for your Azure Cosmos DB database, and update the starter application's database connection string.

#### Task to complete

-   Retrieve the primary connection string for you Azure Cosmos DB account from the azure portal

-   Update the databaseUrl variable in *app.js* to the connection string for your Azure Cosmos DB

-   Insert the database name, best-for-you-organics, into the connection string

*Exit criteria*

-   Your app is connected to a database named best-for-you-organics, in your Azure Cosmos DB account. The plans will be gone from the home page since no data has been added to the Cosmos DB yet.

    ![The plans are no longer visible in this screenshot of the starter application; best-for-you-organics information is displayed.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image18.png "Starter application screenshot")

### Task 3: Pre-create and scale collections

In this task, you will create the collections needed for your database migration, and increase each collections' throughput from the default 1,000 RUs to 2,500 RUs. This is done to avoid throttling during the migration, and reduce the time required to import data.

#### Task to complete

-   Add collections to the best-for-you-organics database Cosmos DB for plans, users, and orders with a throughput of 2500 RUs

*Exit criteria*

-   You have all the required collections in the Cosmos DB database

### Task 4: Import data to the API for MongoDB using mongoimport

In this task, you will use *mongoimport.exe* to import data to your Cosmos DB account.

#### Task to complete

-   Export the data from your local MongoDB instance into JSON files using *mongoexport.exe*

-   Import the data into your Azure Cosmos DB account using *mongoimport.exe*

-   Stop your local MongoDB instance

*Exit criteria*

-   The seed data from your local MongoDB is visible in your Cosmos DB instance, and is served up in the application page in a browser

-   MongoDB is no longer running on your Lab VM

### Task 5: Install Azure Cosmos DB extension for VS Code

In this task, you will install the Azure Cosmos DB extension for VS Code, to take advantage of the integration with Azure Cosmos DB. This extension allows you to view and interact with your Cosmos DB databases, collections, and documents directly from VS Code.

#### Task to complete

-   Install the Azure Cosmos DB extension in VS Code

*Exit criteria*

-   You can view the best-for-you-organics database and collections in VS Code

### Task 6: Decrease collection throughput

In this task, you will decrease the throughput on your collections. Azure Cosmos DB uses an hourly billing rate, so reducing the throughput after the data migration will help save costs.

#### Task to complete

-   Decrease the throughput for each collection in the best-for-you-organics database to 400 RUs

*Exit criteria*

-   You have scaled down the throughput for each of your collections

## Exercise 3: Containerize the app

Duration: 30 minutes

This exercise walks you through containerizing an existing MERN application using Docker, pushing the image to an Azure Container Registry, then deploying the image to Web App for Containers directly from VS Code.

### Task 1: Create an Azure Container Registry

In this task, you will be creating a private Docker registry in the Azure portal, so you have a place to store the custom Docker image you will create in later steps.

#### Task to complete

-   Provision a basic Azure Container Registry, named bestforyouregistrySUFFIX, with the admin user enabled

*Exit criteria*

-   You can navigate to the Azure Container Registry in the Azure portal

### Task 2: Install Docker extension in VS Code

The Docker extension for VS Code is used to simplify the management of local Docker images and commands, as well as the deployment of a built app image to Azure.

#### Task to complete

-   Install the Docker extension in VS Code

*Exit criteria*

-   You can view your Azure Container Registry in the Docker extension in VS Code

    ![The Azure Container Registry is selected in the Docker extension in VS Code.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image19.png "View your Azure Container Registry")

### Task 3: Create Docker image and run the app

In this task, you will use VS Code, and the Docker extension, to add the necessary files to the project to create a custom Docker image for the mcw-oss-paas-devops app.

#### Task to complete

-   Using the Docker tools in VS Code, add Docker files to your workspace

-   Build a Docker image from the Dockerfile, with a name in the format \[registry\]/\[image name\]:\[tag\], so it is ready to be pushed to your Azure Container Registry

*Exit criteria*

-   View the image in the Docker extension in VS Code

    ![The image is selected in the Docker extension in VS Code.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image20.png "View the image ")

### Task 4: Run the containerized App

In this task, you will run the app from the container you built in the previous task.

#### Task to complete

-   Run the containerized app in interactive mode

*Exit criteria*

-   You can use a browser to navigate to the containerized app at http://localhost:3000

### Task 5: Push image to Azure Container Registry

In this task, you are going to push the image to your Azure Container Registry.

#### Task to complete

-   Push the image to your Azure Container Registry

*Exit criteria*

-   You can view your image in your Azure Container Registry's repositories

    ![In your Azure Container Registry's repository, Repositories is selected and highlighted under Services, and best-for-you-organics is highlighted under Repositories.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image21.png "View your image")

## Exercise 4: Set up Web App for Containers

Duration: 10 minutes

In this exercise, you will deploy the containerized app to a Web App for Containers instance from the image stored in your Azure Container Registry.

### Task 1: Provision Web App for Containers

#### Task to complete

-   Provision a Web App for Containers instance in the Azure portal, named best-for-you-app-SUFFIX

    -   Set the image source as best-for-you-organics image in your Azure Container Registry, using the latest tag

*Exit criteria*

-   You can navigate to the provisioned App Service in the Azure portal, and view the Docker Container details

### Task 2: Navigate to the deployed app

In this task, you will navigate to the deployed app, and log in to verify it is functioning correctly.

#### Task to complete

-   Using a web browser, navigate to the URL for your App Service

*Exit criteria*

-   You can successfully sign in to the site with the following credentials to verify everything is working as expected.

    -   <demouser@bfyo.com>

    -   Password.1!! 
    
    ![Two Person Plan, Four Person Plan, and High-Pro Plan boxes are visible in this screenshot of the deployed application.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image22.png "View the deployed application")

## Exercise 5: Configure CI/CD pipeline

Duration: 60 minutes

In this exercise, you are going to add continuous delivery (CD) to a Jenkins continuous integration (CI) using Release Management in VSTS.

### Task 1: Prepare GitHub account for service integrations

In this task, you will be adding a Jenkins service integration into your GitHub account. This integration will enable a Jenkins continuous integration build job to be triggered when code is checked in to your GitHub repository.

#### Task to complete

-   Add a Jenkins (GitHub plugin) service integration into GitHub for your Jenkins server

-   Add a deploy key in GitHub, using an ssh key generated on your Jenkins server

*Exit criteria*

-   You can successfully connect to your Jenkins server from GitHub using the command: ssh git\@github.com

### Task 2: Configure Continuous Integration (CI) with Jenkins

In this task, you will set up a simple Jenkins CI pipeline, which will build the mcw-oss-paas-devops application with every code commit into GitHub.

#### Task to complete

-   Navigate to your Jenkins server in a web browser, using the DNS name of your Jenkins server

-   Set up an SSH tunnel to Jenkins on port 8080

-   Configure your Jenkins server, installing recommended plugins

-   Add the NodeJS and VS Team Services Continuous Deployment plug-ins to your Jenkins server

    -   Configure the NodeJS installation to install automatically

-   Create a new freestyle project in Jenkins named best-for-you-build

    -   Specify it as a GitHub project, pointing to the mcw-oss-paas-devops project in your GitHub account

    -   Add Git as the source code repository, and point to the mcw-oss-paas-devops project in your GitHub account

    -   Add a GitHub hook build trigger for GITScm polling

*Exit criteria*

-   You can navigate to <http://localhost:8080> on your Lab VM, and login to Jenkins

    ![Log in is selected and highlighted on the Jenkins login page.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image23.png "Log in to Jenkins")

### Task 3: Trigger CI build

In this task you will commit your pending changes in VS Code to you GitHub repo, and trigger the Jenkins CI build job.

#### Task to complete

-   Commit the pending changes to the mcw-oss-paas-devops project in VS Code, and push them to your GitHub repo

*Exit criteria*

-   You can see the commit and associated build job in Jenkins

    ![The commit and associated build job is displayed in this Jenkins screenshot.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image24.png "View the commit and associated build job")

### Task 4: Create Free Visual Studio Team Services Account

In this task, you will create a free Visual Studio Team Services (VSTS) account. If you already have a VSTS account, you can skip this task.

#### Task to complete

-   Register for a VSTS account at <https://www.visualstudio.com/>

-   Create a new team project, named BestForYouOrganics

*Exit criteria*

-   You have the BestForYouOrganics project in your VSTS account

### Task 5: Create a VSTS personal access token

In this task, you will create a personal access token with the Release (read, write, execute, and manage) permission in VSTS, which will allow Jenkins to communicate with Team Services account.

#### Task to complete

-   Generate a Personal Access Token in VSTS, with the authorized scopes of Release (read, write, execute and manage)

*Exit criteria*

-   You have copied the generated Personal Access Token into a text editor for later use

### Task 6: Configure Jenkins for Team Services integration

In this task, you will add the necessary configuration information to allow Jenkins to send build artifacts to a VSTS release.

#### Task to complete

-   Add a post-build action to your Jenkins best-for-you-build project to archive the artifacts, using the path \*\*/\*

-   Add a Trigger release in TFS/Team Services post-build action to your Jenkins best-for-you-build project

    -   Connect it to the BestForYouOrganics project in your VSTS account

    -   Set the release definition to BestForYouOrganics CD

*Exit criteria*

-   Your Jenkins build job has been updated to send the build artifacts to a VSTS release definition

### Task 7: Create a Jenkins service endpoint is VSTS

In this task, you will configure a service endpoint for your Jenkins server in VSTS.

#### Task to complete

-   Add a service endpoint to your VSTS BestForYouOrganics project pointing to your Jenkins server

*Exit criteria*

-   You have a Jenkins service endpoint in VSTS

    ![Verify connection is highlighted in the Add new Jenkins Connection dialog box, and a green check mark is displayed next to Connection: Verified.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image25.png "Verify the connection")

### Task 8: Create a Team Services release definition

In this task, you will create the release definition in VSTS, which will take the build artifacts from Jenkins, and push them to the Azure Container Registry associated with your Web App for Containers.

#### Task to complete

-   Create a new release definition for the BestForYouOrganics project in VSTS

    -   Use empty environment

        -   Set Agent phase to use Hosted Linux Preview Agent queue

        -   Add Docker task to Build an image

        -   Add Docker task to Push an image

        -   Add Azure App Service Deploy task

    -   Add a Jenkins artifact, pointing to your Jenkins service endpoint

-   Configure the tasks for your environment

    -   App type: Linux App

    -   App service name: best-for-you-app-SUFFIX

    -   Registry or namespace: bestforyouregistrySUFFIX

    -   Repository: best-for-you-organics

*Exit criteria*

-   Your VSTS release definition successfully deploys updated images to your Web App for Containers

### Task 9: Trigger CI/CD pipeline

In this task, you will commit a change to the mcw-oss-paas-devops starter application, and trigger the full CI/CD pipeline through Jenkins and VSTS.

#### Task to complete

-   Add the following header to the src/components/plan/Plan.js file, just above the Grid containing plans.

-   Commit the change, and push it to your GitHub account

*Exit criteria*

-   The CI/CD pipeline completes successfully, and your app is updated

-   The deployment can take 30+ minutes, so move on to the next exercise, and come back and check the deployment progress periodically. ![Welcome to Best for You Organics Company! is highlighted above the Two Person Plan, Four Person Plan, and High-Pro Plan boxes in this screenshot of the starter application.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image26.png "Commit a change to the starter application")

## Exercise 6: Create Azure Function for order processing

Duration: 45 minutes

In this task, you will create an Azure Function that will be triggered by orders being added to the Orders collection in Azure Cosmos DB. This Function will trigger whenever a document in the orders collection is inserted or updated. The function checks the processed field on the order document, ensuring only unprocessed orders are sent to the processing queue.

### Task 1: Provision a Function App

In this task, you will create a Function App in Azure, which will host your Functions.

#### Task to complete

-   Create a Function App in the Azure portal

*Exit criteria*

-   You can navigate to your Function App in the Azure portal

### Task 2: Configure storage queues

In this task, you will add two storage queues to the storage account provisioned when you created your Function App. These queues will be used to store orders and notifications needing to be processed.

#### Task to complete

-   Create storage queues in the storage account associated with your Function App

    -   orderqueue

    -   notificationqueue

*Exit criteria*

-   You created the two queues

### Task 3: Create Cosmos DB trigger function

In this task, you will create a function that will be triggered whenever a document is inserted into the orders collection in your Azure Cosmos DB. This function sends all new orders to a queue for processing and shipping. This function will use a Cosmos DB trigger and an output binding to an Azure Storage Queue.

#### Task to complete

-   Create a Cosmos DB trigger function, named OrdersCosmosTrigger

    -   Connect to your Cosmos DB account

    -   Collection name: orders (case sensitive)

    -   Rename the Document collection parameter to newOrders

    -   Add an Azure Storage Queues output

        -   Name: outputQueue

        -   Queue name: orderqueue (case sensitive)

    -   Get the code for the OrdersCosmosTrigger function from the OrdersCosmosTrigger.js in the AzureFunctions folder in VS Code

-   Sign into the app, and place an order

    -   Email address: <demouser@bfyo.com>

    -   Password: Password.1!!

-   Update on order directly in Cosmos DB, setting the processed field to true

*Exit criteria*

-   You can see items processing through the OrdersCosmosTrigger function logs

    ![Two items are highlighted in the OrdersCosmosTrigger function logs.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image27.png "View items processing through the function logs")

-   Verify items are being written to the order queue in the Azure Storage account 

    ![Refresh is highlighted on the Messages blade, and two items are displayed under Message Text.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image28.png "Verify that items are being written to the order queue")

### Task 4: Create Queue function

In this task, you will create a second function which will be triggered the output of the OrdersCosmosTrigger function. This will simulate the order processing, and will add items to the notificationqueue, if the order processing is complete, and sendNotifications is true for the order.

This will use an Azure Storage Queue trigger, and an input dataset from Cosmos DB, pulling in customers. Output dataset will be Azure Cosmos DB orders table, and an update to set processed = true, and the processedDate to today.

#### Task to complete

-   Add a Queue trigger Function, using the output from the OrdersCosmosTrigger as its trigger

    -   Name: ProcessOrders

    -   Queue name: orderqueue (case sensitive)

    -   Rename the Message parameter name to orderToProcess

    -   Add an Azure Cosmos DB input, connected to the users collection in your Cosmos DB

    -   Add an Azure Queue Storage output

        -   Message parameter name: outputQueue

        -   Queue name: notificationqueue (case sensitive)

    -   Get the code for the ProcessOrders function from VS Code, in the AzureFunctions folder, select ProcessOrders.js

-   Register a new account in the app, and use a cell phone number where you can receive text message in the phone field

-   Sign in and place an order

*Exit criteria*

-   Verify you can see the ProcessOrders Function processing records in the Azure portal

    ![One item is highlighted in this screenshot of the ProcessOrders Function processing records.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image29.png "Verify that you can see processing records")

##  Exercise 7: Create Logic App for sending SMS notifications

Duration: 30 minutes

In this exercise you will create Logic App which will trigger when an item is added to the notificationqueue Azure Storage Queue. The Logic App will use a Twilio connection to send an SMS message to the phone number included in the notificationqueue message.

### Task 1: Create Free Twilio account

In this task, you will use a free Twilio account to send SMS notifications to customers, informing them that their order has been processed, and is on its way.

#### Task to complete

-   Create a free Twilio account at <https://www.twilio.com/try-twilio>

-   Get a free phone number with SMS capabilities

*Exit criteria*

-   You have a Twilio phone number

### Task 2: Create Logic App

In this task, you will create a new Logic App, which will use the Twilio connector to send SMS notifications to users, informing them that their weekly order has processed and shipped.

#### Task to complete

-   Provision a Logic App in the Azure portal, named OrderNotifications

-   Create a new blank Logic App in the designer

    -   Add an Azure Queue connector, which triggers when there are messages in the queue

    -   Connect to the notificationqueue in your Function App's storage account

    -   Check for new messages every 1 minute

-   Add a Data Operations -- Parse JSON action

    -   Set the content to the Message Text from the notificationqueue messages

    -   Generate a schema by using one of the messages in the notificationqueue as a template

-   Add a Twilio -- Send Text Message (SMS) action

    -   Connect to your Twilio account

    -   Set the From phone number to your Twilio phone number

    -   Set the To phone number to the notificationPhone parameter from the Parse JSON action

    -   Set the Text to "Hello \[firstName\], your Best for You Organics weekly order has shipped!

        -   \[firstName\] is the firstName parameter from the Parse JSON action

-   Add an Azure Queues -- Delete Message action

    -   Queue name: notificationqueue

    -   Set the Message ID to the Message ID parameter from the When there are messages in the queue action

    -   Set the Pop Receipt to the Pop Receipt parameter from the When there are messages in the queue action

*Exit criteria*

-   You receive a text message when you place in order from the account with your cell phone number.

## After the hands-on lab 

Duration: 10 minutes

In this exercise, you will deprovision all Azure resources that were created in support of this hands-on lab.

### Task 1: Delete Azure resource groups

1.  In the Azure portal, select Resource groups from the left-hand menu, and locate and delete the following resource groups.

    a.  hands-on-labs-SUFFIX

    b.  jenkins-SUFFIX

### Task 2: Delete WebHooks and Service Integrations

1.  In your GitHub account

    a.  Delete the Jenkins service integration

    b.  Delete the Personal Access Token you created for integration with VS Code.

2.  In your VSTS account

    c.  Delete the Personal Access Token you created for integration with Jenkins.

    d.  Delete the Jenkins service endpoint you added

    e.  Delete the BestForYouOrganics CD release

You should follow all steps provided *after* attending the Hands-on lab.

## Appendix A: Lab VM setup

Appendix A provides steps for manually provisioning and setting up the Lab VM used as a development machine for this lab.

### Task 1: Create VM config script

In this task, you will create a script file that will be used as a custom extension for configuring your Linux virtual machine in Azure. This script contains commands to install all the required software and configure a desktop on the Linux VM. These commands could also be run from an SSH shell manually.

1.  Open a web browser, and navigate to <https://raw.githubusercontent.com/ZoinerTejada/mcw-oss-paas-devops/master/LabVM/labvmconfig.sh>.

2.  Copy the contents displayed in the browser into a text editor, such as Notepad, and save the file as **labvmconfig.sh**. Note the location you saved the file, as you will be referencing it in the next task.

### Task 2: Create a Linux virtual machine

In this task, you will provision a Linux virtual machine (VM) running Ubuntu Server 16.04 LTS.

1.  In the [Azure Portal](https://portal.azure.com/), select **+Create a resource**, then enter "ubuntu" into the search bar, and select **Ubuntu Server 16.04 LTS** from the results. 
    
    ![+ Create a resource is highlighted in the navigation pane of the Azure portal, ubuntu is highlighted in the search box, and the Ubuntu Server 16.04 LTS row is highlighted in the search results.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image30.png "Azure Portal")

2.  On the **Ubuntu Server 16.04 LTS** blade, select **Create**.

    ![On the Windows 10 Pro, Version 1709 blade, Create is highlighted below Select a deployment model: Resource Manager.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image31.png "Create button")

3.  Set the following configuration on the **Basics** tab.

    -   **Name:** Enter LabVM.

    -   **VM disk type:** Select **SSD**.

    -   **User name:** Enter demouser

    -   **Authentication Type:** Select **Password**

    -   **Password:** Enter Password.1!!

    -   **Subscription:** Select the subscription you are using for this hands-on lab

    -   **Resource Group:** Select **Create new**, and enter "hands-on-lab-(SUFFIX)" as the name of the new resource group

    -   **Location:** Select either **East US**, **West US 2**, **West Europe**, or **Southeast Asia**, as these are currently the only regions which offer Dv3 and Ev3 VMs. Remember this location for other resources in this hands-on lab 
    
    ![Basics is selected on the Create virtual machine blade, and the information above is entered on the Basics blade at right.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image32.png "Create virtual machine and Basics blades")

4.  Select **OK** to move to the next step.

5.  On the **Choose a size** blade, select **View all**. This machine will be doing nested virtualization, so it needs to be in either the Dv3 or Ev3 series, so selecting **D2S\_V3 Standard** is a good baseline option. If that size is not available in the region you selected, go back to the **Basics** blade, and try one of the other regions listed above. 
    
    ![Size is selected on the Create virtual machine blade, the information above is entered on the Choose a size blade at right, and View all, and D2S\_V3 Standard are highlighted.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image33.png "Create virtual machine and Choose a size blades")

6.  Click **Select** to move on to the **Settings** blade.

7.  On the **Settings** blade, select **Network security group (firewall)**, then select **+Add an inbound rule** in the Create network security group blade. 
    
    ![Settings is selected on the Create virtual machine blade, the Network security group (firewall) option is selected on the Settings blade, and + Add an inbound rule is highlighted on the Create network security group blade.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image34.png "Various blades")

8.  On the **Add inbound security rule** blade, select **RDP** from the **Service** drop down, then select **OK**. 
    
    ![RDP is selected in the Service drop down on the Add inbound security rule blade.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image35.png "Add inbound security rule blade")

9.  Select **OK** on the Create network security group blade.

10. Next, select **Extensions** on the **Settings** blade, and select **Add extension**. 
    
    ![Settings is selected on the Create virtual machine blade, the Extensions option is selected on the Settings blade, and Add extension is highlighted on the Extensions blade.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image36.png "Various blades")

11. On the **New resource** blade, select **Custom Script for Linux**, then select **Create** on the **Custom Script for Linux** blade. By using a custom script, you can install software and configure the VM as part of the provisioning process.
    
    ![Custom Script for Linux is selected on the New resource blade, and the Custom Script for Linux blade is displayed on the right.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image37.png "New resource and Custom Script for Linux blades")

12. On the **Install extension** blade:

    -   **Script files**: Select the **labvmconfig.sh** file you saved in the previous task

    -   **Command**: Enter "bash labvmconfig.sh"

    -   Select **OK**
    
    ![The Install extension blade is displayed and the information above are entered into the fields.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image38.png "Install extension blade")

13. Select **OK** on the Extension blade.

14. Select **OK** on the Settings blade.

15. Select **Create** on the **Create** blade to provision the virtual machine. 

    -   ![Summary is selected on the Create virtual machine blade, and Create is selected on the Create blade at right.](images/Hands-onlabunguided-OSSPaaSandDevOpsimages/media/image39.png "Create virtual machine and Create blades")

16. It may take 10+ minutes for the virtual machine to complete provisioning. You can move on to the next task while you wait for this to complete.