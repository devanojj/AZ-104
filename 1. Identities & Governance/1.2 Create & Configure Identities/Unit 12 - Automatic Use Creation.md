## What is SCIM?
Open standard for **automating user provisioning and de-provisioning** between identity systems.

*System for Cross-Domain Identity Management*

---

## Components
- **HCM** – HR system (e.g., Workday, SAP)
- **Microsoft Entra ID** – Identity repository
- **Microsoft Entra Provisioning Service** – Uses **SCIM 2.0** to sync users/groups
- **Target System** – App with a SCIM endpoint

---

## Benefits
- Automatic user creation
- Automatic updates
- Automatic de-provisioning
- Keeps identities synchronised across systems

---

## API-Driven Inbound Provisioning
Used when an HR system **doesn't support SCIM**.
- Automation/scripts send workforce data to the **Microsoft Entra Provisioning API**
- Supports:
  - Workday
  - SAP SuccessFactors
  - Custom HR systems

---

## AZ-104 Exam Tips
- **SCIM = Automatic provisioning & de-provisioning**
- Uses **SCIM 2.0**
- Syncs users between **HR systems, Microsoft Entra ID, and applications**
- If SCIM isn't available → Use **API-driven inbound provisioning**