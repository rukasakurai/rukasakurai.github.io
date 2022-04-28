# SQL on Azure
What is a good way to periodically initialize a database use for a demo app?
e.g., APP_DB, BACKUP_DB

Options:
- import
- copy
- restore

# Option: Import
az sql db delete/rename
az sql db import (import bacpac)
(bacpac file is stored in Azure Storage)

az sql db delete --name coreDB --resource-group rg-DemoProduct --server sql-server-ruka 
az storage blob generate-sas --account-name stdemoproduct -c dbbackup -n coreDB-2021-5-12-14-39.bacpac --permissions r  --expiry 2021-05-15T00:00:00Z
az sql db create --name coreDB --resource-group rg-DemoProduct --server sql-server-ruka --edition "GeneralPurpose"
az sql db import -s sql-server-ruka -n coreDB -g rg-DemoProduct -p <password> -u <userame> \
    --storage-key <Blob SAS Token> \
    --storage-key-type SharedAccessKey \
    --storage-uri https://stdemoproduct.blob.core.windows.net/dbbackup/coreDB-2021-5-12-14-39.bacpac

# Option: Restore
az sql db delete/rename
az sql db restore
(Maintain a BACKUP_DB, which needs to be on the same Azure SQL Server as the APP_DB)

# Option: Copy
az sql db delete/rename
az sql db copy
(Maintain a BACKUP_DB, which does not need to be on the same Azure SQL Server as APP_DB)



