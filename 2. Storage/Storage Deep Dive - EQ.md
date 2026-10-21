Practice questions for [[Storage Deep Dive]]. Sections mirror that note so you can jump back to whatever you miss.

---
## 1. Storage Account Basics

**You need a storage account that will host SMB file shares, blob containers and queues for a line-of-business application. Which account type should you choose?**
- Premium block blobs
- *Standard general-purpose v2 (correct)*
- Premium page blobs

**Your organisation requires an NFS 4.1 file share for a Linux workload. Which storage account type must you create?**
- Standard general-purpose v2 with large file shares enabled
- *Premium file shares (FileStorage) (correct)*
- Premium block blobs

**A storage account was created as Standard general-purpose v2. The application now requires single-digit millisecond latency on block blobs. What must you do?**
- Change the performance tier from Standard to Premium on the existing account
- Change the account's access tier to Hot and enable last access time tracking
- *Create a new Premium block blob account and copy the data with AzCopy (correct)*

**Your company has a compliance requirement that data must be geo-replicated to a secondary region. The application team has requested a Premium block blob account for performance. What should you tell them?**
- Premium block blob accounts support GRS but not GZRS
- *Premium accounts support only LRS or ZRS, so geo-redundancy is not possible (correct)*
- Premium accounts support RA-GZRS once large file shares are enabled

**Which storage account name is valid?**
- *contosodata01 (correct)*
- Contoso-Data-01
- contoso_data_prod_eastus_01

**You are reviewing an application's configuration and find the endpoint `https://contosodata.dfs.core.windows.net`. Which capability is this account using?**
- Azure Files over NFS
- *Azure Data Lake Storage Gen2 (hierarchical namespace) (correct)*
- Static website hosting

**Which blob type should be used to back an unmanaged disk for a virtual machine?**
- Block blob
- Append blob
- *Page blob (correct)*

**An application writes continuous log entries and only ever adds data to the end of the file. Which blob type is designed for this?**
- Block blob
- *Append blob (correct)*
- Page blob

---
## 2. Redundancy and Failover

**Your organisation needs storage that survives the loss of an entire availability zone but has a regulatory requirement that data must not leave the primary region. Which redundancy option should you configure?**
- Geo-redundant storage (GRS)
- *Zone-redundant storage (ZRS) (correct)*
- Read-access geo-zone-redundant storage (RA-GZRS)

**An application must continue to read data during a full regional outage, without waiting for Microsoft or an administrator to initiate a failover. Which redundancy options satisfy this?**
- GRS or GZRS
- ZRS or GZRS
- *RA-GRS or RA-GZRS (correct)*

**Which statement about geo-replication in Azure Storage is correct?**
- Data is replicated synchronously to the secondary region, so there is no risk of data loss
- *Replication to the secondary region is asynchronous, so data written after the Last Sync Time can be lost (correct)*
- Replication to the secondary region is synchronous for GZRS and asynchronous for GRS

**You initiate a customer-managed unplanned failover on an RA-GZRS account following a regional outage. What is the redundancy configuration of the account immediately afterwards?**
- It remains RA-GZRS in the new primary region
- It becomes RA-GRS with the original primary as the new secondary
- *It becomes locally redundant storage (LRS) in the new primary region (correct)*

**After completing an unplanned failover, your manager asks you to restore zone redundancy by converting the account to GZRS. What should you tell them?**
- Conversion to GZRS is supported once the original primary region is available again
- *Conversion to ZRS, GZRS or RA-GZRS is not supported after an unplanned failover; only GRS or RA-GRS can be reconfigured (correct)*
- Conversion to GZRS is supported but requires the account to be converted to ZRS first

**Which value should you check before initiating an unplanned failover to understand the potential data loss?**
- *Last Sync Time (correct)*
- Geo-replication lag threshold in the account's diagnostic settings
- The secondary region's availability SLA

**Your team wants to run a disaster recovery drill on a GZRS account without any data loss, and be able to fail back afterwards. Which option should you use?**
- Unplanned failover, then reconfigure geo-redundancy
- *Planned failover (correct)*
- Microsoft-managed failover requested through a support ticket

