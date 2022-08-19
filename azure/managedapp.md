# Service catalog managed application definitions & Managed applications
Service catalog managed application definitions & Managed applications may be useful in situations where in the IT context, there is a Publisher of an application and a Consumer of an application. To be careful, the Consumer is not the end-user of a GUI-based application but rather another IT role.

It is often used for ISVs to publish their applications in the Azure Marketplace.

# Trying it out
## Simply deploy
### References
- Quickstart: https://docs.microsoft.com/en-us/azure/azure-resource-manager/managed-applications/publish-service-catalog-app?tabs=azure-powershell
- Creating and deploying with CLI: https://docs.microsoft.com/en-us/azure/azure-resource-manager/managed-applications/scripts/managed-application-define-create-cli-sample
### My notes
Tested below steps through the Azure Cloud Shell on 2022-07-27 (and some subsequent days after that)
```
APP_DEF=Application_1
POSTFIX=20220815try1temp
appDefinitionResourceGroup=rg-$POSTFIX
managedAppResourceGroup=rg-$POSTFIX
appResourceGroup=mrg-$POSTFIX
LOC=japaneast
STO=st$POSTFIX
tag="create-managed-application"

mkdir $POSTFIX
cd $POSTFIX

az group create --name $appDefinitionResourceGroup --location $LOC

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

#### Testing the ARM template
##### With what-if
Test if needed, e.g.,
- deploy with `az deployment group what-if --resource-group $appDefinitionResourceGroup --template-file mainTemplate.json`

##### Try deploying ith URI
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

### Create the zip file

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

roleid=$(az role definition list --name Reader --query [].name --output tsv)
roleid=$(az role definition list --name Owner --query [].name --output tsv)
roleid=$(az role definition list --name "Storage Account Contributor" --query [].name --output tsv) で作るとRGにもそれが付与される
```
The definition prepared above
```
az managedapp definition create \
  --name $APP_DEF \
  --location $LOC \
  --resource-group $appDefinitionResourceGroup \
  --lock-level ReadOnly \
  --display-name ${APP_DEF}_DisplayName \
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
appid=$(az managedapp definition show --name $APP_DEF --resource-group $appDefinitionResourceGroup --query id --output tsv)

# Get subscription ID
subid=$(az account show --query id --output tsv)

# Construct the ID of the managed resource group (the resource group containing the resources defined by the ARM template that was used to create the Service catalog application definition)
managedGroupId=/subscriptions/$subid/resourceGroups/$appResourceGroup

# Create the managed application
storageAccountNamePrefix=st${POSTFIX:0:5}
az managedapp create --name storageApp --location "$LOC" --kind "Servicecatalog" --resource-group $appDefinitionResourceGroup --managedapp-definition-id $appid --managed-rg-id $managedGroupId --parameters "{\"storageAccountNamePrefix\": \"$storageAccountNamePrefix\", \"storageAccountType\": \"Standard_LRS\"}"
```

## How to update
## Reference
https://arsenvlad.medium.com/setting-incremental-deployment-mode-for-azure-managed-app-service-catalog-definition-fd6e71ffcf07

## What I've tried
I've tried updating the zip file referenced in package-file-uri of the application definition, and then the steps in Arsen Vladimirskiy's blog post to update via REST PUT (this seemed to be needed). But not sure this is the right approach.

### GET the application definition
```
subid=$(az account show --query id --output tsv)

az rest --method get --url /subscriptions/$subid/resourceGroups/$managedAppResourceGroup/providers/Microsoft.Solutions/applicationDefinitions/$APP_DEF?api-version=2019-07-01 -o json > managedapp-definition.json
```

### Modify the JSON
- Add the `packageFileUri`, which is missing
  - Without it, the subsequent PUT will result in the erro `Bad Request({"error":{"code":"ApplicationDefinitionMissingRequiredProperties","message":"The application definition 'ManagedStorage' request is invalid because at least one of 'MainTemplate, CreateUiDefinition' properties is missing."}})`
- Make any other modifications needed
```
"properties": {
    "deploymentPolicy": {
    "packageFileUri": "<URI>",
...
```

### PUT after modifying the JSON
```
az rest --method put --url /subscriptions/$subid/resourceGroups/$managedAppResourceGroup/providers/Microsoft.Solutions/applicationDefinitions/$APP_DEF?api-version=2019-07-01 --body @managedapp-definition.json -o json
```
(Tested that it's also possible to update the definition using an ARM template and `az deployment group create`)

# Other notes
## Incremental / Complete
Complete might be the recommended. Also it might not matter that much.

# When deployment fails
Q: After a deployment failure of a managed application (i.e., instanciating a managed application and its resource from a managed application definition), is it ok to just try to redeploy it again (e.g., after removing the issue that was causing the failure)?
A: Delete the Managed Application, and redeploy
- Deleting the Managed Application will delete the managed resource group and resources
- Deleting is required since the "Managed Application Details - Application Name" cannot be the same with an exiting one

# Updating an existing deployment definition and deployment
Can't updated a managed application and its managed resources from an updated Service catalog managed application definition
https://stackoverflow.microsoft.com/questions/317377
https://stackoverflow.com/questions/65361860/how-to-update-existing-azure-managed-applications-with-a-new-package-version

# Bring your own storage for the managed application definition
https://docs.microsoft.com/en-us/azure/azure-resource-manager/managed-applications/publish-service-catalog-app?tabs=azure-powershell#bring-your-own-storage-for-the-managed-application-definition