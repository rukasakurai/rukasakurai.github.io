# 2022 March 1 notes
Tried the following:
https://docs.microsoft.com/en-us/azure/azure-monitor/app/azure-web-apps-net-core

Choices
- To apply the settings to a .NET Core 3.1 application on App Service that I had
- To create a new Application Insights resource in the same resource group and linked to an existing Log Analytics Workspace

Was interested to see if I could see the Application Map, but from the "Application map" UI was directed to the following documentation:
https://docs.microsoft.com/en-us/azure/azure-monitor/app/distributed-tracing#how-to-enable-distributed-tracing

The "Availability" section also showed no data, neither immediately after creating the Application Insight resource, nor a few hours later.

Followed the following instructions, and was able to view the Application map. The Application map showed calls to Azure SQL, login.microsoft.com, and 
https://docs.microsoft.com/en-us/azure/azure-monitor/app/asp-net-core