**A storage account is configured with Azure File Sync. The business continuity team asks you to document the customer-managed failover procedure for it. What is the correct response?**
- Failover is supported but cloud tiering must be disabled first
- *Customer-managed failover is not supported for storage accounts used with Azure File Sync (correct)*
- Failover is supported only for the file service, not the blob service

**After a storage account failover completes, what happens to the account's primary endpoint URL?**
- A new URL is generated in the secondary region and must be updated in all applications
- *The URL stays the same; the DNS entry for the primary endpoint now points to the new primary region (correct)*
- The URL stays the same but applications must append the secondary region suffix

**Which redundancy options offer 16 nines of durability?**
- LRS and ZRS
- ZRS and GZRS only
- *GRS, GZRS and their read-access variants (correct)*

**You need to add zone redundancy to an existing LRS account in a region that supports availability zones, with no downtime. What is the correct approach?**
- Create a new ZRS account and migrate the data with AzCopy, as conversion is unsupported
- *Perform a redundancy conversion from LRS to ZRS (correct)*
- Change the account's performance tier to Premium, which enables ZRS automatically

---
## 3. Networking and Security

**Your organisation requires that an Azure storage account is reachable from an on-premises datacentre over ExpressRoute using a private IP address, with no public endpoint exposure. What should you configure?**
- A virtual network service endpoint on the gateway subnet
- *A private endpoint with a corresponding private DNS zone (correct)*
- A storage firewall rule allowing the on-premises public IP range

**You need to restrict a storage account so that only virtual machines in a specific subnet can access blob data. Cost must be minimised and on-premises access is not required. What should you configure?**
- A private endpoint for the blob sub-resource
- *A virtual network service endpoint on the subnet, with a matching network rule on the account (correct)*
- A network security group rule denying all outbound traffic except to the storage service tag

**Which statement correctly contrasts service endpoints and private endpoints?**
- Both assign a private IP from the virtual network to the storage account
- *A service endpoint keeps traffic on the Azure backbone but the account still resolves to a public IP, whereas a private endpoint assigns a private IP from your virtual network (correct)*
- A service endpoint is configured per sub-resource, whereas a private endpoint covers the whole storage account

**You create a private endpoint for a storage account, but virtual machines in the VNet still connect to the public IP address. What is the most likely cause?**
- The private endpoint was created for the wrong subscription
- *The private DNS zone is missing or not linked to the virtual network, so the name still resolves publicly (correct)*
- Secure transfer required is disabled on the storage account

**An application needs private access to both blob containers and file shares on the same storage account. How many private endpoints are required?**
- One, because a private endpoint covers the entire storage account
- *Two, because a private endpoint targets a single sub-resource (correct)*
- One, provided the hierarchical namespace is enabled

**You add a firewall rule to a storage account using the CIDR range 10.1.0.0/24 to allow a subnet, but the rule is rejected. Why?**
- Firewall rules accept only single IP addresses, not CIDR ranges
- *Storage firewall rules accept public IP ranges only; private RFC 1918 addresses are rejected (correct)*
- The account must be converted to Premium before firewall rules can be added

**Azure Backup must write recovery points to a storage account whose public network access is set to "Enabled from selected virtual networks and IP addresses". What is the supported way to allow this?**
- Add the published Azure datacentre IP ranges for the region as firewall rules
- *Enable the "Allow trusted Microsoft services to access this storage account" exception (correct)*
- Set the default network action to Allow for the duration of each backup job

**You want to grant a firewall exception to one specific Azure resource rather than to all trusted Microsoft services. What should you configure?**
- A service endpoint policy on the resource's subnet
- *A resource instance rule on the storage account (correct)*
- A managed identity with the Storage Account Contributor role

**Which storage account setting forces all clients to use HTTPS?**
- Minimum TLS version
- *Secure transfer required (correct)*
- Permitted scope for copy operations

**A container's public access level cannot be changed from Private to Blob, and the option is greyed out in the portal. What is the cause?**
- The account is using customer-managed keys
- *"Allow blob anonymous access" is disabled at the storage account level (correct)*
- The container has an active legal hold

**Your security team requires that all access to blob data is authorised through Microsoft Entra ID and that account keys cannot be used. Which setting should you configure?**
- Set the minimum TLS version to 1.2 and enable secure transfer required
- *Set `allowSharedKeyAccess` to false on the storage account (correct)*
- Assign the Storage Blob Data Owner role and remove all Contributor assignments

