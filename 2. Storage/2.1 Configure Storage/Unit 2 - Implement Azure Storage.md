- Microsoft's cloud storage platform for **modern data storage**
- Provides: **Object storage**, **Cloud file shares**, **Messaging store**, **NoSQL store**
- Used by:
    - Web/mobile/desktop applications
    - IaaS VMs
    - PaaS applications
---
#### Data Categories

|Category|Description|Azure Storage|
|---|---|---|
|**Virtual machine data**|Persistent disks and cloud file shares|Managed Disks, Azure Files|
|**Unstructured data**|Non-relational data with little/no fixed structure|Blob Storage, Data Lake Storage|
|**Structured data**|Relational/schema-based data|Table Storage, Cosmos DB, Azure SQL|

---
#### Virtual Machine Data
- **Managed Disks**
    - Persistent **block storage** for Azure VMs
    - Data disks can store: Database files, Website content, Application code
- Number of data disks depends on **VM size**
- **Azure Files**
    - Fully managed cloud **file shares**
---
#### Unstructured Data
- Data without a fixed relational structure
- **Blob Storage:** Highly scalable **object storage** & REST-based
- **Azure Data Lake Storage**
    - Designed for big-data/analytics workloads
    - Provides **HDFS-compatible** storage
    - Built on Blob Storage with hierarchical namespace

---
#### Structured Data
- Data organized using a **schema**
- Usually represented as: Rows, Columns, Keys
- **Azure Table Storage:** Autoscaling **NoSQL** store
- **Azure Cosmos DB:** Globally distributed database
- **Azure SQL Database:** Fully managed **relational database (PaaS)**

---
#### Key Azure Storage Features
**Durability & Availability**
- Data redundancy protects against hardware failures
- Can replicate data across:
    - Datacenters
    - Geographic regions
- Protects against outages and disasters

**Security**
- Azure Storage encrypts **data at rest**
- Fine-grained access control available
- **Microsoft Entra ID + RBAC** can be used for authorization

**Scalability**
- Designed for **massive scale**
- Supports modern application storage and performance requirements

**Manageability**
- Microsoft manages:
    - Hardware maintenance
    - Updates
    - Critical infrastructure issues

**Accessibility**
- Access data globally using **HTTP/HTTPS**
- Supported:
    - Azure Portal
    - Azure Storage Explorer
    - Azure CLI
    - Azure PowerShell
    - REST API
    - SDKs: .NET, Java, Node.js, Python, PHP, Ruby, Go
---
#### Blob Storage Protocol Support
**SFTP**
- Blob Storage supports **SFTP**
- Allows existing SFTP tools to transfer files directly to/from blobs
- Requires **Hierarchical Namespace (HNS)**
- HNS can be enabled:
    - During storage account creation
    - Later under **Settings → Configuration**

**NFSv3**
- Blob Storage supports **NFSv3**
- Linux clients can mount a container as an **NFS share**
- Useful for migrating Linux file workloads to Azure

---
#### Default Authorization
- Azure portal option: **Default to Microsoft Entra authorization**
- Makes **RBAC** the default authorization method instead of shared access keys
- Improves security by reducing reliance on storage account keys

---
#### Exam Tips
- **Blob Storage → unstructured/object data**
- **Azure Files → managed cloud file shares**
- **Managed Disks → VM block storage**
- **Table Storage → NoSQL**
- **Cosmos DB → globally distributed database**
- **Azure SQL → relational database**
- **Data Lake Storage → big data + HDFS**
- **SFTP → requires Hierarchical Namespace**
- **NFSv3 → Linux file workloads**
- **Microsoft Entra ID + RBAC → preferred secure authorization**
- Azure Storage is **highly durable, available, scalable, and encrypted**.