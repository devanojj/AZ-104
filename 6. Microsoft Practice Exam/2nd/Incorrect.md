Your Microsoft Entra tenant and on-premises Active Directory domain contain multiple users.

You need to configure self-service password reset (SSPR) functionality. The solution must minimize costs.

Which Microsoft Entra ID edition should you use?

Select only one answer.

Microsoft Entra ID Free

Microsoft Entra ID P1

**This answer is correct.**

Microsoft Entra ID P2

**This answer is incorrect.**

Only Microsoft Entra ID P1 and P2 support SSPR, but Microsoft Entra ID P1 is the lower cost option.

[Enable Azure Active Directory self-service password reset - Microsoft Entra | Microsoft Learn](https://learn.microsoft.com/azure/active-directory/authentication/tutorial-enable-sspr)

[What is self-service password reset in Azure Active Directory? - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/allow-users-reset-their-password/2-self-service-password-reset)

You have a Microsoft Entra tenant named contoso.com that contains a group named Group1. Group1 contains the following users:

- User1 — Type: Member; Sync from on-premises: Yes
- User2 — Type: Member; Sync from on-premises: No
- User3 — Type: Guest; Sync from on-premises: No

Password writeback is enabled in Microsoft Entra Connect Sync.

You enable self-service password reset (SSPR) for Group1.

You need to identify which users can use SSPR.

Which users should you identify?

Select only one answer.

User2 only

User1 and User2 only

**This answer is correct.**

User2 and User3 only

User1, User2, and User3

**This answer is incorrect.**

SSPR registration is required for users in scope who are eligible to use SSPR. In this scenario, Group1 is in scope and includes two member users (User1 and User2) and one guest user (User3). Because password writeback is enabled, the synced member (User1) can use SSPR to write changes back to on-premises AD, and the cloud-only member (User2) can reset in Entra ID—both must register. Guest users (User3) are not supported for SSPR in the resource tenant (they manage passwords in their home tenant), so they do not need to register here.

[Introduction](https://learn.microsoft.com/en-us/training/modules/allow-users-reset-their-password/1-introduction)  
[Manage self-service password reset in Microsoft Entra ID](https://learn.microsoft.com/en-us/training/modules/protect-identities-azure-acative-directory/6-manage-self-service-password-reset)  
[Exercise configure and deploy self-service password reset](https://learn.microsoft.com/en-us/training/modules/manage-user-authentication/5-exercise-configure-deploy-self-service-password-reset)  
[Tutorial: Enable Microsoft Entra self-service password reset writeback to an on-premises environment](https://learn.microsoft.com/en-us/entra/identity/authentication/tutorial-enable-sspr-writeback)  
[What is Microsoft Entra authentication?](https://learn.microsoft.com/en-us/training/modules/manage-security-controls-identity-access/16-what-microsoft-entra-authentication)


You have an Azure subscription.

You plan to create an Azure Policy definition named Policy1.

You need to include remediation information in Policy.

To which definition section should you add remediation information for Policy1?

Select only one answer.

metadata

**This answer is correct.**

mode

parameters

policyRule

**This answer is incorrect.**

You must use the RemediationDescription field in the metadata section from properties to specify a custom recommendation. The remaining options are Azure policies, but do not allow specific custom remediation information.

[Create custom Azure security policies in Microsoft Defender for Cloud | Microsoft Learn](https://learn.microsoft.com/azure/defender-for-cloud/custom-security-policies?pivots=azure-portal#enhance-your-custom-recommendations-with-detailed-information)

[Improve incident response with alerting on Azure - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/incident-response-with-alerting-on-azure/)



ou have an Azure virtual network named VNet1.

You create an Azure Private DNS zone named contoso.com.

You need to ensure that the virtual machines on VNet1 register in the contoso.com private DNS zone.

What should you do?

Select only one answer.

Add a virtual network link to contoso.com.

**This answer is correct.**

Add Azure DNS Private Resolver to VNet1.

**This answer is incorrect.**

Configure each virtual machine to use a custom DNS server.

Configure VNet1 to use a custom DNS server.

To associate a virtual network to a private DNS zone, you add the virtual network to the zone by creating a virtual network link.

Azure DNS Private Resolver is used to proxy DNS queries between on-premises environments and Azure DNS.

A custom DNS server will work if you deploy a DNS server as a virtual machine or an appliance, however, this configuration does not work with a private DNS zone.

[Quickstart - Create an Azure private DNS zone using the Azure portal | Microsoft Learn](https://learn.microsoft.com/azure/dns/private-dns-getstarted-portal)

[Configure Azure DNS - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-azure-dns/)

5. Monitor and maintain Azure resources


You are creating an Azure virtual machine that will run Windows Server.

You need to ensure that VM1 will be part of a virtual machine scale set.

Which setting should you configure during the creation of the virtual machine?

Select only one answer.

Availability options

**This answer is correct.**

Azure Spot instance

**This answer is incorrect.**

Management

Region

You must configure the virtual machine scale set from the availability options. Azure spot instance is used to add virtual machines with a discounted price. Region will not affect the configuration of the availability options. The management setting allows you to configure the monitoring and management options for the virtual machine.

[Availability options for Azure Virtual Machines - Azure Virtual Machines | Microsoft Learn](https://learn.microsoft.com/azure/virtual-machines/availability)

[Configure virtual machine availability - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-virtual-machine-availability/)

ou need to create an Azure App Service web app that runs on Windows. The web app requires scaling to five instances, 45 GB of storage, and a custom domain name. The solution must minimize costs.

Which App Service plan should you use?

Select only one answer.

Basic

**This answer is incorrect.**

Free

Premium

Standard

**This answer is correct.**

The Standard service plan can host unlimited web apps, up to 50 GB of disk space, and up to 10 instances. The plan will cost approximately $0.10/hour. The Free plan only offers 1 GB of disk size and 0 instances to host the app. The Premium plan offers 250 GB of disk space and up to 30 instances and will cost approximately $0.20/hour. The Basic plan offers 10 GB of disk space and up to three virtual machines.

You have an Azure subscription that contains virtual machines, virtual networks, application gateways, and load balancers.

You need to monitor the network health of the resources.

Which Azure service should you use?

Select only one answer.

Azure Monitor

**This answer is incorrect.**

Azure Network Watcher

**This answer is correct.**

Azure Resource Manager

network security groups (NSGs)


You have an Azure virtual machine named VM1 that is protected by using Azure site recovery.

You fail over VM1 from the primary region to the secondary region.

You need to reprotect VM1 after the failover so that VM1 will replicate back to the primary region.

What is the VM1 status before the reprotection?

Select only one answer.

Committing failover

Failover committed

**This answer is correct.**

Failover confirmed

**This answer is incorrect.**

Starting failover

Before you begin, you must ensure that the virtual machine status is Failover committed. This will ensure replication back to the primary region.

[Tutorial to fail over Azure VMs to a secondary region for disaster recovery with Azure Site Recovery. - Azure Site Recovery | Microsoft Learn](https://learn.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback)

[Configure file and folder backups - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-file-folder-backups/)

ou have an Azure subscription that contains a storage account named storage1. storage1 contains an Azure Files share named share1.

You need to ensure that users can authenticate to share1 by using Microsoft Entra and access the file share by using SMB.

What should you do?

Select only one answer.

Configure identity-based access.

**This answer is correct.**

Generate a shared access signature (SAS) and a connection string.

**This answer is incorrect.**

Enable public network access.

Regenerate the access keys.

**Objective:**

2.1 Configure access to storage

**What This Item Tests:**

Configure identity-based access for Azure Files

**Additional Reading:**

[Review Azure Storage security strategies - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/configure-storage-security/2-review-strategies)

Correct - Identity-based access for an Azure Storage account is a security model that uses Microsoft Entra ID or Active Directory to authorize requests to storage data, rather than relying on a static storage account key or SAS.  
Incorrect – SAS tokens and access keys provide key-based access, rather than identity-based access, and enabling public network access does not configure authentication or authorization.

You have an Azure subscription.

You plan to create a storage account named storage1.

You need to ensure that storage1 provides POSIX-compliant access control lists (ACLs).

Which option should you configure when creating storage1?

Select only one answer.

access tier

**This answer is incorrect.**

hierarchical namespace

**This answer is correct.**

SFTP

version-level immutable support

To enable POSIX-compliant access control lists (ACLs), the hierarchical namespace must be used. The remaining options are valid for a storage account, but do not provide the POSIX-compliant feature.

[Azure Data Lake Storage Gen2 Hierarchical Namespace | Microsoft Learn](https://learn.microsoft.com/azure/storage/blobs/data-lake-storage-namespace)

[Configure storage accounts - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-storage-accounts/)

You create an Azure Storage account.

You need to create a lifecycle management rule to move blobs to Cool storage if the blobs have not been accessed for 30 days.

What should you do first?

Select only one answer.

Enable access tracking.

**This answer is correct.**

Enable versioning for blobs.

**This answer is incorrect.**

Refresh the blob inventory.

Rotate the storage account keys.

A lifecycle management rule can be used to move or delete blobs automatically. The rule can be based on the time the blob was last modified or the time the blob was last accessed (read or write). To perform an action based on the access time, access tracking must be enabled. This can incur additional storage costs.

[Configure a lifecycle management policy - Azure Storage | Microsoft Learn](https://learn.microsoft.com/azure/storage/blobs/lifecycle-management-policy-configure?tabs=azure-portal)

[Configure Azure Blob Storage - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-blob-storage/)

ou have an Azure subscription that contains a storage account.

You need to recommend a storage solution for storing infrequently accessed data. The solution must meet the following requirements:

- The data must be stored for at least 90 days.
- The data must be available within seconds.
- Storage costs must be minimized.

Which tier should you recommend?

Select only one answer.

Cold

**This answer is correct.**

Cool

**This answer is incorrect.**

Hot

Premium

The correct solution is the Cold tier, because it is an online storage tier in Azure designed for infrequently accessed data that must remain available within seconds. The Cold tier has a recommended minimum retention period of 90 days, aligning directly with the scenario, and offers lower storage costs than Hot or Cool tiers while still supporting immediate access. The Cool tier requires only 30 days of retention and has higher costs than Cold for long-term storage, the Hot tier is optimized for frequently accessed data at higher storage prices, and the Premium tier is intended for high-performance workloads, not for cost efficiency. Therefore, Cold best satisfies the requirements for cost savings, online availability, and the 90-day storage requirement.

[Explore Azure Blob storage](https://learn.microsoft.com/en-us/training/modules/explore-azure-blob-storage/2-blob-storage-overview)  
[Assign blob access tiers](https://learn.microsoft.com/en-us/training/modules/configure-blob-storage/4-create-blob-access-tiers)