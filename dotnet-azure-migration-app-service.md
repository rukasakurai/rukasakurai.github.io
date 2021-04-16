# Migrate your .NET web app or service to Azure App Service
## Microsoft documentation
https://docs.microsoft.com/en-us/dotnet/azure/migration/app-service#com-and-com-components

## Classic ASP
There seems to be annecdotal studies online which say that Classic ASP runs on Windows-based App Services, but I have not been able to find Microsoft official documentation saying that Classic ASP is supported.
Examples of annecdotal studies include
- [Run Classic ASP on Windows 10 and App Service](https://edi.wang/post/2019/7/17/run-classic-asp-on-windows-10-and-azure-app-service)

## COM Components
[Here](https://docs.microsoft.com/dotnet/azure/migration/app-service#com-and-com-components) it says that "Azure App Service does not allow the registration of COM components on the platform. If your app makes use of any COM components, these need to be rewritten in managed code and deployed with the site or application"

### COM
The Component Object Model(COM) was first published in 1993 according to [Wikipedia](https://en.wikipedia.org/wiki/Component_Object_Model), and the Microsoft official documentation for it seems to be managed under the [Windows section](https://docs.microsoft.com/en-us/windows/win32/com/the-component-object-model). "COM specifies an object model and programming requirements that enable COM objects (also called COM components, or sometimes simply objects) to interact with other objects." "COM is referred to as a binary standard; a standard that applies after a program has been translated to binary machine code."

### How to identify whether an app uses COM components
I haven't been able to find a good guide, and with the bits-and-pieces of information that I have found, I think it is likely that it may be difficult to look at a legacy system to determine whether COM components are used. 
- [This article](https://networkencyclopedia.com/com-component/) says that "an in-process COM component has the extension .ocx or .dll, while an out-of-process COM component (one running outside the calling application process) has the extension .exe." This implies that it would be difficult to determine by looking at the file names alone.
- [This documentation](https://docs.microsoft.com/windows/win32/com/component-object-model--com--portal#run-time-requirements) has links to interfaces such as "OLE and Data Transfer", and it might be possible to do a search of these interfaces in the source code of the application, but that would probably take a lot of time, and it might be less time consuming to actually try testing the application code on App Service