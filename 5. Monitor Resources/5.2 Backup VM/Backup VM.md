## 1. Core Architecture & Components
Azure Backup provides native, automated, agent-less protection for Azure IaaS Virtual Machines.

### Architectural Workflow
1. **Snapshot Phase (Operational Tier):** Local disk snapshot taken via VM extension (`VMSnapshot` for Windows, `VMSnapshotLinux` for Linux). Instant restore point is generated immediately (retained 1–5 days locally).
2. **Data Transfer Phase (Vault Tier):** Snapshot data is transferred asynchronously to the Recovery Services Vault without impacting VM compute performance.

### Vault Types Comparison
* **Recovery Services Vault (RSV):** Primary vault for Azure VMs, SQL Server/SAP HANA on Azure VMs, Azure Files, and on-premises MARS/MABS.
* **Backup Vault:** Dedicated to newer workloads (Azure Blobs, Azure Disks, Azure Database for PostgreSQL).

---

## 2. Backup Consistency Types

| Consistency Level          | Windows OS Mechanism             | Linux OS Mechanism        | Data State & Recovery Impact                                                                       |
| :------------------------- | :------------------------------- | :------------------------ | :------------------------------------------------------------------------------------------------- |
| **Application-Consistent** | VSS (Volume Shadow Copy Service) | Custom pre/post scripts   | **Highest integrity.** Flushes memory cache & pending I/O before snapshot. Ready with no DB fixup. |
| **File System-Consistent** | VSS fallback on error            | Native `fsfreeze`         | All files consistent; pending in-memory database transactions are omitted.                         |
| **Crash-Consistent**       | VM shut down / VSS failure       | VM shut down / no scripts | Typical power-loss state. Requires disk-check (`chkdsk`/`fsck`) upon recovery.                     |

> [!tip] Exam Rule
> Windows VMs automatically get Application Consistency via native VSS.  Linux VMs default to File System Consistency unless custom **pre- and post-scripts** are configured.

---

## 3. Backup Policies & Tiers

A **Backup Policy** specifies *frequency*, *instant recovery retention*, and *vault retention rules*.

### Standard Policy vs. Enhanced Policy

| Feature | Standard Backup Policy | Enhanced Backup Policy |
| :--- | :--- | :--- |
| **Backup Frequency** | 1x per day (Daily) | Multiple per day (Hourly / every 4 to 24 hrs) |
| **Instant Restore Retention** | 1 to 5 days[cite: 1, 4] | 1 to 30 days[cite: 4] |
| **Supported VM Types** | Standard VMs[cite: 4] | Trusted Launch VMs, Confidential VMs, Ultra Disks, Shared Disks[cite: 4] |
| **Snapshot Redundancy** | Matches disk redundancy | Supports Zone-Redundant (ZRS) operational snapshots[cite: 4] |

### Retention Scheme (GFS)
* **Operational Snapshot Tier:** Instant Restore (1–5 days Standard, 1–30 days Enhanced) for rapid rollbacks
* **Vault Tier:** Long-term GFS (Grandfather-Father-Son) retention configured for **Daily**, **Weekly**, **Monthly**, and **Yearly** points

---

## 4. Vault Storage Redundancy & Cross-Region Restore (CRR)

* **Locally Redundant Storage (LRS):** 3 copies in a single datacenter
* **Zone-Redundant Storage (ZRS):** 3 copies across 3 availability zones in the primary region
* **Geo-Redundant Storage (GRS):** 3 copies in primary region + 3 copies replicated asynchronously to the paired secondary region
* **Cross-Region Restore (CRR):**
  * Requires **GRS**
  * Allows restoring VMs, disks, and files directly into the **secondary paired region** at any time (even when primary region is fully operational)

> [!warning] Exam Trap: Changing Vault Redundancy
> You **cannot** change storage redundancy (e.g., GRS to LRS) once any backup item is protected in the vault All protected backup items and data must be removed first

---

## 5. VM Restore Options

```text
Restore Options
├── 1. Create New VM         ──► Spins up a fully configured VM directly from restore point
├── 2. Restore Disks         ──► Restores raw managed disks + generates ARM template for customization
├── 3. Replace Existing Disk ──► Swaps OS / Data disks in-place on existing VM (least downtime)
└── 4. Item-Level Recovery   ──► Mounts iSCSI drive script (Port 3260, 12-hr validity) to recover files
````



### Breakdown of Restore Methods
- **Create New VM:** Fastest complete rebuild; Azure creates compute instance, NIC, and attaches restored disks
- **Restore Disks:** Restores VHD managed disks to a target storage account; provides ARM template to customise NIC, subnet, VM size, or tags
- **Replace Existing Disks:** In-place disk swap on the original VM; preserves original network configuration and VM identity
- **Item-Level File Recovery (ILR):** Downloads an OS script (`.exe` on Windows, `.sh` on Linux) that mounts point-in-time recovery volumes over **iSCSI** (Port 3260) Active for **12 hours** to copy granular files


## 6. Security, Governance & Immutability

- **Soft Delete:**
    - Retains deleted backup data for **14 to 180 days** (default: 14 days) in a recoverable state at no extra cost
    - Protects against accidental deletion and ransomware

- **Multi-User Authorization (MUA) with Resource Guard:**
    - Requires authorization from a separate **Resource Guard** resource (managed by a different admin/role) before performing critical destructive actions (disabling soft delete, modifying policies, deleting vaults)
    
- **Backup Security PIN:**
    - Temporary 6-digit portal-generated PIN (valid 5 minutes) required for high-risk operations via MARS agent / CLI
    
- **Azure Backup Center:**
    - Central governance pane to monitor compliance, track alerts, and assign backup policies at scale across subscriptions and regions
        

## Exam Quick Reference (Cheat Sheet)

| Scenario / Exam Trigger                                    | Correct Answer / Action                                                |
| ---------------------------------------------------------- | ---------------------------------------------------------------------- |
| **Fastest restore without data transfer from vault**       | Restore using **Instant Restore Point** (Snapshot tier, 1–5 days)      |
| **Recover individual lost file without restoring full VM** | Use **Item-Level File Recovery** (mounts iSCSI drive, valid 12 hrs)    |
| **Test DR / restore VM in paired secondary region**        | Enable **GRS + Cross-Region Restore (CRR)** on Recovery Services Vault |
| **Application consistency for Linux VM**                   | Configure **Pre-scripts and Post-scripts** in VM backup configuration  |
| **Prevent unauthorized deletion of backup vault/data**     | Configure **MUA (Multi-User Authorization) with Resource Guard**       |
| **Change vault redundancy from GRS to LRS**                | Stop protection and **delete all backup data** in the vault first      |
| **Protect Trusted Launch or Ultra Disk VMs**               | Use an **Enhanced Backup Policy**                                      |
| **Recover accidentally deleted backup data**               | Use **Undelete** within the **Soft Delete** window (14–180 days)       |