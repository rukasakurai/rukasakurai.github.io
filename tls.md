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

Q: How does one make a PFX file?
A: 
- One way is `openssl pkcs12 -export -out certificate.pfx -inkey privateKey.key -in certificate.crt -certfile more.crt`
- References:
  - https://www.ssl.com/how-to/create-a-pfx-p12-certificate-file-using-openssl/
  - https://www.secomtrust.net/service/pfw/apply/sr/3_2_AZURE_APPGW.html
  - https://rms.ne.jp/sslserver/csr/csr_azure-cloud-services/
  - https://en.wikipedia.org/wiki/PKCS_12
  - https://au.godaddy.com/help/windows-install-codedriver-signing-certificate-and-create-pfx-file-2698 
- Some TLS certificate provides don't provide instructures for creating PFX files:
  - https://www.cybertrust.co.jp/sureserver/support/technical/iis10.html#

Q: How does one add TLS to a website hosted on Azure App Service using LetsEncrypt?
A: https://medium.com/@marcmathijssen/add-ssl-to-azure-web-app-using-letsencrypt-9125c3fdfb03
- The above blog uses an HTTP challenge, but I found it easier to use a DNS challenge.

