## 1. Pre-Deployment Planning Checklist

Before creating an Azure VM, key configuration decisions must be evaluated:
* **Resource Group & Location:** Select a region close to users that supports required VM sizes/features.
* **Network & Subnet:** VM must connect to a Virtual Network (VNet) and subnet. Dedicated IP configuration (Dynamic vs. Static).
* **VM Size & Compute Tier:** Based on workload CPU, memory, IOPS, and disk requirements (General purpose, Compute optimized, Memory optimized, Storage optimized, GPU, High performance).
* **Storage / Disks:**
  * **OS Disk:** Default SATA/NVMe drive with boot volume (up to 4 TB, persistent).
  * **Temporary Disk:** Short-term volatile storage (`D:` on Windows, `/dev/sdb1` on Linux) — *lost on deallocation/migration; do not store persistent data here*.
  * **Data Disks:** Attached SCSI/NVMe drives for application/database storage (up to 32,767 GiB per disk).
* **Operating System & Image:** Azure Marketplace images, custom images via Azure Compute Gallery, or specialized OS.
* **Pricing & Sizing Model:** Pay-as-you-go, Reserved Instances (1-3 yrs), Spot VMs (unused capacity at steep discounts, can be evicted), Azure Hybrid Benefit (AHB).

---

## 2. VM Sizing Categories

| Category | Series | Target Workloads |
| :--- | :--- | :--- |
| **General Purpose** | B, Dsv5, Dasv5 | Balanced CPU-to-memory; testing, small-to-medium DBs, low/medium web traffic |
| **Compute Optimized** | Fsv2 | High CPU-to-memory ratio; medium web servers, network appliances, batch processes |
| **Memory Optimized** | Esv5, Edsv5, M | High memory-to-CPU ratio; relational databases, in-memory caches, analytics |
| **Storage Optimized** | Lsv3 | High disk throughput and I/O; Big Data, SQL/NoSQL databases, data warehousing |
| **GPU Accelerated** | NC, NV, ND | Heavy graphics rendering, video editing, AI/ML model training and inferencing |
| **High Performance (HPC)**| HB, HC | High-performance computing, molecular modeling, simulations with InfiniBand |

---

## 3. Azure VM Availability Options

| Availability Level | Fault Domain (FD) | Update Domain (UD) | Scope Protected | Max SLA |
| :--- | :--- | :--- | :--- | :--- |
| **Single VM** | N/A | N/A | Hardware replacement (Premium SSD/Ultra) | 99.9% |
| **Availability Set** | Up to 3 | Up to 20 | Single rack / switch / power unit failure | 99.95% |
| **Availability Zones** | Unique datacenter | Unique datacenter | Entire datacenter outage within region | 99.99% |

### Key Availability Terms
* **Fault Domain (FD):** Physical rack with shared power source and network switch.
* **Update Domain (UD):** Logical group of hardware that can undergo planned maintenance/reboots simultaneously.
* **Availability Sets:** Distributes VMs across multiple FDs and UDs within the same datacenter. (Managed disks align automatically with VM fault domains).
* **Availability Zones (AZ):** Physically separated datacenters within an Azure region, each with independent power, cooling, and networking.

> [!important] Exam Note
> A VM **cannot** be added or moved into an Availability Set after creation without deleting and recreating the VM from its disk.

---

## 4. VM Management & Automation Extensions

### Virtual Machine Extensions
Small applications that provide post-deployment configuration and automation tasks on Azure VMs.
* **Custom Script Extension:** Runs custom scripts (PowerShell, Bash) on VMs during/after deployment.
* **Azure Desired State Configuration (DSC) Extension:** Enforces configuration state using PowerShell DSC.
* **Azure Network Watcher Agent Extension:** Captures packets and monitors network performance.
* **Azure Monitor Agent (AMA):** Collects telemetry, event logs, and performance metrics.

### Additional VM Management Services
* **Azure Bastion:** Secure RDP/SSH connectivity directly via browser using TLS (Port 443), eliminating public IP exposure on VMs.
* **Azure Automation State Configuration / Runbooks:** Process automation and scheduled task management.
* **Azure Auto-Shutdown:** Automatically stops and deallocates VMs at scheduled times to minimize compute costs.
* **Boot Diagnostics:** Captures serial console logs and screen snapshots to troubleshoot unbootable VMs.

---

## 5. VM Backup & Disaster Recovery

### Azure Backup for Virtual Machines
* Uses **Recovery Services Vault** or **Backup Vault**.
* Native, application-consistent backups for Windows (VSS) and file-system consistent for Linux without shutting down the VM.
* **Instant Restore Point:** Retains local disk snapshots for 1–5 days for fast operational recovery.
* **Vault Retention Policy:** Daily, weekly, monthly, and yearly backup snapshots stored in vault storage (LRS, GRS, CRR - Cross-Region Restore).

### Azure Site Recovery (ASR)
* Orchestrates replication, failover, and failback of VMs across Azure regions for complete business continuity and disaster recovery (BCDR).

---

## 6. Exam Quick Reference (Cheat Sheet)

| Concept | Key Exam Rule / Trigger |
| :--- | :--- |
| **Stopped vs. Deallocated** | *Stopped* via OS still incurs compute charges; *Deallocated* via Azure stops compute charges (disks/IPs still billed). |
| **Temporary Drive (D:)** | Non-persistent SSD storage; wiped upon VM deallocation, resizing, or host hardware migration. |
| **Azure Hybrid Benefit (AHB)** | Use on-premises Windows Server / Red Hat / SUSE licenses with Software Assurance to save up to 85% on compute rates. |
| **Availability Set Limit** | Max 3 Fault Domains, max 20 Update Domains. |
| **Availability Zones SLA** | Requires $\ge 2$ VMs deployed across distinct Availability Zones for 99.99% SLA. |
| **Custom Script Timeout** | Default execution limit is 90 minutes. |
| **Azure Bastion** | Fully managed PaaS proxy; requires a dedicated subnet named `AzureBastionSubnet` (minimum `/26`). |
