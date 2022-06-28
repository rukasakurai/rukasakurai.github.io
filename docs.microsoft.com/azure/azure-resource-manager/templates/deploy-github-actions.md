https://docs.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-github-actions

# 2022-06-28 Notes
ALIAS=20220628temp
RG=rg-$ALIAS
LOC=japaneast
az group create -n $RG -l $LOC

SUB_ID=xxxx
SP_NAME=sp-$ALIAS

az ad sp create-for-rbac --name $SP_NAME --role contributor --scopes /subscriptions/$SUB_ID/resourceGroups/$RG --sdk-auth