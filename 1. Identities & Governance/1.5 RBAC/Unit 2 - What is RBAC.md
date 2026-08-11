**RBAC** built on **Azure Resource Manager (ARM)**.
- Give users only the permissions they need.
- Balance **team autonomy** with **central governance**.
- Works with **Microsoft Entra ID**.

Every role assignment has **3 elements**:

|Element|Meaning|
|---|---|
|**Security Principal**|Who|
|**Role Definition**|What|
|**Scope**|Where|

---
#### 1. Security Principal — Who
The identity receiving permissions.
- **User**
- **Group**
- **Application / service principal**
---
#### 2. Role Definition — What
A collection of permissions that determines what actions can be performed.
Common built-in roles:

- **Owner**
    - Full access
    - Can **delegate access** to others
- **Contributor**
    - Can create and manage Azure resources
    - **Cannot grant access**
- **Reader**
    - Can view resources
    - Cannot modify them
- **User Access Administrator**
    - Can manage **user access** to Azure resources
- **Custom roles**
    - Created when built-in roles don't meet requirements

**Exam Tip**

> **Owner = Contributor + ability to delegate access**
---
#### 3. Scope — Where
Determines where the role applies.
- Management group
- Subscription
- Resource group
- Individual resource

Example:
- Website Contributor at **resource group scope**
- User can manage websites within that resource group.
---
#### Role Assignment
> **Security Principal + Role Definition + Scope**

```
Marketing Group
      ↓
Contributor Role
      ↓
Sales Resource Group
```

- Grant access → **create role assignment**
- Revoke access → **remove role assignment**
---
#### Access Control (IAM - Identity Access Management) 
**Access control (IAM)** is the Azure portal area used to manage RBAC.

Use it to:
- View **who has access**
- View assigned **roles**
- Grant access
- Remove access

**Exam Tip**
- If asked where to manage Azure RBAC in the portal → **Access control (IAM)**
---
#### Azure RBAC Is an Allow Model
**RBAC = Allow model**
- Role assignments **grant permissions**.
- Multiple role assignments are **combined**.

Example:

```
Role 1 → Read
Role 2 → Write

Effective permissions → Read + Write
```

- Azure RBAC doesn't normally use one role to cancel another role's permissions.
---
#### Actions & NotActions
Role definitions contain:

- **Actions** → operations the role can perform.
- **NotActions** → operations excluded from the role.

**Effective permissions:**

```
Actions − NotActions = Effective Permissions
```

Example: **Contributor**

- `Actions: *` → can perform most control-plane operations.
- `NotActions` excludes certain operations, including:
    - Delete/create role assignments
    - Create roles
    - Grant User Access Administrator at tenant scope
    - Certain blueprint operations

**Exam Tip**
- **NotActions ≠ explicit deny**
- They define exclusions from a role's allowed actions. 

If there is a NotAction and an Action - the Actions wins! As it in not an explicit deny

---
#### Key Exam Facts
- **Microsoft Entra ID** → authentication/identity + directory.
- **Azure RBAC** → authorization/access to Azure resources.
- **Microsoft Entra Connect** → connects on-premises AD with Microsoft Entra ID.
- **1 subscription → 1 Microsoft Entra directory**.
- **Resource → 1 resource group**.
- **Resource group → 1 subscription**.
- **RBAC scope:** Management group → Subscription → Resource group → Resource.
- Parent-scope permissions are **inherited**.
- RBAC assignment = **Who + What + Where**.
- **Owner** can delegate access.
- **Contributor** cannot delegate access.
- **Reader** can only view.
- **User Access Administrator** manages access.
- **Access control (IAM)** = portal for RBAC.
- Azure RBAC uses an **allow model**.
- Multiple role assignments can **combine permissions**.
- **Actions − NotActions = effective permissions**.