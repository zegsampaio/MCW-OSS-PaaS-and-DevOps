![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
OSS PaaS and DevOps
</div>

<div class="MCWHeader2">
Hands-on lab step-by-step
</div>

<div class="MCWHeader3">
June 2020
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2020 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

- [OSS PaaS and DevOps hands-on lab step-by-step](#oss-paas-and-devops-hands-on-lab-step-by-step)
  - [Abstract and learning objectives](#abstract-and-learning-objectives)
  - [Overview](#overview)
  - [Solution architecture](#solution-architecture)
  - [Requirements](#requirements)
  - [Exercise 1: Run starter application](#exercise-1-run-starter-application)
    - [Task 1: Connect to your Lab VM](#task-1-connect-to-your-lab-vm)
    - [Task 2: Grant permissions to Docker](#task-2-grant-permissions-to-docker)
    - [Task 3: Integrate GitHub into VS Code](#task-3-integrate-github-into-vs-code)
    - [Task 4: Clone the starter application](#task-4-clone-the-starter-application)
    - [Task 5: Launch the starter application](#task-5-launch-the-starter-application)
  - [Exercise 2: Migrate the database to Cosmos DB](#exercise-2-migrate-the-database-to-cosmos-db)
    - [Task 1: Create and scale collections](#task-1-create-and-scale-collections)
    - [Task 2: Update database connection string](#task-2-update-database-connection-string)
    - [Task 3: Import data to the API for MongoDB using mongoimport](#task-3-import-data-to-the-api-for-mongodb-using-mongoimport)
    - [Task 4: Install Azure Tools extensions for VS Code](#task-4-install-azure-tools-extensions-for-vs-code)
    - [Task 5: Decrease collection throughput](#task-5-decrease-collection-throughput)
  - [Exercise 3: Containerize the app](#exercise-3-containerize-the-app)
    - [Task 1: Open Azure Container Registry with Docker extension in VS Code](#task-1-open-azure-container-registry-with-docker-extension-in-vs-code)
    - [Task 2: Create a Docker image and run the app](#task-2-create-a-docker-image-and-run-the-app)
    - [Task 3: Run the containerized app](#task-3-run-the-containerized-app)
    - [Task 4: Push the image to Azure Container Registry](#task-4-push-the-image-to-azure-container-registry)
  - [Exercise 4: Set up Web App for Containers](#exercise-4-set-up-web-app-for-containers)
    - [Task 1: Provision Web App for Containers](#task-1-provision-web-app-for-containers)
    - [Task 2: Navigate to the deployed app](#task-2-navigate-to-the-deployed-app)
  - [Exercise 5: Configure CI/CD pipeline](#exercise-5-configure-cicd-pipeline)
    - [Task 1: Enable Continuous Deployment on Web App](#task-1-enable-continuous-deployment-on-web-app)
    - [Task 2: Prepare GitHub account for service integrations](#task-2-prepare-github-account-for-service-integrations)
    - [Task 3: Open a connection to Jenkins](#task-3-open-a-connection-to-jenkins)
    - [Task 4: Configure Continuous Integration with Jenkins](#task-4-configure-continuous-integration-with-jenkins)
    - [Task 5: Trigger CI build](#task-5-trigger-ci-build)
    - [Task 6: Install Docker on the Jenkins VM](#task-6-install-docker-on-the-jenkins-vm)
    - [Task 7: Add continuous delivery to Jenkins build job](#task-7-add-continuous-delivery-to-jenkins-build-job)
    - [Task 8: Trigger CI-CD pipeline](#task-8-trigger-ci-cd-pipeline)
  - [Exercise 6: Create Azure Functions for order processing](#exercise-6-create-azure-functions-for-order-processing)
    - [Task 1: Configure storage queues](#task-1-configure-storage-queues)
    - [Task 2: Create a timer triggered function](#task-2-create-a-timer-triggered-function)
    - [Task 3: Create Queue function](#task-3-create-queue-function)
  - [Exercise 7: Create Logic App for sending email notifications](#exercise-7-create-logic-app-for-sending-email-notifications)
    - [Task 1: Create a SendGrid account](#task-1-create-a-sendgrid-account)
    - [Task 2: Create a Logic App](#task-2-create-a-logic-app)
  - [After the hands-on lab](#after-the-hands-on-lab)
    - [Task 1: Delete Azure resource groups](#task-1-delete-azure-resource-groups)
    - [Task 2: Delete WebHooks and Service Integrations](#task-2-delete-webhooks-and-service-integrations)

# OSS PaaS and DevOps hands-on lab step-by-step

## Abstract and learning objectives

In this hands-on lab, you implement a solution for integrating and deploying complex open-source software (OSS) workloads into Azure PaaS. You migrate an existing MERN (MongoDB, Express.js, React.js, Node.js) stack application from a hosted environment into Azure Web App for Containers, migrate a MongoDB instance into Cosmos DB, enhance application functionality using serverless technologies, and fully embrace modern DevOps tools.

At the end of this hands-on lab, you will be better able to migrate and deploy OSS applications into Azure PaaS using modern DevOps methodologies and Docker containers.

## Overview

Best For You Organics Company is one of the leading online health food suppliers in North America, serving customers in Canada, Mexico, and the United States. They launched their highly-successful e-commerce website, which sells subscriptions to their meal service, in 2016, and have been steadily increasing their subscriber-base since. Their service caters to working professionals looking for convenient, reliable access to healthy meal choices, and pre-packaged recipes without having to spend too much time preparing the meals.

Their CIO is a big proponent of open-source software, and their web application uses the MERN stack (MongoDB, Express.js, React.JS, Node.js). They host their code in a private GitHub repository. They currently have a continuous integration workflow, triggered by each code check-in/commit in GitHub, using Jenkins.

As their service has grown, they have found that the management of VM and server infrastructure is a real challenge. They want to learn more about how Platform as a Service (PaaS) solutions for OSS applications on Azure might be able to help. Their goal is to focus their expenditures and efforts on their core business, rather than infrastructure. The development team at Best For You Organics has indicated they have some experience with Docker. They are interested in what options might be available for using containers to deploy their application into a cloud environment. They are also interested in learning more about identity management.

The development team has also expressed that they would like to continue using GitHub as their code repository but is interested in improving upon their DevOps pipeline. They currently use Jenkins for their builds and are interested in any tools available in a cloud offering that could help with release management, or other aspects of a fully-integrated, modern DevOps pipeline. Ultimately, their goal is to automate and simplify deployments through CI/CD capabilities and deliver updates faster and more reliably.

## Solution architecture

Below is a diagram of the solution architecture you will build in this lab. Please study this carefully, so you understand the whole of the solution as you are working on the various components.

![This diagram consists of icons that are connected by arrows. On the left, the Developer icon (VS Code) points linearly to the GitHub Repo and Jenkins icons. The previous two icons are enclosed in a box labeled CI/CD Pipeline. Jenkins points to Web App for Containers on the right. Various arrows point from Web App for Containers to Azure Container Registry (a double-sided arrow); Logic Apps (a linear arrow that also points from Logic Apps to Customers); Customers (a linear arrow); and Azure Cosmos DB (a double-sided arrow that also points from Azure Cosmos DB to Azure Functions with another double-sided arrow).](media/solution-architecture-diagram.png "Solution architecture diagram")

The solution begins with developers using Visual Studio Code (VS Code) as their code editor. In VS Code, they can leverage rich integrations with GitHub, Docker, and Azure. From a high level, developers will package the entire OSS application inside a custom Docker container using the Docker extension in VS Code. The image will be pushed to an Azure Container Registry as part of a continuous integration/continuous delivery (CI/CD) pipeline using GitHub and Jenkins. This Docker image will then be deployed to a Web App for Containers instance, as part of their continuous delivery process using The Azure App Service Jenkins plugin.

The MongoDB database will be imported into Azure Cosmos DB, using mongoimport.exe, and access the database from the application will continue to use the MongoDB APIs. The database connection string in the application will be updated to point to the new Cosmos DB.

Serverless architecture is applied to order processing and customer notifications. Azure Functions are used to automate the processing of orders, billing credit cards, and updating the database as order processing is completed. Logic Apps are applied to send SMS notifications via a Twilio connector to customers, informing them that their order has been processed and shipped.

In this hands-on lab, you will assist them with completing the OSS application and database migrations into Azure. You will implement a containerized solution. For this, you will create a custom Docker image and push it to an Azure Container Registry. You will then configure a CI/CD pipeline to deploy the application to a Web App for Containers instance. You will also help them implement functionality enhancements using serverless architecture services in Azure.

## Requirements

1. Microsoft Azure subscription must be pay-as-you-go or MSDN.
   - Trial subscriptions will _not_ work.
2. Linux virtual machine configured with:
   - Visual Studio Code
   - Azure CLI
   - Docker
   - Node.js and npm
   - MongoDB Community Edition

## Exercise 1: Run starter application

Duration: 30 minutes

In this exercise, you create a local copy of the starter application on your Lab VM, add some sample data to the local MongoDB database, and run the application.

### Task 1: Connect to your Lab VM

In this task, you create an RDP connection to your Lab VM. If you are already connected, skip to [Task 2](#task-2-grant-permissions-to-docker).

1. In the [Azure portal](https://portal.azure.com) and select **Resource groups** from the Azure services list.

   ![Resource groups is highlighted in the Azure services list.](media/azure-services-resource-groups.png "Azure services")

2. Select the hands-on-lab-SUFFIX resource group from the list.

   ![Resource groups is selected in the Azure navigation pane, and the "hands-on-lab-SUFFIX" resource group is highlighted.](./media/resource-groups.png "Resource groups list")

3. Select **LabVM** from the list of available resources.

   ![LabVM is highlighted in the list of available resources.](media/rg-labvm.png "Select LabVM")

4. On the **LabVM** Overview blade, select **Connect** from the toolbar and then select **RDP** from the drop-down menu.

   ![Connect -> RDP is highlighted in the toolbar for the LabVM.](media/labvm-connect-rdp.png "LabVM toolbar")

5. On the Connect with RDP screen, select **Download RDP File**.

   ![The Download RDP File button is highlighted on the Connect with RDP dialog.](media/labvm-connect-with-rdp.png "Connect with RDP")

6. Open the download file to launch a Remote Desktop Client (RDP) application.

7. Select **Connect** on the Remote Desktop Connection dialog.

8. Select **Yes** to connect, if prompted that the identity of the remote computer cannot be verified.

   ![This is a screenshot of the Remote Desktop Connection prompt about connecting to the remote despite the identity of the remote computer being unverified. Yes is selected.](media/remote-desktop-connection.png "Remote Desktop Connection dialog box")

9. Enter the following credentials (or the non-default credentials if you changed them):

   - **Username:** demouser
   - **Password:** Password.1!!

   ![The credentials above are entered in the Login to xrdp dialog box.](media/login-to-xrdp.png "Login to xrdp dialog box")

10. Select **OK** to log into the Lab VM.

### Task 2: Grant permissions to Docker

In this task, you will grant permissions to the _demouser_ account to access the Unix socket needed to communicate with the Docker engine.

1. On your Lab VM, open a **Bash** shell by selecting the Start menu and then expanding Debian, Applications, and Shells.

   ![In the Lab VM start menu, Debian -> Applications -> Shells is expanded, and Bash is highlighted in the Shells menu.](media/labvm-menu-bash-shell.png "Start menu")

2. At the command prompt, enter the following command:

   ```bash
   sudo usermod -a -G docker $USER
   ```

3. After running the command, reboot the Lab VM using the following command at the bash shell:

   ```bash
   sudo reboot
   ```

4. Log back in using the same credentials:

   - **Username:** demouser
   - **Password:** Password.1!!

5. After logging back in, relaunch the bash shell and run the following command to test that the demouser account has proper permissions:

   ```bash
   docker run hello-world
   ```

   ![Screenshot of the bash shell command sudo usermod -a pG docker $USER output. Includes output from running the test command, docker run hello-world.](media/bash-docker-hello-world.png "Bash Shell")

### Task 3: Integrate GitHub into VS Code

In this task, you will install the GitHub extension in VS Code and configure a service integration with your GitHub account. This integration will allow you to push your code changes to GitHub directly from VS Code.

1. On your Lab VM, open **VS Code** under Programming on the Start menu.

   ![Visual Studio Code is highlighted under Programming on the start menu.](media/labvm-menu-programming-vscode.png "Visual Studio Code")

2. In VS Code, select the **Extensions** icon from the left-hand menu, enter "github" into the **Extensions** search box, and select the **GitHub** extension.

   ![Extensions is highlighted in the Visual Studio Code navigation pane, github is entered in the Extensions search box, and GitHub is highlighted below.](media/vscode-extensions-github.png "Visual Studio Code navigation pane")

3. Select **Install** in the Extension: GitHub window.

   ![Install is highlighted in the Extension: GitHub window.](media/vscode-extensions-github-install.png "Install the GitHub extension")

4. To connect VS Code with your GitHub account, you need to generate a Personal access token.

5. Open a browser window and navigate to your GitHub account (<https://github.com>).

6. Within your GitHub account, select **your user profile icon** in the top right, then select **Settings** from the menu.

   ![The user profile icon is highlighted at the top left of the GitHub account page, and Settings is highlighted in the sub-menu.](media/github-profile-settings.png "Select your account settings")

7. On the Settings screen, select **Developer settings** at the bottom of the Personal settings menu on the left-hand side of the screen.

   ![Developer settings is highlighted at the bottom of the Personal settings menu on the Settings screen.](media/github-developer-settings.png "Select Developer settings")

8. On the Developer settings page, select **Personal access tokens** from the left-hand menu.

   ![Personal access tokens is highlighted on the Developer settings page.](media/github-personal-access-tokens.png "Select Personal access tokens")

9. Select **Generate new token**.

   ![The Generate new token button is highlighted on the Personal access tokens page.](media/github-generate-new-token.png "Select Generate new token")

10. Enter a token description, such as "VS Code Integration", and then check the box next to **repo** under **Select scopes**, which will select all the boxes under it.

    ![On the New personal access token page, VS Code Integration is entered under Token description, and the checkboxes next to and under repo are selected under Select scopes.](media/github-new-personal-access-token.png "Select the scopes")

11. Select **Generate token** near the bottom of the screen.

    ![The Generate token button is displayed.](media/github-generate-token.png "Generate token button")

12. Select the **copy** button next to the token that is generated.

    ![The copy button that is next to the new personal access button is highlighted.](media/github-copy-personal-access-token.png "Copy the new personal access button")

    > **Important**: Make sure you copy the new personal access token before you navigate away from the screen, or you will need to regenerate the token. Save the copied token by pasting it into a text editor for future reference. This will also be used as your password when pushing changes to GitHub.

13. Open **VS Code** on your Lab VM.

14. Select the **View** menu, then select **Command Palette** from the menu.

    ![View is highlighted in the menu, and Command Palette is highlighted below.](media/vscode-view-menu-command-palette.png "Visual Studio Code window, View menu")

15. In the box that appears at the top center of the VS Code window, enter "Set Personal Access Token," then select **GitHub: Set Personal Access Token**, when it appears.

    ![The VS Code command palette displays Set Personal text in the textbox and the GitHub: Set Personal Access Token item is selected in the suggested commands list.](media/vscode-command-palette-set-personal-access-token.png "Select GitHub: Set Personal Access Token")

16. Paste the Personal access token you copied from GitHub into the box and press **Enter**.

    ![The Personal access token is pasted in the box.](media/github-paste-personal-access-token.png "Paste the Personal access token")

17. VS Code is now connected to your GitHub account.

18. Close VS Code.

### Task 4: Clone the starter application

In this task, you will clone the starter application, creating a local copy on your Lab VM.

1. On your Lab VM, open a browser and navigate to your GitHub account (<https://github.com>).

2. Within your GitHub account, navigate to the forked copy of the `MCW-OSS-PaaS-and-Devops` application page, select **Clone or download**, then select the **copy** link next to the web URL.

   ![The Clone or download button is selected on the forked copy of the MCW-OSS-PaaS-and-Devops application page, and the web URL is displayed below it.](media/github-clone.png "Select Clone or download")

3. Open a new bash shell, and enter the following command:

   ```bash
   cd ~
   ```

4. Next, enter the following command, replacing `[EMAIL]` with the email address you used when creating your GitHub account. This will associate your git email address with the commits made from the Lab VM.

   ```bash
   git config --global user.email "[EMAIL]"
   ```

   ![The above command to associate your git email address with the commits made from the Lab VM is displayed in the bash terminal window.](media/bash-git-config.png "bash terminal window")

5. At the prompt, enter the following command, replacing `[CLONE-URL]` with URL you copied from GitHub in step 2 above:

   ```bash
   git clone [CLONE-URL]
   ```

   ![The git clone command is displayed in the bash terminal window.](media/bash-git-clone.png "bash terminal window")

6. Now, change the directory to the cloned project by entering the following at the prompt:

   ```bash
   cd MCW-OSS-PaaS-and-DevOps
   ```

   ![The command to change the directory is displayed in the bash terminal window.](media/bash-cd-mcw-oss-paas-and-devops.png "bash terminal window")

7. Finally, issue a command to open the starter project in VS Code by typing:

   ```bash
   code .
   ```

8. A new VS Code window will open, with the `MCW-OSS-PaaS-and-DevOps` folder opened.

   ![The MCW-OSS-PaaS-and-Devops folder is open in the Explorer pane in the Visual Studio Code window, and the Welcome pane is open on the right.](media/vscode-mcw-oss-paas-and-devops.png "Visual Studio Code window")

9. You are now ready to begin working with the project in VS Code.

### Task 5: Launch the starter application

In this task, you will seed the MongoDB with sample data, then run the application locally, connected to your MongoDB instance. This task is to verify the connection to MongoDB and that it contains the seeded plan data before we migrate the application and data to Azure Cosmos DB.

1. Return to VS Code, select **Terminal** from the menu, and select **New Terminal**.

   ![New Terminal is highlighted in the Terminal menu in Visual Studio Code.](media/vscode-terminal-menu.png "Visual Studio Code Terminal menu")

2. This will open a new bash terminal window at the bottom of the VS Code dialog.

3. Change directories to the `Hands-on lab/lab-files` folder by running the following command. **Important**: Be sure to include the quotes around the path, since it contains a space.

   ```bash
   cd "Hands-on lab/lab-files"
   ```

4. At the bash prompt, use the `npm install` command to ensure the required components are installed on your Lab VM.

   ```bash
   sudo npm install
   ```

   ![This is a screenshot of the terminal window at the bottom of the Visual Studio Code dialog box. The command above is entered at the command prompt, and the results of the command are displayed.](media/vscode-terminal-npm-install.png "Bash terminal window")

5. Next, enter the following to seed the local MongoDB database with plans, user accounts, and orders.

   ```bash
   node data/Seed.js
   ```

   ![The above command to seed the local MongoDB database is displayed in the bash terminal window.](media/vscode-terminal-seedjs.png "Bash terminal window")

   > **Note**: If you receive an error that the service is not running, start the MongoDB service by executing the `sudo service mongod start` command at the shell prompt.

6. Next, build the application using the following:

   ```bash
   sudo npm run build
   ```

7. Finally, enter the following to start the web server for the application. You will see a `connection successful` message in the terminal window.

   ```bash
   npm start
   ```

8. Open a browser and navigate to <http://localhost:3000> to view the landing page of the starter application. You will see three plans listed on the application home page, which are pulled from the local MongoDB database.

   ![Two Person Plan, Four Person Plan, and High-Pro Plan boxes are visible in this screenshot of the application home page.](media/bfyo-web-home.png "View the three plans")

9. Return to the VS Code integrated terminal window, and press **CTRL+C** to stop the application.

## Exercise 2: Migrate the database to Cosmos DB

Duration: 30 minutes

In this exercise, you update the starter application's database connection string to point to your Azure Cosmos DB account. You then, use `mongoimport.exe` to migrate the data in your MongoDB database into Cosmos DB collections, and verify with the application that you are connected to your Cosmos DB database.

### Task 1: Create and scale collections

In this task, you create the collections needed for your database migration and increase each collection's throughput from the default 400 RUs to 2,500 RUs. This is done to avoid throttling during the migration, and reduce the time required to import data.

> To learn more about RUs and throughput provisioning in Cosmos DB, read [Request Units in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/request-units).

1. Navigate to your Azure Cosmos DB account in the Azure portal by selecting **Resource groups** from the Azure home page, and then select the **best-for-you-db-SUFFIX** Azure Cosmos DB account resource from the list.

2. On the Cosmos DB blade, select **Data Explorer** from the left-hand menu and then select **New Collection**.

   ![Data Explorer is selected and highlighted in the left-hand menu of the Azure portal, and New Collection is highlighted on the right.](media/cosmos-db-add-collection.png "Azure Cosmos DB")

3. In the **Add Collection** dialog, enter the following:

   - **Database id**: Choose **Create new**, and enter **best-for-you-organics**.
   - **Provision database throughput**: Uncheck this box.
   - **Collection id**: Enter **orders**.
   - **Storage capacity**: Select **Fixed (10 GB)**.
   - **Throughput**: Enter **2500**.
   - Select **OK** to create the collection.

   ![The information above is entered in the Add Collection dialog box.](media/cosmos-db-new-collection-orders.png "Add Collection")

4. On the Collections blade, select **New Collection** again to create another collection.

   ![The New Collection button is highlighted on the Cosmos DB Collections blade.](media/cosmos-db-new-collection.png "New Collection")

5. On the Add Collection dialog, enter the following:

   - **Database id**: Select **Use existing** and select the **best-for-you-organics** database from the list.
   - **Collection id**: Enter **users**.
   - **Storage capacity**: Select **Fixed (10 GB)**.
   - **Throughput**: Enter **2500**.
   - Select **OK** to create the collection.

   ![The information for the users collection above is entered into the Add Collection dialog.](media/cosmos-db-new-collection-users.png "Add Collection")

6. Repeat steps 4 and 5, this time entering **plans** as the collection name.

7. The best-for-you-organics database will have three collections listed under it when you are complete.

   ![The best-for-you-organics database is displayed, with orders, plans, and users collections under it.](media/cosmos-db-database-and-collections.png "Cosmos DB Collections")

### Task 2: Update database connection string

In this task, you will retrieve the connection string for your Azure Cosmos DB database and update the starter application's database connection string.

1. On the **Azure Cosmos DB account** blade, select **Connection String** under **Settings** in the left-hand menu, and copy the **PRIMARY connection string**.

   ![Connection String is selected under Settings on the Azure Cosmos DB account blade, and the Primary Connection String value is highlighted on the Read-write Keys tab on the right.](media/cosmos-db-connection-strings.png "Azure cosmos DB account blade")

2. Return to VS Code.

3. Open `app.js` from the `Hands-on lab/lab-files` directory of the application and locate the line that starts with **var databaseUrl** (line 16).

   ![App.js is selected and highlighted in the Explorer pane in the Visual Studio Code window, and the line starting with var databaseUrl is highlighted on the right.](media/vscode-app-js-database-url.png "Visual Studio Code window")

4. Replace the value of the **databaseUrl** variable with the Cosmos DB connection string you copied from the Azure portal.

   ![The value of the databaseUrl variable is displayed in the Visual Studio Code window.](media/vscode-app-js-connection-string.png "Visual Studio Code window")

5. The connection string copied from the portal does not specify the name of the database, so you now need to add that to the connection string. Scroll to the end of the value you just pasted in and locate **10255/?ssl=** in the string.

   ![10255/?ssl= is highlighted in the string in the Visual Studio Code window.](media/vscode-app-js-connection-string-database-name-position.png "Visual Studio Code window")

6. Between the "/" and the "?" insert **best-for-you-organics** to specify the database name.

   ![best-for-you-organics is selected in the string in the Visual Studio Code window.](media/vscode-app-js-connection-string-database-name.png "Visual Studio Code window")

7. Save `app.js`.

8. In the VS Code integrated terminal, enter the following command to rebuild the application:

   ```bash
   sudo npm run build
   ```

9. When the build completes, start the application by typing the following:

   ```bash
   npm start
   ```

10. Return to your browser and refresh the application page. Note: You may need to press **CTRL+F5** in your browser window to clear the browser cache while refreshing the page.

    ![This is a screenshot of the refreshed application page.](media/bfyo-web-home-empty.png "Refreshed application screenshot")

    > **Note**: Notice the three plans that were displayed on the page previously are no longer there. The application is now pointing to your Azure Cosmos DB, and the plans collection does not contain any data yet.

11. Now, let's move on to copying the data from the local MongoDB instance into Cosmos DB.

### Task 3: Import data to the API for MongoDB using mongoimport

In this task, you will use `mongoimport.exe` to import data to your Cosmos DB account. There is a shell script located in the `MCW-OSS-PaaS-and-DevOps` project, which handles exporting the data out of your MongoDB into JSON files on the local file system. These files will be used for the import into Cosmos DB.

1. On your Lab VM, open a new integrated bash prompt in VS Code by selecting the **+** next to the shell dropdown in the integrated terminal pane.

   ![This is a screenshot of the terminal window at the bottom of the Visual Studio Code dialog box. The + button is highlighted.](media/vscode-terminal-new.png "Bash terminal window")

2. At the prompt, enter the following command to grant execute permissions on the export script:

   ```bash
   chmod +x "Hands-on lab/lab-files/data/mongo-export.sh"
   ```

3. Next, run the script by entering the following command:

   ```bash
   "Hands-on lab/lab-files/data/mongo-export.sh"
   ```

   ![The above command to export data from the local MongoDB database is displayed in the bash terminal window.](media/vscode-terminal-mongo-export.png "Bash terminal window")

4. The script creates the folder ~/MongoExport, and exports each of the collections in your MongoDB to JSON files. Navigate to the MongoExport directory for your user account by running the following command:

   ```bash
   cd ~/MongoExport
   ```

5. You are now ready to import the data into Azure Cosmos DB using `mongoimport.exe`.

6. You will use the following command template for executing the data import:

   ```bash
   mongoimport --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file <your_collection>.json
   ```

7. To get the values needed for the template command above, return to the **Connection string** blade for your Azure Cosmos DB account in the Azure portal. Leave this window up, as you will need the **HOST**, **USERNAME**, and **PRIMARY PASSWORD** values from this page to import data to your Cosmos DB account.

   ![Connection String is selected under Settings on the Azure Cosmos DB account blade. On the Read-write Keys tab on the right, the values under Host, Username, and Primary Password are highlighted.](media/cosmos-db-connection-strings-values.png "Azure Cosmos DB account blade")

8. Replace the values as follows in the template command above:

   - `<your_hostname>`: Copy and paste the **Host** value from your **Cosmos DB Connection String** blade.
   - `<your_username>`: Copy and paste the **Username** value from your **Cosmos DB Connection String** blade.
   - `<your_password>`: Copy and paste the **Primary Password** value from your **Cosmos DB Connection** **String** blade.
   - `<your_database>`: Enter "best-for-you-organics"
   - `<your_collection>`: Enter "plans" (Note there are two instances of `<your-collection>` in the template command).

9. Your final command should look something like:

   ```bash
   mongoimport --host best-for-you-db.documents.azure.com:10255 -u best-for-you-db -p miZiDmNrn8TnSAufBvTQsghbYPiQOY69hIHgFhSn7Gf10cvbRLXvqxaherSKY6vQTDrvHHqYyICP4OcLncqWew== --db best-for-you-organics --collection plans --ssl --sslAllowInvalidCertificates --type json --file plans.json
   ```

10. Copy and paste the final command at the command prompt to import the plans collection into Azure Cosmos DB.

    ![The final command to import the plans collection into Azure Cosmos DB is displayed in the Command Prompt window.](media/vscode-terminal-mongoimport.png "Bash terminal window")

    > **Note**: You will see a message indicating the number of documents imported, which should be 3 for plans.

11. Verify the import by selecting **Data Explorer** in your Cosmos DB account in the Azure portal, expanding plans, and selecting **Documents**. You will see the three documents imported listed.

    ![Data Explorer is selected and highlighted on the Azure Cosmos DB account blade. On the Collections blade, the turned delta next to plans is highlighted, and Documents is selected and highlighted below it. On the Mongo Documents tab to the right, the first of three imported documents is selected and highlighted.](media/cosmos-db-plans-documents.png "Azure Cosmos DB account blade")

12. Repeat step 8 for the users and orders collections, replacing the `<your_collection>` values with:

    - users:

    ```bash
    mongoimport --host best-for-you-db.documents.azure.com:10255 -u best-for-you-db -p miZiDmNrn8TnSAufBvTQsghbYPiQOY69hIHgFhSn7Gf10cvbRLXvqxaherSKY6vQTDrvHHqYyICP4OcLncqWew== --db best-for-you-organics --collection users --ssl --sslAllowInvalidCertificates --type json --file users.json
    ```

    - orders:

    ```bash
    mongoimport --host best-for-you-db.documents.azure.com:10255 -u best-for-you-db -p miZiDmNrn8TnSAufBvTQsghbYPiQOY69hIHgFhSn7Gf10cvbRLXvqxaherSKY6vQTDrvHHqYyICP4OcLncqWew== --db best-for-you-organics --collection orders --ssl --sslAllowInvalidCertificates --type json --file orders.json
    ```

13. To verify the starter application is now pulling properly from Azure Cosmos DB, return to your browser running the starter application (<http://localhost:3000>), and refresh the page. You should now see the three plans appear again on the home page. These were pulled from your Azure Cosmos DB database.

    ![Two Person Plan, Four Person Plan, and High-Pro Plan boxes are visible in this screenshot of the application home page.](media/bfyo-web-home.png "View the three plans")

14. You have successfully migrated the application and data to use Azure Cosmos DB with MongoDB APIs.

15. Return to the Integrated terminal window of VS Code, which is running the application, and press **CTRL+C** to stop the application.

### Task 4: Install Azure Tools extensions for VS Code

In this task, you install the Azure Tools extensions for VS Code to take advantage of the integration with Azure Cosmos DB. This extension pack allows you to view and interact with your Cosmos DB databases, collections, and documents directly from VS Code.

1. Select the **Extensions** icon, enter "azure tools" into the search box, select the **Azure Tools** extension, and then select **Install** in the Extension: Azure Account window.

   ![The Extensions icon is highlighted on the left side of the VS Code window. On the right, "azure tools" is in the search box, Azure Tools is highlighted below it, and Install is highlighted on the right side.](media/vscode-extensions-cosmosdb.png "Install the Azure Tools extensions")

2. In the left-hand menu of VS Code, you should now see an Azure icon. Select that, and then select **Sign in to Azure**.

   ![Sign in to Azure is highlighted below AZURE COSMOS DB in the bottom left-hand corner of Visual Studio Code window.](media/vscode-azure-sign-in.png "Sign in to Azure")

   > **Note**: If you don't see the Azure icon, restart VS Code, and reopen the `MCW-OSS-PaaS-and-DevOps` project folder.

3. Enter your Azure account credentials in the browser window that appears.

4. If presented with a prompt to enter a password for a new keyring, enter "**Password.1!!**" as the password and select **Continue**.

   ![Choose password for new keyring dialog, with Password.1!! entered in the password and confirm boxes.](media/ubuntu-new-keyring.png "Choose password window")

5. Once you have signed in, you will receive a message that you can close the browser window.

6. Back in VS Code, you should now see your Azure account listed under Azure Cosmos DB, along with your Azure account email listed in the status bar of VS Code.

   ![Your Azure account is listed under Azure Cosmos DB, and your Azure email is listed in the status bar of the Visual Studio Code window.](media/vscode-azure-cosmosdb.png "Azure Cosmos DB window")

7. From here, you can view your databases, collections, and documents, as well as edit documents directly in VS Code, and push the updated documents back into your database.

### Task 5: Decrease collection throughput

In this task, you will decrease the throughput of your collections. Azure Cosmos DB uses an hourly billing rate, so reducing the throughput after the data migration will help save costs.

1. From the Azure Cosmos DB menu pane in Visual Studio Code, right-click on the **best-for-you-db** database and then select **Open in Portal**.

   ![Open in Portal is highlighted in the best-for-you-db content menu.](media/vscode-azure-cosmosdb-open-in-portal.png "Cosmos DB")

2. In the Azure portal, select **Scale** from the left-hand menu, under **Collections** on the Azure Cosmos DB account blade.

3. Expand the **orders** collection and select **Scale & Settings**.

4. Change the **Throughput** value to **400** and select **Save**.

   ![Scale is selected and highlighted on the left side of the Azure Cosmos DB account blade. On the Collections blade, Scale & Settings is selected and highlighted below orders. On the Scale & Settings tab to the right, 400 is entered and highlighted in the Throughput box, and the Save icon is highlighted above it.](media/cosmos-db-scale.png "Azure Cosmos DB account blade")

5. Repeat steps 3 & 4 for the **plans** and **users** collections.

## Exercise 3: Containerize the app

Duration: 30 minutes

This exercise walks you through containerizing the existing Best For You Organics Company MERN application using Docker, pushing the image to an **Azure Container Registry**, then deploying the image to **Web App for Containers** directly from VS Code.

### Task 1: Open Azure Container Registry with Docker extension in VS Code

The Docker extension for VS Code is used to simplify the management of local Docker images and commands, as well as the deployment of a built app image to Azure.

1. On your Lab VM, return to VS Code and the open `MCW-OSS-PaaS-and-DevOps` starter project.

2. You should see a Containers icon in the left-hand menu of Visual Studio Code. Select the **Containers** icon. Expand **Registries** and then expand **Azure**. If you do not see Azure listed, select **Connect registry** and select **Azure** from the list of registries. You should see the **bestforyouregistry** you created above listed.

   ![In the Docker VS Code extension, Registries and Azure are expanded, and the bestforyouregistry is highlighted.](media/vscode-extension-docker-registries.png "Docker extension")

   > **Note**: If you don't see the Containers icon in the left-hand menu, close and reopen VS Code, and the `MCW-OSS-PaaS-and-DevOps` project.

### Task 2: Create a Docker image and run the app

In this task, you use VS Code and the Docker extension to add the necessary files to the project to create a custom Docker image for the `MCW-OSS-PaaS-and-DevOps` app.

1. On your Lab VM, return to VS Code, and the `MCW-OSS-PaaS-and-DevOps` project.

2. Open the VS Code Command Palette, by selecting **View** from the menu, then **Command Palette**.

3. Enter "add docker" into the Command Palette and select **Docker: Add docker files to workspace**.

   ![Docker: Add docker files to workspace is selected under add docker that is entered in the Command Palette.](media/vscode-command-palette-add-docker.png "Select Docker: Add docker files to workspace")

4. At the **Select Application Platform** prompt, select **Node.js**.

   ![Node.js is highlighted at the Select Application Platform prompt.](media/vscode-command-palette-app-platform.png "Select Node.js")

5. At the **Include optional Docker Compose files?** prompt, select **Yes**.

   ![Yes is highlighted at the Include optional Docker Compose files prompt.](media/vscode-command-palette-docker-compose.png "Include optional Docker Compose files")

6. Ensure port "3000" is entered on the next screen, and press **Enter**.

   ![3000 is selected in the field above What port does your app listen on?](media/vscode-command-palette-docker-port.png "Explorer")

7. This will add Dockerfile, along with several configuration files for Docker compose to the project.

   ![Dockerfile and several configuration files are highlighted in the Explorer pane.](media/vscode-docker-files.png "Explorer pane")

8. Select **Dockerfile** from the file navigator and observe the contents. This file provides the commands required to assemble a Docker image for the `MCW-OSS-PaaS-and-DevOps` application.

   ![This is a screenshot of the Dockerfile contents. At this time, we are unable to capture all of the information in the window. Future versions of this course should address this.](media/vscode-docker-file.png "Dockerfile screenshot")

9. Currently, the Docker extension for Visual Studio Code places the generated Docker files in the root directory of the project. Because the project files are all located in the `lab-files` directory, we need to move the generated files into that folder. Select the `.dockerignore`, `docker-compose.debug.yml`, `docker-compose.yml`, and `Dockerfile` files, and then drag and drop them into the `Hands-on lab/lab-files` folder. Respond Yes if prompted about moving the files.

   ![The docker files specified above are highlighted within the Hands-on lab/lab-files folder.](media/vscode-docker-move-files.png "Visual Studio Code files")

10. Next, you will tell Docker to build an image for your app. Select the **Terminal** menu in Visual Studio Code, and then select **New Terminal**.

    ![New Terminal is highlighted in the Terminal menu in Visual Studio Code.](media/vscode-terminal-menu.png "Visual Studio Code Terminal menu")

11. In the new terminal window, change the directory to the `Hands-on lab/lab-files` folder by executing the following command:

    ```bash
    cd "Hands-on lab/lab-files"
    ```

12. With the next command you will provide a **registry**, **image name**, and **tag**, using the following format. This format will allow the image to be pushed to your container registry.

    > `[registry]/[image name]:[tag]`

13. For this, you will need the Login server value from your Azure Container registry's **Access keys** blade.

    ![Access keys is selected under Settings on the Azure Container registry's Access keys blade, and on the right, bestforyouregistry.azurecr.io is highlighted in the Login server box on the right.](media/acr-login-server.png "Container registry blade")

14. At the terminal prompt, enter the following, where `[Login server]` is the Login server value from Azure:

    ```bash
    docker build --rm -f "Dockerfile" -t [Login server]/best-for-you-organics:latest .
    ```

    > **Note**: Be sure to include the "." at the end of the line.

15. For example:

    ```bash
    docker build --rm -f "Dockerfile" -t bestforyouregistry.azurecr.io/best-for-you-organics:latest .
    ```

16. Press **Enter**, which will trigger the build of the image. You will see the following output in the terminal window:

    ![The docker build command above is entered into the VS Code terminal window.](media/vscode-terminal-docker-build.png "Docker build")

17. Once the build completes, select the **Docker** icon in the left-hand menu of Visual Studio Code and then expand **Images**. You will see the `best-for-you-organics` image.

    ![The image is highlighted in the DOCKER extension explorer under Images.](media/vscode-extensions-docker-images.png "Docker extension explorer")

18. You can also use the `docker images` command in the Integrated terminal to list the images.

    ![The docker images command is entered in the terminal window, with the list of images is displayed.](media/vscode-terminal-docker-images.png "Docker images")

### Task 3: Run the containerized app

In this task, you run the app from the container you built in the previous task.

1. In the **Images** area of the Docker extension in VS Code, expand the `best-for-you-organics` image and then right-click on the `latest` image and select **Run Interactive**.

   ![The image is selected in the Images area of the Docker extension explorer, and Run Interactive is highlighted in the shortcut menu.](media/vscode-extensions-docker-images-latest.png "Docker extension explorer")

   > **Note**: If you receive an error that the address is already in use, return to the terminal window where you ran the application using the `npm start` command and press `CTRL+C` to stop the application. Then, try the above action again.

2. Notice in the Interactive terminal that a docker run command is issued. Using the VS Code Docker extension, you can issue some docker commands without needing to work from the command line.

   ![The docker run command is highlighted in the Interactive terminal window.](media/vscode-terminal-docker-run.png "Interactive terminal window")

3. Verify the web app and container are functioning by opening a browser window and navigating to <http://localhost:3000>.

   ![Two Person Plan, Four Person Plan, and High-Pro Plan boxes are visible in this screenshot of the web app.](media/bfyo-web-home.png "View the web app")

4. In the Integrated terminal of VS Code, for the interactive session, press **CTRL+C** to stop the container.

### Task 4: Push the image to Azure Container Registry

In this task, you are going to push the image to your Azure Container Registry.

1. In the Azure portal, navigate to the hands-on-lab-SUFFIX resource group, and select the **bestforyouregistrySUFFIX** Container registry from the list of resources.

   ![The bestforyouregistry Container registry is highlighted in the list of resources on the Azure portal.](media/azure-rg-bestforyouregistry.png "List of resources")

2. On the **bestforyouregistrySUFFIX** blade, select **Access keys** under settings in the left-hand menu and then select **Enable** under **Admin user**.

    ![The Enable option is selected and highlighted for the Admin user on the Container registry Access keys blade.](media/container-registry-enable-admin-user.png "Enable admin user")

3. Leave this page up as you will be referencing the **Login server**, **Username**, and **password** in the next task.

   ![Access keys is selected under Settings on the Azure Container registry's Access keys blade, and the values in the Login server, Username, and Name boxes are highlighted on the right.](media/acr-access-keys.png "Container Registry blade")

4. Return to the Integrated terminal window in VS Code and enter the following command to log in to your Azure Container Registry, replacing the bracketed values with those from the container registry access keys page.

   ```bash
   docker login [Login Server] -u [Username]
   ```

5. For example:

   ```bash
   docker login bestforyouregistry.azurecr.io -u bestforyouregistry
   ```

6. Copy and paste the Container registry password when prompted to complete the login process.

   ![This is a screenshot of the password prompt in the Visual Studio Code Integrated terminal window.](media/vscode-terminal-docker-login.png "Integrated terminal window")

7. Once you are successfully logged in, expand the bestforyouregistry image in the Docker extension section of the VS Code, right-click on the latest image, and select **Push**.

   ![The image is selected in the Images area of the Docker extension explorer, and Push is highlighted in the shortcut menu.](media/vscode-docker-push.png "Docker extension explorer")

8. If prompted to set the default registry path, select **Yes**.

   ![Yes is highlighted on the dialog to set the default registry path.](media/vscode-extensions-docker-push.png "Set default registry path")

9. Note the "docker push" command issued in the terminal window.

   ![This is a screenshot of the docker push command in the Visual Studio Code Integrated terminal window.](media/vscode-terminal-docker-push.png "Integrated terminal window")

10. To verify the push, return to the **bestforyouregistry** Registry container blade in the Azure portal, and select **Repositories** under **Services** on the left-hand side, and note the **best-for-you-organics** repository.

   ![In your Azure Container Registry's repository, Repositories is selected and highlighted under Services, and best-for-you-organics is highlighted under Repositories.](media/acr-repositories.png "Container registry blade")

## Exercise 4: Set up Web App for Containers

Duration: 10 minutes

In this exercise, you deploy the containerized app to a Web App for Containers instance from the image stored in your Azure Container Registry.

### Task 1: Provision Web App for Containers

1. In the [Azure portal](https://portal.azure.com/), select the **Show portal menu** icon and then select **+Create a resource** from the menu.

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. Enter "web app for containers" into the Search the Marketplace box, select **Web App for Containers** from the results, and then select **Create**.

   ![Web App for Containers entered into the search box and is highlighted on the search results.](media/create-web-app-for-containers-resource.png "Web App for Containers")

3. On the **Web App Create** Basics Tab, enter the following:

   **Project Details**:

   - **Subscription**: Select the subscription you are using for this lab.
   - **Resource group**: Select **Create new** and enter hands-on-lab-web-SUFFIX for the name of the new resource group.

   **Instance Details**:

   - **Name**: Enter **best-for-you-app-SUFFIX** (the name must be globally unique).
   - **Publish**: Select **Docker Container**.
   - **Operating System**: Select **Linux**.
   - **Region**: Select the region you are using for resources in this hands-on lab.

   **App Service Plan**:

   - **Linux Plan**: Accept the default assigned value, which will create a new App Service plan.
   - **Sku and size**: Select **Change size**, choose **Dev/Test**, select the **B1** pricing tier, and select **Apply**.

   ![The values specified above are entered into the Create Web App Basics tab.](media/web-app-for-containers-basics-tab.png "Create Web App")

4. Select **Next: Docker**.

5. On the **Docker** tab, enter the following:

   - **Options**: Select **Single Container**.
   - **Image source:** Select **Azure Container Registry**.
   - **Registry**: Select **bestforyouregistrySUFFIX**.
   - **Image**: Select **best-for-you-organics**.
   - **Tag**: Select **latest**.
   - **Startup File**: Leave blank.

   ![The values specified above are set in the Docker tab.](media/web-app-for-containers-docker-tab.png "Create Web App")

6. Select **Review + create**, and then select **Create**.

### Task 2: Navigate to the deployed app

In this task, you navigate to the deployed app and log in to verify it is functioning correctly.

1. When you receive the notification that the Web App for Containers deployment has completed, navigate to the Web App by selecting the **notifications icon**, and selecting **Go to resource**.

   ![The Go to resource button is highlighted at the bottom of the successful deployment notification window.](media/azure-go-to-resource.png "Notifications window")

2. On the **Overview** blade of **App Service**, select the **URL** for the App Service.

   ![The URL for the App Service is highlighted on the Overview blade of App Service.](media/app-service-url.png "App Service blade")

3. A new browser window or tab will open, and you should see the `MCW-OSS-PaaS-and-DevOps` application's home page displayed.

4. Select **Sign In** on the top right of the page and login with the following credentials to verify everything is working as expected:

   - **Email address**: <demouser@bfyo.com>
   - **Password**: Password.1!!

   ![Two Person Plan, Four Person Plan, and High-Pro Plan boxes are visible in this screenshot of the MCW-OSS-PaaS-and-DevOps home page.](media/bfyo-web-login.png "Sign in to the MCW-OSS-PaaS-and-DevOps home page")

## Exercise 5: Configure CI/CD pipeline

Duration: 60 minutes

In this exercise, you are going to use Jenkins to implement a continuous integration (CI) and continuous delivery (CD) pipeline to deploy the containerized MERN app to Web App for Containers in Azure.

### Task 1: Enable Continuous Deployment on Web App

In this task, you will turn Continuous Deployment on for your Web App.

1. Return to your Web App for Containers blade in the Azure portal, select **Container settings** from the left-hand menu, and then select **On** under Continuous Deployment.

   ![The Web App blade is displayed, with Container settings selected and highlighted on the left, the On button for Continuous Deployment selected and highlighted, and the Save button highlighted.](media/web-app-continuous-deployment.png "Continuous Deployment")

2. Select **Save**.

### Task 2: Prepare GitHub account for service integrations

In this task, you add a Jenkins service integration into your GitHub account. This integration enables a Jenkins CI build job to be triggered when code is checked in to your GitHub repository.

1. On your Lab VM, navigate to your Jenkins VM in the [Azure portal](https://portal.azure.com) by selecting **Resource groups** from the Azure services list, and then selecting your **hands-on-lab-SUFFIX** or **hands-on-lab-jenkins-SUFFIX** resource group from the list.

   ![Resource groups is highlighted in the Azure services list.](media/azure-services-resource-groups.png "Azure services")

2. On the resource group blade, select your **Jenkins** virtual machine.

   ![The Jenkins VM is highlighted in this of resources in the hand-on-lab resource group.](media/rg-jenkins-vm.png "Resource group blade")

3. On the **Overview** blade of your Jenkins virtual machine, locate the **DNS name**, and copy the value.

   ![DNS name and its value are highlighted on the Overview blade.](media/jenkins-vm-dns-name.png "Overview blade")

4. Return to your forked `MCW-OSS-PaaS-and-DevOps` application page in GitHub, select **Settings**, then select **Webhooks** from the left-hand menu, and then select **Add Webhook**.

   ![Webhooks is highlighted on the left-hand menu, and Add webhook is highlighted at the top.](media/github-settings-webhooks.png "Select Webhooks")

5. If prompted, enter your GitHub account password to continue.

   ![A password is entered in the Confirm password to continue dialog box.](media/github-password.png "Confirm password dialog box")

6. On the Add webhook page, enter the following:

   - **Payload URL**: Enter <http://YOUR-JENKINS-URL/github-webhook/>, replacing `YOUR-JENKINS-URL` with the Jenkins DNS name you copied from the Azure portal. **Important**: Make sure to include the trailing backslash ("/").
   - **Content type**: Select **application/json**.
   - **Secret**: Leave blank.
   - Under **Which events would you like to trigger this webhook?**, select **Just the _push_ event.**
   - **Active**: Check this box.

   ![The value in the Payload URL box is highlighted in the Webhooks / Add Webhook dialog box.](media/github-add-webhook-settings.png "Jenkins webhook settings")

7. Select **Add webhook**.

8. A warning will be displayed. This is a permissions error that will be resolved in a later step.

   ![A warning icon is displayed due to an HTTP 403 error.](media/github-webhook-403-warning.png "Http Forbidden warning")

9. Next, you need to grant the Jenkins user access to your GitHub repository by adding a deploy key in the GitHub settings. Return to your Jenkins virtual machine page in the Azure portal, select **Connect**, and then **SSH** from the drop-down menu.

   ![The SSH option of the Connect button on the Jenkins VM overview blade is highlighted.](media/jenkins-connect.png "Connect")

10. On the Connect via SSH with client dialog, copy the **Run the example command below to connect to your VM** value.

    ![The copy button for the specified SSH command is highlighted.](media/jenkins-connect-ssh.png "Connect via SSH with client")

11. Paste the copied command into a text edit so that it can be modified prior to execution.

12. Edit the command to remove `-i <private key path>`, since the Jenkins instance was configured to use password authentication. The command should now look something like the following:

    ```bash
    ssh jenkinsadmin@jenkins-mcw.westus2.cloudapp.azure.com
    ```

13. Open a new bash shell and paste the modified SSH command at the prompt. Enter "yes" if prompted about continuing to connect and enter the jenkinsadmin password, `Password.1!!`, when prompted.

    ![The information above is displayed in this screenshot of the bash terminal.](media/bash-jenkins-ssh.png "bash terminal screenshot")

14. At the `jenkinsadmin@Jenkins` prompt, enter:

    ```bash
    ssh-keygen
    ```

15. Press **Enter** to accept the default file in which to save the key.

16. Press **Enter** to use an empty passphrase, and re-enter it to confirm.

    > **Note**: The use of an empty password is done only for simplicity in this hands-on lab. This is not recommended for actual environments.

17. Copy the location into which your public key has been saved.

    ![In this screenshot of the bash terminal, ssh-keygen and the location into which your public key has been saved are highlighted.](media/bash-jenkins-ssh-keygen.png "bash terminal screenshot")

18. Show the public key using the following command, replacing `[KEY-PATH]` with the location of your public key.

    ```bash
    cat [KEY-PATH]
    ```

19. Copy the key displayed so that it can be added to GitHub.

    ![The displayed key is highlighted in this screenshot of the bash terminal.](media/bash-jenkins-rsa-key.png "bash terminal screenshot")

20. Return to the Settings page of your forked repo in your GitHub account in the browser, select the **Deploy keys** option from the left-hand menu, and then select **Add deploy key**.

    ![Deploy keys banner is displayed, and the Add deploy key button is highlighted.](media/github-deploy-keys.png "Deploy keys")

21. Enter "Jenkins" for the title, paste the SSH key you copied above into the Key field, removing any trailing spaces, and select **Add key**.

    ![On the GitHub account page, Deploy keys is selected in the left-hand menu, Jenkins is in the Title box, and the SSH key that you copied is in the Key field.](media/github-deploy-keys-add.png "Add key")

22. To ensure that everything is working, return to the Jenkin's bash shell, and enter the below command which will check the connection to GitHub.

    ```bash
    ssh git@github.com
    ```

23. Enter "yes" when prompted about continuing.

24. You should see a message like the following, indicating a successful authentication and closed connection.

    ![A message indicating a successful connection is highlighted in this screenshot of the Jenkins bash terminal.](media/bash-jenkins-ssh-git.png "bash terminal")

25. The GitHub side of the integration with Jenkins is complete. Next, you will configure Jenkins as part of your CI/CD pipeline.

### Task 3: Open a connection to Jenkins

In this task, you create an SSH tunnel to the Jenkins server and configure it for use with the MERN application.

1. Return to your **Jenkins** VM blade in the Azure portal.

2. On the **Overview** blade of your Jenkins VM, locate the **DNS name**, and copy the value.

   ![DNS name and its value are highlighted on the Overview blade.](media/jenkins-vm-dns-name.png "Overview blade")

3. Open a new browser window or tab and paste the copied DNS name into the browser's address bar to navigate to your Jenkins server.

4. Select **Log in** on the Welcome to Jenkins screen.

   ![The Log in link is highlighted on the Welcome to Jenkins dialog.](media/jenkins-welcome-login.png "Welcome to Jenkins!")

5. On the Jenkins on Azure screen, you will see a message that this Jenkins instance does not support https, so logging in through a public IP address has been disabled. You will need to create an SSH tunnel to connect to the Jenkins instance securely.

6. To set up an SSH tunnel to Jenkins, copy the ssh command provided in the Jenkins on Azure window, as highlighted in the screenshot below.

   ![The ssh command that Jenkins provides is highlighted in the Jenkins on Azure window.](media/jenkins-on-azure.png "Jenkins On Azure window")

7. Open a new bash shell, and at the command prompt paste the copied ssh command, replacing "username" with **jenkinsadmin**. The command will resemble the following:

   ```bash
   ssh -L 127.0.0.1:8080:localhost:8080 jenkinsadmin@jenkins-kb.westus.cloudapp.azure.com
   ```

8. If prompted that authenticity of the Jenkins host cannot be established, enter "yes" to continue.

9. Enter the **jenkinsadmin** password, "Password.1!!"

   ![The ssh command above is highlighted in the bash window, and yes is highlighted next to Are you sure you want to continue connecting (yes/no)?](media/bash-jenkins-ssh-tunnel.png "bash window")

10. After you have started the SSH tunnel, open a new browser tab or window, and navigate to <http://localhost:8080/>.

11. As you did above, select **Log in** on the Welcome to Jenkins screen. This will launch the Getting Started dialog.

    ![After navigating to http://localhost:8080, the Getting Started page is displayed, providing the path to the initial administrator password, /var/lib/jenkins/secrets/initialAdminPassword.](media/jenkins-unlock.png "Unlock Jenkins window")

12. To get the initial password, return to the SSH tunnel bash window, and run the following command:

    ```bash
    sudo cat /var/lib/jenkins/secrets/initialAdminPassword
    ```

13. Copy the password returned and paste it into a text editor, such as Notepad.exe or Leafpad on the LabVM.

    ![The returned password is highlighted in the bash window.](media/bash-jenkins-initial-password.png "bash window")

14. Return to the Getting Started screen in your browser, paste the password into the **Administrator password** box, and select **Continue**.

    ![Screenshot of the Administrator password pasted into the box on the Getting Started screen, and Continue selected.](media/jenkins-admin-password.png "Administrator password field")

15. From the Jenkins dashboard, select **Manage Jenkins** from the left-hand menu and then select **Manage Plugins**.

    ![Manage Jenkins is highlighted in the left-hand menu of the Jenkins window, and Manage Plugins is highlighted on the right.](media/jenkins-dashboard-manage.png "Jenkins window")

16. With the **Available** tab selected, install the **NodeJS** plug-in by entering **nodejs** into the Filter box, and selecting the **NodeJS** plug-in in the results, and then selecting **Install without restart**.

    ![On the Manage Plugins screen, the Available tab is selected, "nodejs" is entered into the filter box, and NodeJS is checked in the filter results. The Install without restart button is highlighted.](media/jenkins-plugins-nodejs.png "Manage Plugins page")

17. With the **Available** tab still selected, enter **GitHub** into the Filter box.

    ![On the Manage Plugins screen, "GitHub" is entered into the filter box.](media/jenkins-plugins-github.png "Manage Plugins page")

18. Locate and select the **GitHub** plugin.

    ![The GitHub plugin is checked.](media/jenkins-plugins-github-checked.png "GitHub plugin")

19. Select **Install without restart**.

20. Select **Manage Jenkins** from the left-hand menu.

    ![Screenshot of the Jenkins left-hand menu with Manage Jenkins link highlighted.](media/jenkins-manage.png "Jenkins menu")

21. Select **Global Tool Configuration**.

    ![Screenshot of Manage Jenkins page, with Global Tool Configuration option highlighted.](media/jenkins-global-tool-configuration.png "Manage Jenkins page")

22. Find **NodeJS** and select **Add NodeJS** next to NodeJS installations.

23. Enter **bestforyounode** as the Name, ensure **Install automatically** is checked, and accept the default (latest) version of nodejs.

    ![In the NodeJS dialog box, bestforyounode is in the Name box, and Install automatically is selected below it.](media/jenkins-nodejs-install.png "NodeJS dialog box")

24. Select **Save**.

    ![This is a screenshot of the Save (selected) and Apply buttons.](media/jenkins-global-tool-configuration-save.png "Select Save")

### Task 4: Configure Continuous Integration with Jenkins

In this task, you create a simple Jenkins continuous integration (CI) pipeline, which builds the `MCW-OSS-PaaS-and-DevOps` application with every code commit into GitHub.

1. Return to the **Jenkins** dashboard, and select **New Item** from the left-hand menu.

2. Enter "best-for-you-build" as the name, select **Freestyle project**, and select **OK**.

   ![Best-for-you-build is entered in the Enter an item name box, and Freestyle project and OK are selected below it.](media/jenkins-best-for-you-build.png "Enter an item name section")

3. On the **General** tab of the project page:

   - Select **GitHub project**, and enter the URL for your forked copy of the `MCW-OSS-PaaS-and-DevOps` project page in your GitHub account.
   - Uncheck **Restrict where this project can be run**, if it is checked.

   ![On the General tab of the project page, GitHub project is selected and the URL for your forked copy of the best-for-you-organics project page in your GitHub account is entered.](media/jenkins-project-general.png "Project page General tab")

4. Next, scroll down to the **Source Code Management** section, select **Git**, and enter the URL to your project, including the ".git" extension.

   ![Git is selected on the Source Code Management tab of the project page, and the URL to your project, including the .git extension, is entered in the Repository URL box.](media/jenkins-project-source-code.png "Source Code Management section")

5. Scroll down to the **Build Triggers** section and select **GitHub hook trigger for GITScm polling**.

   ![The GitHub hook trigger for GITScm polling is selected and highlighted in the Build Triggers section.](media/jenkins-project-build-triggers.png "Build Triggers section")

6. Scroll down to the **Build Environment** section, and select **Provide Node & npm bin/ folder to PATH**, select your NodeJS Installation from the list, and leave the default values for npmrc file and Cache location.

   ![In the Jenkins Build Environment section, Provide Node & npm /bin folder to PATH is checked, the NodeJS Installation is specified, and the default values are set for npmrc file and Cache location.](media/jenkins-project-build-environment.png "Jenkins Build Environment")

7. In the **Build** section, select **Add build step**, and select **Execute shell** from the options.

   ![In the Jenkins Add build step menu, Execute shell is highlighted.](media/jenkins-project-add-build-step.png "Add Execute Shell Build step")

8. In the **Execute shell** Command block, enter:

   ```bash
   cd "Hands-on lab/lab-files"
   npm install
   npm run build
   ```

   ![In the Execute build section, npm install and npm run build are entered on separate lines in the command block.](media/jenkins-project-execute-shell-command.png "Jenkins Execute Build shell")

9. Finally, select **Save**.

   ![This is a screenshot of the Save (selected) and Apply buttons.](media/jenkins-save.png "Select Save")

10. Your Jenkins CI build job should now be triggered whenever a push is made to your repository.

### Task 5: Trigger CI build

In this task, you commit your pending changes in VS Code to your GitHub repo and trigger the Jenkins CI build job.

1. Return to VS Code on your Lab VM, and the open `MCW-OSS-PaaS-and-DevOps` project.

2. Observe that the **source control icon** on the left-hand navigation bar has a badge indicating you have uncommitted changes. Select the **icon**.

   ![The Source Control icon is highlighted on the Visual Studio Code Activity Bar.](media/vscode-source-control.png "Visual Studio Code Activity Bar")

3. In the **SOURCE CONTROL: GIT** pane enter a commit message, such as "Added Docker configuration," and select **+** next to **CHANGES** to stage all the pending changes.

   ![Added Docker configuration is highlighted in the SOURCE CONTROL: GIT pane, and the plus sign (+) next to CHANGES is highlighted on the right.](media/vscode-source-control-stage-changes.png "Visual Studio Code Activity Bar")

4. Select the **checkmark** to commit the changes.

   ![The checkmark next to commit the changes is highlighted in the SOURCE CONTROL: GIT pane.](media/vscode-source-control-commit.png "Visual Studio Code Activity Bar")

5. Next, select the **ellipsis** to the right of the checkmark, and select **Push** from the dropdown.

   ![The ellipsis (...) to the right of the checkmark is highlighted in the SOURCE CONTROL: GIT pane, and Push is highlighted in the submenu.](media/vscode-source-control-push.png "Visual Studio Code Activity Bar")

6. When prompted, select **Allow** to authorize Visual Studio Code to access Github.

7. Select **Continue** in the browser window that opens to authorize access to your GitHub account.

   ![The Continue button is highlighted on the Authorize Visual Studio Code to access GitHub dialog.](media/github-authorize-access-from-vs-code.png "Authorize Visual Studio Code to access GitHub")

8. On the Authorize GitHub for VSCode window, select **Authorize github**.

   ![The Authorize github button is highlighted on the Authorize GitHub for VSCode dialog.](media/github-authorize.png "Authorize GitHub for VSCode")

9. Ensure you see "Success!" in the following screen.

   ![A message that authorization was successful is displayed, with a Success header.](media/github-authorize-success.png "GitHub authorization was successful")

10. Select **Allow** in the dialog that appears and follow any additional prompts to complete the authorization within Visual Studio Code.

11. Return to your best-for-you-build job in Jenkins, and locate the **Build History** block on the left-hand side. Select **#1** to view the details of the build job, caused by your GitHub commit.

    ![Screenshot of Build History on the best-for-you-organics project page, with build job #1 highlighted.](media/jenkins-build-history-list.png "Build History section")

12. If the build is still running, you can select **Console Output** from the left-hand menu to view the ongoing build activities.

    ![The Console Output option is highlighted and selected in the left-hand menu, and the build output is displayed.](media/jenkins-build-active-console-output.png "Console output")

13. Once the build completes, you can see the changes you committed on the build page.

    ![The committed changes are displayed on the build page.](media/jenkins-build-history-details.png "Build page")

14. You have successfully set up your CI pipeline.

### Task 6: Install Docker on the Jenkins VM

In this task, you install Docker CE on your Jenkins VM, so it can be used to build images from the build artifacts produced by your CI build.

1. The first step is to ensure no older versions of Docker are installed on your Jenkins VM. Using the SSH tunnel bash terminal you opened previously, navigate to the command prompt, and enter:

   ```bash
   sudo apt-get remove docker docker-engine docker.io
   ```

2. Next, you need to set up a Docker repository on the host machine. The begin, update the `apt` package index:

   ```bash
   sudo apt-get update
   ```

3. Install the necessary packages to allow `apt` to use a repository over HTTPS, entering `y` when prompted to continue.

   ```bash
   sudo apt-get install \
   apt-transport-https \
   ca-certificates \
   curl \
   software-properties-common
   ```

4. Add Docker's official GPG key.

   ```bash
   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   ```

5. Verify that you now have the key with fingerprint `9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88`, by searching for the last eight characters of the fingerprint:

   ```bash
   sudo apt-key fingerprint 0EBFCD88
   ```

6. You should see output similar to:

   ```bash
   pub   4096R/0EBFCD88 2017-02-22
       Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
   uid                  Docker Release (CE deb) <docker@docker.com>
   sub   4096R/F273FCD8 2017-02-22
   ```

7. Next, enter the following commands to set up the **stable** repository.

   ```bash
   sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
   ```

8. You are now ready to install Docker CE. Start by updating the `apt` package index.

   ```bash
   sudo apt-get update
   ```

9. Install the latest version of Docker CE, entering `y` when prompted to continue.

   ```bash
   sudo apt-get install docker-ce
   ```

10. Verify that Docker CE is installed correctly by running the `hello-world` image.

    ```bash
    sudo docker run hello-world
    ```

    ![Hello world message from Docker on the Jenkins server.](media/bash-jenkins-docker-hello-world.png "Jenkins Docker install")

11. The final step is to add permission to the `jenkins` user to Docker.

    ```bash
    sudo usermod -a -G docker jenkins
    sudo chmod 664 /run/docker.sock
    ```

12. Now, restart the Jenkins service, entering the jenkinsadmin password, `Password.1!!`, when prompted.

    ```bash
    service jenkins restart
    ```

### Task 7: Add continuous delivery to Jenkins build job

In this task, you add continuous deployment (CD) to the Jenkins build pipeline. This uses a post-build action to create a new Docker image from the build, push that image to your Azure Container Registry, and deploy the image to your Web App for Containers instance.

1. In a new browser window, navigate to your Container registry in the [Azure portal](https://portal.azure.com) by selecting **Resource groups** from the Azure services list, selecting the **hands-on-lab-SUFFIX** resource group from the list, and then selecting the **Container registry** resource.

2. On the Container registry blade, select **Access keys** from the left-hand menu and leave this page open for the following steps.

3. Restarting the Jenkins service terminates any existing connections. Return to your **Jenkins** dashboard and select **Log in** on the toolbar.

4. On the login screen, enter the following:

   - **Username**: admin
   - **Password**: Enter the initial admin password you copied into a text editor previously.

   > As a reminder, the initial admin password was retrieved by executing the following command from the SSH tunnel bash window: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

5. Once logged back in, select the **best-for-you-build** project. When prompted to log in, use the initial admin password you retrieve in the bash shell previously and copied into a text editor.

   ![The best-for-you-build project is highlighted on the Jenkins dashboard.](media/jenkins-dashboard-best-for-you-build.png "Jenkins dashboard")

6. Select **Configure** from the left-hand menu.

   ![Configure is highlighted in the left-hand menu.](media/jenkins-project-configure.png "Jenkins left-hand menu")

7. On the configure screen, scroll down to the Build section and select the Execute shell block you created previously.

8. Return to the Container registry Access keys blade in the Azure portal, copy the values specified below and paste them into the appropriate values in the `docker build` command below.

   - **Login server**: Copy the Azure Container Registry's Login server value, and paste into the command below as the `ACR_URL` value.
   - **Username**: Copy the Username value for your Azure Container Registry, and paste into the command below as the `ACR_USERNAME` value.
   - **Password**: Enter the password value for your Azure Container Registry, and paste into the command below as the `ACR_PASSWORD` value.

     ![In the Container Registry Access keys blade, the Login Server, Username, and password fields are highlighted.](media/azure-container-registry-access-keys.png "Container Registry Access keys")

   ```bash
   ACR_URL="<your-container-registry-login-server>"
   ACR_USERNAME="<your-container-registry-username>"
   ACR_PASSWORD="<your-container-registry-password>"

   docker build --tag "${ACR_URL}/best-for-you-organics:latest" .
   docker login $ACR_URL --username $ACR_USERNAME --password $ACR_PASSWORD
   docker push "${ACR_URL}/best-for-you-organics:latest"
   ```

9. You final command should look similar to the following:

   ```bash
   ACR_URL="bestforyouregistrykb.azurecr.io"
   ACR_USERNAME="bestforyouregistrykb"
   ACR_PASSWORD="uddDqqTdBdxaI=QMmTqTfAg75cJHnfcj"

   docker build --tag "${ACR_URL}/best-for-you-organics:latest" .
   docker login $ACR_URL --username $ACR_USERNAME --password $ACR_PASSWORD
   docker push "${ACR_URL}/best-for-you-organics:latest"
   ```

10. Copy the completed command text and paste it below the `npm run build` line within the Execute shell Command box. The Execute shell command should now look similar to the following:

    ![The command above is appended to the execute shell command.](media/jenkins-execute-shell-command-cd.png "Execute shell")

11. Select **Save**.

   ![This is a screenshot of the Save (selected) and Apply buttons.](media/jenkins-save.png "Select Save")

### Task 8: Trigger CI-CD pipeline

In this task, you commit changes to the `MCW-OSS-PaaS-and-DevOps` starter application and trigger the full CI/CD pipeline through Jenkins, resulting in the updated application being added to a new Docker image, pushed to ACR, and deployed to Web App for Containers.

1. Return to VS Code on your Lab VM, open the `.dockerignore` file, and delete the line containing **DockerFile\*** from the file. This will allow Jenkins to use the file to build the container in the Jenkins CI/CD pipeline.

2. Next, open the `src/components/plan/Plans.js` file, and insert the following markup between `<div class="container">` and `<Grid>`:

   ```html
   <h3>Welcome to Best For You Organics Company</h3>
   ```

   ![The src/components/plan/Plans.js file is highlighted on the left side of Visual Studio Code, and the markup above is highlighted on the right.](media/vscode-plans-js.png "VS Code")

3. Save the updated files.

4. As you did in [Task 4](#task-4-trigger-ci-build), above, select the **Source Control icon** from the left-hand menu, enter a commit comment, select **+** to stage the change, and select the **checkmark** to commit the change, and push to GitHub. Enter your credentials if prompted. This will trigger the Jenkins CI/CD pipeline.

   ![The updated files are listed under Staged Changes in VS Code, and a commit comment is entered.](media/visual-studio-code-cd-file-updates-commit.png "Visual Studio Code")

5. Return to your Jenkins dashboard, and select the **best-for-you-build** project, and select the latest build number under Build History.

   ![The Jenkins build history is displayed, with a progress bar next to #2.](media/jenkins-build-history.png "Jenkins build history")

6. On the Build page, select **Console Output** from the left-hand menu.

   ![Console Output is highlighted in the left-hand menu on the Jenkins project build page.](media/jenkins-build-console-output.png "Build console output")

7. On the Console Output page, you can monitor the build progress. When it completes after a few minutes, you will see a SUCCESS message similar to the following:

   ![A build and Docker deployment success message is displayed in the console output in Jenkins.](media/jenkins-build-console-output-success.png "Build and Docker deployment success")

8. When the deployment is complete, you can verify the changes deployed successfully by navigating to your Web App for Containers instance in **hands-on-lab-web-SUFFIX** resource group in the Azure portal and selecting the URL on the overview blade. The deployment of the container can take several minutes to complete, so refreshes may take a few minutes to show the new header.

   > **Tip**: It may help to open the app in an Incognito or InPrivate browser window, as the old page may be cached.

9. You should see the home page, with a new header above the three plans on the page.

   ![Welcome to Best for You Organics Company! is highlighted above the Two-Person Plan, Four-Person Plan, and High-Pro Plan boxes in this screenshot of the home page.](media/bfyo-web-welcome.png "Home page")

## Exercise 6: Create Azure Functions for order processing

Duration: 45 minutes

In this task, you create the Azure Functions that handle order processing. The first function sends unprocessed order details into a storage queue. This function uses a timer trigger and checks the processed field on order documents, ensuring only unprocessed orders are sent to the processing queue. A second function simulates order processing and sends notifications to the user who placed the order.

### Task 1: Configure storage queues

In this task, you add two storage queues to the storage account provisioned when you created your Function App. These queues are used to store orders and notifications that need to be processed.

1. In the [Azure portal](https://portal.azure.com), select **Resource groups** from the Azure services list and then select the **hands-on-lab-func-SUFFIX** resource group.

   ![Resource groups is highlighted in the Azure services list.](media/azure-services-resource-groups.png "Azure services")

2. Select the **bestforyouordersSUFFIX** storage account that was created when you provisioned your Function App.

3. Select **Queues** from the **Services** area of the **Overview** blade.

   ![Queues is highlighted in the Services area of the Overview blade.](media/storage-account-queues.png "Overview blade Services area")

4. On the **Queue service** blade, select **+Queue** to add a new queue.

   ![+ Queue is highlighted on the Queue service blade.](media/storage-account-queue-add.png "Queue service blade")

5. In the **Add** queue dialog, enter **orderqueue** for the **Queue name**, and select **OK**.

   ![The name "orderqueue" is entered in the Queue name box in the Add queue dialog box.](media/storage-order-queue.png "Add queue dialog box")

6. Select **+Queue** again, and this time enter "notificationqueue" for the **Queue name**.

   ![The name "notificationqueue" is entered in the Queue name box in the Add queue dialog box.](media/storage-notification-queue.png "Add queue dialog box")

### Task 2: Create a timer triggered function

In this task, you create a function that function sends all new orders to a queue for processing and shipping. The function uses a Timer trigger and an output binding to an Azure Storage Queue.

1. In the [Azure portal](https://portal.azure.com), navigate to your Function App by selecting it from the list of resources in the **hands-on-lab-func-SUFFIX** resource group.

2. From the left-hand menu on your **Function Apps** blade, select **Functions**, then select **+ Add**.

   ![Functions is selected and highlighted in the left-hand menu on the Function Apps blade, and + Add is highlighted on the right.](media/function-app-new.png "Function Apps blade")

3. In the trigger search box, enter "timer" and select the **Timer trigger**.

   ![Timer is entered into the search box and the Timer trigger box is highlighted.](media/function-timer-trigger.png "Timer trigger")

4. In the **Timer trigger** dialog, enter the following:

   - **Name**: Enter **OrdersTimerTrigger**.
   - **Schedule**: Leave the default value, `0 */5 * * * *`. This will execute the trigger every 5 minutes.
   - Select **Create Function**.

   ![The information above is entered in the Timer trigger blade.](media/function-app-new-function.png "Timer trigger blade")

5. After the function is created, select **Integration** under Developer on the new function's left-hand menu.

   ![Integration is selected and highlighted under the new function.](media/function-app-integrate.png "New function menu")

6. Next, select **+ Add Output**.

   ![+ Add Output is highlighted under Outputs, Azure Queue Storage is selected below it, and Select is selected at the bottom.](media/function-app-new-output.png "Select Azure Queue Storage")

7. In the **Create Output** dialog, enter the following:

   - **Binding Type**: Select Azure Queue Storage.
   - **Message parameter name**: Enter `outputQueue`.
   - **Queue name**: Enter `orderqueue` (all lowercase, as casing matters)
   - **Storage account collection:** Select **AzureWebJobsStorage** from the list (this is the bestforyouordersSUFFIX storage account you created when you provisioned your Function App).
   - Select **OK**.

   ![The information above is entered in the Azure Queue Storage output dialog box, and Save is selected at the bottom.](media/function-app-storage-queue-output.png "Azure Queue Storage output page")

8. Now, return to the **bestforyouorders** Function App page in the Azure portal and in the left-hand menu, and then select **Advanced Tools** under the **Development Tools** section. On the Advanced Tools blade, select **Go**.

   ![Advanced Tools in selected and highlighted under Development tools on the bestforyouorders Function App blade. The Advanced tools blade is displayed with the Go link highlighted.](media/function-app-platform-features-kudu.png "Function App Advanced Tools")

9. In the new browser window that opens, select **Debug console -> PowerShell** from the top menu.

    ![The Kudu window is displayed with Debug console, PowerShell highlighted in the menu.](media/kudu-debug-console.png "Kudu")

10. In the Kudu file browser, navigate to `home\site\wwwroot`.

    ![The site folder is highlighted in the Kudu file browser.](media/kudu-file-browser-site.png "Site")

    ![The wwwroot folder is highlighted in the Kudu file browser.](media/kudu-file-browser-wwwroot.png "wwwroot")

11. In the `wwwroot` folder, select **+** and then select **New file** to add a new file.

    ![The new file (+) icon is highlighted, and New file is highlighted in the context menu.](media/kudu-file-browser-new-file.png "New file")

12. Enter `package.json` as the name of the file and press Enter.

    ![The name of the new file is entered as package.json.](media/kudu-file-browser-package-json.png "package.json")

13. After creating the new file, select the pencil (Edit) icon next to it.

    ![The pencil icon is highlighted next to package.json.](media/kudu-file-browser-edit-package-json.png "package.json")

14. Copy and paste the following JSON into the file editor and then select **Save**.

    ```json
    {
      "name": "best-for-you-organics",
      "dependencies": {
        "mongodb": "3.x"
      }
    }
    ```

    ![The JSON above is entered into the file editor screen, and the Save button is highlighted.](media/kudu-file-editor.png "File editor")

15. In the Kudu Remote Execution Console, run the following at the command prompt:

    ```powershell
    npm install
    ```

    ![At the Kudu PowerShell prompt, the command above is entered, and the output of the command is displayed.](media/kudu-powershell.png "PowerShell")

    > **Note**: The addition of the `mongodb` dependency is necessary because the Cosmos DB MongoDB API cannot be used to bind a Function App to Cosmos DB. Only the SQL API is currently compatible with Function App triggers.

16. Return to your Function App blade in the Azure portal, select **Functions** from the left-hand menu and then select the **OrdersTimerTrigger** function in the list of functions.

    ![Functions is selected in the left-hand menu and the OrdersTimerTrigger is highlighted in the functions list.](media/function-app-orders-timer-trigger.png "Functions list")

17. On the OrdersTimerTrigger blade, select **Code + Test** from the left-hand menu.

    ![Code + Test is highlighted under Developer on the Azure Function menu.](media/function-app-code-test.png "Function Code + Test")

18. To get the code for the `OrdersTimerTrigger` function, go into the project in VS Code, expand the `Hands-on lab/lab-files/AzureFunctions` folder, and open the `OrdersTimerTrigger.js` file.

19. Replace the `uri` variable value on line 4 of the `OrdersTriggerTimer.js` file with the primary connection string for your Cosmos DB, which you can retrieve from the Connection String blade of your Azure Cosmos DB account.

    ![The Azure Cosmos DB account connection string blade is displayed, and the primary connection string is highlighted.](media/cosmos-db-connection-string.png "Connection string")

20. Copy all of the code within the file.

21. Return to your `OrdersTimerTrigger` function in the Azure portal and paste the code into the `index.js` block, overwriting all the existing code. Your `index.js` file should now look like the following:

    ![This is a screenshot of the index.js block.](media/function-app-index-js.png "Index.js block")

22. Select **Save** on the Function toolbar.

    ![The Save button is highlighted on the Function toolbar.](media/function-app-save-code.png "Function App toolbar")

23. Next, select **Test/Run** on the toolbar.

    ![The Test/Run button is highlighted on the Function toolbar.](media/function-app-test-run.png "Function App toolbar")

24. Observe the function running in the Logs block.

    ![Logs is highlighted below the code block.](media/function-app-logs.png "Select Logs")

25. Return to the starter application in your browser window, and select **Sign In**.

    ![Two Person Plan, High-Pro Plan, and Four Person Plan boxes are visible in this screenshot of the starter application, and Sign In is highlighted at the top.](media/bfyo-web-sign-in.png "Sign in to the starter application")

26. On the Login screen, enter the following credentials, and select **Login**:

    - **Email address:** <demouser@bfyo.com>
    - **Password:** Password.1!!

    ![The credentials above are entered in the Login page.](media/bfyo-web-login-page.png "Login page")

27. After logging in, you will be returned to the home page. Choose **Select this plan** for any of the plans.

    ![Two Person Plan, High-Pro Plan, and Four Person Plan boxes are visible in this screenshot of the home page, and all three boxes' Select this plan buttons are highlighted.](media/bfyo-web-plans.png "Select a plan")

28. On the **Place Order** screen, select **Place Order**. This will create a new order in the Cosmos DB `orders` collection. Within 5 minutes, the Timer trigger of your function will fire and then send the order on to the `orderqueue` for processing.

    ![The Place Order button is highlighted at the bottom of the Place Order page.](media/bfyo-place-order.png "Place your order page")

29. Finally, verify items are being written to the order queue, by going to the queue in the Azure Storage account, and observing that items have been added to the `orderqueue`.

    ![The Refresh button is highlighted in the Azure Storage account, and Message Text appears in the order queue below.](media/storage-queue-items.png "Messages blade")

### Task 3: Create Queue function

In this task, you create a second function, which is triggered by the output of the OrdersTimerTrigger function. This simulates the order processing and adds items to the notificationqueue if the order processing is complete, and the `sendNotifications` property is true for the order.

This uses an Azure Storage Queue trigger, and an input dataset from Cosmos DB, pulling in customers. The output dataset is the Azure Cosmos DB orders table. The update sets `processed = true` and the `processedDate` to today's date.

1. In the Azure portal, navigate to the **bestforyouorders** Function app, select **Functions** from the left-hand menu and then select **+ Add**.

   ![Functions is selected and highlighted in the left-hand menu on the Function Apps blade, and + Add is highlighted on the right.](media/function-app-new.png "Function Apps blade")

2. On the New Function dialog, enter "queue" onto the search filter and select **Azure Queue Storage trigger** from the list.

   ![This is a screenshot of the Queue trigger box.](media/azure-queue-storage-trigger.png "Queue trigger box")

3. On the **Queue trigger New Function** dialog, enter the following:

   - **New Function:** Enter `ProcessOrders`.
   - **Queue name:** Enter `orderqueue`.
   - **Storage account connection:** Select **AzureWebJobsStorage**.
   - Select **Create Function**.

   ![The information above is entered in the Queue trigger New Function dialog box.](media/process-orders-function.png "Queue trigger New Function dialog box")

4. When the function has been created, select **Integration** on the ProcessOrders Function blade.

   ![Integration is selected and highlighted under the new function.](media/function-app-integrate.png "New function menu")

5. On the Integration blade, select **Azure Queue Storage (myQueueItem)** under **Trigger**.

   ![The Integration blade of the ProcessOrders function is displayed and the Azure Queue Storage (myQueueItem) trigger is highlighted.](media/function-integration-process-orders.png "Function Integration")

6. On the Edit Trigger dialog, change the Message parameter name to "orderToProcess" and select **Save**.

   ![The value of "orderToProcess" is highlighted in the Message parameter name box on the Edit Trigger dialog.](media/function-app-integrate-process-orders.png "Azure Queue Storage trigger section")

7. Now, select **+ Add output**

   ![+ Add output in highlighted under Outputs on the Integration blade of the ProcessOrders function.](media/function-app-process-orders-add-output.png "Integration Outputs")

8. On the Create Output dialog, enter the following:

   - **Binding Type**: Select **Azure Queue Storage**.
   - **Message parameter name**: Enter `outputQueue`.
   - **Queue name:** Enter `notificationqueue` (all lowercase, as casing matters).
   - **Storage account collection:** Select **AzureWebJobsStorage** from the list.
   - Select **OK**.

   ![The information above is entered in the Azure Queue Storage output dialog box.](media/function-app-azure-queue-storage-output.png "Azure Queue Storage output dialog box")

9. Now, select the **ProcessOrders** function on the Integration blade.

   ![The ProcessOrders function is selected and highlighted on the Integration blade.](media/function-app-process-orders.png "Integration Function")

10. To get the code for the **ProcessOrders** function, go into the project in VS Code, expand the `Hands-on lab/lab-files/AzureFunctions` folder, select `ProcessOrders.js`, and copy all the code in the file.

11. Return to the **ProcessOrders** Function in the Azure portal and paste the code into the `index.js` block, overwriting all the existing code, and select **Save**. Your `index.js` file should now look like the following:

    ![The code for the ProcessOrders function is pasted in the index.js block.](media/function-app-code-process-orders.png "Index.js block")

12. Next, select **Logs** below the code block, so you can observe the Function being called during the next steps.

    ![Logs is highlighted below the code block.](media/function-app-logs-bar.png "Select Logs")

13. To trigger the function, return to the starter application in your browser window. If you are still logged in, select **Logout**. Then, select **Sign In**, and on the **Sign In** screen, select **Register**.

    ![In this screenshot of the starter application, Sign In is highlighted at the top, and the Register button is highlighted below.](media/bfyo-web-register.png "Sign in to the starter application")

    > **Note**: You may need to select **Logout** if you are still logged in with the demouser account.

14. Complete the registration form. Be sure to include a valid email address so you can receive notifications of order processing in the next exercise. (If you opt not to enter a valid email address, you can still complete the next Exercise, but will not receive the email notifications that your order has been processed.)

    - You only need to enter data into the **First name**, **Last name**, and **email address** fields. All other fields have been pre-populated to save time.
    - The password has been set to `Password.1!!`. If you choose to enter a different password, note that when you log into the account.

15. After registering, you should be automatically logged into the site. If not, select **Sign In** from the Home page, enter the email address you provided during registration and the password (`Password.1!!`) on the login screen, and then select **Login**.

16. Select the **Select this plan** button for any plan on the home page, and on the Order screen, select **Place Order**.

    ![The Place Order button is highlighted on the Order page.](media/bfyo-place-order.png "Order page")

17. Return to your **ProcessOrders** function page in the Azure portal and observe the logs.

    ![A notification is highlighted in the logs on the ProcessOrders Function page in the Azure portal.](media/function-app-logs-notification-sent.png "ProcessOrders Function page")

    > **Note**: It can take up to five minutes for the OrdersTimerTrigger to fire. The ProcessOrders function will fire immediately after the OrderTimerTrigger function.

18. Your order was sent to the notificationqueue and is pending the notification being sent to your email address.

## Exercise 7: Create Logic App for sending email notifications

Duration: 30 minutes

In this exercise, you create Logic App, which triggers whenever an item is added to the `notificationqueue` Azure Storage Queue. The Logic App sends an email message to the email address included in the `notificationqueue` message.

### Task 1: Create a SendGrid account

In this task, you will create a SendGrid account through the Azure portal to send email notifications to customers, informing them that their order has been processed and is on its way.

1. In the [Azure portal](https://portal.azure.com/), select the **Show portal menu** icon and then select **+Create a resource** from the menu.

   ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. Enter "SendGrid" into the Search the Marketplace box.

   ![SendGrid is entered into the Search the Marketplace box.](media/azure-create-resource-send-grid.png "Create SendGrid Resource")

3. On the SendGrid blade, select **Create**.

   ![The Create button is highlighted on the SendGrid blade.](media/sendgrid-create.png "Create SendGrid")

4. On the Create a new SendGrid Account **Basics** tab, enter the following:

   **Project details**:

   - **Subscription** Select the subscription you are using for this hands-on lab.
   - **Resource group**: Choose **Use existing** and select the **hands-on-lab-SUFFIX** resource group.
   - **Location**: Select the location you are using for resources in this hands-on lab.

   **Account details**:

   - **Name**: **bfyoemail**
   - **Password**: **Password.1!!**
   - **Pricing tier**: Select the **Free** plan.

   **Contact details**:

   - Enter your information into the required fields (First Name, Last Name, Email, Company, and Website)

     > **Note**: Website is not listed as a required field, but currently, the deployment fails if this is not populated.

   ![The SendGrid configuration values specified above are entered into the create a new SendGrid account blade.](media/sendgrid-create-settings.png "Create SendGrid account")

5. Select **Review + create** and then select **Create** to provision the SendGrid account.

6. When the SendGrid account finishes provisioning, select **Go to resource** from the notifications pane in the Azure portal.

   ![The Go to resource button is highlighted in the SendGrid deployment notification.](media/go-to-resource-sendgrid.png "Go to resource")

7. On the SendGrid account blade, select **Manage** from the toolbar.

   ![The Manage button is highlighted on the SendGrid account toolbar.](media/sendgrid-manage.png "SendGrid account Manage")

8. If prompted, select **Send Confirmation Email** and follow the prompts in the email you receive to confirm your account.

9. On the SendGrid page that opens, expand **Settings** in the left-hand menu, select **API Keys**, and then select **Create API Key**.

   ![The Create API Key button is highlighted on the API Keys page.](media/sendgrid-create-api-key.png "SendGrid API Keys")

10. On the Create API Key page, enter the following:

    - **API Key Name**: Enter `bfyo-api-key`.
    - **API Key Permissions**: Select **Full Access**.
    - Select **Create & View**.

    ![The values specified above are entered into the Create API Key page.](media/sendgrid-create-api-key-page.png "Create API Key")

11. Leave the API Key Created screen that appears open. You will be copying the key and pasting it into your Logic App in the next task.

    ![The API Key Created screen is displayed.](media/sendgrid-api-key-created.png "API Key Created")

### Task 2: Create a Logic App

In this task, you create a new Logic App, which uses the SendGrid connector to send email notifications to users, informing them that their order has processed and shipped.

1. In the Azure portal, navigate to the **OrderNotifications** Logic App in the **hands-on-lab-SUFFIX** resource group.

2. In the Logic App Designer, select **Blank Logic App** under **Templates**.

   ![Blank Logic App is highlighted under Templates in Logic App Designer Templates section.](media/logic-app-templates-blank.png "Logic App Designer, Templates section")

3. In the blank logic app template, select **All** and then select **Azure Queues** under **Connectors**.

   ![Azure Queues is highlighted under Connectors.](media/logic-app-connectors-azure-queues.png "Connectors section")

4. Select **When there are messages in a queue** under Triggers.

   ![When there are messages in a queue is highlighted under Triggers.](media/logic-app-triggers-when-there-are-messages-in-a-queue.png "Triggers section")

5. On the When there are messages in a queue dialog, enter **bestforyouorders** for the, **Connection Name** select the bestforyouorders **Storage account** from the list and select **Create**.

   ![In the When there are messages in a queue dialog box, Bestforyouorders is in the Connection Name box, the bestforyouorders row is highlighted at the top of the list below Storage account, and the Create button is highlighted at the bottom.](media/logic-app-trigger-queues-configure.png "When there are messages in a queue dialog box")

6. In the next **When there are messages in a queue** dialog, select **notificationqueue** from the **Queue Name** list, and set the interval to **1 minute**.

   ![The information above is entered in the When there are messages in a queue dialog box.](media/logic-app-trigger-message-in-queue-configure.png "When there are messages in a queue dialog box")

7. Select **+ New step**.

   ![The New step button is displayed.](media/logic-app-new-step.png "Logic App new step")

8. In the **Choose an action box**, enter "parse," and select **Data Operations**.

   ![In the When there are messages in a queue dialog box, Parse is in the Choose an action box and Data Operations below in the list.](media/data-operations-parse.png "When there are messages in a queue dialog box")

9. Under Data Operations, select **Parse JSON**.

   ![Parse JSON is highlighted under Data Operations.](media/logic-app-data-operations-parse-json.png "Data Operations")

10. In the Parse JSON box, select the **Content** box, select **Add dynamic content +**, then select **Message Text** from the input parameters list that appears.

    ![In the Parse JSON window, Message Text is in the Content box, Add dynamic content is highlighted, and Message Text is highlighted below in the input parameters list.](media/logic-app-parse-json-content.png "Parse JSON window")

11. Next, select **Use sample payload to generate schema** below the **Schema** box.

    ![In the Parse JSON window, Use sample payload to generate schema is highlighted below the Schema box.](media/logic-app-parse-json-schema.png "Parse JSON window")

12. In the dialog that appears, paste the following JSON into the sample JSON payload box, then select **Done**.

    ```json
    {
      "orderId": "5a6748c5d0d3199cfa076ed3",
      "userId": "demouser@bfyo.com",
      "notificationEmail": "demouser@bfyo.com",
      "firstName": "Demo"
    }
    ```

    ![The JSON above is pasted in the sample JSON payload dialog box, and Done is selected below.](media/logic-app-parse-json-sample-payload.png "Paste the JSON in the dialog box")

13. You will now see the schema for messages coming from the notification queue in the Schema box.

    ![The completed Parse JSON box is displayed.](media/logic-app-parse-json-complete.png "Parse JSON")

14. Select **+ New** **step**.

    ![The + New step button is displayed](media/logic-app-new-step.png "Logic App new step")

15. In the **Choose an action box**, enter "sendgrid," and select **SendGrid** under Connectors.

    ![SendGrid is entered into the search box, and the SendGrid connection is highlighted under connectors.](media/logic-app-connectors-sendgrid.png "Choose a connector")

16. In the **SendGrid** connector dialog, select **Send email (V4)**.

    ![Send email (v4) is highlighted in the list of SendGrid actions.](media/logic-app-sendgrid-send-email.png "SendGrid")

17. In the **SendGrid** box, enter the following:

    - **Connection Name**: Enter `bfyo-sendgrid`.
    - **SendGrid Api Key**: Return to the **API Key Created** screen in your SendGrid account, and then copy and paste the API key you generated.
    - Select **Create**.

    ![The SendGrid connection configuration information above is entered into the SendGrid box.](media/logic-app-sendgrid-create.png "SendGrid")

18. In the **Send email (V4)** box, enter the following:

    - **From**: Enter your email address.
    - **To**: Click in the box, select **Add dynamic content**, and then select the **notificationEmail** property. **NOTE**: If under the Parse JSON Dynamic Content section, you see a message that there were not any outputs to match the input format, select **See more** in the message.

    ![See more is highlighted under Parse JSON in the dynamic content dialog.](media/logic-app-dynamic-content-see-more.png "Dynamic content")

    - **Subject**: Enter "Order: " and then select **orderId** from the dynamic content dialog.
    - **Email body**: Select **firstName** from the dynamic content dialog, and then enter ", Your Best For You Organics Company order has shipped."

    ![The Send email (V3) dialog is completed with the values specified above.](media/logic-app-send-email-v2-complete.png "Send email (V3)"))

19. Select **+ New step**.

    ![The Add an action button is highlighted under + New step.](media/logic-app-new-step.png "Add an action button")

20. In the **Choose an action** dialog, enter "queues" into the search box, and select **Delete message** under Actions.

    ![Queue is highlighted in the Choose an action search box, and Azure Queues -- Delete message is highlighted below.](media/logic-app-azure-queues-delete-message.png "Choose an action dialog box")

21. Select **notificationqueue** for the Queue Name.

22. For Message ID, select the **Message ID** parameter from the dynamic content parameter list.

23. For Pop Receipt, select the **Pop Receipt** parameter from the dynamic content parameter list.

    ![The settings above are entered into the Delete Message box.](media/logic-app-azure-queue-delete-message-settings.png "Delete message")

24. Select **Save** on the **Logic Apps Designer** toolbar.

    ![Save is highlighted on the Logic Apps Designer blade toolbar.](media/logic-app-save.png "Logic Apps Designer blade")

25. The Logic App will begin running immediately, so if you entered your valid email address when you registered your account in the Best for You Organics starter app, and placed an order, you should receive an email message within a minute or two of selecting Save.

    ![The email message from the LogicApp is displayed.](media/order-email-message.png "Email message")

## After the hands-on lab

Duration: 10 minutes

In this exercise, you will de-provision all Azure resources that you created in support of this hands-on lab.

### Task 1: Delete Azure resource groups

1. In the Azure portal, select **Resource groups** from the left-hand menu and locate and delete the following resource groups.

   - hands-on-lab-SUFFIX
   - hands-on-lab-func-SUFFIX
   - hands-on-lab-jenkins-SUFFIX
   - hands-on-lab-web-SUFFIX

### Task 2: Delete WebHooks and Service Integrations

1. In your GitHub account:

   - Delete the Jenkins service integration.
   - Delete the Personal Access Token you created for integration with VS Code.

You should follow all steps provided _after_ attending the Hands-on lab.
