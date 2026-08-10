#### Azure Governance Hierarchy
Azure governance uses **4 management levels**:

1. **Management Groups** → Manage multiple subscriptions.
2. **Subscriptions** → Management, billing, and scale boundary.
3. **Resource Groups** → Logical grouping of resources.
4. **Resources** → Individual Azure services/resources.

**Hierarchy:** Management Group → Subscription → Resource Group → Resource

- Settings applied at a higher level are **inherited by lower levels**.
- Management groups can be **nested**.
- Management groups can extend **6 levels below the tenant root group**.

---

#### Resource
Basic building block of Azure.

Examples:
- VM
- Virtual Network
- Database
- AI Service

---

#### Resource Group
Logical container for Azure resources.

- Resource must belong to **one** resource group.
- Resource cannot belong to multiple resource groups simultaneously.
- Actions applied to a resource group affect its resources.
- Deleting a resource group → **deletes all resources inside it**.
- Access permissions applied to a resource group → apply to its resources.

---

#### Subscription
Unit of **management, billing, and scale**.

- Contains resource groups.
- Has **resource quotas/limits**.
- Provides authenticated and authorised access to Azure services.
- Linked to an **Azure account / Microsoft Entra identity**.

---

#### Management Group
Container for **subscriptions**.

- Scope above subscriptions.
- Used for **policy, access, and compliance**.
- Subscriptions inherit conditions from management groups.
- Can be **nested**.

---

#### Azure Resource Manager (ARM)
Azure's **deployment and management service**.

- Create, update, and delete resources.
- Provides the **management layer** for Azure.
- Supports:
  - ARM templates
  - RBAC
  - Azure Policy
  - Auditing
  - Monitoring
  - Tagging

---

#### Control Plane
Manages **Azure resources**.

Examples:
- Create VM.
- Delete storage account.
- Configure resources.
- Apply policies.

**Azure Resource Manager** handles control plane operations.

**Azure Policy** operates in the control plane.

---

#### Data Plane
Accesses and manages the **data inside resources**.

Examples:
- Upload/download files from Storage.
- Query SQL database.
- Read Key Vault secrets.

- Uses service-specific permissions such as **RBAC** and **ACLs**.
- Requests go directly to the service's data endpoint.
- Does **not** go through Azure Resource Manager.

---

#### Greenfield vs Brownfield

**Greenfield** → Resources are being **created/updated** with policy already in place.

**Flow:**
RBAC → Azure Policy → Resource Provider

- RBAC is evaluated **before Azure Policy**.
- If RBAC denies the request → Azure Policy isn't evaluated.
- For updates, Azure Policy evaluates the **resulting target state**, not just the changes.

**Brownfield** → Resources **already exist** when a policy is assigned.

- Azure Policy performs a **compliance scan**.
- Automatic scan → approximately **every 24 hours**.
- Scan can also be manually triggered.
- Existing non-compliant resources are **flagged**, not automatically deleted.
- Future deployments can be **blocked** by the policy.

---

# AZ-104 Exam Facts

- **Management Group** → manages subscriptions.
- **Subscription** → management, billing, scale.
- **Resource Group** → contains resources.
- **Resource** → individual Azure service.
- **Higher scope → lower scopes inherit settings.**
- **ARM** → deployment & management.
- **Control Plane** → manages resources.
- **Data Plane** → accesses resource data.
- **RBAC is evaluated before Azure Policy.**
- **Greenfield** → policy exists before resource.
- **Brownfield** → resources exist before policy.
- **Brownfield compliance scan** → automatically approximately every 24 hours.