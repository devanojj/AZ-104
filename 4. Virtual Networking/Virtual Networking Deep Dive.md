
1. **Configure and manage virtual networks in Azure** — VNets & subnets, VNet peering, public IP addresses, user-defined routes, troubleshoot network connectivity
2. **Configure secure access to virtual networks** — NSGs & ASGs, evaluate effective security rules, Azure Bastion, service endpoints for PaaS, private endpoints for PaaS
3. **Configure name resolution and load balancing** — Azure DNS, internal or public load balancer, troubleshoot load balancing

> [!warning] Things that changed recently — older study material is wrong on these
> * **Basic Load Balancer** and **Basic public IP** were **retired 30 September 2025** → Standard SKU is the only answer.
> * **Default outbound access**: for API versions released after **31 March 2026**, subnets in **new VNets are private by default** — VMs need an explicit outbound method. The portal already defaults to private subnets.
> * **Application Gateway v1** was **retired 28 April 2026** → v2 (Standard_v2 / WAF_v2) only.
> * **NSG flow logs**: no new ones can be created since **30 June 2025**, full retirement **30 September 2027** → use **VNet flow logs**.
> * **Peered VNet address space** *can* now be changed — you must **Sync** the peering afterwards.

---
## 1. Virtual Networks & Subnets

### VNet Fundamentals
* A VNet is **regional** and belongs to **one subscription**. It cannot span regions, can connect regions with peering or gateways.
* **Layer 3 only** — no broadcast, multicast, or VLANs.
* Address space should use **RFC 1918** ranges: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`.
* You **cannot** use: `224.0.0.0/4` (multicast), `255.255.255.255/32` (broadcast), `127.0.0.0/8` (loopback), `169.254.0.0/16` (link-local), `168.63.129.16/32` (Azure platform/DNS IP).
* A VNet can have **multiple address ranges**. You can add/remove ranges later — remove only if no resources use it.
* Address spaces **must not overlap** with any VNet you want to peer with, or with on-premises ranges you want to connect.

### Subnets
* Each subnet must sit inside the VNet's address space and **cannot overlap** other subnets.
* **Azure reserves 5 IPs per subnet:**

| Address (for `10.0.1.0/24`) | Purpose           |
| --------------------------- | ----------------- |
| `10.0.1.0`                  | Network address   |
| `10.0.1.1`                  | Default gateway   |
| `10.0.1.2`, `10.0.1.3`      | Azure DNS mapping |
| `10.0.1.255`                | Broadcast         |

* **Smallest IPv4 subnet: /29** (8 − 5 = **3 usable**). Largest: /2. **IPv6 subnets must be exactly /64.**

|Prefix|Total|Usable|
|---|---|---|
|/29|8|**3**|
|/28|16|**11**|
|/27|32|**27**|
|/26|64|**59**|
|/24|256|**251**|

* You can **resize** a subnet that has resources only if the new range still contains every IP in use.
* Resources in different subnets of the same VNet **route to each other by default** — no route tables needed.

### Things Attached at Subnet Level
Each subnet can have **zero or one** of each:
* **NSG**
* **Route table**
* **NAT gateway**

Plus: **service endpoints**, **delegation**, **private endpoint network policies**, and the **private subnet** flag.

### Special-Purpose Subnets
|Subnet name|Used by|Size|Notes|
|---|---|---|---|
|**`GatewaySubnet`**|VPN / ExpressRoute gateways|/27 or larger recommended|Name must be exact. See [[GatewaySubnet]]|
|**`AzureBastionSubnet`**|Azure Bastion (Basic/Standard/Premium)|**/26 or larger**|Nothing else may be deployed in it; **UDRs not supported**|
|**`AzureFirewallSubnet`**|Azure Firewall|**/26**|Dedicated|
|*(any name)*|Application Gateway v2|/24 recommended|Dedicated to App Gateways only|

### Subnet Delegation
* Hands a subnet to a specific PaaS service that injects into your VNet (App Service VNet integration, SQL Managed Instance, Container Instances, etc.).
* You **can't change a delegation** while that service's resources are still deployed in the subnet.

### VNet DNS Settings
* **Default:** Azure-provided DNS (`168.63.129.16`) — resolves VM hostnames **within the same VNet only**.
* **Custom:** set DNS servers on the VNet (or override per NIC). **VMs must be restarted** (or renew their DHCP lease) to pick up a change.
* Azure-provided DNS **does not resolve names across peered VNets** → use a **Private DNS zone** linked to both (see section 8).

---
## 2. IP Addressing

### Private IP Addresses
|Allocation|Behaviour|Use For|
|---|---|---|
|**Dynamic** (default)|Next free address in the subnet|Most VMs|
|**Static**|You choose an address in the subnet; it's held for the NIC|**Domain controllers, DNS servers**, anything referenced by IP (firewall rules, NVA next hops)|

* Set a static private IP **on the NIC's IP configuration in Azure — never inside the guest OS**. Hard-coding it in the OS can break connectivity.
* A NIC can have **multiple IP configurations** (one primary).

### Public IP Addresses — Standard SKU
**Basic SKU is retired** (no new Basic after 31 March 2025, retired 30 September 2025). Standard is the answer for every public IP question.

|Property|Standard SKU|
|---|---|
|**Allocation**|**Static only**|
|**Security**|**Secure by default — closed to inbound** until an **NSG** allows it|
|**Availability zones**|No-zone, **zonal**, or **zone-redundant** (zone-redundant needs a region with 3 AZs)|
|**Routing preference**|**Microsoft network** (default, traffic stays on Microsoft's WAN) or **Internet** (hands off to ISPs sooner, cheaper)|
|**Tier**|**Regional** (default) or **Global** (required for a **cross-region load balancer** frontend; no AZ support)|
|**Idle timeout**|4–30 min inbound (default 4)|
|**Billing**|Charged **whether or not it's attached** — delete unused IPs|
|**Works with**|Standard LB, NAT gateway, Azure Firewall, Bastion, VPN gateway, App Gateway v2, VM NICs|

* **Public IP prefix** — a reserved, **contiguous** block of static public IPs. Use it when a partner needs to allow-list a predictable range.
* **DNS name label** — gives `<label>.<region>.cloudapp.azure.com`.
* A Standard LB requires a **Standard** public IP.

> [!warning] Old exam question, new answer
> "VM lost its public IP after a restart" was a **Basic dynamic** public IP releasing on deallocation. Standard IPs are **always static**, so this no longer happens with new IPs.

### Default Outbound Access & Explicit Outbound
* **Default outbound access** = an implicit, Microsoft-owned public IP that Azure gave VMs without any outbound configuration. It can change without notice and isn't filterable.
* **New VNets** (API versions after **31 March 2026**) create **private subnets** (`defaultOutboundAccess = false`) → no internet egress unless you configure one of:

|Explicit outbound method|Notes|
|---|---|
|**NAT gateway** on the subnet|**Recommended.** Up to **16 public IPs**, **64,512 SNAT ports per IP**, takes precedence over other outbound methods|
|**Public IP on the VM's NIC**|Simple, per-VM|
|**Outbound rules on a Standard public LB**|SNAT through the LB's frontend IP|
|**UDR `0.0.0.0/0` → Azure Firewall / NVA**|Centralised egress filtering|

* **Existing VNets aren't changed.** Changing a subnet to/from private only applies to a VM after it's **stopped (deallocated)**.

---
## 3. VNet Peering

See also [[VNet Peering]], [[Unit 3 - Transit Gateway]], [[Unit 5 - Extend Peering]].

### What Peering Is
* Connects two VNets over the **Microsoft backbone using private IPs** — no gateway, no public internet, low latency.
* **Regional peering** (same region) or **global peering** (different regions).
* Works **across subscriptions** and **across Microsoft Entra tenants**.
* **No downtime** to resources when you create it.
* **Requirements:** **non-overlapping address spaces**. You **can't move** a VNet to another RG/subscription while it's peered — delete the peering first.

### Peering Status
A peering is **two links** (A→B and B→A). The portal creates both at once.

|Status|Meaning|Fix|
|---|---|---|
|**Initiated**|Only one link exists|Create the link from the other VNet|
|**Connected**|Both links exist — traffic flows|—|
|**Disconnected**|One link was deleted|**Delete the remaining link and recreate both**|

### Peering Is Non-Transitive
A ↔ B and B ↔ C does **not** give A ↔ C. Options:
1. Peer A ↔ C directly (**mesh**).
2. **Hub-and-spoke** with an **NVA / Azure Firewall** in the hub + **UDRs** on spoke subnets + **Allow forwarded traffic** (see [[Service chaining]]).
3. **Azure Virtual Network Manager** connectivity configs (hub-spoke or mesh at scale) — see [[Azure Virtual Network Manager]].
4. **Azure Virtual WAN** (Standard tier) for managed transit.

### Peering Settings
|Setting (per link)|What it does|
|---|---|
|**Allow access to remote VNet**|Lets traffic flow across the peering. Turn off to temporarily block without deleting|
|**Allow forwarded traffic**|Accepts traffic that **didn't originate** in the remote VNet (e.g. forwarded by a hub NVA). **Needed for spoke-to-spoke via NVA**. Not needed for VPN gateway transit|
|**Allow gateway transit** (*"Use this VNet's gateway or Route Server"*)|Set on the **hub** — lets peers use its VPN/ExpressRoute gateway|
|**Use remote gateways** (*"Use the remote VNet's gateway or Route Server"*)|Set on the **spoke** — use the hub's gateway|

> [!tip] Gateway transit rules
> * Hub = **Allow gateway transit**. Spoke = **Use remote gateways**. Both are required.
> * The spoke **must not have its own gateway**.
> * **Use remote gateways** can be enabled on **only one** peering per VNet.
> * Works over **regional and global** peering.
> * **Basic VPN Gateway SKU doesn't support gateway transit.**

### Changing Address Space on a Peered VNet
* **Supported.** Add, remove or resize the range, then select the peering and click **Sync** — for **every** remote peered VNet.
* Run sync after **each** change rather than batching. If you skip it, the remote side keeps routing to the old ranges (and the peering can go Disconnected).

### Other Peering Facts
* **NSGs still apply** across a peering. The **`VirtualNetwork` service tag includes peered VNet address spaces**, so the default `AllowVNetInBound` rule permits peered traffic.
* Peering adds system routes with next hop **VNet peering** for each remote address range (visible in effective routes after a few minutes).
* **Name resolution** across peered VNets needs a **Private DNS zone** linked to both, or custom DNS.
* Billed **per GB in and out** of each side; global peering costs more.

### Peering vs VNet-to-VNet VPN
|  |**VNet peering**|**VNet-to-VNet VPN**|
|---|---|---|
|Path|Microsoft backbone, private|IPsec tunnel through VPN gateways|
|Gateway needed|❌|✅ one in each VNet|
|Bandwidth / latency|Highest / lowest|Limited by gateway SKU|
|Encrypted|Not by peering itself|✅ IPsec|
|Use when|Default choice|Encryption between VNets is mandated|

---
## 4. Routing & User-Defined Routes

See also [[Unit 2 - Azure Routes]], [[UDR]], [[NVA]], [[Effective Routes]].

### Default System Routes
Created automatically for every subnet — you **can't create or delete** them, only override.

|Address prefix|Next hop type|
|---|---|
|VNet address space|**Virtual network**|
|`0.0.0.0/0`|**Internet**|
|`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`|**None** (dropped) — changes to *Virtual network* if you use the range|
|`100.64.0.0/10`|**None**|

**Optional system routes** appear when you enable features:

|Feature|Next hop type|Added to|
|---|---|---|
|VNet peering|**VNet peering**|All subnets|
|VPN/ExpressRoute gateway|**Virtual network gateway** (BGP / local network gateway prefixes)|All subnets|
|Service endpoint|**VirtualNetworkServiceEndpoint**|**Only the subnet with the endpoint**|

### UDR Next Hop Types
|Next hop|Use for|
|---|---|
|**Virtual appliance**|Send to an **NVA / Azure Firewall** — you must give its **private IP** (can be an **internal LB frontend** for HA NVAs)|
|**Virtual network gateway**|Force traffic to the VPN gateway (**forced tunnelling** of `0.0.0.0/0` to on-prem)|
|**Virtual network**|Override a route to keep traffic in the VNet|
|**Internet**|Send directly to the internet (e.g. carve out an exception to forced tunnelling)|
|**None**|**Drop** the traffic|

* You **cannot** create a UDR with next hop `VirtualNetworkServiceEndpoint` or `VNet peering`.
* **Service tags** can be used as the UDR address prefix (e.g. route `Storage` directly to Internet).

### Route Tables
* A subnet has **zero or one** route table. One route table can be associated with **many subnets**.
* The route table must be in the **same region and subscription** as the VNet.
* Up to **400 UDRs per route table** (1,000 with AVNM routing configurations).
* **Propagate gateway routes** (BGP route propagation): turn **off** on workload subnets when you want all traffic forced through a firewall instead of learning on-prem routes from the gateway. **Never disable it on `GatewaySubnet`.**

### NVA Requirements
For a VM to forward traffic it didn't originate:
1. **IP forwarding enabled on the NIC in Azure** (NIC → IP configurations).
2. Forwarding enabled **inside the OS** / appliance software.
3. **UDRs** pointing at its private IP.
4. In hub-and-spoke: **Allow forwarded traffic** on the peerings.

### How Azure Picks a Route
1. **Longest prefix match first** — `10.0.0.0/24` beats `10.0.0.0/16` for `10.0.0.5`.
2. If prefixes are identical: **UDR > BGP > System**.

> [!warning] Exceptions to remember
> * System routes for **VNet, peering and service endpoint** traffic are preferred even over more specific BGP routes.
> * **Service endpoint routes can't be overridden** by a route table.
> * A **private endpoint** adds a **/32** route, which beats a broad UDR like `10.0.0.0/8 → firewall`. To force PE traffic through a firewall, enable **UDR network policies** on the PE subnet and use a UDR prefix **equal to or narrower** than the VNet address space.

* For **outbound** traffic: **NSG outbound rules are processed first, then the UDR**.

---
## 5. NSGs & ASGs

See also [[Unit 3 - NSG Rules]], [[Unit 4 - Effective Rules]], [[Unit 6 - Application Security Groups]].

### NSG Basics
* **Stateful** L3/L4 filter (5-tuple: source IP, source port, destination IP, destination port, protocol). Return traffic for an allowed flow is automatically allowed.
* Associate to a **subnet** and/or a **NIC**. **One NSG per subnet** and **one per NIC**, but one NSG can be reused on many subnets/NICs.
* NSG must be in the **same region** as the resource it's attached to.
* Up to **1,000 rules per NSG**.

### Rule Properties
|Property|Values|
|---|---|
|**Priority**|**100–4096**. **Lower number = processed first.** First match wins, processing stops|
|**Source / Destination**|Any, IP, CIDR, **service tag**, or **ASG**|
|**Ports**|Single, range, or list|
|**Protocol**|TCP, UDP, ICMP, Any|
|**Direction**|Inbound or outbound — separate rule spaces|
|**Action**|Allow or Deny|

* **Augmented rules** allow multiple IPs/ranges/ports in one rule — but **not multiple service tags or multiple ASGs** in one field.
* Inbound NSG rules are evaluated **after public → private NAT**, so the destination is the VM's **private IP**.

### Default Rules (can't delete — override with lower numbers)
|Priority|Name|Direction|Action|
|---|---|---|---|
|65000|**AllowVNetInBound**|In|Allow `VirtualNetwork` → `VirtualNetwork`|
|65001|**AllowAzureLoadBalancerInBound**|In|Allow `AzureLoadBalancer` → Any|
|65500|**DenyAllInBound**|In|Deny|
|65000|**AllowVnetOutBound**|Out|Allow `VirtualNetwork` → `VirtualNetwork`|
|65001|**AllowInternetOutBound**|Out|Allow → `Internet`|
|65500|**DenyAllOutBound**|Out|Deny|

> [!tip] Read the defaults carefully
> An empty NSG is **not** deny-everything: it allows all VNet (and peered VNet) traffic, the LB health probe, and all outbound internet. It only blocks **inbound from the internet**.

### Evaluation Order (Subnet + NIC)
|Traffic|First|Then|
|---|---|---|
|**Inbound**|**Subnet NSG**|**NIC NSG**|
|**Outbound**|**NIC NSG**|**Subnet NSG**|

**Both** must allow the traffic. A deny at either level blocks it.

**Worked example:** subnet NSG allows inbound 3389 from your IP at priority 100. The NIC NSG has no custom rules. → RDP **fails** — the NIC NSG falls through to **DenyAllInBound (65500)**.

### Common Service Tags
`VirtualNetwork` (VNet + peered + on-prem via gateway), `Internet`, `AzureLoadBalancer` (`168.63.129.16`), `Storage` / `Storage.<Region>`, `Sql`, `AzureCloud`, `AzureMonitor`, `GatewayManager`, `AzureBastion`. Microsoft maintains the IP lists.

### Application Security Groups (ASGs)
* Logical group of **NICs** by workload (web, app, db) — use the ASG as source/destination in NSG rules instead of IPs.
* **All NICs in an ASG must be in the same VNet.** If a rule uses ASGs as both source and destination, both ASGs must be in the same VNet.
* Up to **10 ASGs** in a rule's source or destination. A NIC can belong to multiple ASGs.
* ASGs **don't filter traffic themselves** — they only make NSG rules readable and dynamic.

### Evaluating Effective Security Rules
* **NIC → Effective security rules** shows the **merged** subnet + NIC rules actually applied. The VM must be **running**.
* **IP flow verify** (Network Watcher) — give it a 5-tuple, it returns **Allow/Deny and the exact rule name**.
* **NSG diagnostics** (Network Watcher) — shows each NSG in the path and which rule matched at each level.
* Best practice: apply an NSG at **either** subnet or NIC level, not both, to avoid confusing interactions.

---
## 6. Azure Bastion

Browser-based **RDP/SSH over TLS (port 443)** from the Azure portal to VMs using their **private IP**. **VMs need no public IP** and no RDP/SSH exposure to the internet.

### SKUs
|Feature|**Developer**|**Basic**|**Standard**|**Premium**|
|---|---|---|---|---|
|Cost|**Free**|Paid|Paid|Paid|
|Dedicated host|❌ shared|✅|✅|✅|
|Needs `AzureBastionSubnet`|❌|✅|✅|✅|
|Needs public IP|❌|✅|✅|❌ (private-only option)|
|VMs in **peered VNets**|❌|✅|✅|✅|
|Concurrent connections|**1 VM at a time**|✅|✅|✅|
|Instances|—|2 fixed|**2–50 (host scaling)**|2–50|
|Kerberos auth|✅|✅|✅|✅|
|**Native client** (`az network bastion rdp/ssh`)|❌|❌|✅|✅|
|**Shareable link**|❌|❌|✅|✅|
|**IP-based connection** (incl. on-prem via VPN/ER)|❌|❌|✅|✅|
|Custom inbound port|❌|❌|✅|✅|
|File upload/download (native client)|❌|❌|✅|✅|
|RDP to Linux / SSH to Windows|❌|❌|✅|✅|
|**Session recording**|❌|❌|❌|✅|
|**Private-only deployment**|❌|❌|❌|✅|

* Each instance supports **20 RDP / 40 SSH** concurrent sessions → Basic = 40 RDP, Standard/Premium max = 1,000 RDP.
* **Upgrade only — no downgrade.** Downgrading means delete and recreate. Upgrades take ~10 minutes.

### Requirements
* Subnet named exactly **`AzureBastionSubnet`**, **/26 or larger**, nothing else in it, **no UDRs**.
* **Standard SKU, static** public IP in the same region (except Developer and Premium private-only).
* If you put an NSG on `AzureBastionSubnet`, it must allow at least: inbound **443 from `Internet`**, **443 from `GatewayManager`**, **443 from `AzureLoadBalancer`**; outbound **3389/22 to `VirtualNetwork`** and **443 to `AzureCloud`**.
* Target VM NSGs must allow **3389/22 from the `AzureBastionSubnet` range** (or `VirtualNetwork`).

> [!tip] Scenario mapping
> * "Free, dev/test, one VM" → **Developer**
> * "Reach VMs in peered spokes from one Bastion in the hub" → **Basic** or higher
> * "Connect with the local RDP/SSH client" or "upload files" or "share a link with a contractor" → **Standard**
> * "Record sessions for audit" or "no public IP on Bastion" → **Premium**

---
## 7. Service Endpoints vs Private Endpoints (PaaS)

Storage-specific detail is in [[Storage Deep Dive]]; this section is the networking view. See also [[Service Endpoint]] and [[Private Endpoint]].

|  |**Service endpoint**|**Private endpoint**|
|---|---|---|
|What it is|Subnet setting that routes traffic to a PaaS **service** over the backbone|A **NIC with a private IP** in your subnet mapped to **one resource instance**|
|Configured on|The **subnet** (+ a VNet rule on the resource)|A resource in your VNet (+ Private DNS zone)|
|Source IP seen by service|VM's **private IP**|VM's **private IP**|
|Service's DNS answer|**Public IP** (unchanged)|**Private IP** via `privatelink.*` zone|
|Scope|**All instances** of the service type (e.g. every storage account)|**One specific resource** and sub-resource → **data exfiltration protection**|
|From on-premises (VPN/ER)|❌|✅|
|From peered VNets|❌ (enable per subnet)|✅ regional and global|
|Cost|Free|Per endpoint + per GB|
|Microsoft's recommendation|Legacy option|**Recommended**|

### Service Endpoint Details
* Enable per subnet, per service: `Microsoft.Storage`, **`Microsoft.Storage.Global`** (cross-region — use for new deployments; can't coexist with `Microsoft.Storage` on one subnet), `Microsoft.Sql`, `Microsoft.KeyVault`, etc.
* Two steps: **enable the endpoint on the subnet** *and* **add a VNet rule on the resource's firewall**. Enabling alone grants nothing.
* Enabling/disabling **resets existing TCP connections** to that service from the subnet — do it in a maintenance window.
* Azure SQL: VNet must be in the **same region** as the server. Cross-tenant only works for **Storage and Key Vault**.
* **Service endpoint policies** (Storage) — restrict the subnet to **specific storage accounts**, closing the "any storage account" exfiltration gap.
* Shows in effective routes as next hop **`VirtualNetworkServiceEndpoint`**.

### Private Endpoint Details
* One PE per **target sub-resource** (`blob`, `file`, `sqlServer`, `vault`…).
* **DNS:** integrate with a Private DNS zone (`privatelink.blob.core.windows.net`, `privatelink.file.core.windows.net`, `privatelink.database.windows.net`, `privatelink.vaultcore.azure.net`) linked to every VNet that needs to resolve it. Without it, the name still resolves to the public IP.
* **Approval:** **automatic** if you have RBAC rights on the target resource, otherwise **manual** — the resource owner approves. States: **Approved, Pending, Rejected, Disconnected**. Only Approved endpoints pass traffic.
* **Network policies are disabled by default** on PE subnets → NSGs and UDRs **don't apply** to PE traffic until you enable `PrivateEndpointNetworkPolicies` (NSG, UDR, or both).
* A PE **doesn't automatically disable public access** — also lock down the resource's public network access/firewall.
* Connections are **one-directional** — clients connect in; the service can't initiate to your VNet.
* **Private Link service** = the reverse: expose **your own** service (behind a **Standard internal LB**) to consumers' private endpoints.

---
## 8. Azure DNS

See also [[DNS]], [[CNAME]], [[Virtual Network Link]], [[Split-horizon DNS]], [[DNS Forwarding]].

### Public DNS Zones
* Hosts records for domains you own — **Azure DNS does not sell/register domains** (use a registrar or App Service Domains).
* **Delegation:** create the zone → Azure assigns **four name servers** (`ns1-xx.azure-dns.com`, `.net`, `.org`, `.info`) → update the **NS records at your registrar** to those servers.
* **Child zone delegation** (e.g. `partners.contoso.com`): create the child zone, then add an **NS record set in the parent zone** pointing to the child's name servers.
* **SOA** and **NS** record sets at the apex (`@`) are created automatically.
* Record types: **A, AAAA, CAA, CNAME, MX, NS, PTR, SOA, SRV, TXT**.
* A **record set** = all records with the same name and type, sharing one **TTL**. A CNAME record set holds **only one** record.
* **CNAME is not allowed at the zone apex** (`contoso.com`).
* RBAC: **DNS Zone Contributor** manages public zones; **Private DNS Zone Contributor** for private zones. Protect production zones with **resource locks**.

### Alias Records
* An **alias record set** points to an **Azure resource** instead of a fixed value. Supported on **A, AAAA, CNAME**.
* Targets: **public IP**, **Traffic Manager profile**, **Azure Front Door**, **Azure CDN endpoint**, or **another record set in the same zone**.
* **Solves the apex problem** — `contoso.com` → Front Door / Traffic Manager / public IP without a CNAME.
* **Auto-updates** when the target IP changes, and becomes empty if the target is deleted → **prevents dangling DNS records**.
* Requires the **Microsoft.Network** resource provider to be registered.

### Private DNS Zones
* **Global** resource. Records **resolve only from VNets linked to the zone** — not from the internet.
* **Virtual network link** types:
  * **Registration VNet** (autoregistration on) — Azure **creates A records** for VMs automatically.
  * **Resolution VNet** (autoregistration off) — can query, no automatic records.
* Cross-VNet resolution works **without peering** — just link both VNets.

> [!warning] Autoregistration limits
> * A VNet can have **only one registration zone** (but up to 1,000 linked zones in total).
> * A zone can have up to **100 registration VNets** and **1,000 VNet links**.
> * Autoregistration covers **VMs only** (not ILBs or PaaS), the **primary NIC only**, and **no PTR records**.
> * Records are removed automatically when the VM is deleted.

* **Custom DNS servers on the VNet override** linked private zones. Fix: on the custom DNS server add a **conditional forwarder to `168.63.129.16`**, or use **Azure DNS Private Resolver**.
* **Split-horizon:** a public and a private zone with the **same name** — linked VNets get private answers, the internet gets public ones.
* **Hybrid resolution** with **Azure DNS Private Resolver**:
  * On-prem → Azure private zones: point on-prem conditional forwarders at the resolver's **inbound endpoint**.
  * Azure → on-prem domains: **outbound endpoint** + **DNS forwarding ruleset**.
* Don't use `.local` for private zone names.

---
## 9. Azure Load Balancer

See also [[Azure Health Probe]], [[HA ports]], [[Inbound NAT]], [[General Notes for LB Distribution]].

### Basics
* **Layer 4** (TCP/UDP) — no URL, host header, cookie or TLS awareness.
* **Public LB** (frontend public IP, internet → VMs) or **Internal LB** (frontend private IP, inside VNet / from on-prem).
* SKUs: **Standard** (the only choice for new LBs), **Gateway** (chains third-party NVAs). **Basic retired 30 September 2025.**

### Components
|Component|Purpose|
|---|---|
|**Frontend IP configuration**|Public or private IP clients connect to (can have several)|
|**Backend pool**|VMs / VMSS instances in **one VNet**, added by **NIC** or **IP address**|
|**Health probe**|**TCP, HTTP or HTTPS**. HTTP/S needs a **200 OK**. Probes come from **`168.63.129.16`** (`AzureLoadBalancer` tag)|
|**Load-balancing rule**|Frontend IP:port → backend pool:port, tied to a probe|
|**Inbound NAT rule**|Port-forward to a **specific** VM (e.g. frontend 50001 → VM1:3389)|
|**Outbound rule**|SNAT for backend VMs to reach the internet via the LB's public IP|

### Distribution Modes
|Session persistence|Hash|Behaviour|
|---|---|---|
|**None** (default)|**5-tuple**|Any healthy instance, per flow|
|**Client IP**|**2-tuple** (source + destination IP)|Same client IP → same VM|
|**Client IP and protocol**|**3-tuple**|Same client IP + protocol → same VM|

Switching modes causes **no downtime**.

### Standard LB Features
* **Secure by default** — public LB frontends are **closed to inbound** until an **NSG** on the backend subnet/NIC allows the traffic.
* **Zone-redundant** or **zonal** frontends; **99.99% SLA**.
* **HA ports** — **internal Standard LB only**. Frontend and backend port **0**, protocol **All** → load-balance every TCP/UDP flow on every port. Classic use: **HA pair of NVAs/firewalls**.
* **Floating IP** (direct server return) — needed for SQL Always On listeners and some NVA designs.
* **TCP reset on idle**, idle timeout configurable.
* **Global tier** (cross-region LB) — a public LB with a **Global** tier public IP in front of regional LBs.
* Probe-down behaviour: **existing TCP connections stay alive** even when an instance (or every instance) is probed down; only **new** flows stop.
* A Standard **public** LB only gives backends outbound internet if you add an **outbound rule** (or use a NAT gateway — preferred).

### Choosing a Load Balancer
|  |**Regional**|**Global**|
|---|---|---|
|**HTTP(S)** (L7)|**Application Gateway** (+ WAF)|**Azure Front Door** (+ WAF)|
|**Any TCP/UDP** (L4)|**Azure Load Balancer**|**Traffic Manager** (DNS-based) or **cross-region LB**|

### Troubleshooting Load Balancing
|Symptom|Likely cause|Fix|
|---|---|---|
|Backends show **Down** / Health Probe Status 0%|App **not listening** on the probe port (or bound to `127.0.0.1`)|Start the service / fix the binding|
||**NSG deny** rule above 65001 blocking `AzureLoadBalancer` / `168.63.129.16`|Add an allow rule for the `AzureLoadBalancer` tag on the probe port|
||**Guest OS firewall** blocking the probe|Allow the probe port in the OS firewall|
||**Probe port or HTTP path mismatch**, or path returns 404/500|Point the probe at the right port/path that returns 200|
||Backend pool empty / references a deleted VM's NIC|Fix the pool membership|
|Probes healthy but **no data traffic**|VM not listening on the **data** port|Fix the app/port|
||**NSG blocks the data port** — Standard LB is secure by default|Allow the client source (e.g. `Internet`) on the data port|
||Backend VM calling the **ILB frontend it's a member of** (hairpin)|Not supported — call the other backend directly or use a separate LB / proxy|
|Traffic sticks to one VM|**Session persistence** set to Client IP and clients share a NAT IP|Set persistence to None if not needed|
|Backends can't reach the internet|Private subnet, no outbound rule, or **SNAT port exhaustion**|Add a **NAT gateway** (or outbound rule)|

**Diagnostics:**
* **Data Path Availability** metric — is the **frontend** reachable end to end?
* **Health Probe Status** metric — per-backend probe health.
* **SNAT connection count / Used SNAT ports** — outbound exhaustion.
* **Load Balancer Insights** (Azure Monitor), **Resource Health**, and health event logs such as **`NoHealthyBackends`** and **`DataPathAvailabilityCritical`**.

---
## 10. Troubleshooting Network Connectivity

Network Watcher is **enabled automatically per region** when you create/update a VNet (in the **`NetworkWatcherRG`** resource group). Full tool detail in [[Azure Network Watcher]] and the 4.8 Network Watcher units.

### Which Tool Answers Which Question
|Question|Tool|
|---|---|
|"Is traffic from VM1 to 10.0.2.4:1433 allowed — and **which rule** blocks it?"|**IP flow verify**|
|"Which NSGs are in the path and what did each decide?"|**NSG diagnostics**|
|"What rules are actually applied to this NIC?"|**Effective security rules** (NIC blade)|
|"Where is traffic to X being **routed**?"|**Next hop**|
|"What's the full route table on this NIC?"|**Effective routes** (NIC blade)|
|"Can VM1 reach `sql.contoso.com:1433` **right now**? Where does it fail?"|**Connection troubleshoot** (point-in-time; reports NSG, UDR, DNS, guest firewall, CPU/memory faults)|
|"Alert me if latency or reachability **changes over time**" (Azure + on-prem)|**Connection monitor** (continuous; needs Log Analytics; Arc agent for on-prem sources)|
|"I need the **actual packets**"|**Packet capture** (VM extension, saves to storage or local disk)|
|"Why is the **VPN** down?"|**VPN troubleshoot**|
|"Show me the resources and how they connect"|**Topology**|
|"Who talked to whom, allowed or denied, over time?"|**VNet flow logs** + **Traffic analytics**|

* **NSG flow logs:** no new ones since **30 June 2025**, retired **30 September 2027** → **VNet flow logs** (whole VNet, no duplicates, includes AVNM admin rules).
* Packet capture and Connection monitor need the **Network Watcher Agent** extension on Azure VMs.
* Effective routes / effective security rules need the VM **running**.

### Troubleshooting Order
1. **Is it filtered?** → IP flow verify / effective security rules (NSG at subnet **and** NIC, guest firewall).
2. **Is it routed?** → Next hop / effective routes (UDR to **None**, wrong NVA IP, NVA without **IP forwarding**, forced tunnelling).
3. **Is the path up?** → Peering **Connected** and **synced**, gateway healthy, `Allow forwarded traffic` set.
4. **Does the name resolve?** → custom DNS forwarding, Private DNS zone **linked**, PE DNS integration.
5. **Does it have egress?** → **private subnet** with no NAT gateway / public IP / outbound rule.
6. **Still unclear** → Connection troubleshoot, then packet capture.

---
## 11. Related Topics — Lower Priority

Not listed in the skills outline, but they appear as answer options and in scenario questions.

### VPN Gateway
* Encrypted **IPsec/IKE** tunnels over the internet. Deployed into **`GatewaySubnet`** (/27+). **One VPN gateway per VNet.**
* **Site-to-site (S2S)** — on-prem VPN device ↔ Azure. Needs a **local network gateway** (on-prem device's public IP + on-prem address prefixes).
* **Point-to-site (P2S)** — individual client devices. Auth: **certificate**, **Microsoft Entra ID**, or **RADIUS**.
* **VNet-to-VNet** — gateway to gateway.
* **Route-based** (most scenarios, required for P2S and coexistence) vs **policy-based** (legacy, static).
* **Active-active** mode for resilience. **Basic SKU: no gateway transit.**

### ExpressRoute
* **Private** connection through a connectivity provider — **not over the internet**, predictable latency, higher bandwidth.
* Uses an **ExpressRoute gateway** in `GatewaySubnet`. **Global Reach** links on-prem sites through Microsoft's backbone. Often paired with a **S2S VPN as failover**.
* See [[Azure Front Door vs Express Route]].

### NAT Gateway
* Explicit, scalable **outbound-only** internet for a subnet: up to **16 public IPs**, **64,512 SNAT ports per IP**, takes precedence over other outbound methods. Fixes **SNAT exhaustion**. No inbound.

### Azure Firewall
* Managed, stateful **L3–L7** firewall with **FQDN filtering**, threat intelligence, DNAT. Lives in **`AzureFirewallSubnet` (/26)**.
* Force egress through it with a UDR **`0.0.0.0/0` → Virtual appliance → firewall private IP** on each workload subnet.

### Application Gateway (v2 only)
* **Layer 7** HTTP/HTTPS/WebSocket/HTTP/2 load balancer. **v1 retired 28 April 2026.**
* **URL path-based routing**, **multi-site hosting** (host headers), redirects, header/URL rewrite, **TLS termination** and end-to-end TLS, **WAF**, **autoscaling**, **zone redundancy**, **cookie-based session affinity**, **connection draining**.
* Needs a **dedicated subnet** (/24 recommended).
* See the 4.7 Application Gateway units.

### Front Door & Traffic Manager
* **Front Door** — global L7 HTTP(S) entry point with anycast, caching, WAF, fast failover.
* **Traffic Manager** — **DNS-based** global routing for any protocol (priority, weighted, performance, geographic). Doesn't proxy traffic.

### Azure Virtual Network Manager (AVNM)
* Centrally manage **connectivity** (hub-spoke or mesh peering at scale), **security admin rules** (org-wide rules evaluated **before NSGs**), and **routing** across subscriptions using network groups.

---
## 12. Exam Scenario → Answer (and *why*)

|Scenario|Answer|Why that one|
|---|---|---|
|"VMs in two VNets in different regions must talk privately with the lowest latency"|**Global VNet peering**|Backbone, no gateway, highest bandwidth. VNet-to-VNet VPN adds gateways and IPsec overhead — only pick it if encryption is mandated|
|"Spoke1 and Spoke2 are both peered to the Hub but can't reach each other"|Peering is **non-transitive** → NVA/Firewall in hub + **UDRs** + **Allow forwarded traffic**, or peer the spokes directly|Peering never chains through a third VNet|
|"Spokes must use the hub's VPN gateway to reach on-prem"|Hub: **Allow gateway transit**. Spokes: **Use remote gateways**|Both sides are required; spokes must not have their own gateway; Basic SKU doesn't support it|
|"Peering shows **Initiated**"|Create the peering link from the **other** VNet|Only one of the two links exists|
|"Peering shows **Disconnected**"|**Delete** the remaining link and **recreate both**|One link was deleted; it can't be repaired in place|
|"Added an address range to a peered VNet; peers can't reach it"|**Sync** the peering on every remote peer|Remote side still has the old prefixes|
|"Force all internet-bound traffic through Azure Firewall"|Route table: **`0.0.0.0/0` → Virtual appliance → firewall private IP**, associated with workload subnets|UDR overrides the system `0.0.0.0/0 → Internet` route|
|"Traffic to the NVA is dropped even though the UDR is correct"|Enable **IP forwarding** on the NVA's NIC (and in the OS)|Azure drops packets not addressed to the NIC unless forwarding is on|
|"Block a subnet from reaching a specific range entirely"|UDR with next hop **None** (or NSG deny)|`None` drops at the routing layer|
|"Which route wins: `10.0.0.0/16` UDR or `10.0.0.0/24` system?" for `10.0.0.5`|The **/24**|**Longest prefix match** is evaluated before route source|
|"Same prefix exists as UDR, BGP and system route"|**UDR**|Tiebreak order UDR > BGP > System|
|"Group web VMs and allow only them to reach SQL on 1433"|**ASGs** in an NSG rule|No IP maintenance; NICs added to the ASG inherit the rule|
|"Subnet NSG allows RDP but RDP still fails"|Check the **NIC NSG** — falls through to **DenyAllInBound**|Inbound must pass **both** subnet then NIC NSGs|
|"Find which NSG rule is blocking traffic"|**IP flow verify** (or effective security rules / NSG diagnostics)|Returns Allow/Deny **and the rule name**|
|"Find why traffic goes to the wrong place"|**Next hop** / effective routes|It's a routing problem, not a filtering one|
|"Admins need RDP/SSH to VMs with no public IPs, through the portal"|**Azure Bastion**|TLS 443 from the browser, VMs stay private|
|"One Bastion in the hub for VMs in peered spokes"|Bastion **Basic or higher**|Developer SKU doesn't support peering|
|"Use the native RDP client / upload files / shareable link"|Bastion **Standard**|Native client, file transfer and shareable links start at Standard|
|"Record admin sessions for compliance"|Bastion **Premium**|Session recording is Premium-only|
|"Only VMs in Subnet A may reach a storage account; free; no on-prem"|**Service endpoint** + VNet rule on the storage firewall|Free, simple; DNS stays public|
|"Reach Azure SQL from on-prem over ExpressRoute with a private IP; stop exfiltration to other servers"|**Private endpoint** + Private DNS zone|Only PEs give a routable private IP reachable from on-prem, scoped to **one** resource|
|"Private endpoint created but clients still hit the public IP"|Link the **`privatelink.*` Private DNS zone** to the VNet (or fix DNS forwarding)|DNS is still resolving the public record|
|"NSG rules on the PE subnet aren't taking effect"|Enable **private endpoint network policies** on the subnet|They're disabled by default|
|"Host `contoso.com` (apex) on an Azure Front Door / Traffic Manager / public IP"|**Alias record** at `@`|CNAME isn't allowed at the apex; alias also auto-updates|
|"Move DNS for `contoso.com` to Azure"|Create the public zone → update **NS records at the registrar**|Delegation is done at the registrar, not in Azure|
|"VMs should automatically get DNS names in `corp.contoso.com`"|**Private DNS zone** + VNet link with **autoregistration**|Records created/removed with VM lifecycle|
|"VMs in a VNet using custom DNS can't resolve the private zone"|Conditional forwarder to **`168.63.129.16`** or **DNS Private Resolver**|Custom DNS bypasses linked private zones|
|"Load-balance TCP 1433 across SQL VMs inside the VNet"|**Internal Standard Load Balancer**|L4, private frontend|
|"Route `/images` and `/video` to different pools; need WAF"|**Application Gateway v2 (WAF_v2)**|Path-based routing and WAF are L7 features LB doesn't have|
|"Same client must always hit the same VM" (L4)|LB session persistence **Client IP**|2-tuple hash keeps the client on one backend|
|"Load-balance every port for an NVA pair"|**HA ports** on an **internal** Standard LB|Port 0 / All protocols; internal only|
|"Backends healthy but public LB unreachable"|Add an **NSG allow** for the data port|Standard LB is **secure by default**|
|"All backends probed down after hardening NSGs"|Allow **`AzureLoadBalancer`** on the probe port|A custom deny above 65001 blocks the probe from `168.63.129.16`|
|"New VMs in a new VNet can't reach the internet"|Add a **NAT gateway** (or public IP / outbound rule)|New VNets have **private subnets** — no default outbound access|
|"Intermittent outbound failures under load"|**NAT gateway** (SNAT exhaustion)|Up to 16 IPs × 64,512 ports|
|"Partner must allow-list a predictable range of your public IPs"|**Public IP prefix**|Contiguous, static block|
|"Domain controller IP must never change"|**Static private IP** set on the **NIC in Azure**|Not inside the guest OS|

---
## 13. Portal Practice That Pays Off

- [ ] Create a VNet with two subnets, then add an address range and resize a subnet
- [ ] Peer two VNets, delete one side to see **Disconnected**, then fix it; change an address space and **Sync**
- [ ] Build hub-spoke: NVA (IP forwarding on) in hub, UDRs in spokes, **Allow forwarded traffic** — test spoke-to-spoke
- [ ] Create a route table with a `0.0.0.0/0 → None` route and check **Next hop** and **Effective routes**
- [ ] Put NSGs on both subnet and NIC, block RDP at the NIC, and diagnose with **IP flow verify** and **effective security rules**
- [ ] Create ASGs for web/app tiers and write NSG rules with them
- [ ] Deploy **Bastion Developer**, upgrade to **Basic**, connect to a VM in a peered VNet
- [ ] Add a **service endpoint** for Storage on a subnet and a VNet rule on a storage account
- [ ] Create a **private endpoint** for blob with Private DNS integration; `nslookup` the account from a VM before and after
- [ ] Create a public DNS zone, add A, CNAME and an **alias** record to a public IP
- [ ] Create a **Private DNS zone**, link a VNet with autoregistration, deploy a VM and watch the record appear
- [ ] Deploy a **public Standard LB** with two VMs, a health probe and an NSG; break the probe and watch **Health Probe Status**
- [ ] Create a NAT gateway on a private subnet and confirm outbound works

---
## 14. One-Line Exam Facts

* **5 reserved IPs** per subnet: `.0`, `.1`, `.2`, `.3`, last
* Smallest subnet **/29 = 3 usable**; IPv6 subnets are exactly **/64**
* VNets are **regional** and **single-subscription**; no broadcast/multicast
* **Restart VMs** after changing VNet DNS servers
* Set static private IPs **on the NIC in Azure**, never in the guest OS
* **Standard public IP = static only, secure by default, zone-capable**; Basic retired 30 Sept 2025
* **Global tier** public IP → cross-region LB; no availability zones
* New VNets (API after 31 March 2026) → **private subnets**, need **explicit outbound**
* **NAT gateway**: 16 IPs × 64,512 SNAT ports, takes precedence for outbound
* Peering: **non-transitive**, **non-overlapping**, cross-subscription and **cross-tenant**, regional or global
* Peering status: **Initiated** = one link, **Disconnected** = recreate both
* Peered address space change → **Sync** each remote peering
* Gateway transit: hub **Allow gateway transit**, spoke **Use remote gateways**, only **one** remote gateway peering, **not Basic SKU**
* **`VirtualNetwork`** service tag includes peered VNets
* System routes: `0.0.0.0/0 → Internet`; RFC 1918 + `100.64.0.0/10 → None`
* UDR next hops: **Virtual appliance, Virtual network gateway, Virtual network, Internet, None**
* **Longest prefix first**, then **UDR > BGP > System**
* **400 routes** per route table; **one route table per subnet**; same region + subscription
* NVA needs **IP forwarding on the Azure NIC** + in the OS
* Never disable gateway route propagation on **GatewaySubnet**
* NSG priority **100–4096**, **lower wins**, first match stops; **1,000 rules** per NSG
* Defaults: **65000** AllowVNet, **65001** AllowAzureLoadBalancer / AllowInternetOutBound, **65500** DenyAll
* Inbound: **subnet NSG → NIC NSG**; outbound: **NIC NSG → subnet NSG**; both must allow
* ASG NICs must be in the **same VNet**; max **10 ASGs** per rule side
* **IP flow verify** = which rule; **Next hop** = which route; **Connection troubleshoot** = point-in-time; **Connection monitor** = continuous
* **NSG flow logs** → **VNet flow logs** (no new NSG flow logs since 30 June 2025, retire 30 Sept 2027)
* Bastion subnet: **`AzureBastionSubnet`, /26+, no UDRs**; Bastion **upgrade only**
* Bastion: Developer free/1 VM/no peering → Basic peering → **Standard native client, shareable link, IP-connect, scaling** → **Premium recording, private-only**
* Service endpoint: **subnet-level, all instances, DNS stays public, no on-prem**, free
* Private endpoint: **private IP, one resource, DNS `privatelink.*`, on-prem OK**; network policies **off by default**
* `Microsoft.Storage.Global` and `Microsoft.Storage` **can't coexist** on one subnet
* Azure DNS: **delegate via registrar NS records**; **no CNAME at apex**; **alias** fixes it (A/AAAA/CNAME)
* Private DNS: **one registration zone per VNet**, **100 registration VNets** / **1,000 links** per zone, **VMs only, no PTR**
* Custom DNS overrides private zones → forward to **`168.63.129.16`** or use **DNS Private Resolver**
* LB: **L4**; probes **TCP/HTTP/HTTPS** from **`168.63.129.16`**; HTTP probe needs **200**
* LB distribution: **5-tuple** default, **Client IP = 2-tuple**, **Client IP + protocol = 3-tuple**
* **HA ports = internal Standard LB only**, port 0, protocol All
* Standard LB **secure by default** → NSG must allow data traffic
* Backend VM can't reach its **own ILB frontend** (hairpin)
* **App Gateway v1 retired 28 April 2026** — v2 only; L7, path-based, multi-site, WAF
* Regional HTTP → **App Gateway**; global HTTP → **Front Door**; regional L4 → **Load Balancer**; global any-protocol DNS → **Traffic Manager**
