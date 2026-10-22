Practice questions for [[Compute Deep Dive]]. Sections mirror that note so you can jump back to whatever you miss.

---
## 1. ARM Templates & Bicep

**Which three elements are required in every ARM template?**
- parameters, variables and resources
- $schema, parameters and outputs
- *$schema, contentVersion and resources (correct)*

**The person deploying a template must supply the environment name at deployment time. In which template section should you define it?**
- *parameters (correct)*
- variables
- outputs

**After deployment, a pipeline needs the fully qualified domain name of the web app that the template created. Which template section returns it?**
- *outputs (correct)*
- variables
- functions

**A template parameter for a storage SKU must only accept Standard_LRS, Standard_GRS or Standard_ZRS. Which property should you add in the ARM template?**
- *allowedValues (correct)*
- defaultValue
- metadata

**A template parameter holds an administrator password that must not be logged or displayed. Which parameter type should you use?**
- string with a defaultValue
- object
- *securestring (correct)*

**A template sets a storage account name to `[concat('store', uniqueString(resourceGroup().id))]`. What happens when the template is redeployed to the same resource group?**
- *The same name is generated, because uniqueString returns a deterministic hash of its input (correct)*
- A new random name is generated, creating a second storage account
- The deployment fails because the name already exists

**Which expression deploys a resource to the same region as the resource group it's deployed into?**
- `[parameters('location')]`
- *`[resourceGroup().location]` (correct)*
- `[subscription().location]`

**You need an ARM template to create three identical network interfaces from a single resource definition. What should you use?**
- dependsOn
- A condition
- *The copy element (correct)*

**In an ARM JSON template, a virtual machine must not be created until its network interface exists. What should you add to the VM resource?**
- A condition element
- *A dependsOn entry referencing the network interface (correct)*
- An output referencing the NIC

**In Bicep, a virtual machine references `nic.id`, where `nic` is the symbolic name of a network interface in the same file. Do you need to add a dependsOn?**
- *No, Bicep creates the dependency implicitly when a resource references another's symbolic name (correct)*
- Yes, Bicep always requires explicit dependencies
- Yes, but only for resources in different resource groups

**Which Bicep decorator prevents a parameter value from being logged or displayed?**
- @allowed()
- @description()
- *@secure() (correct)*

**Which Bicep syntax deploys three instances of a resource?**
- A copy element inside the resource
- *`[for i in range(0, 3): {...}]` (correct)*
- `resource x '...' = if (count == 3) {...}`

**A Bicep file must create a resource group. What must the file declare, and which command deploys it?**
- `targetScope = 'resourceGroup'` and `az deployment group create`
- `targetScope = 'tenant'` and `az deployment tenant create`
- *`targetScope = 'subscription'` and `az deployment sub create` (correct)*

**What is the default deployment mode for an ARM or Bicep deployment to a resource group?**
- Complete
- *Incremental (correct)*
- Validation

**You deploy a template in Complete mode to a resource group that contains a storage account not defined in the template. What happens to the storage account?**
- It's left unchanged
- *It's deleted (correct)*
- It's added to the template's outputs

**You need to see which resources a template will create, modify or delete before running it, without making any changes. What should you use?**
- Complete mode with --no-wait
- az bicep build
- *The what-if operation (correct)*

**Which command converts an existing ARM JSON template to a Bicep file?**
- *az bicep decompile --file main.json (correct)*
- az bicep build --file main.json
- az group export --name myRG

**Which command converts a Bicep file into an ARM JSON template?**
- az bicep decompile --file main.bicep
- *az bicep build --file main.bicep (correct)*
- az deployment group validate

**You need the exact template and parameter values that were used in a deployment last week, ready to redeploy unchanged. Which export option should you use?**
- Export template from the resource group
- Export template from each individual resource
- *Export the template from the deployment history (correct)*

**Resources were created manually in the portal and later modified by hand. You need a template that reflects their current state. What should you do?**
- Export the template from the deployment history
- Run what-if against the resource group
- *Export the template from the resource group (correct)*

**Where can you export existing resources directly as a Bicep file?**
- The az group export command
- The Export-AzResourceGroup cmdlet
- *The Azure portal's Export template blade (correct)*

