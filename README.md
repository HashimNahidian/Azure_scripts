# Azure_scripts: Create an Azure virtual machine

Task 1: Create a resource group

    Log into the Azure portal.
    Select the Azure Cloud Shell icon to bring up Cloud Shell.
    From the Azure CLI, create a resource group named IntroAzureRG.
    Azure CLI 

    az group create --name IntroAzureRG --location eastus

Task 2: Create a Linux virtual machine

    Use the following Azure CLI command to create a Linux VM.

    From Cloud Shell, run the following az vm create command to create a Linux VM:
    Azure CLI 

    az vm create \
      --resource-group "IntroAzureRG" \
      --name my-vm \
      --size Standard_D2s_v5 \
      --public-ip-sku Standard \
      --image Ubuntu2204 \
      --admin-username azureuser \
      --generate-ssh-keys    

    Your VM takes a few moments to come up. You named the VM my-vm. You use this name to refer to the VM in later steps.

Task 3: Install Nginx

After your VM is created, you'll use a Custom Script Extension to install Nginx. The Custom Script Extension is an easy way to download and run scripts on your Azure VMs. It's just one of the many ways you can configure the system after your VM is up and running.

    Run the following az vm extension set command to configure Nginx on your VM:
    Azure CLI 

    az vm extension set \
      --resource-group "IntroAzureRG" \
      --vm-name my-vm \
      --name customScript \
      --publisher Microsoft.Azure.Extensions \
      --version 2.1 \
      --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' \
      --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'    

    This command uses the Custom Script Extension to run a Bash script on your VM. The script is stored on GitHub. While the command runs, you can choose to examine the Bash script from a separate browser tab. To summarize, the script:
        Runs apt-get update to download the latest package information from the internet. This step helps ensure that the next command can locate the latest version of the Nginx package.
        Installs Nginx.
        Sets the home page, /var/www/html/index.html, to print a welcome message that includes your VM's host name.
