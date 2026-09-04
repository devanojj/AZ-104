- Use the Azure portal to view:
    - Your assigned **RBAC roles**
    - Resource group role assignments
    - Available Azure roles

---
#### View Your Own Role Assignments
_Steps_
1. Sign in to **Azure portal**.
2. Open the **Profile** menu.
3. Select **...** → **My permissions**.
4. View:
    - Assigned **roles**
    - Role **scope**

**Exam Tip**
- **My permissions** → shows the roles assigned to **you**.

---
#### View Resource Group Role Assignments
_Steps_

1. Search for **Resource groups**.
2. Select the required resource group.
3. Select **Access control (IAM)**.
4. Open **Role assignments**.

Shows:

- Who has access
- Which **role** they have
- Where the role applies

**Important**
- **This resource** → role assigned directly at that resource group.
- **Inherited** → role inherited from a **parent scope**.

---
#### View Azure Roles
- Azure provides **70+ built-in roles**.
- Roles = collections of **permissions**.
- Roles can be:
    - **Built-in**
    - **Custom**

_Steps_
1. Open **Access control (IAM)**.
2. Select **Roles**.
3. Select **View** for a role.
4. Open **Assignments** to see users/groups assigned to the role.

---
#### Exam Facts
- **My permissions** → view your own RBAC assignments.
- **Access control (IAM) → Role assignments** → view who has access.
- **This resource** → directly assigned role.
- **Inherited** → inherited from parent scope.
- **Roles** → view available built-in/custom roles.
- RBAC roles contain **permissions**.