**A parameter file must supply a VM administrator password without storing it in plain text. What should the parameter file use?**
- A securestring default value in the template
- A variable that reads an environment variable
- *A Key Vault reference to a secret (correct)*

**Which Azure PowerShell cmdlet deploys a template to a resource group?**
- *New-AzResourceGroupDeployment (correct)*
- New-AzDeployment
- New-AzTemplateSpec

**What does it mean that ARM template deployments are idempotent?**
- *Deploying the same template repeatedly results in the same resources in the same state (correct)*
- Each deployment creates new copies of all resources
- Templates can only be deployed once to a resource group

**Your organisation wants to store approved templates in Azure and let teams deploy them using RBAC permissions. What should you use?**
- A storage account container with a SAS token
- *Template specs (correct)*
- Azure Policy definitions

---
## 2. Creating & Managing Virtual Machines

**A VM was shut down from inside the guest operating system and shows as Stopped. Is compute still billed?**
- No, compute billing stops whenever the VM isn't running
- *Yes, the VM must be Stopped (deallocated) to stop compute charges (correct)*
- No, but the OS disk is billed at a higher rate

**Which data is lost when a VM is deallocated?**
- Data on attached data disks
- *Data on the temporary disk (correct)*
- Data on the OS disk

**A VM is unresponsive because of a suspected problem with the physical host. What should you do to move it to a new host?**
- Reapply the VM
- *Redeploy the VM (correct)*
- Reset the VM password

**You need to install software automatically on a VM immediately after deployment. Which extension should you use?**
- *Custom Script Extension (correct)*
- Network Watcher Agent
- Azure Monitor Agent

**You need to run a PowerShell script on a VM that has no RDP access and no public IP. What should you use?**
- Boot diagnostics
- Serial console logs
- *Run Command (correct)*

**What happens when you resize a running VM to a size that's available on its current hardware cluster?**
- The resize happens with no interruption
- *The VM restarts (correct)*
- The VM is deallocated and must be started manually

**The VM size you want doesn't appear in the list of available sizes for a running VM. What should you do?**
- Delete the VM and recreate it with the new size
- *Deallocate the VM, then resize it (correct)*
- Request a quota increase for the current size

**A VM in an availability set must be resized, but the new size isn't available on the cluster hosting the set. What might you need to do?**
- *Deallocate all VMs in the availability set, then resize (correct)*
- Move the VM out of the availability set first
- Change the availability set's fault domain count

**What does the letter s indicate in the VM size Standard_D4s_v5?**
- *The size supports Premium SSD storage (correct)*
- The size has a local temporary SSD
- The size uses AMD processors

**Which VM series is designed for workloads that run at low CPU most of the time with occasional spikes?**
- *B-series (correct)*
- F-series
- E-series

**An in-memory database such as SAP HANA needs a very high memory-to-CPU ratio. Which VM series should you choose?**
- *E-series or M-series (correct)*
- F-series
- L-series

**A batch-processing workload needs a high CPU-to-memory ratio. Which VM series fits best?**
- E-series
- *F-series (correct)*
- L-series

**Your organisation has Windows Server licences with Software Assurance and wants to reduce VM costs. What should you use?**
- Azure Spot VMs
- Reserved instances
- *Azure Hybrid Benefit (correct)*

**An interruptible batch job must run at the lowest possible cost and can tolerate being stopped at any time. What should you use?**
- B-series VMs
- Azure Hybrid Benefit
- *Azure Spot VMs (correct)*

**A deployment of several D-series VMs fails with a quota error. What should you do?**
- Deploy the VMs into a different resource group
- Change the VMs to a different image
- *Request a vCPU quota increase for that VM family in the region (correct)*

**A VM won't boot and you need to see a screenshot of its console. What should be enabled?**
- Azure Monitor Agent
- Run Command
- *Boot diagnostics (correct)*

---
## 3. VM Disks & Encryption

**Which managed disk type can't be used as an OS disk?**
- Premium SSD
- *Ultra Disk (correct)*
- Standard SSD

**A database data disk requires 150,000 IOPS. Which disk type should you use?**
- Premium SSD
- Premium SSD v2
- *Ultra Disk (correct)*

**A production data disk needs sub-millisecond latency, and you want to set its capacity, IOPS and throughput independently. Which disk type should you choose?**
- Premium SSD
- Standard SSD
- *Premium SSD v2 (correct)*

