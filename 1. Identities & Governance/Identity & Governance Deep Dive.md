The official sub-skills are three groups:
1. **Manage Microsoft Entra users and groups** — create users and groups, manage user and group properties, manage licences, manage external users, configure self-service password reset (SSPR)
2. **Manage access to Azure resources** — manage built-in Azure roles, assign roles at different scopes, interpret access assignments
3. **Manage Azure subscriptions and governance** — Azure Policy, resource locks, tags, resource groups, subscriptions, costs (alerts, budgets, Azure Advisor), management groups

> [!warning] Things that changed recently — older study material is wrong on these
> * A new **Azure Policy assignment** takes about **5 minutes** to apply; results for a new/updated resource appear in about **15 minutes**; the full compliance scan runs every **24 hours**. Older material says 30 minutes.
> * **Role assignment limit:** **5,000 per subscription** and **500 per management group**. Older material says 2,000 or 4,000.
> * **Role Based Access Control Administrator** is the least-privilege built-in role for delegating role assignments, and supports **conditions**. Prefer it over Owner or User Access Administrator.
> * **SSPR for cloud-only users** isn't included in Entra ID **Free** (Free only allows a password *change*). Reset needs Microsoft 365 Business Standard+ or **P1/P2**; on-premises **writeback** needs **P1/P2** or Business Premium.
> * **Classic subscription administrators** (Service Administrator, Co-Administrator) are retired. Use Azure RBAC (Owner).

---
## 1. Microsoft Entra ID Fundamentals
### Tenant, Directory, Subscription
* A **tenant** is a dedicated instance of Microsoft Entra ID (the directory) for an organisation.
* **Every subscription trusts exactly one tenant.** A tenant can have many subscriptions.
* Identity is in Entra ID; authorisation to Azure resources is **Azure RBAC** — two separate systems.

### Entra ID vs Active Directory Domain Services
|            | **Microsoft Entra ID**                                  | **AD DS** (on-premises)     |
| ---------- | ------------------------------------------------------- | --------------------------- |
| Protocols  | HTTPS/REST, **OAuth 2.0, SAML, OpenID Connect**, WS-Fed | **Kerberos, NTLM, LDAP**    |
| Structure  | **Flat** — no OUs, no GPOs                              | OUs, GPOs, forests, domains |
| Querying   | Microsoft Graph API                                     | LDAP                        |
| Devices    | Entra join / registration                               | Domain join                 |
| Federation | Built-in (SaaS apps, other IdPs)                        | Needs AD FS                 |

* **Microsoft Entra Domain Services** = managed domain controllers (LDAP, Kerberos, NTLM, Group Policy) for legacy apps in Azure — you don't manage the DCs.
* **Hybrid identity:** **Microsoft Entra Connect Sync** or **Entra Cloud Sync** syncs on-prem users/groups to Entra ID. Sign-in options: **password hash sync**, **pass-through authentication**, **federation**. See [[Microsoft Entra Connect Sync]], [[Entra Cloud Sync]].

### Licensing
| Feature                                       | **Free** | **P1**                    | **P2** |
| --------------------------------------------- | -------- | ------------------------- | ------ |
| Users, groups, SSO, B2B guests                | ✅        | ✅                         | ✅      |
| MFA via **security defaults**                 | ✅        | ✅                         | ✅      |
| **Conditional Access**                        | ❌        | ✅                         | ✅      |
| **Dynamic membership groups**                 | ❌        | ✅                         | ✅      |
| **Group-based licensing**                     | ❌        | ✅ (or M365/O365 E3-level) | ✅      |
| Self-service group management                 | ❌        | ✅                         | ✅      |
| **SSPR with on-prem writeback**               | ❌        | ✅                         | ✅      |
| Administrative units (admins of an AU)        | ❌        | ✅                         | ✅      |
| Custom Entra roles                            | ❌        | ✅                         | ✅      |
| **Entra ID Protection** (risk-based policies) | ❌        | ❌                         | ✅      |
| **Privileged Identity Management (PIM)**      | ❌        | ❌                         | ✅      |
| SLA                                           | —        | 99.9%                     | 99.9%  |

See [[Entra ID Protection]], [[Privileged Identity Management]], [[Conditional Access]].

---
## 2. Users

### User Types
|Source|Description|
|---|---|
|**Cloud identity**|Created in Entra ID|
|**Directory-synchronised**|Synced from on-prem AD — **edit them on-premises** (AD is the source of authority)|
|**Guest** (external)|Invited B2B user; `userType = Guest`|

