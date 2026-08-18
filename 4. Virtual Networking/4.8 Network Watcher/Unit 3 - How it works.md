Azure service for monitoring and troubleshooting **IaaS network resources**.
- Automatically available when a **VNet is created in an Azure region** in your subscription.
- Access via Azure Portal → Search → **Network Watcher**.
---
#### Topology
**Purpose:** Visualise resources in a VNet and their relationships.
**Resources shown:** Subnets, NICs, NSGs, Load Balancers + health probes, Public IPs, VNet peering, Virtual network gateways, VPN gateway connections, VMs, VM Scale Sets

**Resource properties:**
- **Name** → resource name
- **ID** → resource URI
- **Location** → Azure region
- **Associations** → relationships between resources
    - `Contains`
    - `Associated`
---
#### Connection Monitor
**Purpose:** **Continuous, end-to-end connectivity monitoring**.
- Supports:
    - Azure
    - Hybrid/on-premises environments
- Measures **latency** between resources.
- Detects connectivity changes:
    - Network configuration changes
    - NSG rule changes
- Probes VMs at regular intervals.
- Diagnoses problems and provides recommended fixes.
- Requires monitoring agents on monitored hosts.
- Azure VMs → install **Network Watcher Agent VM extension**.

**Exam Tip:**
- **Connection Monitor = continuous monitoring**
- **Connection Troubleshoot = point-in-time test**
---
#### IP Flow Verify
**Purpose:** Determine whether VM traffic is **Allowed or Denied**.
Uses **5-tuple** information:
- Protocol → TCP/UDP
- Local IP
- Remote IP
- Local port
- Remote port

**Exam Tip:**  
**IP Flow Verify → Is traffic allowed/denied + why?**

---
#### Next Hop
**Purpose:** Troubleshoot **routing**.
- Determines the next hop for traffic from a VM/NIC.
- Returns: Next-hop type, Next-hop IP, Associated route table
- Identifies incorrect routing to: On-premises, Virtual appliance, Nowhere

**Routes:**
- User-defined route → returns **User Route**
- Otherwise → **System Route**

**Exam Tip:**  
**Next Hop = routing problems**

---
#### Effective Security Rules
**Purpose:** Determine why traffic is **allowed or denied**.
- Considers **all NSGs** applied to the resource.
- Evaluates rules based on: Source IP, Destination IP, Ports
- Shows the combined/effective rules affecting the resource.

**Exam Tip:**  
**Effective Security Rules = what NSG rules actually apply?**

---
#### Packet Capture
**Purpose:** Remotely capture network traffic from a VM.
- Uses a **VM extension**.
- Can be triggered through: Azure Portal, PowerShell, Azure CLI, REST API
- Supports **5-tuple filters**: Protocol, Local IP, Remote IP, Local port, Remote port
- Captured data stored: Local disk, Azure Storage Blob

**Exam Tip:**  
**Packet Capture = inspect actual network packets**

---
#### Connection Troubleshoot
**Purpose:** Test **TCP connectivity** between a source and destination.
Destination can be: VM, FQDN, URI, IP address

**Successful connection provides:**
- Latency (ms)
- Probe packets sent
- Number of hops

**Common failure types:**

|Fault|Meaning|
|---|---|
|**CPU**|High CPU utilization|
|**Memory**|High memory utilization|
|**GuestFirewall**|Firewall outside Azure blocked traffic|
|**DNSResolution**|Destination IP couldn't be resolved|
|**NetworkSecurityRule**|NSG blocked traffic|
|**UserDefinedRoute**|Incorrect user-defined route|

---
#### VPN Troubleshoot
**Purpose:** Diagnose **VPN gateways and connections**.
- Available through: Azure Portal, PowerShell, Azure CLI, REST API
- Checks gateway/connection health.
- Long-running diagnostic operation.

**Results include:**
- `startTime` → troubleshooting start
- `endTime` → troubleshooting end
- `code` → `UnHealthy` if a diagnosis fails
- `results` → detected faults
    - Fault ID
    - Summary
    - Detailed description
    - Recommended actions
    - Documentation links
---
#### Exam Facts
- **Topology** → visualise network resources/relationships
- **Connection Monitor** → continuous connectivity + latency
- **IP Flow Verify** → allowed/denied traffic + NSG rule
- **Next Hop** → routing diagnosis
- **Effective Security Rules** → all effective NSG rules
- **Packet Capture** → capture network packets
- **Connection Troubleshoot** → point-in-time TCP connectivity test
- **VPN Troubleshoot** → VPN gateway/connection health
- **5-tuple** → Protocol + Local IP + Remote IP + Local Port + Remote Port