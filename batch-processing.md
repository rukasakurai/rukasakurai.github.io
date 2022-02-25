「バッチ処理をどうやってAzureで実現するか」

バッチ処理とは
バッチ処理とは、複数のものの集合（バッチ）を処理すること。「バッチ」が何かについては、どういう単位で考えるかによって、異なって来る。
例えば、複数の「申請」や「発注」を特定に時間に処理することを「バッチ処理」と呼ぶことがある。この場合は、指定した時間が到達したことがトリガーになり、複数の「申請」や「発注」に対しての処理が行われる。
特定の時間をトリガーとする処理を「バッチ処理」と呼ぶことは多いが、時間到達以外のイベントをトリガー（人による操作やHTTP Request等）に複数のもの（バッチ）に対して処理が行われた場合も、バッチ処理と言える。

時間トリガー
Azureでは時間をトリガーとする処理を設定する方法が複数ある。
例えば、
・Azure Functions: https://docs.microsoft.com/en-us/azure/azure-functions/functions-bindings-timer?tabs=csharp
・Azure DevOps: https://docs.microsoft.com/en-us/azure/devops/pipelines/process/scheduled-triggers?view=azure-devops&tabs=yaml
・Azure Logic Apps: https://docs.microsoft.com/en-us/azure/logic-apps/concepts-schedule-automated-recurring-tasks-workflows
・Azure Batch: https://docs.microsoft.com/en-us/azure/batch/batch-job-schedule
・Azure Automation: https://docs.microsoft.com/en-us/azure/automation/shared-resources/schedules
・Azure Service Bus: https://docs.microsoft.com/en-us/azure/service-bus-messaging/message-sequencing#scheduled-messages
・Azure Databricks: https://docs.microsoft.com/en-us/azure/databricks/jobs#job-schedule
・Azure Data Lake Analytics: https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-schedule-jobs-ssis
・Azure WebJob: https://docs.microsoft.com/en-us/azure/app-service/webjobs-create#CreateScheduledCRON

また、Azure Kubernetes Serviceの様にそのサービス自体に時間トリガーがなかったとしても、Azure Functions等のタイマートリガーを活用して、処理はAKSで実行するなど、トリガーと処理を別のサービスで実装することもでる。

「Azure バッチ処理」「Azure batch processing」で検索をすると以下が出て来る
・Azure Batch: https://docs.microsoft.com/en-us/azure/batch/batch-technical-overview
・Data系: https://docs.microsoft.com/ja-jp/azure/architecture/data-guide/technology-choices/batch-processing