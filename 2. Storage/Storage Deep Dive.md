*Three groups:*
1. **Configure access to storage** — Firewalls & VNets, SAS tokens, stored access policies, access keys, identity-based access for Azure Files
2. **Configure and manage storage accounts** — Create/configure accounts, redundancy, object replication, encryption, Storage Explorer & AzCopy
3. **Configure Azure Files and Azure Blob Storage** — file shares, blob containers, storage tiers, soft delete (blobs + containers), snapshots & soft delete for Files, blob lifecycle management, blob versioning

---
## 1. Storage Account Basics

### Account Types
| Type                                   | Performance    | Supports                                  | Use When                                                       |
| -------------------------------------- | -------------- | ----------------------------------------- | -------------------------------------------------------------- |
| **Standard general-purpose v2 (GPv2)** | Standard (HDD) | Blob, Files, Queue, Table, Data Lake Gen2 | Default answer for almost every scenario                       |
| **Premium block blobs**                | Premium (SSD)  | Block + append blobs only                 | High transaction rates, small objects, low latency             |
| **Premium file shares**                | Premium (SSD)  | Azure Files only (SMB + NFS)              | Enterprise/high-performance file shares, **NFS needs Premium** |
| **Premium page blobs**                 | Premium (SSD)  | Page blobs only                           | Unmanaged disks / VHDs                                         |

* **Legacy:** GPv1 and BlobStorage accounts still exist — the exam answer is to upgrade to GPv2.
* Premium accounts cannot be geo-redundant → only LRS or ZRS.
* Performance tier (Standard vs Premium) is **chosen at creation and cannot be changed** — you must create a new account and copy data.
* Account name: **3–24 characters, lowercase letters and numbers only, globally unique** (forms the DNS name).

### Service Endpoints (URLs)
```text
https://<account>.blob.core.windows.net
https://<account>.file.core.windows.net
https://<account>.queue.core.windows.net
https://<account>.table.core.windows.net
https://<account>.dfs.core.windows.net    (Data Lake Gen2)
https://<account>.web.core.windows.net    (static website)
```

### Redundancy
Full detail in [[Unit 5 - Replication]]. Decision table:

|Redundancy|Copies|Survives Node|Survives Zone|Survives Region|Read Secondary|
|---|---|---|---|---|---|
|**LRS**|3 (one DC)|✅|❌|❌|❌|
|**ZRS**|3 (3 AZs)|✅|✅|❌|❌|
|**GRS**|3 + 3 (2 regions)|✅|❌|✅|❌|
|**RA-GRS**|3 + 3|✅|❌|✅|✅|
|**GZRS**|3 AZs + 3 (2 regions)|✅|✅|✅|❌|
|**RA-GZRS**|3 AZs + 3|✅|✅|✅|✅|

* **Durability:** LRS 11 9's, ZRS 12 9's, GRS/GZRS **16 9's**.
* Geo-replication to the secondary is **asynchronous** → possible data loss, measured by **Last Sync Time**.
* **RA-** prefix is the *only* way to read the secondary without a failover.

### Changing Redundancy After Creation
| Change                                 | Method                                                     |
| -------------------------------------- | ---------------------------------------------------------- |
| LRS ⇄ GRS / RA-GRS (same region pair)  | Portal switch, no downtime                                 |
| ZRS ⇄ GZRS / RA-GZRS                   | Portal switch                                              |
| **LRS → ZRS** (adding zone redundancy) | **Conversion** — customer-initiated or Microsoft-initiated |
| GRS → GZRS                             | Must first remove geo-redundancy, convert, then re-add     |
| Standard ⇄ Premium                     | **Not supported** — new account + AzCopy                   |

> [!warning] Conversion limits
> Conversion requires the region to support availability zones, and you cannot convert while a geo-failover is in progress. To go from GRS straight to GZRS the practical path is: **GRS → LRS → ZRS → GZRS**, or a manual migration.

### Customer-Managed Failover
Only available on **GRS, RA-GRS, GZRS, RA-GZRS**.

|  |**Planned failover**|**Unplanned failover**|
|---|---|---|
|Primary region state|Healthy|Unavailable / outage|
|Data loss|**None** (full sync first)|**Possible** — anything after Last Sync Time|
|Redundancy after|Preserved (GZRS → GRS)|**Account becomes LRS** in the new primary|
|Original primary|Retained, failback possible|**Deleted**|
|Use for|DR drills, planned maintenance|Real regional outage|

> [!warning] Exam trap
> After an **unplanned** failover the account is **LRS**. You must manually reconfigure geo-redundancy, and you can only go back to **GRS/RA-GRS** — **not** ZRS/GZRS/RA-GZRS. Always check **Last Sync Time** before failing over.

* Failover changes the DNS entry for the primary endpoint to the secondary region — the account name and URL stay the same.
* Not supported for: Premium accounts, accounts with hierarchical namespace (ADLS Gen2) in some configurations, ZRS-only accounts, or accounts with Azure File Sync.

