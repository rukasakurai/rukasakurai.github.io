https://docs.microsoft.com/en-us/azure/active-directory/manage-apps/what-is-single-sign-on

# prompt=none
It ensures that the user isn't presented with any interactive prompt. If the request can't be completed silently by using single-sign on, the Microsoft identity platform returns an interaction_required error.
https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-auth-code-flow

# PRT(Primary Refresh Token)
A Primary Refresh Token (PRT) is a key artifact of Azure AD authentication on Windows 10 or newer, Windows Server 2016 and later versions, iOS, and Android devices. It is a JSON Web Token (JWT) specially issued to Microsoft first party token brokers to enable single sign-on (SSO) across the applications used on those device
https://docs.microsoft.com/en-us/azure/active-directory/devices/concept-primary-refresh-token

# Azure Hybrid AD Join
https://docs.microsoft.com/en-us/azure/active-directory/devices/concept-azure-ad-join-hybrid

# 気になること
Scenario:
- User wants to be able to access web applications by singing in only once (signing into the Windows PC)
- User is using a Windows PC that is Azure Hybrid AD Joined
- Separate Azure AD tenant for O365 and for Azure resources such as App Service

Question:
If the first time the user signs in to the PC is from home, instead of from inside the company network, what happens?
When the user accesses the web app, the app will redirect the user to Azure AD?
How will the user's device get a primary refresh token? Won't the user's device need to be connected to the company network (e.g., through a VPN)?