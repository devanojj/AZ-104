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



You have an Azure subscription that contains multiple resource groups and Azure App Service web apps. A resource group named RG1 hosts a web app named appservice1.

The App Service uses an SSL certificate.

You create a resource group named RG2.

You plan to move all the resources in RG1 to RG2.  

Which two actions should you perform? Each correct answer presents part of the solution.

Select all answers that apply.

Create a new App Service plan in RG2.

**This answer is incorrect.**

Create a new web app in RG2.

**This answer is incorrect.**

Delete the SSL certificate from RG1 and upload it to RG2.

**This answer is correct.**

Move all the resources from RG1 to RG2.




You have an Azure virtual network named VNet1.

You need to ensure that email is sent to an administrator when a virtual machine is connected to VNet1.

What two settings should you configure? Each correct answer presents part of the solution.

Select all answers that apply.

an action group

**This answer is correct.**

an alert processing rule

an alert rule

**This answer is correct.**

a mail-enabled security group

**This answer is incorrect.**

a Microsoft 365 group

The correct answers are an action group and an alert rule. An alert rule in Azure Monitor is used to detect a specific condition or event—in this case, when a virtual machine is connected to VNet1. The alert rule monitors the relevant activity or resource signal and triggers when the defined condition occurs. An action group defines what happens when the alert fires, such as sending an email notification to an administrator. Therefore, the alert rule detects the event, and the action group performs the notification action. The other options do not directly provide the mechanism to both detect the event and send the email notification.