---
## 2. Networking & Security

### Public Network Access
Three settings on the account:
* **Enabled from all networks** — open to the internet (default on creation).
* **Enabled from selected virtual networks and IP addresses** — firewall mode.
* **Disabled** — private endpoints only.

### Firewall Rules
* Allow by **public IPv4 address or CIDR range** — RFC 1918 private ranges are **rejected**.
* Rules apply to the **data plane only**, not the control plane (an owner can still see the account in the portal).
* **Default action** when traffic matches no rule: **Deny**.
* **Allow trusted Microsoft services** — exception that lets services like Azure Backup, Azure Monitor, Azure Site Recovery, Event Grid, and Azure Functions reach the account despite the firewall. These services authenticate with a **managed identity** and must still have RBAC.
* **Resource instance rules** — scope the exception to a *specific* resource (e.g. one Synapse workspace) rather than all trusted services.

### Service Endpoints vs Private Endpoints
|  |**Service Endpoint**|**Private Endpoint**|
|---|---|---|
|What it is|Optimised route from a subnet to the service over the Azure backbone|A **NIC with a private IP** from your VNet, mapped to the account|
|IP used|VM keeps its **private IP**, traffic stays on the backbone|Traffic goes to a **private IP in your VNet**|
|DNS|Resolves to the **public IP** of the account|Resolves to a **private IP** via `privatelink.blob.core.windows.net`|
|Granularity|Whole **service** across the region|**One sub-resource** (blob, file, queue, table, web, dfs) per endpoint|
|On-prem access|❌ Not reachable over VPN/ExpressRoute|✅ Reachable over VPN/ExpressRoute|
|Cost|Free|Charged per endpoint + data processed|
|Exfiltration risk|Still a public endpoint|Removes the public endpoint entirely|
|Configured on|The **subnet**|The **storage account** (+ Private DNS zone)|

> [!tip] Scenario mapping
> "Only VMs in this subnet" → **service endpoint**. "Reach storage from on-premises over ExpressRoute with a private IP" or "no public exposure at all" → **private endpoint**. Private endpoint needs a **Private DNS zone** or on-prem conditional forwarder, otherwise names still resolve to the public IP.

* You need **one private endpoint per sub-resource** — a blob private endpoint does not cover file shares.
* Private DNS zones: `privatelink.blob.core.windows.net`, `privatelink.file.core.windows.net`, `privatelink.dfs.core.windows.net`, etc.
* See also [[Azure Network Security Perimeter]].

### Transport & Protocol Security
* **Secure transfer required** — forces **HTTPS**; on by default. Must be **off** to mount SMB over the internet unencrypted or to use custom domains over HTTP.
* **Minimum TLS version** — default **TLS 1.2**; TLS 1.0/1.1 are deprecated.
* **Allow Blob anonymous access** — account-level switch. If **disabled**, container "public access level" cannot be set to Blob or Container regardless of the container setting.
* **Permitted scope for copy operations** — restrict `Copy Blob` sources (same account / same tenant / accounts with private links).
* **Allow shared key access** (`allowSharedKeyAccess`) — set to **false** to force Entra ID authorisation. Disabling it also breaks account/service SAS, since those are signed with the key.
* **Cross-tenant replication** — disable unless object replication across tenants is required.

### Encryption
* **SSE (Storage Service Encryption):** all data encrypted at rest with **256-bit AES**, always on, **cannot be disabled**, no extra cost.
* **Microsoft-managed keys (MMK):** default, zero admin.
* **Customer-managed keys (CMK):**
  * Keys live in **Azure Key Vault** or **Managed HSM**, same region as the account.
  * Key Vault must have **soft delete** and **purge protection** enabled.
  * Account accesses the vault via **system-assigned or user-assigned managed identity**, with the **Key Vault Crypto Service Encryption User** role (or an access policy with get/wrap/unwrap).
  * Can be set at account creation or later; supports automatic key version rotation.
  * Gives you the ability to **revoke access** by disabling the key.
* **Customer-provided keys (CPK):** supplied per-request in the API header, **blob only**.
* **Infrastructure encryption:** adds a **second** 256-bit AES layer at the hardware level (double encryption). **Must be enabled at account creation — cannot be added later.**
* **Encryption scopes:** different keys per **container** or **individual blob**; can be set as the container default and optionally prevent overrides.

### Other Protections
* [[Microsoft Defender for Storage]] — malware scanning, sensitive data discovery, threat detection.
* **Resource locks** (`CanNotDelete` / `ReadOnly`) on the account protect against accidental deletion.

---
## 3. Access Control (heavily tested)

