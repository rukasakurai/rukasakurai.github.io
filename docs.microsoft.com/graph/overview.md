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

### Debugging
Did not see errors in the Windows 11 laptop I've been using for 2 years and in the Azure Cloud Shell, but got the following error message when running it on a new Windows 11 laptop: `error NU1100: Unable to resolve 'Microsoft.Extensions.Configuration.Binder (>= 6.0.0)' for 'net6.0'`
https://docs.microsoft.com/en-us/nuget/reference/errors-and-warnings/nu1100

Installing Visual Studio Community 2022 with "ASP.NET and web development" and "Azure developement" and error changed to `error NU1101: Unable to find package Microsoft.Extensions.Conf
iguration.Binder. No packages exist with this id in source(s): Microsoft Visual Studio Offline Packages`

The solution was to run the following to specify api.nuget.org as a package source
`dotnet nuget add source https://api.nuget.org/v3/index.json -n nuget.org`
I likely did not have to install Visual Studio Community 2022.

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