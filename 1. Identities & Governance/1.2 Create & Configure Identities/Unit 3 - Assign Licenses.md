
## Create a User
**Microsoft Entra admin center**
- **Identity** → **Users** → **All users**
- **+ New user** → **Create new user**
- Enter:
  - User Principal Name (UPN)
  - Name
  - Password
- **Create**

---

## Create a Security Group
**Identity** → **Groups** → **All groups**
- **+ New group**
- **Group type:** Security
- **Membership type:** Assigned
- Add:
  - **Owner**
  - **Members**
- **Create**

---

## Assign a License to a Group
**Microsoft 365 admin center**
- **Billing** → **Licenses**
- Select a license
- **Groups** tab
- **+ Assign license**
- Select the security group
- **Assign**

> **Group-based licensing** automatically licenses all members of the group.

---

## Restore a Deleted User
Deleted users are retained for **30 days**.

**Identity** → **Users** → **Deleted users**
- Select user
- **Restore** or **Permanently delete**

> After **30 days**, the user is permanently deleted and **cannot be restored**.

---

## Required Roles
- Global Administrator
- User Administrator
- Partner Tier-1 Support
- Partner Tier-2 Support

---

## Exam Tips
- **Group-based licensing** is managed in the **Microsoft 365 admin center**.
- **Security groups** can be used to assign licenses.
- Deleted users are recoverable for **30 days**.
- Permanent deletion is **irreversible**.