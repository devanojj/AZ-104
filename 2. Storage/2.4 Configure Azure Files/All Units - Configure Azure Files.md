## 🗂️ Azure Files Overview

- PaaS, serverless file shares — no VMs/infra to manage
- Protocols: **SMB**, **NFS**, **HTTP/REST** (SMB and NFS **not** supported on same share; can coexist in same storage account on different shares)
- Max share size: **100 TiB**, max file size: **4 TiB**
- Encrypted at rest and in transit
- Accessible from anywhere with internet (by default)
- Access control: Microsoft Entra ID / AD DS identities
- Previous Versions (via snapshots) + Azure Backup support
- Redundancy set at the **storage account** level

### Use Cases

- Replace/supplement on-prem file servers or NAS
- Lift-and-shift apps needing a file share
- Shared app config storage
- Diagnostic data (logs, dumps)
- Dev/admin tools shared across VMs
- Pairs with **Azure File Sync** for hybrid caching

## ⚖️ Azure Files vs Blob Storage

|Azure Files|Blob Storage|
|---|---|
|SMB/NFS/REST, true directory structure|REST only, flat namespace|
|Accessed via file **shares**|Accessed via **containers**|
|Best for lift-and-shift apps using file system APIs|Best for streaming/random access, massive unstructured data|

## 💾 File Share Tiers & Storage Accounts

|Tier|Backing|Storage account|Redundancy|Billing|Use case|
|---|---|---|---|---|---|
|**Premium**|SSD|FileStorage|LRS, ZRS|Provisioned|Low-latency, high-perf workloads|
|**Transaction Optimized**|HDD|GPv2|LRS, GRS, RA-GRS, ZRS, GZRS, RA-GZRS|Pay-as-you-go|High-transaction workloads|
|**Hot**|HDD|GPv2|LRS, GRS, RA-GRS, ZRS, GZRS, RA-GZRS|Pay-as-you-go|General team/collab shares|
|**Cool**|HDD|GPv2|LRS, GRS, RA-GRS, ZRS, GZRS, RA-GZRS|Pay-as-you-go|Archive/backup|

> [!note] Premium = SSD, provisioned billing (pay for reserved capacity). Standard (Transaction Optimized/Hot/Cool) = HDD, pay-as-you-go.

- **File shares (preview→GA)**: can now be created **without** a storage account, for simplified management.
- SMB traffic uses **port 445** — often blocked by ISPs outbound (common on-prem connectivity issue).

## 🔐 Authentication Methods

|Method|Notes|
|---|---|
|**Identity-based (SMB)**|Sources: on-prem AD DS, Microsoft Entra Domain Services, Microsoft Entra Kerberos. Assign Azure RBAC roles after setup.|
|**Access key**|Storage account has 2 keys (`key1`/`key2`). Full control, static, bypasses all access restrictions — avoid sharing, prefer identity-based auth.|
|**SAS token**|Dynamically generated, scoped (permissions, time window, IP, protocol). Used for REST API access from code.|

## 📸 File Share Snapshots

- **Read-only**, point-in-time, **incremental** (only captures changes since last snapshot)
- Same behavior across SMB/NFS, all public regions
- Adds unique timestamp to share URI
- Uses the share's redundancy setting
- **Up to 200 snapshots** per file share
- Persist until deleted; deleting the **share** deletes **all** snapshots
- Azure Backup can lease snapshots → prevents accidental deletion
- Can restore file, folder, or full share (full restore only needs the **latest** snapshot)
- Manageable via PowerShell/CLI (can schedule via Automation, GitHub Actions, CI)

**Benefits:** protect against app errors/corruption, accidental deletion/changes, backup & recovery history.

## ♻️ Soft Delete (Azure Files)

- Enabled at the **storage account** level
- Deleted shares → "soft deleted" state, not erased immediately
- Configurable retention: **1–365 days**
- Can enable on new or existing shares

**Use cases:** accidental data loss recovery, rollback after failed upgrades, ransomware recovery, long-term retention/compliance, business continuity.

## 🧰 Azure Storage Explorer

- Standalone GUI app (Windows/macOS/Linux) for managing Storage data
- Needs **both** management (ARM) + data layer permissions (Microsoft Entra ID)

**Connection scenarios:**

- Connect to your own subscription's storage accounts
- Local dev storage via Azure Storage Emulator
- Attach **external** storage account (needs account name + key, `key1` in portal)
- Attach storage account via **SAS**
- Attach a single service (blob/queue/table) via **SAS**

> [!tip] For national/sovereign clouds, use "Storage endpoints domain" → **Other** → enter custom endpoint domain.

- Access keys grant access to the **whole account** — store securely, rotate regularly. Regenerating a key doesn't interrupt VM disk access.

## 🔄 Azure File Sync

Caches Azure file share(s) on an on-prem Windows Server or cloud VM — centralizes data in Azure Files while keeping on-prem performance/compatibility.

### Components

|Component|Details|
|---|---|
|**Storage Sync Service**|Top-level Azure resource; manages sync; up to **100 sync groups**; single region; up to **99 registered servers**|
|**Sync group**|1 cloud endpoint + up to **50 server endpoints**|
|**Cloud endpoint**|The Azure file share; only **1 per sync group**|
|**Server endpoint**|Path on registered server; must be NTFS; **cannot** be system volume; no cloud tiering support here|
|**Azure File Sync Agent**|Installed on each Windows Server; background service handling sync|

### Key Facts

- Turns Windows Server into a cache of Azure file shares
- On-prem access via SMB, NFS, or FTPS (any protocol available on Windows Server)
- Unlimited number of caches worldwide
- Limits: **100 sync groups**/Storage Sync Service, **50 server endpoints**/sync group, **99 servers** registered

### Use Cases

- Lift-and-shift apps needing write access both in Azure & on-prem
- Branch office backup/file access
- Backup & disaster recovery (Azure Backup protects on-prem data via sync; fast metadata restore + recall data as needed)
- **Cloud tiering**: keeps only recently-used files local; older files tiered to Azure Files

---

## 🎯 Exam Quick-Reference

- SMB ⟷ NFS: **mutually exclusive per share**, can mix within one storage account
- Max share: 100 TiB | Max file: 4 TiB
- Premium = SSD/FileStorage/provisioned | Standard tiers = HDD/GPv2/pay-as-you-go
- Port **445** = SMB (common blocker issue)
- Snapshots: max **200**/share, incremental, read-only
- Soft delete retention: **1–365 days**, account-level setting
- Storage Sync Service: **100 sync groups** max, **99 servers**
- Sync group: **1 cloud endpoint**, **50 server endpoints** max
- Server endpoint must be NTFS, **not** system volume
- Access keys = full control, bypass RBAC → avoid; prefer identity-based auth