### The Three Authorisation Models
| Model                         | Credential                         | Scope                                                     | Revocation                                         | Audit                            |
| ----------------------------- | ---------------------------------- | --------------------------------------------------------- | -------------------------------------------------- | -------------------------------- |
| **Access keys (Shared Key)**  | Two 512-bit keys, full root access | Entire account, both planes                               | Regenerate the key                                 | Poor — no user identity          |
| **SAS token**                 | Signed URI                         | Narrow: service / container / blob, time-bound            | Rotate key, or delete the **stored access policy** | Poor                             |
| **Microsoft Entra ID (RBAC)** | Entra identity / managed identity  | Role assignment at MG, sub, RG, account, container, queue | Remove role assignment                             | **Best** — full identity in logs |

**Order of preference for the exam: Entra ID > SAS > access keys.**

### Access Keys
See [[Storage Account Keys]].
* Two keys (`key1`, `key2`) so you can rotate without downtime:
  1. Point apps at `key2`
  2. Regenerate `key1`
  3. Point apps back at `key1`
  4. Regenerate `key2`
* Store in **Azure Key Vault**; a Key Vault **storage account key** object can rotate them automatically.
* Reading keys requires a control-plane permission (`listKeys`) — held by **Owner, Contributor, Storage Account Contributor, Storage Account Key Operator Service Role**.
* **Connection string** = account name + key, so it carries full access.

### SAS Types
| SAS Type                | Signed With                                   | Scope                                                                                       | Revoke By                                      | Max Life      |
| ----------------------- | --------------------------------------------- | ------------------------------------------------------------------------------------------- | ---------------------------------------------- | ------------- |
| **User delegation SAS** | **Entra ID credential** (user delegation key) | **Blob and Data Lake only**                                                                 | Revoke the delegation key or the RBAC role     | 7 days        |
| **Service SAS**         | Account key                                   | One service: blob / file / queue / table                                                    | Delete stored access policy, or rotate the key | No hard limit |
| **Account SAS**         | Account key                                   | Multiple services + account-level ops (e.g. `Get Blob Service Properties`, list containers) | Rotate the key only                            | No hard limit |

> [!tip] Best practice
> **User delegation SAS** is the recommended type — it's the only one not backed by an account key, so it can be audited to an Entra identity and revoked without rotating keys. It requires the caller to hold **Storage Blob Delegator** plus a data role.

### Stored Access Policies
* Defined on a **blob container, file share, queue, or table** (not at account level, so **account SAS cannot use one**).
* Holds the start time, expiry, and permissions **server-side**; the SAS references it by `si=<policy-id>`.
* **Max 5 policies** per container/share/queue/table.
* **Why they matter:** you can change permissions/expiry or **delete the policy to revoke every SAS that references it instantly**, without regenerating account keys.
* Many SAS tokens can point to the same policy — delete it and all of them die at once.

### SAS Query Parameters
```text
https://<account>.blob.core.windows.net/<container>/<blob>?<sas-token>
```
|Parameter|Meaning|
|---|---|
|`sp`|Permissions — `r` read, `w` write, `d` delete, `l` list, `a` add, `c` create, `y` process, `t` tag, `f` filter, `i` set immutability|
|`st` / `se`|Start and expiry time (UTC)|
|`sip`|Allowed IP or CIDR range|
|`spr`|Protocol — always force `https`|
|`sv`|Service version|
|`sr`|Resource — `b` blob, `c` container, `bs` blob snapshot, `d` directory|
|`si`|Stored access policy name|
|`skoid` / `sktid`|Present only on a **user delegation SAS** (object + tenant ID of the signer)|
|`sig`|HMAC-SHA256 signature|

### Storage RBAC Roles
> [!warning] Control plane vs data plane
> **Owner / Contributor / Reader** are **control-plane** roles — they manage the account but grant **no data access via Entra ID**. Contributor *can* read access keys, so it reaches data indirectly; **Reader cannot**. To read a blob with your own Entra identity you need a **Data** role.

| Role                                                                             | Grants                                                                     |
| -------------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| **Storage Blob Data Owner**                                                      | Full blob access + POSIX ACL management (ADLS Gen2)                        |
| **Storage Blob Data Contributor**                                                | Read, write, **delete** blobs and containers                               |
| **Storage Blob Data Reader**                                                     | Read and list blobs only                                                   |
| **Storage Blob Delegator**                                                       | Get the user delegation key (required to **create** a user delegation SAS) |
| **Storage File Data SMB Share Reader / Contributor / Elevated Contributor**      | SMB share-level access; *Elevated* adds NTFS permission modification       |
| **Storage File Data Privileged Reader / Contributor**                            | Bypass NTFS ACLs (superuser access to the share)                           |
| **Storage Queue Data Contributor / Reader / Message Processor / Message Sender** | Queue data access                                                          |
| **Storage Table Data Contributor / Reader**                                      | Table data access                                                          |
| **Storage Account Contributor**                                                  | Manage the account, **read keys**, no direct data access                   |
| **Storage Account Key Operator Service Role**                                    | List and regenerate keys only                                              |
| **Reader and Data Access**                                                       | Read keys + read the account                                               |