**After disabling shared key access on a storage account, an application that uses a service SAS stops working. Why?**
- Service SAS tokens require the hierarchical namespace to be enabled
- *Service and account SAS tokens are signed with the account key, so they stop working when shared key access is disabled (correct)*
- Service SAS tokens must be regenerated with a minimum TLS version of 1.2

---
## 4. Encryption

**Which statement about encryption at rest in Azure Storage is correct?**
- It must be enabled during account creation and cannot be added later
- *All data is encrypted with 256-bit AES by default and encryption cannot be disabled (correct)*
- It applies to blob and file data only, not to queues and tables

**Your organisation must be able to revoke access to encrypted storage data by disabling the encryption key. What should you configure?**
- Infrastructure encryption with Microsoft-managed keys
- *Customer-managed keys stored in Azure Key Vault (correct)*
- An encryption scope using Microsoft-managed keys

**You are configuring customer-managed keys for a storage account. Which two Key Vault settings are prerequisites?**
- Private endpoint and firewall restrictions
- RBAC authorisation and a service endpoint
- *Soft delete and purge protection (correct)*

**How does a storage account authenticate to Azure Key Vault when using customer-managed keys?**
- With the storage account's primary access key
- *With a system-assigned or user-assigned managed identity (correct)*
- With a user delegation SAS issued by the Key Vault

**A regulatory requirement states that storage data must be encrypted twice, with a second independent layer applied at the infrastructure level. The storage account already exists and holds production data. What should you tell the compliance team?**
- Enable infrastructure encryption on the existing account from the Encryption blade
- *Infrastructure encryption can only be enabled at account creation, so a new account and data migration are required (correct)*
- Apply an encryption scope with infrastructure encryption to each existing container

**Different containers in the same storage account must be encrypted with different customer-managed keys. What should you configure?**
- A separate storage account per key, as keys are account-level only
- *Encryption scopes (correct)*
- Customer-provided keys passed in each request header

---
## 5. Access Control, SAS and RBAC

**External users need read access to a specific blob container for 24 hours. Your security policy forbids any credential derived from the storage account keys. What should you issue?**
- An account SAS restricted to the blob service with read permission
- A service SAS bound to a stored access policy
- *A user delegation SAS (correct)*

**Which SAS type is secured with Microsoft Entra ID credentials rather than the storage account key?**
- Account SAS
- Service SAS
- *User delegation SAS (correct)*

**What is the maximum validity period of a user delegation SAS?**
- 24 hours
- *7 days (correct)*
- 90 days

**A user must be able to create user delegation SAS tokens for a container. Which role grants the ability to request the user delegation key?**
- Storage Account Key Operator Service Role
- *Storage Blob Delegator (correct)*
- Reader and Data Access

**You need to issue a SAS that permits listing containers and reading the blob service properties at the account level. Which SAS type is required?**
- Service SAS
- *Account SAS (correct)*
- User delegation SAS

**A service SAS has been distributed to several partners and must be revoked immediately. Regenerating the account keys would break unrelated production applications. What is the correct approach?**
- Set the account's minimum TLS version to 1.3, which invalidates existing tokens
- *Delete the stored access policy that the SAS tokens reference (correct)*
- Add a firewall rule denying the partners' IP addresses

**On which resources can a stored access policy be defined?**
- The storage account, so that it applies to every service
- *A blob container, file share, queue or table (correct)*
- An individual blob or file

**What is the maximum number of stored access policies that can be defined on a single blob container?**
- 3
- *5 (correct)*
- 10

**Why can an account SAS not be associated with a stored access policy?**
- Account SAS tokens are signed with a user delegation key rather than the account key
- *Stored access policies are defined on a container, share, queue or table, whereas an account SAS is scoped above that level (correct)*
- Account SAS tokens have a hard-coded maximum lifetime of 7 days

**You examine a SAS URI and find the parameters `skoid` and `sktid`. What does this tell you?**
- The SAS is an account SAS covering multiple services
- The SAS references a stored access policy
- *The SAS is a user delegation SAS, signed with an Entra ID identity (correct)*

**In a SAS token, which parameter should always be set to `https` to protect data in transit?**
- `sv`
- *`spr` (correct)*
- `sip`

