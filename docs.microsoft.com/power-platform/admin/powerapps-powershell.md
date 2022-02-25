# Power Appsの所有者変更
Power Appsで作成したアプリの所有者の移行方法
- 現状(2022/1/6)、共有のGUIからアプリの共同所有者を追加することはできますが、GUIから所有者を変更することはできません
- 管理者権限（グローバル管理者、Azure Active Directory グローバル管理者、Dynamics 365 管理者、Power Platform管理者など）を持っているユーザがPowerShellからSet-AdminPowerAppOwnerを実行すれば、変更可能可能です。ただし、所有者自身であっても管理者権限が付与されておりませんと所有者を変更できません
- Power Automateと同様、アプリをExportしてImportすることで、所有者を変更することができます

## Set-AdminPowerAppOwner
1. 必要なモジュールのインストール
https://docs.microsoft.com/en-us/power-platform/admin/powerapps-powershell#powerapps-cmdlets-for-app-creators-preview

Set-ExecutionPolicyが必要な場合も
https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy

3. コマンド実行
https://www.youtube.com/watch?v=NsYozi3tc1s

2022/1/6にGlobal Admin権限のある環境で実行して、できることを確認した

## Power Apps 所有者と作成者
所有者と作成者ば別の概念。なぜ作成者を知る必要があるかは不明。所有者が分かれば良いのではないかと個人的には思う。