`az group create --name testazure-2 --location eastus`
Creates resource group

`az group list --output table`
Lists resource groups

`az group delete -n testazure-2`
Deletes test resource group

`Get-AzVM`
Gets the list of VM's in the subscription 

`Save-AzDeploymentScriptLog`
Save the log of a deployment script execution 
(Output)

`Save-DeploymentTemplate`
You cane save the ARM template 

`New-AzResourceGroupDeployment`
You can deploy the ARM template

``Add-AzLoadBalancerFrontendIpConfig``
Adds frontend IP configuration to the azure load balancer

`Add-AzNetworkInterfaceTapConfig`
Configures a network interface TAP
(TAP copy / monitor traffic)

`New-AzNetworkSecurityGroup`
New NSG Group

`New-AzNetworkSecurityRuleConfig`
Create security rule for NSG

`Get-AzVirtualNetworkUsageList`

`nbtstat -c`

`netstat -an`
List the ports that the server is listening on

`Test-NetConnection localhost`
Perform a ping/ICMP test.

Using `netstat -an` will . `Test-NetConnection` `Nbtstat -c` checks the NBT cache. `Get-AzVirtualNetwork` gets the virtual networks in a resource group.

