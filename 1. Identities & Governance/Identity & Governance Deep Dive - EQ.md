Practice questions for [[Identity & Governance Deep Dive]]. Sections mirror that note so you can jump back to whatever you miss.

---
## 1. Microsoft Entra ID Fundamentals

**How many Microsoft Entra tenants can a single Azure subscription trust at one time?**
- Two, one for users and one for guests
- Unlimited
- *One (correct)*

**Which protocols does Microsoft Entra ID use for authentication, unlike on-premises AD DS?**
- Kerberos and NTLM
- *OAuth 2.0, SAML and OpenID Connect over HTTPS (correct)*
- LDAP and Kerberos

**A legacy application in Azure needs LDAP and Kerberos authentication, but you don't want to deploy and manage domain controllers. What should you use?**
- Microsoft Entra Connect Sync
- Microsoft Entra ID Protection
- *Microsoft Entra Domain Services (correct)*

**Which feature is available in Microsoft Entra ID P1 but not in the Free edition?**
- Single sign-on to SaaS apps
- MFA through security defaults
- *Conditional Access (correct)*

**Your organisation needs risk-based sign-in policies that respond to leaked credentials and unusual sign-in activity. Which licence is required?**
- Microsoft Entra ID Free
- Microsoft Entra ID P1
- *Microsoft Entra ID P2 (correct)*

**Administrators must activate privileged roles just-in-time with approval and time limits. Which feature and licence do you need?**
- *Privileged Identity Management with P2 (correct)*
- Conditional Access with P1
- Administrative units with P1

**On-premises AD users must be synchronised to Microsoft Entra ID using a lightweight agent that supports disconnected forests. What should you use?**
- Microsoft Entra Domain Services
- *Microsoft Entra Cloud Sync (correct)*
- Microsoft Identity Manager

---
## 2. Users

**Before you can assign a Microsoft 365 licence to a new user, which property must be set?**
- Job title
- Manager
- *Usage location (correct)*

**You need to create 300 user accounts from an HR spreadsheet. What is the most efficient method in the Microsoft Entra admin center?**
- Create each user from the New user page
- Invite each user as an external user
- *Bulk create using the CSV template (correct)*

**What is the least-privileged Microsoft Entra role that can create, edit and delete users?**
- *User Administrator (correct)*
- Global Administrator
- Helpdesk Administrator

**A user account was deleted 12 days ago and must be recovered with its properties. What should you do?**
- *Restore the user from Deleted users (correct)*
- Recreate the account with the same UPN
- Contact Microsoft Support to recover it

**For how long can a deleted user account be restored?**
- 7 days
- 90 days
- *30 days (correct)*

**A security group was deleted by mistake yesterday. How can you recover it?**
- *You can't; security groups are permanently deleted and must be recreated (correct)*
- Restore it from Deleted groups within 30 days
- Restore it from the Microsoft Entra recycle bin with PowerShell

**You change a synchronised user's department in the Microsoft Entra admin center, but it reverts. What should you do?**
- *Change the attribute in on-premises Active Directory, which is the source of authority (correct)*
- Block the user from syncing
- Change the attribute with Microsoft Graph instead

**Helpdesk staff in London must be able to reset passwords only for users in the London office. What should you configure?**
- A dynamic security group for London users
- A custom Azure RBAC role
- *An administrative unit containing the London users, with Helpdesk Administrator scoped to it (correct)*

**Which licence is required for an administrator who is assigned a role scoped to an administrative unit?**
- Microsoft Entra ID Free
- *Microsoft Entra ID P1 (correct)*
- Microsoft Entra ID P2

**A Global Administrator can't see or assign custom security attributes. Why?**
- *Global Administrators don't have access by default; Attribute Definition/Assignment Administrator roles are needed (correct)*
- Custom security attributes require P2
- They must first be enabled in Azure Policy

---
## 3. Groups

**A team needs a shared mailbox, calendar, SharePoint site and Teams workspace. Which group type should you create?**
- *Microsoft 365 group (correct)*
- Security group
- Distribution list

