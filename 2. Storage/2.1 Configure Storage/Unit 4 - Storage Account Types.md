 Storage accounts are **Standard** or **Premium**, depending on cost and performance requirements.
---
#### Standard Storage
- **Lower cost per GB**
- Best for:
    - Bulk storage
    - Infrequently accessed data
    - General-purpose workloads
- **GPv2** is the standard account for most scenarios
---
#### Premium Storage
- **Low, consistent latency**
- Best for:
    - High-performance workloads
    - High transaction rates
    - I/O-intensive applications
    - Databases
    - VM disks
---
#### Storage Account Types

|Type|Services|Redundancy|Use|
|---|---|---|---|
|**Standard GPv2**|Blob, Data Lake, Queue, Table, Files|LRS, GRS, RA-GRS, ZRS, GZRS, RA-GZRS|**Most scenarios**|
|**Premium Block Blobs**|Blob / Data Lake|LRS, ZRS|High transaction rates, low latency|
|**Premium File Shares**|Azure Files|LRS, ZRS|High-performance file shares|
|**Premium Page Blobs**|Page blobs|**LRS only**|VM disks, databases|

---

#### Key Facts

- **Standard GPv2** → recommended general-purpose account
- **Premium Block Blobs** → high-performance blob workloads
- **Premium File Shares** → Azure Files; supports **SMB/NFS**
- **Premium Page Blobs** → **VM disks**; LRS only
- **Standard ↔ Premium cannot be converted directly**
    - Create new account → migrate data
- All storage types use **Storage Service Encryption (SSE)** for data at rest
- Legacy **GPv1 / BlobStorage** → Microsoft recommends upgrading to **GPv2**