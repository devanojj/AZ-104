You have the following resource groups, management groups, and Azure subscriptions:

- Two resource groups named RG1 and RG2 in a subscription named Sub1 and a management group named MG1.
- Two resource groups named RG3 and RG4 in a subscription named Sub2 and a management group named MG1.
- Two resource groups named RG5 and RG6 in a subscription named Sub3 and a management group named MG1.
- Two resource group named RG10 and RG11 in a subscription named Sub4 and a management group named MG2.
- Two resource group named RG11 and RG12 in a subscription named Sub5 and a management group named MG2.

You need to assign a role to a user to ensure the user can view all the resources in the subscriptions. The solution must use the principle of least privilege.

Which role should you assign?

Select only one answer.

the Billing Reader role for all the subscriptions

the Billing Reader role for MG1 and MG2

the Contributor role for MG1 and MG2

the Reader role for MG1 and MG2

**This answer is correct.**


You have an Azure subscription that contains a resource group named RG1. RG1 contains a virtual machine that runs daily reports.

You need to ensure that the virtual machine shuts down when resource group costs exceed 75 percent of the allocated budget.

Which two actions should you perform? Each correct answer presents part of the solution.

Select all answers that apply.

Create an action group of type Runbook, and then select **Scale Up VM**.

Create an action group of type Runbook, and then select **Stop VM** as an action.

**This answer is correct.**

From Cost Management + Billing, create a new cost analysis.

From Cost Management + Billing, modify the Budgets settings.

**This answer is correct.**

You must go to Cost Management + Billing, and then Budgets to edit the budget associated with the resource group resources. You must also create a new action group of the Runbook type, and then choose Stop VM as an action. The cost analysis will not stop the virtual machine from running and the Scale Up VM action group is not required.

[Tutorial - Create and manage Azure budgets - Microsoft Cost Management | Microsoft Learn](https://learn.microsoft.com/azure/cost-management-billing/costs/tutorial-acm-create-budgets)