## Overview & Purpose
When migrating legacy line-of-business (LOB) apps requiring traditional Active Directory capabilities (Kerberos, NTLM, LDAP, Group Policy, Domain Join) to Azure, you traditionally had two options:

1. **Site-to-Site VPN to On-Prem AD DS:** High latency/cost; authentication traffic crosses the VPN.
2. **Deploy Replica DC VMs in Azure:** High admin effort; replication traffic crosses the VPN while authentication stays in the cloud.

**Microsoft Entra Domain Services** replaces both by offering managed domain services directly in Azure without needing to deploy, manage, or patch virtual machine Domain Controllers.

## Key Benefits
- **Zero DC Management:** No patching, updating, monitoring, or AD replication to handle.
- **No High-Privilege Domain Roles:** Eliminates the need for Domain Admin or Enterprise Admin accounts in the managed domain.
- **Flexible Deployment:** Works with hybrid environments (synced via Entra Connect) or cloud-only environments.
- **Legacy App Compatibility:** Seamlessly host apps like SQL Server or SharePoint on Azure VMs using LDAP, NTLM, or Kerberos.

## Critical Limitations (AZ-104 Exam Focus)
- **Flat OU Structure:** Nested Organisational Units (OUs) are not supported.
- **No Schema Extensions:** You cannot modify or extend the Active Directory schema.
- **Limited Group Policy:** Built-in GPOs exist for computers and users, but you **cannot** target specific OUs, nor use WMI or Security Group filtering.
- **Base Objects Only:** Supports only base computer Active Directory objects.