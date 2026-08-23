## 1. Blob Storage Overview & Types
Azure Blob Storage provides massively scalable, unstructured object storage in the cloud.

### Blob Types
* **Block Blobs:** Standard storage for text and binary data (up to ~190.7 TiB using 50,000 blocks). Optimised for streaming and cloud backups.
* **Append Blobs:** Optimised for append-only operations (e.g., logging). Cannot modify or delete existing blocks.
* **Page Blobs:** Optimised for random read/write operations (512-byte pages, up to 8 TiB). Used as Azure VM VHD disks.

### Storage Account Kinds
* **General-purpose v2 (GPv2):** Supports all blob types, all access tiers (Hot, Cool, Cold, Archive), and all storage features.
* **Premium Block Blobs:** High transaction rates, low and consistent latency (SSD-backed).
* **Premium Page Blobs:** For random read/write storage (unmanaged disks).

---

## 2. Containers & Public Access Levels
Blobs reside within **Containers** (flat hierarchy).

### Container Naming Rules
* 3 to 63 characters, lowercase letters, numbers, and hyphens (`-`) only.
* Must start and end with a letter or number.

### Public Access Levels
* **Private (No anonymous access):** Default. Access requires Microsoft Entra ID, Account Key, or SAS.
* **Blob (Anonymous read for blobs only):** Clients can read blob data anonymously, but cannot enumerate/list blobs in the container.
* **Container (Anonymous read for container and blobs):** Clients can read blob data and list all blobs in the container.

> [!important] Exam Note
> Disabling public access at the **Storage Account level** overrides and blocks all container-level public access settings.

---

## 3. Blob Access Tiers

| Access Tier | Use Case | Storage Cost | Access Cost | Minimum Retention |
| :--- | :--- | :--- | :--- | :--- |
| **Hot** | Active, frequently accessed data | Highest | Lowest | None |
| **Cool** | Infrequently accessed (accessed $\ge$ 30 days) | Lower | Higher | 30 days |
| **Cold** | Rarely accessed (accessed $\ge$ 90 days) | Very Low | Higher than Cool | 90 days |
| **Archive** | Rarely accessed, offline backup/compliance | Lowest | Highest | 180 days |

### Tier Characteristics & Rehydration
* **Default Tier:** Configured at the storage account level (Hot or Cool).
* **Blob-level Tiering:** Can be overridden per individual blob.
* **Archive Tier is Offline:** Data cannot be read directly. Must be **rehydrated** back to an online tier (Hot, Cool, Cold).
  * **Copy Blob (Recommended):** Copies archived blob to an online tier without altering the source archive blob.
  * **Set Blob Tier:** Modifies the tier of the existing blob in place.
* **Rehydration Priority Options:**
  * **Standard:** Completed within 15 hours.
  * **High:** Priority processing; completed typically in under 1 hour (higher cost).

> [!warning] Early Deletion Fee
> Deleting or moving a blob before its minimum retention period (30 days for Cool, 90 days for Cold, 180 days for Archive) incurs a pro-rated early deletion fee.

---

## 4. Blob Lifecycle Management
Automates data movement across tiers and data deletion using JSON-based rule sets.

### Rule Structure
* **Filters:** Target specific blobs by:
  * Blob Type (`blockBlob`, `appendBlob`)
  * Blob Prefix (`container1/photos/`)
  * Blob Index Tags (`Project = Phoenix`)
* **Actions:** Trigger state changes:
  * `tierToCool` / `tierToCold` / `tierToArchive`
  * `delete`

### Evaluation Triggers (Days Since)
* `daysAfterModificationGreaterThan` (Last modified date)
* `daysAfterCreationGreaterThan` (Creation date)
* `daysAfterLastAccessTimeGreaterThan` (Requires *Last Access Time Tracking* enabled)
* Supports actions on **Base Blobs**, **Snapshots**, and **Versions**.

> [!tip] Execution Frequency
> Lifecycle management rules run automatically **once per day**.

---

## 5. Blob Object Replication
Asynchronously copies block blobs between a source and destination storage account across Azure regions or within the same region.

### Requirements & Constraints
* **Block Blobs only.**
* **Blob Versioning** must be enabled on **both** source and destination accounts.
* **Blob Change Feed** must be enabled on the **source** account.
* Destination account must have public network access enabled or specific trusted access.
* One-way asynchronous replication only.

---

## 6. Data Protection & Immutability

### Soft Delete
* **Blob Soft Delete:** Retains deleted blob data for a configurable retention window (1–365 days).
* **Container Soft Delete:** Protects entire deleted containers for 1–365 days.

### Blob Versioning & Snapshots
* **Versioning:** Automatically creates a new version on blob create/overwrite. Preserves complete history.
* **Snapshots:** Read-only point-in-time image of a blob.

### Immutable Blob Storage (WORM - Write Once, Read Many)
* **Time-based Retention Policy:** Blobs cannot be modified or deleted for a specified duration.
  * *Unlocked Policy:* Can increase/decrease retention or delete the policy.
  * *Locked Policy:* Strictly compliant; cannot be deleted or shortened, only extended.
* **Legal Hold:** Retains data indefinitely until explicitly cleared by an administrator.

---

## 7. Blob Storage Pricing Components
1. **Storage Capacity:** Volume of stored data per GB/month across tiers.
2. **Data Access & Operations:** Read, write, list, and tier-change transaction costs.
3. **Data Egress:** Outbound data transfer (inbound is free; cross-region and internet egress incur costs).
4. **Rehydration & Early Deletion:** Charges for early tier movement or priority rehydration.