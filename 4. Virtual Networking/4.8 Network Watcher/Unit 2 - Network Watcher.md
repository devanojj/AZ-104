Azure service for monitoring, diagnosing, and analysing **Azure IaaS network resources**.
Monitors/troubleshoots: VMs, VNets, Application Gateway, Load Balancer
**Not intended for PaaS monitoring or web analytics**

---
#### Tool Categories

| Category                | Tools                                                                                                                          |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| **Monitoring**          | Topology, Connection Monitor                                                                                                   |
| **Network Diagnostics** | IP Flow Verify, NSG Diagnostics, Next Hop, Effective Security Rules, Connection Troubleshoot, Packet Capture, VPN Troubleshoot |
| **Traffic**             | Flow Logs, Traffic Analytics                                                                                                   |

---

#### Monitoring
**Topology**
- Visualises Azure network configuration and resource relationships.
- Can span **subscriptions, resource groups, and locations**.
- Useful for understanding the overall network during troubleshooting.

**Connection Monitor**
- Provides **end-to-end connection monitoring**.
- Monitors network performance between endpoints.
- Supports **Azure and hybrid** environments.
- Continuous monitoring over time.
---
#### Network Diagnostic Tools
**IP Flow Verify**
- Checks whether traffic is **Allowed or Denied**.
- VM-level traffic filtering.
- Identifies the **NSG rule** responsible.
- Supports IPv4 and IPv6.

**NSG Diagnostics**
- Detects traffic filtering issues.
- Supports: VM, VM Scale Set, Application Gateway
- Checks IP addresses, IP prefixes, and service tags.
- Identifies the rule allowing/denying traffic.
- Can add a higher-priority NSG rule.

**Next Hop**
- Troubleshoots **routing issues**.
- Determines where traffic will be routed.
- Provides: Next-hop type, Next-hop IP address, Route table ID

**Effective Security Rules**
- Shows all security rules applied to a **network interface**.
- Includes rules from: NIC, Subnet, Combined/aggregate rules

**Connection Troubleshoot**
- Tests connectivity **at a point in time**.
- Sources can include: VM, VM Scale Set, Application Gateway, Bastion
- Destinations can include: VM, FQDN, URI, IPv4 address

**Connection Monitor = continuous**
**Connection Troubleshoot = point-in-time**


**Packet Capture**
- Remotely captures network traffic.
- Captures traffic **to/from VMs or VM Scale Sets**.
- Useful for detailed network troubleshooting.

**VPN Troubleshoot**
- Troubleshoots **VPN gateways and connections**.

---
#### Traffic
**Flow Logs**
- Logs Azure IP traffic.
- Stores traffic data in **Azure Storage**.
- Can log traffic through: NSGs, Azure VNets

**Traffic Analytics**
- Provides **visualisations and analysis** of flow-log data.
---
#### Exam Facts
- Network Watcher → IaaS network monitoring/troubleshooting
- Topology → visualise network/resource relationships
- Connection Monitor → continuous connectivity monitoring
- Connection Troubleshoot → point-in-time connectivity test
- IP Flow Verify → Allowed/Denied + responsible NSG rule
- Next Hop → routing problems
- Effective Security Rules → all rules applied to NIC
- Packet Capture → capture VM network traffic
- VPN Troubleshoot → VPN gateway/connectivity issues
- Flow Logs → record IP traffic → Azure Storage
- Traffic Analytics → visualise/analyse flow logs