**Which disk type is most cost-effective for web servers and dev/test VMs that need consistent but moderate performance?**
- *Standard SSD (correct)*
- Ultra Disk
- Premium SSD v2

**You need to increase the size of a VM's OS disk. What must you do first?**
- Detach the OS disk from the VM
- Take a full snapshot of the disk
- *Deallocate the VM (correct)*

**You need to expand a 1 TiB Premium SSD data disk to 2 TiB on a running VM. What's required?**
- Deallocate the VM before expanding
- *Nothing; data disks can be expanded without downtime in this case (correct)*
- Detach the disk before expanding

**You need to reduce the size of a managed data disk from 512 GiB to 256 GiB. What should you do?**
- *Create a new smaller disk and copy the data, because shrinking a managed disk isn't supported (correct)*
- Deallocate the VM and change the disk size
- Change the performance tier to a smaller size

**After you expand a data disk in Azure, Windows still shows the old volume size. What should you do?**
- Detach and reattach the disk
- *Extend the volume inside the operating system (correct)*
- Change the disk's caching setting

**You need to convert a VM's OS disk from Standard HDD to Premium SSD. What must you do?**
- Take a snapshot and create a new Ultra Disk
- Change the disk type while the VM is running
- *Deallocate the VM, change the disk type, and make sure the VM size supports Premium storage (correct)*

**How many times per day can you change the type of a managed disk?**
- Once
- Unlimited
- *Twice (correct)*

**A Premium SSD needs more IOPS temporarily, and the VM can't be restarted. What should you do?**
- Convert the disk to Ultra Disk
- *Change the disk's performance tier (correct)*
- Change the disk caching to ReadWrite

**You want disk snapshots that are billed only for the data that changed since the previous snapshot. What should you use?**
- *Incremental snapshots (correct)*
- Full snapshots
- Azure Backup instant restore

**A Windows Server Failover Cluster needs one managed disk attached to two VMs at the same time. What should you configure?**
- Ultra Disk with ReadWrite caching
- *A shared disk with maxShares set to 2 (correct)*
- Two disks with zone-redundant storage

**Which statement describes server-side encryption (SSE) for managed disks?**
- It must be enabled manually and encrypts the temporary disk
- It runs inside the guest OS using BitLocker or DM-Crypt
- *It's always on, encrypts disks at rest in the storage cluster, but doesn't encrypt the temp disk or disk caches (correct)*

**A compliance requirement says the temporary disk and the OS and data disk caches must be encrypted, without using the VM's CPU. What should you enable?**
- *Encryption at host (correct)*
- Azure Disk Encryption
- Server-side encryption with customer-managed keys

**You try to enable encryption at host on a new VM, but the option is unavailable. What should you do first?**
- *Register the EncryptionAtHost feature for the Microsoft.Compute resource provider on the subscription (correct)*
- Enable Azure Disk Encryption on the subscription
- Create a disk encryption set

**You need to enable encryption at host on an existing running VM. What must you do?**
- *Deallocate the VM, enable encryption at host, then start it (correct)*
- Restart the VM from inside the guest OS
- Recreate the VM from a snapshot

**A VM previously had Azure Disk Encryption enabled. You need to use encryption at host instead. What's required?**
- *Create new disks and a new VM, because encryption at host can't be enabled on a VM that has ever used ADE (correct)*
- Disable ADE, then enable encryption at host on the same disks
- Enable both features together during a maintenance window

**Your security team must control the keys used to encrypt managed disks. What should you configure?**
- Platform-managed keys with key rotation
- Azure Disk Encryption with a BitLocker recovery key
- *A disk encryption set that references a key in Azure Key Vault (customer-managed keys) (correct)*

**Which Key Vault setting is mandatory when you use it for customer-managed disk encryption keys?**
- *Purge protection (correct)*
- RBAC authorisation
- A private endpoint

**What is the status of Azure Disk Encryption (ADE)?**
- It's the recommended encryption for new VMs
- *It retires on 15 September 2028; Microsoft recommends encryption at host for new VMs (correct)*
- It was retired in 2025

**How does Azure Disk Encryption encrypt a VM's disks?**
- *Inside the guest OS using BitLocker (Windows) or DM-Crypt (Linux) (correct)*
- At the storage cluster, with no impact on the VM
- At the VM host, without using the VM's CPU

