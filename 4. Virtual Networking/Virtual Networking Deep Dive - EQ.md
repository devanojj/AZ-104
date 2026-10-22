## 1. Virtual Networks & Subnets

**How many IP addresses can be assigned to resources in a subnet with a /28 prefix?**
- *11 (correct)*
- 14
- 16

**You need a subnet that will host exactly 25 virtual machines. Which is the smallest subnet prefix that meets the requirement?**
- */27 (correct)*
- /28
- /26

**In the subnet 10.1.2.0/24, which IP address can never be assigned to a virtual machine?**
- 10.1.2.4
- *10.1.2.3 (correct)*
- 10.1.2.254

**What is the smallest IPv4 subnet that Azure supports?**
- /30
- /28
- */29 (correct)*

**A team asks you to create a single virtual network that spans the East US and West Europe regions. What should you tell them?**
- Create the virtual network with zone-redundant subnets so it spans both regions
- Add a GatewaySubnet in each region inside the same virtual network
- *A virtual network is regional; create one per region and connect them with peering (correct)*

**Which address range cannot be used in a virtual network's address space?**
- 10.0.0.0/16
- 172.20.0.0/16
- *169.254.0.0/16 (correct)*

**You change the DNS servers configured on a virtual network, but existing VMs continue to use the old DNS servers. What should you do?**
- *Restart the VMs so they receive the new DNS settings (correct)*
- Re-associate the network security group with the subnet
- Recreate the subnets so they inherit the new setting

**You need to enlarge a subnet that already contains running VMs. Which statement is correct?**
- *The subnet can be resized as long as the new range still contains every IP address in use (correct)*
- All VMs must be deleted before a subnet can be resized
- Subnets cannot be resized after creation

**How many route tables can be associated with a single subnet?**
- *One (correct)*
- Two — one for inbound and one for outbound
- Unlimited, evaluated by priority

**You need to deploy a PaaS service that injects its resources into a dedicated subnet in your virtual network. Which subnet configuration hands the subnet to that service?**
- A service endpoint
- *Subnet delegation (correct)*
- Private endpoint network policies

**What is required for VMs in two different subnets of the same virtual network to communicate?**
- *Nothing — Azure routes traffic between subnets in a virtual network by default (correct)*
- A route table with a route whose next hop is Virtual network
- VNet peering between the two subnets

**VMs in VNet A use Azure-provided DNS and cannot resolve the host names of VMs in peered VNet B. What should you do?**
- Enable "Allow forwarded traffic" on both peering links
- Enable gateway transit on the VNet A peering
- *Create a private DNS zone and link it to both virtual networks (correct)*

---
## 2. IP Addressing

**A VM running as a domain controller must keep the same private IP address permanently. How should you configure this?**
- Set a static IP address in the Windows network adapter settings inside the VM
- Reserve the address by creating a public IP prefix
- *Set the private IP allocation to Static on the NIC's IP configuration in Azure (correct)*

**Which allocation method is available when you create a Standard SKU public IP address?**
- Dynamic or static
- Dynamic only
- *Static only (correct)*

**You attach a Standard SKU public IP address to a VM's NIC. No NSG is associated with the NIC or subnet. RDP connections from the internet fail. Why?**
- Standard public IPs support outbound traffic only
- The routing preference must be set to Internet for inbound traffic
- *Standard public IPs are secure by default and are closed to inbound traffic until an NSG allows it (correct)*

**You are deploying a cross-region load balancer. What type of public IP address must its frontend use?**
- A Standard SKU, regional tier, zone-redundant public IP
- A public IP prefix with one address in each region
- *A Standard SKU public IP with the Global tier (correct)*

**A partner organisation needs to add your outbound public IP addresses to an allow list and requires a predictable, contiguous range. What should you deploy?**
- A NAT gateway with a single public IP
- Several Standard public IPs in the same resource group
- *A public IP prefix (correct)*

**You want outbound traffic from a public IP to leave Microsoft's network as soon as possible to reduce bandwidth costs. What should you configure?**
- The Global tier on the public IP
- A zone-redundant public IP
- *Routing preference set to Internet (correct)*

**Your cost report shows charges for several Standard public IP addresses that are not attached to any resource. What is the explanation?**
- *Standard public IPs are billed whether or not they are attached to a resource (correct)*
- Unattached public IPs are free; the charges are for outbound data transfer
- Charges apply only because the IPs are zone-redundant

**You create a new virtual network using a current API version and deploy a VM without a public IP. The VM cannot reach the internet. What is the most likely cause?**
- *New virtual networks use private subnets by default, so the VM needs an explicit outbound method (correct)*
- The AllowInternetOutBound default NSG rule is missing
- Azure-provided DNS is not configured on the virtual network

**Which explicit outbound connectivity method does Microsoft recommend for a subnet of VMs that need scalable internet access?**
- Default outbound access
- A Standard public IP on every VM
- *NAT gateway (correct)*

