- **DNS zone** = container for all DNS records for a domain.
- Example: `wideworldimports.com`
---
When you buy a domain (e.g., `wideworldimports.com`) from a registrar, the registrar initially manages the DNS. Delegating to **Azure DNS** hands control of your DNS records over to Azure's globally distributed infrastructure.

#### The Delegation Sequence
1. **Create the DNS Zone in Azure:** Creates a empty container in Azure designed to host your domain's DNS records.
2. **Grab the 4 Azure NS Servers:** Azure automatically assigns 4 unique name servers (e.g., `ns1-01.azure-dns.com`, `ns2-01.azure-dns.net`, etc.).
3. **Update Your Registrar:** Log into your domain provider and replace their default NS records with **all 4** Azure name servers. This ensures high availability and redundancy.
4. **Verification:** Use tools like `nslookup` or `dig` to verify the delegation.
---
#### Azure Private DNS Zone
**Definition**
- DNS zone for **private name resolution** inside Azure.

**Key Points**
- **Not visible on the Internet**
- **No domain registrar required**
- Used for VM name resolution in **VNets**
- Can use custom domain names
---
#### Create Private DNS Zone
**Required**
- **Resource group**
- **Zone name**
Example:

private.wideworldimports.com

---

#### Public vs Private DNS

|Public DNS|Private DNS|
|---|---|
|Internet-facing|Internal Azure|
|Domain registrar required|No registrar required|
|Public name resolution|VNet name resolution|
|Delegate using **NS records**|Link using **VNet link**|
