Under contruction

# Kubernetes

# Azure Kubernetes Service (AKS)
1. Deploying a sample image managed by Microsoft to AKS
2. Deploying a sample image from your own Azure Container Registry (ACR) to AKS
3. Building an image from sample code with a sample Dockerfile, pushing it to ACR, and deploying it to AKS
4. Create a new Dockerfile for sample code, building an image, pushing it to ACR, and deploying it to AKS
## Deploying a sample image managed by Microsoft to AKS 
[This Learn tutorial](https://docs.microsoft.com/en-us/learn/modules/aks-deploy-container-app/) provides steps to deploy a sample AKS application. Below are the commands outlined in the Learn tutorial. I was able to successfully deploy an application on my Azure subscription (as opposed to the Learn Sandbox subscription) using the Azure Cloud Shell without any trouble. It takes about 15 minutes (mostly time waiting for commands to finish executing, including about 5 minutes to create the Kubernetes cluster, and 5 minutes to create the additional node pool). It uses a container that is available at mcr.microsoft.com/mslearn/samples/contoso-website.

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
```

Edit the yaml file with `code .` as follows

```
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: contoso-website
spec:
  selector: # Define the wrapping strategy
    matchLabels: # Match all pods with the defined labels
      app: contoso-website # Labels follow the `name: value` template
  template: # This is the template of the pod inside the deployment
    metadata:
      labels:
        app: contoso-website
    spec:
      nodeSelector:
        kubernetes.io/os: linux
      containers:
        - image: mcr.microsoft.com/mslearn/samples/contoso-website
          name: contoso-website
          resources:
            requests:
              cpu: 100m
              memory: 128Mi
            limits:
              cpu: 250m
              memory: 256Mi
          ports:
            - containerPort: 80
              name: http
```

Continue with the following commands

```
kubectl apply -f ./deployment.yaml

kubectl get deploy contoso-website

kubectl get pods

touch service.yaml
```

Edit the yaml file with `code .` as follows

```
#service.yaml
apiVersion: v1
kind: Service
metadata:
  name: contoso-website
spec:
  type: ClusterIP
  selector:
    app: contoso-website
  ports:
    - port: 80 # SERVICE exposed port
      name: http # SERVICE port name
      protocol: TCP # The protocol the SERVICE will listen to
      targetPort: http # Port to forward to in the POD
```

Continue as follows

```
kubectl apply -f ./service.yaml

kubectl get service contoso-website

touch ingress.yaml
```

Edit the yaml file with `code .` as follows

```
#ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: contoso-website
  annotations:
    kubernetes.io/ingress.class: addon-http-application-routing
spec:
  rules:
    - host: contoso.<uuid>.<region>.aksapp.io
      http:
        paths:
          - backend: # How the ingress will handle the requests
              service:
               name: contoso-website # Which service the request will be forwarded to
               port:
                 name: http # Which port in that service
            path: / # Which path is this rule referring to
            pathType: Prefix # See more at https://kubernetes.io/docs/concepts/services-networking/ingress/#path-types
```

Continue as follows

```
az aks show \
  -g $RESOURCE_GROUP \
  -n $CLUSTER_NAME \
  -o tsv \
  --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName
```

Use the output and edit ingress.yaml, and then continue as follows

```
kubectl apply -f ./ingress.yaml

kubectl get ingress contoso-website
```

### Deploying a sample image from your own Azure Container Registry (ACR) to AKS
Do the below first, and use `crcontosovideo.azurecr.io/contoso-website` in deployment.yaml, and then run `kubectl apply -f ./deployment.yaml`. This takes about 5 minutes. (Since the deployed image is the same as the deployed image in the section before, there's no change to the application)
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

### Building an image from sample code with a sample Dockerfile, pushing it to ACR, and deploying it to AKS
Do the below first, and use `crcontosovideo.azurecr.io/webimage` in deployment.yaml, and then run `kubectl apply -f ./deployment.yaml`. This should take about 5 minutes (including 2 minutes to build the image). The first time I tried it by using VS Code and its terminal worked, but the second time I tried it from the Azure Cloud Shell, the deployment failed with the new pod showing errors such as CrashLoopBackoff.
```
git clone https://github.com/MicrosoftDocs/mslearn-deploy-run-container-app-service.git

cd mslearn-deploy-run-container-app-service/dotnet
```
Modify the code with `code .` if you'd like, and then
```
az acr build --registry $CONTAINER_REGISTRY --image webimage .
```

### Create a new Dockerfile for sample code, building an image, pushing it to ACR, and deploying it to AKS
Do the below first, and use `crcontosovideo.azurecr.io/mvcmovie` in deployment.yaml, and then run `kubectl apply -f ./deployment.yaml`. This takes about 5 minutes (including 2 minutes to build the image).
```
dotnet new mvc -o MvcMovie
cd MvcMovie
touch Dockerfile
```
Craete the following Dockerfile
```
FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY MvcMovie.csproj .
RUN dotnet restore MvcMovie.csproj
COPY . .
RUN dotnet build MvcMovie.csproj -c Release -o /app

FROM build AS publish
RUN dotnet publish MvcMovie.csproj -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "MvcMovie.dll"]
```
Run the following commands
```
export CONTAINER_REGISTRY=crContosoVideo
az acr build --registry $CONTAINER_REGISTRY --image mvcmovie .
```
