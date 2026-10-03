~~You have an Azure virtual network named VNet1.~~

~~You need to ensure that email is sent to an administrator when a virtual machine is connected to VNet1.~~

~~What two settings should you configure? Each correct answer presents part of the solution.~~

~~Select all answers that apply.~~

~~an action group~~

~~**This answer is correct.**~~

~~an alert processing rule~~

~~an alert rule~~

~~**This answer is correct.**~~

~~a mail-enabled security group~~

~~**This answer is incorrect.**~~

~~a Microsoft 365 group~~

~~The correct answers are an action group and an alert rule. An alert rule in Azure Monitor is used to detect a specific condition or event—in this case, when a virtual machine is connected to VNet1. The alert rule monitors the relevant activity or resource signal and triggers when the defined condition occurs. An action group defines what happens when the alert fires, such as sending an email notification to an administrator. Therefore, the alert rule detects the event, and the action group performs the notification action. The other options do not directly provide the mechanism to both detect the event and send the email notification.~~

~~[Monitoring Azure virtual networks | Microsoft Docs](https://docs.microsoft.com/azure/virtual-network/monitor-virtual-network)~~

~~[Introduction to Azure Monitor](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-monitor/)~~





~~You have an Azure subscription that contains a resource group named RG1. RG1 contains an Azure virtual machine named VM1.~~

~~You need to use VM1 as a template to create a new Azure virtual machine.~~

~~Which three methods can you use to complete the task? Each correct answer presents a complete solution.~~

~~Select all answers that apply.~~

~~From Azure Cloud Shell, run the `Get-AzVM` and `New-AzVM` cmdlets.~~

~~**This answer is incorrect.**~~

~~From Azure Cloud Shell, run the `Save-AzDeploymentScriptLog` and `New-AzResourceGroupDeployment` cmdlets.~~

~~From Azure Cloud Shell, run the `Save-AzDeploymentTemplate` and `New-AzResourceGroupDeployment` cmdlets.~~

~~**This answer is correct.**~~

~~From RG1, select **Export template**, select **Download**, and then, from Azure Cloud Shell, run the `New-AzResourceGroupDeployment` cmdlet.~~

~~**This answer is correct.**~~

~~From VM1, select **Export template**, and then select **Deploy**.~~

~~**This answer is correct.**~~

~~From RG1, selecting the Download option from the Export template page exports the Azure Resource Manager (ARM) template from the resource group properties. You can then deploy the ARM template by running the `New-AzResourceGroupDeployment` cmdlet.~~

~~By using the `Save-AzDeploymentTemplate` cmdlet, you can save the resource ARM template. You can then deploy the ARM template by running the `New-AzResourceGroupDeployment` cmdlet.~~

~~From VM1, selecting the Deploy option from the Export template page allows you to deploy a new Azure virtual machine and use the configuration of VM1 as the template.~~

~~The `Save-AzDeploymentScriptLog` cmdlet is used to save the log of a deployment script execution.~~

~~The `Get-AzVM` cmdlet generates a list of virtual machines that are created in the Azure subscription.~~

~~[Use Azure portal to export a template - Training | Microsoft Learn](https://learn.microsoft.com/azure/azure-resource-manager/templates/export-template-portal)~~ 

~~[Export template in Azure PowerShell - Azure Resource Manager | Microsoft Learn](https://learn.microsoft.com/azure/azure-resource-manager/templates/export-template-powershell)~~



~~You are creating an Azure virtual machine that will run Windows Server.~~

~~You need to ensure that VM1 will be part of a virtual machine scale set.~~

~~Which setting should you configure during the creation of the virtual machine?~~

~~Select only one answer.~~

~~Availability options~~

~~**This answer is correct.**~~

~~Azure Spot instance~~

~~Management~~

~~Region~~

~~**This answer is incorrect.**~~

~~You must configure the virtual machine scale set from the availability options. Azure spot instance is used to add virtual machines with a discounted price. Region will not affect the configuration of the availability options. The management setting allows you to configure the monitoring and management options for the virtual machine.~~

~~[Availability options for Azure Virtual Machines - Azure Virtual Machines | Microsoft Learn](https://learn.microsoft.com/azure/virtual-machines/availability)~~

~~[Configure virtual machine availability - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-virtual-machine-availability/)~~



~~You have an on-premises network.~~

~~You have an Azure subscription that contains a virtual network named VNet1. VNet1 is connected to the on-premises network by using ExpressRoute.~~

~~You perform the following actions:~~

- ~~Create a storage account named storage1~~
- ~~Associate VNet1 to storage1 and configure network routing to use Microsoft network routing.~~

~~You need to ensure that only connections from the on-premises network are allowed to access storage1. The solution must minimize administrative effort.~~

~~What should you do?~~

~~Select only one answer.~~

~~Configure the network settings of storage1.~~

~~**This answer is correct.**~~

~~Create a routing table. Add a filter rule to the table.~~

~~Create a shared access signature (SAS).~~

~~Create an ExpressRoute circuit. Create a filter on the ExpressRoute connection.~~

~~**This answer is incorrect.**~~

~~The correct solution is to configure the network settings of the storage account, because Azure Storage allows you to restrict access by enabling firewall and virtual network rules so that only traffic from specific VNets or on-premises networks (via ExpressRoute or VPN) is allowed. This approach directly satisfies the requirement with minimal administrative effort, since it leverages built-in network settings. Creating a routing table with filter rules would not block storage access—it only influences packet routing. A SAS token controls authentication and permissions but does not restrict the network source of requests. Creating another ExpressRoute circuit and configuring filters adds unnecessary complexity when network rules on the storage account already provide the needed control.~~

~~[Secure storage endpoints](https://learn.microsoft.com/en-us/training/modules/configure-storage-accounts/7-secure-storage-endpoints)~~   
~~[Control network access to your storage account](https://learn.microsoft.com/en-us/training/modules/secure-azure-storage-account/5-control-network-access)~~




 ~~ou have an Azure subscription that contains 25 virtual machines.~~

~~You need to ensure that each virtual machine is associated to a specific department for reporting purposes.~~

~~What should you use?~~

~~Select only one answer.~~

~~administrative units~~

~~**This answer is incorrect.**~~

~~management groups~~

~~storage accounts~~

~~tags~~

~~**This answer is correct.**~~

~~Tags are metadata elements that can be applied to Azure resources. Tags can be used for tracking resources such as virtual machines and associating each resource to a department for billing and reporting purposes.~~

~~Administrative units are containers used for delegating administrative roles to manage a specific portion of Microsoft Entra. Administrative units cannot contain Azure virtual machines.~~

~~Management groups are containers that can be used to manage access, policy, and compliance across multiple Azure subscriptions.~~

~~Azure Storage accounts contain Azure Storage data objects, including blobs, file shares, queues, tables, and disks. A storage account cannot contain virtual machines.~~

~~[Tag resources, resource groups, and subscriptions for logical organization - Azure Resource Manager | Microsoft Learn](https://learn.microsoft.com/azure/azure-resource-manager/management/tag-resources?tabs=json)~~

~~[Introduction to Azure virtual machines](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-virtual-machines/)~~


~~You have an Azure subscription that contains 10 virtual machines.~~

~~You need to ensure that a user named User1 can tag all the virtual machines by using the Azure portal. The solution must follow the principle of least privilege.~~

~~What should you do?~~

~~Select only one answer.~~

~~From the Azure portal, create a custom role that has the Microsoft.Compute virtual machines/*/write permission.~~

~~From the Azure portal, modify the Access control (IAM) settings of the virtual machines.~~

~~**This answer is correct.**~~

~~From the Azure portal, modify the Policies settings of the Azure subscription.~~

~~From the command line, run the az role assignment create command.~~

~~**This answer is incorrect.**~~

~~The correct solution is to update the Access control (IAM) settings of the virtual machines in the Azure portal and assign User1 a role that grants tagging rights, such as the built-in Tag Contributor role. This follows the principle of least privilege because it gives User1 only the permissions required to apply and manage tags, without granting full write or administrative rights. Creating a custom role with full virtualMachines/*/write permission is unnecessary and too broad, modifying Policies only enforces tagging rules rather than granting permissions, and using the az role assignment create command is another way to assign roles but does not specify the least-privilege role or the portal-based method requested in the scenario.~~

~~[Apply tags with Azure portal](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources-portal)~~  
~~[Understand Azure Automation](https://learn.microsoft.com/en-us/training/modules/manage-azure-paas-resources-using-automated-methods/3-understand-azure-automation)~~  
~~[Apply tags with Azure CLI](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources-cli)~~  
~~[Apply tags with Azure PowerShell](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources-powershell)~~  
~~[Label mission-critical workloads](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-mission-critical-workload)~~  
~~[Use tagging to organize resources](https://learn.microsoft.com/en-us/training/modules/control-and-organize-with-azure-resource-manager/3-use-tagging-to-organize-resources)~~

~~You have two Azure subscriptions named Sub1 and Sub2.~~

~~Sub1 contains a virtual network named VNet1 and a VPN gateway. Sub2 contains a virtual network named VNet2.~~

~~You have an on-premises device named Device1 that runs Windows and has a Point-to-Site (P2S) VPN client installed.~~

~~You configure network peering between VNet1 and VNet2.~~

~~You need to ensure that Device1 can access VNet2 when a VPN connection is established.~~

~~What should you do?~~

~~Select only one answer.~~

~~Create a private endpoint in Sub2.~~

~~Deploy Azure Front Door to Sub2.~~

~~**This answer is incorrect.**~~

~~Download and reinstall the P2S VPN client on Device1.~~

~~**This answer is correct.**~~

~~Run the `New-SelfSignedCertificate` cmdlet on Device1.~~

~~Point-to-Site (P2S) VPN clients must be downloaded and reinstalled again after virtual network peering is successfully configured to ensure that the new routes are downloaded to the client.~~

~~A private endpoint and Azure Front Door are not required nor used to be able to access VNet2 from VNet1.~~

~~Device1 already has a digital certificate when you install the P2S VPN client, so you do not need to create new certificate manually.~~

~~[Create, change, or delete an Azure virtual network peering | Microsoft Learn](https://learn.microsoft.com/azure/virtual-network/virtual-network-manage-peering?tabs=peering-portal#requirements-and-constraints)~~


~~You have an Azure subscription that contains four virtual machines. Each virtual machine is connected to a subnet on a different virtual network.~~

~~You install the DNS Server role on a virtual machine named VM1.~~

~~You configure each virtual network to use the IP address of VM1 as the DNS server.~~  

~~You need to ensure that all four virtual machines can resolve IP addresses by using VM1.~~

~~What should you do?~~

~~Select only one answer.~~

~~Configure a DNS server on all four virtual machines.~~

~~Configure network peering.~~

~~**This answer is correct.**~~

~~Create and associate a route table to all four subnets.~~

~~**This answer is incorrect.**~~

~~Create Site-to-Site (S2S) VPNs.~~

~~By default, Azure virtual machines can communicate only with other virtual machines that are connected to the same virtual network. If you want a virtual machine to communicate with other virtual machines that are connected to other virtual networks, you must configure network peering.~~

~~A route table controls how network traffic is routed. But without network peering, network traffic is still limited to single virtual network.~~

~~Configuring a Site-to-Site (S2S) VPN is incorrect because you are not connecting on-premises virtual machines to the cloud.~~

~~[Virtual Network service endpoints](https://learn.microsoft.com/en-us/azure/virtual-network/virtual-network-service-endpoints-overview)~~

~~An organization uses a Microsoft Azure Standard Load Balancer to distribute traffic across multiple virtual machines (VMs) in a backend pool. Users report intermittent connectivity issues with applications on these VMs.~~

~~You need to troubleshoot and resolve connectivity issues.~~

~~Which three actions should you perform? Each correct answer presents part of the solution.~~

~~Select all answers that apply.~~

~~Check the health probe configuration.~~

~~**This answer is correct.**~~

~~Ensure VMs respond to the configured port.~~

~~**This answer is correct.**~~

~~Increase the timeout setting.~~

~~**This answer is incorrect.**~~

~~Modify the session persistence setting.~~

~~**This answer is incorrect.**~~

~~Restart the VMs.~~

~~Verify NSG rules allow inbound traffic.~~

~~**This answer is correct.**~~

~~To troubleshoot and resolve connectivity issues with a Microsoft Azure Standard Load Balancer, it is essential to check the health probe configuration, ensure VMs respond to the configured port, and verify that NSG rules allow inbound traffic. These actions address potential misconfigurations that could prevent traffic from reaching VMs. Modifying the session persistence setting, increasing the timeout setting, or restarting the VMs do not directly resolve connectivity issues and may introduce new limitations or misconceptions.~~

~~[Secure storage endpoints | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/configure-storage-accounts/7-secure-storage-endpoints)~~  
~~[Create network security group rules | Microsoft Learn](https://learn.microsoft.com/en-us/training/modules/configure-network-security-groups/5-create-network-security-groups-rules)~~