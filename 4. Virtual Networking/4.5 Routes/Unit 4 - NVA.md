Virtual appliance/VM that **routes and inspects network traffic**.
- Functions: **Firewall, router, IDS/IPS, proxy, load balancer, WAN optimizer**
- Azure Marketplace: Cisco, Check Point, Barracuda, Sophos, WatchGuard, SonicWall
- Used to **block unauthorized/malicious traffic** and control traffic between networks/subnets
- Can inspect **Layer 4** traffic; application-aware NVAs can inspect **Layer 7**
---
#### NVA Traffic Flow
- Acts as a **router** between subnets
- Traffic is routed through NVA → inspected → forwarded if allowed
- **IP forwarding** must be enabled on relevant NVA NICs
- Some NVAs use multiple NICs:
    - Management NIC
    - Traffic-processing NICs
---
#### Microsegmentation
- Dedicated **firewall/NVA subnet**
- Web/apps placed in separate subnets
- Traffic forced through NVA for inspection
- Provides more granular security and limits lateral movement
---
#### User-Defined Routes (UDRs)
**Definition:** Custom routes that control where Azure sends traffic.
**Uses:**

- Route traffic through an **NVA**
- **Forced tunneling** → send internet traffic through on-premises network

**Exam Facts:**

- Azure **system routes** provide default routing
- Route tables contain UDRs
- One route table → **multiple subnets**
- One subnet → **only one route table**
- UDRs can override default routing behaviour
---
#### NVA High Availability
- NVA can become a **single point of failure**
- Use **highly available NVA architecture** when traffic depends on it.


