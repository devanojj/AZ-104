You have an Azure subscription.

From PowerShell, you run the `Get-MgUser` cmdlet for a user and receive the following details:

- `Id: 8755b347-3545-3876-3987-999999999999`
- `DisplayName: Ben Smith`
- `Mail: bsmith@contoso.com`
- `UserPrincipalName: bsmith_contoso.com#EXT#@fabrikam.com`

Based upon the output of the cmdlet, which statement accurately describes the user?

Select only one answer.

The user account is disabled.

The user is a guest in the tenant.

**This answer is correct.**

The user is assigned an administrative role.

The user is deleted.

For guest users, the user principal name (UPN) will contain the email of the guest user (bsmith_contoso.com) followed by #EXT# followed by the domain name of the tenant (@fabrikam.com). Regular Microsoft Entra users appear in a format of user@fabrikam.com.

[B2B collaboration overview - Azure AD - Microsoft Entra | Microsoft Learn](https://learn.microsoft.com/azure/active-directory/external-identities/what-is-b2b)