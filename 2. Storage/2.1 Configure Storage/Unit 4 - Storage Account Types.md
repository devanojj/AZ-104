 Storage accounts are **Standard** or **Premium**, depending on cost and performance requirements.

#### Standard Storage
- **Lower cost per GB**
- Best for: Bulk storage, Infrequently accessed data, General-purpose workloads
- **GPv2** is the standard account for most scenarios
---
#### Premium Storage
- **Low, consistent latency**
- Best for: High-performance workloads, I/O-intensive applications, Databases, VM disks
---
#### Storage Account Types

| Type                    | Services                             | Redundancy                           | Use                                 |
| ----------------------- | ------------------------------------ | ------------------------------------ | ----------------------------------- |
| **Standard GPv2**       | Blob, Data Lake, Queue, Table, Files | LRS, GRS, RA-GRS, ZRS, GZRS, RA-GZRS | **Most scenarios**                  |
| **Premium Block Blobs** | Blob / Data Lake                     | LRS, ZRS                             | High transaction rates, low latency |
| **Premium File Shares** | Azure Files                          | LRS, ZRS                             | High-performance file shares        |
| **Premium Page Blobs**  | Page blobs                           | LRS, ZRS                             | VM disks, databases                 |

---

#### Key Facts
- **Geo-redundancy (GRS/GZRS) = Standard GPv2 only.** All premium types are LRS/ZRS only.
- NFS for premium file share only  
- Data Lake Storage Gen2 is not a separate account type. It is Blob storage with **hierarchical namespace** enabled.
- ZRS availability for premium accounts is region-dependent.
- Legacy **GPv1 / BlobStorage** → Microsoft recommends upgrading to **GPv2**
- **Standard ↔ Premium cannot be converted directly** Create new account → migrate data