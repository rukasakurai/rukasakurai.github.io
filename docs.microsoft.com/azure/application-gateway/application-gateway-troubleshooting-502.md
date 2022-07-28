https://docs.microsoft.com/azure/application-gateway/application-gateway-troubleshooting-502

# Certificate mismatch
"The root certificate of the server certificate used by the backend does not match the trusted root certificate added to the application gateway. Ensure that you add the correct root certificate to whitelist the backend."
https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-backend-health-troubleshooting#trusted-root-certificate-mismatch

---
Fixed the issue by
1) Creating a new certificate with `sudo certbot certonly --preferred-challenges dns -d app-demoproduct.com --manual`
Reference: https://ongkhaiwei.medium.com/generate-lets-encrypt-certificate-with-dns-challenge-and-namecheap-e5999a040708
2) Generated pfx file with `sudo openssl pkcs12 -export -out /etc/letsencrypt/live/app-demoproduct.com/app-demoproduct.pfx -inkey /etc/letsencrypt/live/app-demoproduct.com/privkey.pem -in /etc/letsencrypt/live/app-demoproduct.com/cert.pem`
Reference (Step 5): https://medium.com/@marcmathijssen/add-ssl-to-azure-web-app-using-letsencrypt-9125c3fdfb03
3) Added the certificate to Key Vault
4) Created a new managed identity id-DemoProduct for Application Gateway to use to access the certificate in Key Vault, and added the appropriate RBAC role
Reference: https://docs.microsoft.com/en-us/azure/application-gateway/key-vault-certs#obtain-a-user-assigned-managed-identity
5) Updated the certificate on the listener of the Application Gateway to read from Key Vault
6) Updated the Backend setting of Application Gateway to "User well known CA certificate". Not sure why, but was using my own certificate before.

---
temp memo
https://qiita.com/Isato-Hiyama/items/90bd4f33e42385a9db1e

https://ongkhaiwei.medium.com/generate-lets-encrypt-certificate-with-dns-challenge-and-namecheap-e5999a040708

sudo openssl pkcs12 -export -out /etc/letsencrypt/live/app-demoproduct.com/app-demoproduct.pfx -inkey /et c/letsencrypt/live/app-demoproduct.com/privkey.pem -in /etc/letsencrypt/live/app-demoproduct.com/cert.pem


