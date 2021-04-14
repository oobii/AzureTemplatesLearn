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

# Deploying two Vnets 

https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json

 az deployment group create --resource-group learn-b8e39432-b7ae-4793-9f1f-ad4c503ca2ec  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json --parameters azuredeployVnet1.parameters.json 
 
 az deployment group create --resource-group learn-b8e39432-b7ae-4793-9f1f-ad4c503ca2ec  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json --parameters azuredeployVnet2.parameters.json 

 # Add Peering to two existent Vnets
  ## Note: --parameter existingRemoteVirtualNetworkResourceGroupName

 az deployment group create --resource-group learn-b8e39432-b7ae-4793-9f1f-ad4c503ca2ec  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-existing-vnet-to-vnet-peering/azuredeploy.json --parameters azuredeployPeering.parameters.json  --parameter existingRemoteVirtualNetworkResourceGroupName=learn-b8e39432-b7ae-4793-9f1f-ad4c503ca2ec

# Prepare Azure and on-premises virtual networks by using Azure CLI commands

https://docs.microsoft.com/en-us/learn/modules/connect-on-premises-network-with-vpn-gateway/4-exercise-create-a-site-to-site-vpn-gateway-using-azure-cli-commands

az network vnet create \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name Azure-VNet-1 \
    --address-prefix 10.0.0.0/16 \
    --subnet-name Services \
    --subnet-prefix 10.0.0.0/24

    az network vnet subnet create \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --vnet-name Azure-VNet-1 \
    --address-prefix 10.0.255.0/27 \
    --name GatewaySubnet

    az network local-gateway create \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --gateway-ip-address 94.0.252.160 \
    --name LNG-HQ-Network \
    --local-address-prefixes 172.16.0.0/16

az network vnet create \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name HQ-Network \
    --address-prefix 172.16.0.0/16 \
    --subnet-name Applications \
    --subnet-prefix 172.16.0.0/24

    az network vnet subnet create \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --address-prefix 172.16.255.0/27 \
    --name GatewaySubnet \
    --vnet-name HQ-Network

    az network local-gateway create \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --gateway-ip-address 94.0.252.160 \
    --name LNG-Azure-VNet-1 \
    --local-address-prefixes 172.16.255.0/27

## Verify the topology

    az network vnet list --output table
    
    az network local-gateway list \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --output table

## Create a site-to-site VPN gateway by using Azure CLI commands

    az network public-ip create \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name PIP-VNG-Azure-VNet-1 \
    --allocation-method Dynamic

    az network vnet create \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name VNG-Azure-VNet-1 \
    --subnet-name GatewaySubnet

    az network vnet-gateway create \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name VNG-Azure-VNet-1 \
    --public-ip-address PIP-VNG-Azure-VNet-1 \
    --vnet VNG-Azure-VNet-1 \
    --gateway-type Vpn \
    --vpn-type RouteBased \
    --sku VpnGw1 \
    --no-wait

## Create the on-premises VPN gateway
    
    az network public-ip create \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name PIP-VNG-HQ-Network \
    --allocation-method Dynamic

    az network vnet create \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name VNG-HQ-Network \
    --subnet-name GatewaySubnet

    az network vnet-gateway create \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name VNG-HQ-Network \
    --public-ip-address PIP-VNG-HQ-Network \
    --vnet VNG-HQ-Network \
    --gateway-type Vpn \
    --vpn-type RouteBased \
    --sku VpnGw1 \
    --no-wait

    ##  monitor the progress
    watch -d -n 5 az network vnet-gateway list \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --output table

ActiveActive    EnableBgp    EnablePrivateIpAddress   GatewayType    Location        Name              ProvisioningState    ResourceGroup                         ResourceGuid                          VpnType
--------------  -----------  ------------------------ -------------  --------------  ----------------  -------------------  -----------------------------  ------------------------------------  ----------
False           False        False                    Vpn            southcentralus  VNG-Azure-VNet-1  Succeeded            learn-a311de02-eb09-492b-8370-6674af2b2d11  48dc714e-a700-42ad-810f-a8163ee8e001  RouteBased
False           False        False                    Vpn            southcentralus  VNG-HQ-Network    Succeeded            learn-a311de02-eb09-492b-8370-6674af2b2d11  49b3041d-e878-40d9-a135-58e0ecb7e48b  RouteBased

PIPVNGAZUREVNET1=$(az network public-ip show \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name PIP-VNG-Azure-VNet-1 \
    --query "[ipAddress]" \
    --output tsv)

    az network local-gateway update \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name LNG-Azure-VNet-1 \
    --gateway-ip-address $PIPVNGAZUREVNET1

    PIPVNGHQNETWORK=$(az network public-ip show \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name PIP-VNG-HQ-Network \
    --query "[ipAddress]" \
    --output tsv)

    az network local-gateway update \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name LNG-HQ-Network \
    --gateway-ip-address $PIPVNGHQNETWORK

    SHAREDKEY=<shared key>

    az network vpn-connection create \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name Azure-VNet-1-To-HQ-Network \
    --vnet-gateway1 VNG-Azure-VNet-1 \
    --shared-key $SHAREDKEY \
    --local-gateway2 LNG-HQ-Network

    az network vpn-connection create \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name HQ-Network-To-Azure-VNet-1  \
    --vnet-gateway1 VNG-HQ-Network \
    --shared-key $SHAREDKEY \
    --local-gateway2 LNG-Azure-VNet-1

    az network vpn-connection show \
    --resource-group learn-a311de02-eb09-492b-8370-6674af2b2d11 \
    --name Azure-VNet-1-To-HQ-Network  \
    --output table \
    --query '{Name:name,ConnectionStatus:connectionStatus}'

    Name                        ConnectionStatus
--------------------------  ------------------
Azure-VNet-1-To-HQ-Network  Connected

