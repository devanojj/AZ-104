#### When NOT to Use
- Web application doesn't need load balancing
- Low traffic handled by existing infrastructure
- No need for a **backend pool**
---
#### Exam Tips
- **On-premises backend + web routing → Application Gateway**
- **TLS offload → Application Gateway**
- **SQL injection / XSS → Application Gateway WAF**
- **Local session state → Session affinity**
- **Global web app → Front Door**
- **DNS-based routing → Traffic Manager**
    - Failover can be slower due to **DNS caching/TTL**
- **TCP/UDP + ultra-low latency → Load Balancer**
- **Load Balancer is regional**, not global
- **Front Door is global**
- **Application Gateway is regional**