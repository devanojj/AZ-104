contoso.com — the zone apex — must point at an Azure Front Door endpoint, and the record must stop resolving if that endpoint is ever deleted.What do you create?

A A CNAME record set at @
B An A record holding Front Door's current IP address
C An alias record set at @ targeting the Front Door profile
D An NS delegation to Front Door's name servers

**THE ANSWER IS C** CNAME isn't allowed at a zone apex. An alias record set (A, AAAA or CNAME) points at an Azure resource — public IP, Traffic Manager, Front Door, CDN endpoint or another record set — updates itself when the target changes, and empties if the target is deleted, which is what prevents dangling DNS.




Administrators always fall under a two-method policy and can never use security questions, whatever the tenant settings say. Admin SSPR is enabled by default regardless of the Enabled for setting.




Deleting the policy instantly kills every SAS that references it, server-side. Regenerating keys invalidates every SAS and breaks every app using that key, and disabling shared key access breaks all account and service SAS at once. A container, share, queue or table holds at most five policies.




Availability set membership is a creation-time property, and a VM can be neither added to nor removed from a set. Recreate it from its existing disks. Microsoft now recommends a Flexible scale set over availability sets for new designs.






You use Azure CLI to assign the built-in policy Inherit a tag from the resource group, which uses the Modify effect. New resources pick up the tag correctly, but a remediation task for existing resources fails.What is the most likely cause?

AModify never applies to existing resources — DeployIfNotExists is required.BThe assignment has no managed identity with the right role; the portal creates one automatically, the CLI does not.CThe assignment is in Indexed mode and must be changed to All.DTags are never inherited, so remediation of tags is unsupported.

**THE ANSWER IS B**Modify and DeployIfNotExists both act on resources, so the assignment needs a system- or user-assigned managed identity holding the relevant role (Tag Contributor or Contributor here). The portal grants it for you; CLI, PowerShell and the SDKs do not.








After swapping a staging slot into production, the production app starts writing to the staging database.What went wrong?

AThe connection string should have been stored in Key Vault.BThe connection string was not marked as a deployment slot setting, so it swapped along with the code.CCustom domains don't swap, so the binding followed the slot.DAuto swap was enabled on the production slot.

**THE ANSWER IS B**App settings and connection strings move with the code unless you tick Deployment slot setting, which makes them sticky to the slot. Custom domains, TLS bindings, scale settings, IP restrictions, Always On, managed identities and VNet integration never swap.





A storage account must be protected so that nobody — including Owners — can delete it. An application that uses the account must keep working, and it authenticates by calling listKeys.What do you apply?

AA ReadOnly lock on the resource groupBA ReadOnly lock on the storage accountCA CanNotDelete lock on the storage accountDNo lock — create a deny assignment on the account instead

**CORRECT**listKeys is a POST operation, which a ReadOnly lock blocks — that's the classic "app broke after we locked the account" scenario. CanNotDelete stops deletion while still allowing modify and POST. You cannot create deny assignments yourself.






A virtual machine must be restored from backup as a smaller size, on a different subnet.Which restore option do you use?

ACreate new VMBRestore disks, then deploy from the generated ARM templateCReplace existingDFile recovery

**CORRECT**Restore disks gives you managed disks plus a template you can edit before creating the virtual machine, which is the only route to changing size or networking. Create new VM offers no such customisation, Replace existing swaps disks on the original VM, and file recovery mounts the recovery point over iSCSI for 12 hours to pull out individual files.





A Recovery Services vault was created with LRS storage and already protects 30 virtual machines. Compliance now requires Cross Region Restore.What do you do?

AChange the vault's redundancy to GRS, then enable Cross Region Restore.BEnable Cross Region Restore — it works with any redundancy setting.CStop protection and delete the backup data before changing redundancy, or create a new GRS vault and re-protect the virtual machines.DAdd a second vault in the paired region and replicate between the vaults.

**THE ANSWER IS C**Storage redundancy is fixed as soon as any item is protected, which is why you set it before the first backup. Cross Region Restore needs a GRS vault, cannot be turned off once protection starts, and data can take up to 48 hours to appear in the secondary region.




You are writing a custom role that includes DataActions. It must be assignable across three subscriptions that all sit under one management group.How should AssignableScopes be set?

