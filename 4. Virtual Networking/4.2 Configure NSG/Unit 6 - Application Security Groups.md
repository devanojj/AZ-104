Logically group VMs by **workload/application** and use the group in NSG rules.

---
#### Purpose

- Group VMs by role:
    - **Web servers**
    - **Application servers**
    - Database servers
- NSG rules reference the **ASG instead of individual IP addresses**
---
#### Steps
1. Create an **ASG** for each workload/tier
2. Add VM **network interfaces (NICs)** to the ASG
3. Create **NSG rules** using ASGs as source/destination
---
#### Example
**Web ASG → Application ASG**
- Priority **100** → Allow Internet → Web ASG → **HTTP 80 + HTTPS 443**
- Priority **110** → Allow Web ASG → Application ASG → **SQL 1433**
- Priority **120** → Deny Internet → Application ASG → **HTTP 80 + HTTPS 443**
Result:

**Internet → Web → Application/Database**

Only web servers can access the application servers on SQL 1433.

---

#### Key Advantages
- **No IP maintenance** → rules reference ASGs, not individual IPs
- **No subnet requirement** → group VMs by workload instead
- **Simpler NSG rules** → one rule can apply to many VMs
- **Dynamic** → adding a VM to an ASG automatically applies relevant NSG rules
- **Workload-based** → easier to understand and maintain
---
#### ASG vs Service Tag
- **ASG** → groups **VMs** by workload
- **Service Tag** → represents **Azure service IP ranges**
---
#### Exam Tips
- **ASG = logical group of VMs**
- ASGs are used **with NSGs**
- ASG can be **Source or Destination** in an NSG rule
- ASGs are associated with **NICs**, not directly with VMs
- **ASG ≠ subnet**
- Think: **ASG = "which group of VMs?"**