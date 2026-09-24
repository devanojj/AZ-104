**Azure Virtual Network (VNet)** = logical isolation of Azure resources that enables secure network communication.
- Each VNet has its own **CIDR address space**.
- Can connect to other VNets and **on-premises networks**.
- Supports **subnets** and custom **DNS** settings.
- Connected networks must have **non-overlapping CIDR ranges**.

---
#### Common Uses
- **Cloud-only network** → private communication between Azure resources.
- **Extend datacenter** → **Site-to-Site VPN** using **IPsec**.
- **Hybrid cloud** → connect Azure applications to on-premises systems.
- **Subnets** segment a VNet.



