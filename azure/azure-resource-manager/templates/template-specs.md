https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/quickstart-create-template-specs?tabs=azure-powershell

# CLI commands
## Set variables
```
POSTFIX=20220804temp1
LOC=japaneast
subid=<subid>
RG=rg-$POSTFIX
TEMPLATE_SPEC=ts-$POSTFIX
```

## Prepare ARM template
```
mkdir $POSTFIX
cd $POSTFIX
```

## Create Template Spec
```
subid=$(az account show --query id --output tsv)
az account set --subscription $subid

az group create --name $RG --location $LOC

az ts create \
  --name $TEMPLATE_SPEC \
  --version "1.0" \
  --resource-group $RG \
  --location $LOC \
  --template-file "azuredeploy.json"
```

## Deploy template spec
```
TEMPLATE_SPEC=Application_1

id=$(az ts show --name $TEMPLATE_SPEC --resource-group $RG --version "1.0" --query "id")

az deployment group create \
  --resource-group $RG \
  --template-spec $id \
  --parameters storageAccountType='Standard_LRS'
```

## Sharing template specs
<a href="https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/<ARMテンプレートURIエンコードしたもの><img src="https://aka.ms/deploytoazurebutton"/>

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/<ARMテンプレートURIエンコードしたもの><img src="https://aka.ms/deploytoazurebutton"/>

https://techcommunity.microsoft.com/t5/azure-architecture-blog/creating-a-custom-and-secure-azure-portal-offering/ba-p/3038344

https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-to-azure-button