**You need a group of all Windows devices so you can target them with policies. Which group should you create?**
- A dynamic Microsoft 365 group
- An assigned Microsoft 365 group
- *A dynamic device security group (correct)*

**Which objects can be members of a Microsoft 365 group?**
- Users, devices and security groups
- *Users only (correct)*
- Users and service principals

**All users whose department is Sales must be added to a group automatically, and removed when they leave the department. What should you configure?**
- An assigned security group maintained by a scheduled script
- An administrative unit
- *A dynamic user group with the rule user.department -eq "Sales" (correct)*

**What licence is required for dynamic membership groups?**
- Microsoft Entra ID Free for each group owner
- Microsoft Entra ID P2 for each administrator
- *Microsoft Entra ID P1 for each unique user who's a member (correct)*

**You try to manually add a user to a dynamic membership group. What happens?**
- The user is added and kept until the rule next runs
- The user is added permanently
- *You can't; membership of a dynamic group is controlled only by the rule (correct)*

**You need a single dynamic group containing both users and devices from the Finance department. What should you tell the team?**
- *A dynamic rule can't contain both users and devices; create separate groups (correct)*
- Use a Microsoft 365 group with a combined rule
- Use the advanced rule builder

**A device rule must include devices owned by users in the Sales department. Why does the rule fail?**
- Device rules require P2
- Device rules must use the rule builder
- *Device membership rules can reference only device attributes, not the owner's attributes (correct)*

**A licence is assigned to Group A. Group B is a member of Group A. Do users in Group B receive the licence?**
- Yes, licences flow to all nested members
- Only if Group B is also a dynamic group
- *No, group-based licensing applies only to direct members (correct)*

**You need a group that can be assigned Microsoft Entra roles. What must you configure?**
- *A security group created with "Microsoft Entra roles can be assigned to the group" enabled, with assigned membership (correct)*
- A dynamic security group
- A Microsoft 365 group with owners

---
## 4. Licences

**What is the most efficient way to license a large, changing team?**
- Assign licences to each user directly
- *Assign the licence to a security group (group-based licensing) (correct)*
- Ask users to request licences from the admin portal

**Group-based licence assignment fails with CountViolation. What should you do?**
- *Buy more licences or free unused ones, then reprocess (correct)*
- Remove conflicting service plans
- Change the users' usage location

**A user has both an E1 and an E3 licence that include the same service plan, and assignment fails. Which error is this?**
- CountViolation
- DependencyViolation
- *MutuallyExclusiveViolation (correct)*

**An add-on licence can't be assigned because it requires another licence the user doesn't have. Which error is this?**
- *DependencyViolation (correct)*
- CountViolation
- MutuallyExclusiveViolation

**A licence can't be assigned because it isn't available in the user's country. What should you change?**
- The user's UPN suffix
- *The user's usage location (correct)*
- The user's job title

**Users show LicenseAssignmentAttributeConcurrencyException. What should you do?**
- Remove the users from the group and re-add them
- Reprocess every user manually
- *Nothing; Microsoft Entra ID retries automatically (correct)*

**After fixing the cause of a licence error, how do you apply the licence without waiting?**
- *Select Reprocess on the user or group (correct)*
- Remove and re-add the licence to the group
- Delete and recreate the group

**A user gets the same licence directly and through a group. How many licences are consumed?**
- *One (correct)*
- Two
- None until the direct assignment is removed

**You plan to delete a group that has licences assigned. What should you do first?**
- *Remove the licences from the group (correct)*
- Convert it to a dynamic group
- Nothing; licences are removed automatically and immediately

**One of three licences assigned to a group fails for a particular user. What happens to the other two licences for that user?**
- *None of the group's licences are assigned to that user (correct)*
- They're assigned normally
- They're assigned directly instead

---
## 5. External Users (B2B)

**A contractor from a partner company needs temporary access to an internal app using their own company credentials. What should you do?**
- *Invite the contractor as a B2B guest user (correct)*
- Create a new member account in your tenant
- Synchronise the contractor's account with Entra Connect

