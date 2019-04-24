# Appendix A: Lab VM setup

Appendix A provides steps for manually provisioning and setting up the Lab VM used as a development machine for this lab.

## Task 1: Create VM config script

In this task, you will create a script file that will be used as a custom extension for configuring your Linux virtual machine in Azure. This script contains commands to install all the required software and configure a desktop on the Linux VM. These commands could also be run from an SSH shell manually.

1. Open a web browser, and navigate to <https://raw.githubusercontent.com/Microsoft/MCW-OSS-PaaS-and-DevOps/master/Hands-on%20lab/lab-files/LabVM/labvmconfig.sh>.

2. Copy the contents displayed in the browser into a text editor, such as Visual Studio Code or Notepad, and save the file as `labvmconfig.sh`.

    > Note the location where you save the file, as you will be referencing it in the next task.

## Task 2: Create a Linux virtual machine

In this task, you will provision a Linux virtual machine (VM) running Ubuntu Server 16.04 LTS.

1. In the [Azure Portal](https://portal.azure.com/), select **+Create a resource**, then enter "ubuntu" into the search bar, expand **Ubuntu Server** and select **Ubuntu Server 16.04 LTS** from the results.

    ![+ Create a resource is highlighted in the navigation pane of the Azure portal, ubuntu is highlighted in the search box, and the Ubuntu Server 16.04 LTS row is highlighted in the search results.](media/create-resource-ubuntu-server-16.04-lts.png "Azure Portal")

2. On the **Ubuntu Server 16.04 LTS** blade, select **Create**.

    ![On the Windows 10 Pro, Version 1709 blade, Create is highlighted below Select a deployment model: Resource Manager.](media/ubuntu-server-create.png "Select a deployment model field")

3. Set the following configuration on the **Basics** tab:

    **PROJECT DETAILS**:

    - **Subscription:** Select the subscription you are using for this hands-on lab.
    - **Resource Group:** Select **Create new**, and enter "hands-on-lab-(SUFFIX)" as the name of the new resource group.

    **INSTANCE DETAILS**:

    - **Virtual machine name:** Enter **LabVM**.
    - **Region:** Select the region you are using for this hands-on lab.
    - **Availability options**: Select **No infrastructure redundancy required**.
    - **Image**: Select **Ubuntu Server 16.04 LTS**.
    - **Size**: Select **Standard D2s v3** or **Standard E2s_v3**.

    **ADMINISTRATOR ACCOUNT**:

    - **Authentication type:** Select **Password**.
    - **Username:** Enter **demouser**
    - **Password:** Enter **Password.1!!**
    - **Login with Azure Active Directory**: Select **Off**.

    **INBOUND PORT RULES**:

     - **Public inbound ports**: Select **Allow selected ports**.
     - **Select inbound ports**: Expand the list and check the box for **RDP (3389)**.

    ![Basics is selected on the Create virtual machine blade, and the information above is entered on the Basics blade at right.](media/create-ubuntu-server-basics.png "Create a virtual machine")

4. Select **Next : Disks** to move to the Disks tab.

5. On the **Disks** tab, ensure **Premium SSD** is selected for the **OS disk type**.

    ![Premium SSD is selected for the OS disk type on the Disks tab.](media/create-ubuntu-server-disks.png "Create a virtual machine")

6. Select **Next : Networking**.

7. On the **Networking** tab, accept all the defaults and select **Next : Management**.

8. On the **Management** tab, accept all the defaults and select **Next : Advanced**.

9. On the **Advanced** tab, click on the **Select an extension to install** link.

    ![Select an extension to install is highlighted on the Advanced tab.](media/create-ubuntu-server-advanced.png "Create a virtual machine")

10. On the **New resource** blade that appears, select **Custom Script for Linux**, and then select **Create** on the Custom Script for Linux blade.

    ![On the New resource blade, Custom Script for Linux is highlighted and selected. The Create button is highlighted on the Custom Script for Linux blade.](media/create-ubuntu-server-new-resource-extension.png "Create a virtual machine")

11. On the **Install extension** blade:

    - **Script files**: Select the `labvmconfig.sh` file you saved in the previous task.

    - **Command**: Enter `bash labvmconfig.sh`

    - Select **OK**.

    ![The Install extension blade is displayed and the information above are entered into the fields.](media/create-ubuntu-server-install-extension.png "Install extension blade")

12. Select **OK** on the Install extension blade.

13. Select **Review + create** on the Advanced tab.

14. Select **Create** on the **Review + create** tab to provision the virtual machine.

    ![Summary is selected on the Create virtual machine blade, and Create is selected on the Create blade at right.](media/create-ubuntu-server-review-create.png "Create a virtual machine")

15. It may take 10+ minutes for the virtual machine to finish provisioning.