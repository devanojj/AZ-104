**Definition**
- **Activity Log** = records operations performed on Azure resources.
- Can be used to track **RBAC changes**.

**Purpose**
- Audit RBAC changes.
- Troubleshoot access changes.
- Generate reports of role assignment and custom role changes.

---

#### View RBAC Changes
_Steps_
1. Select **All services**.
2. Search for **Activity log**.
3. Open **Activity log**.
4. Set **Timespan** → **Last month**.
5. Add an **Operation** filter.
6. Search for **role**.
7. Select the required RBAC operations:
    - **Create role assignment**
    - **Delete role assignment**
    - **Create or update custom role definition**
    - **Delete custom role definition**
8. Select an operation to view its **activity log details**.

---

#### Export Activity Log
- Activity Log results can be downloaded as a **CSV**.
- Useful for creating an RBAC **audit report**.

---

#### RBAC Operations to Know

|Operation|Purpose|
|---|---|
|**Create role assignment**|Grants a role|
|**Delete role assignment**|Removes a role assignment|
|**Create/update custom role definition**|Creates or modifies a custom role|
|**Delete custom role definition**|Removes a custom role|

---

#### Exam Facts
- **Azure Activity Log** → tracks Azure resource operations.
- Use **Activity Log** to audit **RBAC changes**.
- Filter **Operation** by `role` to find RBAC changes.
- **Timespan → Last month** → view recent RBAC changes.
- Activity Log can be exported as **CSV**.