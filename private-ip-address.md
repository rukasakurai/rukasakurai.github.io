
# What is the difference between Service Endpoints and Private Endpoints?
The [official documentation](https://docs.microsoft.com/en-us/azure/private-link/private-link-faq#what-is-the-difference-between-service-endpoints-and-private-endpoints) says:
>- Private Endpoints grant network access to specific resources behind a given service providing granular segmentation. Traffic can reach the service resource from on premises without using public endpoints.
>- A service endpoint remains a publicly routable IP address. A private endpoint is a private IP in the address space of the virtual network where the private endpoint is configured.

[This article by Becki Lee from Fugue from October 8 2020](https://www.fugue.co/blog/cloud-network-security-101-azure-service-endpoints-vs.-private-endpoints) analyses the difference pretty clearly, and has a nice summary of the difference in implementation:
> With service endpoints, you're still connecting to the target resource's public endpoint. This effectively extends the identity of the VNet to the target resource. With private endpoints, you're assigning the target resource a private IP address from the VNet, essentially bringing it into the network

[This article by Ashish Patel from Nov 1 2020](https://medium.com/awesome-azure/azure-difference-between-azure-private-links-and-azure-service-endpoints-private-links-vs-service-endpoints-8fb0f80ca196) analyzes the differences quite nicely too.

# History of General Availability announcents
- Virtual Network Services Endpoints: 2018 January [GA announcement](https://azure.microsoft.com/updates/general-availability-vnet-service-endpoints-and-firewalls-for-azure-storage/)
- Private Link: 2020 February [GA announcement](https://azure.microsoft.com/updates/private-link-now-available-in-ga/) 
- Private Link support for additional services: 2020 March [GA announcement](https://azure.microsoft.com/updates/privatelinkforpaasga/)
- Private Endpints for App Service: 2020 October [GA announcement](https://azure.microsoft.com/updates/app-service-private-endpoints-ga/)

# Azure Private Link availability
[docs](https://docs.microsoft.com/en-us/azure/private-link/availability)


[Private IP Addresses in Azure](https://docs.microsoft.com/azure/virtual-network/private-ip-addresses) allow communication between resources in Azure.

https://qiita.com/nakazax/items/d1229ad3b3ca4bec95f3