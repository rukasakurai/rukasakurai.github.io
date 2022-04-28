# Referenece Key Vault from App Services
I followed the following steps on 2021 Setp 21, and was able to update my demo application to read from Key Vault for the SQL DB connection string
https://docs.microsoft.com/en-us/azure/app-service/app-service-key-vault-references

What I tested
- Made sure that the application worked before changes
- Delete the connection string from App Service's application settings, and confirmed that the application would fail to access the DB
- Saved the connection string in Key Vault, and referenced it from App Services's application settings, and confirmed that the application could access the DB

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
