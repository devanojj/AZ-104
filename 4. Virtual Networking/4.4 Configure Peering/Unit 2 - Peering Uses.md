- Connects two **Azure VNets** so resources can communicate privately.
- Peered VNets operate like **one network** for connectivity.
---
#### Types
- **Regional peering** → VNets in the **same region**
- **Global peering** → VNets in **different regions**
**Exam Fact**
- Can peer across **subscriptions and tenants**.
---
#### Key Benefits
- Traffic stays on the **Azure backbone**
- **No public Internet** or gateway required
- **Low latency + high bandwidth**
- No downtime when creating peering
- Supports communication across **regions/subscriptions**
---
#### Requirements & Limitations
- VNet address spaces **must not overlap**
- Changing a VNet address space → **delete peering → change address space → recreate peering**
- **Basic Load Balancer** doesn't support cross-region peered VNet access
    - Use **Standard Load Balancer**
- Azure default DNS **doesn't resolve across peered VNets**
    - Use **Azure Private DNS** or custom DNS
---