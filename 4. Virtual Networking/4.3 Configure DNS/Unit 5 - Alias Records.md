- **Alias record** = DNS record that links a domain to an **Azure resource**.
- Especially useful for the **zone apex** (`@` / `wideworldimports.com`).

---
#### Zone Apex
- **Apex** = root of the domain.
- Example: `wideworldimports.com`
- Represented as **`@`** in DNS records.
- **CNAME cannot be used at the zone apex.**
---
#### Alias Targets
Alias records can point to:
- **Traffic Manager** profile
- **CDN** endpoint
- **Public IP** resource
- **Front Door** profile
---
#### Why Use Alias Records?
- **Prevents dangling DNS records**
- Automatically follows changes to the target Azure resource
- Supports **load-balanced applications** at the zone apex
- Avoids relying directly on a resource's changing IP address
---
#### Supported Types
- **A**
- **AAAA**
- **CNAME**
---
#### Exam Tip
**Scenario:** `wideworldimports.com` needs to point to an Azure Load Balancer.
→ Use an **A record alias**.
**Key distinction:**

- **Normal A record** → points directly to an IP
- **Alias A record** → points to an **Azure resource**