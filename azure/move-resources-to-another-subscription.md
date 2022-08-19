# Overview
I am documenting my experience of moving resources from one Azure Subscription to another Azure Subscription under a different tenant.

# Step 1: Consider moving resources (e.g., through portal or with az resource move)
https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/move-resource-group-and-subscription
## Cannot move across AAD tenants
It's documented that the move cannot happen across Subscription under different Azure AD tenants.
Did some additional resource to double check
- It should be possible to do using a third pay-as-you-go subscription, by moving the third pay-as-you-go subscription across tenants: https://social.technet.microsoft.com/wiki/contents/articles/51360.azure-how-to-move-resources-between-subscriptions-under-different-tenants.aspx
- https://docs.microsoft.com/en-us/answers/questions/505287/move-of-resources-between-2-different-tenants-subs.html
- https://github.com/MicrosoftDocs/azure-docs/issues/69654

## Tried on 2022-08-17 anyway
```
source_subscription=
source_tenant=
destination_subscription=
destination_tenant=
az login --tenant $source_tenant
az account show --subscription $source_subscription --query tenantId
az login --tenant $destination_tenant
az account show --subscription $destination_subscription --query tenantId
```

```
az account set -s $destination_subscription
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```

```
source_rg=
destination_rg=
source_resource=$(az resource show -g $source_rg -n <storagename> --resource-type "Microsoft.Storage/storageAccounts" --query id --output tsv)
az resource invoke-action --action validateMoveResources --ids $source_resource
az resource move --destination-group $destination_rg --ids $source_resource
```

I get an error. Decided not to proceed further since this method wouldn't work anyway across multiple subscriptions

# Step 2: Redeploying resources in new tenant
## Step 2-1: Generate ARM template from existing resources
https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/export-template-cli
### From resource group or From history
I believe one would typically chose "From resource group" in most practical scenarios, since each deployment in a histroy typically only represents a step in the overall imperative process
### Tried on 2022-08-17
#### From resource group
```
RG=rg-DemoProduct
az group export --name $RG
az group export --name $RG > temp/azuredeploytemp.json
```
##### Errors
Resulted in the following warnings/errors, but was able to export a partially complete template
```
WARNING: Export template operation completed with errors. Some resources were not exported. Please see details for more information.
ERROR: The schema of resource type 'Microsoft.SaaS/resources' is not available. Resources of this type will not be exported to the template.
ERROR: Could not get resources of the type 'Microsoft.Sql/servers/databases/workloadGroups'. Resources of this type will not be exported.
ERROR: Could not get resources of the type 'Microsoft.Sql/servers/databases/advisors'. Resources of this type will not be exported.
ERROR: Could not get resources of the type 'Microsoft.OperationalInsights/workspaces/dataSources'. Resources of this type will not be exported.
ERROR: Could not get resources of the type 'Microsoft.ContainerService/managedClusters/trustedAccessRoleBindings'. Resources of this type will not be exported.
ERROR: Could not get resources of the type 'Microsoft.ContainerService/managedClusters/privateEndpointConnections'. Resources of this type will not be exported.
ERROR: Could not get resources of the type 'Microsoft.ContainerRegistry/registries/connectedRegistries'. Resources of this type will not be exported.
ERROR: Could not get resources of the type 'Microsoft.ApiManagement/service/portalRevisions'. Resources of this type will not be exported.
ERROR: Could not get resources of the type 'Microsoft.ApiManagement/service/apis/policy'. Resources of this type will not be exported.
ERROR: Could not get resources of the type 'Microsoft.ApiManagement/service/apis/operations/policy'. Resources of this type will not be exported.
ERROR: Could not get resources of the type 'Microsoft.Insights/components/Annotations'. Resources of this type will not be exported.
ERROR: Could not get resources of the type 'microsoft.insights/components/myanalyticsItems'. Resources of this type will not be exported.
```

Therefore the following need to be created in some other way
- SendGrid
The others are likely not resources that are leveraged 

#### From history
```
az deployment group export --resource-group $RG --name app-DemoProduct
az deployment group export --resource-group $RG --name app-DemoProduct > temp/fromdeploymenttemp.json
```
# Step 2-2: Try deploying the ARM template
https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-cli
### Considerations group/subscription/tenant
az deployment group create
az deployment sub create

while it's possible to deploy subscription level resources with ARM (e.g., resource groups), I believe it's easier to create them using az group create, if it's just a resource group

### Tried on 2022-08-17
```
az account set -s $source_subscription
az deployment sub create --location japaneast --template-file temp/rgarmtemp.json
```
And deploying a resource group with ARM worked.

```
RG=rg-temp
az account set -s $destination_subscription
az group create -l japaneast -n $RG
az deployment group create -g $RG --template-file temp/fromdeploymenttemp.json

## Step 2-3: Generate Bicep file
https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/decompile?tabs=azure-cli
### Tried on 2022-08-17
```
az bicep upgrade
az bicep decompile --file temp/fromdeploymenttemp.json
az bicep decompile --file temp/azuredeploytemp.json
```