**A developer has been assigned the Reader role on a storage account but cannot read blob data with their Entra ID credentials. What is the cause?**
- Role assignments take up to 24 hours to propagate
- *Reader is a control-plane role and grants no data-plane access; a data role such as Storage Blob Data Reader is required (correct)*
- The developer must also be assigned the Storage Account Key Operator Service Role

**A junior administrator holds the Contributor role on a storage account. Your security review flags this as excessive for data protection. Why?**
- Contributor includes the Storage Blob Data Owner role by inheritance
- *Contributor can read the account access keys, which grants full access to all data (correct)*
- Contributor allows anonymous public access to be enabled on containers

**An application's managed identity must read, write and delete blobs in a single container, and nothing else. What is the least-privilege assignment?**
- Storage Blob Data Contributor at the subscription scope
- *Storage Blob Data Contributor scoped to that container (correct)*
- Storage Account Contributor scoped to the storage account

**Which role allows a user to list and regenerate storage account keys without granting any other management permissions?**
- Storage Account Contributor
- *Storage Account Key Operator Service Role (correct)*
- Reader and Data Access

**You need to modify NTFS permissions on files inside an Azure file share using identity-based authentication. Which share-level role is required?**
- Storage File Data SMB Share Contributor
- *Storage File Data SMB Share Elevated Contributor (correct)*
- Storage File Data Privileged Reader

**What is the correct key rotation sequence for a storage account?**
- Regenerate key1, regenerate key2, then update all applications to key1
- *Switch applications to key2, regenerate key1, switch applications back to key1, regenerate key2 (correct)*
- Regenerate both keys simultaneously, then update all applications within the grace period

---
## 6. Blob Storage Tiers and Lifecycle

**A dataset is accessed roughly once a month and must be available immediately when requested, with the lowest possible storage cost for an online tier. Which access tier should you use?**
- Hot tier
- *Cold tier (correct)*
- Archive tier

**Compliance records must be retained for seven years, are almost never read, and may take several hours to retrieve. Which access tier is most cost-effective?**
- Cool tier
- Cold tier
- *Archive tier (correct)*

**What is the minimum recommended retention period for data in the cold tier before an early deletion fee applies?**
- 30 days
- *90 days (correct)*
- 180 days

**A blob is moved to the archive tier and then deleted after 45 days. What is the billing consequence?**
- No additional charge, because archive storage is billed monthly in arrears
- *An early deletion fee equivalent to 135 days of archive storage is charged (correct)*
- An early deletion fee equivalent to 45 days of hot tier storage is charged

**You attempt to set the archive tier as the default access tier on a storage account, but the option is not available. Why?**
- The account must have the hierarchical namespace enabled first
- *Archive can only be set at the individual blob level, not as the account default (correct)*
- The account must be converted to a Premium block blob account first

**An application must read a 4 GB blob currently in the archive tier, and the business requires it within the hour. Which rehydration option should you choose?**
- Standard priority rehydration
- *High priority rehydration (correct)*
- Change the account's default access tier to Hot

**What is the maximum expected completion time for a standard priority rehydration from the archive tier?**
- 1 hour
- 4 hours
- *15 hours (correct)*

**You need to read an archived blob without incurring an early deletion fee and without making the original blob unavailable. What should you do?**
- Change the blob's access tier to Hot, then change it back to Archive after reading
- *Copy the archived blob to a new blob in an online tier, leaving the original in the archive tier (correct)*
- Create a snapshot of the archived blob and read the snapshot

**How can an application be notified automatically when a blob's rehydration from the archive tier has completed?**
- Poll the blob's `x-ms-rehydrate-priority` header every 15 minutes
- *Subscribe to the `BlobTierChanged` event in Azure Event Grid (correct)*
- Enable the change feed and read the latest log segment

**Even though archive blobs are offline, which operation can still be performed on them without rehydration?**
- Reading the blob's content with a ranged GET request
- *Listing the blob and reading its metadata (correct)*
- Creating a snapshot of the blob

**Your organisation wants blobs to move to the cool tier after 30 days, to the archive tier after 90 days, and to be deleted after 7 years, with no manual intervention. What should you configure?**
- Azure Storage Actions with a conditional task
- *A blob lifecycle management policy (correct)*
- An object replication policy with a prefix filter

