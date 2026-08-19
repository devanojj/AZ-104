#### Azure Blob Storage
Object storage for **massive amounts of unstructured/nonrelational data**.

**Common Uses**
- Images and documents
- Video/audio streaming
- File storage
- Backup, restore, disaster recovery
- Archiving
- Data analysis

**Key Points**
- Access via **HTTP/HTTPS**
- Globally accessible
- Stores text, binary data, media, etc.
- Access through: URLs, REST API, Azure CLI / PowerShell, Storage client libraries
---

#### Azure Files
Managed **network file shares** in Azure.

**Protocols**
- **SMB** — Server Message Block
- **NFS** — Network File System

**Key Points**
- Multiple VMs can access the same files
- Supports **read/write** access
- Can be accessed through REST API/client libraries
- Useful for migrating applications that already use traditional file shares
- Can store:
    - Configuration files
    - Shared tools/utilities
    - Diagnostic logs
    - Metrics
    - Crash dumps

**Authentication**
- Uses **storage account credentials**
- Mounted users have read/write access to the share

**Exam Tip**
- **Azure Files = shared network file system**
- **SMB/NFS = protocols**

---
#### Azure Queue Storage
**Definition:** Stores messages for **asynchronous processing**.
**Key Points**
- Messages up to **64 KB**
- Queue can contain **millions of messages**
- Creates a backlog of work
- Processing components can scale independently

**Exam Tip**
- **Queue = messages + asynchronous processing**
- Useful for **decoupling** application components

---

#### Azure Table Storage
**Definition:** **NoSQL key/attribute store** for structured, nonrelational data.
**Key Points**
- **Schemaless** design
- Easy to adapt as application requirements change
- Fast and cost-effective
- Often cheaper than traditional SQL for similar data volumes

**Azure Cosmos DB Table API**
- Throughput-optimised tables
- Global distribution
- Automatic secondary indexes

**Exam Tip**
- **Table = structured NoSQL data**
- **Schemaless = flexible structure**

---
#### Choosing Azure Storage

|Requirement|Service|
|---|---|
|Massive unstructured data|**Blob Storage**|
|Images, videos, documents|**Blob Storage**|
|Shared network file system|**Azure Files**|
|SMB / NFS file shares|**Azure Files**|
|Asynchronous message processing|**Queue Storage**|
|Work backlog / decoupling|**Queue Storage**|
|Structured NoSQL data|**Table Storage**|
|Schemaless key/attribute data|**Table Storage**|

---

#### Exam Facts

- **Blob → Objects / unstructured data**
- **Files → Network file shares / SMB / NFS**
- **Queue → Messages / asynchronous processing**
- **Table → Structured NoSQL / schemaless**
- **Blob:** HTTP/HTTPS access
- **Queue messages:** maximum **64 KB**