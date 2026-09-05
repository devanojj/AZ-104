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








You have an Azure subscription that contains a resource group named RG1. RG1 contains an Azure virtual machine named VM1.

You need to use VM1 as a template to create a new Azure virtual machine.

Which three methods can you use to complete the task? Each correct answer presents a complete solution.

Select all answers that apply.

From Azure Cloud Shell, run the `Get-AzVM` and `New-AzVM` cmdlets.

From Azure Cloud Shell, run the `Save-AzDeploymentScriptLog` and `New-AzResourceGroupDeployment` cmdlets.

From Azure Cloud Shell, run the `Save-AzDeploymentTemplate` and `New-AzResourceGroupDeployment` cmdlets.

**This answer is correct.**

From RG1, select **Export template**, select **Download**, and then, from Azure Cloud Shell, run the `New-AzResourceGroupDeployment` cmdlet.

**This answer is correct.**

From VM1, select **Export template**, and then select **Deploy**.

**This answer is correct.**

From RG1, selecting the Download option from the Export template page exports the Azure Resource Manager (ARM) template from the resource group properties. You can then deploy the ARM template by running the `New-AzResourceGroupDeployment` cmdlet.

By using the `Save-AzDeploymentTemplate` cmdlet, you can save the resource ARM template. You can then deploy the ARM template by running the `New-AzResourceGroupDeployment` cmdlet.

From VM1, selecting the Deploy option from the Export template page allows you to deploy a new Azure virtual machine and use the configuration of VM1 as the template.

The `Save-AzDeploymentScriptLog` cmdlet is used to save the log of a deployment script execution.

The `Get-AzVM` cmdlet generates a list of virtual machines that are created in the Azure subscription.