* Data roles can be scoped down to a **single container or queue**, which is the least-privilege answer.
* **File shares (SMB) do not support container-level RBAC for NTFS** — share-level RBAC sets the share permission, then **NTFS ACLs** apply inside.
* Role assignment propagation can take up to **5 minutes**.

---
## 4. Blob Storage

### Blob Types
* **Block blob** — documents, images, backups. Default. Max ~190.7 TiB.
* **Append blob** — logging; writes only append.
* **Page blob** — random read/write, up to 8 TiB; backs **unmanaged disks**.

### Container Public Access Levels
* **Private (no anonymous access)** — default.
* **Blob** — anonymous read of **blobs** if you know the URL, no listing.
* **Container** — anonymous read **and list** of the container's blobs.
* Requires **Allow blob anonymous access** enabled at account level.

### Access Tiers
|Tier|Set At|Min Retention|Availability SLA|Storage Cost|Access Cost|Online?|
|---|---|---|---|---|---|---|
|**Hot**|Account + blob|None|99.9% (99.99% RA-GRS)|Highest|Lowest|✅|
|**Cool**|Account + blob|**30 days**|99%|Lower|Higher|✅|
|**Cold**|Account + blob|**90 days**|99%|Lower still|Higher still|✅|
|**Archive**|**Blob only**|**180 days**|Offline|Lowest|Highest|❌ **Offline**|

* **Hot / Cool / Cold are online** — data is read immediately. **Archive is offline** and must be rehydrated first.
* **Archive cannot be set as the account default** — only per blob (or via lifecycle management).
* Early deletion / tier change before the minimum retention incurs a **prorated early deletion fee** (e.g. archive deleted at day 45 → charged for 135 days).
* Archive blobs keep their **metadata online**, so you can still list them and read metadata.
* Tiers apply to **block blobs only**, in **GPv2 / BlobStorage / Premium block blob** accounts.

### Rehydration from Archive
Two ways:
1. **Copy** the archived blob to a new blob in an online tier (`Copy Blob`) — the original stays in archive, so **no early deletion fee**. Recommended.
2. **Change the blob's tier** to hot/cool/cold — in-place, blob is unreadable until rehydration finishes, and early deletion fees apply.

|Priority|Time (< 10 GB)|Cost|
|---|---|---|
|**Standard**|up to **15 hours**|Lower|
|**High**|under **1 hour**|Higher|

* You can **upgrade** an in-flight rehydration from Standard to High priority.
* Use **Event Grid** (`BlobTierChanged`) to be notified when rehydration completes.

### Lifecycle Management
* **JSON rule-based**, if/then. Runs **once per day**, applied at account level.
* Scope with **prefix match** (`container/folder/`) and **blob index tag** filters; filter by blob type and by base blob / snapshot / version.
* Conditions: `daysAfterModificationGreaterThan`, `daysAfterLastAccessTimeGreaterThan` (needs **last access time tracking** enabled), `daysAfterCreationGreaterThan` (snapshots/versions), `daysAfterLastTierChangeGreaterThan`.
* Actions: `tierToCool`, `tierToCold`, `tierToArchive`, `delete`, `enableAutoTierToHotFromCool`.
* Supported on GPv2, Premium block blob (delete only), BlobStorage, and ADLS Gen2.
* First run after creating or editing a policy can take up to **48 hours** to take effect.

```json
{
  "rules": [{
    "name": "tier-then-delete",
    "enabled": true,
    "type": "Lifecycle",
    "definition": {
      "filters": { "blobTypes": ["blockBlob"], "prefixMatch": ["logs/"] },
      "actions": {
        "baseBlob": {
          "tierToCool": { "daysAfterModificationGreaterThan": 30 },
          "tierToArchive": { "daysAfterModificationGreaterThan": 90 },
          "delete": { "daysAfterModificationGreaterThan": 2555 }
        },
        "snapshot": { "delete": { "daysAfterCreationGreaterThan": 90 } },
        "version": { "delete": { "daysAfterCreationGreaterThan": 90 } }
      }
    }
  }]
}
```

See also [[Storage Actions vs Tasks vs Lifecycle]].

### Data Protection Features
| Feature                   | Protects Against                 | Granularity                         | Notes                                                                                                       |
| ------------------------- | -------------------------------- | ----------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Blob soft delete**      | Deleted/overwritten blobs        | Per blob                            | Retention **1–365 days**                                                                                    |
| **Container soft delete** | Deleted containers               | Per container                       | Retention **1–365 days**; restores the container **with the same name** — does not recover individual blobs |
| **Blob versioning**       | Overwrites                       | Automatic version per write         | Each version billed; combine with lifecycle to delete old versions                                          |
| **Snapshots**             | Point-in-time copy               | Manual, read-only                   | You manage them; billed only for differing blocks                                                           |
| **Point-in-time restore** | Bulk accidental delete/overwrite | Container or prefix                 | **Requires** versioning + change feed + blob soft delete                                                    |
| **Immutability (WORM)**   | Any delete/modify                | Container or version                | Legal hold or time-based retention                                                                          |
| **Change feed**           | —                                | Ordered, durable log of all changes | Prerequisite for PITR and object replication                                                                |
| **Object replication**    | —                                | Async copy to another account       | Needs versioning + change feed on source                                                                    |

