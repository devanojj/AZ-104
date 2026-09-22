You have an Azure subscription.

From PowerShell, you run the `Get-MgUser` cmdlet for a user and receive the following details:

- `Id: 8755b347-3545-3876-3987-999999999999`
- `DisplayName: Ben Smith`
- `Mail: bsmith@contoso.com`
- `UserPrincipalName: bsmith_contoso.com#EXT#@fabrikam.com`

Based upon the output of the cmdlet, which statement accurately describes the user?

Select only one answer.

The user account is disabled.

The user is a guest in the tenant.

**This answer is correct.**

The user is assigned an administrative role.

The user is deleted.

For guest users, the user principal name (UPN) will contain the email of the guest user (bsmith_contoso.com) followed by #EXT# followed by the domain name of the tenant (@fabrikam.com). Regular Microsoft Entra users appear in a format of user@fabrikam.com.

[B2B collaboration overview - Azure AD - Microsoft Entra | Microsoft Learn](https://learn.microsoft.com/azure/active-directory/external-identities/what-is-b2b)

You have an Azure subscription that contains a resource group named RG1.

You plan to create and configure a network security group (NSG) named NSG1 for the following types of traffic:

- Remote Desktop Management
- HTTP

NSG1 will be used on the subnets of multiple virtual networks.

Which two cmdlets should you run? Each correct answer presents part of the solution.

Select all answers that apply.

`Add-AzLoadBalancerFrontendIpConfig`

`Add-AzNetworkInterfaceTapConfig`

`New-AzNetworkSecurityGroup` 

**This answer is correct.**

`New-AzNetworkSecurityRuleConfig` 

**This answer is correct.**

`New-AzNetworkSecurityRuleConfig` allows you to create a rule and provide the type, protocol, direction, and port number. `New-AzNetworkSecurityGroup` creates a network security group (NSG). -`SecurityRules` specifies a list of network security rule objects to create in a NSG.

[New-AzNetworkSecurityRuleConfig (Az.Network) | Microsoft Learn](https://learn.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig?view=azps-9.2.0&viewFallbackFrom=azps-7.5.0)

[New-AzNetworkSecurityGroup (Az.Network) | Microsoft Learn](https://learn.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup?view=azps-9.2.0&viewFallbackFrom=azps-7.5.0)

[Azure network security groups overview | Microsoft Learn](https://learn.microsoft.com/azure/virtual-network/network-security-groups-overview)

[Configure network security groups - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-network-security-groups/)



ou have an Azure subscription that contains an ASP.NET application. The application is hosted on four Azure virtual machines that run Windows Server.

You have a load balancer named LB1 that load balances requests to the virtual machines.

You need to ensure that site users connect to the same web server for all requests made to the application.

Which two actions should you perform? Each correct answer presents part of the solution.

Select all answers that apply.

Configure an inbound NAT rule.

Set Session persistence to **Client IP**.

**This answer is correct.**

Set Session persistence to **None**.

Set Session persistence to **Protocol**.

**This answer is correct.**

By setting Session persistence to Client IP and Protocol, you ensure that site users connect to the same web server for all requests made to the application. Setting Session persistence to None disables sticky sessions and an inbound NAT rule is used to forward traffic from a load balancer frontend to a backend pool.

[Azure Load Balancer distribution modes | Microsoft Learn](https://learn.microsoft.com/azure/load-balancer/distribution-mode-concepts)

[Introduction to Azure Load Balancer](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-load-balancer/)


You have web app that is running in four Windows Server Azure virtual machines behind a load balancer.

Users experience issues when accessing the web app. You suspect an issue with the web server and must check whether the server is listening on port 80.

Which command should you run?

Select only one answer.

`Get-AzVirtualNetworkUsageList`

`nbtstat -c`

`netstat -an`

**This answer is correct.**

`Test-NetConnection localhost`

Using `netstat -an` will list the ports that the server is listening on. `Test-NetConnection` will perform a ping/ICMP test. `Nbtstat -c` checks the NBT cache. `Get-AzVirtualNetwork` gets the virtual networks in a resource group.

[Troubleshoot Azure Load Balancer | Microsoft Learn](https://learn.microsoft.com/azure/load-balancer/load-balancer-troubleshoot-backend-traffic)

[Introduction to Azure Load Balancer](https://learn.microsoft.com/en-us/training/modules/intro-to-azure-load-balancer/)

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

You have an Azure subscription that contains a resource group named RG1.

You have an Azure Resource Manager (ARM) template for an Azure virtual machine.

You need to use PowerShell to provision a virtual machine in RG1 by using the template.

Which PowerShell cmdlet should you run?

Select only one answer.

`New-AzManagementGroupDeployment`

`New-AzResourceGroupDeployment`

**This answer is correct.**

`New-AzSubscriptionDeployment` 

`New-AzVM`

Virtual machines are deployed to resource groups, so you must run the `New-AzResourceGroupDeployment` cmdlet. You can deploy virtual machines to subscriptions or management groups directly, therefore, `New-AzManagementGroupDeployment` and `New-AzSubscriptionDeployment` cannot be used. `New-AzVM` can be used to provision a new virtual machine, but without using a template.

[Deploy resources with PowerShell and template - Azure Resource Manager | Microsoft Learn](https://learn.microsoft.com/azure/azure-resource-manager/templates/deploy-powershell)

[Deploy Azure infrastructure by using JSON ARM templates - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/create-azure-resource-manager-template-vs-code/)

You have an Azure Resource Manager (ARM) template named deploy.json that is stored in an Azure Blob storage container.

You plan to deploy the template by running the `New-AzDeployment` cmdlet.

Which parameter should you use to reference the template?

Select only one answer.

`-Tag`

`-Templatefile`

`-TemplateSpecId`

`-TemplateUri`

**This answer is correct.**

The PowerShell deployment cmdlets can be used to deploy JSON templates that are stored locally in a resources group as a template spec, or from a web-based location. You can use the `-TemplateUri` parameter to specify a web-based location, such as GitHub or an Azure Blob Storage account. You can use `-Templatefile` to specify a local file. You can use `-TemplateSpecId` to specify a template that was save to Azure as a template spec.

[Deploy resources with PowerShell and template - Azure Resource Manager | Microsoft Learn](https://learn.microsoft.com/azure/azure-resource-manager/templates/deploy-powershell)

[Deploy Azure infrastructure by using JSON ARM templates - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/create-azure-resource-manager-template-vs-code/)

our company has a set of resources deployed to an Azure subscription. The resources are deployed to a resource group named app-grp1 by using Azure Resource Manager (ARM) templates.

You need to verify the date and the time that the resources in app-grp1 were created.

Which blade should you review for app-grp1 in the Azure portal?

Select only one answer.

Deployments

**This answer is correct.**

Diagnostics setting

Deployment stacks

Policy