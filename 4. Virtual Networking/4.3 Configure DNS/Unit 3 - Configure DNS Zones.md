- **DNS zone** = container for all DNS records for a domain.
- Example: `wideworldimports.com`
---
#### Create a Public DNS Zone
**Steps**
1. Create DNS zone
2. Get Azure **name servers**
3. Update domain registrar **NS records**
4. Verify **domain delegation**
5. Configure DNS records
---
#### 1. Create DNS Zone
**Required**
- **Subscription**
- **Resource group**
- **Domain name**
- Resource group location

---
#### 2. Get Azure Name Servers
- Open the DNS zone
- Find **NS records**
- Azure provides **4 name servers**
- These are used to delegate the domain to Azure DNS

---
#### 3. Domain Delegation
- Changing the domain registrar's **NS records** to Azure DNS name servers.

**Steps**
- Sign in to domain registrar
- Edit **NS records**
- Replace existing NS records with Azure's **4 name servers**

**Exam Fact**
- Use **all 4 Azure name servers**

---
#### 4. Verify Domain Delegation
- Confirm the domain is now using Azure DNS.

**Key Points**
- Delegation can take **10+ minutes**
- Check the automatically created **SOA record**
- Use `nslookup`

nslookup -type=SOA wideworldimports.com

**SOA**
- **Start of Authority**
- Reference point for the domain's DNS information

---
#### 5. Configure DNS Records
**A Record**
Maps hostname → **IPv4 address**
Name: webserver1
Type: A
TTL: 3600
IP: 10.0.0.5
- **TTL** = how long the record is cached
- `1` TTL = **1 second**

---
#### CNAME Record
**Definition**
- **Canonical Name** / alias
- Maps one domain name → **another domain name**
**Example**
www.wideworldimports.com
wideworldimports.com

**Configuration**
Name: www
Type: CNAME
TTL: 600

**Common Use**
- Multiple domain names access the same website
- Can point to an **Azure Function**

**Exam Tip**
- **A → IP address**
- **CNAME → domain name**

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