**VMs in a busy subnet experience intermittent outbound connection failures under load, and you suspect SNAT port exhaustion. What should you do?**
- Increase the idle timeout on the VMs' public IPs
- *Associate a NAT gateway with the subnet (correct)*
- Add the VMs to an application security group

**What is the maximum number of public IP addresses that can be associated with a single NAT gateway?**
- 8
- *16 (correct)*
- 32

**What is the current status of the Basic SKU public IP address?**
- It is still recommended for development and test workloads
- *It was retired on 30 September 2025; Standard SKU must be used (correct)*
- It is required for VPN gateways

**You mark an existing subnet as private, but a VM in it still egresses using its default outbound IP. What must you do for the change to apply to that VM?**
- *Stop (deallocate) the VM and start it again (correct)*
- Restart the VM from inside the guest operating system
- Delete and recreate the VM's network interface

---
## 3. VNet Peering

**VMs in two virtual networks in different Azure regions must communicate privately with the lowest latency and without deploying gateways. What should you configure?**
- A VNet-to-VNet VPN connection
- ExpressRoute Global Reach
- *Global virtual network peering (correct)*

**VNet A is peered with VNet B, and VNet B is peered with VNet C. VMs in VNet A cannot reach VMs in VNet C. Why?**
- Global peering supports a maximum of two virtual networks
- "Allow gateway transit" must be enabled on VNet B
- *Virtual network peering is non-transitive (correct)*

**A peering shows a status of Initiated. What should you do?**
- Select Sync on the peering
- Delete and recreate the peering on both virtual networks
- *Create the peering link from the remote virtual network (correct)*

**A peering shows a status of Disconnected. What should you do?**
- Select Sync on the remaining peering link
- Enable "Allow access to remote virtual network" on the remaining link
- *Delete the remaining peering link and recreate both links (correct)*

**You try to peer VNet1 (10.0.0.0/16) with VNet2 (10.0.128.0/17). What happens?**
- The peering succeeds and routing uses longest prefix match
- The peering succeeds only if the virtual networks are in different regions
- *The peering fails because the address spaces overlap (correct)*

**You add a new address range to a hub virtual network that is peered with several spokes. VMs in the spokes cannot reach resources in the new range. What should you do?**
- Delete and recreate every peering
- Restart the VMs in the spoke virtual networks
- *Sync the peering for each remote peered virtual network (correct)*

**Can you peer virtual networks that belong to subscriptions in different Microsoft Entra tenants?**
- *Yes, peering is supported across subscriptions and across Microsoft Entra tenants (correct)*
- No, peering requires both virtual networks to be in the same tenant
- Only by using a VNet-to-VNet VPN connection instead of peering

**Spoke virtual networks must use a VPN gateway deployed in the hub virtual network to reach on-premises. Which peering settings are required?**
- *"Allow gateway transit" on the hub peering, and "Use remote gateways" on the spoke peerings (correct)*
- "Use remote gateways" on the hub peering, and "Allow gateway transit" on the spoke peerings
- "Allow forwarded traffic" on both sides of every peering

**You cannot enable "Use remote gateways" on a spoke's peering to the hub. What is the most likely cause?**
- The hub is in a different region from the spoke
- The spoke is in a different subscription from the hub
- *The spoke virtual network already has its own virtual network gateway (correct)*

**Gateway transit is not working, and the hub's VPN gateway is the Basic SKU. What is the cause?**
- Global peering doesn't support gateway transit
- "Allow forwarded traffic" must be disabled for gateway transit
- *The Basic VPN Gateway SKU doesn't support gateway transit (correct)*

**Spoke1 and Spoke2 are peered with a hub that contains an NVA. UDRs on the spoke subnets send traffic to the NVA, and IP forwarding is enabled on the NVA's NIC. Spoke-to-spoke traffic is still dropped. What should you enable?**
- Gateway transit on the hub peerings
- "Use remote gateways" on the spoke peerings
- *"Allow forwarded traffic" on the spoke peerings (correct)*

**You need to move a peered virtual network to a different resource group. What must you do?**
- *Delete the peering, move the virtual network, then recreate the peering (correct)*
- Move it directly; peerings move with the virtual network
- Move it only within the same subscription, which preserves the peering

**You need to temporarily stop traffic over a peering without deleting it. What should you do?**
- Disable "Allow forwarded traffic"
- Remove the route table from the subnets
- *Disable "Allow access to remote virtual network" on the peering (correct)*

**An NSG on a spoke subnet allows inbound traffic from the VirtualNetwork service tag. Does this include traffic from a peered virtual network?**
- No, the VirtualNetwork tag includes only the local address space
- Only when gateway transit is enabled
- *Yes, the VirtualNetwork service tag includes the address spaces of peered virtual networks (correct)*

