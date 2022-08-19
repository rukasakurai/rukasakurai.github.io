https://docs.microsoft.com/en-us/azure/container-apps/microservices-dapr?tabs=bash

# Tried on 2022-08-18
```
POSTFIX=20220818temp
subid=
RESOURCE_GROUP="my-container-apps"
LOCATION="japaneast"
CONTAINERAPPS_ENVIRONMENT="my-environment"
STORAGE_ACCOUNT="sttemprukatemp"
STORAGE_ACCOUNT_CONTAINER="mycontainer"
REGISTRY_USERNAME=
REGISTRY_PASSWORD=

az upgrade
az login
az account set -s $subid
az extension add --name containerapp --upgrade
az provider register --namespace Microsoft.App
az provider register --namespace Microsoft.OperationalInsights

az group create \
  --name $RESOURCE_GROUP \
  --location $LOCATION

az containerapp env create \
  --name $CONTAINERAPPS_ENVIRONMENT \
  --resource-group $RESOURCE_GROUP \
  --location "$LOCATION"

az storage account create \
  --name $STORAGE_ACCOUNT \
  --resource-group $RESOURCE_GROUP \
  --location "$LOCATION" \
  --sku Standard_RAGRS \
  --kind StorageV2

STORAGE_ACCOUNT_KEY=`az storage account keys list --resource-group $RESOURCE_GROUP --account-name $STORAGE_ACCOUNT --query '[0].value' --out tsv`

mkdir $POSTFIX
cd $POSTFIX
touch statestore.yaml
```

```
# statestore.yaml for Azure Blob storage component
componentType: state.azure.blobstorage
version: v1
metadata:
- name: accountName
  value: XXX
- name: accountKey
  secretRef: account-key
- name: containerName
  value: mycontainer
secrets:
- name: account-key
  value: YYY
scopes:
- nodeapp
```

```
az containerapp env dapr-component set \
    --name $CONTAINERAPPS_ENVIRONMENT --resource-group $RESOURCE_GROUP \
    --dapr-component-name statestore \
    --yaml statestore.yaml

az containerapp create \
  --name nodeapp \
  --resource-group $RESOURCE_GROUP \
  --environment $CONTAINERAPPS_ENVIRONMENT \
  --image dapriosamples/hello-k8s-node:latest \
  --target-port 3000 \
  --ingress 'internal' \
  --min-replicas 1 \
  --max-replicas 1 \
  --enable-dapr \
  --dapr-app-id nodeapp \
  --dapr-app-port 3000 \
  --env-vars 'APP_PORT=3000' \
  --registry-server hub.docker.com \
  --registry-username $REGISTRY_USERNAME \
  --registry-password $REGISTRY_PASSWORD

az containerapp create \
  --name pythonapp \
  --resource-group $RESOURCE_GROUP \
  --environment $CONTAINERAPPS_ENVIRONMENT \
  --image dapriosamples/hello-k8s-python:latest \
  --min-replicas 1 \
  --max-replicas 1 \
  --enable-dapr \
  --dapr-app-id pythonapp \
  --registry-server hub.docker.com \
  --registry-username $REGISTRY_USERNAME \
  --registry-password $REGISTRY_PASSWORD

LOG_ANALYTICS_WORKSPACE_CLIENT_ID=`az containerapp env show --name $CONTAINERAPPS_ENVIRONMENT --resource-group $RESOURCE_GROUP --query properties.appLogsConfiguration.logAnalyticsConfiguration.customerId --out tsv`

az monitor log-analytics query \
  --workspace $LOG_ANALYTICS_WORKSPACE_CLIENT_ID \
  --analytics-query "ContainerAppConsoleLogs_CL | where ContainerAppName_s == 'nodeapp' and (Log_s contains 'persisted' or Log_s contains 'order') | project ContainerAppName_s, Log_s, TimeGenerated | sort by TimeGenerated | take 5" \
  --out table

az group delete --resource-group $RESOURCE_GROUP

# Questions
How to switch state stores:
https://github.com/MicrosoftDocs/azure-docs/issues/97310
https://docs.dapr.io/reference/components-reference/supported-state-stores/setup-azure-blobstorage/