**What is the status of unmanaged disks (VHD page blobs in storage accounts attached to VMs)?**
- Supported but not recommended
- *Fully retired on 31 March 2026; VMs must use managed disks (correct)*
- Retiring in 2028

---
## 4. Moving VMs

**What happens when you move a running VM to a different resource group in the same subscription?**
- *The VM keeps running, but its resource ID changes (correct)*
- The VM is deallocated during the move
- The VM is redeployed to a new host

**A move of a VM to another subscription fails with MissingMoveDependentResources. What should you do?**
- *Include the dependent resources, such as disks, NIC, VNet, public IP and NSG, in the same move (correct)*
- Deallocate the VM and retry
- Move the VM first, then its disks

**You need to move a VM to a subscription in a different Microsoft Entra tenant. What should you tell the team?**
- Move it from the portal with the cross-tenant option
- Move it with Azure Resource Mover
- *Resource moves require both subscriptions to be in the same tenant; you must redeploy instead (correct)*

**You try to move one VM out of an availability set to another resource group by itself. What happens?**
- It succeeds and the VM leaves the availability set
- *It isn't supported; VMs in an availability set can't be moved individually (correct)*
- It succeeds only if the VM is deallocated

**You need to move a VM from East US to West Europe. What should you use?**
- A resource move to a resource group in West Europe
- An ARM template export with a new location parameter only
- *Azure Resource Mover or Azure Site Recovery (correct)*

**After moving a VM to a new subscription, users who had Contributor on the VM lose access. Why?**
- *Role assignments don't move with the resource and must be recreated (correct)*
- Moves always reset the VM's local accounts
- RBAC is disabled during the 4-hour move lock

**A resource move fails immediately even though all dependencies are included. Which lock is the most likely cause?**
- A CanNotDelete lock on the VM
- *A ReadOnly lock on the source or destination resource group or subscription (correct)*
- A tag policy on the destination

**A VM with a Standard SKU public IP must move to another subscription. What should you do first?**
- *Disassociate the Standard public IP from the VM (correct)*
- Upgrade the public IP to the Global tier
- Convert the public IP to a public IP prefix

**You need to move a resource group and everything in it to another subscription. What should you do?**
- *Move the resources into a resource group in the target subscription, because a resource group itself can't be moved (correct)*
- Move the resource group object to the new subscription
- Use Azure Resource Mover to move the resource group

**While a VM is being moved between resource groups, an administrator tries to resize it. What happens?**
- *The operation fails, because the source and target resource groups are locked during the move (correct)*
- The resize is queued and applied after the move
- The move is cancelled

---
## 5. Availability Sets, Zones & Scale Sets

**What is the maximum number of fault domains in an availability set?**
- *3 (correct)*
- 2
- 5

**What is the maximum number of update domains in an availability set?**
- *20 (correct)*
- 5
- 3

**What is the default number of update domains in an availability set?**
- 3
- 20
- *5 (correct)*

**What does a fault domain represent?**
- A group of VMs rebooted together during planned maintenance
- A separate datacentre within a region
- *A group of hardware that shares a common power source and network switch (correct)*

**What does an update domain represent?**
- *A group of VMs that can be rebooted together during planned maintenance (correct)*
- Hardware that shares a power source and network switch
- A separate region

**You need to add an existing VM to an availability set. What must you do?**
- Stop the VM and change its availability setting
- *Delete the VM and recreate it in the availability set from its existing disks (correct)*
- Use Azure Resource Mover

**What SLA applies to two or more VMs in the same availability set?**
- *99.95% (correct)*
- 99.9%
- 99.99%

**What SLA applies to two or more VMs deployed across availability zones?**
- 99.9%
- *99.99% (correct)*
- 99.95%

**A single VM uses Standard SSD managed disks. What is its SLA?**
- 99.9%
- *99.5% (correct)*
- 95%

**An application must survive the loss of an entire datacentre within a region. What should you use?**
- *VMs deployed across availability zones (correct)*
- An availability set with three fault domains
- A proximity placement group

**A region doesn't support availability zones. You need protection against a rack or power failure for two VMs. What should you use?**
- A zone-redundant scale set
- A proximity placement group
- *An availability set (correct)*

