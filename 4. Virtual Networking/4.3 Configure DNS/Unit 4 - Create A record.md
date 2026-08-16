#### A Record
**Definition**
- Maps hostname → **IPv4 address**.
www.example.com → 10.10.10.10
- One **A record set** can contain multiple IP addresses.
- **AAAA** → IPv6

---
#### TTL
- **Time To Live** = how long a DNS response is cached.
- Lower TTL → changes reflected sooner.
---
#### Alias Record Set
- Supported by **A, AAAA, CNAME**.
- Points to an **Azure resource**.
---
#### DNS Testing
**`nslookup`**

- Tests DNS resolution.
- Can query a specific **name server**.

nslookup www.example.com <name-server>

