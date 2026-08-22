**Firewalls & Virtual Networks**

- Restrict storage access to:
    - Specific **VNets/subnets**
    - **Public IP ranges**
- Configure in **Storage Account → Networking → Firewalls and virtual networks**
- VNets/subnets must be in the same **region or region pair**
- Test access restrictions after configuration

---

#### Service Endpoints

- Storage keeps its **public endpoint**
- Restricts access to selected **VNets/subnets**
- Simpler configuration
- Suitable for **development** or scenarios allowing some public access

---

#### Private Endpoints

- Assigns a **private IP** from your VNet to the storage account
- Traffic stays on the **Microsoft backbone**
- No public internet exposure
- Provides stronger **network isolation**
- Recommended for **production, security, and compliance**

---

#### Exam Tip

- **Service endpoint → public endpoint + VNet restriction**
- **Private endpoint → private IP + private network access**
- **Production/isolation → Private Endpoint**