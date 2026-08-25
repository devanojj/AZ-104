block-level storage volumes managed entirely by Microsoft and attached to Azure Virtual Machines. They abstract away physical storage accounts, handling provisioning, replication, scaling, and fault tolerance automatically.

 **High Availability & Durability:** Engineered for 99.999% availability, providing 3 internal replicas within a region.Redundancy options include Locally Redundant Storage (LRS) and Zone-Redundant Storage (ZRS).

**Fault Isolation** 
Integrated with *availability sets* (placing disks across distinct storage clusters/stamps to avoid single points of failure) and *availability zones*.

**Security & Encryption**
_Server-Side Encryption (SSE):_ Default encryption at rest with platform-managed or customer-managed keys (CMK).
_Host-based Encryption:_ Encrypts data directly on the VM host before passing it to storage.
_Azure Disk Encryption (ADE):_ OS-level encryption using BitLocker (Windows) or DM-Crypt (Linux).

**Snapshots & Backups:** Point-in-time, crash-consistent full or incremental copies used for disaster recovery or spinning up duplicate environments.