**A scale set must run a mix of VM sizes and combine Spot with on-demand instances. Which orchestration mode should you use?**
- Uniform
- Either mode; both support mixed sizes
- *Flexible (correct)*

**A scale set was created with Uniform orchestration. You now need Flexible orchestration. What should you do?**
- *Create a new scale set with Flexible orchestration, because the mode can't be changed after creation (correct)*
- Change the orchestration mode in the scale set's configuration
- Update the upgrade policy to Rolling

**What is the maximum number of instances in a single Virtual Machine Scale Set?**
- *1,000 (correct)*
- 100
- 500

**What is the default upgrade policy for a Virtual Machine Scale Set?**
- Automatic
- Rolling
- *Manual (correct)*

**You want scale set instances updated in batches while the application stays available. What must be in place for a Rolling upgrade policy?**
- *The Application Health extension or a load balancer health probe (correct)*
- At least two availability sets
- Automatic OS upgrades

**What is the risk of the Automatic upgrade policy?**
- Instances are never updated until you trigger it
- It only updates instances in one availability zone
- *The scale set might take down all instances at the same time to upgrade them (correct)*

**When the scale set scales in, the oldest instances must be removed first. Which scale-in policy should you configure?**
- Default
- *OldestVM (correct)*
- NewestVM

**One scale set instance is running a long job and must not be removed by autoscale. What should you configure?**
- *Instance protection: protect from scale-in (correct)*
- A CanNotDelete resource lock on the scale set
- The NewestVM scale-in policy

**One scale set instance must also be excluded from upgrades and reimage operations started on the scale set. Which protection should you apply?**
- *Protect from scale set actions (correct)*
- Protect from scale-in
- A ReadOnly lock on the instance's disk

**Autoscale keeps adding an instance and then removing it minutes later. What should you do?**
- Remove the scale-in rule
- *Widen the gap between the scale-out and scale-in thresholds and lengthen the cool down (correct)*
- Change the scale-in policy to OldestVM

**When does autoscale use the default instance count?**
- When the scale set is first created
- Every night at midnight UTC
- *When metrics aren't available to evaluate the rules (correct)*

**A web tier must run 10 instances during business hours on weekdays and 2 at other times. What should you configure?**
- *A schedule-based autoscale profile (correct)*
- A metric rule based on CPU percentage
- Instance protection on 8 instances

**VMs in a new Flexible scale set can't reach the internet. What is the most likely cause, and fix?**
- The scale set needs automatic OS upgrades enabled
- The upgrade policy must be Rolling
- *Flexible scale sets don't get default outbound access; add an explicit method such as a NAT gateway (correct)*

---
## 6. Containers

**Which Azure Container Registry tier supports geo-replication?**
- Basic
- *Premium (correct)*
- Standard

**Which Azure Container Registry tier is required to use private endpoints?**
- Basic
- *Premium (correct)*
- Standard

**What is the lowest Azure Container Registry tier that supports anonymous pull access?**
- Basic
- Premium
- *Standard (correct)*

**An App Service app must pull images from ACR without storing any credentials. What should you configure?**
- The ACR admin user with its password in an app setting
- A repository-scoped token
- *A managed identity for the app with the AcrPull role on the registry (correct)*

**A build pipeline must push images to ACR and nothing else. Which role should you assign?**
- AcrPull
- *AcrPush (correct)*
- Contributor

**What is the default state of the ACR admin user?**
- Enabled with one password
- Enabled only on Premium registries
- *Disabled (correct)*

**You need to build a container image in Azure from a Dockerfile without Docker installed locally. What should you use?**
- az acr import
- *az acr build (ACR Tasks) (correct)*
- az acr login

**Images must be rebuilt automatically whenever their base image is updated. What should you configure?**
- *An ACR task with a base image update trigger (correct)*
- An ACR webhook
- Geo-replication

**What is the login server for a registry named contosoregistry?**
- contosoregistry.azurecontainer.io
- contosoregistry.azurewebsites.net
- *contosoregistry.azurecr.io (correct)*

**You need to copy a public image from another registry into your ACR without pulling it to a local machine. What should you use?**
- az acr build
- *az acr import (correct)*
- docker save and docker load

**What is an Azure Container Instances container group?**
- A set of identical container instances spread across availability zones
- A registry repository that groups related images
- *Containers scheduled on the same host that share a lifecycle, network and storage volumes (correct)*