> [!warning] Ordering trap
> Soft delete, versioning and snapshots **do not protect against deleting the storage account or the whole container's contents at scale** — that's what **point-in-time restore** and **resource locks** are for. And nothing in this table survives account deletion: pair with a **`CanNotDelete` lock**.

### Immutable Storage (WORM)
Two policy types, both of which prevent delete and overwrite:
* **Time-based retention policy** — data is immutable for *N* days. In **unlocked** state you can shorten/lengthen/delete it (for testing, max 5 unlocked edits). Once **locked** it can **only be extended** (max 5 extensions) and **cannot be deleted or shortened** — this is the compliance answer.
* **Legal hold** — immutable indefinitely until the hold tag is explicitly removed. Use when the retention period is unknown (e.g. live litigation).

Scope: **container-level** (applies to all blobs) or **version-level** (needs versioning; can be set per blob or as a container default).
* Must enable **version-level immutability support** at account or container creation for version-level scope.
* Reads are always allowed. **Creating** new blobs is allowed; modifying or deleting existing ones is not.
* "Prevent deletion for 7 years / SEC 17a-4 / regulatory compliance" → **locked time-based retention policy**.

### Object Replication
* Asynchronously copies **block blobs** from a **source** account/container to a **destination** account/container.
* **Prerequisites: blob versioning on both accounts, and change feed on the source.**
* Replication policy = up to **10 rules**, each mapping one source container to one destination container, with optional prefix filter and "copy blobs created after" setting.
* Can be **cross-region, cross-subscription, and cross-tenant** (if cross-tenant replication is allowed).
* Does **not** copy snapshots; metadata and tiers are replicated. Destination can then apply its own lifecycle policy (e.g. tier to archive).
* **Use cases:** reduce read latency by placing data near consumers, compute in a second region, cost optimisation by archiving the replica.
* Contrast with **GRS** — GRS is whole-account, Microsoft-managed, secondary not writable; object replication is **container-level, you control it, and the destination is a normal writable account**.

### Static Website Hosting
* Enable on the account → creates the **`$web`** container, served from `https://<account>.z##.web.core.windows.net`.
* Set **index document** and **error document path**.
* Supports custom domains; for HTTPS on a custom domain, front it with **Azure CDN / Front Door** (see [[Azure Front Door & CDN]]).
* No server-side code — static only.

---
## 5. Azure Files

### Protocols
|  |**SMB**|**NFS (v4.1)**|
|---|---|---|
|Accounts|Standard **and** Premium|**Premium file shares only** (FileStorage)|
|Clients|Windows, Linux, macOS|Linux only|
|Auth|Identity-based (Entra/AD) or storage key|**No identity-based auth** — host/network-based (needs private endpoint or service endpoint)|
|Encryption in transit|SMB 3.x encryption|Not encrypted — must restrict to a VNet|
|Snapshots|✅|✅|
|AD integration|✅|❌|

### Tiers
|Tier|Media|Account Kind|Notes|
|---|---|---|---|
|**Premium**|SSD|FileStorage|Provisioned model, low latency, high IOPS, supports NFS|
|**Transaction optimised**|HDD|GPv2|Standard, highest storage price of the HDD tiers, cheapest transactions|
|**Hot**|HDD|GPv2|Balanced — team shares|
|**Cool**|HDD|GPv2|Cheapest storage, most expensive transactions — archival/backup shares|

* **Standard (HDD)** shares: pay-as-you-go, max **100 TiB** with large file shares enabled (**must enable large file shares** on the account; not supported with GRS/GZRS in some cases — default limit is 5 TiB without it).
* **Premium (SSD)** shares: **provisioned** capacity, IOPS and throughput scale with provisioned size, max **100 TiB**.
* You **cannot convert** between Standard and Premium — create a new share and copy (AzCopy / Robocopy).
* Changing tier within Standard incurs transaction charges.

### Identity-Based Authentication (SMB)
One identity source per storage account, and you must disable one before enabling another.

|Source|Identities|Needs line of sight to DCs?|Linux clients|
|---|---|---|---|
|**On-prem AD DS**|Hybrid (synced via Entra Connect / cloud sync)|✅ Yes|✅|
|**Microsoft Entra Domain Services**|Cloud identities in the managed domain|Client must be **joined to the managed domain**|✅|
|**Microsoft Entra Kerberos**|Hybrid + cloud-only (cloud-only is preview)|❌ **No** — Entra ID issues the tickets|❌ Not supported|

