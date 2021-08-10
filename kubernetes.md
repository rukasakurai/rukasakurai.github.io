Under contruction

# Kubernetes

# Azure Kubernetes Service (AKS)
## Sample AKS application
The below Learn tutorial provides steps to deploy a sample AKS application
https://docs.microsoft.com/en-us/learn/modules/aks-deploy-container-app/
Copying the commands outlined in the Learn tutorial. Was able to successfully deploy an application on my Azure subscription (as opposed to the Learn Sandbox subscription) without any trouble.
It uses a container that is available at mcr.microsoft.com/mslearn/samples/contoso-website
```
export RESOURCE_GROUP=rg-aks-contoso-video

az group create --location japaneast \
                --name $RESOURCE_GROUP

export CLUSTER_NAME=aks-contoso-video

az aks create \
    --resource-group $RESOURCE_GROUP \
    --name $CLUSTER_NAME \
    --node-count 2 \
    --enable-addons http_application_routing \
    --generate-ssh-keys \
    --node-vm-size Standard_B2s \
    --network-plugin azure

az aks nodepool add \
    --resource-group $RESOURCE_GROUP \
    --cluster-name $CLUSTER_NAME \
    --name userpool \
    --node-count 2 \
    --node-vm-size Standard_B2s

az aks get-credentials --name $CLUSTER_NAME --resource-group $RESOURCE_GROUP

kubectl get nodes

touch deployment.yaml
<Edit yaml file>

kubectl apply -f ./deployment.yaml

kubectl get deploy contoso-website

kubectl get pods

touch service.yaml
<Edit yaml file>

kubectl apply -f ./service.yaml

kubectl get service contoso-website

touch ingress.yaml
<Edit yaml file>

az aks show \
  -g $RESOURCE_GROUP \
  -n $CLUSTER_NAME \
  -o tsv \
  --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName

kubectl apply -f ./ingress.yaml

kubectl get ingress contoso-website

```

### Sample AKS application (deployment from Azure Container Registry)
Do the below first, and use `crcontosovideo.azurecr.io/contoso-website` in deployment.yaml
```
export CONTAINER_REGISTRY=crContosoVideo

az acr create --resource-group $RESOURCE_GROUP \
  --name $CONTAINER_REGISTRY --sku Basic

az acr import \
--name $CONTAINER_REGISTRY \
--source mcr.microsoft.com/mslearn/samples/contoso-website \
--image contoso-website

az acr repository show-manifests \
  --name $CONTAINER_REGISTRY \
  --repository contoso-website

az aks update -n $CLUSTER_NAME -g $RESOURCE_GROUP --attach-acr $CONTAINER_REGISTRY

  ```

### Sample AKS application (deployment from code to ACR to AKS)
Do the below first, and use `crcontosovideo.azurecr.io/webimage` in deployment.yaml
```
git clone https://github.com/MicrosoftDocs/mslearn-deploy-run-container-app-service.git

cd mslearn-deploy-run-container-app-service/dotnet

<Modify the code if you'd like>

az acr build --registry $CONTAINER_REGISTRY --image webimage .
```