**By default, who can invite guest users into your tenant?**
- *Anyone in the organisation, including guests and non-admins (correct)*
- Only Global Administrators
- Only users with the Guest Inviter role

**Only administrators and users with the Guest Inviter role should be able to invite guests. Where should you configure this?**
- Cross-tenant access settings
- *External collaboration settings – guest invite settings (correct)*
- Conditional Access

**You must prevent users from inviting guests from a competitor's domain. What should you configure?**
- An Azure Policy deny assignment
- A Conditional Access named location
- *Collaboration restrictions with a deny list of domains (correct)*

**Guests must only be able to see their own profile and not browse the directory. Which guest user access setting should you choose?**
- *Restricted to properties and memberships of their own directory objects (correct)*
- Same access as members
- Limited access to properties and memberships of directory objects

**What is the default guest user access level?**
- *Limited access to properties and memberships of directory objects (correct)*
- Same access as members
- Restricted to their own objects

**You want to trust MFA performed in a partner's Microsoft Entra tenant so their users aren't prompted again. What should you configure?**
- External collaboration settings
- *Cross-tenant access settings (inbound trust) (correct)*
- An identity provider for Google federation

**External collaboration settings allow all domains, but cross-tenant access settings block a specific organisation. What happens when you invite a user from that organisation?**
- The invitation succeeds because external collaboration settings allow it
- *The most restrictive setting applies, so the collaboration is blocked (correct)*
- The invitation is sent but the guest can't sign in to apps

**You need to invite 200 external auditors at once. What should you use?**
- *Bulk invite with the CSV template (correct)*
- Bulk create users
- Entra Connect Sync

---
## 6. Self-Service Password Reset

**Cloud-only users with only a Microsoft Entra ID Free licence forget their passwords. Can they use SSPR to reset them?**
- Yes, SSPR reset is included in Free
- Yes, but only from the Windows sign-in screen
- *No; Free allows only a password change; reset needs Microsoft 365 Business Standard or Entra ID P1/P2 (correct)*

**Hybrid users reset their passwords in the cloud, and the new password must apply to on-premises AD. What's required?**
- Pass-through authentication only
- *Password writeback via Entra Connect or Cloud Sync, with P1/P2 or Microsoft 365 Business Premium (correct)*
- Microsoft Entra Domain Services

**You want to pilot SSPR with only the IT department. Which setting should you choose for "Self service password reset enabled"?**
- *Selected, targeting the IT group (correct)*
- All
- None

**How many authentication methods can you require for a user to reset a password?**
- 1 to 3
- *1 or 2 (correct)*
- Exactly 2

**Which statement about SSPR for administrator accounts is correct?**
- Administrators follow the same policy as users
- Administrators can use security questions
- *Administrators always require two methods and can't use security questions (correct)*

**Which SSPR authentication method is least recommended?**
- Microsoft Authenticator notification
- *Security questions (correct)*
- Email to an alternate address

**Users must register their SSPR methods the first time they sign in. Which setting should you enable?**
- Notify users on password resets
- *Require users to register when signing in (correct)*
- Allow users to unlock accounts

**How does password writeback communicate with on-premises AD?**
- Through an inbound firewall rule on port 389
- *Through an outbound connection over port 443, with no inbound firewall rules needed (correct)*
- Through a site-to-site VPN

---
## 7. Azure RBAC

**What three elements make up an Azure role assignment?**
- *Security principal, role definition and scope (correct)*
- User, group and subscription
- Role, policy and lock

**What is the correct order of Azure RBAC scopes, from broadest to narrowest?**
- Subscription, management group, resource group, resource
- Management group, resource group, subscription, resource
- *Management group, subscription, resource group, resource (correct)*

**A user must create and manage all resources in a subscription but must not grant access to others. Which role should you assign?**
- *Contributor (correct)*
- Owner
- User Access Administrator

**What is the difference between Owner and Contributor?**
- Contributor can delete resources; Owner can't
- Owner can only read resources
- *Owner can also assign roles to others; Contributor can't (correct)*

