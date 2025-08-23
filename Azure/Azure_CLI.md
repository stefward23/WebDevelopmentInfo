

# Powershell
## Create new resource group
New-AzResourceGroup -Name rg_powershell -Location "East US"


# Azure CLI
## Create new resource group
az group create --name rg_cli --location eastus 


# azCopy 
## Copy a file to a container
azcopy copy "filename" "SAS_generated_URL"
## Copy a file from container to container
azcopy copy "SAS_generated_URL" "SAS_generated URL" "container_URL"

# Azure Storage Explorer