* **Kerberos must be enabled** in the account's SMB security settings for any of these.
* Access is a **two-layer check**: **share-level RBAC role** (Storage File Data SMB Share Reader / Contributor / Elevated Contributor) **then NTFS/directory ACLs** inside the share.
* "Entra-joined devices, no domain controller connectivity, no on-prem AD" → **Microsoft Entra Kerberos**.
* "On-prem AD users with existing NTFS ACLs" → **AD DS** (this is the common AZ-104 answer).
* Without identity-based auth, clients authenticate with the **storage account key** as the NTLM user `Azure\<account-name>`.

### Mounting
**Windows** (PowerShell / `net use`, port **445** outbound must be open):
```powershell
cmdkey /add:"<account>.file.core.windows.net" `
       /user:"localhost\<account>" /pass:"<storage-key>"
New-PSDrive -Name Z -PSProvider FileSystem `
       -Root "\\<account>.file.core.windows.net\<share>" -Persist
```

**Linux** (SMB, needs `cifs-utils`):
```bash
sudo mount -t cifs //<account>.file.core.windows.net/<share> /mnt/share \
  -o vers=3.1.1,username=<account>,password=<key>,dir_mode=0777,file_mode=0777,serverino
```

**Linux** (NFS 4.1, Premium only, no credentials):
```bash
sudo mount -t nfs <account>.file.core.windows.net:/<account>/<share> /mnt/share -o vers=4,minorversion=1,sec=sys
```

> [!warning] Port 445
> Most ISPs block outbound **TCP 445**, so mounting over the internet usually fails. Fixes: use a **VPN / ExpressRoute with a private endpoint**, or use **Azure File Sync** instead.

### Protection
* **Share snapshots** — read-only point-in-time copies, incremental, up to **200 per share**. Taken manually, by Azure Backup, or via File Sync. Users can self-restore from **Previous Versions** in Windows Explorer.
* **Soft delete for file shares** — account-level setting, retention **1–365 days** (default 7). Recovers a **deleted share**, not individual files. Deleted shares go into a "soft deleted" state and can be undeleted.
* **Azure Backup for Azure Files** — snapshot-based, scheduled, with vault-level retention. See [[Backup]].
* **Large file shares** must be enabled for shares over 5 TiB.

### Azure File Sync
Turns a Windows Server into a cache of an Azure file share. See [[Sync Group]] and [[Cloud Tiering]].

**Components:**
* **Storage Sync Service** — the top-level Azure resource.
* **Sync group** — defines the sync topology for **one** Azure file share.
* **Cloud endpoint** — the Azure file share (**one per sync group**).
* **Server endpoint** — a path on a registered Windows Server (volume, folder, or share). Multiple servers per sync group = multi-site sync.
* **Registered server** — the server with the **Azure File Sync agent** installed, trusted with the Storage Sync Service.

**Cloud tiering:**
* Keeps **frequently accessed files local**, replaces cold files with **pointers (reparse points)** and recalls on access.
* Policies: **volume free space %** (always honoured first) and optional **date policy** (tier files not accessed in N days).
* Only the **namespace** is always kept locally, so the server sees every file.
* Not supported on the system volume or with some antivirus/backup products that read full file content.

**Exam facts:**
* Windows Server **2016+** (2012 R2 with limits); requires the agent, **not** supported on Linux.
* Use for: **lift-and-shift of an on-prem file server**, multi-site sync, branch offices, reducing on-prem storage, and **cloud-side backup** of on-prem data.
* A storage account **with Azure File Sync cannot be failed over** (customer-managed failover unsupported).
* Changes made directly in the Azure file share are detected by a **change detection job that runs every 24 hours**; server-side changes sync immediately.

---
## 6. Tools

| Tool                               | What It Is                                                                                   | Use When                                                                                                                                            |
| ---------------------------------- | -------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Azure Storage Explorer**         | Free cross-platform GUI desktop app                                                          | Browse/manage blobs, files, queues, tables, ADLS; attach via Entra ID, SAS, key, or connection string; manage ACLs                                  |
| **AzCopy**                         | Command-line copy utility                                                                    | Bulk upload/download, account-to-account copy (**server-to-server**, no local hop), sync, resumable jobs. Auth with `azcopy login` (Entra) or a SAS |
| **Azure Portal / Cloud Shell**     | Browser                                                                                      | Ad-hoc, small files                                                                                                                                 |
| **Azure PowerShell / CLI**         | Scripted                                                                                     | Automation                                                                                                                                          |
| **Azure Import/Export**            | Ship your own disks to a datacentre; now surfaced as part of the **Azure Data Box** resource | Very large offline transfer where network is impractical; export needs a **WAImportExport** journal file                                            |
| **Azure Data Box / Data Box Disk** | Microsoft-supplied appliance (Disk = import only)                                            | Terabyte-to-petabyte offline migration                                                                                                              |
| **Azure Storage Mover**            | Managed migration service, agent-based                                                       | Server-side migration of SMB/NFS/S3 into Azure. See [[Azure Storage Mover]]                                                                         |
| **Robocopy**                       | Windows copy tool                                                                            | Copying into a **mounted** Azure file share preserving ACLs                                                                                         |
| **Azure Data Factory**             | Pipeline/ETL                                                                                 | Scheduled, transformed data movement                                                                                                                |