### Creating Users
* **Portal:** *New user → Create new user* (member) or *Invite external user* (guest).
* **Bulk operations** (CSV template): **bulk create**, **bulk invite**, **bulk delete**, **bulk restore**, download users/groups lists. Download the template and keep the headers exactly as provided.
* **PowerShell / Microsoft Graph / CLI** for automation.
* Key properties:
  * **User principal name (UPN)** — the sign-in name
  * **Display name**, job info, **manager**
  * **Usage location** — **required before assigning a licence**
  * **Block sign-in**
* Minimum role to manage users: **User Administrator**. Don't use Global Administrator for routine tasks.

### Deleting & Restoring
* Deleted users are **soft-deleted for 30 days** and can be **restored with all properties** (licences too, even if you're now over your licence count). After 30 days they're **permanently deleted** and can't be recovered — not even by Microsoft.
* **Microsoft 365 groups** are also soft-deleted for 30 days. **Security groups are deleted permanently** straight away.
* A **synced** user deleted in the cloud may be **re-created at the next sync** if it still exists on-prem.

### Administrative Units & Custom Security Attributes
* **Administrative unit (AU)** — a container of users/groups/devices that **scopes an Entra admin role** (e.g. Helpdesk Administrator for only the London users). Needs **P1 for the AU admins**. See [[AU]], [[AU vs Security Group]].
* **Custom security attributes** — business-specific key/values on users and apps (e.g. `Project=Falcon`) usable in access decisions and ABAC conditions. Managed by **Attribute Definition Administrator** and **Attribute Assignment Administrator** — **Global Administrator has no access by default**. See [[Security Attributes]], [[Attribute Definition Administrator]].

---
## 3. Groups

### Group Types
|                           | **Security group**                                             | **Microsoft 365 group**                                        |
| ------------------------- | -------------------------------------------------------------- | -------------------------------------------------------------- |
| Used for                  | Access to resources, **licences**, Azure/Entra role assignment | Collaboration: shared **mailbox, calendar, SharePoint, Teams** |
| Members                   | Users, devices, service principals, **other security groups**  | **Users only**                                                 |
| Dynamic device membership | ✅                                                              | ❌                                                              |
| Restorable after deletion | ❌                                                              | ✅ (30 days)                                                    |

### Membership Types
* **Assigned** — added manually.
* **Dynamic user** — rule on user attributes, e.g. `(user.department -eq "Sales") and (user.country -eq "UK")`.
* **Dynamic device** — rule on device attributes, e.g. `(device.deviceOSType -eq "Windows")`. **Security groups only.**

> [!warning] Dynamic group rules
> * Needs **Entra ID P1** for every unique user in a dynamic group.
> * You **can't manually add or remove** members of a dynamic group.
> * One rule can't mix **users and devices**, and device rules **can't use the owner's attributes**.
> * The rule builder handles up to 5 expressions — use the text box for more complex rules.
> * You can change a group from assigned to dynamic (and back).

### Other Group Facts
* **Group owners** can manage membership. **Self-service group management** (users create/request groups) needs P1.
* **Role-assignable groups** (`isAssignableToRole`, set at creation) can hold **Entra roles**; membership must be **assigned**, not dynamic.
* **Group-based licensing applies only to direct members** — users in a nested group don't get the licence.
* **Expiration policy** can be set for Microsoft 365 groups.

---
## 4. Licences

* Assign **directly** or **group-based** (preferred) in the Entra admin centre or Microsoft 365 admin centre. Set **usage location** first.
* A user with the same licence both directly and through a group **consumes only one** licence.
* **Delete licences from a group before deleting the group.**
* After fixing an error, **Reprocess** the user or group.

|Error|Cause|Fix|
|---|---|---|
|**CountViolation**|Not enough licences|Buy more or free some up|
|**MutuallyExclusiveViolation**|Two conflicting service plans|Remove/disable one of the conflicting plans|
|**DependencyViolation**|Add-on needs a prerequisite plan|Make sure required plans stay assigned|
|**ProhibitedInUsageLocationViolation**|Licence not available in the user's usage location|Change usage location|
|**Duplicate proxy address**|Exchange proxy address conflict|Fix the address, then reprocess|
|**LicenseAssignmentAttributeConcurrencyException**|Concurrent licence operations|Nothing — Entra retries automatically|

* If **one licence in a group fails** for a user, **none** of that group's licences are assigned to them.
* See [[Errors]] and the 1.2 licensing units.

---
## 5. External Users (B2B Collaboration)

* Invite: **Users → New user → Invite external user** (or bulk invite). The guest gets `userType = Guest` and redeems an email invitation, then signs in with **their own** identity (their Entra tenant, Microsoft account, Google, email one-time passcode…).
* Guests can be added to groups, apps and **Azure RBAC role assignments** like any user.

### External Collaboration Settings
| Setting                        | Options (default in **bold**)                                                                                                                                               |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Guest user access**          | Same access as members · **Limited access to properties and memberships of directory objects** · Restricted to their own objects (most restrictive)                         |
| **Guest invite settings**      | **Anyone in the organisation, including guests and non-admins** · Members and specific admin roles · Only users with specific admin roles (e.g. **Guest Inviter**) · No one |
| **Collaboration restrictions** | Allow invitations to any domain · **deny list** of domains · **allow list** of domains                                                                                      |
| **Self-service sign-up**       | Let external users sign up for apps via user flows                                                                                                                          |

* **Cross-tenant access settings** control **inbound and outbound** B2B with **other Entra tenants** (by user, group, app) and can **trust their MFA and device claims**. When external collaboration settings and cross-tenant settings disagree, the **most restrictive** applies.
* **B2B direct connect** — mutual trust for Teams shared channels without guest accounts.

---
## 6. Self-Service Password Reset (SSPR)

### Licensing
|Scenario|Free|M365 Business Standard|M365 Business Premium|Entra ID P1/P2|
|---|---|---|---|---|
|Cloud user **changes** a known password|✅|✅|✅|✅|
|Cloud user **resets** a forgotten password|❌|✅|✅|✅|
|Hybrid user reset with **on-prem writeback**|❌|❌|✅|✅|

### Configuration (Entra ID → Password reset)
* **Enabled for:** **None**, **Selected** (a group), or **All**.
* **Authentication methods:** mobile app notification, mobile app code, email, mobile phone (SMS/call), office phone, security questions.
* **Number of methods required to reset:** **1 or 2**.
* **Administrators always use a two-method policy**, and **security questions can't be used by admins** (admin SSPR is on by default regardless of your settings).
* **Registration:** require users to register at sign-in, and re-confirm every N days.
* **Notifications:** notify users on reset; notify all admins when another admin resets.
* **On-premises integration:** **password writeback** via **Entra Connect Sync** or **Cloud Sync** (outbound 443 only, no inbound firewall rules), and optional account unlock.
* **Customisation:** Helpdesk link/email, **company branding** on the sign-in page.

---
## 7. Azure RBAC — Roles and Assignments
### Entra Roles vs Azure Roles
|          | **Microsoft Entra roles**                                                                              | **Azure roles (RBAC)**                                                |
| -------- | ------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------- |
| Control  | **The directory**: users, groups, licences, apps, domains                                              | **Azure resources**: VMs, storage, networks…                          |
| Examples | Global Administrator, User Administrator, Helpdesk Administrator, Guest Inviter, Billing Administrator | Owner, Contributor, Reader, User Access Administrator, VM Contributor |
| Scope    | Tenant or administrative unit                                                                          | Management group → subscription → resource group → resource           |
| Portal   | Entra ID → Roles and administrators                                                                    | **Access control (IAM)** on any scope                                 |

> [!tip] Global Administrator ≠ Owner
> Entra roles grant **no** Azure resource access. A **Global Administrator** can **elevate access** (Entra ID → Properties → *Access management for Azure resources* = Yes), which grants **User Access Administrator at root scope `/`** — then they can assign themselves any Azure role. Remove it when done.

### Role Assignment = Who + What + Where
* **Security principal (who):** user, group, service principal, **managed identity**.
* **Role definition (what):** collection of permissions.
* **Scope (where):** **management group → subscription → resource group → resource**. Assignments are **inherited** by child scopes.

### Key Built-in Roles
|Role|Can manage resources|Can assign roles|Notes|
|---|---|---|---|
|**Owner**|✅|✅|Full control|
|**Contributor**|✅|❌|Can't assign roles, manage Blueprints or share image galleries|
|**Reader**|View only|❌|—|
|**User Access Administrator**|View only|✅|Manages access only|
|**Role Based Access Control Administrator**|View only|✅ (RBAC only)|**Least privilege for delegating access**; supports **conditions** (e.g. "may only assign Backup Reader to the Sales group")|

Service-specific roles to recognise:
* **Virtual Machine Contributor** — manage VMs, but not the VNet or storage they connect to
* **Network Contributor**
* **Storage Account Contributor**
* **Storage Blob Data Owner / Contributor / Reader** — data plane
* **Website Contributor**
* **DNS Zone Contributor**
* **Backup Contributor / Backup Operator / Backup Reader**
* **Monitoring Reader / Monitoring Contributor**
* **Tag Contributor** — tags only
* **Resource Policy Contributor**
* **Cost Management Reader / Contributor**, **Billing Reader**
* **Key Vault Administrator / Secrets User**

### Role Definitions & Custom Roles
```json
{
  "Name": "VM Operator",
  "Description": "Start, restart and deallocate VMs",
  "Actions": [
    "Microsoft.Compute/virtualMachines/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Compute/virtualMachines/deallocate/action"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [ "/subscriptions/<subscription-id>" ]
}
```
* **Actions** = control-plane operations; **DataActions** = data-plane operations (e.g. read blob contents).
* **Effective permissions = Actions − NotActions.** `NotActions` is **not a deny** — another role assignment that grants that action still wins (RBAC is **additive**).
* **AssignableScopes** — where the custom role can be assigned. **Can't be root `/`**, can contain **only one management group**, and a role with **DataActions can't be assigned at management group scope**.
* Up to **5,000 custom roles per tenant**. Create by **cloning** a role in the portal, or from JSON with PowerShell (`New-AzRoleDefinition`) / CLI (`az role definition create`).
* **Deny assignments** block actions even for Owners, but **you can't create them directly** — they're created by Azure (managed applications, deployment stacks).

### Interpreting Access
* **Access control (IAM) → Check access** — see one user/group/principal's assignments at that scope (including inherited ones).
* **Role assignments** tab — who has what here; the *Scope* column shows **"This resource"** vs **inherited**. **Download role assignments** exports a CSV.
* **Roles** tab — what each role allows. **Deny assignments** tab.
* **Activity log** — filter on *Create role assignment* / *Delete role assignment* to audit who granted access when.
* An **inherited** assignment can only be removed **at the scope where it was assigned** (e.g. the subscription), not on the child resource.
* Assignment changes can take **up to ~10 minutes** to apply; sign out/in to refresh.

**Worked example:** Alex has **Reader** on the subscription and **Contributor** on RG-Web.
* In **RG-Web**: Reader + Contributor = **Contributor** (additive).
* In **RG-Data**: **Reader** (inherited from the subscription).
* Alex **can't grant access** to anyone (neither role includes `Microsoft.Authorization/roleAssignments/write`).

---
## 8. Azure Policy

### Building Blocks
| Object                      | What it is                                                                                                                                                                                                  |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Policy definition**       | JSON `if` (conditions) → `then` (**effect**)                                                                                                                                                                |
| **Initiative** (policy set) | A group of definitions assigned together (e.g. a regulatory standard)                                                                                                                                       |
| **Assignment**              | A definition/initiative applied at a **management group, subscription or resource group**, with **parameters**                                                                                              |
| **Exclusion** (`notScopes`) | Child scopes skipped when the assignment is created                                                                                                                                                         |
| **Exemption**               | Resource/scope excused from an assignment (or specific definitions in an initiative): **Waiver** (accepting non-compliance temporarily) or **Mitigated** (intent met another way); optional **expiry date** |
| **Remediation task**        | Fixes **existing** non-compliant resources for **Modify / DeployIfNotExists**                                                                                                                               |

### Effects
|Effect|What it does|When|
|---|---|---|
|**Deny**|Blocks the create/update request|Request time|
|**Audit**|Marks non-compliant + activity log warning, doesn't block|Request + scan|
|**Append**|Adds properties (legacy; prefer Modify)|Request time|
|**Modify**|Adds/updates/removes **tags or properties** (e.g. inherit a tag from the RG)|Request time; **remediation** for existing|
|**AuditIfNotExists**|Audits when a **related resource** is missing (e.g. no diagnostic settings)|After deployment|
|**DeployIfNotExists**|**Deploys** the missing related resource|After deployment; **remediation** for existing|
|**DenyAction**|Blocks specific actions (currently **DELETE**)|Request time|
|**Disabled**|Turns the definition off|—|
|**Manual**|Compliance is attested manually|—|

* **Evaluation order at request time:** Disabled → Append/Modify → **Deny** → Audit. AuditIfNotExists and DeployIfNotExists run after the resource provider succeeds.
* **Modify** and **DeployIfNotExists** assignments need a **managed identity** (system- or user-assigned) with the right roles. The portal grants them automatically; CLI/PowerShell/SDK don't.
* New or updated resources are remediated **automatically**. **Existing** resources need a **remediation task**.
* **Deny doesn't delete** existing non-compliant resources — it only reports them as non-compliant.
* **Enforcement mode = Disabled** (*DoNotEnforce*) evaluates compliance **without applying the effect** — use it to test safely, then roll out in rings.
* **Mode:** `All` (all resource types, incl. RGs/subscriptions) or `Indexed` (only resources that support **tags and location** — use for tag/location policies).

### Common Built-in Definitions
* **Allowed locations** (resources), **Allowed locations for resource groups**
* **Allowed resource types** / **Not allowed resource types**
* **Allowed virtual machine size SKUs**
* **Require a tag and its value on resources** (Deny)
* **Add a tag to resources** / **Inherit a tag from the resource group** (Modify)
* **Audit VMs that don't use managed disks**

### Evaluation Timing
|Trigger|Time|
|---|---|
|New or updated **assignment**|Applied in **~5 minutes**, then evaluation starts|
|Resource **created/updated** in scope|Result in **~15 minutes**|
|Subscription created/moved into an MG with an assignment|~30 minutes|
|**Standard compliance scan**|Every **24 hours**|
|**On-demand scan**|`az policy state trigger-scan` / `Start-AzPolicyComplianceScan`|

### Policy vs RBAC vs Locks
|  |**Azure RBAC**|**Azure Policy**|**Resource lock**|
|---|---|---|---|
|Question answered|**Who** can do **what**|What **state** resources must be in|Stop **deletion/changes** of critical resources|
|Applies to|Users/principals|Resource properties|Everyone, **including Owners**|
|Example|Dev team can create VMs in RG-Dev|VMs may only be created in UK South|Nobody can delete the production VNet|

* Assigning policies needs **Owner** or **Resource Policy Contributor**.

---
## 9. Resource Locks

|Lock (CLI name)|Read|Modify|Delete|
|---|---|---|---|
|**Delete** (`CanNotDelete`)|✅|✅|❌|
|**Read-only** (`ReadOnly`)|✅|❌|❌|

* Apply at **subscription, resource group or resource**. Locks are **inherited** by everything below, including resources added later. The **most restrictive** lock in the chain wins.
* Locks apply to **every user and role, including Owner**. To delete a locked resource, someone with **`Microsoft.Authorization/locks/*`** (**Owner** or **User Access Administrator**) must **remove the lock first**.
* A **Delete** lock on any resource in an RG blocks **deleting the RG** — the whole delete fails (no partial delete).
* Locks only affect the **control plane** (`management.azure.com`). They **don't protect data** — a locked storage account's blobs can still be deleted; a locked database's tables can still be changed.
* **Cancelling a subscription isn't blocked** by locks.

> [!warning] Read-only lock side effects
> Many "read" operations are really **POST** requests, which a Read-only lock blocks:
> * **Storage account:** can't **list keys**; can't create containers through the control plane; can't add role assignments scoped to it.
> * **VM** (or its RG): can't **start or restart** the VM.
> * **App Service:** can't scale or change configuration.
> Use **Delete** locks for most "protect from accidents" scenarios.

```bash
az lock create --name NoDelete --lock-type CanNotDelete --resource-group RG-Prod
```

---
## 10. Tags

See [[Azure Tags]].

* Name/value pairs on **resources, resource groups and subscriptions** — **not management groups**.
* **Not inherited.** Tags on a resource group or subscription aren't applied to its resources unless you use **Azure Policy** (*Inherit a tag from the resource group* / *subscription* — Modify effect + remediation for existing resources).
* **Limits:** **50 tags** per resource/RG/subscription; tag **name 512** characters (**128 for storage accounts**), **value 256** characters. Some types allow only 15 (e.g. DNS zones, Automation, CDN). Not every resource type supports tags.
* Tag **names are case-insensitive**, **values are case-sensitive**.
* Tags are **plain text** — never store secrets in them.
* Use for **cost reporting** (group/filter in Cost analysis), automation and ownership.
* **Permissions:** **Tag Contributor** can manage tags without access to the resource itself; Contributor can tag anything it can write.
* CLI/PowerShell: `az tag update --operation merge`, `Update-AzTag -Operation Merge|Replace|Delete` (Replace overwrites all existing tags).

---
## 11. Resource Groups

* A logical container for resources that share a lifecycle.
* **Every resource is in exactly one resource group**; a resource group is in **one subscription**; resource groups **can't be nested** and **can't be renamed**.
* The RG has a **location** (where its **metadata** is stored), but it can contain resources from **any region**.
* **RBAC, Policy, locks and tags** can be applied at RG scope; RBAC/Policy/locks are inherited by its resources (tags aren't).
* **Deleting a resource group deletes everything in it.**
* **Move resources** between RGs/subscriptions: resource IDs change, source and target RGs are locked during the move, dependent resources may need to move together (see [[Compute Deep Dive]]).
* **Resource providers** (e.g. `Microsoft.Network`) must be **registered** in a subscription before you can create that resource type there.

---
## 12. Subscriptions

* A **billing and management boundary**, with its own **quotas/limits** (raise via *Usage + quotas* / support request).
* **Offer types:** Free, Pay-As-You-Go, Enterprise Agreement, Microsoft Customer Agreement, CSP, Visual Studio/MSDN.
* **Change directory** (associate the subscription with another Entra tenant):
  * **All role assignments and custom roles are permanently deleted.**
  * **Policy assignments are removed.**
  * **Managed identities** must be re-enabled/recreated, and **Key Vault tenant IDs** updated.
  * Billing ownership doesn't change; resources keep running.
  * Requires **Owner** on the subscription and an account that exists in **both** tenants.
* **Transfer billing ownership** to another account (optionally to another tenant as well).
* **Cancel** — resources are deactivated, then deleted after a waiting period (locks don't prevent it).
* **Move a subscription** between management groups (see section 14).

---
## 13. Cost Management — Budgets, Alerts and Advisor

### Budgets
* Created in **Cost Management → Budgets** at **management group, subscription, resource group** or billing scope.
* Reset **monthly, quarterly or annually** (or billing-period aligned) until an expiry date.
* **Alert conditions:** up to **5 thresholds** (% of budget) on **Actual** or **Forecasted** cost, each emailing recipients.
* **Action groups** (run automation, e.g. a runbook to shut down VMs) are supported at **subscription and resource group** scope only.

> [!warning] Budgets don't stop spending
> A budget only **notifies**. Resources keep running unless you wire the alert to an **action group** that does something (runbook, Logic App, Function).

* Cost data lands within **8–24 hours** and budgets are evaluated **every 24 hours**.

### Cost Alerts
|Alert|Fires when|
|---|---|
|**Budget alert**|Actual/forecasted cost crosses a budget threshold|
|**Credit alert**|Azure Prepayment (EA) credit is being consumed (90%/100%)|
|**Department spending quota alert**|An EA department hits its quota|
|**Anomaly alert**|Unexpected spike or dip in daily cost (subscription scope)|
|**Scheduled alert**|Emails a saved cost view daily/weekly/monthly|

### Azure Advisor
* Free, personalised recommendations in **five categories**: **Reliability, Security, Performance, Cost, Operational excellence** (the Well-Architected pillars).
* **Advisor score** — overall % and per category; postponed/dismissed recommendations drop out of the score.
* **Cost:** right-size or shut down **underutilised VMs** (low CPU and network over **7 days**; thresholds adjustable), delete unattached disks, buy reservations/savings plans.
* Recommendations can be filtered by subscription, resource type and **tags**. Changes can take up to a day to be recognised.

### Other Cost Levers
* **Reservations** (1/3-year commitment) and **savings plans**, **Azure Hybrid Benefit**, **Spot VMs**, auto-shutdown, **tags** for cost allocation.
* **Pricing calculator** (estimate new deployments) vs **TCO calculator** (compare on-prem vs Azure).
* Roles: **Cost Management Reader/Contributor**, **Billing Reader**.

---
## 14. Management Groups

* A governance scope **above subscriptions**. **RBAC and Policy assigned to a management group are inherited** by every child MG, subscription, RG and resource.
* **Limits:** **10,000** MGs per tenant; **6 levels of depth** (not counting the root or subscriptions); each MG or subscription has **exactly one parent**; unlimited subscriptions per MG.
* **Root management group** ("Tenant root group", ID = tenant ID): **can't be moved or deleted**; **nobody has access by default** — a Global Administrator must **elevate access** first.
* **New subscriptions go under the root MG** by default — configure a **default management group** in hierarchy settings (e.g. a *Sandbox* MG). You can also **require authorisation** to create new MGs (by default any user can).
* **Moving a subscription** to another MG needs: **Owner** on the subscription (role-assignment write), and **MG write** (e.g. Management Group Contributor) on **both** the current and target parent MGs — unless that parent is the root.
* All subscriptions in a hierarchy must trust the **same tenant**.
* **Tags aren't supported** on management groups. A custom role can name **only one** MG in AssignableScopes.
* ARM caches the hierarchy for up to **30 minutes**, so moves can take a while to show.

---
## 15. Exam Scenario → Answer (and *why*)

|Scenario|Answer|Why that one|
|---|---|---|
|"Add all users in the Sales department to a group automatically"|**Dynamic user** security group (`user.department -eq "Sales"`) + **P1**|Membership follows the attribute; no manual upkeep|
|"Group all Windows devices for app deployment"|**Dynamic device** **security** group|Microsoft 365 groups can't contain devices|
|"Team needs a shared mailbox, calendar and SharePoint site"|**Microsoft 365 group**|Security groups don't provision collaboration resources|
|"Assign licences to 500 new staff efficiently"|**Group-based licensing** on a security group|One assignment, automatic for new members|
|"Licence assignment fails: not enough licences"|**CountViolation** → buy or free licences, then **reprocess**|Error tells you the cause|
|"Licence assignment fails for a user in a new country"|Set/fix **usage location**|Licences require a valid usage location|
|"Restore a user deleted 2 weeks ago"|**Deleted users → Restore**|Soft-deleted for 30 days with properties and licences|
|"Recover a security group deleted yesterday"|Not possible — **recreate** it|Only Microsoft 365 groups are soft-deleted|
|"Helpdesk may reset passwords only for London users"|**Administrative unit** + Helpdesk Administrator scoped to it|AUs scope Entra admin roles|
|"Contractor from a partner needs access to one app"|**B2B guest invite**|They use their own identity; you control access|
|"Only admins may invite guests"|External collaboration settings → **Only users assigned to specific admin roles** (e.g. Guest Inviter)|Default lets everyone, including guests, invite|
|"Block invitations to a competitor's domain"|**Collaboration restrictions — deny list**|Domain-level allow/deny for invitations|
|"Guests must not see other users' details"|Guest user access → **Restricted to own objects**|Default is limited, not restricted|
|"Users reset their own passwords; changes must reach on-prem AD"|SSPR + **password writeback** (Entra Connect/Cloud Sync) + **P1**|Writeback needs P1/P2 or Business Premium|
|"Require two verification methods for SSPR"|Number of methods required = **2**|Admins always need 2 regardless|
|"User must create and manage resources but not grant access"|**Contributor**|Owner can assign roles|
|"Delegate role assignments but only Backup roles to the Sales group"|**Role Based Access Control Administrator** **with conditions**|Least privilege; Owner/UAA are unrestricted|
|"Give a user access to one VM only"|Assign the role at the **VM (resource) scope**|Narrowest scope that works|
|"Developer needs full access to everything in one RG"|**Contributor at the RG scope**|Least privilege that still covers the RG|
|"Same permissions across 30 subscriptions"|Assign at a **management group**|Inherited by all children; one assignment|
|"Built-in roles don't fit — need start/restart VMs only"|**Custom role** (clone VM Contributor, trim Actions)|Exactly the actions required|
|"Removed a user's role on a VM but they still have access"|Access is **inherited** — remove it at the **parent scope** where it was assigned|Child scopes can't remove inherited assignments|
|"Who granted Owner last week?"|**Activity log** filtered on *Create role assignment*|IAM shows current state, not history|
|"What access does Alex have on this RG?"|**IAM → Check access**|Shows direct and inherited assignments|
|"Global Admin can't see any subscriptions"|**Elevate access** → User Access Administrator at root `/`|Entra roles don't grant Azure access|
|"Resources may only be created in UK South and UK West"|Policy **Allowed locations** (Deny)|RBAC can't restrict regions|
|"Every resource must have a CostCentre tag; block if missing"|**Require a tag** (Deny)|Prevents non-compliant deployments|
|"Resources should get the RG's CostCentre tag, including existing ones"|**Inherit a tag from the resource group** (Modify) + **remediation task**|Tags aren't inherited; Modify fixes new, remediation fixes existing|
|"Deploy diagnostic settings wherever they're missing"|**DeployIfNotExists** + managed identity + remediation|Audit would only report|
|"Test a policy's impact without blocking anything"|Assignment **enforcement mode Disabled**|Compliance is evaluated, effect isn't applied|
|"One resource must be excused from a policy temporarily"|**Exemption — Waiver** with expiry|Exclusion is permanent and set at assignment|
|"New policy shows no compliance data after 10 minutes"|Wait (~15 min per resource, 24 h scan) or **`az policy state trigger-scan`**|Evaluation is asynchronous|
|"Nobody, including Owners, may delete the production RG"|**CanNotDelete lock** on the RG|RBAC can't restrict Owners; locks can|
|"Prevent any configuration change to a firewall"|**ReadOnly lock**|Blocks modify and delete|
|"App fails to read storage keys after a lock was applied"|It's a **ReadOnly** lock — listKeys is a POST; use a Delete lock or Entra auth|Read-only blocks POST operations|
|"Delete a locked resource"|**Owner/User Access Administrator removes the lock**, then deletes|Locks override permissions|
|"Tag subscription resources without giving resource access"|**Tag Contributor**|Tags only|
|"Get an email when spend reaches 80% of £5,000"|**Budget** with an 80% **actual** alert|Budgets notify; Advisor doesn't|
|"Automatically shut down VMs when the budget is exceeded"|Budget → **action group** → runbook/Logic App|Budgets alone never stop resources|
|"Find VMs you can downsize to save money"|**Azure Advisor — Cost**|Right-sizing recommendations from 7-day usage|
|"New subscriptions shouldn't land in the root MG"|Set a **default management group**|Root is the default otherwise|
|"Move a subscription between management groups"|Owner on the subscription + MG write on **source and target** parents|Three permission checks|
|"Associate a subscription with a new tenant — what breaks?"|**All RBAC assignments, custom roles and policy assignments**; managed identities need recreating|Billing is unaffected|

---
## 16. Portal Practice That Pays Off

- [ ] Create a user, set usage location, assign a licence; bulk-create users from CSV
- [ ] Delete a user and restore it from *Deleted users*
- [ ] Create a **dynamic user** group (department rule) and a **dynamic device** group
- [ ] Assign a licence to a group, cause a **CountViolation**, fix it and **reprocess**
- [ ] Invite a **guest**, then restrict guest invites to admins and add a domain to the **deny list**
- [ ] Enable **SSPR** for a selected group with 2 methods; test as a user
- [ ] Create an **administrative unit** and scope Helpdesk Administrator to it
- [ ] Assign Contributor at an RG and Reader at the subscription; use **Check access** to see the combined result
- [ ] Clone VM Contributor into a **custom role** with only start/restart/deallocate
- [ ] Assign **Role Based Access Control Administrator** with a condition limiting which roles can be assigned
- [ ] Find a role assignment in the **Activity log**
- [ ] Assign **Allowed locations** and **Require a tag**; try to deploy a non-compliant resource
- [ ] Assign **Inherit a tag from the resource group** and run a **remediation task**
- [ ] Create an **exemption** (Waiver) and run `az policy state trigger-scan`
- [ ] Add **CanNotDelete** and **ReadOnly** locks; try to delete/modify and to list storage keys
- [ ] Create a **budget** with actual and forecasted alerts and an action group
- [ ] Review **Advisor** cost recommendations and score
- [ ] Create a management group hierarchy, move a subscription, set a default MG

---
## 17. One-Line Exam Facts

* Every subscription trusts **one** tenant; a tenant can have many subscriptions
* **P1** = Conditional Access, dynamic groups, group-based licensing, SSPR writeback; **P2** = ID Protection + PIM
* **Usage location** must be set before assigning a licence
* Deleted users and **Microsoft 365 groups**: restorable for **30 days**; **security groups**: gone immediately
* Dynamic groups: **P1 per user**, no manual members, can't mix users and devices
* **Device** dynamic membership = **security groups only**; M365 groups = users only
* Group-based licensing applies to **direct members only**
* Licence errors: **CountViolation, MutuallyExclusiveViolation, DependencyViolation, ProhibitedInUsageLocationViolation** → fix then **Reprocess**
* Guest defaults: **anyone (incl. guests) can invite**; guests have **limited** directory access
* SSPR: Free = change only; reset needs Business Standard+/P1; writeback needs **P1**/Business Premium
* SSPR admins: **always 2 methods**, **no security questions**
* Entra roles ≠ Azure roles; Global Admin **elevates** to **User Access Administrator at `/`**
* Owner = Contributor + assign roles; UAA/RBAC Admin = assign roles only
* **RBAC Administrator** + **conditions** = least-privilege delegation
* RBAC is **additive**; **NotActions isn't a deny**
* Scope order: **MG → subscription → RG → resource**, inherited downward
* **5,000** role assignments per subscription, **500** per MG; **5,000** custom roles per tenant
* Custom role AssignableScopes: not `/`, **one MG max**; DataActions roles **not assignable at MG**
* Deny assignments are **system-created only**
* Policy assignment scopes: **MG, subscription, RG**; **exclusions** at assignment, **exemptions** (Waiver/Mitigated) afterwards
* **Modify/DINE** need a **managed identity**; existing resources need a **remediation task**
* Policy timing: assignment **~5 min**, resource result **~15 min**, full scan **24 h**, on-demand `az policy state trigger-scan`
* Enforcement mode **Disabled** = evaluate without enforcing
* Tag/location policies use mode **Indexed**
* Locks: **CanNotDelete** (modify OK) / **ReadOnly** (no modify or delete); inherited; override Owners
* Lock management needs **Owner or User Access Administrator**
* Locks are **control plane only**; ReadOnly blocks **POST** (storage list keys, VM start)
* Tags: **50** per resource, **not inherited**, not on **management groups**; names case-insensitive, values case-sensitive
* RG: can't nest, can't rename; metadata in RG location; delete RG = delete all
* Change subscription directory → **RBAC assignments, custom roles and policy assignments deleted**
* Budgets **only notify**; action groups at **subscription/RG** scope; up to **5** thresholds; actual or forecasted
* Advisor: **Reliability, Security, Performance, Cost, Operational excellence**
* MGs: **10,000** per tenant, **6 levels** below root, one parent each; new subs land in **root** unless a default MG is set
