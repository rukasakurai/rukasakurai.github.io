# Notes for doing tutorial on March 15th, 2022
RG="rg-20220315temp"
az group create -g $RG -l japaneast

az backup vault create --resource-group $RG \
    --name myRecoveryServicesVault \
    --location japaneast

Took about 30 seconds or less to create.

az backup vault backup-properties set \
    --name myRecoveryServicesVault  \
    --resource-group $RG \
    --backup-storage-redundancy "LocallyRedundant"

Took about 2-3 seconds to create.

az vm create \
    --resource-group $RG \
    --name myVM \
    --image Win2019Datacenter \
    --public-ip-sku Standard \
    --admin-username azureuser

Took about 1 min to create.
Added a test file on the desktop.

az backup protection enable-for-vm \
    --resource-group $RG \
    --vault-name myRecoveryServicesVault \
    --vm myVM \
    --policy-name DefaultPolicy

Took about 50 seconds to create.

az backup protection backup-now \
    --resource-group $RG \
    --vault-name myRecoveryServicesVault \
    --container-name myVM \
    --item-name myVM \
    --backup-management-type AzureIaaSVM \
    --retain-until 16-03-2022

Took less than 5 seconds.

az backup job list \
    --resource-group $RG \
    --vault-name myRecoveryServicesVault \
    --output table

Displaying the list is immediate.
Took 1 hour and 1 minute for the backup job to complete.
