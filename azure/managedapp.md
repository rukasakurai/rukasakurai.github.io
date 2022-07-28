# Service catalog managed application definitions & Managed applications
Service catalog managed application definitions & Managed applications may be useful in situations where in the IT context, there is a Publisher of an application and a Consumer of an application. To be careful, the Consumer is not the end-user of a GUI-based application but rather another IT role.

It is often used for ISVs to publish their applications in the Azure Marketplace.

# Trying it out
## Simply deploy
### References
- Quickstart: https://docs.microsoft.com/en-us/azure/azure-resource-manager/managed-applications/publish-service-catalog-app?tabs=azure-powershell
- Creating and deploying with CLI: https://docs.microsoft.com/en-us/azure/azure-resource-manager/managed-applications/scripts/managed-application-define-create-cli-sample
### My notes
Tested below steps through the Azure Cloud Shell on 2022-07-27
```
# Variable block
POSTFIX=20220728try1temp
appDefinitionResourceGroup=rg-$POSTFIX
managedAppResourceGroup=rg-$POSTFIX
appResourceGroup=mrg-$POSTFIX
LOC=japaneast
STO=st$POSTFIX
managedApp=ManagedStorage
tag="create-managed-application"

# Make directory for work
mkdir $POSTFIX
cd $POSTFIX

# Create the resource group for the publisher
az group create --name $appDefinitionResourceGroup --location $LOC

# Prepare the ARM template set
git clone https://github.com/rukasakurai/managedapp
cd managedapp
```

Edit if needed
#### Testing the ARM template with arm-ttk
```
cd ~
git clone https://github.com/Azure/arm-ttk
```
```
cd $POSTFIX/managedapp
pwsh
Import-Module /home/ruka/arm-ttk/arm-ttk/arm-ttk.psd1
$TestResults = Test-AzTemplate
echo $TestResults
$TestFailures =  $TestResults | Where-Object { -not $_.Passed }
echo $TestFailures
exit
```
### Testing the UI
https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade

#### Testing the ARM template by deploying it
##### With Azure CLO
Test if needed, e.g.,
- deploy with `az deployment group create --resource-group $appDefinitionResourceGroup --template-file mainTemplate.json`

##### With URI
https://qiita.com/tsubasaxZZZ/items/e18d123a83ff2e0aa6f3  
```
pwsh
$ARM_LOCATION=[System.Web.HTTPUtility]::UrlEncode("https://raw.githubusercontent.com/rukasakurai/managedapp/main/mainTemplate.json")
$URI="https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/$ARM_LOCATION"
echo $URI
```

Below does not work
```
pwsh
$ARM_LOCATION=[System.Web.HTTPUtility]::UrlEncode("https://raw.githubusercontent.com/rukasakurai/managedapp/main/mainTemplate.json")
$UI_DEF_LOCATION=[System.Web.HTTPUtility]::UrlEncode("https://raw.githubusercontent.com/rukasakurai/managedapp/main/createUiDefinition.json")
$URI="https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/$ARM_LOCATION/uiFormDefinitionUri/$UI_DEF_LOCATION"
echo $URI
```


```
zip -r app.zip mainTemplate.json createUiDefinition.json
```


https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsebassem%2FAzure-ARM-UI%2Fmain%2Fazuredeploy.json/uiFormDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2Fsebassem%2FAzure-ARM-UI%2Fmain%2FazuredeployUI.json

https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fsebassem%2FAzure-ARM-UI%2Fmain%2Fazuredeploy.json/uiFormDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2Fsebassem%2FAzure-ARM-UI%2Fmain%2FazuredeployUI.json


#### Option 1: Upload to GitHub
```
git add app.zip
git commit -m "Updated app.zip"
git push

packagefileuri=https://github.com/rukasakurai/managedapp/raw/main/app.zip
```

##### Option 2: Upload to Blob
```
az storage account create \
    --name $STO \
    --resource-group $appDefinitionResourceGroup \
    --location $LOC \
    --sku Standard_LRS \
    --kind StorageV2

az storage container create \
    --account-name $STO \
    --name appcontainer \
    --public-access blob

az storage blob upload \
    --account-name $STO \
    --container-name appcontainer \
    --name "app.zip" \
    --file "app.zip"

packagefileuri=$(az storage blob url --account-name $STO --container-name appcontainer --name app.zip --output tsv)
```

#### Create the definition for a managed application
```
groupid=<groupId>

roleid=$(az role definition list --name Owner --query [].name --output tsv)
```
The definition prepared above
```
az managedapp definition create \
  --name $managedApp \
  --location $LOC \
  --resource-group $appDefinitionResourceGroup \
  --lock-level ReadOnly \
  --display-name "Managed Storage Account" \
  --description "Managed Azure Storage Account" \
  --authorizations "$groupid:$roleid" \
  --package-file-uri $packagefileuri
```
Temp note: One of the definitions from the set of samples
```
az managedapp definition create \
  --name "ManagedWebApp" \
  --location $LOC \
  --resource-group $appDefinitionResourceGroup \
  --lock-level ReadOnly \
  --display-name "Managed Web Application" \
  --description "Web App with Azure mgmt" \
  --authorizations "$groupid:$roleid" \
  --package-file-uri "https://raw.githubusercontent.com/Azure/azure-managedapp-samples/master/Managed Application Sample Packages/201-managed-web-app/managedwebapp.zip"
```

####  Create managed application
```
# Create application resource group
echo "Creating $managedAppResourceGroup in "$LOC"..."
az group create --name $managedAppResourceGroup --location "$LOC" --tags $tag

# Get ID of managed application definition
appid=$(az managedapp definition show --name $managedApp --resource-group $appDefinitionResourceGroup --query id --output tsv)

# Get subscription ID
subid=$(az account show --query id --output tsv)

# Construct the ID of the managed resource group (the resource group containing the resources defined by the ARM template that was used to create the Service catalog application definition)
managedGroupId=/subscriptions/$subid/resourceGroups/$appResourceGroup

# Create the managed application
storageAccountNamePrefix=st${POSTFIX:0:5}
az managedapp create --name storageApp --location "$LOC" --kind "Servicecatalog" --resource-group $managedAppResourceGroup --managedapp-definition-id $appid --managed-rg-id $managedGroupId --parameters "{\"storageAccountNamePrefix\": {\"value\": \"st$storageAccountNamePrefix\"}, \"storageAccountType\": {\"value\": \"Standard_LRS\"}}"
```

## How to update
## Reference
https://arsenvlad.medium.com/setting-incremental-deployment-mode-for-azure-managed-app-service-catalog-definition-fd6e71ffcf07

## What I've tried
I've tried updating the zip file referenced in package-file-uri of the application definition, and then the steps in Arsen Vladimirskiy's blog post to update via REST PUT (this seemed to be needed). But not sure this is the right approach.

## ARMのテストや失敗時のロールバックについて
### Syntax Error
#### 1st time
#### 2nd time (updating the resource)

### Azure Policy Error
#### 1st time
#### 2nd time (updating the resource)

# Other notes
## Incremental / Complete
Complete might be the recommended. Also it might not matter that much.