[Monitoring Azure virtual networks | Microsoft Docs](https://docs.microsoft.com/azure/virtual-network/monitor-virtual-network)

[Introduction to Azure Monitor](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-monitor/)



You have an Azure subscription that contains a resource group named RG1. RG1 contains two virtual machines named VM1 and VM2.

You need to inspect all the network traffic from VM1 to VM2.The solution must use Azure Monitor metrics.

Which two actions should you perform? Each correct answer presents part of the solution.

Select all answers that apply.

Configure a log alert.

**This answer is incorrect.**

Configure Network In and Network Out.

Install AzureNetworkWatcherExtension.

**This answer is correct.**

Use packet capture.

**This answer is correct.**

Azure Network Watcher variable packet capture allows you to create packet capture sessions to track traffic to and from a virtual machine. Packet capture helps to diagnose network anomalies both reactively and proactively.

[Tutorial: Monitor network communication between two virtual machines using the Azure portal | Microsoft Learn](https://learn.microsoft.com/azure/network-watcher/connection-monitor)

[Introduction to Packet capture in Azure Network Watcher | Microsoft Learn](https://learn.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview)

[Introduction to Azure Network Watcher](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-network-watcher/)



You have an Azure subscription that contains 20 virtual networks and 500 virtual machines.

You deploy a new virtual machine named VM501.

You discover that VM501 is unable to communicate with a virtual machine named VM20 in the subscription. You suspect that a network security group (NSG) is the cause of the issue.

You need to identify whether an NSG is blocking communications. The solution must minimize administrative effort.

What should you use?

Select only one answer.

diagnostic logs

IP flow verify

**This answer is correct.**

virtual network flow logs

packet capture

**This answer is incorrect.**

IP flow verify lets you specify a source and destination IPv4 address, port, protocol (TCP or UDP), and traffic direction (inbound or outbound). IP flow verify can identify the specific network security group (NSG) that prevents communication. NSG flow logs is a feature of Azure Network Watcher that allows you to log information about IP traffic flowing through an NSG. Although the logs may help you identify the source of the issue, it requires much more configuration and manual evaluation. Packet capture allows you to create packet capture sessions to track traffic to and from a virtual machine. Packet capture may help narrow down the scope of the issue, but it will not identify the specific NSG that prevents communication.

[Azure Network Watcher | Microsoft Learn](https://learn.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

[Introduction to Azure Network Watcher - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-network-watcher/)



You have a Log Analytics workspace that collects data from various data sources.

You create a new Azure Monitor log query.

You plan to view data pinned as a chart to a shared dashboard.

What is the maximum number of days for which data can be shown on the shared dashboard?

Select only one answer.

14

30

**This answer is correct.**

90

**This answer is incorrect.**

180

Data shown on a shared dashboard can only be displayed for a maximum of 30 days.

[Azure Monitor workbook chart visualizations - Azure Monitor | Microsoft Learn](https://learn.microsoft.com/azure/azure-monitor/visualize/workbooks-chart-visualizations)

[Introduction to Azure Monitor](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-monitor/)


You need to create Azure alerts based on metric values and activity log events.

The solution must meet the following requirements:

- Set a limit on how many times an alert notification is sent.
- Call an Azure function when an alert is triggered.
- Configure the alert to have a severity of warning when triggered.

Which two resources should you create? Each correct answer presents part of the solution.

Select all answers that apply.

an action group

**This answer is correct.**

an alert rule

**This answer is correct.**

a notification

a secure webhook

**This answer is incorrect.**

You must create an action group to set up an action and create an alert rule to set the severity of the errors. A notification is only used to send email and you do not need to call a webhook.

[Manage action groups in the Azure portal - Azure Monitor | Microsoft Learn](https://learn.microsoft.com/azure/azure-monitor/alerts/action-groups)

You have 100 virtual machines deployed to Azure. You have Azure Monitor alerts configured for CPU and memory utilization for the virtual machines.

You open Azure Monitor alerts and discover 50 closed alerts for the virtual machines.

What can cause the alert state to be Closed?

Select only one answer.

An administrator manually changed the state of the alerts.

**This answer is correct.**

The alerts are older than 60 days.

The alert rule contains an action group that remediates the alert conditions.

**This answer is incorrect.**

The conditions that caused the alerts are no longer present.

The alert state is manually set by the user and does not have any automated logic behind it. The alert state can be either New, Acknowledged, or Closed.

[Manage Azure Monitor alerts - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-azure-alerts/2-manage-azure-monitor-alerts)


You have an Azure subscription that contains the following virtual networks:

- VNet1: Has an IP address space of 10.10.0.0/16 and contains a subnet named Subnet1 (10.10.1.0/24) that hosts a virtual machine named VM1 that runs Windows Server.
- VNet2: Has an IP address space of 10.20.0.0/16 and contains a subnet named Subnet2 (10.20.1.0/24) that hosts a virtual machine named VM2 that runs Windows Server.

VNet1 and VNet2 are connected by using virtual network peering.

Users report that VM1 cannot connect to VM2.

You need to verify whether the traffic from VM1 to the 10.20.0.0/16 subnet uses virtual network peering as the next hop.

What should you use?

Select only one answer.

Connection troubleshoot in Azure Network Watcher from VM1 to VM2

the effective routes for the network interface of VM1

**This answer is correct.**

Azure Network Watcher next hop for the network interface of VM1

the Network Controller role in VM1

**This answer is incorrect.**

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


You have an Azure subscription that contains two resource groups named RG1 and RG2.

RG1 contains the following resources:

- A virtual network named VNet1 located in the East US Azure region
- A network security group (NSG) named NSG1 located in the West US Azure region

RG2 contains the following resources:

- A virtual network named VNet2 located in the East US Azure region
- A virtual network named VNet3 located in the West US Azure region

You need to associate NSG1.

To which subnets can you associate NSG1?

Select only one answer.

the subnets of all the virtual networks

the subnets of VNet1 only

the subnets of VNet1 and VNet2

**This answer is incorrect.**

the subnets of VNet3 only

**This answer is correct.**

You can assign an NSG to the subnet of the virtual network in the same region as the NSG and NSG1 is in the West US region.

[Plan Azure virtual networks | Microsoft Learn](https://learn.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm)

[Configure network security groups - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-network-security-groups/)

You have an Azure subscription that contains an Azure DNS zone named contoso.com.

You add a new subdomain named test.contoso.com.

You plan to delegate test.contoso.com to a different DNS server.

How should you configure the domain delegation?

Select only one answer.

Add an A record for test.contoso.com.

Add an NS record set named test to the contoso.com zone.

**This answer is correct.**

Create the SOA record for test.contoso.com.

Modify the A record for contoso.com.

**This answer is incorrect.**

You must create a DNS NS record set named test in the contoso.com zone. An NS zone must be created at the apex of the zone named contoso.com. You do not need to create the SOA record set in test.contoso.com. It must only be created in contoso.com. You do not need to create or modify the DNS A record.

[Delegate a subdomain - Azure DNS | Microsoft Learn](https://learn.microsoft.com/azure/dns/delegate-subdomain)

[Host your domain on Azure DNS - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/host-domain-azure-dns/)

Your organization uses an Azure Load Balancer to manage traffic for VMs hosting a web application. Users experience uneven traffic distribution, with some VMs receiving more traffic than others.

You need to configure the load balancer to ensure even traffic distribution across all VMs in the backend pool.

What should you do?

Select only one answer.

Add more VMs to the pool.

Adjust the load balancing rule settings.

Disable session persistence.

**This answer is correct.**

Enable session persistence (source IP affinity).

**This answer is incorrect.**

Disabling session persistence ensures even traffic distribution by removing any affinity that directs traffic to the same VM. Adjusting the load balancing rule settings might seem like a solution but does not address the root cause of uneven distribution. Enabling source IP affinity maintains session persistence, potentially exacerbating the uneven distribution of traffic. Adding more VMs does not solve the distribution issue caused by session persistence settings.

[Configure Azure virtual machine network settings - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/create-windows-virtual-machine-in-azure/6-manage-vm)


You have an Azure subscription that contains a storage account named storage1. storage1 contains an Azure Files share named share1.

You need to ensure that users can authenticate to share1 by using Microsoft Entra and access the file share by using SMB.

What should you do?

Select only one answer.

Configure identity-based access.

**This answer is correct.**

Generate a shared access signature (SAS) and a connection string.

Enable public network access.

Regenerate the access keys.

**Objective:**

2.1 Configure access to storage

**What This Item Tests:**

Configure identity-based access for Azure Files

**Additional Reading:**

[Review Azure Storage security strategies - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/configure-storage-security/2-review-strategies)

Correct - Identity-based access for an Azure Storage account is a security model that uses Microsoft Entra ID or Active Directory to authorize requests to storage data, rather than relying on a static storage account key or SAS.  
Incorrect – SAS tokens and access keys provide key-based access, rather than identity-based access, and enabling public network access does not configure authentication or authorization.


ou need to create an Azure Storage account that supports the Azure Data Lake Storage Gen2 capabilities.

Which two types of storage accounts can you use? Each correct answer presents a complete solution.

Select all answers that apply.

premium block blobs

**This answer is correct.**

premium file shares

standard general-purpose v2

**This answer is correct.**

premium page blobs

**This answer is incorrect.**

To support Data Lake Storage, the storage account must support blob storage, which is available as standard general-purpose v2 and premium block blobs. Additionally, when you create the storage account, you must enable the hierarchical namespace.

[Create a storage account for Azure Data Lake Storage Gen2 - Azure Storage | Microsoft Learn](https://learn.microsoft.com/azure/storage/blobs/create-data-lake-storage-account)

[Determine storage account types - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-storage-accounts/4-determine-storage-account-kinds)


Your need to create an Azure Storage account that meets the following requirements:

- Stores data in a minimum of two availability zones
- Provides high availability

Which type of storage redundancy should you use?

Select only one answer.

geo-redundant storage (GRS)

locally-redundant storage (LRS)

read-access geo-redundant storage (RA-GRS)

**This answer is incorrect.**

zone-redundant storage (ZRS)

**This answer is correct.**

Zone-redundant storage (ZRS) replicates a storage account synchronously across three Azure availability zones in the primary region. For ensuring high availability, Microsoft recommends using ZRS in the primary region and also replicating to a secondary region.

[Data redundancy - Azure Storage | Microsoft Learn](https://learn.microsoft.com/azure/storage/common/storage-redundancy)

[Determine replication strategies - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-storage-accounts/5-determine-replication-strategies)


ou have two premium block blob Azure Storage accounts named storage1 and storage2.

You need to configure object replication from storage1 to storage2.

Which three features should be enabled before configuring object replication? Each correct answer presents part of the solution.

Select all answers that apply.

blob versioning for storage1

**This answer is correct.**

blob versioning for storage2

**This answer is correct.**

change feed for storage1

**This answer is correct.**

change feed for storage2

**This answer is incorrect.**

point-in-time restore for the containers on storage1

**This answer is incorrect.**

point-in-time restore for the containers on storage2

Object replication can be used to replicate blobs between storage accounts. Before configuring object replication, you must enable blob versioning for both storage accounts, and you must enable the change feed for the source account.

[Configure object replication - Azure Storage | Microsoft Learn](https://learn.microsoft.com/azure/storage/blobs/object-replication-configure?tabs=portal)

[Configure Azure Blob Storage - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-blob-storage/)

You create an Azure Storage account.

You need to create a lifecycle management rule to move blobs to Cool storage if the blobs have not been accessed for 30 days.

What should you do first?

Select only one answer.

Enable access tracking.

**This answer is correct.**

Enable versioning for blobs.

**This answer is incorrect.**

Refresh the blob inventory.

Rotate the storage account keys.

A lifecycle management rule can be used to move or delete blobs automatically. The rule can be based on the time the blob was last modified or the time the blob was last accessed (read or write). To perform an action based on the access time, access tracking must be enabled. This can incur additional storage costs.

[Configure a lifecycle management policy - Azure Storage | Microsoft Learn](https://learn.microsoft.com/azure/storage/blobs/lifecycle-management-policy-configure?tabs=azure-portal)

[Configure Azure Blob Storage - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-blob-storage/)


You are deploying a virtual machine by using an availability set in the East US Azure region.

You have deployed 18 virtual machines in two fault domains and 10 update domains.

Microsoft performed planned physical hardware maintenance in the East US region.

What is the maximum number of virtual machines that will be unavailable?

Select only one answer.

2

**This answer is correct.**

8

9

**This answer is incorrect.**

18

18 virtual machines are shared across 10 update domains. The first 10 virtual machines go to 10 update domains, so eight update domains will have two virtual machines. When there is physical hardware maintenance, some virtual machines will be unavailable based on their configuration. If there was a rack failure, then 18 virtual machines will be distributed to two fault domains with nine virtual machines each.

[Availability sets overview - Azure Virtual Machines | Microsoft Learn](https://learn.microsoft.com/azure/virtual-machines/availability-set-overview)

[Configure virtual machine availability - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-virtual-machine-availability/)



You plan to deploy an Azure virtual machine.

You are evaluating whether to use an Azure Spot instance.

Which two factors can cause an Azure Spot instance to be evicted? Each correct answer presents a complete solution.

Select all answers that apply.

the average CPU usages of the instance

**This answer is incorrect.**

the Azure capacity needs

**This answer is correct.**

the current price of the instance

**This answer is correct.**

the time of day

Azure Spot instances allow you to provision virtual machines at a reduced cost, but these virtual machines can be stopped by Azure when Azure needs the capacity for other pay-as-you-go workloads, or when the price of the spot instance exceeds the maximum price that you have set. These virtual machines are good for dev, testing, or for workloads that do not require any specific SLA.

[Use Azure Spot Virtual Machines - Azure Virtual Machines | Microsoft Learn](https://learn.microsoft.com/azure/virtual-machines/spot-vms)

[Configure virtual machine availability - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-virtual-machine-availability/)



ou have an Azure subscription that contains an Azure Storage account named vmstorageaccount1.  

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

n Azure container instance (Docker container) can mount Azure File Storage shares as directories and use them as persistent storage. An Azure container instance cannot mount and use as persistent storage blob containers, queues and tables.

.

[Persistent Docker volumes with Azure File Storage | Azure Blog and Updates | Microsoft Azure](https://azure.microsoft.com/blog/persistent-docker-volumes-with-azure-file-storage/)

[Configure Azure Container Instances - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-azure-container-instances/)

Your development team plans to deploy an Azure container instance. The container needs a persistent storage layer.

Which service should you use?

Select only one answer.

Azure Blob storage

Azure Files

**This answer is correct.**

Azure Queue Storage

Azure SQL Database

**This answer is incorrect.**

You can persist data for Azure Container Instances with the use of Azure Files. Azure Files offers fully managed file shares hosted in Azure Storage that are accessible via the industry standard Server Message Block (SMB) protocol.

[Mount Azure Files volume to container group - Azure Container Instances | Microsoft Learn](https://learn.microsoft.com/azure/container-instances/container-instances-volume-azure-files)

[Explore Azure Storage services - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-storage-accounts/3-explore-azure-storage-services?ns-enrollment-type=learningpath&ns-enrollment-id=learn.az-104-manage-storage)

Your company has an Azure subscription that is linked to a Microsoft Entra tenant.

You have been asked to limit the access to the Kubernetes API server.

Which two options should you choose? Each correct answer presents a complete solution.

Select all answers that apply.

API server authorized IP ranges

**This answer is correct.**

public cluster

private cluster

**This answer is correct.**

Azure tags

**This answer is incorrect.**

You can use API server authorized IP ranges if you want to maintain a public endpoint for the API server but restrict access to a set of trusted IP ranges. You can use a private cluster if you want to limit the API server to only be accessible from within your virtual network.

[Introduction to Kubernetes - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/configure-azure-kubernetes-service/)



You have an Azure subscription that contains an Azure container app named cont1.

You plan to add scaling rules to cont1.

You need to ensure that cont1 replicas are created based on received messages in Azure Service Bus.

Which scale trigger should you use?

Select only one answer.

CPU usage

**This answer is incorrect.**

event-driven

**This answer is correct.**

HTTP traffic

memory usage

Azure Container Apps allows a set of triggers to create new instances, called replicas. For Azure Service Bus, an event-driven trigger can be used to run the escalation method. The remaining scale triggers cannot use a scale rule based on messages in an Azure service bus.

[Scaling in Azure Container Apps | Microsoft Learn](https://learn.microsoft.com/azure/container-apps/scale-app#event-driven)

[Configure Azure Container Instances - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-azure-container-instances/)


You have an Azure subscription that contains an Azure App Service web app named App1.

You have the following diagnostic logging configurations:

- Application Logging (FileSystem): Error
- Application Logging (Blob): Information
- Detailed Error Message: Warning
- Web Server Logging: Verbose

You need to configure diagnostic logging to store all warnings or higher.  

Which types of diagnostic logging and severity should you enable?

Select all answers that apply.

Application Logging (Blob)

**This answer is correct.**

Application Logging (FileSystem)

**This answer is correct.**

Detailed Error Message

Verbose

Warning

**This answer is correct.**

You must enable the Application Logging (Blob) diagnostic, which can be stored for more than a week. You must also set the severity level to warning, to store warning, error, and critical log messages.

[Enable diagnostics logging - Azure App Service | Microsoft Learn](https://learn.microsoft.com/azure/app-service/troubleshoot-diagnostic-logs)

[Configure Azure App Service - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-azure-app-services/)



You have a Microsoft Entra tenant that contains a user named User1.

You need to ensure that User1 can invite external users to the tenant. The solution must follow the principle of least privilege.

Which role should you assign to User1?

Select only one answer.

Global Administrator

Groups Administrator

**This answer is incorrect.**

Guest Inviter

**This answer is correct.**

Security Administrator

The correct solution is to assign the Guest Inviter role, because it grants only the specific ability to invite external users into the Microsoft Entra tenant, aligning with the principle of least privilege. The Global Administrator role would allow full tenant-wide control and far exceeds the requirement. The Groups Administrator role allows management of groups but not external user invitations. The Security Administrator role manages security settings and reports but does not enable guest invitations. Therefore, the Guest Inviter role provides the exact permissions needed without granting unnecessary rights.

[Exercise - add guest users to directory](https://learn.microsoft.com/en-us/training/modules/implement-manage-external-identities/6-exercise-add-guest-users-to-directory)  
[Invite external users - individually and in bulk](https://learn.microsoft.com/en-us/training/modules/implement-manage-external-identities/5-invite-external-users-individually-bulk)  
[Manage external user accounts in Microsoft Entra ID](https://learn.microsoft.com/en-us/training/modules/implement-manage-external-identities/9-user-accounts-azure-active-directory)  
[Manage external collaboration](https://learn.microsoft.com/en-us/training/modules/implement-manage-external-identities/3-collaboration)  
[Exercise - configure external collaboration](https://learn.microsoft.com/en-us/training/modules/implement-manage-external-identities/4-exercise-configure-external-collaboration)


You have an Azure subscription.

You run the following command:

```
  Get-AzRoleDefinition | Format-Table -Property Name, Id
```

The command output contains data that includes the following:

```
CustomRole1   111-222-333-444-555
Owner         8e3af657-a8ff-443c-a75c-2fe8c4bcb635
Contributor   b24988ac-6180-42a0-ab88-20f7382dd24c
Reader        acdd72a7-3385-48ef-bd42-f606fba81ae7
```

You have a script that manages access to resources at the resource group level. The assignment process is automated by running the following PowerShell script nightly.

```
$rg = "RG1"
$RoleName = "111-222-333-444-555"
$Role = Get-AzRoleDefinition -Name $RoleName
New-AzRoleAssignment -SignInName user1@contoso.com
    -RoleDefinitionName $Role.Name `
    -ResourceGroupName $rg
```

User1 is unable to access the RG1 resource group. You discover that the script fails to complete for User1.

You need to modify the script to ensure that it does not fail.

What should you change in the script?

Select only one answer.

`$Role = Add-AzRoleDefinition -Name $RoleName` 

`$Role = Get-AzRoleAssignment -Name $RoleName`

`$Role = Set-AzRoleAssignment -Name $RoleName` 

**This answer is incorrect.**

`$RoleName = "CustomRole1"`

**This answer is correct.**

For the script to work as written, the $RoleName variable should refer to the name instead of the ID.

[Assign Azure roles using Azure PowerShell - Azure RBAC | Microsoft Learn](https://learn.microsoft.com/azure/role-based-access-control/role-assignments-powershell)

[Secure your Azure resources with Azure role-based access control (Azure RBAC)](https://learn.microsoft.com/training/modules/secure-azure-resources-with-rbac/)


  
You need to identify which virtual machines are underutilized.

Which Azure Advisor settings should you use?

Select only one answer.

Cost

**This answer is correct.**

High Availability

Operational Excellence

Performance

**This answer is incorrect.**

The Cost blade allows you to optimize and reduce your overall Azure spending. You can use this to identify the virtual machines that are underutilized. The Performance blade allows you to improve the speed of your applications. High availability is unavailable via Azure Advisor. Operational Excellence helps you achieve process and workflow efficiency, resource manageability, and deployment best practices.

[Introduction to Azure Advisor - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/intro-to-azure-advisor/)


You need to ensure that a user named User1 can tag all the virtual machines by using the Azure portal. The solution must follow the principle of least privilege.

What should you do?

Select only one answer.

From the Azure portal, create a custom role that has the Microsoft.Compute virtual machines/*/write permission.

From the Azure portal, modify the Access control (IAM) settings of the virtual machines.

**This answer is correct.**

From the Azure portal, modify the Policies settings of the Azure subscription.

From the command line, run the az role assignment create command.

**This answer is incorrect.**

The correct solution is to update the Access control (IAM) settings of the virtual machines in the Azure portal and assign User1 a role that grants tagging rights, such as the built-in Tag Contributor role. This follows the principle of least privilege because it gives User1 only the permissions required to apply and manage tags, without granting full write or administrative rights. Creating a custom role with full virtualMachines/*/write permission is unnecessary and too broad, modifying Policies only enforces tagging rules rather than granting permissions, and using the az role assignment create command is another way to assign roles but does not specify the least-privilege role or the portal-based method requested in the scenario.

[Apply tags with Azure portal](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources-portal)  
[Understand Azure Automation](https://learn.microsoft.com/en-us/training/modules/manage-azure-paas-resources-using-automated-methods/3-understand-azure-automation)  
[Apply tags with Azure CLI](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources-cli)  
[Apply tags with Azure PowerShell](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources-powershell)  
[Label mission-critical workloads](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-mission-critical-workload)  
[Use tagging to organize resources](https://learn.microsoft.com/en-us/training/modules/control-and-organize-with-azure-resource-manager/3-use-tagging-to-organize-resources)