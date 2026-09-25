Distributes incoming network traffic across multiple **VMs or VM Scale Set instances**.
- Prevents a single server from becoming overloaded
- Improves **availability** and network performance
- **Load-balancing rules** → determine traffic distribution
- **Health probes** → check backend health and avoid unhealthy instances
- Supports **TCP and UDP**
- Scales to **millions of flows**
- Defaults to 5 Tuple
---
**==Public Load Balancer==**
Load balances internet traffic to Azure VMs.

---
**==Internal Load Balancer==**
Load balances private/internal traffic using private IPs.
- Front-end isn't exposed directly to the internet
- Used for Azure or on-premises → Azure traffic
- Multi-tier applications : Front-end web VMs → backend processing VMs

- ---
#### Exam Tips
- Health probe → determines healthy backend instances
- Load-balancing rule → determines how traffic is distributed
- Both types support inbound + outbound scenarios.
- Load Balancer Health Probes, determine which VMs are healthy

*Troubleshoot Load Balancer*
Intermittent connectivity issues : Check health probe + verify SKU's match for public IP & load balancer 

Why? : Inactive or incorrectly configured probe can lead to traffic being routed to unhealthy instances, causing connectivity issues. 
Mismatched SKUs can disrupt proper operation and lead to connectivity problems.

What not to do : Changing the load balancer's distribution mode, does not resolve the underlying configuration problems

The SKU for public IP and load balancer need to match due to being different kinds like (Basic or Standard) They both just need to be the same