ATo the management group onlyBTo the root scope "/"CTo the three subscription IDsDTo one subscription — the others inherit it

**THE ANSWER IS C**A custom role containing DataActions cannot be assigned at management group scope. AssignableScopes can never be "/", and can name at most one management group, so list the subscriptions explicitly.





Data vs Management Plane Operations





n existing general-purpose v2 account must have a second, hardware-level layer of encryption at rest.What do you do?

AEnable infrastructure encryption from the account's Encryption blade.BConfigure a customer-managed key in Azure Key Vault.CCreate a new account with infrastructure encryption enabled and migrate the data.DCreate an encryption scope on each container.

**THE ANSWER IS C**Infrastructure encryption adds a second 256-bit AES layer, and can only be turned on when the account is created. Customer-managed keys change who controls the key, not how many layers there are. The hierarchical namespace is the other creation-time storage decision worth remembering.





n existing general-purpose v2 account must have a second, hardware-level layer of encryption at rest.What do you do?

AEnable infrastructure encryption from the account's Encryption blade.BConfigure a customer-managed key in Azure Key Vault.CCreate a new account with infrastructure encryption enabled and migrate the data.DCreate an encryption scope on each container.

**THE ANSWER IS C**Infrastructure encryption adds a second 256-bit AES layer, and can only be turned on when the account is created. Customer-managed keys change who controls the key, not how many layers there are. The hierarchical namespace is the other creation-time storage decision worth remembering.




All traffic leaving a subnet must pass through Azure Firewall, so you add a UDR for 10.0.0.0/8 with the firewall as next hop. Traffic to a private endpoint in the same virtual network bypasses it anyway.Why, and what fixes it?

APrivate endpoints ignore route tables entirely; use a service endpoint instead.BThe private endpoint adds a /32 route that wins on longest prefix — enable UDR network policies on the private endpoint's subnet and use a prefix no broader than the virtual network's address space.CThe firewall needs IP forwarding enabled on its network interface.DPrivate endpoint traffic always routes to the internet; add an NSG to block it.

**CORRECT**Network policies are disabled by default on private endpoint subnets, so NSGs and UDRs simply don't apply to that traffic, and the platform's /32 route beats any broader UDR. Enabling private endpoint network policies brings your rules back into play.



Users on Microsoft Entra joined Windows laptops must reach an SMB file share with their own identity. There is no on-premises Active Directory and no line of sight to a domain controller.What do you configure on the storage account?

AOn-premises AD DS authenticationBMicrosoft Entra Domain Services authenticationCMicrosoft Entra KerberosDAccess with the storage account key as Azure\<account-name>

**THE ANSWER IS C**With Entra Kerberos, Entra ID issues the tickets, so no domain controller connectivity is needed — Windows clients only. AD DS and Entra Domain Services both need line of sight to a DC. Only one identity source can be active per storage account.




After tightening NSG rules, every backend behind a Standard public load balancer reports Health Probe Status 0%.What do you check first?

AThat the load balancer SKU matches the public IP SKUBThat no custom deny rule above priority 65001 is blocking the AzureLoadBalancer service tag on the probe portCThat session persistence is set to Client IPDThat the backend pool was added by IP address rather than by NIC

**CORRECT**Probes originate from 168.63.129.16, covered by the AzureLoadBalancer service tag and permitted by the default rule at 65001. A custom deny with a lower priority number overrides it. Then check the app is actually listening on the probe port, the HTTP path returns 200, and the guest firewall allows it.




Encryption at host must be enabled on an existing production virtual machine, but the option is unavailable.Which three things do you check?

AThe OS disk is Premium SSD, the VM is zonal, and it has a public IPBThe EncryptionAtHost feature is registered on the subscription, the VM is deallocated, and the VM size supports itCA Disk Encryption Set exists, the VM is running, and boot diagnostics are enabledDAzure Disk Encryption is already enabled, the VM is deallocated, and its disks are LRS

**CORRECT**Register the feature on the subscription, deallocate the VM, and confirm the size supports it. It also cannot be enabled on a VM that has ever had Azure Disk Encryption — migrating from ADE means new disks and a new VM. ADE itself retires on 15 September 2028.