**Definition**
- VNet peering is **non-transitive**.
- Peering does **not automatically pass through** another VNet.
- A ↔ B = works
- B ↔ C = works
- A ↔ C = **doesn't work** unless separately configured
---

#### Extending VNet Peering
**Hub-and-Spoke**
- **Hub** contains shared networking services:
    - **NVA**
    - **VPN Gateway**
- Spokes peer with the hub.
- Traffic can be routed through the hub.
---
#### User-Defined Routes (UDR)
- Custom routes controlling **where traffic goes**.
- Next hop can be:
    - VM in a **peered VNet**
    - **VPN Gateway**
---
#### Service Chaining
**Definition**

- Routes traffic through a **network appliance or gateway** in another VNet.
- Uses **UDRs**
- UDR next hop → [[NVA]]/VM or VPN Gateway in peered VNet

VNet A → UDR → NVA in Hub → Destination

---
#### Azure Virtual Network Manager
- Centrally manages VNet **peering/topologies**.
- Supports:
    - **Hub-and-spoke**
    - **Mesh**
- Automates peering across multiple VNets.

---
#### Exam Facts
- **VNet peering = non-transitive**
- A ↔ B ↔ C ≠ A ↔ C
- **Hub-and-spoke** → central networking
- **UDR** → control traffic path
- **Service chaining** → route through NVA/gateway
- **VNet Manager** → centrally manage peering at scale