**Your security policy requires that all traffic between two virtual networks is encrypted with IPsec. Which connectivity option meets the requirement?**
- Global virtual network peering
- Regional virtual network peering with "Allow forwarded traffic"
- *A VNet-to-VNet VPN connection (correct)*

**Can a virtual network have "Use remote gateways" enabled on two different peerings at the same time?**
- Yes, if both remote virtual networks are in the same region
- *No, only one peering per virtual network can use remote gateways (correct)*
- Yes, up to five peerings

---
## 4. Routing & User-Defined Routes

**What is the next hop type of the default system route for 0.0.0.0/0?**
- None
- *Internet (correct)*
- Virtual network gateway

**A VM in a virtual network with address space 10.0.0.0/16 sends traffic to 172.16.5.4. There are no route tables, peerings or gateways. What happens to the traffic?**
- *It is dropped, because the default system route for 172.16.0.0/12 has a next hop of None (correct)*
- It is sent to the internet
- It is routed within the virtual network

**All internet-bound traffic from workload subnets must pass through an Azure Firewall with the private IP 10.0.1.4. What should you configure?**
- An NSG outbound rule that allows traffic only to 10.0.1.4
- *A UDR for 0.0.0.0/0 with next hop type Virtual appliance and next hop address 10.0.1.4 (correct)*
- A UDR for 0.0.0.0/0 with next hop type Internet and next hop address 10.0.1.4

**A subnet's effective routes include a UDR for 10.0.0.0/16 to a virtual appliance and a system route for 10.0.0.0/24 to Virtual network. Which route is used for traffic to 10.0.0.25?**
- *The 10.0.0.0/24 route, because longest prefix match is evaluated first (correct)*
- The UDR, because user-defined routes always override system routes
- Both, using equal-cost multipath

**A UDR, a BGP route and a system route all have exactly the same address prefix. Which is used?**
- The system route
- *The user-defined route (correct)*
- The BGP route

**Traffic is correctly routed to an NVA by a UDR, but the NVA drops packets that aren't addressed to it. What should you configure?**
- Accelerated networking on the NVA's NIC
- A public IP address on the NVA
- *IP forwarding on the NVA's NIC in Azure (correct)*

**Which UDR next hop type causes matching traffic to be dropped?**
- Internet
- *None (correct)*
- Virtual network

**A route table in West Europe cannot be associated with a subnet in an East US virtual network. Why?**
- *A route table must be in the same region and subscription as the virtual network (correct)*
- The virtual networks must be peered first
- Gateway route propagation must be enabled

**By default, what is the maximum number of user-defined routes in a single route table?**
- 100
- *400 (correct)*
- 1,000

**Workload subnets are learning on-premises routes from the VPN gateway through BGP, which lets traffic bypass the hub firewall. What should you do?**
- *Disable "Propagate gateway routes" on the route table associated with the workload subnets (correct)*
- Disable gateway route propagation on the GatewaySubnet route table
- Add a route for 0.0.0.0/0 with next hop None

**On which subnet should you never disable gateway route propagation?**
- AzureBastionSubnet
- *GatewaySubnet (correct)*
- AzureFirewallSubnet

**Which next hop type can you NOT select when creating a user-defined route?**
- *VirtualNetworkServiceEndpoint (correct)*
- Virtual appliance
- Virtual network gateway

**All internet-bound traffic from Azure VMs must be sent to on-premises for inspection (forced tunnelling). Which UDR achieves this?**
- 0.0.0.0/0 with next hop type Internet
- 0.0.0.0/0 with next hop type None
- *0.0.0.0/0 with next hop type Virtual network gateway (correct)*

**A UDR sends 10.0.0.0/8 to a firewall, but traffic to a private endpoint in the same virtual network bypasses the firewall. Why?**
- *The private endpoint adds a more specific /32 route, which wins on longest prefix match (correct)*
- Service endpoint routes can't be overridden
- UDRs don't apply to traffic within the same virtual network

**For outbound traffic from a subnet that has both an NSG and a route table, which is processed first?**
- *The NSG outbound rules, then the route table (correct)*
- The route table, then the NSG
- Both are evaluated simultaneously and the most restrictive wins

**You deploy two NVAs for high availability. What should UDRs use as the next hop address?**
- The public IP of the active NVA
- *The frontend IP of an internal Standard Load Balancer with HA ports in front of the NVAs (correct)*
- The virtual network gateway

---
## 5. NSGs & ASGs

**What is the valid range for a custom NSG rule priority?**
- 1–65535
- 100–65000
- *100–4096 (correct)*

**An NSG has a rule at priority 100 that denies all inbound traffic, and a rule at priority 200 that allows inbound TCP 443. What happens to inbound HTTPS traffic?**
- It is allowed, because the 443 rule is more specific
- *It is denied, because priority 100 is processed first and processing stops at the first match (correct)*
- It is allowed, because allow rules take precedence over deny rules

