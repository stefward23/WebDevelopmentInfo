

# Powershell
## Create new resource group
New-AzResourceGroup -Name rg_powershell -Location "East US"


# Azure CLI
## Create new resource group
az group create --name rg_cli --location eastus 
<<<<<<< HEAD


# azCopy 
## Copy a file to a container
azcopy copy "filename" "SAS_generated_URL"
## Copy a file from container to container
azcopy copy "SAS_generated_URL" "SAS_generated URL" "container_URL"

# Azure Storage Explorer
=======
## Create new user
az ad user create \
  --display-name "CLICreatedUser" \
  --user-principal-name clicreateduser@stefmward23outlook.onmicrosoft.com \
  --password "" \
>>>>>>> b703c1fc04b0df59dd5ac649cc07bf09588a8fae


# Creating a VM
az vm create --resource-group "rg-MyFirstResourceGroup" --name "vm-2" --image Win2022Datacenter --public-ip-sku Standard --admin-username "brett" --admin-password "" --vnet-name "VNET-Two" --subnet "default" --nsg-rule "RDP"


# Powershell script to install and run IIS 
az vm run-command invoke -g "rg-MyFirstResourceGroup" -n "vm-2" --command-id RunPowerShellScript --scripts "Install-WindowsFeature -name Web-Server -IncludeManagementTools"

# Open port for RDP
az vm open-port --port 80 --resource-group "rg-MyFirstResourceGroup" --name "vm-2"


