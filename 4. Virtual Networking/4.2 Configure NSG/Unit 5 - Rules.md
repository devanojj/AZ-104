**Definition:** Control **inbound/outbound traffic** using Network Security Group (NSG) rules.

---

#### Security Rule Properties
- **Source** → where traffic comes **from** → inbound
- **Destination** → where traffic goes **to** → outbound
- **Service** → **protocol + port** (e.g. HTTPS, RDP, SSH, DNS)
- **Priority** → determines rule processing order
    - **Lower number = higher priority**
    - Rules processed in priority order

---

#### Augmented Security Rules

**Definition:** One NSG rule can contain **multiple sources, destinations, or services**.

- Multiple **IP addresses**
- Multiple **ports / port ranges**
- Mix **Service Tags + ASGs + IP addresses**
- Reduces number of NSG rules
- Helps prevent **NSG rule sprawl**

**Example:**

- Instead of 4 rules for `80`, `443`, `8080`, `8090`
- Create **1 rule containing all 4 ports**

---

#### Exam Tips

- **Source = traffic coming from**
- **Destination = traffic going to**
- **Service = protocol/port**
- **Priority: lowest number wins**
- **Augmented rules = multiple values in one rule**