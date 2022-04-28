# Terminology
## General
DNS zones: Hosts DNS records
DNS record: Maps IP addresses
Private DNS zone: DNS zones that are not open to the public
Public DNS zone: DNS zones that are open to the public
DHCP (Dynamic Host Configuration Protocol): "The Dynamic Host Configuration Protocol is a network management protocol used on Internet Protocol networks for automatically assigning IP addresses and other communication parameters to devices connected to the network using a client–server architecture" -Wikipedia
DNS Zone file: A text file that describes a DNS zone
DNS resource record (DNS record): Each record has name, ttl, class, type, record data

## Azure Private DNS
Assumes a VNet
The default configuration is using Azure DNS (168.63.129.16), or can do custom. 
Azure DNS is not routable, i.e. cannot use from on-premises. But anything within an Azure VNet can talk to Azure DNS. 
The DNS config is at the Azure Virtual Network level. But if one wants to can do a DNS config at the NIC level, although by default the NIC uses the DNS config of the virtual network.
If I use the default setup, then there is a special kind of DNS automatically created for me (for free and created when the VNet is created), called an IDNS (Internal DNS). IDNS is always there whether I use custom DNS or not. Any resource created within the VNet will get a registered with the IDNS and get xxx.internal.cloudapp.net. And this is addressable through the Azure DNS.

If you want to do more, then you create an Azure Private DNS Zone.
With Azure Private DNS Zone, can create a full range or records.
I connect Virtual Networks to it.
Can link a Virtual Network to an Azure Private DNS Zone for "registration".
A Virtual Network can only connect to 1 Azure Private DNS Zone for registration purposes. i.e., Resources created in a VNet can only auto-register to 1 Azure Private DNS Zone.
A Virtual Network can connect to other Azure Private DNS Zones for "resolution". Up to a 1000.
Multiple VNets can use the same Azure Private DNS Zone for registration, up to 100 for registration.
VMs (incl VMSS VMs, AKS VMs, SQL MI VMs) get auto-registered. Every VM in the VNet gets auto-registered (cannot auto-register only a subet of VMs in the VNet).

Azure Private DNS Zones are global. Any region, any Subscription, any VNet, any tenant (as long as have permissions). IDNS is per VNet.

Custom (e.g., Bind, Active Directory Domain Services DNS)
What if I still want to use Azure DNS, e.g., for private link.

Can do forwarding. Or can do conditional forwarding.
If the custom DNS is on an Azure VM (e.g., AD DS DNS), then can forward to Azure DNS (168.63.129.16)
But if the custom DNS is on-prem, then cannot forward to Azure DNS. Then need to set up a DNS resolver within a VN in an Azure Virtual Network. The DNS on-prem would forward to DNS resolver in the VNet, which it will forward to Azure DNS.

John Savill's "Understanding DNS in Azure"
https://www.youtube.com/watch?v=Hiohn35DIqA&t=1299s

Public DNS Zones:
Can manually create records
Will need to set Azure as the authoritive server for the zone. Just creating a zone doesn't mean that the Internet will use it. Need to be the owner of the domain, and go to the registration body and say that the authoritize DNS for this zone is Azure.

"Split brain DNS":
Where the same zone exists in both private and public. For example if you need a public presence, but there are other things that you don't want to expose to the internet.
Private trumps public.

Azure Traffic Manager:
Routes appropriately based on the requesting DNS server.

Is DNS managed as code?
https://blog.apnic.net/2019/02/04/how-to-managing-dns-zones-using-git/

Azure Private DNSに条件付きフォワーダーはない?
Azure Firewallにフォワーダー機能はある？条件付きはない？