**An administrator must be able to assign roles but should not be able to manage resources. Which roles meet the requirement?**
- Contributor or Reader
- *User Access Administrator or Role Based Access Control Administrator (correct)*
- Owner or Contributor

**A team lead must be allowed to assign only the Backup Reader and Backup Contributor roles, and only to the Sales group. What should you assign?**
- Owner at the subscription
- *Role Based Access Control Administrator with a condition restricting roles and principals (correct)*
- User Access Administrator at the resource group

**A user in another department needs to manage a single VM. Following least privilege, where should you assign the role?**
- At the subscription scope
- At the resource group scope
- *At the virtual machine (resource) scope (correct)*

**A developer needs full access to everything in one resource group. Which assignment follows least privilege?**
- Owner at the subscription
- Contributor at the subscription
- *Contributor at the resource group (correct)*

**The same group needs Reader on 40 subscriptions. What is the most efficient approach?**
- Assign Reader on each subscription
- Create a custom role for each subscription
- *Place the subscriptions in a management group and assign Reader there (correct)*

**A user has Reader on the subscription and Contributor on RG-Web. What are their effective permissions on resources in RG-Web?**
- Reader, because the subscription assignment takes precedence
- No access, because the assignments conflict
- *Contributor, because role assignments are additive (correct)*

**A custom role has Actions of `*` and NotActions of `Microsoft.Compute/virtualMachines/delete`. The same user also has Virtual Machine Contributor. Can they delete VMs?**
- No, NotActions is an explicit deny
- No, custom roles take precedence over built-in roles
- *Yes, NotActions only excludes from that role; another role's Actions still grants it (correct)*

**Which role definition property lists permissions for working with data inside a resource, such as reading blob contents?**
- Actions
- AssignableScopes
- *DataActions (correct)*

**None of the built-in roles let operators only start, restart and deallocate VMs. What should you do?**
- *Create a custom role containing only those actions (correct)*
- Assign Virtual Machine Contributor with a resource lock
- Use Azure Policy to deny other actions

**Which value is NOT allowed in a custom role's AssignableScopes?**
- A subscription
- *The root scope "/" (correct)*
- A resource group

**A custom role includes DataActions. At which scope can it NOT be assigned?**
- Subscription
- *Management group (correct)*
- Resource group

**How many management groups can you list in the AssignableScopes of one custom role?**
- Unlimited
- Up to 10
- *One (correct)*

**What is the maximum number of Azure role assignments per subscription?**
- *5,000 (correct)*
- 2,000
- 4,000

**You need to block a specific action for everyone, including Owners. Can you create a deny assignment directly?**
- Yes, from Access control (IAM) > Deny assignments > Add
- *No, deny assignments are created only by Azure (for example by managed applications or deployment stacks) (correct)*
- Yes, with New-AzDenyAssignment

**A Global Administrator signs in to the Azure portal and can't see any subscriptions. What should they do?**
- *Elevate access, which grants User Access Administrator at the root scope "/" (correct)*
- Assign themselves Owner from Microsoft Entra roles
- Ask Microsoft Support to grant access

**Which statement about Microsoft Entra roles and Azure roles is correct?**
- *They are separate; Entra roles manage the directory and Azure roles manage Azure resources (correct)*
- Entra roles automatically grant equivalent Azure permissions
- Azure Owner can manage users in Entra ID

**A team member can't view resources in a resource group. Where should you check their access?**
- *The resource group > Access control (IAM) > Check access (correct)*
- Their profile > My permissions
- Microsoft Entra ID > Roles and administrators

**You need a report of role assignments created during the last week. Where should you look?**
- *The Activity log filtered on the Create role assignment operation (correct)*
- Access control (IAM) > Role assignments
- Access control (IAM) > Download role assignments

**You remove a user's role assignment on a VM, but they can still manage it. What is the most likely cause?**
- Role assignment removals take 24 hours
- *The access is inherited from a higher scope and must be removed where it was assigned (correct)*
- The VM has a resource lock

---
## 8. Azure Policy