**How often does a blob lifecycle management policy run?**
- Continuously, as blobs are written
- *Once per day (correct)*
- Every 6 hours

**You create a new lifecycle management policy, but no blobs have been tiered after two hours. What is the most likely explanation?**
- The policy requires the change feed to be enabled before it will run
- *A new or edited policy can take up to 48 hours to take effect on its first run (correct)*
- Lifecycle policies only apply to blobs created after the policy was saved

**A lifecycle rule must act on blobs based on how recently they were read rather than when they were last written. What must be enabled?**
- Blob versioning on the storage account
- *Last access time tracking (correct)*
- The change feed on the source container

**Which lifecycle management filter restricts a rule to blobs in a specific virtual folder path?**
- *Prefix match (correct)*
- Blob index tag match
- Blob type filter

---
## 7. Blob Data Protection

**A user overwrote an important blob yesterday and needs the earlier content back. Which feature, if enabled beforehand, automatically retains the previous content of every write?**
- Blob snapshots
- *Blob versioning (correct)*
- Container soft delete

**An administrator deleted a container containing 4,000 blobs. Container soft delete is enabled with a 14-day retention period. What can you recover?**
- Individual blobs, which must be restored one at a time from the soft-deleted state
- *The container and its contents, restored under the same container name (correct)*
- Only the container's metadata and access policy; the blobs must be restored from a snapshot

**What is the maximum retention period that can be configured for blob soft delete?**
- 90 days
- 180 days
- *365 days (correct)*

**A script accidentally deleted and overwrote thousands of blobs across a container three hours ago. You need to return the container to its state before the script ran. Which feature should you use?**
- Blob soft delete, restoring each affected blob
- *Point-in-time restore (correct)*
- Object replication from the secondary region

**Which three features must be enabled before point-in-time restore can be configured for block blobs?**
- Blob versioning, object replication and container soft delete
- *Blob versioning, change feed and blob soft delete (correct)*
- Change feed, last access time tracking and blob index tags

**Financial records must be retained in an unalterable state for seven years to satisfy a regulatory audit, and no administrator may shorten that period. What should you configure?**
- Container soft delete with a 2,555-day retention period
- A legal hold on the container
- *A locked time-based retention policy (correct)*

**Which statement about a locked time-based retention policy is correct?**
- It can be shortened by an account Owner but not by a Contributor
- *It can only be extended, and cannot be shortened or deleted (correct)*
- It is automatically removed once the retention interval elapses

**Your legal team requires that data relating to an ongoing investigation cannot be deleted. The duration of the investigation is unknown. What should you apply?**
- A locked time-based retention policy set to the maximum interval
- *A legal hold (correct)*
- A `CanNotDelete` resource lock on the storage account

**Which operation is still permitted on a container protected by an active immutability policy?**
- Overwriting an existing blob with new content
- Deleting a blob whose retention interval has not elapsed
- *Reading existing blobs and creating new ones (correct)*

**You want to copy block blobs from a container in East US to a container in a separate, writable storage account in West Europe, and then archive the copies with a lifecycle policy. What should you configure?**
- RA-GZRS redundancy with a lifecycle policy on the secondary region
- *An object replication policy (correct)*
- A planned failover to the West Europe paired region

**Which prerequisites must be in place before an object replication policy can be created?**
- Change feed on both the source and destination accounts
- *Blob versioning on both accounts and the change feed on the source account (correct)*
- Point-in-time restore on the source account and blob soft delete on the destination

**What is the maximum number of rules that can be defined in a single object replication policy?**
- 5
- *10 (correct)*
- 50

**Which of the following is not replicated by an object replication policy?**
- Blob metadata
- Blob access tiers
- *Blob snapshots (correct)*

**You enable static website hosting on a storage account. Which container is created to serve the content?**
- `$root`
- *`$web` (correct)*
- `$logs`

---
## 8. Azure Files

**Which Azure Files configuration supports the NFS 4.1 protocol?**
- Standard general-purpose v2 with large file shares enabled
- *Premium file shares (FileStorage) (correct)*
- Standard general-purpose v2 in the transaction optimised tier