**Two containers in the same ACI container group need to communicate. How should they connect?**
- Through each container's own public IP address
- *Through localhost on their exposed ports (correct)*
- Through an Azure Load Balancer

**You need to deploy a multi-container group of Windows containers in ACI. What should you tell the team?**
- Use a YAML file instead of the portal
- Deploy it into a virtual network
- *Multi-container groups support only Linux containers; Windows supports a single container per group (correct)*

**How should you deploy an ACI container group that contains a web container and a logging sidecar?**
- The Azure portal Create container instance wizard
- *A YAML file or an ARM/Bicep template (correct)*
- docker compose up against the ACI endpoint

**What is the default restart policy for an ACI container group?**
- Never
- OnFailure
- *Always (correct)*

**An ACI container runs a nightly batch job and should only restart if it exits with an error. Which restart policy should you use?**
- *OnFailure (correct)*
- Always
- Never

**A container in ACI must keep its data after the container restarts. What should you mount?**
- An emptyDir volume
- *An Azure Files share (correct)*
- A secret volume

**A database password must be passed to an ACI container without being visible in the portal or API. What should you use?**
- A standard environment variable
- *A secure environment variable (correct)*
- A command-line argument

**You need to increase an existing ACI container group from 1 CPU to 2 CPUs. What should you do?**
- Update the CPU value in the portal
- Restart the container group with a new CPU value
- *Delete the container group and recreate it with the new CPU value (correct)*

**How do you autoscale Azure Container Instances based on CPU load?**
- Configure a CPU-based scale rule on the container group
- *ACI has no built-in autoscale; deploy more container groups or use Container Apps or AKS (correct)*
- Set the restart policy to Always

**An ACI container group has two containers, each requesting 1 CPU and 1.5 GB of memory. What does the group get?**
- 1 CPU and 1.5 GB, shared
- *2 CPUs and 3 GB (correct)*
- 1 CPU and 3 GB

**An ACI container group deployed into a virtual network needs outbound internet access. What must you configure?**
- *A NAT gateway on the subnet (correct)*
- A public IP on the container group
- A service endpoint on the subnet

**What is the default minimum number of replicas for an Azure Container Apps revision?**
- 1
- 3
- *0 (correct)*

**What is the default maximum number of replicas for an Azure Container Apps revision?**
- 1
- 1,000
- *10 (correct)*

**A container app must scale based on the number of messages in an Azure Service Bus queue. Which type of scale rule should you use?**
- *Custom (KEDA) (correct)*
- HTTP
- TCP

**A container app is slow on the first request after idle periods. You need at least one replica always running. What should you configure?**
- *A minimum replica count of 1 (correct)*
- A maximum replica count of 1
- An HTTP scale rule with a concurrency of 1

**You need to send 80% of a container app's traffic to the current version and 20% to a new version. What should you configure?**
- *Multiple revision mode with traffic splitting (correct)*
- Single revision mode with two containers
- Two separate container apps behind Traffic Manager

**What is the default revision mode for Azure Container Apps?**
- Multiple
- Labels
- *Single (correct)*

**Which change to a container app creates a new revision?**
- Adding a secret
- *Changing the container image (correct)*
- Changing ingress settings

**A container app must be reachable only from other apps in its environment and the virtual network. How should you configure ingress?**
- External ingress with IP restrictions
- No ingress
- *Internal ingress (correct)*

**A steady, always-busy container workload needs reserved compute and predictable cost in Container Apps. Which workload profile should you use?**
- *Dedicated (correct)*
- Consumption
- Flexible

**You need to run a container task on a schedule until it completes, within an existing Container Apps environment. What should you use?**
- A container app with min replicas 0
- An ACI container group with restart policy Always
- *A Container Apps job (correct)*

**Does Azure Container Apps support vertical autoscaling of CPU and memory?**
- Yes, through custom KEDA rules
- Yes, in the Dedicated workload profile
- *No, only horizontal scaling of replicas is supported (correct)*

**A team needs full access to the Kubernetes API and control over node pools. Which service should you choose?**
- *Azure Kubernetes Service (correct)*
- Azure Container Apps
- Azure Container Instances

---
## 7. App Service

**Three web apps run in the same App Service plan. One app has a CPU spike. What's the effect on the others?**
- None; each app has dedicated instances
- They're moved to a different plan automatically
- *They're affected, because all apps in a plan share the same instances (correct)*

