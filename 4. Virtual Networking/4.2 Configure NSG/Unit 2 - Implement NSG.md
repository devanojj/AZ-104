Azure firewall rules that **allow/deny inbound and outbound traffic**.

- Applied to:
    - **Subnet** → protects all resources in subnet
    - **NIC** → controls traffic through that NIC
- Rules control **inbound/outbound** traffic.
- A subnet can have **max 1 NSG**.
- A NIC can have **0 or 1 NSG**.
- NSGs can be associated with multiple resources.

---

#### NSG + Subnet

- Controls traffic for **all VMs/resources in the subnet**.
- Can be used to create a **DMZ**.
- DMZ = buffer between internal resources and the internet.

---

#### NSG + NIC

- Controls traffic through a specific **network interface**.
- More granular than subnet-level control.

---

#### Exam Tips

- **Subnet = max 1 NSG**
- **NIC = 0 or 1 NSG**
- **NSG = allow/deny inbound + outbound**
- **Subnet NSG → all resources in subnet**
- **NIC NSG → specific NIC**