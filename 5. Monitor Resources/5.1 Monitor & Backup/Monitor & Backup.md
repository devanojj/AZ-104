Azure-managed service for backing up and restoring Azure and supported on-premises workloads.

| Concept | Meaning |
|---|---|
| **Azure Backup** | Backup and restore service |
| **RPO** | Maximum acceptable **data loss** |
| **RTO** | Maximum acceptable **recovery time** |
| **Recovery Point** | Point-in-time copy that can be restored |
| **Backup Policy** | Defines **backup schedule + retention** |
| **Recovery Services Vault** | Stores/manages backup data for many Azure Backup workloads |
| **Backup Vault** | Vault used by newer Azure Backup workloads |
| **Backup Center** | Central management of backups across Azure |

*What can be backed up*
**Azure VMs**, **Azure Files**, **SQL Server in Azure VMs**, **SAP HANA in Azure VMs**
**Azure Blobs**, **Azure Disks**, Supported Azure databases, **On-premises Windows** 


==**Azure VM Backup**==
- Backs up the entire VM
- Uses a backup extension
- Backup stored in a *Recovery Services* vault
- Uses incremental backups after the initial backup

==**Incremental backup**==
- Only changed data is backed up
- Reduces storage and backup time

**VM restore options**
- Restore entire VM
- Create a new VM
- Restore disks
- Restore individual files

---

## Backup Agents & Extensions

| Component | Used For |
|---|---|
| **Backup Extension** | Azure VM backup integration |
| **MARS Agent** | On-premises Windows files/folders/system state |
| **MABS** | Enterprise backup server for supported on-premises workloads |
| **DPM** | Microsoft Data Protection Manager |


> **MARS → Windows files/folders/system state**
> **VM Backup Extension → Azure VM backup**

---

## Backup Storage

| Tier | Purpose |
|---|---|
| **Snapshot** | Fast recovery |
| **Vault-Standard** | Normal backup storage |
| **Archive** | Long-term, rarely accessed backups |

**Think:**

`Snapshot → Fast restore`

`Vault → Normal backup`

`Archive → Long-term retention`

---

## Storage Redundancy

| Option | Meaning |
|---|---|
| **LRS** | Copies within a single datacenter |
| **ZRS** | Copies across availability zones |
| **GRS** | Copies to a paired region |

**Exam Tip**

- LRS → **local**
- ZRS → **zones**
- GRS → **geographic**

---

## Backup Security

### Soft Delete
- Protects backup data from accidental/malicious deletion
- Deleted backup data is retained for a recovery period
- Default soft-delete retention is **14 days**

### Other Security Features
- **RBAC** → controls access
- **Encryption** → protects backup data
- **Private endpoints** → private network access
- Alerts → monitor backup activity

---

## Backup Policy

A **Backup Policy** defines:

- **When** backups run
- **How often** backups run
- **How long** recovery points are retained

Example:

`Daily backup → retain for 30 days`

---

## Azure Backup vs Site Recovery

| Azure Backup | Azure Site Recovery |
|---|---|
| Protects **data** | Protects **workloads** |
| Backup + restore | Disaster recovery |
| Recovery points | Replication + failover |
| Recover deleted/corrupted data | Recover from regional/site failure |

**Exam Tip**

> **Backup = restore data**

> **Site Recovery = failover workloads**

---

## SQL Server Backup

Azure Backup supports SQL Server running in Azure VMs.

Can perform:

- **Full backups**
- **Differential backups**
- **Transaction log backups**
- **Point-in-time restore**

**Exam Fact**

- Transaction log backups can provide an RPO as low as **15 minutes** for supported SQL Server scenarios.

---

## Backup Center

**Backup Center** = centralized backup management.

Can be used to:

- Monitor backups
- Manage backup policies
- Configure backups
- View backup jobs
- Manage multiple:
  - Subscriptions
  - Regions
  - Vaults
  - Workloads

---

## Common Uses

- Protect Azure VMs
- Recover accidentally deleted data
- Recover corrupted data
- Protect against ransomware
- Long-term data retention
- Compliance requirements
- Disaster recovery preparation

---

## Exam Facts

- **RPO** → amount of data you can lose
- **RTO** → amount of downtime you can tolerate
- **Backup Policy** → schedule + retention
- **Recovery Services vault** → backup storage/management
- **Backup Center** → centralized backup management
- **MARS** → on-prem Windows backup
- **Backup Extension** → Azure VM backup
- **Soft Delete** → protects deleted backup data
- **Snapshot** → fast recovery
- **Archive** → long-term retention
- **Incremental backup** → only changed data
- **Azure Backup ≠ Site Recovery**
- **LRS** → local redundancy
- **ZRS** → zone redundancy
- **GRS** → geo redundancy