**An NFS file share must be secured. Which statement is correct?**
- NFS shares support Microsoft Entra Kerberos authentication for Linux clients
- *NFS shares have no identity-based authentication, so access must be restricted with a private endpoint or service endpoint (correct)*
- NFS shares are encrypted in transit by default, so no network restriction is required

**Your organisation has on-premises Active Directory users who must access an Azure file share over SMB and retain their existing NTFS permissions. Which identity source should you configure?**
- Microsoft Entra Kerberos
- Microsoft Entra Domain Services
- *On-premises Active Directory Domain Services (AD DS) (correct)*

**Remote workers use Microsoft Entra joined Windows laptops with no connectivity to any domain controller, and your organisation has no on-premises Active Directory. Which identity source supports SMB access to an Azure file share?**
- On-premises Active Directory Domain Services
- Microsoft Entra Domain Services
- *Microsoft Entra Kerberos (correct)*

**Why can Microsoft Entra Kerberos authentication work without line of sight to a domain controller?**
- The storage account acts as a read-only domain controller for the share
- *Microsoft Entra ID issues the Kerberos tickets directly (correct)*
- Kerberos tickets are cached indefinitely on the Entra joined device

**Which limitation applies to Microsoft Entra Kerberos authentication for Azure Files?**
- It supports only Premium file shares
- *Linux client authentication is not supported (correct)*
- It cannot be used with hybrid identities synced from on-premises Active Directory

**You need to change an Azure file share's identity source from Microsoft Entra Domain Services to on-premises AD DS. What must you do first?**
- Create a second storage account, as the identity source is immutable
- *Disable the existing identity source before enabling the new one (correct)*
- Enable both sources and set AD DS as the preferred provider

**With identity-based authentication configured, how is access to files inside an Azure file share determined?**
- Entirely by the share-level RBAC role assignment
- Entirely by NTFS permissions on the directories and files
- *By the share-level RBAC role first, then by the NTFS permissions inside the share (correct)*

**A user cannot mount an Azure file share from a home network, although the credentials are correct and the share is accessible from an Azure VM. What is the most likely cause?**
- The share is in the cool tier, which does not support direct mounting
- *The internet service provider is blocking outbound TCP port 445 (correct)*
- The storage account's minimum TLS version is set to 1.2

**What is the maximum number of snapshots that can be retained for a single Azure file share?**
- 100
- *200 (correct)*
- 365

**Which Windows feature allows end users to restore an earlier version of a file themselves from an Azure file share snapshot?**
- File History
- *Previous Versions in File Explorer (correct)*
- Storage Spaces Direct

**Soft delete is enabled on the file service. An administrator deletes a single file from a share by mistake. What can soft delete recover?**
- The individual file, from the share's soft-deleted items view
- *Nothing — file share soft delete recovers a deleted share, not individual files (correct)*
- The individual file, but only if versioning is also enabled on the share

**A standard file share must grow beyond 5 TiB. What must be enabled?**
- The hierarchical namespace on the storage account
- *Large file shares on the storage account (correct)*
- The Premium performance tier on the file share

---
## 9. Azure File Sync

**An on-premises Windows file server is running out of disk space, but users must continue to access all files at local network speed. Which solution addresses both requirements?**
- Migrate the share to Azure Files and map a drive over the internet
- *Deploy Azure File Sync with cloud tiering enabled (correct)*
- Deploy Azure Storage Mover with a scheduled migration job

**In Azure File Sync, what does a cloud endpoint represent?**
- The registered Windows Server that hosts the local cache
- A path on the server that is synchronised, such as a volume or folder
- *The Azure file share that the sync group synchronises with (correct)*

**How many cloud endpoints can a single Azure File Sync sync group contain?**
- *One (correct)*
- Up to 5
- Up to 100, matching the server endpoint limit

**What does a server endpoint define in an Azure File Sync topology?**
- The Storage Sync Service resource in Azure
- *A specific path on a registered server, such as a volume or folder, that is synchronised (correct)*
- The network path used by clients to reach the Azure file share

**When cloud tiering replaces a file's content with a pointer, what remains on the local server?**
- Nothing; the file disappears from the directory listing until recalled
- *The file's namespace entry, as a reparse point, so the file is still visible (correct)*
- A compressed copy of the file, restored transparently on access