**What is the lowest App Service plan tier that supports autoscale rules?**
- Basic
- *Standard (correct)*
- Premium v3

**What is the lowest App Service plan tier that supports deployment slots?**
- Basic
- Free
- *Standard (correct)*

**What is the maximum number of instances in a Standard App Service plan?**
- *10 (correct)*
- 3
- 30

**A web app must scale out to 25 instances. Which tier should you choose?**
- *Premium v3 or v4 (correct)*
- Basic
- Standard

**What is the maximum number of instances in a Basic App Service plan?**
- 1
- 10
- *3 (correct)*

**How many deployment slots does the Premium tier support per app?**
- *20 (correct)*
- 5
- 10

**An app in a Premium plan has 8 deployment slots. You try to scale the plan down to Standard and it fails. Why?**
- *Standard supports only 5 slots, fewer than the app is using (correct)*
- Standard doesn't support deployment slots
- Slots must be deleted before any scale operation

**You want App Service to scale out based on HTTP traffic without writing any rules, with prewarmed instances. What should you use, and which tiers support it?**
- Autoscale, Standard and above
- *Automatic scaling, Premium v2 to v4 (correct)*
- Manual scale, Basic and above

**A web app needs more memory per instance. What should you do?**
- Scale out the App Service plan
- *Scale up the App Service plan (correct)*
- Enable per-app scaling

**You want to move a web app to a different App Service plan. What is a requirement?**
- Both plans must use the same pricing tier
- The target plan must be empty
- *The target plan must be in the same resource group and region (correct)*

**What is the lowest App Service tier that supports a custom domain with a TLS/SSL binding?**
- Free
- *Basic (correct)*
- Shared

**You need to map the root domain contoso.com to a web app. Which DNS records should you create?**
- *An A record for @ pointing to the app's IP, and a TXT record named asuid (correct)*
- A CNAME record for @ and a TXT record named asuid
- A CNAME record for www only

**You need to map www.contoso.com to a web app. Which DNS records should you create?**
- An A record for @ and a TXT record named asuid
- *A CNAME record for www pointing to the app's default hostname, and a TXT record named asuid.www (correct)*
- An MX record for www

**What is the purpose of the asuid TXT record when you add a custom domain to App Service?**
- It enables HTTPS for the domain
- *It verifies that you own the domain (correct)*
- It routes traffic to the correct deployment slot

**You need a free certificate for *.contoso.com on App Service. What should you tell the team?**
- Create an App Service managed certificate for the wildcard
- *App Service managed certificates don't support wildcards; use an App Service certificate or import one (correct)*
- Create a managed certificate on a Basic plan

**Creating a free managed certificate for the root domain contoso.com fails. The app has IP-based access restrictions. What is the cause?**
- Managed certificates only support subdomains
- *The app must be reachable from the internet, so access restrictions block issuance (correct)*
- The plan must be Premium

**You need a certificate that you can export and use on servers outside App Service. Which option should you choose?**
- *An App Service certificate (paid) (correct)*
- An App Service managed certificate
- A public certificate (.cer)

**You import a certificate from Key Vault into App Service, and it fails with an access error. What should you do?**
- Enable the Key Vault firewall
- Assign the app's managed identity the Key Vault Administrator role
- *Grant the App Service resource provider read access to the vault's certificates and secrets (correct)*

**Older clients that don't support SNI must connect to an app over HTTPS. Which binding is required, and from which tier?**
- SNI SSL, Basic
- SNI SSL, Free
- *IP-based SSL, Standard (correct)*

**All HTTP requests to a web app must be redirected to HTTPS. What should you enable?**
- *HTTPS Only (correct)*
- Minimum TLS version 1.2
- Client certificates

**Which statement describes App Service automatic backups?**
- They require a storage account and run daily
- They're available only in Premium and Isolated tiers
- *They run hourly, need no storage account, and are kept for 30 days (correct)*

**An app runs in a Basic plan. Which slots can you back up and restore?**
- All deployment slots
- *Only the production slot (correct)*
- None; backup requires Standard

**What is the maximum size of an App Service custom backup?**
- 30 GB
- 4 GB
- *10 GB (correct)*

