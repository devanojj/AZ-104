- **Cloud identities** - These users exist only in Microsoft Entra ID. Examples are administrator accounts and users that you manage yourself. Their source is **Microsoft Entra ID** or **External Microsoft Entra directory** if the user is defined in another Microsoft Entra instance but needs access to subscription resources controlled by this directory. When these accounts are removed from the primary directory, they're deleted.

- **Directory-synchronised identities** - These users exist in an on-premises Active Directory. A synchronisation activity brings these users into Microsoft Entra ID. #Microsoft_Entra_Connect_Sync is the recommended synchronisation tool for most organisations—it uses a lightweight cloud-managed agent and supports multiple disconnected forests. 
- #Microsoft_Entra_Connect_Sync  remains available for complex scenarios such as device synchronisation or groups with more than 50,000 members. Their source is **Windows Server AD**.

- **Guest users** - These users exist outside your organisation. Examples are accounts from other cloud providers and Microsoft accounts. Their source is **Invited user**. This type of account is useful when external vendors or contractors need access to your organization's resources. Once their help is no longer necessary, you can remove the account and all of their access.







