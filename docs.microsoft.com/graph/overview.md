# 2022-06-21 and 22 Memo
Tried:
https://docs.microsoft.com/en-us/azure/active-directory/develop/console-app-quickstart?pivots=devlang-dotnet-core

## Run app
```
git clone https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2
cd active-directory-dotnetcore-daemon-v2/1-Call-MSGraph/daemon-console/
dotnet run
```
This should result in an error saying "Invalid URI: The format of the URI could not be determined" since the values in appsettings.json are dummy values.

## Create App Registration etc
$DATE=20220622

az ad app create --display-name ${DATE}temp

$APP_ID=<APP_ID>

az ad app credential reset --id $APP_ID

## Update appsettings.json
Update app settings.json with the results from "az ad app credential reset --id $APP_ID"

`dotnet run`

Should result in an error with message "Failed to call the web API: Forbidden"

## Provide authorization
az ad app permission list --id $APP_ID

az ad sp create --id $APP_ID

az ad sp show --id $APP_ID

$SP=<SP>

az ad app permission grant --id $APP_ID --api $SP --scope Directory.Read.All

# Clean up
## CLI
I tried the below commands on 2022/6/22, but it resulted in an error with the message "does not exist or one of its queried reference-property objects are not present". Not sure if it's a bug or if it's an issue with how I called the commands.
az ad sp delete --id $SP
az ad app delete --id $APP_ID
## Portal
Was able to delete the Service Principal and App Registration through the portal.