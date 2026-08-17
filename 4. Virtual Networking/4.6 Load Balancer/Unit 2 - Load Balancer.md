Distributes incoming network traffic across multiple **VMs or VM Scale Set instances**.
- Prevents a single server from becoming overloaded
- Improves **availability** and network performance
- **Load-balancing rules** → determine traffic distribution
- **Health probes** → check backend health and avoid unhealthy instances
- Supports **TCP and UDP**
- Scales to **millions of flows**
---
#### Public Load Balancer
**Purpose:** Load balances **internet traffic** to Azure VMs.
- Public IP + port → mapped to private IP + port of backend VM
- Example: distribute web traffic across multiple web servers
- Can provide **outbound connectivity** for VMs
---
#### Internal Load Balancer
**Purpose:** Load balances **private/internal traffic** using private IPs.
- Front-end isn't exposed directly to the internet
- Used for Azure or **on-premises → Azure** traffic
- Common for **multi-tier applications**
    - Front-end web VMs → backend processing VMs
- Supports:
    - **Within VNet**
    - **Cross-premises**
    - **Multi-tier applications**
    - **Line-of-business (LOB) applications**
---
#### Exam Tips
- **Public LB → internet-facing**
- **Internal LB → private/internal traffic**
- **Health probe → determines healthy backend instances**
- **Load-balancing rule → determines how traffic is distributed**
- Both types support **inbound + outbound** scenarios.