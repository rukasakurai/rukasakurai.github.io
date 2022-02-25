https://docs.microsoft.com/en-us/azure/app-service/tutorial-custom-container

# 2022 Feb 25 notes
Tried both the Linux as well as the Windows version of the tutorial
- Linux version: Went smoothly except at the end when trying to connect to the container using SSH. Got a page not found when trying to access the webssh/host endpoint
- Windows version: Faced several issues and wasn't able to complete the tutorial
  - The containers were so large that it took a long time to pull and push the images
  - Visual Studio kept asking me to login to Azure, every minute or so, when setting up the 'Publish' settings
  - Publish failed do the error details below

# Linux container
https://docs.microsoft.com/en-us/azure/app-service/tutorial-custom-container?pivots=container-linux
## 2022 Feb 24 and 25 notes
```
PRODUCT=tutorial-custom-container
RG="rg-$PRODUCT"
az group create --location japaneast --name $RG
ACR="crtutorialcustomcontainer"
az acr create --name $ACR --resource-group $RG --sku Basic --admin-enabled true

az acr credential show --resource-group $RG --name $ACR

docker login $ACR.azurecr.io --username crtutorialcustomcontainer

docker tag appsvc-tutorial-custom-image $ACR.azurecr.io/appsvc-tutorial-custom-image:latest

docker push $ACR.azurecr.io/appsvc-tutorial-custom-image:latest

az acr repository list -n $ACR

PLAN=plan-$PRODUCT
az appservice plan create --name $PLAN --resource-group $RG --is-linux

APP=app-$PRODUCT
az webapp create --resource-group $RG --plan $PLAN --name $APP --deployment-container-image-name $ACR.azurecr.io/appsvc-tutorial-custom-image:latest

az webapp config appsettings set --resource-group $RG --name $APP --settings WEBSITES_PORT=8000

APP_ID=`az webapp identity assign --resource-group $RG --name $APP --query principalId --output tsv`
SUBS_ID=`az account show --query id --output tsv`

az role assignment create --assignee $APP_ID --scope /subscriptions/$SUBS_ID/resourceGroups/$RG/providers/Microsoft.ContainerRegistry/registries/$ACR --role "AcrPull"

az resource update --ids /subscriptions/$SUBS_ID/resourceGroups/$RG/providers/Microsoft.Web/sites/$APP/config/web --set properties.acrUseManagedIdentityCreds=True

az webapp config container set --name $APP --resource-group $RG --docker-custom-image-name $ACR.azurecr.io/appsvc-tutorial-custom-image:latest --docker-registry-server-url https://$ACR.azurecr.io

echo https://$APP.azurewebsites.net

az webapp log config --name $APP --resource-group $RG --docker-container-logging filesystem

az webapp log tail --name $APP --resource-group $RG

CI_CD_URL=`az webapp deployment container config --enable-cd true --name $APP --resource-group $RG --query CI_CD_URL --output tsv`

az acr webhook create --name appserviceCD --registry $ACR --uri $CI_CD_URL --actions push --scope appsvc-tutorial-custom-image:latest

eventId=$(az acr webhook ping --name appserviceCD --registry $ACR --query id --output tsv)
az acr webhook list-events --name appserviceCD --registry $ACR --query "[?id=='$eventId'].eventResponseMessage"
```
# Windows container
https://docs.microsoft.com/en-us/azure/app-service/tutorial-custom-container?pivots=container-windows

## 2022 Feb notes
- Was not able to complete the tutorial
- Followed guidance, expect using Visual Studio 2022 instead of 2019
- Downloaded the specified zip file, and uploaded the contents onto GitHub: https://github.com/rukasakurai/custom-font-win-container-master

```
2/24/2022 9:29:01 AM
System.NullReferenceException: Object reference not set to an instance of an object.
at Microsoft.WebTools.Azure.Publish.AppServiceLinux.AzureContainerRegistryCredentials.<GetAdminCredentialsAsync>d__5.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
at System.Runtime.ExceptionServices.ExceptionDispatchInfo.Throw()
at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
```