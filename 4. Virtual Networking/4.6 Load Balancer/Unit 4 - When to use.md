- Replacing **on-premises hardware load balancers**
- Applications with **multiple VM tiers**
- High-volume / low-latency applications
- Need **health probes** → remove failed VMs from rotation
- Need **session persistence** → client remains with same VM
- Need **Inbound NAT rules** → RDP/SSH access to individual VMs
---
#### When NOT to Use
- Single VM with **low traffic**
- No need for multiple backend VMs
- Need **Layer 7 / web-aware** features:
    - WAF
    - Path-based routing
    - TLS/SSL termination
    - Application-aware routing
---
#### Load Balancer vs Other Services

|Service|Layer|Scope|Use When|
|---|---|---|---|
|**Load Balancer**|**L4**|Regional|TCP/UDP, ultra-low latency, VM load balancing|
|**Application Gateway**|**L7**|Regional|Web apps, **WAF**, TLS termination, path-based routing|
|**Front Door**|**L7**|Global|Global web apps, acceleration, WAF, fast failover|
|**Traffic Manager**|**DNS**|Global|DNS-based routing between global endpoints|

---
#### Application Gateway
- **Regional L7** load balancer
- Best for **web applications**
- **WAF**
- TLS/SSL termination
- Path/host-based routing
- Can offload CPU-intensive TLS processing
**Exam:** Web app + **WAF/path routing** → **Application Gateway**

---
#### Front Door
- **Global L7** application delivery
- Global load balancing + site acceleration
- **WAF**
- TLS/SSL offload
- Path-based routing
- Caching
- **Fast failover**
**Exam:** Web app deployed across **multiple Azure regions** → **Front Door**

---
#### Traffic Manager
- **DNS-based** global load balancing
- Routes users between Azure regions/endpoints
- Operates at **domain/DNS level**
- Failover can be slower due to **DNS caching + TTL**

**Exam:** **DNS-based** global routing → **Traffic Manager**

---
#### Exam Facts
- **L4 → Load Balancer → TCP/UDP**
- **L7 → Application Gateway → Regional web apps**
- **L7 → Front Door → Global web apps**
- **DNS → Traffic Manager → Global routing**
- **WAF → Application Gateway / Front Door**
- **Ultra-low latency → Load Balancer**
- **Health probes → Load Balancer**
- **Session persistence → Load Balancer**
- **Inbound NAT → RDP/SSH to specific VM**
- **Public LB → external traffic**
- **Internal LB → internal tier-to-tier traffic**
- **Zone-redundant → high availability across zones**