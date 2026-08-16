**Definition**
- **Gateway transit** = allows a peered VNet to use another VNet's **VPN Gateway or Azure Route Server**.
- Common in **hub-and-spoke** networks.
---
#### Gateway Transit
**Hub VNet**
- Contains the **VPN Gateway / Route Server**
- Enable **Virtual network gateway or Route Server**
- Means: **"Other peered VNets can use my gateway."**

**Spoke VNet**
- Enable **Remote virtual network gateway or Route Server**
- Means: **"I want to use the remote VNet's gateway."**

**Both settings are required:**
- Hub → **Allow gateway transit**
- Spoke → **Use remote gateway**

---
#### VNet Peering settings

- **Traffic to remote virtual network** → allows traffic to remote VNet
- **Traffic forwarded from remote virtual network** → allows forwarded traffic from remote VNet
- **Virtual network gateway or Route Server** → allows peered VNets to use this VNet's gateway
- **Remote virtual network gateway or Route Server** → allows this VNet to use the remote VNet's gateway

---
#### VPN Gateway
- VNet can have **one VPN Gateway**
- Gateway transit supports **regional + global peering**
- Shared gateway can provide:
    - **Site-to-Site VPN** → on-premises
    - **VNet-to-VNet** → another VNet
    - **Point-to-Site VPN** → client device
- Avoids deploying a VPN Gateway in every VNet
---
#### Gateway Transit Benefits
- **Centralised connectivity**
- **Lower cost** → fewer VPN Gateways
- Simpler **hub-and-spoke** architecture
- Peered VNets can access resources **outside the peering**
- No need for a separate VPN Gateway in each spoke

---
#### NS
- **NSGs** can allow/block traffic between peered VNets and subnets.
- Peering does **not bypass NSG rules**.

---

#### Exam Facts
Hub → Allow gateway transit
Spoke → Use remote gateway
- **Gateway transit** = share the gateway
- **Remote gateway** = use the shared gateway
- **Peering** = private connectivity between VNets
- **Gateway transit** = access beyond the peered VNet via its gateway