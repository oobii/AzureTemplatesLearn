Azure Lab - templates
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

*Deploying parameterized template*

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

  *Deploy Template with output*
  today=$(date +"%d-%b-%Y");DeploymentName="addMyOutput-"$today; az deployment group create   --name $DeploymentName   --template-file $templateFile   --parameters  storageName=learnexerciseem0
  
  *Create an expression to set a unique storage account name*
  az deployment group create   --name $DeploymentName   --template-file $templateFile   --parameters storagePrefix=storageEM0