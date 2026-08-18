Azure service for troubleshooting **IaaS network connectivity and performance**.

---
#### Common Uses
- **VM connectivity** troubleshooting
- **VPN connection** troubleshooting
- **Cross-region latency** testing
---
#### VM Connectivity
**Scenario:**  
One IaaS VM cannot establish a PowerShell connection to another VM.

**Tool:** **IP Flow Verify**
- Tests whether traffic is **Allowed or Denied**.
- Specify: Local IP + port, Remote IP + port, TCP/UDP, Inbound/Outbound
- Performs a logical test against network rules.
- Identifies the **NSG rule** blocking traffic.

**Example:**
- PowerShell over HTTPS → **TCP 5986**

**Exam Tip:**  
**IP Flow Verify → Which NSG rule is allowing/denying traffic?**

---
#### VPN Troubleshooting
**Scenario:**  
On-premises → Azure VM connectivity through a **Site-to-Site VPN** fails.

**Tool:** **VPN Troubleshoot**
- Diagnoses **Virtual Network Gateway** connections.
- Checks common gateway/VPN issues.
- Returns: Health diagnosis, Logs, Recommended fixes
- Available through: Azure Portal, PowerShell, Azure CLI

**Exam Tip:**  
**VPN Troubleshoot → VPN gateway/connection issues**

---
#### Cross-Region Latency
**Purpose:** Determine optimal Azure region placement.
- Test latency between VMs in different regions.
- Helps determine whether IaaS resources should:
    - Remain in one region
    - Be distributed across regions
- Useful when designing **multi-region applications**.

**Example:**
- Compare on-premises application → Storage latency.
- Compare Azure VM → same Storage endpoint.
- High latency can indicate:
    - Migrate on-premises workload to Azure
    - Move Azure VM to another region

**Exam Tip:**  
**Network Watcher → latency testing → region placement**

---

#### When NOT to Use Network Watcher
- Primarily for **IaaS networking**.
- Not designed for:
    - **PaaS connectivity troubleshooting**
    - **Web analytics**
- Provides intermediate-level diagnostics.
- Advanced requirements may require **third-party network tools**.
- For Azure service issues → check **Azure Status / Service Health**.
---

#### Exam Facts
- **VM connectivity → IP Flow Verify**
- **VPN issues → VPN Troubleshoot**
- **Cross-region latency → Network Watcher**
- **Network Watcher → primarily IaaS**
- **PaaS/Web analytics → not Network Watcher**
- **Advanced diagnostics → third-party tools**