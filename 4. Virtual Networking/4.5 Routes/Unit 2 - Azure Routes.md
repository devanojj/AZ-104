Azure routing determines how network traffic moves between subnets, VNets, on-premises networks, and the internet.
- **System routes** = automatically created by Azure
- **User-defined routes (UDRs)** = custom routes created by you
- **BGP routes** = routes learned from on-premises/network gateways

- VNet peering = connects two VNets
- Azure routes = control where traffic goes inside/between networks

---

#### System Routes
- Automatically assigned to every subnet
- **Cannot be created/deleted**
- Can be **overridden by UDRs**
- Allow Azure VMs to communicate by default

|Address Prefix|Next Hop|
|---|---|
|VNet address space|**Virtual network**|
|`0.0.0.0/0`|**Internet**|
|`10.0.0.0/8`|**None**|
|`172.16.0.0/12`|**None**|
|`192.168.0.0/16`|**None**|
|`100.64.0.0/10`|**None**|

**Next-hop types:**
- **Virtual network** → traffic stays within VNet
- **Internet** → traffic goes to internet
- **None** → traffic is dropped

---

#### Additional System Routes
Azure can create additional routes when using:

- **VNet peering**
- **Service chaining**
- **Virtual network gateway**
- **Service endpoints**

---

#### VNet Peering & Service Chaining

- **VNet peering** → connects VNets
- Can be **same-region or cross-region**
- Adds routes to route tables
- **Service chaining** → allows traffic to be forced through an NVA/firewall between networks
- UDRs can override routes to force traffic through:
    - **NVA**
    - **Azure VPN Gateway**

---

#### Virtual Network Gateway

- Connects Azure ↔ on-premises
- Supports encrypted traffic over the internet
- Used with:
    - **VPN**
    - **ExpressRoute**
- Contains routing tables and gateway services

---

#### Service Endpoints

- Extend Azure service access into a VNet's **private address space**
- Allows VMs to access supported Azure services directly
- Can restrict service access to selected VNets
- Creating a service endpoint causes Azure to add routes

---

#### User-Defined Routes (UDRs)

**Definition:** Custom routes used to override system routes and control traffic flow.

**Common uses:**

- Force traffic through an **NVA**
- Force traffic through a **firewall**
- Block traffic
- Override default routing

**Next-hop types:**

|Next Hop|Purpose|
|---|---|
|**Virtual appliance**|Send traffic to NVA/firewall|
|**Virtual network gateway**|Send traffic through VPN gateway|
|**Virtual network**|Route within VNet|
|**Internet**|Send traffic to internet|
|**None**|**Drop traffic**|

**Exam fact:**

- Virtual appliance → specify the appliance's **private IP**
- NVA NIC must have **IP forwarding enabled**
- Internal Load Balancer private IP can also be used as a virtual appliance next hop
- UDRs **cannot** use `VirtualNetworkServiceEndpoint` as a next-hop type

---

#### Service Tags in UDRs

- Service tag = group of Azure service IP prefixes
- Microsoft manages the underlying IP ranges
- Automatically updated when Azure IPs change
- Reduces the number of UDRs required

---

#### Border Gateway Protocol (BGP)

**Definition:** Routing protocol used to exchange routes between networks.

- Exchanges routes between **Azure and on-premises**
- Common with:
    - **ExpressRoute**
    - **Site-to-site VPN**
- On-premises gateway can advertise routes to Azure
- Helps provide **dynamic routing** and resilience

**Exam Tip:**

- **UDR = manually define routing**
- **BGP = dynamically exchange routing information**

---

#### Route Selection

Azure uses **longest prefix match** when multiple routes exist.

Example:

- `10.0.0.0/16`
- `10.0.0.0/24`

Traffic to `10.0.0.2` → **`/24` wins** because it is more specific.

**Rule:**

> **Longest prefix = most specific route = selected first**

- Cannot create multiple UDRs with the same address prefix.

---

#### Route Priority

If routes have the **same address prefix**:

1. **User-defined route (UDR)**
2. **BGP route**
3. **System route**

**Exam Facts**

- **UDR > BGP > System**
- **Longest prefix match** is considered first
- UDRs can override system routes
- **None = drop traffic**
- **Virtual appliance = NVA/firewall**
- **BGP = dynamic route exchange**