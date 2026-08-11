1. 

What is the purpose of Azure Policy and how does it help in managing resources?

Azure Policy is used to manually configure resources on a case-by-case basis.

Azure Policy assesses compliance at scale and enforces organizational and regulatory standards across Azure environments.

Correct. Azure Policy assesses compliance at scale and enforces organizational and regulatory standards across Azure environments. Many built-in policies are included, and you can create custom policies. Examples of what you can achieve with Azure Policy include allowing only certain virtual machine sizes for your project, recommending system updates on servers, and allowing multifactor authentication for subscription accounts.

Azure Policy is a networking tool that manages traffic between virtual networks.

Azure Policy is solely for encrypting data at rest in Azure.

2. 

What are the two recommended steps to deploy policies safely in an existing environment?

Start from assignments with Enforcement Mode Disabled and then deploy policies in deployment rings.

Correct. Starting with Enforcement Mode Disabled and deploying policies in deployment rings ensures a controlled and safe approach to managing policy resources in your existing environment.

Start from assignments with Enforcement Mode Enabled and then deploy policies in deployment rings.

Incorrect. This approach would be risky because activating Enforcement Mode would enforce policies across the entire environment at once. Starting with Enforcement Mode Enabled could lead to disruptions or compliance violations without a gradual validation process.

Start from assignments with Enforcement Mode Disabled and then deploy policies in parallel.

Start from assignments with Enforcement Mode Enabled and then deploy policies in parallel.

3. 

What is the purpose of the Enforcement Mode in Azure Policy?

It prevents resource evaluation from happening at all.

It triggers entries in the Azure Activity log.

Incorrect. When Enforcement Mode is set to 'DoNotEnforce', it doesn't trigger entries in the Azure Activity log.

It allows testing the policy's outcome on existing resources without initiating the policy effect.

Correct. Enforcement Mode lets you deactivate the enforcement of certain policy effects for testing purposes.

4. 

What is the role of Azure Resource Manager in the governance of Azure applications and resources?

It's the deployment and management service for Azure, providing a management layer that allows creation, update, and deletion of resources.

Correct. Azure Resource Manager is the deployment and management service for Azure, allowing control over resources.

It's a unit of management, billing, and scale in Azure.

It provides a level of scope above subscriptions.

5. 

At what levels can Azure Policies be assigned?

Management Group

Management Group and Subscription

Management Group, Subscription, and Resource Group

Correct. Azure Policies can be assigned at the Management Group, Subscription, and Resource Group levels.

Subscription and Resource Group

Incorrect. Azure Policies can be assigned at more levels than just Subscription and Resource Group.