**AzCopy essentials:**
```bash
azcopy login                                     # Entra ID auth
azcopy copy "/local/data" "https://acct.blob.core.windows.net/cont?<SAS>" --recursive
azcopy sync "/local/data" "https://acct.blob.core.windows.net/cont" --delete-destination=true
azcopy copy "https://src.blob.../cont?<SAS>" "https://dst.blob.../cont?<SAS>" --recursive
azcopy jobs resume <job-id>
```
* `copy` = one-way copy; `sync` = make destination match source (can delete).
* AzCopy needs **Storage Blob Data Contributor** (not just Contributor) when using Entra ID.

---
## 7. Lower Priority — Know What They Are

* **Queue storage** — simple, durable message queue, messages up to **64 KB**, up to **7 days** default TTL (can be infinite). For decoupling app components. Contrast with **Service Bus** (enterprise messaging, sessions, topics, FIFO, 256 KB/100 MB) — see [[Azure Message Bus]].
* **Table storage** — schemaless **NoSQL key/value** store, partitioned by **PartitionKey + RowKey**. The premium/global option is **Cosmos DB Table API**.
* **Azure Data Lake Storage Gen2** — big data analytics on blob storage. **Enabled by turning on the hierarchical namespace (HNS)** at account creation. Gives real directories, atomic directory rename, and **POSIX ACLs** on top of RBAC. See [[Azure Data Lake Storage]] and [[Hierarchical Namespace - POSIX ACLs]].
  * HNS can be enabled at creation or by **upgrading** an existing GPv2 account — but **upgrading is one-way**.
  * Endpoint is `dfs.core.windows.net`; blob endpoint still works.
* **Azure managed disks / page blobs** — see [[Azure Managed Disks]] and [[Azure Disks]].

---
## 8. Exam Scenario → Answer (and *why*)

| Scenario                                                                | Answer                                                             | Why that one                                                                                                                                                                                                                                 |
| ----------------------------------------------------------------------- | ------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| "Users need read access to a container for 24 hours"                    | **User delegation SAS**                                            | Time-bound and scoped, and signed with an **Entra credential** rather than the account key — so it's auditable to an identity and revocable without rotating keys. Account/service SAS would expose the key's blast radius                   |
| "Must survive a regional outage **and be readable during it**"          | **RA-GZRS** (or **RA-GRS**)                                        | The **RA-** prefix is the only way to read the secondary without a failover. Pick **RA-GZRS** if the region has availability zones (covers zone *and* region failure); **RA-GRS** if it doesn't                                              |
| "Data accessed rarely, must be retrievable **within hours**"            | **Archive** — if "immediately" then **Cool/Cold**                  | "Within hours" matches archive's rehydration (standard up to 15 h, high priority < 1 h). If the wording is "immediately available" or "milliseconds", archive is wrong because it's **offline** — use Cool (30-day min) or Cold (90-day min) |
| "Prevent deletion for 7 years"                                          | **Locked time-based retention policy** (immutable storage)         | Once locked it can only be **extended**, never shortened or deleted — not even by an Owner. Soft delete and locks can be undone by an admin, so they fail a compliance requirement                                                           |
| "Prevent deletion, retention period unknown / ongoing investigation"    | **Legal hold**                                                     | Indefinite until the tag is removed; a time-based policy needs a fixed number of days                                                                                                                                                        |
| "Sync on-prem file server to Azure, keep local access fast"             | **Azure File Sync** with **cloud tiering**                         | Keeps hot files on the server for LAN-speed access while the full namespace and cold data live in Azure. A plain file share mount would depend on WAN latency and port 445                                                                   |
| "Only VMs in subnet X may reach the account"                            | **Service endpoint** + firewall rule                               | Cheapest way to restrict to a subnet. Choose a **private endpoint** instead if on-prem/ExpressRoute access or removal of the public endpoint is required                                                                                     |
| "Reach storage privately from on-premises"                              | **Private endpoint** + Private DNS zone                            | Service endpoints don't extend across VPN/ExpressRoute; only a private endpoint gives a routable private IP in the VNet                                                                                                                      |
| "Restore many blobs deleted yesterday across a container"               | **Point-in-time restore**                                          | Blob soft delete restores items one by one; PITR restores a container/prefix to a timestamp. Needs versioning + change feed + soft delete turned on **beforehand**                                                                           |
| "Revoke a SAS immediately without downtime"                             | **Delete the stored access policy**                                | Regenerating the account key kills *every* SAS and every app using the key; deleting the policy kills only the tokens bound to it                                                                                                            |
| "Reduce read latency for users in another region, with a writable copy" | **Object replication**                                             | GRS secondary is read-only and not selectable per container; object replication gives a normal writable destination account you control                                                                                                      |
| "Developer must read blobs with their own account, least privilege"     | **Storage Blob Data Reader** on the container                      | Reader (control plane) grants no data access; Contributor grants far too much and leaks the keys                                                                                                                                             |
| "Azure Backup must write to a firewalled account"                       | **Allow trusted Microsoft services** (or a resource instance rule) | Trusted services authenticate with a managed identity and bypass the IP firewall; adding Azure datacentre IPs is unsupported                                                                                                                 |
| "NFS file share"                                                        | **Premium file shares (FileStorage)** + private/service endpoint   | NFS is Premium-only and has no identity-based auth, so it must be network-restricted                                                                                                                                                         |
| "Entra-joined laptops, no on-prem AD, SMB share"                        | **Microsoft Entra Kerberos**                                       | Entra ID issues the tickets, so no line of sight to a domain controller is needed; AD DS and Entra Domain Services both require DC connectivity                                                                                              |
| "Enable analytics with directories and POSIX ACLs"                      | **Hierarchical namespace** (ADLS Gen2)                             | HNS is the single switch that turns a GPv2 account into Data Lake Gen2                                                                                                                                                                       |
| "Move 500 TB with poor WAN bandwidth"                                   | **Azure Data Box** (or Import/Export)                              | Offline shipping beats the network at that scale; AzCopy would take too long                                                                                                                                                                 |
| "Need double encryption at rest"                                        | **Infrastructure encryption**                                      | Must be enabled **at account creation** — can't be retrofitted                                                                                                                                                                               |
| "Must control and revoke the encryption key"                            | **Customer-managed key** in Key Vault                              | Disabling the key in the vault revokes access; MMK gives you no key control                                                                                                                                                                  |

