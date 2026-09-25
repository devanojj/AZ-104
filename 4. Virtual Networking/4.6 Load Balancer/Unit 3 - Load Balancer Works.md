**Layer 4** load balancer using TCP/UDP properties to distribute traffic.
Cannot inspect application content → use Application Gateway for Layer 7

---
#### Core Components
- **Front-end IP** → client-facing **public or private IP**
- **Load-balancing rule** → maps frontend IP/port → backend IP/port
- **Back-end pool** → VMs / VM Scale Set instances receiving traffic
- **Health probe** → removes unhealthy instances from new traffic
- **Inbound NAT rule** → maps frontend port → specific VM
- **[[HA ports]]** → load balance all TCP/UDP ports
- **Outbound rule** → provides **SNAT** for outbound connections

---
#### Health Probes
Determines whether a backend instance can receive **new connections**.
- **TCP** → checks successful TCP connection
- **HTTP/HTTPS** → expects **HTTP 200**
- Failed probe → no new connections sent
- Existing connections **continue** until ended, idle timeout, or VM shutdown

---
#### NAT & Outbound
**Inbound NAT:**
- Maps load balancer public IP/port → specific VM
- Example: Public port → **TCP 3389 (RDP)**

**Outbound rule:**
- Uses **SNAT**
- Allows backend VMs to access the **internet/public endpoints**
---

#### Exam Facts
- **Load Balancer = Layer 4**
- **Application Gateway = Layer 7**
- **Health probe failure → stops NEW connections**
- **HA ports = All protocols + Port 0**
- **Session persistence = same client → same backend VM**