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





You have an Azure subscription that contains a resource group named RG1. RG1 contains an Azure virtual machine named VM1.

You need to use VM1 as a template to create a new Azure virtual machine.

Which three methods can you use to complete the task? Each correct answer presents a complete solution.

Select all answers that apply.

From Azure Cloud Shell, run the `Get-AzVM` and `New-AzVM` cmdlets.

**This answer is incorrect.**

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



You are creating an Azure virtual machine that will run Windows Server.

You need to ensure that VM1 will be part of a virtual machine scale set.

Which setting should you configure during the creation of the virtual machine?

Select only one answer.

Availability options

**This answer is correct.**

Azure Spot instance

Management

Region

**This answer is incorrect.**

You must configure the virtual machine scale set from the availability options. Azure spot instance is used to add virtual machines with a discounted price. Region will not affect the configuration of the availability options. The management setting allows you to configure the monitoring and management options for the virtual machine.

[Availability options for Azure Virtual Machines - Azure Virtual Machines | Microsoft Learn](https://learn.microsoft.com/azure/virtual-machines/availability)

[Configure virtual machine availability - Training | Microsoft Learn](https://learn.microsoft.com/training/modules/configure-virtual-machine-availability/)