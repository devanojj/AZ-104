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