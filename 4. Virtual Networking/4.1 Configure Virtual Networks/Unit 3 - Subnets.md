**Azure Subnet** = logical division of a **VNet** using a range of IP addresses.
- Subnet address range must be within the VNet **CIDR**.
- Subnet ranges **cannot overlap**.
- Each subnet uses **CIDR notation**. 
- Azure routes traffic between subnets **by default**.

---
#### Reserved IP Addresses
Reserves 5 IP addresses per subnet: First 4 addresses & Last address

Example `192.168.1.0/24`:
- `.0` → VNet address
- `.1` → Default gateway
- `.2` & `.3` → Azure DNS
- `.255` → Broadcast address

---
#### Subnet Considerations
- **VPN Gateway** → requires a dedicated **[[GatewaySubnet]]**.
- **Network Virtual Appliance (NVA)** → place resources in different subnets if traffic must pass through the NVA.
- **NSG** → each subnet can have **0 or 1 NSG**.
- **Private Link** → provides private connectivity to Azure services without exposing traffic to the **public internet**.