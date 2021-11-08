
# Creating my custom VM from a Microsoft managed image
## Create a VM
First create the source application on an Azure VM.
To create a VM: https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-cli
In the Azure CLI, 
```
RESOURCE_GROUP=rg-temp-`date '+%Y%m%d%H%M'`
VM_NAME=vm-app-source

az group create --location japaneast --name $RESOURCE_GROUP

az vm create \
    --resource-group $RESOURCE_GROUP \
    --name $VM_NAME \
    --public-ip-sku Standard \
    --image Win2019Datacenter \
    --admin-username rusakura
```

To access, either
- In PowerShell `mstsc /v:publicIpAddress` (e.g., `mstsc /v:20.89.184.249`) to RDP into the VM
- Or, download the RDP client from the Azure Portal

## Installing applications
In administration mode of Windows PowerShell
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

choco install microsoft-edge
choco install googlechrome

choco install vscode

choco install dotnetcore-sdk
choco install dotnetcore-runtime

```

## Localization
Ref: https://docs.microsoft.com/ja-jp/windows-hardware/manufacture/desktop/localize-windows

### Japananese keyboard
Couldn't figure out how to change the keyboard layout.

Added Japanese language through the GUI, and through PowerShell. I could get the display language and the input language to change, but couldn't successfully change the keyboard layout.

With PowerShell, tried the below
```
powershell -command "Set-WinUserLanguageList -Force <language>
```

## Enable HTTP access from the Internet
```
az vm open-port --port 80 --resource-group $RESOURCE_GROUP --name vm-app-source
```

Within the VM, `Install-WindowsFeature -name Web-Server -IncludeManagementTools` to install the IIS web server.
Access the VM's IP address from a browser to view the default site.

Download Microsoft Edge using Internet Explorer, and then install it.
Download .NET Core 3.1 using Internet Explorer, and then install it.

## Create new .NET Core website

Add a new web site in IIS
https://docs.microsoft.com/en-us/iis/get-started/getting-started-with-iis/create-a-web-site
Publish an ASP.NET Core app to IIS
https://docs.microsoft.com/en-us/aspnet/core/tutorials/publish-to-iis?view=aspnetcore-5.0&tabs=netcore-cli
Maybe this is relevant too
https://docs.microsoft.com/en-us/iis/application-frameworks/scenario-build-an-aspnet-website-on-iis/configuring-step-1-install-iis-and-asp-net-modules
https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/?view=aspnetcore-5.0#iis-configuration

```
dotnet new mvc -o MvcMovie
cd MvcMovie
dotnet restore MvcMovie.csproj
dotnet build MvcMovie.csproj -c Release -o /app
dotnet publish --configuration Release
```

Set the Physical Path for the IIS site to /app, and the port to something other than 80.

First time:
Accessing the website from the browser did not work at first.
But it worked the next day. I had shutdown the VM, so I rebooted it in the morning. Perhaps the rebooting helped. I had also modified some setting the other day, which might have been applied after rebooting.

Second time:
Accessing the website from the browser did not work.
"HTTP Error 403.14 - Forbidden The Web server is configured to not list the contents of this directory."

Install the .NET Core Hosting Bundle:
```
choco install dotnetcore-runtime
```
and rebooted, but still got the 403.14 error.

```
dotnet new mvc -o MvcMovie
cd MvcMovie
dotnet restore MvcMovie.csproj
dotnet build MvcMovie.csproj -c Release -o /app
dotnet publish --configuration Release  -o /app
```
and then recreated a new site in IIS but getting

HTTP Error 500.19 - Internal Server Error: The requested page cannot be accessed because the related configuration data for the page is invalid.

Tried editting the Application Pool, by setting the .NET CLR version to No Managed Code, but still get the 500.19 error.

Tried rebooting but didn't work.

Tried creating and running in VS code, but got 
このサイトにアクセスできませんlocalhost で接続が拒否されました。

create a shared image that has edge and .net and vs code and maybe visual studio

I had to disable the "IE Enablses Seduciryt configuration throuhg Server Manager
(Install Edge, VS Code)
Install Dotnet
Enable IIS

Installed this. did I have to? https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer
