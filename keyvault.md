# HTTP Error 500.30 - ANCM In-Process Start Failure
## Error displayed by browser
HTTP Error 500.30 - ANCM In-Process Start Failure
Common solutions to this issue:
The application failed to start
The application started but then stopped
The application started but threw an exception during startup
Troubleshooting steps:
Check the system event log for error messages
Enable logging the application process' stdout messages
Attach a debugger to the application process and inspect
For more information visit: https://go.microsoft.com/fwlink/?LinkID=2028265

## Solution?
az webapp identity assign \
    --resource-group learn-4c43e2ee-b18b-472d-bfd3-d8cda1a7005b \
    --name <your-unique-app-name>

## Learn notes
Got the same error as above after following the learn instructions. Did it twice and got same results.

az keyvault create \
    --resource-group learn-a6f2c626-6277-48a3-be0e-9ea2d82a606b \
    --location centralus \
    --name kv-20210702-ruka

az keyvault secret set \
    --name SecretPassword \
    --value reindeer_flotilla \
    --vault-name kv-20210702-ruka

az appservice plan create \
    --name keyvault-exercise-plan \
    --sku FREE \
    --location centralus \
    --resource-group learn-a6f2c626-6277-48a3-be0e-9ea2d82a606b

az webapp create \
    --plan keyvault-exercise-plan \
    --resource-group learn-a6f2c626-6277-48a3-be0e-9ea2d82a606b \
    --name app-20210702-ruka


az webapp config appsettings set \
    --resource-group learn-a6f2c626-6277-48a3-be0e-9ea2d82a606b \
    --name app-20210702-ruka \
    --settings 'VaultName=kv-20210702-ruka'

az webapp identity assign \
    --resource-group learn-a6f2c626-6277-48a3-be0e-9ea2d82a606b \
    --name app-20210702-ruka

{
  "principalId": "3bda48c1-958c-4f83-a6c9-2f7b957a5b35",
  "tenantId": "604c1504-c6a3-4080-81aa-b33091104187",
  "type": "SystemAssigned",
  "userAssignedIdentities": null
}

az keyvault set-policy \
    --secret-permissions get list \
    --name kv-20210702-ruka \
    --object-id 3bda48c1-958c-4f83-a6c9-2f7b957a5b35

dotnet publish -o pub
zip -j site.zip pub/*

az webapp deployment source config-zip \
    --src site.zip \
    --resource-group learn-a6f2c626-6277-48a3-be0e-9ea2d82a606b \
    --name app-20210702-ruka

https://app-20210702-ruka.azurewebsites.net/api/SecretTest

------------------

az keyvault set-policy \
    --secret-permissions get list \
    --name kv-demoproduct-release \
    --object-id 27509ccc-246a-4e4c-b47e-d927d3ee2621