**How does App Service authenticate to the storage account for custom backups?**
- With the app's managed identity
- *With a SAS-based URL, because managed identity isn't supported for backup (correct)*
- With the storage account access key in an app setting

**You need to restore a production web app from backup with the least downtime. What should you do?**
- Restore directly over the production slot
- *Restore to a staging slot, then swap it into production (correct)*
- Restore to a different region

**A web app must call a SQL Server VM that has only a private IP in a virtual network. What should you configure?**
- A private endpoint for the web app
- *VNet integration for the web app (correct)*
- Access restrictions on the web app

**A web app must be reachable only from resources inside a virtual network. What should you configure?**
- VNet integration
- *A private endpoint for the web app (correct)*
- Hybrid Connections

**What is a requirement for the subnet used by App Service VNet integration?**
- It must contain the target VMs
- It must have a service endpoint for Microsoft.Web
- *It must be dedicated and delegated to Microsoft.Web/serverFarms (correct)*

**Which subnet size does Microsoft recommend for App Service VNet integration?**
- /29
- */26 (correct)*
- /16

**A web app must reach one SQL Server on-premises on TCP 1433. There's no VPN or ExpressRoute. What should you use?**
- *Hybrid Connections (correct)*
- VNet integration
- A private endpoint

**A web app must accept traffic only from your corporate public IP range. What should you configure?**
- *Access restrictions (correct)*
- VNet integration with an NSG
- A NAT gateway

**A web app has a private endpoint and access restrictions that allow only your corporate IPs. Do the access restrictions apply to traffic arriving through the private endpoint?**
- Yes, all traffic is evaluated against access restrictions
- *No, access restrictions don't apply to private endpoint traffic (correct)*
- Only for the scm site

**A partner must allow-list a single, static outbound IP address for a web app's calls. What should you configure?**
- *VNet integration with a NAT gateway on the integration subnet (correct)*
- An app-assigned address
- A private endpoint

**What is the default hostname of a deployment slot named staging for an app named contoso?**
- staging.contoso.azurewebsites.net
- *contoso-staging.azurewebsites.net (correct)*
- contoso.staging.azurewebsites.net

**When you swap a staging slot into production, which setting moves with the code by default?**
- *App settings that aren't marked as deployment slot settings (correct)*
- Custom domain names
- Scale settings

**When you swap slots, which setting stays with the slot?**
- Connection strings without the slot setting flag
- *Custom domain names (correct)*
- Framework version

**The staging slot must keep its own database connection string after a swap. What should you do?**
- Store the connection string in Key Vault
- Disable auto swap
- *Mark the connection string as a Deployment slot setting (correct)*

**A swap into production introduced a bug. What is the fastest way to roll back?**
- *Swap the slots again (correct)*
- Restore the latest automatic backup
- Redeploy the previous build to production

**You want 10% of production traffic to go to the staging slot for testing. What should you configure?**
- Auto swap
- *A traffic percentage for the slot (testing in production) (correct)*
- Swap with preview

**What does swap with preview do?**
- Swaps only the code without any settings
- Swaps the slots for one hour and then reverts
- *Applies the target slot's settings to the source slot and pauses so you can validate before completing the swap (correct)*

**A production app uses a system-assigned managed identity. What happens to the identity when you swap a staging slot into production?**
- The identity moves to the staging slot
- *Managed identities aren't swapped; each slot keeps its own (correct)*
- Both slots share the identity after the swap

**A web app must require users to sign in with Microsoft Entra ID without any code changes. What should you configure?**
- *App Service Authentication (Easy Auth) (correct)*
- Access restrictions with the AzureActiveDirectory service tag
- A private endpoint

---
## 8. Related Topics

**Which vault should you use to back up Azure VMs with Azure Backup?**
- A Key Vault
- *A Recovery Services vault (correct)*
- A storage account container

**VMs must be replicated to another region so you can fail over during a regional outage. What should you use?**
- Azure Backup with cross-region restore
- *Azure Site Recovery (correct)*
- Availability zones

**You need to share and version custom VM images across several subscriptions and regions. What should you use?**
- A storage account container of VHDs
- Template specs
- *Azure Compute Gallery (correct)*

**Several VMs in an application tier need the lowest possible network latency between them. What should you use?**
- An availability zone per VM
- A load balancer with session persistence
- *A proximity placement group (correct)*
