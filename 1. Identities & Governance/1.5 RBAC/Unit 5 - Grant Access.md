**Purpose**
- Assign a user the **Virtual Machine Contributor** role.
- Scope the role to a specific **resource group**.
- Allows VM management without granting unnecessary permissions.

_Steps_
1. Sign in to the **Azure portal** as an administrator with permission to assign roles.
2. Search for **Resource groups**.
3. Select the required resource group.
4. Select **Access control (IAM)**.
5. Select **Role assignments**.
6. Select **Add** → **Add role assignment**.
7. Under **Role**, search for and select **Virtual Machine Contributor**.
8. Select **Next**.
9. Under **Members**, select **Select members**.
10. Search for and select the required user.
11. Select **Select**.
12. Select **Next**.
13. Review the assignment.
14. Select **Review + assign**.

**Important**
- The user receives the role at the **resource group scope**.
- The user can manage VMs **within that resource group**.
- They don't automatically receive permissions outside the scope.

---

#### Permissions Required to Assign Roles
You need a role that allows you to create role assignments, such as:
- **User Access Administrator**
- **Owner**

**Exam Tip**
- **User Access Administrator** → manages access to Azure resources.
- **Owner** → full access + can delegate access.
- If **Add role assignment** is disabled → you likely lack permission to assign roles.

---

#### Remove Access
**Purpose**

- Remove a user's RBAC role assignment.

_Steps_
1. Open **Access control (IAM)**.
2. Go to **Role assignments**.
3. Select **View Assignments**.
4. Find the user and their assigned role.
5. Select the checkbox.
6. Select **Delete**.
7. Select **Yes** to confirm.

**Important**
- Removing the **role assignment** removes the access granted by that assignment.

---

#### Exam Facts
- Follow the **least privilege** principle.
- **Virtual Machine Contributor** → create and manage VMs.
- Assigning the role at **resource group scope** limits access to that resource group.
- **Role assignment** = grants access.
- **Deleting role assignment** = removes access.
- **User Access Administrator / Owner** → can assign roles.
- RBAC permissions are inherited from **parent scopes**.