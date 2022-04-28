# About this page
This page is about the backup of data stored in SQL on Azure

# Backup手段
- SQL Server on IaaSの場合
 - Decentralized management: SQL Serverのバックアップをユーザ部署に任せたい → SQL Serverの機能: https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases
  - Azureにバックアップしたい場合
   - Backup To URL: https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/sql-server-backup-to-url
   - Backup to Azure Blob Storage: https://docs.microsoft.com/sql/relational-databases/backup-restore/back-up-multiple-databases-to-azure-blob-storage-powershell?view=sql-server-ver15
  - On-premisesにバックアップしたい場合 → https://docs.microsoft.com/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server
 - Centralized management: SQL ServerのバックアップをIT部門等一か所で一元で管理したい → Azure Backup経由: https://docs.microsoft.com/azure/backup/backup-azure-sql-database
  - Azure Portalで: https://docs.microsoft.com/azure/backup/backup-azure-sql-database
  - REST APIで: https://docs.microsoft.com/azure/backup/backup-azure-sql-vm-rest-api
- SQL Server on Azure以外の場合 や SQL以外のワークロードも一緒にバックアップしたい場合 → Microsoft Azure Backup Server (MABS)を活用: https://docs.microsoft.com/en-us/azure/backup/backup-azure-sql-mabs
Azure SQL Single DatabaseやAzure SQL Managed Instanceの場合
 - → 自動的にバックアップ: https://docs.microsoft.com/azure/azure-sql/database/automated-backups-overview)
- データだけバックアップしたい場合: https://docs.microsoft.com/ja-jp/azure/azure-sql/database/database-export

# Downtime
## Backup
Online backupなので、backupはダウンタイムは発生しない

## Restore
Restoreに関しては、復元にかかる時間（例: リソースのプロビジョニングとデータの書き込み）はあるが、Geoレプリケーションの様に事前にレプリケーションがされていれば、復元のダウンタイムは、切り替え時に発生するかもしれない瞬断だけということですね。
・SQLの置き場所/構成に依存する
　・Azure SQLやMIの場合は、Geoレプリケーションをしていて、Failover Groupを組んでいるか
　・SQL on Azure VMやSQL on-premisesの場合は、自分でそういう構成を組んでいるか
・事前にレプリケーションをしていない場合は、復元にかかる時間は復元先のリソース（例: HyperScaleだと早いイメージ）やDBサイズに依存
https://docs.microsoft.com/en-us/azure/azure-sql/database/auto-failover-group-sql-db