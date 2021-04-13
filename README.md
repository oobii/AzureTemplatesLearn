# Azure Lab - templates
az login
az group create \
  --name {name of your resource group} \
  --location "{location}”
templateFile="{provide-the-path-to-the-template-file}"

az deployment group create \
  --name blanktemplate \
  --resource-group myResourceGroup \
  --template-file $templateFile


az resource list -g examplegroup

az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
az provider list --query "sort_by([?registrationState=='Registered'].{Provider:namespace, Status:registrationState}, &Provider)" --out table

—
az login
az account list \
   --refresh \
   --query "[?contains(name, 'Concierge Subscription')].id" \
   --output table

az account list --refresh --all 

az account set --subscription "Concierge Subscription”

az account set --subscription {your subscription ID}

az group list -o table

az configure --defaults group=learn-48a3acae-af44-4213-8537-7ebe65808b47

templateFile="azuredeploy.json"
today=$(date +"%d-%b-%Y")
DeploymentName="blanktemplate-"$today

az group deployment create \
 --name $DeploymentName \
 --template-file $templateFile

templateFile="azuredeploy.json"
today=$(date +"%d-%b-%Y")
DeploymentName="addstorage-"$today

az group deployment create \
  --name $DeploymentName \
  --template-file $templateFile

# Deploying parameterized template

templateFile="azuredeploy.json"
today=$(date +"%d-%b-%Y")
DeploymentName="addnameparameter-"$today

az deployment group create \
  --name $DeploymentName \
  --template-file $templateFile \
  --parameters storageName={your-unique-name}  

templateFile="azuredeploy.json"
today=$(date +"%d-%b-%Y")
DeploymentName="addSkuParameter-"$today

az deployment group create \
  --name $DeploymentName \
  --template-file $templateFile \
  --parameters storageSKU=Basic storageName={your-unique-name}

  # Deploy Template with output
  today=$(date +"%d-%b-%Y");DeploymentName="addMyOutput-"$today; az deployment group create   --name $DeploymentName   --template-file $templateFile   --parameters  storageName=learnexerciseem0
  
  # Create an expression to set a unique storage account name
  az deployment group create   --name $DeploymentName   --template-file $templateFile   --parameters storagePrefix=storageEM0

  # Add Tags

templateFile="azuredeploy.json"
today=$(date +"%d-%b-%Y")
DeploymentName="updateTags-"$today

# Deploying with Parameter file

templateFile="azuredeploy.json"
devParameterFile="azuredeploy.parameters.dev.json"
today=$(date +"%d-%b-%Y")
DeploymentName="addParameterFile-"$today

az deployment group create \
  --name $DeploymentName \
  --template-file $templateFile \
  --parameters $devParameterFile

# Deploying with what-if check for changes 
az deployment group create       --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-before.json"

After mofification - checking what has changed
// Dont use it   az1 deployment group create \
  --dont-confirm-with-what-if \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"

az deployment group   what-if   --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/what-if/what-if-after.json"

You'll notice that the result is color coded in addition to having a prefix:
Purple and ~ for any modifications
Green and + for new resources to be created
Orange and - for deletions

# Deploying Linked Template 

templateFile=./linkedtemplate.json
today=$(date +"%Y-%m-%d")
deploymentname="DeployLocalTemplate-3-"$today
az deployment group create --name $deploymentname --template-file $templateFile
  
# Deploying with script

az account set --subscription XXX
az account list -o table
az group create --location canadacentral --name $resourceGroupName
today=$(date +"%d-%b-%Y")
deploymentName="deploymentscript-"$today
templateFile="azuredeployscript.json"
az deployment group create     --resource-group $resourceGroupName     --name $deploymentName     --template-file $templateFile
