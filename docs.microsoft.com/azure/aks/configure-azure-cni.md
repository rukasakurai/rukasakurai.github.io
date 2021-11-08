POSTFIX="20211021temp"
RG="rg-${POSTFIX}"
LOC="japaneast"
PLUGIN=azure
AKSNAME="aks-${POSTFIX}"
VNET_NAME="vnet-${POSTFIX}"
AKSSUBNET_NAME="aks-subnet"

# Create Resource Group
az group create --name $RG --location $LOC

# Dedicated virtual network with AKS subnet
az network vnet create \
    --resource-group $RG \
    --name $VNET_NAME \
    --location $LOC \
    --address-prefixes 10.42.0.0/16 \
    --subnet-name $AKSSUBNET_NAME \
    --subnet-prefix 10.42.1.0/24

SUBNETID=$(az network vnet subnet show -g $RG --vnet-name $VNET_NAME --name $AKSSUBNET_NAME --query id -o tsv)

az aks create \
    --resource-group $RG \
    --name $AKSNAME \
    --network-plugin azure \
    --vnet-subnet-id $SUBNETID \
    --docker-bridge-address 172.17.0.1/16 \
    --dns-service-ip 10.2.0.10 \
    --service-cidr 10.2.0.0/24 \
    --generate-ssh-keys