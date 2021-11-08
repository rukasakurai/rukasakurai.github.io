# References
Roadmap: https://github.com/Azure/Communication/blob/master/roadmap.md
UI Library: https://azure.github.io/communication-ui-library/

# Sample Applications
- Chat: https://github.com/Azure-Samples/communication-services-web-chat-hero
- Calling: https://github.com/Azure-Samples/communication-services-web-calling-hero
- Meeting (Chat+Calling): https://github.com/rukasakurai/chat-calling-sample
- Debugging tool (Calling): https://github.com/Azure-Samples/communication-services-web-calling-tutorial
- Explaining the sample applications: https://microsoft-my.sharepoint.com/:p:/p/rusakura/Edef8d3kveJHoJDGlxm8W8gB0fzMq8pHFxUc71dzffYwZw?e=SvFZEK

# Sample Azure CLI commands for deploying ACS
```
export RESOURCE_GROUP=rg-acsdemo

az group create --location japaneast \
                --name $RESOURCE_GROUP

export ACS_NAME=acs-acsdemo

az communication create --name $ACS_NAME --location "Global" --data-location "Japan" --resource-group $RESOURCE_GROUP

az communication list --resource-group $RESOURCE_GROUP

az communication list-key --name $ACS_NAME --resource-group $RESOURCE_GROUP

export APP_PLAN_NAME=plan-acsdemo
az appservice plan create -g $RESOURCE_GROUP -n $APP_PLAN_NAME --sku F1

export APP_NAME=app-acsdemo
az webapp create -g $RESOURCE_GROUP -p $APP_PLAN_NAME -n $APP_NAME

export APP_CALLING_NAME=app-acscallingdemo
az webapp create -g $RESOURCE_GROUP -p $APP_PLAN_NAME -n $APP_CALLING_NAME
```