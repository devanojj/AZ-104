- Routes traffic using **hostnames + URL paths**
- Regional service
- Can include **WAF**, TLS/SSL termination, autoscaling
---
#### Core Components
- **Front-end IP** → receives client requests
    - 1 public IP + 1 private IP maximum
- **Listener** → accepts traffic based on:
    - Protocol
    - Port
    - Hostname
    - IP address
    - **Basic** or **Multi-site**
- **Routing rule** → connects listener → backend pool
- **Backend pool** → servers receiving traffic
- **HTTP settings** → protocol, session stickiness, connection draining, timeout, health probes
---
#### Backend Pools
Can contain:
- **VMs**
- **VM Scale Sets**
- **Azure App Service**
- **On-premises servers**
- Uses **round-robin** load balancing
- Servers should be configured consistently
- Health probes remove unhealthy servers from rotation
---
#### Routing
**Path-based routing**
- Routes based on URL path
- `/video/*` → video servers
- `/images/*` → image servers

**Multi-site routing**
- Routes based on **hostname/domain**
- `contoso.com` → Pool A
- `fabrikam.com` → Pool B
- Useful for **multitenant applications**

---
#### Web Application Firewall (WAF)
- Optional component
- Inspects requests **before they reach the listener**
- Protects against common **OWASP** threats:
    - SQL injection
    - Cross-site scripting (XSS)
    - Command injection
    - Request smuggling
    - Bots/scanners
- Uses **OWASP Core Rule Set (CRS)**
- Can customize rules and request inspection

---
#### TLS/SSL
**TLS termination**
- Gateway decrypts traffic
- Offloads CPU-intensive TLS processing from backend servers
- Backend servers don't need TLS certificates
---
#### Health Probes
- Determines whether backend servers are healthy
- HTTP status **200–399 = healthy**
- Unhealthy servers don't receive traffic
- Default probe waits **30 seconds** before marking a server unavailable
---
#### Security
- Internet connects to **Application Gateway**, not directly to backend servers
- Only expose **port 80/443** on the gateway
- Reduces backend servers' **attack surface**
---
#### Additional Features
- **Redirection** → HTTP → HTTPS / another site
- **HTTP header rewrite**
- **Custom error pages**
- **Autoscaling** → scales with traffic
- Native **WebSocket + HTTP/2** support

---
#### Exam Tips
- **L7 + web application → Application Gateway**
- **WAF → Application Gateway**
- **URL path routing → Application Gateway**
- **Hostname/multi-site routing → Application Gateway**
- **TLS termination → Application Gateway**
- **L4 + TCP/UDP + ultra-low latency → Load Balancer**
- **Global web application → Front Door**
- **DNS-based global routing → Traffic Manager**