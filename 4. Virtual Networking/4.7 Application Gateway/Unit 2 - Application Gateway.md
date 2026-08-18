> **Layer 7 load balancer** for managing **HTTP/HTTPS web traffic** to backend web applications.

### Backend Pools
Can route traffic to:
- Azure VMs
- VM Scale Sets
- Azure App Service
- On-premises servers
-----
### Key Features

- **Layer 7** → Makes routing decisions based on application/HTTP traffic.
- **Load balancing** → Uses **round-robin** to distribute requests across backend servers.
- **Session stickiness** → Keeps requests from the same session going to the **same backend server**.
    - Useful for e-commerce/transactions to prevent sessions being disrupted.
- **WAF** → Protects web applications against common web vulnerabilities.
- **TLS/SSL** → Supports encryption:
    - Client ↔ Application Gateway
    - Application Gateway ↔ Backend server
- **Protocols:** HTTP, HTTPS, HTTP/2, WebSocket
- **End-to-end encryption** → Encrypts traffic between client → gateway → backend.
- **Autoscaling** → Automatically adjusts capacity based on traffic.
- **Connection draining** → Gracefully removes backend servers during maintenance/updates without abruptly terminating existing connections.
-------
### AZ-104 Exam Tips

- **Application Gateway = Layer 7**
- **Load Balancer = Layer 4**
- **WAF + HTTP/HTTPS routing = Application Gateway**
- **Session stickiness = same client session → same backend server**