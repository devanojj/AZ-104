You have an Azure subscription that contains a virtual network named VNet1.

You plan to deploy a virtual machine named VM1 to be used as a network inspection appliance.

You need to ensure that all network traffic passes through VM1.

What should you do?

Select only one answer.

Configure a user-defined route.

**This answer is correct.**

Create a virtual network gateway.

Modify the default route.

Modify the system route.

Azure automatically creates a route table for each subnet on an Azure virtual network and adds system default routes to the table. You can override some of the Azure system routes with custom user-defined routes and add more custom routes to route tables. Azure routes outbound traffic from a subnet based on the routes on a subnet's route table.


You have an Azure virtual network that contains four subnets. Each subnet contains 10 virtual machines.

You plan to configure a network security group (NSG) that will allow inbound traffic over TCP port 8080 to two virtual machines on each subnet. The NSG will be associated to each subnet.

You need to recommend a solution to configure the inbound access by using the fewest number of NSG rules possible.

What should you use as the destination in the NSG?

Select only one answer.

an application security group

**This answer is correct.**

a service tag

the subnets of the virtual machines

Application security groups allow you to group together the network interfaces from multiple virtual machines, and then use the group as the source or destination in an NSG rule. The network interfaces must be in the same virtual network.

You can use the IP address of each virtual machine as the destination, but you must create a rule for each virtual machine.

Using the subnets will require four rules and will also allow traffic to all the virtual machines on those subnets.

Service tags are for specific Azure services, such as Azure App Service or Azure Backup.

[Azure application security groups overview | Microsoft Learn](https://learn.microsoft.com/azure/virtual-network/application-security-groups)

[Configure network security groups - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-network-security-groups/)


You create several Azure virtual machines that run Windows Server.

You need to connect to the virtual machines without exposing RDP ports over the internet.

Which Azure service should you deploy?

Select only one answer.

Azure Bastion

**This answer is correct.**

Azure Front Door

Azure Network Watcher

Azure Virtual Desktop

Azure Bastion is a service that lets you connect to a virtual machine by using a browser, without exposing RDP and SSH ports. Azure Monitor helps you maximize the availability and performance of applications and services. Azure Network Watcher provides tools to monitor, diagnose, view metrics, and enable or disable logs for resources in an Azure virtual network. Remote Desktop is a feature of the operating system, which exposes the RDP port to connect to a server from the internet.

[About Azure Bastion | Microsoft Learn](https://learn.microsoft.com/azure/bastion/bastion-overview)

[Configure virtual networks - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-virtual-networks/)


You have three network security groups (NSGs) named NSG1, NSG2, and NSG3. Port 80 is blocked in NSG3 and allowed in NSG1 and NSG2.

You have four Azure virtual machines that have the following configurations:

VM1:

- Subnet: Subnet1
- Network card: NIC1
- NIC1 is associated with NSG2.

VM2:

- Subnet: Subnet1
- Network card: NIC2
- NIC2 is associated with NSG3.

VM3:

- Subnet: Subnet3
- Network card: NIC3
- NIC3 is associated with NSG3.

VM4:

- Subnet: Subnet2

You have the following subnets:

- Subnet1 is associated with NSG1.
- Subnet2 is associated with NSG3.
- Subnet 3 does not have an NSG associated.

Which virtual machine can be accessed over the internet on port 80?

Select only one answer.

VM1

**This answer is correct.**

VM2

VM3

VM4






