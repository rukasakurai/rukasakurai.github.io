# 2022 Feb 24 notes
```
$PRODUCT="custom-font-win-container-master"
$RG="rg-$PRODUCT"
az group create --location japaneast --name $RG

$DEVVM="vm-dev"

az vm create --resource-group $RG --name $DEVVM --image MicrosoftVisualStudio:visualstudio2022:vs-2022-ent-latest-ws2022:2022.02.18 --public-ip-sku Standard --admin-username xxx
```

Resulted in the error below.

```
Deployment failed. Correlation ID: 035cbae2-9ea1-48cc-9797-cc1b064e1ae2. {
  "error": {
    "code": "InvalidParameter",
    "message": "The value of parameter linuxConfiguration is invalid.",
    "target": "linuxConfiguration"
  }
}
```

```
az vm create --resource-group $RG --name $DEVVM --image MicrosoftVisualStudio:visualstudio2022:vs-2022-ent-latest-ws2022:2022.02.18 --public-ip-sku Standard --admin-username xxx --admin-password yyy
```

Was able to create but could not connect with RDP. Opened RDP port in inbound network rules but still could not connect. Didn't bother to debug further.