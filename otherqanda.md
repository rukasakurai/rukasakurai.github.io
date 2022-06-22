Q: Power PlatformのログをAzureに連携して管理することができるか
A（2022年5月時点）: Power Platformの監査ログを Microsoft 365セキュリティおよびコンプライアンス センターに送信することができる
https://docs.microsoft.com/power-platform/admin/audit-data-user-activity

Q: Azure Monitor HTTP Data Collector APIはいつGAする予定か？
A（2022年5月時点）: 
GA日について公開情報はありませんが、Azure Monitor HTTP Data Collector APIはGAせずに、2022年2月にPreviewがアナウンスされてたCustom logs API in Azure Monitor LogsがGAする可能性があります。インターフェイスは異なるものの、Azure Monitor HTTP Data Collector APIからCustom logs API in Azure Monitor Logsに移行するドキュメンテーションもあります。
https://docs.microsoft.com/azure/azure-monitor/logs/custom-logs-overview
https://docs.microsoft.com/azure/azure-monitor/logs/custom-logs-migrate

Filed an issue:
https://github.com/Azure/Azure-Sentinel/issues/5143

Q: Azure Blob Storageに格納されたファイル毎に、Power Appsから更新できるユーザを制限したい場合、どう実装すれば良いか。ユーザがPower Apps経由でアップロードしたファイルのみ参照・更新できるように設定する方法
A（2022年5月時点）:
- Azure Blob StorageについているSAS等の機能だけでは、難しいのではないか: https://docs.microsoft.com/en-us/azure/storage/common/authorize-data-access
- 実装方法は複数あるが、例えば以下の実装が可能かもしれいない
 - ユーザ毎にAzure Blob StorageのContainerを分け、Azure FunctionsでどのContainerを見に行くかを制御する
 - Power Appsからは、Azure Functionsを呼び出す
 - Power AppsにAzure ADでログインしたユーザのクレデンシャルをどうやって、Azure Functionsに渡すかということは複数実装方法がある可能性があり、また実装面での課題があるかもしれない