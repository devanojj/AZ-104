- Azure service for hosting and managing **DNS zones** and resolving domain names → IP addresses.

---
#### DNS
- **DNS** = Domain Name System
- Converts domain names → IP addresses.

**How it works**
- Checks **cache** first
- If not found → queries other DNS servers
- Returns IP or **domain not found**
---
#### DNS Record Types
- **A** → hostname → IPv4
- **AAAA** → hostname → IPv6
- **CNAME** → alias → another domain
- **MX** → mail server
- **TXT** → text / domain verification
- **NS** → name server
- **SOA** → Start of Authority
- **SRV** → service location
- **PTR** → IP → hostname
- **CAA** → allowed Certificate Authorities
---
#### Record Sets
**Definition**
- Multiple records with the same **name + type**.
**Example**
www.example.com → 10.0.0.1
www.example.com → 10.0.0.2

**Exam Fact**
- **SOA and CNAME cannot contain record sets**

---
#### Azure DNS
**Definition**
- Azure-hosted **DNS service** for managing DNS zones and records.

**Key Points**
- Managed through **Portal, CLI, PowerShell, REST API**
- Automatically creates **NS + SOA**
- Uses **Azure RBAC**, Activity Logs and Resource Locks
- **Does not register domains** → use a domain registrar
- **Does not support DNSSEC**
---
#### Azure Private DNS
**Definition**
- Provides private DNS name resolution for **Azure VNets**.

**Key Points**
- Link DNS zone → **VNet**
- Supports resolution **between VNets**
- No custom DNS infrastructure required
- Supports **split-horizon DNS**
- Supports A, AAAA, CNAME, TXT, MX, SOA, PTR, SRV
---
#### Azure DNS Alias Records
- DNS record that points directly to an **Azure resource**.

**Can point to**
- Azure **Public IP**
- **Traffic Manager** profile
- **CDN** endpoint

**Supported record types**
- **A**
- **AAAA**
- **CNAME**

**Exam Tip**
- **Alias → Azure resource**
- **CNAME → another domain name**

---
#### Exam Facts
- **A = IPv4**
- **AAAA = IPv6**
- **CNAME = domain alias**
- **MX = mail**
- **Private DNS = VNet**
- **Alias = Azure resource**
- **NS + SOA = automatically created**
- **Azure DNS ≠ domain registrar**
- **Azure DNS ≠ DNSSEC**