**What does an Azure Policy initiative contain?**
- A set of role assignments
- A single policy rule with multiple effects
- *A group of policy definitions assigned together (correct)*

**At which scopes can you assign an Azure Policy?**
- Management group only
- *Management group, subscription and resource group (correct)*
- Subscription and resource group only

**Resources may only be created in UK South and UK West. Which built-in policy should you assign?**
- *Allowed locations (correct)*
- Allowed resource types
- Require a tag and its value

**Which policy effect blocks a non-compliant resource from being created?**
- Audit
- *Deny (correct)*
- Append

**You assign a Deny policy for allowed locations. What happens to existing resources in other regions?**
- They're deleted
- *They remain and are reported as non-compliant (correct)*
- They're moved to an allowed region

**You need to log non-compliant resources without blocking deployments. Which effect should you use?**
- Deny
- *Audit (correct)*
- DenyAction

**Every resource must get its resource group's CostCentre tag, including resources that already exist. What should you use?**
- The Require a tag and its value policy with Deny
- *The Inherit a tag from the resource group policy (Modify) plus a remediation task (correct)*
- Tags on the resource group, which are inherited automatically

**Diagnostic settings must be deployed automatically on any resource that doesn't have them. Which effect should you use?**
- AuditIfNotExists
- *DeployIfNotExists (correct)*
- Append

**What does a policy assignment that uses Modify or DeployIfNotExists require?**
- A resource lock on the scope
- *A managed identity with permissions to make the changes (correct)*
- An initiative containing at least two definitions

**A DeployIfNotExists policy fixes new resources, but existing non-compliant resources are unchanged. What should you do?**
- Reassign the policy with Deny
- *Create a remediation task (correct)*
- Wait for the 24-hour compliance scan

**You want to see the compliance impact of a Deny policy on an existing environment without blocking anything. What should you set?**
- *The assignment's enforcement mode to Disabled (correct)*
- The effect to Disabled
- An exemption for every resource group

**One storage account must be temporarily excused from an initiative while a fix is planned. What should you create?**
- An exclusion when editing the assignment
- *A policy exemption with the Waiver category and an expiry date (correct)*
- A resource lock

**What does the Mitigated exemption category mean?**
- The resource's non-compliance is temporarily accepted
- The resource was deleted
- *The policy's intent is met through another method (correct)*

**You assigned a new policy 10 minutes ago and the compliance dashboard is empty. What should you do?**
- Reassign the policy at a higher scope
- *Wait for evaluation to complete, or start an on-demand scan with az policy state trigger-scan (correct)*
- Delete and recreate the assignment

**How often does the standard Azure Policy compliance evaluation run?**
- Every hour
- Every 7 days
- *Every 24 hours (correct)*

**A custom policy checks tags and location. Which mode should the definition use?**
- *Indexed (correct)*
- All
- Microsoft.KeyVault.Data

**Which statement best distinguishes Azure Policy from Azure RBAC?**
- *RBAC controls what users can do; Policy controls what state resources must be in (correct)*
- Policy controls who can deploy resources; RBAC controls their properties
- Both control user permissions

**At request time, which effect is evaluated before Deny?**
- Audit
- DeployIfNotExists
- *Modify (correct)*

**Which built-in role allows a user to create and manage policy assignments without full Owner rights?**
- *Resource Policy Contributor (correct)*
- Policy Insights Data Writer
- Security Reader

---
## 9. Resource Locks

**Nobody, including subscription Owners, should be able to delete a production resource group, but changes must still be allowed. What should you apply?**
- A ReadOnly lock
- *A CanNotDelete (Delete) lock (correct)*
- A Deny policy for delete operations

**A firewall's configuration must not be changed or deleted by anyone. What should you apply?**
- A CanNotDelete lock
- Reader role for all users
- *A ReadOnly lock (correct)*

**A resource group has a CanNotDelete lock. What can an Owner do to a VM inside it?**
- Nothing; the VM is read-only
- Delete the VM but not change it
- *Modify the VM but not delete it (correct)*

**Which roles can create or remove management locks?**
- *Owner and User Access Administrator (correct)*
- Contributor and Owner
- Reader and Contributor