**A new NSG containing only the default rules is associated with a subnet. Which traffic is blocked?**
- *Inbound traffic from the internet (correct)*
- Traffic between VMs in the same virtual network
- Outbound traffic to the internet

**NSGs are associated with both a subnet and a VM's NIC. In what order are they evaluated for inbound traffic?**
- The NIC NSG, then the subnet NSG
- *The subnet NSG, then the NIC NSG (correct)*
- Only the NIC NSG, because it is more specific

**NSGs are associated with both a subnet and a VM's NIC. In what order are they evaluated for outbound traffic?**
- The subnet NSG, then the NIC NSG
- *The NIC NSG, then the subnet NSG (correct)*
- Only the subnet NSG

**The subnet NSG allows inbound TCP 3389 from your admin IP at priority 100. The VM's NIC has an NSG with only the default rules. Can you RDP to the VM?**
- *No, the NIC NSG blocks it with DenyAllInBound (correct)*
- Yes, because the subnet NSG allowed the traffic
- Yes, because AllowVNetInBound permits it

**How many NSGs can be associated with a single network interface?**
- Two
- Unlimited
- *One (correct)*

**An inbound NSG rule allows TCP 443 to a web server. Do you need an outbound rule to allow the responses?**
- *No, NSGs are stateful, so return traffic is allowed automatically (correct)*
- Yes, an outbound rule for the ephemeral ports is required
- Only for UDP traffic

**You need an NSG rule that allows only Azure Load Balancer health probes. Which source should you use?**
- VirtualNetwork
- Internet
- *The AzureLoadBalancer service tag (correct)*

**A VM has a public IP address. When you write an inbound NSG rule for traffic to it, what destination should you use?**
- The VM's public IP address
- *The VM's private IP address, because NSGs evaluate inbound traffic after public-to-private translation (correct)*
- Either address; NSGs evaluate both

**You try to create a single NSG rule with both the Storage and Sql service tags in the destination field. What happens?**
- *It isn't supported; a rule field can contain only one service tag (correct)*
- It succeeds as an augmented security rule
- It succeeds only for outbound rules

**You try to add NICs from VMs in two different virtual networks to the same application security group, and it fails. Why?**
- *All NICs in an application security group must be in the same virtual network (correct)*
- ASGs can contain a maximum of two NICs
- ASGs are scoped to a single subnet

**What is an application security group associated with?**
- Subnets
- *Network interfaces (correct)*
- Virtual networks

**What is the maximum number of application security groups that can be referenced in the source of a single NSG rule?**
- 1
- *10 (correct)*
- 100

**You need to see the combined NSG rules from both the subnet and the NIC that apply to a VM. What should you use?**
- Network Watcher next hop
- *Effective security rules on the VM's network interface (correct)*
- Network Watcher topology

**Effective security rules for a VM's NIC are unavailable in the portal. What is the most likely reason?**
- Network Watcher is in a different resource group
- The NSG is associated with the subnet rather than the NIC
- *The VM isn't running (correct)*

**You need to block all traffic between two VMs in the same subnet. What should you do?**
- Add a UDR with next hop None for the subnet range
- This isn't possible, because traffic within a subnet bypasses NSGs
- *Add NSG deny rules with priority numbers lower than 65000 (correct)*

**You cannot associate an NSG in East US with a subnet in West US. Why?**
- *An NSG must be in the same region as the subnet or NIC it's associated with (correct)*
- The NSG must be in the same resource group as the virtual network
- NSGs can only be associated with NICs across regions

**After you add a "deny all inbound" rule at priority 4000, every backend VM behind a load balancer is marked unhealthy. What should you add?**
- *An allow rule from the AzureLoadBalancer service tag on the probe port with a priority number lower than 4000 (correct)*
- An allow rule from the Internet service tag at priority 4001
- An allow rule from VirtualNetwork at priority 65000

---
## 6. Azure Bastion

**Developers need free, browser-based access to one test VM at a time. Which Bastion SKU should you use?**
- *Developer (correct)*
- Basic
- Standard

**One Bastion host in a hub virtual network must provide access to VMs in peered spoke virtual networks, at the lowest cost. Which SKU should you choose?**
- *Basic (correct)*
- Developer
- Premium

**Administrators want to connect to VMs through Bastion using the native RDP client on their own computers. What is the minimum SKU?**
- *Standard (correct)*
- Developer
- Basic

**Compliance requires that all Bastion RDP and SSH sessions are recorded. Which SKU is required?**
- *Premium (correct)*
- Standard
- Basic

**Your security team does not allow a public IP address on the Bastion host. What should you deploy?**
- Bastion Standard with IP-based connection
- *Bastion Premium with private-only deployment (correct)*
- Bastion Basic with a NAT gateway

