## What Are They?
Business-specific **key-value pairs** assigned to Microsoft Entra objects.

---
## Supported Objects
- Users
- Enterprise Applications (Service Principals)
---
## Features
- Tenant-wide
- Description supported
- Data types:
  - String
  - Integer
  - Boolean
- Single or multiple values
- Free-form or predefined values
- Can sync from on-premises Active Directory
---
## Not Supported
- Microsoft Entra Domain Services
- SAML claims
- JWT claims
---

## AZ-104 Exam Tips
- **Custom Security Attributes = business-specific metadata**
- Used for **categorisation, filtering, and access control**
- Supported on **Users** and **Enterprise Applications**
- **vs. Dynamic Groups:** Custom Attributes store key-value metadata ("What describes this object?"), whereas Dynamic Groups manage membership ("Who belongs together?").
- **Use Case:** Attach fine-grained business metadata directly to users without creating hundreds of complex dynamic groups.