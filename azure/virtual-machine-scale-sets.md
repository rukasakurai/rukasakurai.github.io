Under contruction
# Virtual Machine Scale Sets
https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview

# Creating a Virtual Machine Scale Set from a Windows Virtual Machine on-premises
## On-premisesのWindows仮想マシンからAzureの仮想マシン作成
例: Windows Server 2019 の Standard エディションの仮想マシンを作成
Hyper-V の仮想マシンを Azure VM に移行する
https://qiita.com/KentoNaka/items/d33fdcb3c8edacb38900

Azure にアップロードする Windows VHD または VHDX を準備する
https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/prepare-for-upload-vhd-image

VHD を Azure にアップロードするか、他のリージョンにマネージド ディスクをコピーする - Azure PowerShell
https://docs.microsoft.com/ja-jp/azure/virtual-machines/windows/disks-upload-vhd-to-managed-disk-powershell

## Shared Image Gallery
### Preparing the prerequisite
Prerequisite: An Azure VM

RESOURCE_GROUP=rg-temp-`date '+%Y%m%d%H%M%S'`
VNET_NAME=vm-sourceVNET
SOURCE_VM=vm-source
PIP_NAME=MyIp
BASTION_NAME=MyBastion

az group create --location japaneast --name $RESOURCE_GROUP

az network vnet create --resource-group $RESOURCE_GROUP --name $VNET_NAME --address-prefix 10.0.0.0/16 --location japaneast

az vm create \
    --resource-group $RESOURCE_GROUP \
    --name $SOURCE_VM \
    --image Win2019Datacenter \
    --public-ip-sku Standard \
    --vnet-name $VNET_NAME
    --admin-username azureuser

az network vnet subnet create --address-prefixes 10.0.1.0/26 --name AzureBastionSubnet --resource-group $RESOURCE_GROUP --vnet-name $VNET_NAME

az network public-ip create --resource-group $RESOURCE_GROUP --name $PIP_NAME --sku Standard --location japaneast
az network bastion create --name $BASTION_NAME --public-ip-address $PIP_NAME --resource-group $RESOURCE_GROUP --vnet-name $VNET_NAME --location japaneast

https://docs.microsoft.com/en-us/azure/bastion/tutorial-create-host-portal#remove-vm-public-ip-address

Make a modification (e.g., install an application, change the language, add a text file)

### Creating a shared image gallery

SIG_NAME=myGallery
az sig create --resource-group $RESOURCE_GROUP --gallery-name $SIG_NAME

IMAGE_DEFINITION=myImageDefinition
az sig image-definition create \
   --resource-group $RESOURCE_GROUP \
   --gallery-name $SIG_NAME \
   --gallery-image-definition $IMAGE_DEFINITION \
   --publisher myPublisher \
   --offer myOffer \
   --sku mySKU \
   --os-type Windows \
   --os-state specialized

az vm get-instance-view -g $RESOURCE_GROUP -n $SOURCE_VM --query id

az sig image-version create \
   --resource-group $RESOURCE_GROUP \
   --gallery-name $SIG_NAME \
   --gallery-image-definition $IMAGE_DEFINITION \
   --gallery-image-version 1.0.0 \
   --target-regions "japaneast" "japanwest" \
   --replica-count 2 \
   --managed-image "/subscriptions/5ebfbee2-ecb2-4371-81df-1883203a2715/resourceGroups/rg-20210831temp2/providers/Microsoft.Compute/virtualMachines/vm-source"

# Updating Azure VM Scale Set without downtime with Rolling Updates
https://medium.com/microsoftazure/updating-azure-vm-scale-set-without-downtime-with-rolling-updates-734dcb540d6b

## Creating VM from image
PortalでSIGからVMイメージ作成して、txtファイルがあることを確認できた

## Creating VMSS from image
PortalでSIGからBastionと同一VNet内にVMSSを作成して、BastionからVMSS内のどれかのVMに接続し、txtファイルがあることを確認できた

以下のようにCLIでもできるはず

IMAGE_ID=`az sig image-definition show -r $SIG_NAME -g $RESOURCE_GROUP -i $IMAGE_DEFINITION | jq '.id' -r`

VMSS_NAME=tempMyScaleSet2
az vmss create \
   --resource-group $RESOURCE_GROUP \
   --name $VMSS_NAME \
   --image $IMAGE_ID \
   --vnet-name $VNET_NAME \
   --specialized

BastionからConnectし、イメージに接続することができた