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