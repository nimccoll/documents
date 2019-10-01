# Linux Development VM Setup Instructions

## Prerequisites
1. You have created a Linux Virtual Machine in Azure running Ubuntu 18.04 LTS
1. You have opened ports 22 and 3389 to support SSH and RDP
1. You have created a DNS name for the Public IP address of the virtual machine to allow easy connectivity from Putty and Remote Desktop Connection Manager
1. You have downloaded and installed Putty to your local workstation from https://putty.org/
1. You have downloaded and installed Remote Desktop Connection Manager from https://www.microsoft.com/en-us/download/details.aspx?id=44989

## Setup Remote Desktop Connectivity
1. Start Putty and open an SSH session to the DNS name of your Linux virtual machine
1. Once you have connected enter the following command to update the root password. Make sure to set it to a secure password that you can remember. This is simply a precaution in case something happens to the account you setup in Azure when you created the virtual machine
 
     `sudo passwd root`

1. Next make sure the account you setup in Azure when you created the virtual machine is part of the admin group by entering the following command:
 
    `sudo usermod -G admin {username}` 

1. Now we are ready to install XRDP and the KDE desktop environment. I am using KDE because the look and feel is similar to Windows and it behaves well with XRDP. Feel free to choose another desktop environment if you prefer. Detailed instructions for setting up XRDP with other popular Linux desktops can be found here https://www.youtube.com/watch?v=sDZ6zmuYsho
 
    `sudo apt-get update`  
    `sudo apt-get install kde-standard`  
    `sudo apt-get install xrdp`  
    `echo "startkde" > ~/.xsession`

1. The last command updates the XRDP configuration to start the KDE destop environment when an RDP session is established. At this point restart the virtual machine via the Azure portal to apply all of the configuration changes we have made.

## Configure the Remote Desktop Connection Manager
1. Once the virtual machine has been restarted we could simply use the Connect option in the Azure portal to start an RDP session. However, you may notice some display anomolies if you are running on a high resolution device such as a Surface Book. To correct these issues we can configure an entry in the Remote Desktop Connection Manager for the virtual machine in order to adjust the display settings.
1. Start Remote Desktop Connection Manager and select the Add Server... option from the Edit menu
1. On the Server Settings tab enter the fully-qualified DNS name of the virtual machine in the Server name text box. Adjust the display name to your liking.
1. Click the Login Credentials tab and uncheck the Inherit from parent checkbox. Enter the user name of the account you setup when you created the virtual machine and optionally the password if you don't want to be prompted when you connect. Make sure to clear the Domain text box as this is not a Windows machine.
1. Click the Remote Desktop Settings tab and uncheck the Inherit from parent checkbox. Select the Full Screen radio button.
1. Click the Display Settings tab and uncheck the Inherit from parent checkbox. Check the Scale docked remote desktop to fit window checkbox. Check the Scale undocked remote desktop to fit window checkbox.
1. Click the Add button to add the server to your Remote Desktop Connection Manager environment.
1. Start an RDP session with your virtual machine by double-clicking the server's icon in the Remote Desktop Connection Manager or by clicking the server's icon and selecting the Connect option from the Session menu

## Setup Development Tools
 1. Once you have established an RDP session, login to your virtual machine and open up a terminal window to install your development tools.
 1. Install a browser of your choice. I will be installing Firefox.
 
    `sudo apt-get install firefox`  
1. Use your browser to download the Visual Studio Code .deb package from https://go.microsoft.com/fwlink/?LinkID=760868
1. In your terminal window navigate to your Downloads folder and install Visual Studio Code from the .deb package using the following command:
 
    `sudo apt install ./{visual studio code package name}.deb`   

1. At this point you can install whichever development frameworks and tools you would like. I will be installing the following.  
    - .Net Core
    - PowerShell Core for Linux and the Azure PowerShell module
    - Azure CLI
    - Docker

1. Install .Net Core by issuing the following commands:

    `wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb`  
    `sudo dpkg -i packages-microsoft-prod.deb`  
    `sudo add-apt-repository universe`  
    `sudo apt-get update`  
    `sudo apt-get install apt-transport-https`  
    `sudo apt-get install dotnet-sdk-3.0`  

    Issue the following command to verify that your .Net Core installation was succesful:

    `dotnet --version`

1. Install PowerShell Core for Linux by issuing the following commands:

    `wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb`  
    `sudo dpkg -i packages-microsoft-prod.deb`  
    `sudo apt-get update`  
    `sudo add-apt-repository universe`  
    `sudo apt-get install -y powershell`  
     
    Issue the following command to verify that your PowerShell Core installation was successful:  

   `pwsh`

1. Install the Azure PowerShell module by starting PowerShell Core in administrator mode and issuing the following commands:

    `sudo pwsh`  
    `Install-Module -Name Az -AllowClobber -Scope AllUsers`  

1. Install the Azure CLI by issuing the following command:

    `curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash`  

    Issue the following command to verify that the Azure CLI has been successfully installed:

    `az --version`   

1. Next we will install Docker by issuing the following command:

    `curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh`  

    If you run into any issues with this script, you can manually install Docker by issuing the following commands:

    `sudo apt update`  
    `sudo apt install apt-transport-https ca-certificates curl software-properties-common`  
    `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`  
    `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"`  
    `sudo apt update`
    `sudo apt install docker-ce`  
    `sudo gpasswd -a {username} docker`

    Logout and log back into your virtual machine following the installation of Docker to ensure your permissions have been updated. Issue the following command to verify that Docker has been installed successfully:

    `docker --version`  

1. Start Visual Studio Code by issuing the following command:

    `code .`  

1. Once Visual Studio Code has opened install the following extensions to complete your installation:  
     - C#
     - PowerShell
     - Docker
     - Azure IoT Tools  

    Install any additional extensions you feel may be useful to you during development

This completes the setup of you Linux development VM