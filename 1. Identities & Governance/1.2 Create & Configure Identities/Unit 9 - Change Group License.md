## Change Group License
- Managed in **Microsoft 365 Admin Center**
- **Billing → Licenses → Select License → Groups → Assign**

---

# Common License Errors
### Not Enough Licenses
- Buy more licenses or free unused ones.
- **PowerShell:** `CountViolation`

### Conflicting Licenses
- Two service plans can't coexist.
- Remove or disable the conflicting plan.
- **PowerShell:** `MutuallyExclusiveViolation`

### Missing Prerequisite
- Some add-on licenses require another license first.
- Example: Workplace Analytics → Exchange Online.
- **PowerShell:** `DependencyViolation`

### Invalid Usage Location
- User's **Usage Location** must support the license.
- Users inherit the tenant location if none is set.
- **PowerShell:** `ProhibitedInUsageLocationViolation`

### Duplicate Proxy Address
- Exchange Online proxy address must be unique.
- Fix the address, then **Reprocess** the user/group.

### LicenseAssignmentAttributeConcurrencyException
- Temporary error when multiple groups assign the same license.
- Microsoft Entra retries automatically.
- **No action required.**

---

# Reprocess Licenses
After fixing an issue:
- **Group → Licenses → Reprocess**
- **User → Licenses → Reprocess**

---

# Multiple Licenses
- A group can have multiple product licenses.
- If one license fails, **none** of the group's licenses are assigned.

---

# Deleting a Licensed Group
- Remove licenses **before** deleting the group.
- Dependent licenses may be converted to **direct assignments**.

---

# Migrating to Group-Based Licensing
1. Create licensing group.
2. Assign licenses to group.
3. Verify inherited licenses.
4. Remove direct user licenses.

> During migration, users may temporarily have **both direct and inherited licenses** (only **one license** is consumed).

---

# Changing License Plans (E1 → E3)
Before changing:
- Enough licenses available.
- No conflicting licenses.
- Required prerequisite licenses exist.
- If using synced groups, allow time for synchronization.

---

# AZ-104 Exam Tips
- Most licensing issues are:
  - Insufficient licenses
  - Conflicting plans
  - Missing prerequisites
  - Incorrect Usage Location
- **Reprocess** users/groups after fixing errors.
- Group-based licensing is preferred over direct licensing.