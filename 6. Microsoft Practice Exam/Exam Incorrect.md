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



You have an Azure subscription that contains an ASP.NET application. The application is hosted on four Azure virtual machines that run Windows Server.

You have a load balancer named LB1 that load balances requests to the virtual machines.

You need to ensure that site users connect to the same web server for all requests made to the application.

Which two actions should you perform? Each correct answer presents part of the solution.

Select all answers that apply.

Configure an inbound NAT rule.

**This answer is incorrect.**

Set Session persistence to **Client IP**.

**This answer is correct.**

Set Session persistence to **None**.

Set Session persistence to **Protocol**.

**This answer is correct.**

By setting Session persistence to Client IP and Protocol, you ensure that site users connect to the same web server for all requests made to the application. Setting Session persistence to None disables sticky sessions and an inbound NAT rule is used to forward traffic from a load balancer frontend to a backend pool.

[Azure Load Balancer distribution modes | Microsoft Learn](https://learn.microsoft.com/azure/load-balancer/distribution-mode-concepts)

[Introduction to Azure Load Balancer](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-load-balancer/)


You have an Azure subscription that contains an Azure DNS zone named contoso.com.

You add a new subdomain named test.contoso.com.

You plan to delegate test.contoso.com to a different DNS server.

How should you configure the domain delegation?

Select only one answer.

Add an A record for test.contoso.com.

Add an NS record set named test to the contoso.com zone.

**This answer is correct.**

Create the SOA record for test.contoso.com.

**This answer is incorrect.**

Modify the A record for contoso.com.

You must create a DNS NS record set named test in the contoso.com zone. An NS zone must be created at the apex of the zone named contoso.com. You do not need to create the SOA record set in test.contoso.com. It must only be created in contoso.com. You do not need to create or modify the DNS A record.

[Delegate a subdomain - Azure DNS | Microsoft Learn](https://learn.microsoft.com/azure/dns/delegate-subdomain)

[Host your domain on Azure DNS - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/host-domain-azure-dns/)


Your company has deployed an Azure Load Balancer to distribute traffic across multiple VMs in a web farm. Users report intermittent connection timeouts when accessing the web app.

You need to resolve the connection timeout issues and ensure even traffic distribution by the load balancer.

What should you do?

Select only one answer.

Change the distribution mode to five-tuple hash.

**This answer is correct.**

Configure a health probe for the load balancer.

**This answer is incorrect.**

Enable session persistence with source IP affinity.

Upgrade the load balancer to a higher SKU.


You are creating an Azure virtual machine that will run Windows Server.

You need to ensure that VM1 will be part of a virtual machine scale set.

Which setting should you configure during the creation of the virtual machine?

Select only one answer.

Availability options

**This answer is correct.**

Azure Spot instance

**This answer is incorrect.**

Management

Region

You must configure the virtual machine scale set from the availability options. Azure spot instance is used to add virtual machines with a discounted price. Region will not affect the configuration of the availability options. The management setting allows you to configure the monitoring and management options for the virtual machine.

[Availability options for Azure Virtual Machines - Azure Virtual Machines | Microsoft Learn](https://learn.microsoft.com/azure/virtual-machines/availability)

[Configure virtual machine availability - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-virtual-machine-availability/)




You have two Azure virtual machines named VM1 and VM2 that run Windows Server.

VM1 has a single data disk that stores backup files.  

You need to move the data disk from VM1 to VM2 as quickly as possible.

What should you do first?

Select only one answer.

Detach the data disk from VM1.

**This answer is correct.**

Restart VM1.

Stop VM1.

**This answer is incorrect.**

Stop VM2.

You can detach a disk from a running virtual machine (hot removal). You do not need to stop VM2 or restart the VM1.

[Detach a data disk from a Windows VM - Azure - Azure Virtual Machines | Microsoft Learn](https://learn.microsoft.com/azure/virtual-machines/windows/detach-disk)

[Introduction to Azure virtual machines](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-virtual-machines/)


You have an Azure virtual machine.

You receive a notification that the virtual machine is going to be affected by an underlying maintenance activity on the physical infrastructure.

You need to move the virtual machine to a different host to avoid a service interruption.

What should you do?

Select only one answer.

Apply an Azure policy.

Apply an Azure tag.

Move the virtual machine to another Azure subscription.

**This answer is incorrect.**

Redeploy the virtual machine.

**This answer is correct.**

You must redeploy the virtual machine, which can move the virtual machine to a different host. Azure will shut down the virtual machine and move the virtual machine to a new node within the Azure infrastructure.

[Redeploy Windows virtual machines in Azure - Virtual Machines | Microsoft Learn](https://learn.microsoft.com/troubleshoot/azure/virtual-machines/redeploy-to-new-node-windows)

[Introduction to Azure virtual machines](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-virtual-machines/)



You have an Azure subscription that contains an Azure Storage account named vmstorageaccount1.  

You create an Azure container instance named container1.

You need to configure persistent storage for container1.

What should you create in vmstorageaccount1?

Select only one answer.

a blob container

**This answer is incorrect.**

a file share

**This answer is correct.**

a queue

a table

An Azure container instance (Docker container) can mount Azure File Storage shares as directories and use them as persistent storage. An Azure container instance cannot mount and use as persistent storage blob containers, queues and tables.

.

[Persistent Docker volumes with Azure File Storage | Azure Blog and Updates | Microsoft Azure](https://azure.microsoft.com/blog/persistent-docker-volumes-with-azure-file-storage/)

[Configure Azure Container Instances - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-azure-container-instances/)



Your development team plans to deploy an Azure container instance. The container needs a persistent storage layer.

Which service should you use?

Select only one answer.

Azure Blob storage

**This answer is incorrect.**

Azure Files

**This answer is correct.**

Azure Queue Storage

Azure SQL Database

You can persist data for Azure Container Instances with the use of Azure Files. Azure Files offers fully managed file shares hosted in Azure Storage that are accessible via the industry standard Server Message Block (SMB) protocol.

[Mount Azure Files volume to container group - Azure Container Instances | Microsoft Learn](https://learn.microsoft.com/azure/container-instances/container-instances-volume-azure-files)

[Explore Azure Storage services - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-storage-accounts/3-explore-azure-storage-services?ns-enrollment-type=learningpath&ns-enrollment-id=learn.az-104-manage-storage)


You have an Azure subscription that contains a container app named App1. App1 is configured to use cached data.

You plan to create a new container.

You need to ensure that the new container automatically refreshes the cache used by App1.

Which type of container should you configure?

Select only one answer.

blob

**This answer is incorrect.**

init

privileged

sidecar

**This answer is correct.**

Azure Container Apps manages the details of Kubernetes and container orchestration. Containers in Azure Container Apps can use any runtime, programming language, or development stack of your choice. You can define multiple containers in a single container app to implement the sidecar pattern, for example, an agent that reads logs from the primary app container in a shared volume and forwards them to a logging service.

[Containers in Azure Container Apps | Microsoft Learn](https://learn.microsoft.com/azure/container-apps/containers)

