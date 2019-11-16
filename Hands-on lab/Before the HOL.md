![Microsoft Cloud Workshops](https://github.com/Microsoft/MCW-Template-Cloud-Workshop/raw/master/Media/ms-cloud-workshop.png "Microsoft Cloud Workshops")

<div class="MCWHeader1">
OSS PaaS and DevOps
</div>

<div class="MCWHeader2">
Before the hands-on lab setup guide
</div>

<div class="MCWHeader3">
November 2019
</div>

Information in this document, including URL and other Internet Web site references, is subject to change without notice. Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places, and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, e-mail address, logo, person, place or event is intended or should be inferred. Complying with all applicable copyright laws is the responsibility of the user. Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.

Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document. Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.

The names of manufacturers, products, or URLs are provided for informational purposes only and Microsoft makes no representations and warranties, either expressed, implied, or statutory, regarding these manufacturers or the use of the products with any Microsoft technologies. The inclusion of a manufacturer or product does not imply endorsement of Microsoft of the manufacturer or product. Links may be provided to third party sites. Such sites are not under the control of Microsoft and Microsoft is not responsible for the contents of any linked site or any link contained in a linked site, or any changes or updates to such sites. Microsoft is not responsible for webcasting or any other form of transmission received from any linked site. Microsoft is providing these links to you only as a convenience, and the inclusion of any link does not imply endorsement of Microsoft of the site or the products contained therein.

© 2019 Microsoft Corporation. All rights reserved.

Microsoft and the trademarks listed at <https://www.microsoft.com/en-us/legal/intellectualproperty/Trademarks/Usage/General.aspx> are trademarks of the Microsoft group of companies. All other trademarks are property of their respective owners.

**Contents**

<!-- TOC -->

- [OSS PaaS and DevOps before the hands-on lab setup guide](#oss-paas-and-devops-before-the-hands-on-lab-setup-guide)
  - [Requirements](#requirements)
  - [Before the hands-on lab](#before-the-hands-on-lab)
    - [Task 1: Provision a resource group](#task-1-provision-a-resource-group)
    - [Task 2: Create a development virtual machine](#task-2-create-a-development-virtual-machine)
    - [Task 3: Provision a Jenkins server](#task-3-provision-a-jenkins-server)
    - [Task 4: Create a GitHub account](#task-4-create-a-github-account)
    - [Task 5: Fork the starter app](#task-5-fork-the-starter-app)

<!-- /TOC -->

# OSS PaaS and DevOps before the hands-on lab setup guide

## Requirements

1. Microsoft Azure subscription must be pay-as-you-go or MSDN.
   - Trial subscriptions will *not* work.
2. Linux virtual machine configured with:
   - Visual Studio Code
   - Azure CLI
   - Docker
   - Node.js and npm
   - MongoDB Community Edition

## Before the hands-on lab

Duration: 30 minutes

In this exercise, you set up the Azure environment for use throughout the rest of the hands-on lab. You should follow all steps provided *before* attending the Hands-on lab.

> **IMPORTANT**: Many Azure resources require unique names. Throughout these steps, you will see the word "SUFFIX" as part of resource names. You should replace this with your Microsoft alias, initials, or another value to ensure the resource is uniquely named.

### Task 1: Provision a resource group

In this task, you create an Azure resource group for the resources used throughout this lab.

1. In the [Azure portal](https://portal.azure.com), select **Resource groups** from the Azure services list.

    ![Resource groups is highlighted in the Azure services list.](media/azure-services-resource-groups.png "Azure services")

2. On the Resource groups blade, select **+Add**.

    ![+Add is highlighted in the toolbar on Resource groups blade.t](media/resource-groups-add.png "Resource groups")

3. On the Create a resource group **Basics** tab, enter the following:

    - **Subscription**: Select the subscription you are using for this hands-on lab.
    - **Resource group**: Enter **hands-on-lab-SUFFIX** as the name of the new resource group.
    - **Region**: Select the region you are using for this hands-on lab.

    ![The values specified above are entered into the Create a resource group Basics tab.](media/create-resource-group.png "Create resource group")

4. Select **Review + Create**.

5. On the **Review + create** tab, ensure the Validation passed message is displayed and then select **Create**.

### Task 2: Create a development virtual machine

In this task, you provision a Linux virtual machine (VM) running Ubuntu Server 16.04 LTS, which is used as your development machine throughout this lab. The VM is created using an Azure Resource Manager (ARM) template from a GitHub repository. The ARM template includes a custom extension script that installs Docker, Visual Studio Code (VS Code), MongoDB, and other required software on the VM. The ARM template also adds an inbound port rule that opens port 3389 on the network security group for the VM to allow RDP connections.

> **Note**: If you would like to review the steps to provision the VM manually and installed software, see [Appendix A](./Appendix-A).

1. To open a custom deployment screen in the Azure portal, select the Deploy to Azure button below:

    <a href ="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2FMCW-OSS-PaaS-and-DevOps%2Fmaster%2FHands-on%20lab%2Flab-files%2FLabVM%2Fazure-deploy.json" target="_blank" title="Deploy to Azure">
    <img src="http://azuredeploy.net/deploybutton.png"/>
    </a>

2. On the custom deployment screen in the Azure portal, enter the following:

    - **Subscription**: Select the subscription you are using for this hands-on lab.
    - **Resource group**: Select the hands-on-lab-SUFFIX resource group from the dropdown list.
    - **Location**: Select the location you used for the hands-on-lab-SUFFIX resource group.
    - **Virtual Machine Name**: Accept the default value, LabVM.
    - **Admin Username**: Accept the default value, demouser.
    - **Admin Password**: Accept the default value, Password.1!!
    - Check the box to agree to the Azure Marketplace terms and conditions.

    ![The Custom deployment blade is display, and the information above is entered into it.](media/azure-custom-deployment.png "Custom deployment blade")

3. Select **Purchase**.

> It can take about 20 minutes to deploy the Lab VM. You can move on to the next task while the VM is deploying.

### Task 3: Provision a Jenkins server

In this task, you provision an Azure Linux VM, which will serve as your Jenkins server for this hands-on lab.

1. In the [Azure portal](https://portal.azure.com/), select the **Show portal menu** icon and then select **+Create a resource** from the menu.

    ![The Show portal menu icon is highlighted, and the portal menu is displayed. Create a resource is highlighted in the portal menu.](media/create-a-resource.png "Create a resource")

2. Enter "jenkins" into the **Search the Marketplace** box, select the **Jenkins** compute item from the results, and then select **Create**.

    ![+ Create a resource is highlighted in the navigation pane of the Azure portal, and Everything is highlighted to the right. On the Everything blade to the right, jenkins is highlighted in the search box, and the Jenkins row is highlighted in the search results.](media/create-resource-jenkins.png "Azure Portal")

3. On the **Create Jenkins Basics** tab, enter the following:

    - **Subscription:** Select the subscription you are using for this hands-on lab.
    - **Resource group:** Select the hands-on-lab-SUFFIX resource group from the list.
    - **Region:** Select the region you are using for resources in this hands-on lab.
    - **Name:** Enter **Jenkins**.
    - **User name:** Enter **jenkinsadmin**.
    - **Authentication type:** Select **Password**
    - **Password:** Enter **Password.1!!**

    ![The Create Jenkins Basic tab is displayed, and the information above is entered into the form.](media/create-jenkins-basics.png "Configure settings on the Create Jenkins Basics blade")

4. Select **Next: Additional settings**, and on the **Additional Settings** tab set the following configuration:

    - **Size**: Accept the default VM size of **Standard DS2 v2**.
    - **VM disk type**: Select **SSD**.
    - **Virtual network**: Accept the default value of **(new) jenkins-vnet**.
    - **Subnet**: Accept the default value of **(new) jenkins (10.x.0.0/24)**.
    - **Public IP address**: Accept the default value of **(new) jenkins-pip**.
    - **Domain name label**: Enter a globally unique value, such as **jenkins-SUFFIX**.
    - **Jenkins release type**: Select **LTS**.
    - **JDK Type**: Select **Zulu**.

    ![The Additional Settings tab is displayed, and the values specified above are entered into the appropriate fields in the form.](media/create-jenkins-additional-settings.png "Jenkins VM Additional settings")

5. Select **Next: Integration settings**.

6. On the **Integration Settings** blade accept the default settings and select **Review + create**.

    ![The Integration Settings tab is selected on the Create Jenkins blade, showing the default settings.](media/create-jenkins-integration-settings.png "Jenkins VM Integration settings")

7. On the **Review + create** blade, ensure the `Validation passed` message is displayed, and then select **Create**.

    ![Validation information is displayed on the Review + create tab, with the Validation passed message displayed.](media/create-jenkins-summary.png "Validation passed")

8. It can take 10+ minutes for the VM to provision. You can move on to the next task while you wait.

### Task 4: Create a GitHub account

In this task, you sign up for a free GitHub account, which is used for hosting a copy of the sample application used throughout this lab. This account will be integrated into the CI/CD workflow for pushing updates to the application into Azure.

> **Note**: If you already have a GitHub account, and wish to use that account, you can skip to the [next task](#task-5-fork-the-starter-app).

1. Navigate to <https://github.com> in a web browser.

2. In the form on the page, enter a **username**, your **email** address, and a **password**, then select **Sign up for GitHub**.

    ![This is a screenshot of the sign-up form on github.com.](media/github-signup.png "Sign up for GitHub")

3. On the Welcome to GitHub screen, select **Unlimited public repositories free** under **Choose your personal plan** and select **Continue**.

    ![Unlimited public repositories free is selected under Choose your personal plan on the Welcome to GitHub screen.](media/github-welcome.png "Select Unlimited public repositories free")

4. On the next screen, you can select options to tailor your experience and select **Submit**, or select **skip this step**, next to **Submit,** to complete your registration.

    ![Very experienced, Development, and I'm a professional are selected in Step 2 on the Welcome to GitHub screen.](media/github-experience.png "Select options in Step 2")

### Task 5: Fork the starter app

In this task, you will fork the [OSS PaaS and DevOps MCW GitHub repository](https://github.com/Microsoft/MCW-OSS-PaaS-and-DevOps) to create a copy of the starter application in your GitHub account.

> As part of this workshop, you will be linking a Jenkins Continuous Integration/Continuous Delivery pipeline to your GitHub repo and committing code changes into that forked repo. To do this, you must create a copy of the starter application in a repo where you have permission to commit changes.

1. Log into your GitHub account, and then navigate to the [OSS PaaS and DevOps MCW GitHub repository](https://github.com/Microsoft/MCW-OSS-PaaS-and-DevOps).

2. At the top right of the project's GitHub page, select **Fork**.

    ![Fork is highlighted at the top right of the projects GitHub page.](media/github-fork.png "Select Fork")

3. If you have more than one GitHub account, select the account you are using for this hands-on lab.

4. This will start the process of making a copy of the repo and starter application into your GitHub account, in a repository named **MCW-OSS-PaaS-and-DevOps**.

5. Once completed, the project page will open.

    ![The Code tab is highlighted on this screenshot of the project page. At this time, we are unable to capture all of the information in the window. Future versions of this course should address this.](media/github-repo.png "Screenshot of the project page")

You should follow all steps provided *before* attending the Hands-on lab.
