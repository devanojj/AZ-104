- Azure service for hosting and managing **DNS zones** and resolving domain names → IP addresses.

---
#### DNS
Domain Name System, converts domain names → IP addresses.

**How it works:** Checks **cache** first, If not found → queries other DNS servers, Returns IP or **domain not found**

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
- Multiple records with the same **name + type**.

**Example**
www.example.com → 10.0.0.1
www.example.com → 10.0.0.2

**Exam Fact**
- **SOA and CNAME cannot contain record sets**

---
#### Azure DNS
Azure-hosted DNS service for managing DNS zones and records.

**Key Points**
- Managed through **Portal, CLI, PowerShell, REST API**
- Automatically creates **NS + SOA**
- Uses **Azure RBAC**, Activity Logs and Resource Locks
- **Does not register domains** → use a domain registrar
- **Does not support DNSSEC**
---
#### Azure Private DNS
Provides private DNS name resolution for **Azure VNets**.

**Key Points**
- Link DNS zone → **VNet**
- Supports resolution **between VNets**
- No custom DNS infrastructure required
- Supports **[[Split-horizon DNS]]**
- Supports A, AAAA, CNAME, TXT, MX, SOA, PTR, SRV
---
#### Azure DNS Alias Records
DNS record that points directly to an **Azure resource**.

**Can point to:** Azure Public IP | Traffic Manager profile | CDN endpoint

**Supported record types**
- **A**
- **AAAA**
- **CNAME**

**Exam Tip**
- **Alias → Azure resource**
- **CNAME → another domain name**


*To configure VN to a private DNS, create virtual network link*

*DNS Private Resolver*
Used to proxy DNS between on premises and DNS. 

*Custom DNS Server*
Can be used to register VNet but can't be private 