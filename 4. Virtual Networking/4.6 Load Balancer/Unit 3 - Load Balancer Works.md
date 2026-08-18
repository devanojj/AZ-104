**Layer 4** load balancer using TCP/UDP properties to distribute traffic.

- Uses **IP addresses, ports and protocol**
- Cannot inspect application content → use **Application Gateway** for Layer 7

---

#### Core Components

- **Front-end IP** → client-facing **public or private IP**
- **Load-balancing rule** → maps frontend IP/port → backend IP/port
- **Back-end pool** → VMs / VM Scale Set instances receiving traffic
- **Health probe** → removes unhealthy instances from new traffic
- **Inbound NAT rule** → maps frontend port → specific VM
- **HA ports** → load balance all TCP/UDP ports
- **Outbound rule** → provides **SNAT** for outbound connections

---

#### Load-Balancing Rules

Uses a **5-tuple hash**:

1. Source IP
2. Source port
3. Destination IP
4. Destination port
5. Protocol (**TCP/UDP**)

- Supports multiple ports and frontend IPs
- Automatically redistributes traffic when backend instances are added/removed

---

#### Health Probes

Determines whether a backend instance can receive **new connections**.

- **TCP** → checks successful TCP connection
- **HTTP/HTTPS** → expects **HTTP 200**
- Failed probe → no new connections sent
- Existing connections **continue** until ended, idle timeout, or VM shutdown

---

#### Session Persistence

**Session affinity/stickiness:** Keeps a client's connections going to the same backend VM.

- **None (default)** → any healthy VM
- **Client IP (2-tuple)** → Source IP + Destination IP
- **Client IP + Protocol (3-tuple)** → Source IP + Destination IP + Protocol

---

#### High Availability (HA) Ports

- Rule: **Protocol = All + Port = 0**
- Load balances **all TCP/UDP ports**
- Decision made per **5-tuple flow**
- Useful for **NVAs** and scenarios requiring many ports

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
- **5-tuple = Source IP + Source Port + Destination IP + Destination Port + Protocol**
- **HA ports = All protocols + Port 0**
- **Session persistence = same client → same backend VM**