You have an Azure subscription that contains the following virtual networks:

- VNet1: Has an IP address space of 10.10.0.0/16 and contains a subnet named Subnet1 (10.10.1.0/24) that hosts a virtual machine named VM1 that runs Windows Server.
- VNet2: Has an IP address space of 10.20.0.0/16 and contains a subnet named Subnet2 (10.20.1.0/24) that hosts a virtual machine named VM2 that runs Windows Server.

VNet1 and VNet2 are connected by using virtual network peering.

Users report that VM1 cannot connect to VM2.

You need to verify whether the traffic from VM1 to the 10.20.0.0/16 subnet uses virtual network peering as the next hop.

What should you use?

Select only one answer.

Connection troubleshoot in Azure Network Watcher from VM1 to VM2

**This answer is incorrect.**

the effective routes for the network interface of VM1

**This answer is correct.**

Azure Network Watcher next hop for the network interface of VM1

the Network Controller role in VM1

**Objective:**

4.1 Configure and manage virtual networks in Azure

**What This Item Tests:**

Create and configure virtual networks and subnets

**Additional Reading:**

[Constraints for peered virtual networks - Training | Microsoft Learn](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview#troubleshoot)

[Network troubleshooter - Training | Microsoft Learn](https://learn.microsoft.com/en-us/troubleshoot/azure/app-service/troubleshoot-vnet-integration-apps#network-troubleshooter)

[Manage virtual networks - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/describe-microsoft-azure-resources-management/4-manage-virtual-networks)

**Rationale:**

Viewing the effective routes on the network interface of VM1 shows all the system, peering, and user-defined routes that Azure applies to outbound traffic, including the next hop type for the 10.20.0.0/16 prefix.

Connection troubleshoot validates reachability but does not display routing decisions.

Azure Network Watcher next hop is a diagnostic tool that identifies the next routing hop (type, IP address, and route table ID) for traffic leaving a virtual machine. Next hop does not display routing decisions.

The Network Controller role in Windows Server is a centralized, programmable management point for Software Defined Networking (SDN).



