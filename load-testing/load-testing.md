https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/


# App Service
## Create Web App
```
PREFIX=20220315temp
RG=rg-$PREFIX
PLAN=plan-$PREFIX
APP=app-$PREFIX

az appservice plan create -g $RG -n $PLAN --sku P1V2 
az webapp create -g $RG -p $PLAN -n $APP
```

## Load Test
```
LOADTESTING=lt-$PREFIX
echo $LOADTESTING
Created Azure Load Testing (preview) resource from Azure Portal

echo $APP.azurewebsites.net
```

アップロードしたTest Plan: [Link](./SampleTest.jmx)

設定したパラメーター
- webapp_url
- num_threads
- duration

