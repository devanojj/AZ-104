#### Azure RBAC
**Azure Role-Based Access Control (RBAC)** = Authorization system for managing access to Azure resources.
- **Who** → Who can access.
- **What** → What actions they can perform.
- **Where** → Which resources they can access.
---
#### Purpose
- Secure Azure resources.
- Grant users/partners **only the access they need**.
- Control access to:
    - VMs
    - Websites
    - Networks
    - Storage
---

#### AZ-104 Exam Facts
- **RBAC = Authorization**, not authentication.
- Controls **who can do what, where**.
- Used to manage access to **Azure resources**.


- **`Get-AzRoleDefinition`** → Get an existing **role definition**
- **`Add-AzRoleDefinition`** → Create a **custom role**
- **`Get-AzRoleAssignment`** → Find **who has access**
- **`New-AzRoleAssignment`** → **Assign** a role
- **`Set-AzRoleAssignment`** → Modify an **existing assignment**