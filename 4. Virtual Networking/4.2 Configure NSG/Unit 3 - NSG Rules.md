**Definition:** Filter inbound/outbound network traffic.

---
#### Key Points
- **Default rules** are automatically created; cannot be deleted.
- **Priority:** `100–4096` → **lower number = higher priority**.
- Rules processed **highest priority → lowest**.
- Custom rules can **override default rules** by having higher priority.

#### Rule Settings
- **Source** → IP, Service Tag, ASG, etc.
- **Source port**
- **Destination** → IP, Service Tag, ASG, etc.
- **Protocol** → TCP, UDP, ICMP, etc.
- **Action** → Allow / Deny
- **Priority** → 100–4096
---
#### Default Traffic

- **Inbound:** Deny all except **VNet** + **Azure Load Balancer**
- **Outbound:** Allow **Internet** + **VNet**
---

#### Exam Facts
- **Lower priority number = processed first**
- **Cannot delete default rules**
- Custom rules can **override defaults**
- NSG can be associated with a **subnet or NIC**