**An Owner needs to delete a resource that has a CanNotDelete lock. What must happen?**
- A support ticket must be raised
- The Owner must elevate access
- *The lock must be removed first, then the resource deleted (correct)*

**A lock is applied to a resource group. Which resources does it protect?**
- Only resources that existed when the lock was applied
- *All current and future resources in the resource group (correct)*
- Only the resource group object

**A ReadOnly lock is applied to a storage account. An application that lists the account keys starts failing. Why?**
- *Listing keys is a POST operation, which a ReadOnly lock blocks (correct)*
- ReadOnly locks rotate the keys
- ReadOnly locks block all data-plane access

**A CanNotDelete lock is on a storage account. Can users still delete blobs in it?**
- No, the lock protects all data in the account
- *Yes, locks apply to the control plane and don't protect data-plane operations (correct)*
- Only if they're Owners

**A resource group containing VMs has a ReadOnly lock. What happens when an operator tries to start a stopped VM?**
- It starts normally because start isn't a modify operation
- *It fails, because starting a VM is a POST operation that the lock blocks (correct)*
- It starts, but can't be stopped again

**One resource in a resource group has a CanNotDelete lock. What happens when you try to delete the resource group?**
- *The whole deletion fails (correct)*
- All unlocked resources are deleted and the locked one remains
- The resource group is deleted along with the locked resource

---
## 10. Tags

**You add a CostCentre tag to a resource group. Do the resources in it get the tag?**
- *No, tags aren't inherited; use Azure Policy to apply them (correct)*
- Yes, tags are inherited automatically
- Only resources created after the tag was added

**What is the maximum number of tag name-value pairs on a single resource?**
- 15
- *50 (correct)*
- 100

**Which of these can't be tagged?**
- *A management group (correct)*
- A subscription
- A resource group

**A finance user must add and update tags on resources without being able to change the resources. Which role should you assign?**
- Contributor
- Reader
- *Tag Contributor (correct)*

**Which statement about tag case-sensitivity is correct?**
- *Tag names are case-insensitive and tag values are case-sensitive (correct)*
- Tag names and values are both case-sensitive
- Tag names and values are both case-insensitive

**You run Update-AzTag with -Operation Replace on a resource that has five tags, supplying one new tag. What is the result?**
- Six tags
- *Only the one new tag (correct)*
- The five original tags, unchanged

**You need cost reports grouped by department. What should you use?**
- Resource locks
- Management groups per department
- *Tags with a department value, filtered in Cost analysis (correct)*

---
## 11. Resource Groups

**How many resource groups can a resource belong to at the same time?**
- *One (correct)*
- Two
- As many as needed

**Which statement about resource groups is correct?**
- Resource groups can be nested inside other resource groups
- All resources in a group must be in the group's region
- *A resource group's location stores its metadata, but it can contain resources from any region (correct)*

**A resource group must be renamed to match a new naming convention. What should you do?**
- Rename it from its Overview page
- *Create a new resource group and move the resources into it (correct)*
- Rename it with az group update

**What happens when you delete a resource group?**
- Only empty resource groups can be deleted
- *All resources in it are deleted (correct)*
- Resources are moved to the default resource group

**A deployment fails because the subscription isn't registered for Microsoft.ContainerInstance. What should you do?**
- Request a quota increase
- Change the subscription's directory
- *Register the resource provider in the subscription (correct)*

---
## 12. Subscriptions

**You change the Microsoft Entra directory associated with a subscription. What happens to existing Azure role assignments?**
- They're copied to the new tenant
- They remain but are disabled
- *They're permanently deleted (correct)*

**Which of the following is also lost when a subscription is associated with a different tenant?**
- *Policy assignments and custom roles (correct)*
- The resources in the subscription
- The subscription's billing owner

**After associating a subscription with a new tenant, VMs can no longer access Key Vault with their managed identities. What should you do?**
- Restore the old tenant association
- *Re-enable or recreate the managed identities, update the Key Vault tenant ID and re-grant permissions (correct)*
- Reassign the same object IDs in the new tenant

