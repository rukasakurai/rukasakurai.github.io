# 2022-06-21 Memo
Tried:
https://docs.microsoft.com/en-us/azure/active-directory/develop/console-app-quickstart?pivots=devlang-dotnet-core

az ad app create --display-name 20220621temp

$APP_ID=<APP_ID>

az ad app credential reset --id $APP_ID

az ad app permission list --id $APP_ID

az ad sp create --id $APP_ID

az ad sp show --id $APP_ID

$SP=<SP>

az ad app permission grant --id $APP_ID --api $SP --scope Directory.Read.All
