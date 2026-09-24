**Connects Azure VNets privately over the Microsoft backbone.**

- **Same region** → VNet Peering
- **Different regions** → Global VNet Peering
- Requires **non-overlapping IP address spaces**
- Uses **private IPs** — no VPN required
- **Not transitive** by default
- Peering status → **Connected**

**AZ-104:** `VNet ↔ VNet = Peering`