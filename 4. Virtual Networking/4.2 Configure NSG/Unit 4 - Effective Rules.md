**Definition:** Shows the **combined rules actually applied** to a VM.

---

#### Processing Order

- **Inbound:** Subnet NSG → **NIC NSG**
- **Outbound:** NIC NSG → **Subnet NSG**
- NSGs are evaluated **independently**.

---

#### Key Points

- NSG on **both subnet + NIC** → traffic must be allowed at **both levels**.
- No NSG at a level → traffic isn't restricted by an NSG at that level.
- **Intra-subnet traffic** can be blocked with deny rules.
- **Lower priority number = processed first**.
- Leave priority gaps: `100, 200, 300` → easier to add rules.

---

#### Effective Security Rules

- **Azure Portal → Effective security rules**
- Shows the **actual rules applied** to a VM/NIC.
- **Network Watcher** provides a consolidated view.

---

#### Exam Facts

- **Inbound:** Subnet → NIC
- **Outbound:** NIC → Subnet
- Allow traffic must pass **both NSG levels**.
- **Effective rules** = what actually applies after all NSGs are evaluated.