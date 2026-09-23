Two options migrating traditional Active Directory capabilities:
1. **Site-to-Site VPN to On-Prem AD DS:** High latency/cost; authentication traffic crosses the VPN.
2. **Deploy Replica DC VMs in Azure:** High admin effort; replication traffic crosses the VPN while authentication stays in the cloud.

==**Microsoft Entra Domain Services**== 
Replaces both directly in Azure without having to deploy, manage, or patch virtual machine Domain Controllers.

==**Key Benefits**==
- **No High-Privilege Domain Roles:** Eliminates the need for Domain Admin or Enterprise Admin accounts in the managed domain.
- **Flexible Deployment:** Works with hybrid environments (synced via Entra Connect) or cloud-only environments.
- **Legacy App Compatibility:** Seamlessly host apps like SQL Server or SharePoint on Azure VMs using LDAP, NTLM, or Kerberos.

==**Limitations (AZ-104 Exam Focus)**==
- **Flat OU Structure:** Nested Organisational Units (OUs) are not supported.
- **No Schema Extensions:** You cannot modify or extend the Active Directory schema.
- **Limited Group Policy:** Built-in GPOs exist for computers and users, but you **cannot** target specific OUs, nor use WMI or Security Group filtering.
- **Base Objects Only:** Supports only base computer Active Directory objects.