**What are the subnet requirements for a Basic, Standard or Premium Bastion deployment?**
- A subnet named BastionSubnet with a /27 or larger prefix
- *A subnet named AzureBastionSubnet with a /26 or larger prefix (correct)*
- The GatewaySubnet with a /26 or larger prefix

**You deployed Bastion Standard and now want to downgrade to Basic to save money. What should you do?**
- *Delete the Bastion host and redeploy it with the Basic SKU, as downgrading isn't supported (correct)*
- Change the SKU in the portal's Configuration blade
- Change the SKU with the Azure CLI, as the portal blocks downgrades

**Which port and protocol do users' browsers use to connect to Azure Bastion?**
- *HTTPS over TCP 443 (correct)*
- RDP over TCP 3389
- SSH over TCP 22

**Do VMs accessed through Azure Bastion require public IP addresses?**
- *No, Bastion connects to the VMs' private IP addresses (correct)*
- Yes, Bastion forwards to the VM's public IP
- Only Linux VMs accessed by SSH

**You want to add a route table to AzureBastionSubnet so that Bastion traffic passes through a firewall. What is the result?**
- It's required for Bastion to reach peered virtual networks
- *It isn't supported; UDRs can't be applied to AzureBastionSubnet (correct)*
- It's supported only with the Premium SKU

**Administrators need to use Bastion to reach an on-premises server over a site-to-site VPN by entering its IP address. Which SKUs support this?**
- Developer and Basic
- Basic only, through virtual network peering
- *Standard and Premium, using IP-based connection (correct)*

**A contractor without Azure portal access needs temporary RDP access to one VM through Bastion. Which feature should you use, and what is the minimum SKU?**
- Kerberos authentication, Basic SKU
- Native client support, Developer SKU
- *A shareable link, Standard SKU (correct)*

**Your organisation needs about 300 concurrent RDP sessions through Bastion. Which SKUs can support this?**
- Basic, which has two fixed instances
- *Standard or Premium, with host scaling (correct)*
- Developer, which uses a shared pool

**What must the NSG on a target VM's subnet allow for Bastion connections to work?**
- *Inbound TCP 3389/22 from the AzureBastionSubnet address range (correct)*
- Inbound TCP 443 from the Internet service tag
- Inbound TCP 3389 from the Internet service tag

---
## 7. Service Endpoints vs Private Endpoints

**Only VMs in a specific subnet should be able to reach a storage account. There must be no extra cost and no on-premises access is required. What should you configure?**
- *A service endpoint on the subnet and a virtual network rule on the storage account (correct)*
- A private endpoint for the blob sub-resource
- Azure Bastion with an NSG

**On-premises servers connected through ExpressRoute must reach an Azure SQL database over a private IP address. What should you configure?**
- A Microsoft.Sql service endpoint on the GatewaySubnet
- *A private endpoint for the SQL server (correct)*
- A service endpoint policy

**A subnet uses a storage service endpoint. You must stop VMs in the subnet from copying data to storage accounts outside your organisation. What should you configure?**
- *A service endpoint policy that allows only your storage accounts (correct)*
- An NSG outbound rule using the Storage service tag
- A private DNS zone for privatelink.blob.core.windows.net

**A VM in a subnet with a storage service endpoint resolves the storage account's DNS name. What does it resolve to?**
- *The storage account's public IP address (correct)*
- A private IP address from the subnet
- 168.63.129.16

**You enable a Microsoft.Storage service endpoint on a subnet, but VMs are still denied by the storage account firewall. What should you do?**
- *Add a virtual network rule for that subnet in the storage account's networking settings (correct)*
- Restart the storage account
- Create a private endpoint as well

**What happens when you enable a service endpoint on a subnet that has running workloads?**
- The VMs in the subnet restart
- The DNS servers for the subnet change to private resolvers
- *Existing TCP connections from the subnet to that service are reset (correct)*

**You enable a Microsoft.Sql service endpoint on a subnet. Where must the Azure SQL server be located?**
- In any region
- *In the same region as the virtual network (correct)*
- In the paired region of the virtual network

**What is created in your virtual network when you deploy a private endpoint?**
- A route table with a route to the service
- A NAT gateway for the service
- *A network interface with a private IP address from the subnet (correct)*

**Applications must reach both blob and file services on one storage account privately. How many private endpoints do you need?**
- One, because a private endpoint covers the whole account
- *Two, one for each sub-resource (correct)*
- One, provided the hierarchical namespace is enabled

**After creating a private endpoint, VMs still connect to the service's public IP. What should you fix?**
- *Link the matching privatelink private DNS zone to the virtual network (correct)*
- Add a service endpoint on the subnet
- Enable "Allow trusted Microsoft services"