**What is required to change a subscription's directory?**
- Global Administrator in the target tenant only
- Contributor on the subscription
- *Owner on the subscription and an account that exists in both tenants (correct)*

**A deployment fails because the region's vCPU limit for the subscription has been reached. What should you do?**
- *Request a quota increase for the subscription (correct)*
- Move the subscription to a management group
- Create a new resource group

**A subscription with CanNotDelete locks on its resources is cancelled. What happens?**
- Cancellation is blocked until the locks are removed
- *The cancellation proceeds; locks don't block it (correct)*
- Locked resources are moved to another subscription

---
## 13. Cost Management

**Finance wants an email when spending on a subscription reaches 80% of a monthly budget. What should you create?**
- An Azure Advisor alert
- A metric alert on the subscription
- *A Cost Management budget with an 80% actual-cost alert (correct)*

**You want to be warned before spending is likely to exceed the budget. Which alert type should you configure?**
- An actual-cost alert at 100%
- *A forecasted-cost alert (correct)*
- An anomaly alert

**What happens to resources when a budget's 100% threshold is reached?**
- They're stopped automatically
- They're deallocated but not deleted
- *Nothing; budgets only notify unless an action group automates a response (correct)*

**VMs must be shut down automatically when a resource group's budget is exceeded. What should you configure?**
- *A budget alert that triggers an action group running an automation runbook or Logic App (correct)*
- An Azure Policy Deny effect
- A ReadOnly lock triggered by the budget

**At which scopes can a budget trigger an action group?**
- Management group and billing account
- *Subscription and resource group (correct)*
- Any scope

**How many alert thresholds can a single budget have?**
- *5 (correct)*
- 1
- 10

**You need to be notified of an unexpected spike in daily spend, even if the budget hasn't been reached. What should you configure?**
- A forecasted budget alert
- *A cost anomaly alert (correct)*
- A credit alert

**Which five categories does Azure Advisor use?**
- *Reliability, Security, Performance, Cost and Operational excellence (correct)*
- Cost, Security, Compliance, Identity and Networking
- Availability, Backup, Cost, Monitoring and Performance

**You need to find underutilised VMs that could be resized or shut down. What should you use?**
- Azure Policy compliance
- *Azure Advisor cost recommendations (correct)*
- Cost analysis

**You need to estimate the monthly cost of a new deployment before building it. What should you use?**
- The TCO calculator
- Azure Advisor
- *The Azure pricing calculator (correct)*

---
## 14. Management Groups

**How many levels of management groups can you create below the root?**
- *6 (correct)*
- 3
- 10

**What is the maximum number of management groups in a tenant?**
- 1,000
- *10,000 (correct)*
- 100

**How many parent management groups can a subscription have?**
- Up to 6
- Unlimited
- *One (correct)*

**By default, where are new subscriptions placed in the management group hierarchy?**
- In a management group named Default
- *Under the root management group (correct)*
- Outside the hierarchy until assigned

**New subscriptions created by developers must automatically land in a Sandbox management group with restrictive policies. What should you configure?**
- *Sandbox as the default management group in hierarchy settings (correct)*
- An Azure Policy assignment at the root
- A resource lock on the root management group

**Which statement about the root management group is correct?**
- It can be renamed and deleted by a Global Administrator
- *It can't be moved or deleted, and nobody has access to it by default (correct)*
- All users have Reader on it by default

**You want to move a subscription from MG-A to MG-B. Which permissions are required?**
- Contributor on the subscription only
- *Owner on the subscription and management group write access on both MG-A and MG-B (correct)*
- Owner on MG-B only

**A policy that restricts VM SKUs is assigned to a management group. Which resources does it apply to?**
- Only resources created directly in the management group
- Only the first level of child subscriptions
- *All child management groups, subscriptions, resource groups and resources (correct)*

**By default, who can create new management groups in a tenant?**
- Only Global Administrators
- Only Owners of the root management group
- *Any user in the tenant, unless you require authorisation in hierarchy settings (correct)*
