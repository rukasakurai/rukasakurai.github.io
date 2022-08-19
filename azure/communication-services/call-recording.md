https://docs.microsoft.com/en-us/azure/communication-services/quickstarts/voice-video-calling/call-recording-sample?pivots=programming-language-csharp


```
POSTFIX=20220810try1temp
RG=rg-$POSTFIX
LOC=japaneast
STO=st$POSTFIX
ACS=acs-$POSTFIX
CONTAINER=cont-$POSTFIX

az group create -n $RG -l $LOC
az communication create --name $ACS --location "Global" --data-location "Japan" --resource-group $RG
az communication list-key -n $ACS -g $RG
export COMMUNICATION_SERVICES_CONNECTION_STRING=`az communication list-key -n $ACS -g $RG | jq .primaryConnectionString`
echo $COMMUNICATION_SERVICES_CONNECTION_STRING

az storage account create \
  --name $STO \
  --resource-group $RG \
  --location $LOC \
  --sku Standard_LRS \
  --kind StorageV2

az storage container create \
    --name $CONTAINER \
    --account-name $STO \
    --auth-mode login
