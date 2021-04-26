# TLS (Transport Layer Security)
## About this page
Everything related to TLS for now. May organize it differently later.

# Q&A
Q: When making an TLS connection with the name given by default in App Service (e.g., *.azurewebsites.net), how should the certificate be issued and used?
A: 
- Domain names managed by Microsoft (e.g., *.azurewebsites.net) are granted wildcard certificates by default
- If there is a need to procure another TLS certificate, then 
  - TLS certificates can be purchase and use certificates from App Service: https://docs.microsoft.com/azure/app-service/configure-ssl-certificate#import-an-app-service-certificate
  - TLS certificates can be purchased from non-App Service sources, and applied to App Service: https://docs.microsoft.com/azure/app-service/configure-ssl-certificate#import-certificate-into-app-service
- It is common to put App Service behind services such as API Management/Front Door/Application Gateway for features such as authentication and web-application firewall, in which case the TLS certificate can be applied to such services (and if appropriate terminate the TLS protocal there and have HTTP communication with App Service)

Q: How can access to App Service be restricted to specific sources?
A: Ways to restrict access to App Service to specific sources include
- IP address restriction
- ID and password authentication
- [Configure TLS mutual authentication (a.k.a. client certificate authentication) for Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-web-configure-tls-mutual-auth)
- Other

Q: How can access to Azure API Management be restricted to specific sources?
A: Ways to restrict access to App Service to specific sources include
- [VNet integration](https://docs.microsoft.com/ja-jp/azure/api-management/api-management-using-with-vnet)
- [Use subscription keys issued by API Management](https://docs.microsoft.com/ja-jp/azure/api-management/api-management-howto-create-subscriptions)
- [Configure API Management to use client certificates](https://docs.microsoft.com/ja-jp/azure/api-management/api-management-howto-mutual-certificates-for-clients)

Q: How does one apply a TLS client certificate to App Service?
A: Can't currently find App Service specific documentation. [This ASP.NET documentation](https://docs.microsoft.com/aspnet/core/security/authentication/certauth), might be relevant for C#/ASP.NET

