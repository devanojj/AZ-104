
## Purpose
Assign licenses to **groups** instead of individual users. No PowerShell automation required

---

## Requirements
- Microsoft Entra ID **Premium P1+** **or**
- Microsoft 365 **Enterprise E3+**

> You must own **enough licenses for every unique user** in licensed groups.

---

## How It Works
- Assign a license to a **Security Group**
- All group members inherit the license
- New members → License assigned automatically
- Removed members → License removed automatically

---

## Features
- Supports **Security Groups** (cloud or synced from on-prem AD)
- Can disable individual **service plans** within a license (e.g., disable Viva Engage)
- Supports Microsoft 365, EMS, Dynamics 365
- Users can receive licenses from **multiple groups** or direct assignment
- Duplicate licenses are only **counted once**
- License changes usually apply **within minutes**

---

## Common Issues
- Not enough available licenses
- Conflicting license assignments
- Missing **Usage Location**

> **Usage Location** must be set before assigning licenses (or user inherits the tenant default).

---

## AZ-104 Exam Tips
- ✅ Group-based licensing simplifies administration.
- ✅ Licenses are assigned to **Security Groups**.
- ✅ Users automatically gain/lose licenses based on group membership.
- ✅ One user consuming the same license from multiple groups uses **only one license**.
- ✅ Verify **Usage Location** if license assignment fails.