[Use Azure portal to export a template - Training | Microsoft Learn](https://learn.microsoft.com/azure/azure-resource-manager/templates/export-template-portal) 

[Export template in Azure PowerShell - Azure Resource Manager | Microsoft Learn](https://learn.microsoft.com/azure/azure-resource-manager/templates/export-template-powershell)



Your company plans to host an application on four Azure virtual machines.

You need to ensure that at least two virtual machines are available if a single Azure datacenter fails.

Which availability option should you select for the virtual machine?

Select only one answer.

an availability set

an availability zone

**This answer is correct.**

scale sets

To protect against datacenter level failures, and if you want connectivity to multiple machines, you must ensure that the virtual machines are deployed across various availability zones.

[What are Azure regions and availability zones? | Microsoft Learn](https://learn.microsoft.com/azure/reliability/availability-zones-overview)

[Configure virtual machine availability - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-virtual-machine-availability/)


You have an Azure subscription that contains an Azure container app named cont1.

You plan to add scaling rules to cont1.

You need to ensure that cont1 replicas are created based on received messages in Azure Service Bus.

Which scale trigger should you use?

Select only one answer.

CPU usage

event-driven

**This answer is correct.**

HTTP traffic

memory usage

Azure Container Apps allows a set of triggers to create new instances, called replicas. For Azure Service Bus, an event-driven trigger can be used to run the escalation method. The remaining scale triggers cannot use a scale rule based on messages in an Azure service bus.

[Scaling in Azure Container Apps | Microsoft Learn](https://learn.microsoft.com/azure/container-apps/scale-app#event-driven)

[Configure Azure Container Instances - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-azure-container-instances/)


You have an Azure virtual machine that you back up by using Azure Backup.

The backup policy sub type is Standard, and the backup policy has the following configurations:

- Backup schedule frequency: Weekly
- Retain instant recovery snapshot(s) for: 5 days
- Retention of weekly backup point: On Sunday at 8:00 AM for 12 weeks

You discover that Instant Restore is consuming more storage than expected.

You need to reduce the amount of storage consumed by Instant Restore.

What should you do first?

Select only one answer.

Change the backup schedule frequency to Daily.

Change the retention of weekly backup points to 1 week.

Modify the backup policy to reduce the retention of instant recovery snapshots.

**This answer is correct.**

Provision an additional blob storage container.

Correct – The “Retain instant recovery snapshot(s)” setting directly determines how long snapshots are stored locally before being transferred to the Recovery Services vault. Reducing this from 5 days to 2 days lowers Instant Restore storage usage.

[Azure Instant Restore Capability - Azure Backup | Microsoft Learn](https://learn.microsoft.com/azure/backup/backup-instant-restore-capability)

[Configure file and folder backups - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-file-folder-backups/)


You have an Azure subscription that contains a virtual network named VNet1.

You plan to enable VNet1 connectivity to on-premises resources by using an encrypted connection.

What should you configure for VNet1?

Select only one answer.

a private endpoint connection

a public IP address

a virtual network gateway

**This answer is correct.**

internet routing

A VPN gateway is a type of virtual network gateway that sends encrypted traffic between a virtual network and an on-premises location across a public connection. You can also use a VPN gateway to send traffic between virtual networks across the Azure backbone. A VPN gateway connection relies on the configuration of multiple resources, each of which contains configurable settings.

[Introduction to Azure VPN Gateway - Training | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-vpn-gateway/)


You have a virtual machine named VM1 that is assigned to a network security group (NSG) named NSG1.

NSG1 has the following outbound security rules:

Rule1:

- Priority: 900
- Name: BlockInternet
- Port: 80
- Protocol: TCP
- Source: Any
- Destination: Any
- Action: Block

Rule2:

- Priority: 1000
- Name: AllowInternet
- Port: 80
- Protocol: TCP
- Source: Any
- Destination: Any
- Action: Allow

You need to ensure that internet access to VM1 on port 80 is allowed.

What should you do?

Select only one answer.

Change the action of Rule2.

Change the name of Rule1.

Change the priority of Rule2.

**This answer is correct.**

Change the source in Rule2.

Rule1 has higher priority, so the action will be blocked. You can increase the priority of Rule2, decrease the priority of Rule1, or change the action of Rule1 to achieve the goal.

[Azure network security groups overview | Microsoft Learn](https://learn.microsoft.com/azure/virtual-network/network-security-groups-overview)

[Configure network se](https://learn.microsoft.com/training/modules/configure-network-security-groups/)

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


You have an Azure subscription.

You plan to implement four Azure virtual networks that will be peered. All virtual machines will use a DNS suffix of contoso.com.

You need to configure name resolution for the virtual networks to ensure that all the virtual machines can communicate by using their FQDNs. The solution must minimize administrative effort.

What should you use?

Select only one answer.

a DNS server on an Azure virtual machine

an Azure Private DNS zone

**This answer is correct.**

an Azure public DNS zone

Azure-provided name resolution

Azure Private DNS allows for private name resolution between Azure virtual networks. Azure public DNS provides DNS for public access, such as name resolution for a publicly accessible website. Azure-provided name resolution does not support user-defined domain names and only supports a single virtual network. A DNS server on a virtual machine can also be used to achieve the goal but involves much more administrative effort to implement and maintain than using Azure Private DNS.

[Name resolution for resources in Azure virtual networks | Microsoft Learn](https://learn.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#azure-provided-name-resolution)

[Host your domain on Azure DNS - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/host-domain-azure-dns/)


You have deployed a web application in Microsoft Azure using a public Microsoft Load Balancer to distribute traffic across virtual machines. Users report intermittent connectivity issues.

You need to troubleshoot the connectivity issues for consistent application access.

Each correct answer presents part of the solution. Which two actions should you take?

Select all answers that apply.

Change the load balancer's distribution mode to Source IP affinity.

Check the health probe configuration.

**This answer is correct.**

Check the network security group rules for the virtual machines.

Verify matching SKUs for the load balancer and public IP.

**This answer is correct.**

Checking the health probe configuration is crucial because an inactive or incorrectly configured probe can lead to traffic being routed to unhealthy instances, causing connectivity issues. Verifying matching SKUs for the load balancer and public IP is also essential, as mismatched SKUs can disrupt proper operation and lead to connectivity problems. Checking the network security group rules might seem relevant but does not address the root cause of the connectivity issues. Changing the load balancer's distribution mode might seem like it could improve session persistence but does not resolve the underlying configuration problems causing the connectivity issues.

[Improve application scalability and resiliency by using Azure Load Balancer](https://learn.microsoft.com/en-us/training/modules/improve-app-scalability-resiliency-with-load-balancer)

You have an Azure subscription that contains a storage account named storage1.

You need to provide a partner organization with access to storage1. Access to storage1 must automatically expire after 24 hours.

What should you configure?

Select only one answer.

a shared access signature (SAS)

**This answer is correct.**

an access key

Azure Content Delivery Network (CDN)

lifecycle management

A SAS provides secure delegated access to resources in a storage account. With a SAS, you have granular control over how a client can access data, including time restrictions.

Access keys and Azure CDN provide permanent access to resources. They will require manual steps to remove access. Lifecycle management is not needed.

[Configure Azure Storage security - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-storage-security/)

[Grant limited access to data with shared access signatures (SAS) - Azure Storage | Microsoft Learn](https://learn.microsoft.com/azure/storage/common/storage-sas-overview)

ou have an Azure subscription that contains a storage account named storage1.

You need to ensure that access to storage1 is prevented from the internet.

What should you configure on storage1?

Select only one answer.

Access keys

Data protection

Encryption

Networking

**This answer is correct.**

The Networking node of a storage account provides settings to configure public network access and network routing. To disable public network access, you can disable public network access, or configure the access to only allow specific virtual networks and IP addresses.

[Configure Azure Storage security - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-storage-security/)

[Configure Azure Storage firewalls and virtual networks | Microsoft Learn](https://learn.microsoft.com/azure/storage/common/storage-network-security?tabs=azure-portal)

ou have an on-premises network.

You have an Azure subscription that contains a virtual network named VNet1. VNet1 is connected to the on-premises network by using ExpressRoute.

You perform the following actions:

- Create a storage account named storage1
- Associate VNet1 to storage1 and configure network routing to use Microsoft network routing.

You need to ensure that only connections from the on-premises network are allowed to access storage1. The solution must minimize administrative effort.

What should you do?

Select only one answer.

Configure the network settings of storage1.

**This answer is correct.**

Create a routing table. Add a filter rule to the table.

Create a shared access signature (SAS).

Create an ExpressRoute circuit. Create a filter on the ExpressRoute connection.

The correct solution is to configure the network settings of the storage account, because Azure Storage allows you to restrict access by enabling firewall and virtual network rules so that only traffic from specific VNets or on-premises networks (via ExpressRoute or VPN) is allowed. This approach directly satisfies the requirement with minimal administrative effort, since it leverages built-in network settings. Creating a routing table with filter rules would not block storage access—it only influences packet routing. A SAS token controls authentication and permissions but does not restrict the network source of requests. Creating another ExpressRoute circuit and configuring filters adds unnecessary complexity when network rules on the storage account already provide the needed control.

[Secure storage endpoints](https://learn.microsoft.com/en-us/training/modules/configure-storage-accounts/7-secure-storage-endpoints)   
[Control network access to your storage account](https://learn.microsoft.com/en-us/training/modules/secure-azure-storage-account/5-control-network-access)


You plan to configure object replication between two Azure Storage accounts.

The Blob service of the source storage account has the following settings:

- Hierarchical namespace: Disabled
- Default access tier: Hot
- Blob public access: Enabled
- Blob soft delete: Enabled (7 days)
- Container soft delete: Enabled (7 days)
- Versioning: Disabled
- Change feed: Enabled
- NFS v3: Disabled
- Allow cross-tenant replication: Enabled

Which setting should be modified on the source storage account to support object replication?

Select only one answer.

Blob soft delete

Change feed

Hierarchical namespace

Versioning

**This answer is correct.**

Versioning must be enabled for both the source and destination accounts. In this scenario, versioning is currently disabled.

[Object replication overview - Azure Storage | Microsoft Learn](https://learn.microsoft.com/azure/storage/blobs/object-replication-overview)

[Configure Azure Blob Storage - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-blob-storage/)


You have an Azure Resource Manager (ARM) template named Template1 that is used to deploy Azure virtual machines.

Template1 contains the following text. 

"resources": [  
  {  
    "type": "Microsoft.Compute/virtualMachines",  
    "apiVersion": "2025-04-01",  
    "name": "[parameters('vmName')]",  
    "location": "[resourceGroup().location]",  
    "properties": {  
      &lt;text removed&gt;  
    }  
  }  
]

You need to deploy two Azure virtual machines by using Template1.

What should you add to Template1?

Select only one answer.

a copy element

**This answer is correct.**

the API version

the Azure subscription ID

the resource group location

The correct solution is to add a copy element, because ARM templates use the copy property to deploy multiple instances of a resource, such as two virtual machines, in a single deployment. The API version is already specified in the template and does not control the number of resources deployed. The subscription ID is never hardcoded in ARM templates since deployments are scoped to a subscription, and the resource group location is already provided through "[resourceGroup().location]". Therefore, only the copy element enables the template to create two virtual machines from a single resource definition.

[Add flexibility to your Azure Resource Manager template by using template functions](https://learn.microsoft.com/en-us/training/modules/modify-azure-resource-manager-template-reuse/2-azure-resource-manager-functions)  
[Examine Azure Resource Manager templates](https://learn.microsoft.com/en-us/training/modules/explore-azure-governance-manageability/3-examine-azure-resource-manager-templates)  
[Azure Resource Manager documentation](https://learn.microsoft.com/en-us/training/modules/arm-template-whatif/2-deployment-modes)

our company plans to host an application on four Azure virtual machines.

You need to ensure that at least two virtual machines are available if a single Azure datacenter fails.

Which availability option should you select for the virtual machine?

Select only one answer.

an availability set

an availability zone

**This answer is correct.**

scale sets

To protect against datacenter level failures, and if you want connectivity to multiple machines, you must ensure that the virtual machines are deployed across various availability zones.

[What are Azure regions and availability zones? | Microsoft Learn](https://learn.microsoft.com/azure/reliability/availability-zones-overview)

[Configure virtual machine availability - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-virtual-machine-availability/)