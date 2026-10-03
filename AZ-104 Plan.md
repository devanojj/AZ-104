###### General Notes
~~Go back over incorrect / correct in section 6~~
~~Go back over~~ 
- ~~section 1~~
- ~~section 2~~
- ~~section 3~~
- ~~section 4~~
- ~~section 5~~
~~Go back over 1.4.6 / All of 1.5 / All of 1.6~~
~~Go back over prerequisites~~ 

###### Labs
*General Labs* 
==**Virtual Networking**==
~~Create and configure virtual networks~~
~~Implement virtual networking~~
Host your domain on Azure DNS: Exercise - Create alias records for Azure DNS
~~Implement Intersite Connectivity~~
~~Route traffic through the NVA~~

**==Storage Account==**
~~Provide storage for the public website~~
~~Manage Azure Storage~~

==**Computer Resources**==
Create a VM using the Azure portal
~~Implement Web Apps~~

==**Monitor & Backup**==
~~Restore Azure virtual machine data~~

*Following GitHub Lab*
**==[[Lab 9]]==**
==**[[Lab 10]]**==
==**[[Lab 11]]**==


###### Later
John Savill - Study Cram v2 : 21
~~Go back over practice assessment~~ 
Find more exams : Gemini Exam, ChatGPT Exam 
Go back over the YouTube video MS Learn
Go back over notes written down 


**What to cover:**

1. **Storage account basics**
    - Account types: Standard GPv2 vs Premium (block blob, file shares, page blob)
    - Redundancy: LRS, ZRS, GRS, GZRS, RA-GRS, RA-GZRS. Know which survives a zone failure vs a region failure, and which gives read access to the secondary.
    - Changing redundancy after creation, and failover (customer-managed)
2. **Networking and security**
    - Firewalls and virtual networks, service endpoints vs private endpoints
    - Secure transfer, minimum TLS, public network access, "allow trusted Microsoft services"
    - Encryption: Microsoft-managed vs customer-managed keys, infrastructure encryption
3. **Access control (heavily tested)**
    - Access keys vs SAS (account, service, user delegation) vs Entra ID RBAC
    - Stored access policies, and how to revoke a SAS
    - Storage RBAC roles, e.g. Storage Blob Data Contributor vs Contributor
4. **Blob storage**
    - Access tiers: hot, cool, cold, archive, and rehydration from archive
    - Lifecycle management rules
    - Versioning, soft delete, snapshots, immutability policies (legal hold, time-based retention)
    - Object replication, change feed, point-in-time restore
5. **Azure Files**
    - SMB vs NFS, Premium vs Standard, share snapshots, soft delete
    - Mounting on Windows and Linux, AD DS or Entra Domain Services authentication
    - Azure File Sync: server endpoint, cloud endpoint, sync group, cloud tiering
6. **Tools**
    - Storage Explorer, AzCopy, Azure Import/Export, Storage Mover (high level)
7. **Table, queue and Data Lake Gen2** are lower priority. Know what they are and that Data Lake Gen2 is enabled by turning on the hierarchical namespace.

**How it shows up in the exam:**

- Scenario: "Users need read access to a container for 24 hours" → user delegation SAS
- "Must survive a regional outage and be readable during it" → RA-GZRS or RA-GRS
- "Data accessed rarely, must be retrievable within hours" → cool vs archive
- "Prevent deletion for 7 years" → immutability policy
- "Sync on-prem file server to Azure" → Azure File Sync

**Portal practice that pays off:**

- Create an account, then change replication, tier and networking settings
- Generate each SAS type, then revoke one via a stored access policy
- Set a lifecycle rule and an immutability policy
- Lock the account to one VNet with a service endpoint, then a private endpoint, and see how access changes
- Create a file share, mount it from a VM, and take a snapshot

If you can explain _why_ you'd pick one option over another for each of these, storage is covered.