**Which cloud tiering policy is always honoured first?**
- The date policy, which tiers files not accessed within a set number of days
- *The volume free space policy (correct)*
- The maximum file size policy

**A file is added directly to the Azure file share through the portal rather than on the server. How long can it take before the registered servers see the change?**
- Within 5 minutes, as change detection is event-driven
- *Up to 24 hours, because the Azure-side change detection job runs daily (correct)*
- Up to 48 hours, matching the lifecycle management interval

**Which operating system is required for an Azure File Sync server endpoint?**
- Any Linux distribution with the `cifs-utils` package installed
- *Windows Server, with the Azure File Sync agent installed and the server registered (correct)*
- Windows 10 or Windows 11 with the SMB Direct feature enabled

---
## 10. Tools

**You need to copy 20 TB of blobs between two storage accounts in different regions without downloading the data to an intermediate machine. Which tool should you use?**
- Azure Storage Explorer
- *AzCopy, using a server-to-server copy (correct)*
- Robocopy from an Azure virtual machine

**Which AzCopy command makes the destination container match the source exactly, removing files that no longer exist at the source?**
- `azcopy copy` with the `--recursive` flag
- *`azcopy sync` with `--delete-destination=true` (correct)*
- `azcopy jobs resume` with the `--mirror` flag

**You authenticate AzCopy with `azcopy login` using your Entra ID account, which holds the Contributor role on the storage account. Uploads fail with an authorisation error. What should you do?**
- Set the account's minimum TLS version to 1.2
- *Assign a data-plane role such as Storage Blob Data Contributor (correct)*
- Re-authenticate using a service SAS instead, as Entra ID is unsupported by AzCopy

**Which tool provides a free, cross-platform graphical interface for managing blobs, file shares, queues, tables and Data Lake ACLs?**
- Azure Cloud Shell
- *Azure Storage Explorer (correct)*
- Azure Storage Mover

**Your organisation must transfer 500 TB of data to Azure, and the available WAN bandwidth would make an online transfer take months. What should you use?**
- AzCopy with the `--cap-mbps` flag removed to maximise throughput
- *Azure Data Box (correct)*
- Azure Data Factory with a self-hosted integration runtime

**You need to export data from Azure to physical disks shipped back to your datacentre. Which option is not suitable?**
- Azure Data Box
- *Azure Data Box Disk (correct)*
- Azure Import/Export

**You are copying files from an on-premises Windows file server into a mounted Azure file share and must preserve the NTFS access control lists. Which tool is designed for this?**
- AzCopy with the `--preserve-smb-permissions` flag disabled
- *Robocopy (correct)*
- Azure Storage Explorer

**Which managed service uses an agent to migrate on-premises SMB and NFS shares into Azure with centralised job tracking?**
- Azure File Sync
- *Azure Storage Mover (correct)*
- Azure Import/Export

---
## 11. Queue, Table and Data Lake Gen2

**What is the maximum size of a message in Azure Queue storage?**
- 4 KB
- *64 KB (correct)*
- 256 KB

**An application requires message sessions, topics with subscriptions and guaranteed FIFO ordering. Which service should you choose?**
- Azure Queue storage with a 7-day time to live
- *Azure Service Bus (correct)*
- Azure Table storage with a partitioned schema

**Which two properties together form the unique key of an entity in Azure Table storage?**
- AccountKey and RowKey
- *PartitionKey and RowKey (correct)*
- PartitionKey and Timestamp

**How is Azure Data Lake Storage Gen2 enabled on a storage account?**
- By creating a Premium block blob account and enabling the dfs endpoint
- *By enabling the hierarchical namespace (correct)*
- By assigning the Storage Blob Data Owner role and enabling POSIX ACLs

**Which capability does the hierarchical namespace add to a general-purpose v2 account?**
- Synchronous replication across three availability zones
- *True directory objects with atomic directory rename and POSIX ACLs (correct)*
- Identity-based authentication for SMB file shares

**You enable the hierarchical namespace on an existing general-purpose v2 account to support an analytics workload. The project is later cancelled and you are asked to revert the change. What should you tell the team?**
- The hierarchical namespace can be disabled from the Data Lake Gen2 blade
- *Upgrading an account to enable the hierarchical namespace is a one-way operation and cannot be reversed (correct)*
- The namespace can be disabled only after all containers are deleted