---
## 9. Portal Practice That Pays Off

- [x] Create a GPv2 account, then change **replication**, **access tier** and **networking** settings
- [ ] Generate each **SAS type** (account, service, user delegation), then **revoke one via a stored access policy**
- [ ] Set a **lifecycle rule** (hot → cool → archive → delete) and read the generated JSON
- [ ] Apply an **unlocked time-based retention policy**, then lock it and try to delete a blob
- [ ] Lock the account to one VNet with a **service endpoint**, then swap to a **private endpoint** and watch DNS resolution change
- [ ] Create a file share, **mount it from a VM**, take a **snapshot**, restore a file from Previous Versions
- [ ] Assign **Storage Blob Data Reader** to yourself, remove Contributor, and prove you can read but not write
- [ ] Enable **versioning + change feed + soft delete**, delete blobs, then run a **point-in-time restore**
- [ ] Run an **AzCopy** server-to-server copy between two accounts with SAS
- [ ] Trigger a **customer-managed planned failover** on a test GRS account and check the redundancy afterwards

---
## 10. One-Line Exam Facts

* Account name: **3–24 chars, lowercase + numbers, globally unique**
* **Premium accounts: LRS or ZRS only** — never geo-redundant
* Performance tier (Standard/Premium) is **fixed at creation**
* **Infrastructure encryption** and **hierarchical namespace** are creation-time decisions (HNS can be upgraded one-way)
* **SSE is always on**, 256-bit AES, cannot be disabled
* **Archive tier: blob-level only**, 180-day minimum, **offline**
* **Cool = 30 days, Cold = 90 days, Archive = 180 days** minimum retention
* Rehydration: **standard ≤ 15 h, high priority < 1 h** (under 10 GB)
* **Max 5 stored access policies** per container/share/queue/table
* **User delegation SAS max life = 7 days**
* Soft delete retention (blob, container, file share): **1–365 days**
* **Max 200 snapshots** per file share
* **Lifecycle management runs once a day**; a new policy can take **48 hours** to first take effect
* Object replication needs **versioning on both + change feed on source**; max **10 rules** per policy
* PITR needs **versioning + change feed + blob soft delete**
* **Unplanned failover → account becomes LRS**, original primary deleted, only GRS/RA-GRS recoverable
* **NFS = Premium file shares only**, no identity-based auth
* **Entra Kerberos = no domain controller line of sight**, Windows clients only
* One **identity source per storage account** for Azure Files
* **One cloud endpoint per sync group**; cloud tiering always keeps the **namespace** local
* Azure File Sync change detection from the Azure side: **every 24 hours**
* **Reader** (control plane) ≠ data access; **Storage Blob Data Reader** is what reads blobs
* **Contributor can read access keys**; Reader cannot
* **TCP 445** must be open outbound to mount SMB over the internet