**NSG rules on the subnet that contains a private endpoint are not filtering traffic to it. What should you do?**
- Associate the NSG directly with the private endpoint's NIC
- Add the private endpoint to an application security group
- *Enable private endpoint network policies for NSGs on the subnet (correct)*

**A user who has no permissions on the target resource creates a private endpoint to it. What is the connection's status?**
- Approved
- *Pending, until the resource owner approves it (correct)*
- Rejected

**You create an approved private endpoint for a storage account. Is the account's public endpoint automatically disabled?**
- *No, you must restrict public network access separately (correct)*
- Yes, for all Azure PaaS services
- Yes, but only for storage accounts

**You want to let customers in other Microsoft Entra tenants reach your application, which runs behind a Standard internal load balancer, through private endpoints in their own virtual networks. What should you create?**
- A service endpoint
- A virtual network peering to each customer
- *A Private Link service (correct)*

**A subnet already has the Microsoft.Storage service endpoint. You want to use Microsoft.Storage.Global instead. What should you do?**
- *Remove Microsoft.Storage and then add Microsoft.Storage.Global, because they can't coexist on one subnet (correct)*
- Add Microsoft.Storage.Global alongside the existing endpoint
- Create a private endpoint, which is required for cross-region access

---
## 8. Azure DNS

**You create a public DNS zone for contoso.com in Azure DNS. What must you do so that Azure DNS answers queries for the domain?**
- Create a CNAME record at your registrar pointing to the Azure zone
- *Update the NS records at the domain registrar to the Azure DNS name servers (correct)*
- Add an SOA record in the Azure zone

**Can you purchase a new domain name directly through Azure DNS?**
- *No, Azure DNS hosts zones but doesn't register domain names (correct)*
- Yes, for .com and .net domains
- Yes, for any top-level domain

**You need contoso.com (the zone apex) to resolve to an Azure Front Door endpoint. What should you create?**
- *An alias record at the apex (correct)*
- A CNAME record at the apex
- An A record pointing to the Front Door endpoint's IP address

**Which record types support Azure DNS alias record sets?**
- A, MX and TXT
- All record types
- *A, AAAA and CNAME (correct)*

**When a public IP address is deleted, you want the DNS record pointing to it to stop returning a stale address automatically. What should you use?**
- *An alias record that references the public IP resource (correct)*
- A record set with a TTL of 0
- A PTR record

**You need to delegate partners.contoso.com to a separate Azure DNS zone. contoso.com is already hosted in Azure DNS. What should you do?**
- Create a CNAME record for partners in the contoso.com zone
- Register partners.contoso.com with a new registrar
- *Create the partners.contoso.com zone, then add an NS record set for partners in the contoso.com zone (correct)*

**Which record sets are created automatically at the apex when you create a public DNS zone?**
- A and AAAA
- MX and TXT
- *SOA and NS (correct)*

**How many records can a CNAME record set contain?**
- 20
- Unlimited
- *One (correct)*

**VMs should be registered automatically in the private DNS zone corp.contoso.com as they're deployed. What should you configure?**
- A conditional forwarder to 168.63.129.16
- *A virtual network link with autoregistration enabled (correct)*
- An alias record set for each VM

**A virtual network already has autoregistration enabled with one private DNS zone. You try to enable autoregistration with a second zone, and it fails. Why?**
- Private DNS zones can't share virtual networks
- *A virtual network can have only one registration zone (correct)*
- The virtual networks must be peered first

**VMs in two virtual networks that are not peered must resolve records in the same private DNS zone. What is required?**
- Peering between the virtual networks
- *Link both virtual networks to the private DNS zone (correct)*
- An Azure DNS Private Resolver in each virtual network

**You want an internal load balancer's frontend IP to be registered in a private DNS zone with autoregistration enabled. What happens?**
- The ILB is registered automatically with its frontend name
- The ILB is registered only if it has a static frontend IP
- *Nothing — autoregistration supports VMs only, so you must create the record manually (correct)*

**A virtual network uses a custom DNS server running on a VM. VMs can't resolve records in a private DNS zone linked to the virtual network. What should you configure on the DNS server?**
- A forwarder to the registrar's name servers
- *A conditional forwarder for the zone to 168.63.129.16 (correct)*
- A secondary zone transferred from Azure DNS

**On-premises clients must resolve records in an Azure private DNS zone. Which Azure DNS Private Resolver component should on-premises DNS servers forward to?**
- An outbound endpoint
- A DNS forwarding ruleset
- *An inbound endpoint (correct)*

**Azure VMs must resolve names in an on-premises domain. Which Azure DNS Private Resolver components do you need?**
- An inbound endpoint only
- *An outbound endpoint and a DNS forwarding ruleset (correct)*
- A virtual network link with autoregistration

**You create public and private DNS zones that both use the name contoso.com. What do VMs in a virtual network linked to the private zone receive?**
- An error, because zone names must be unique
- *The private zone's records (split-horizon DNS) (correct)*
- The public records, because public zones take precedence

