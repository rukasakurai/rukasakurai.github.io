# Troubleshoot Remote Desktop connections to an Azure virtual machine
docs.microsoft.com has a good [page](https://docs.microsoft.com/en-us/troubleshoot/azure/virtual-machines/detailed-troubleshoot-rdp) about this, but I have found the need for additional guidance when working with enterprise customers

# Other
Enterprise companies may have Group Policies that may restrict RDP. Many times these policies are managed my a person/organization in charge of networking, so employees of these enterprise companies would need to contact such a person/organization.

Some questions to ask oneself before contacting the networking person/organization are
- Have you ruled out the possibility that it is an issue with your settings in Azure
  - Have you checked the Network Security Group settings in Azure?
  - Have you tried accessing the VM through a computer that is not managed by the company?
- Have you tried connecting both through your enterpriese VPN, as well as without it?
- Is there a proxy server managed by your company that you used to connect to the internet? (TODO: Add command to check this)
- Do you have sufficient justification to ask the networking team to change their settings?
- If you are asking for a change in Group Policies, are you aware of what specifically you are asking them to do?
  - For example, do you know what system the Group Policies are managed on (e.g., Azure AD versus AD), or whether the below articles are relevant
    - https://thesysadminchannel.com/how-to-enable-remote-desktop-via-group-policy-gpo/
    - https://docs.microsoft.com/en-us/azure/active-directory-domain-services/manage-group-policy