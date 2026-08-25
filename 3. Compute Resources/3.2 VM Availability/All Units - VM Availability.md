## 1. Availability Planning & Downtime Events

| Event Type | Cause | Impact & Azure Mitigation |
| :--- | :--- | :--- |
| **Planned Maintenance** | Host OS updates, hardware firmware, hypervisor patching | Azure notifies in advance; uses live migration or reboots Update Domains sequentially. |
| **Unplanned Hardware Maintenance** | Impending physical component failure (NIC, power supply) | Azure migrates VM to healthy physical host; minor performance pause or reboot. |
| **Unexpected Downtime** | Total rack failure, power outage, catastrophic hardware crash | Azure detects crash and automatically boots VM on another physical host in the cluster. |

---

## 2. Availability Sets (Rack-Level Redundancy)

Logical grouping of $\ge 2$ VMs within the same datacenter to protect against single rack/switch failures.

### Fault Domains (FD) vs. Update Domains (UD)
* **Fault Domains (Physical):** Shared physical rack with common power source and network switch.
  * Maximum: **3 Fault Domains** (numbered 0, 1, 2; some regions support max 2).
* **Update Domains (Logical):** Groups of VMs rebooted together during planned host maintenance.
  * Maximum: **20 Update Domains** (Default: 5).
  * Only **one UD is rebooted at a time**; Azure allows a **30-minute recovery window** before restarting the next UD.

### Managed Disk Alignment
* Azure automatically aligns Managed Disk storage clusters with the VM's assigned Fault Domain.
* Avoids placing disks for multiple VMs onto a single storage rack.

> [!important] Exam Critical Constraints
> * **At-Creation Only:** A VM **must** be assigned to an Availability Set at creation time. You **cannot** move an existing VM into/out of an Availability Set without deleting and recreating it.
> * **SLA Guarantee:** Requires $\ge 2$ VMs in the same Availability Set for a **99.95% SLA**.
> * **Shared FD Bug Prevention:** Do not stop/deallocate VMs while sequentially provisioning VMs in an Availability Set, as Azure might allocate the new VM's disk to the same FD.

---

## 3. Availability Zones (Datacenter-Level Redundancy)

Physically separate datacenter locations within the same Azure region, each with independent power, cooling, and networking.

* **Zone Designations:** Zones 1, 2, and 3 within a single region.
* **SLA Guarantee:** Deploying $\ge 2$ VMs across distinct Availability Zones provides a **99.99% SLA**.
* **Zonal vs. Zone-Redundant:**
  * **Zonal:** Resource pinned to a specific single zone (e.g., VM in Zone 1).
  * **Zone-Redundant:** Azure replicates resource automatically across multiple zones (e.g., Zone-Redundant Storage, Gateway SKUs).

---

## 4. Vertical vs. Horizontal Scaling

| Dimension | Scaling Type | Action | Downtime / State |
| :--- | :--- | :--- | :--- |
| **Vertical (Scale Up / Down)** | Sizing adjustments | Change CPU, RAM, or disk size (e.g., `D2s_v5` $\rightarrow$ `D4s_v5`) | **Requires VM reboot/deallocation**; limited by hardware host capacity. |
| **Horizontal (Scale Out / In)** | Capacity adjustments | Add or remove identical VM instances behind a load balancer | **Zero application downtime**; ideal for cloud-native, stateless workloads. |

---

## 5. Azure Virtual Machine Scale Sets (VMSS)

Centrally manage, configure, and automatically scale a group of load-balanced VMs.

### Orchestration Modes

| Feature | Uniform Orchestration | Flexible Orchestration (Recommended) |
| :--- | :--- | :--- |
| **VM Consistency** | Identical VM instances only | Heterogeneous (mix VM sizes, OS types, Spot + Pay-As-You-Go) |
| **Capacity Scale** | Up to 1,000 instances | Up to 1,000 instances |
| **High Availability** | Spans Fault Domains or Zones automatically | Mix Availability Zones and regional Availability Sets |
| **Individual VM Control**| Limited standalone VM actions | Full manual control over individual VM instances |

### Upgrade Policies (OS / Template Updates)
* **Automatic:** VMSS rolls out updates immediately in batches across all instances.
* **Rolling:** Progressively updates VMs in phases with pauses and health probe validation.
* **Manual:** Existing VMs do not update automatically; administrator triggers update via portal/CLI (`az vmss update-instances`).

### Overprovisioning
* When enabled (`overprovision = true`), Azure spins up more VMs than requested, confirms healthy status, and deletes excess instances.
* **Benefit:** Speeds up deployment and scaling times.
* **Disable When:** Using custom per-core licensing, stateful setups, or strict billing thresholds.

---

## 6. Autoscale Configuration & Rules

Automatically adjusts VM count based on performance metrics or predefined schedules.

### Core Components
* **Instance Limits:** `Minimum` (baseline reserve), `Maximum` (budget/capacity ceiling), `Default` (fallback on metric failure).
* **Metric Triggers:**
  * Aggregations: Average, Minimum, Maximum, Total over a time grain (e.g., 5–10 mins).
  * Metrics: CPU Percentage, Memory, Disk Queue, Network In/Out, Service Bus Queue depth.
* **Cool-Down Period:**
  * Time to wait after a scale action before initiating another (prevents **flapping / thrashing**).
  * Default is usually 5–10 minutes.

### Scale-In Policies (Instance Selection Order)
When demand drops, determines which VM instances are deleted first:
1. **Default:** Balances instances across zones and fault domains, then deletes the **newest** VM.
2. **NewestVM:** Deletes the most recently created VM first (protects long-running batch jobs).
3. **OldestVM:** Deletes the oldest VM first (useful for retiring older image versions).

---

## 7. Exam Quick Reference (Cheat Sheet)

| Concept | Key Exam Rule / Trigger |
| :--- | :--- |
| **Availability Set Limits** | Max **3 Fault Domains** (power/switch), Max **20 Update Domains** (reboot groups). |
| **Availability Set SLA** | **99.95%** (requires $\ge 2$ VMs in same set). |
| **Availability Zone SLA** | **99.99%** (requires $\ge 2$ VMs in different zones). |
| **Single VM SLA** | **99.9%** (requires Premium SSD or Ultra Disk for all OS/data disks). |
| **UD Maintenance Window** | Only **1 UD** reboots at a time with a **30-minute recovery window**. |
| **VMSS Uniform vs Flexible** | Uniform = identical VMs/large scale; Flexible = mixed VM sizes, Spot + On-Demand. |
| **VMSS Scale-in Flapping** | Resolved by tuning threshold deltas and extending the **cool-down period**. |
| **Scale-in Selection** | Default balances across FDs/Zones first, then removes newest VM. |
