**Your company is setting up a new environment in Azure. You want to group virtual machines handling different services and define security rules accordingly. Which Azure feature would allow you to group the virtual machines based on their service type and manage security rules?**
- *Application Security Groups* *(correct)*
- Network Security Groups (selected)
- Azure Resource Groups

**Why is it a good practice to leave gaps in the priority numbering of network security group rules?**
- To improve the speed of rule processing
- *To allow for easy addition of new rules without modifying existing ones (correct)*
- To reduce the risk of rule conflicts

**A business wants to enforce security policies based on the roles of different virtual machines in their infrastructure. Which Azure feature should they implement for logical grouping and security rule assignment?**
- *Application Security Groups (correct)*
- Network Security Groups
- Azure Resource Manager Templates

**Which configuration is necessary for a network security group to deny all outbound traffic except traffic destined for a specified external IP address?**
- Allow outbound traffic from all ports and deny inbound traffic from the external IP address.
- Set the source to the external IP address and action to deny.
- *Define a rule with the destination set to the external IP address and action set to allow, and another rule with destination set to Any and action set to deny. (correct)*

**Which Azure feature allows the definition of security rules based on logical groupings of virtual machines by application workloads?**
- Azure Active Directory Groups
- Network Security Groups
- *Application Security Groups (correct)*

**What is the role of a demilitarised zone (DMZ) when using network security groups in Azure?**
- To automate the application of security rules
- To eliminate the need for individual network security groups
- *To act as a buffer between internal resources and external traffic (correct)*

**Which statement correctly describes how application security groups can enhance network security management in Azure?**
- *They allow you to define security rules based on groups of virtual machines rather than individual IP addresses. (correct)*
- They provide automatic updates to all virtual machines in a region.
- They enable centralised monitoring of network traffic across Azure regions. (selected) 

**When configuring network security group rules, what is the significance of the priority value assigned to each rule?**
- *A lower priority value indicates the rule is processed before higher priority values (correct)*
- A higher priority value indicates the rule is processed before lower priority values.
- Priority values indicate the amount of network bandwidth allocated to the rule.

**What is a key benefit of using application security groups in Azure?**
- They eliminate the need for network security groups entirely.
- *They allow you to define network security based on application logic rather than IP addresses. (correct)*
- They automatically configure security rules for all new VMs in a network.

