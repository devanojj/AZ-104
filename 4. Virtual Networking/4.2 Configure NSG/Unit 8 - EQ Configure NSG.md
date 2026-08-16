Your company is setting up a new environment in Azure. You want to group virtual machines handling different services and define security rules accordingly. Which Azure feature would allow you to group the virtual machines based on their service type and manage security rules?

 

Application Security Groups

Network Security Groups

Incorrect

Azure Resource Groups

2.

Why is it a good practice to leave gaps in the priority numbering of network security group rules?

 

To improve the speed of rule processing

To allow for easy addition of new rules without modifying existing ones

Correct

To reduce the risk of rule conflicts

3.

A business wants to enforce security policies based on the roles of different virtual machines in their infrastructure. Which Azure feature should they implement for logical grouping and security rule assignment?

 

Application Security Groups

Correct

Network Security Groups

Azure Resource Manager Templates

4.

Which configuration is necessary for a network security group to deny all outbound traffic except traffic destined for a specified external IP address?

 

Allow outbound traffic from all ports and deny inbound traffic from the external IP address.

Set the source to the external IP address and action to deny.

Define a rule with the destination set to the external IP address and action set to allow, and another rule with destination set to Any and action set to deny.

Correct

5.

Which Azure feature allows the definition of security rules based on logical groupings of virtual machines by application workloads?

 

Azure Active Directory Groups

Network Security Groups

Application Security Groups

Correct

6.

What is the role of a demilitarized zone (DMZ) when using network security groups in Azure?

 

To automate the application of security rules

To eliminate the need for individual network security groups

To act as a buffer between internal resources and external traffic

Correct

7.

Which statement correctly describes how application security groups can enhance network security management in Azure?

 

They allow you to define security rules based on groups of virtual machines rather than individual IP addresses.

They provide automatic updates to all virtual machines in a region.

They enable centralized monitoring of network traffic across Azure regions.

Incorrect

8.

When configuring network security group rules, what is the significance of the priority value assigned to each rule?

 

A lower priority value indicates the rule is processed before higher priority values.

Correct

A higher priority value indicates the rule is processed before lower priority values.

Priority values indicate the amount of network bandwidth allocated to the rule.

9.

What is a key benefit of using application security groups in Azure?

 

They eliminate the need for network security groups entirely.

They allow you to define network security based on application logic rather than IP addresses.

Correct

They automatically configure security rules for all new VMs in a network.




✅ **Application Security Groups (ASGs)**

### Why?

The key phrase is **“group virtual machines based on their service type and manage security rules.”**

**ASGs** let you group VMs according to their **application/service role**, then reference those groups in **NSG rules**.

Example:

- `WebServers` → Web VMs
- `AppServers` → Application VMs
- `DatabaseServers` → Database VMs

Then an NSG rule could say:

**WebServers → AppServers → Allow TCP 443**

### Why the others are wrong?

- ❌ **Network Security Groups (NSGs)** → Define the **security rules**, but don't group VMs by application/service type.
- ❌ **Azure Resource Groups** → Organize Azure resources for **management/lifecycle**, not network security rules.

### Remember for AZ-104

**ASG = group VMs by application/service → use them in NSG rules**

**NSG = contains the actual network security rules**

So for this question: **Application Security Groups**.






**They allow you to define security rules based on groups of virtual machines rather than individual IP addresses.**

### Why?

**Application Security Groups (ASGs)** allow you to group VMs based on their **application/service role** and then use those groups in **NSG rules**.

Example:

- `WebServers` → Web VMs
- `AppServers` → Application VMs
- `DBServers` → Database VMs

An NSG rule can then reference:

**WebServers → AppServers → Allow TCP 443**

Instead of specifying individual VM IP addresses.

### Why the others are wrong?

❌ **Automatic updates to all virtual machines**

- ASGs don't manage VM updates or patching.

❌ **Centralized monitoring of network traffic**

- ASGs don't provide network monitoring.
- They're used to simplify **NSG security rules**.

### AZ-104 takeaway

**ASG = Group VMs by application/service → reference groups in NSG rules**

**NSG = Defines the actual network security rules**

👉 **Answer: The first option.**