**A user must manage records in public DNS zones, and nothing else. Which built-in role follows least privilege?**
- Network Contributor
- Private DNS Zone Contributor
- *DNS Zone Contributor (correct)*

**What is the maximum number of virtual networks that can be linked with autoregistration enabled to a single private DNS zone?**
- 1
- *100 (correct)*
- 1,000

---
## 9. Azure Load Balancer

**At which OSI layer does Azure Load Balancer operate?**
- Layer 3
- Layer 7
- *Layer 4 (correct)*

**Which health probe protocols does a Standard Load Balancer support?**
- TCP and UDP
- *TCP, HTTP and HTTPS (correct)*
- HTTP only

**When does an HTTP health probe mark a backend instance as healthy?**
- When the instance returns any HTTP response
- When the instance returns a 3xx redirect
- *When the instance returns HTTP 200 (correct)*

**From which IP address do Azure Load Balancer health probes originate?**
- The load balancer's frontend IP
- *168.63.129.16 (correct)*
- 169.254.169.254

**What is the default distribution mode of Azure Load Balancer?**
- *A five-tuple hash (correct)*
- A two-tuple hash of source and destination IP
- Round robin

**Requests from the same client IP must always reach the same backend VM. Which session persistence setting should you choose?**
- *Client IP (two-tuple) (correct)*
- None
- Client IP and protocol (three-tuple)

**Which type of load balancer supports HA ports?**
- A public Standard Load Balancer
- A Gateway Load Balancer only
- *An internal Standard Load Balancer (correct)*

**Administrators need to RDP to a specific backend VM by connecting to the load balancer's public IP on port 50001. What should you configure?**
- *An inbound NAT rule (correct)*
- A load-balancing rule with Client IP session persistence
- An outbound rule

**Backend VMs in a Standard public load balancer's pool need outbound internet access through the load balancer's public IP. What should you configure?**
- *An outbound rule (correct)*
- An inbound NAT rule
- An HA ports rule

**Health probes succeed, but clients on the internet can't reach a public Standard Load Balancer. What is the most likely cause?**
- Floating IP is disabled on the load-balancing rule
- *No NSG rule allows the client traffic on the data port, because Standard Load Balancer is secure by default (correct)*
- The load balancer must be downgraded to Basic for internet traffic

**A VM in an internal load balancer's backend pool can't connect to that load balancer's frontend IP, but other clients can. What is the cause?**
- The VM's NSG blocks the AzureLoadBalancer service tag
- *A backend VM connecting to the frontend of its own internal load balancer (hairpin) isn't supported (correct)*
- Session persistence is set to None

**Which load balancer metric shows whether the frontend is reachable end to end?**
- Health Probe Status
- SNAT connection count
- *Data Path Availability (correct)*

**Which load balancer metric shows the health of each individual backend instance?**
- Data Path Availability
- *Health Probe Status (correct)*
- Used SNAT ports

**An HTTP health probe uses the path /health, but the application returns 404 for that path. All backends are marked down. What should you do?**
- *Update the probe to use a path that returns HTTP 200 (correct)*
- Change the probe interval to 60 seconds
- Change the distribution mode to Client IP

**Does changing a load balancer's distribution mode cause downtime?**
- Yes, existing connections are reset
- *No, the distribution mode can be changed without downtime (correct)*
- Yes, the frontend IP is reassigned

**On a Standard Load Balancer, what happens to established TCP connections when a backend instance starts failing its health probe?**
- They are reset immediately
- *They stay alive; only new flows are sent to healthy instances (correct)*
- They are drained over 30 minutes

**Which public IP SKU must a Standard public load balancer's frontend use?**
- Basic
- *Standard (correct)*
- Either Basic or Standard

**What is the scope of backend pool members in a Standard Load Balancer?**
- *VMs or scale set instances in a single virtual network (correct)*
- VMs in a single availability set
- VMs in a single subnet

**You need to load-balance TCP 1433 across SQL Server VMs for clients inside the virtual network only. What should you deploy?**
- A public Standard Load Balancer
- Application Gateway v2
- *An internal Standard Load Balancer (correct)*

**A web application must route /images and /video to different backend pools and needs a web application firewall. What should you deploy?**
- A public Standard Load Balancer with two backend pools
- Traffic Manager with path-based routing
- *Application Gateway v2 with the WAF_v2 SKU (correct)*

**You need DNS-based global routing for a non-HTTP application deployed in three regions. What should you use?**
- *Traffic Manager (correct)*
- Azure Front Door
- Application Gateway

---
## 10. Troubleshooting Network Connectivity

**Traffic from VM1 to 10.0.2.4 on TCP 1433 fails. You need to know whether it's allowed and which NSG rule is responsible. What should you use?**
- Next hop
- Connection monitor
- *IP flow verify (correct)*

**You suspect traffic from a VM is being sent to the wrong place. Which Network Watcher tool shows the next hop type and the route responsible?**
- IP flow verify
- *Next hop (correct)*
- Packet capture

**You need continuous monitoring of latency and reachability between Azure VMs and an on-premises server, with alerts on changes. What should you use?**
- Connection troubleshoot
- IP flow verify
- *Connection monitor (correct)*

**You need a one-time check of whether a VM can reach sql.contoso.com on port 1433, including the hops and any NSG, UDR or DNS fault. What should you use?**
- Connection monitor
- *Connection troubleshoot (correct)*
- Topology

**You need to inspect the actual packets sent and received by a VM. What should you use?**
- VNet flow logs
- *Packet capture (correct)*
- IP flow verify

**You need to log all allowed and denied IP flows across a virtual network. NSG flow logs aren't available to create. What should you use?**
- NSG diagnostics
- *Virtual network flow logs (correct)*
- Connection monitor

**What is the status of NSG flow logs?**
- They are fully supported and recommended for new deployments
- *New NSG flow logs can't be created since 30 June 2025, and the feature retires on 30 September 2027 (correct)*
- They were retired on 30 September 2025

**How is Azure Network Watcher enabled?**
- It must be deployed manually in each subscription
- It's enabled once per subscription in the subscription's home region
- *It's enabled automatically per region when you create or update a virtual network, in the NetworkWatcherRG resource group (correct)*

**What must be installed on an Azure VM before you can run a packet capture on it?**
- The Azure Monitor Agent
- *The Network Watcher Agent VM extension (correct)*
- The Log Analytics gateway

**A site-to-site VPN connection is down and you need a diagnosis of the gateway and connection. Which tool should you use?**
- *VPN troubleshoot (correct)*
- Next hop
- IP flow verify

**You need a visual diagram of the resources in a virtual network and how they're connected. What should you use?**
- *Topology (correct)*
- Effective routes
- Traffic analytics

**What does traffic analytics require in addition to flow logs?**
- An Azure Firewall policy
- *A Log Analytics workspace (correct)*
- A Network Watcher agent on every VM

**Next hop shows the next hop type None for traffic from a VM to the internet. What is the most likely cause?**
- *A user-defined route with next hop None matches the destination (correct)*
- An NSG is denying outbound internet traffic
- The VM has no public IP address

**Connection troubleshoot reports a UserDefinedRoute fault. What should you investigate?**
- The VM's guest firewall
- *The route table associated with the source subnet (correct)*
- The VM's CPU and memory usage

---
## 11. Related Topics

**What must the subnet that hosts a VPN gateway be named?**
- *GatewaySubnet (correct)*
- VPNSubnet
- AzureGatewaySubnet

**In a site-to-site VPN configuration, which Azure resource represents the on-premises VPN device and its address prefixes?**
- The virtual network gateway
- *The local network gateway (correct)*
- The connection resource

**Remote employees need to connect their individual laptops to an Azure virtual network over an encrypted tunnel. Which connection type should you use?**
- Site-to-site VPN
- ExpressRoute
- *Point-to-site VPN (correct)*

**Your organisation needs a private connection between its datacentre and Azure that doesn't traverse the public internet. What should you use?**
- *ExpressRoute (correct)*
- Site-to-site VPN
- Global VNet peering

**You need to filter outbound traffic from Azure VMs by fully qualified domain name. What should you deploy?**
- An NSG with service tags
- *Azure Firewall (correct)*
- A NAT gateway

**What subnet does Azure Firewall require?**
- GatewaySubnet, /27
- A subnet of any name, /24
- *AzureFirewallSubnet, /26 (correct)*

**What is the current status of Application Gateway v1?**
- It's supported but no longer receives new features
- *It was retired on 28 April 2026; v2 must be used (correct)*
- It's recommended for small workloads

**One Application Gateway must serve contoso.com and fabrikam.com, each with its own backend pool. Which feature should you use?**
- *Multi-site hosting (correct)*
- URL path-based routing
- Cookie-based session affinity

**During maintenance, a backend VM must stop receiving new connections while existing ones complete. Which Application Gateway feature should you use?**
- Session affinity
- *Connection draining (correct)*
- Health probes

**A global web application needs a single HTTP(S) entry point with caching, a web application firewall and fast regional failover. What should you use?**
- Traffic Manager
- *Azure Front Door (correct)*
- Application Gateway v2

**You need to centrally manage hub-and-spoke peering across dozens of virtual networks in several subscriptions. What should you use?**
- Azure Policy only
- Network Watcher
- *Azure Virtual Network Manager (correct)*

**How many VPN gateways can a single virtual network have?**
- Two, for active-active mode
- One per GatewaySubnet, with up to two GatewaySubnets
- *One (correct)*
