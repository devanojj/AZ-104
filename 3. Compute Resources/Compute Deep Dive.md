
1. **Automate deployment by using ARM templates or Bicep files** — interpret, modify (ARM and Bicep), deploy, export a deployment / convert ARM to Bicep
2. **Create and configure virtual machines** — create a VM, **encryption at host**, move a VM (RG / subscription / region), VM sizes, VM disks, availability zones and availability sets, VM Scale Sets
3. **Provision and manage containers in the Azure portal** — Azure Container Registry, Azure Container Instances, Azure Container Apps, sizing and scaling for ACI and ACA
4. **Create and configure Azure App Service** — App Service plan, scaling, create an app, certificates and TLS, custom DNS names, backup, networking settings, deployment slots


> [!warning] Things that changed recently — older study material is wrong on these
> * **Azure Disk Encryption (ADE)** retires **15 September 2028**. The outline now tests **encryption at host** instead — it's the recommended option for new VMs.
> * **Unmanaged disks** (VHDs in storage accounts) were **fully retired 31 March 2026**. Managed disks only.
> * **Standard HDD as an OS disk** retires **8 September 2028** (data disks unaffected).
> * **App Service backups** now work from the **Basic** tier, and **automatic backups** run hourly with **no storage account**. Custom backups can no longer include linked databases in new configurations.
> * **App Service Premium** tiers (v2–v4) scale to **30** instances; **automatic scaling** (HTTP-driven) is available on Premium v2–v4.
> * **VM Scale Sets** created with CLI/PowerShell default to **Flexible** orchestration, which Microsoft recommends over availability sets.

---
## 1. ARM Templates & Bicep

### ARM Template (JSON) Structure
| Element          | Required? | Purpose                                                              |
| ---------------- | --------- | -------------------------------------------------------------------- |
| `$schema`        | ✅         | Schema location — also identifies the deployment scope               |
| `contentVersion` | ✅         | Your version number for the template (e.g. `1.0.0.0`)                |
| `parameters`     | Optional  | Values supplied **at deployment time** (parameter file, CLI, portal) |
| `variables`      | Optional  | Values computed **inside** the template to simplify expressions      |
| `functions`      | Optional  | User-defined functions                                               |
| `resources`      | ✅         | What gets deployed                                                   |
| `outputs`        | Optional  | Values returned after deployment (e.g. a resource ID or FQDN)        |

**Parameter properties:** `type` (`string`, `int`, `bool`, `object`, `array`, `securestring`, `secureObject`), `defaultValue`, `allowedValues`, `minValue`/`maxValue`, `minLength`/`maxLength`, `metadata.description`.

**Functions you'll see in exam snippets:**
* `parameters('name')` / `variables('name')` — read a value
* `resourceGroup().location` — deploy to the RG's region
* `uniqueString(resourceGroup().id)` — deterministic 13-char hash, used to make globally unique names
* `concat()` — join strings
* `resourceId()` — build a resource ID (often inside `dependsOn`)
* `reference()` — read a runtime property of a deployed resource

**Other keywords:**
* `dependsOn` — explicit deployment order
* `copy` — loop to create multiple instances (`"count": 3`, with `copyIndex()` for unique names)
* `condition` — deploy the resource only if true

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageSku": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [ "Standard_LRS", "Standard_GRS", "Standard_ZRS" ]
    }
  },
  "variables": {
    "storageName": "[concat('store', uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2023-01-01",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "sku": { "name": "[parameters('storageSku')]" },
      "kind": "StorageV2"
    }
  ],
  "outputs": {
    "storageId": {
      "type": "string",
      "value": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageName'))]"
    }
  }
}
```

### The Same Thing in Bicep
```bicep
@allowed([
  'Standard_LRS'
  'Standard_GRS'
  'Standard_ZRS'
])
param storageSku string = 'Standard_LRS'

var storageName = 'store${uniqueString(resourceGroup().id)}'

resource storage 'Microsoft.Storage/storageAccounts@2023-01-01' = {
  name: storageName
  location: resourceGroup().location
  sku: { name: storageSku }
  kind: 'StorageV2'
}

output storageId string = storage.id
```

|Concept|ARM JSON|Bicep|
|---|---|---|
|Parameter|`"parameters": {}`|`param name type = default`|
|Restrict values|`"allowedValues": []`|`@allowed([...])` decorator|
|Secret parameter|`"type": "securestring"`|`@secure() param x string`|
|Variable|`"variables": {}`|`var name = ...`|
|Resource|object in `resources` array|`resource symbolicName 'type@apiVersion' = {}`|
|Dependency|explicit `dependsOn`|**implicit** when you reference another resource's symbolic name (e.g. `vnet.id`)|
|Loop|`copy` element|`[for i in range(0, 3): {...}]`|
|Condition|`"condition": true`|`resource x '...' = if (deploy) {...}`|
|Reuse|linked/nested templates|`module`|
|Existing resource|`reference()` / `resourceId()`|`resource x '...' existing = {...}`|
|Scope|from `$schema`|`targetScope = 'subscription'` (default `resourceGroup`)|

* Bicep is **transpiled to ARM JSON** before deployment — `az bicep build --file main.bicep`. Anything ARM can do, Bicep can do.
* **Parameter files:** `azuredeploy.parameters.json` (JSON) or `main.bicepparam` (Bicep). Secrets in parameter files can be pulled from **Key Vault references** instead of plain text.

### Deploying
```bash
az deployment group create -g myRG --template-file main.bicep --parameters @main.parameters.json
az deployment group what-if -g myRG --template-file main.bicep          # preview changes
az deployment sub create -l eastus --template-file sub.bicep            # subscription scope
```
```powershell
New-AzResourceGroupDeployment -ResourceGroupName myRG -TemplateFile main.json -TemplateParameterFile main.parameters.json
New-AzResourceGroupDeployment -ResourceGroupName myRG -TemplateFile main.json -WhatIf
```
* **Scopes:** resource group (`az deployment group`), subscription (`az deployment sub`), management group (`az deployment mg`), tenant (`az deployment tenant`).
* **Deployment modes:**
  * **Incremental** (default) — adds/updates resources in the template; **leaves other resources in the RG alone**.
  * **Complete** — **deletes resources in the RG that aren't in the template**. Dangerous.
* **What-if** previews creates/modifies/deletes without changing anything. `--confirm-with-what-if` / `-c` previews then prompts.
* Templates are **idempotent** — redeploying the same template gives the same result.
* **Template specs** — store a template as an Azure resource and share it via RBAC.

### Export and Convert
|Option|What you get|
|---|---|
|**Export from resource group / resource** (portal "Export template")|**Snapshot of current state**, includes manual changes, every property, mostly **hard-coded** values. Can export as **ARM JSON or Bicep** (Bicep export is portal-only)|
|**Export from deployment history**|**Exact copy** of the template that was deployed, with its parameters. **ARM JSON only**. Doesn't include later manual changes|

* **Convert ARM → Bicep:** `az bicep decompile --file main.json` (or `bicep decompile main.json`). Best effort — review names like `storageName_var`.
* **Convert Bicep → ARM:** `az bicep build --file main.bicep`.
* CLI export: `az group export --name myRG > main.json`.

---
## 2. Creating & Managing Virtual Machines

See also [[VM]], [[VM Sizes]], [[Azure Spot Instance]].

### Create-Time Decisions
* **Region**, **image** (Marketplace, Azure Compute Gallery, custom), **size**, **authentication** (SSH key / password), **disks**, **VNet/subnet/NIC/public IP/NSG**, **security type** (Standard, Trusted launch, Confidential).
* **Availability options** — no redundancy / **availability zone** / **availability set** / **scale set**. Chosen **at creation**.
* **Cost options** — **Spot** (cheap, can be evicted), **Azure Hybrid Benefit** (bring Windows Server/SQL/RHEL/SUSE licences), reservations / savings plans.
* **Boot diagnostics** — serial console log and screenshot for unbootable VMs.

### VM States & Billing
| State                                          | Compute billed?    | Notes                                                         |
| ---------------------------------------------- | ------------------ | ------------------------------------------------------------- |
| **Running**                                    | ✅                  | —                                                             |
| **Stopped** (shut down from inside the OS)     | ✅ **still billed** | Hardware still allocated                                      |
| **Stopped (deallocated)** (Stop in portal/CLI) | ❌                  | Disks still billed; **temp disk wiped**; dynamic IPs released |

* **Temporary disk** (`D:` on Windows, `/dev/sdb1` on Linux) is lost on **deallocation, resize, redeploy, or host move** — never store data there.
* **Redeploy** moves the VM to a new host (fixes host-level problems). **Reapply** reruns provisioning for a VM stuck in a failed state.

### Extensions & Configuration
* **Custom Script Extension** — run PowerShell/Bash after deployment (90-minute limit).
* **DSC extension** / Azure Automation State Configuration — enforce config state. See [[DSC vs Runbook vs Custom Script]].
* **Azure Monitor Agent**, **Network Watcher Agent**, **Run Command** (run scripts without RDP/SSH).

### VM Sizes
| Family             | Optimised for                                       |
| ------------------ | --------------------------------------------------- |
| **B**              | Burstable — low baseline CPU with occasional spikes |
| **D**              | General purpose — balanced CPU/memory               |
| **F**              | Compute — high CPU-to-memory                        |
| **E / M**          | Memory — databases, SAP HANA, caches                |
| **L**              | Storage — high disk throughput / IOPS               |
| **N** (NC, ND, NV) | GPU — AI/ML, rendering                              |
| **H** (HB, HC)     | High-performance computing                          |

**Reading a size name:** `Standard_D4ds_v5` → family **D**, **4** vCPUs, **d** = local temp disk, **s** = supports **Premium SSD**, **v5** = generation. **a** = AMD processor.

### Resizing
* Resizing a **running** VM **restarts it**.
* If the new size **isn't available on the current hardware cluster**, **deallocate** first (then any size in the region is available).
* In an **availability set**, if the new size isn't on the set's cluster you may need to **deallocate every VM in the set**.
* List valid sizes: `az vm list-vm-resize-options` / `Get-AzVMSize -ResourceGroupName rg -VMName vm`.
* Moving to Premium SSD needs a size that supports premium storage (the **`s`** suffix).
* **vCPU quota** is per region and per VM family — request an increase if a deployment fails on quota.

---
## 3. VM Disks & Encryption

See also [[Azure Disks]] and [[Azure Managed Disks]] in the storage notes.

### Disk Roles
* **OS disk** — boot volume; OS uses up to **4,095 GiB** of it.
* **Temporary disk** — local to the host, non-persistent.
* **Data disks** — persistent; the maximum count depends on VM size.

### Managed Disk Types
|Type|Max size|Max IOPS|OS disk?|Use for|
|---|---|---|---|---|
|**Ultra Disk**|64 TiB|400,000|❌|SAP HANA, top-tier DBs; change performance without restart|
|**Premium SSD v2**|64 TiB|80,000|❌|Production, sub-ms latency; set size/IOPS/throughput independently|
|**Premium SSD**|32 TiB|20,000|✅|Production workloads|
|**Standard SSD**|32 TiB|6,000|✅|Web servers, light apps, dev/test|
|**Standard HDD**|32 TiB|2,000|✅ (retiring as OS disk 8 Sept 2028)|Backup, infrequent access|

* Disks can be **LRS** or **ZRS** (Premium SSD and Standard SSD support ZRS).
* **Host caching** per disk: **None**, **ReadOnly**, **ReadWrite**.

### Managing Disks
* **Attach / detach** a data disk while the VM is running.
* **Expand** (can't shrink):
  * **Data disks** can be expanded **without downtime**, except Standard/Premium disks going from **≤ 4 TiB to > 4 TiB** (deallocate first).
  * **OS disks** and **shared disks** require the VM to be **deallocated**.
  * Then **extend the volume inside the OS**.
* **Change disk type** (Standard HDD ⇄ Standard SSD ⇄ Premium SSD): **deallocate the VM**, change it, start again. Allowed **twice per day**. To get to Ultra/Premium SSD v2 for most disks, use an **incremental snapshot** and create a new disk.
* **Premium SSD performance tier** can be raised **without downtime** (downgrade once per 12 hours).
* **Snapshots** — **full** or **incremental** (billed for changed data only). Ultra and Premium SSD v2 support **incremental only**.
* **Shared disks** — attach one disk to multiple VMs (`maxShares > 1`) for clustered apps like Windows Server Failover Clustering. Enable on a disk that's **detached from all VMs**.

### Encryption Options
|  |**SSE** (server-side)|**Encryption at host**|**Azure Disk Encryption** (retiring)|
|---|---|---|---|
|Where it encrypts|In the storage cluster|**On the VM host** — end to end|**Inside the guest OS** (BitLocker / DM-Crypt)|
|OS & data disks at rest|✅ always on|✅|✅|
|**Temp disk**|❌|✅ (platform-managed keys)|✅|
|**Disk caches**|❌|✅|✅|
|Data encrypted compute → storage|❌|✅|✅|
|Uses VM CPU|No|**No**|**Yes**|
|Customer-managed keys|Via **Disk Encryption Set**|Via **Disk Encryption Set**|Via Key Vault KEK|
|Status|Default, can't be disabled|**Recommended for new VMs**|**Retires 15 Sept 2028**|

### Encryption at Host
1. **Register the feature** on the subscription: `az feature register --namespace Microsoft.Compute --name EncryptionAtHost` (or `Register-AzProviderFeature`). Wait for **Registered**.
2. For **customer-managed keys**, create a **Key Vault (purge protection on)** + key + **Disk Encryption Set**.
3. Enable on the VM:
   * **New VMs** — tick *Encryption at host* on the Disks tab.
   * **Existing VMs** — must be **deallocated** and reallocated.

**Restrictions:**
* **Can't** be enabled on a VM that **has ever had ADE** enabled, and ADE can't be enabled on disks with encryption at host. ADE → encryption at host migration means **new disks and VMs**.
* Not every VM size supports it — check before resizing.
* On a scale set, only **new** instances are encrypted after enabling.

---
## 4. Moving VMs

### To Another Resource Group or Subscription
* The VM **keeps running** — only the **resource ID changes**. Update scripts/dashboards that use it.
* Source and target resource groups are **locked** during the move (up to 4 hours).
* **Both subscriptions must be in the same Microsoft Entra tenant.**
* **Dependent resources move together.** For a cross-subscription move, the VM, **managed disks, NIC, VNet (and all NICs on it), public IP, NSG** must be in the **same RG** and move in one operation. Error: `MissingMoveDependentResources`.
* Disassociate a **Standard SKU public IP** before moving across subscriptions.
* **Role assignments don't move** — recreate them.

**Not supported:**
* VMs in an **availability set** moved **individually**
* Marketplace-plan VMs **across subscriptions**
* Moves when a **ReadOnly lock** is on the source, destination or subscription
* Moving a **resource group itself** (move its resources instead)
* A **peered VNet** — disable the peering first

### To Another Region or Zone
* **Azure Resource Mover** — select resources → validate dependencies → prepare → initiate move → commit. Can target a specific zone or availability set in the new region.
* **Azure Site Recovery** — replicate and fail over; also used to move between **availability zones** in the same region or between public and government clouds.
* A plain "move" never changes a resource's region.

---
## 5. Availability: Sets, Zones and Scale Sets

See also [[Availability Zone vs Set]].

|Option|Protects against|SLA|Notes|
|---|---|---|---|
|**Single VM** — Premium SSD / Premium SSD v2 / Ultra on all disks|Host failure (auto-restart)|**99.9%**|Standard SSD = **99.5%**, Standard HDD = **95%**|
|**Availability set** (≥ 2 VMs)|Rack, power, switch failure; planned maintenance|**99.95%**|One datacentre|
|**Availability zones** (≥ 2 VMs in different zones)|Whole datacentre failure|**99.99%**|Physically separate datacentres in a region|

### Availability Sets
* **Fault domains** (shared power/network rack): up to **3** (region-dependent, sometimes 2).
* **Update domains** (rebooted together during planned maintenance): up to **20**, default **5**. One UD at a time, with a 30-minute recovery window.
* **Set at creation only** — you can't add an existing VM to a set or move it out. **Recreate the VM** from its disks.
* No extra cost. Can't be combined with zones.
* Microsoft now recommends **VM Scale Sets with Flexible orchestration** for high availability.

### Availability Zones
* Pick **Zone 1, 2 or 3** at creation. Disks and public IPs are zonal or zone-redundant.
* Pair with a **zone-redundant Standard Load Balancer**.
* Use availability sets only in regions **without zones**, or when you need the **lowest VM-to-VM latency**.

### VM Scale Sets (VMSS)
|                         | **Flexible** (recommended)                     | **Uniform**            |
| ----------------------- | ---------------------------------------------- | ---------------------- |
| VM types                | **Mixed sizes**, **Spot + on-demand**          | Identical instances    |
| Instance management     | Standard VM APIs; Azure Backup & ASR supported | VMSS VM APIs only      |
| Max instances           | 1,000                                          | 1,000                  |
| Spread                  | Zones or fault domains                         | Zones or fault domains |
| Default outbound access | ❌ — needs explicit outbound (NAT gateway etc.) | ✅                      |
| Automatic OS upgrades   | ❌                                              | ✅                      |

* **Orchestration mode is set at creation and can't be changed.** CLI/PowerShell default to **Flexible**.
* **Upgrade policy** (how instances get model changes): **Manual** (default), **Automatic** (no ordering guarantees — could take all down), **Rolling** (batches; needs the **Application Health extension** or an LB **health probe**).
* **Scale-in policy:** **Default** (balance across zones/FDs, then delete highest instance ID), **NewestVM**, **OldestVM**.
* **Instance protection:** *Protect from scale-in* (autoscale won't remove it) or *Protect from scale set actions* (also skips upgrades/reimage).
* **Automatic instance repairs** replace unhealthy instances (needs health monitoring).
* **Spot instances** with an **eviction policy** of **Deallocate** or **Delete**.

### Autoscale (VMSS, App Service, and others)
* **Instance limits:** minimum, maximum, **default** (used when metrics aren't available).
* **Rules:** metric (e.g. average CPU > 70% over 10 minutes → +1) or **schedule** (e.g. scale to 10 on weekdays 08:00).
* **Cool down** (default 5 minutes) between actions prevents **flapping**.
* Always pair a **scale-out** rule with a **scale-in** rule and leave a gap between the thresholds (e.g. out > 75%, in < 25%).
* Host metrics (CPU, disk, network) need no agent. Guest OS metrics such as memory need a monitoring agent.

---
## 6. Containers

See also [[ACI vs ACA]], [[ACR]], [[Container]].

### Azure Container Registry (ACR)
Private registry for container images and OCI artifacts — `myregistry.azurecr.io`. Name must be globally unique, 5–50 alphanumeric characters.

|Feature|**Basic**|**Standard**|**Premium**|
|---|---|---|---|
|Entra auth, webhooks, delete|✅|✅|✅|
|Included storage / throughput|Lowest|Higher|Highest|
|Anonymous pull|❌|✅|✅|
|**Geo-replication**|❌|❌|✅|
|**Private endpoints (Private Link)**|❌|❌|✅|
|Customer-managed keys|❌|❌|✅|

* **Zone redundancy** is on by default in supported regions for every tier.
* **Authentication** — best to worst:
  1. **Managed identity** of the pulling service with the **AcrPull** role.
  2. Individual login `az acr login` (Entra).
  3. Service principal.
  4. **Admin user** — disabled by default; one shared account, two passwords. Avoid.
* **Roles:** **AcrPull** (pull), **AcrPush** (push + pull), **AcrDelete**. Contributor/Owner manage the registry itself.
* **ACR Tasks** — build images in Azure without local Docker: `az acr build --registry myreg --image app:v1 .`. Triggers: source commit, **base image update**, schedule.
* **Import** an image from another registry: `az acr import`.

### Azure Container Instances (ACI)
Serverless, run-a-container-now. No orchestration, billed **per second**.

* **Container group** = containers scheduled on the **same host**, sharing **lifecycle, IP/DNS label, ports and volumes** (like a Kubernetes pod). They talk over **`localhost`**.
* **Multi-container groups are Linux only**; Windows supports a single container per group.
* **Deploy:** portal/CLI for single containers; **YAML** or **ARM/Bicep** for multi-container groups.
* **Restart policy:**
  * **Always** (default) — long-running services.
  * **OnFailure** — batch jobs (restart only on non-zero exit).
  * **Never** — run once.
* **Networking:** public IP + DNS label (`<label>.<region>.azurecontainer.io`), or deploy into a **dedicated VNet subnet** (no other resources; /24 recommended; outbound needs a **NAT gateway**).
* **Volumes:** **Azure Files** share (persistent), `emptyDir`, `secret`.
* **Secure environment variables** (`secureValue`) hide secrets from the API/portal.

**Sizing & scaling ACI:**
* You request **CPU and memory per container**. The group's allocation is the **sum** of its containers (minimum **1 CPU / 1 GB per group**).
* Changing **CPU, memory, GPU, OS type, restart policy, network profile or zone** requires **deleting and recreating** the group.
* **No built-in autoscale.** Scale by running **more container groups** behind Application Gateway/Traffic Manager, or move to **Container Apps** / AKS.

### Azure Container Apps (ACA)
Serverless platform for **microservices, APIs and event-driven workers**, built on Kubernetes + **KEDA** + Dapr, without managing a cluster.

* **Environment** — secure boundary shared by a group of apps (same VNet and Log Analytics workspace).
* **Workload profiles:** **Consumption** (serverless, scale to zero, pay per use), **Dedicated** (reserved VM-backed compute, steady workloads/GPU), **Flexible** (preview).
* **Ingress:** **external** (internet) or **internal** (environment/VNet only); HTTP (auto/HTTP2) or **TCP**. External TCP ingress needs a custom VNet.
* **Revisions** — immutable snapshots of an app version. Changing the container image or scale rules (**revision-scope**) creates a new revision; changing secrets or ingress (**application-scope**) doesn't.
* **Revision modes:**
  * **Single** (default) — the new revision gets 100% of traffic once it's ready; zero downtime.
  * **Multiple** — **traffic splitting** by percentage (blue-green, A/B).
  * **Labels** — stable URL per label.
* **Jobs** — run-to-completion tasks: manual, scheduled, or event-driven.

**Sizing & scaling ACA:**
* **Vertical:** set CPU and memory **per container**. Vertical **autoscaling isn't supported**.
* **Horizontal (replicas per revision):**

|Limit|Default|Range|
|---|---|---|
|Min replicas|**0** (scale to zero)|0–1,000|
|Max replicas|**10**|1–1,000|

* **Scale rules:** **HTTP** (concurrent requests, default 10), **TCP** (concurrent connections), **Custom** (KEDA — CPU, memory, Service Bus, Event Hubs, Kafka, Redis…).
* The app scales out when **any** rule's threshold is met.
* **Scale to zero = no usage charges.** Set **min replicas ≥ 1** to avoid cold starts or to keep one instance always running.

### Choosing a Container Service
|Need|Choose|
|---|---|
|Run one container or a short batch job fast, no orchestration|**ACI**|
|Microservices, HTTP APIs, event-driven, **scale to zero**, revisions, traffic splitting|**Container Apps**|
|Full Kubernetes API and node-level control|**AKS**|
|Web app in a container with slots, custom domains, Easy Auth|**App Service** (Web App for Containers)|
|Strong isolation / full OS control|**VM**|

---
## 7. App Service

See also [[App Service Plan]], [[App Service SSL]], [[Scale Up vs Scale Out]].

### App Service Plan
The **plan** is the compute (region, OS, tier, instance size, instance count). **All apps and slots in a plan share its instances** — a noisy app affects the others. Isolate critical apps in their own plan.

|Tier|Max instances|Custom domain|TLS binding|Autoscale|Slots|Backups|VNet integration / Private endpoint|
|---|---|---|---|---|---|---|---|
|**Free (F1)**|1 (shared)|❌|❌|❌|❌|❌|❌|
|**Shared (D1)**|1 (shared)|✅|❌|❌|❌|❌|❌|
|**Basic (B1–B3)**|**3**|✅|✅ SNI|Manual only|❌|✅ (production slot only)|✅|
|**Standard (S1–S3)**|**10**|✅|✅ SNI + IP|✅ rules|**5**|✅|✅|
|**Premium v2/v3/v4**|**30**|✅|✅|✅ rules + **automatic scaling**|**20**|✅|✅|
|**Isolated v2** (App Service Environment)|**100**|✅|✅|✅|**20**|✅|Runs inside your VNet|

* **SLA 99.95%** from Basic upwards. **Always On** needs Basic+.
* **Managed certificates** need **Basic+**.
* **Zone redundancy** is available on Premium v2+ and Isolated v2.
* An app can only move to another plan in the **same resource group and region**.
* You can't scale down to Standard if the app has **more than 5 slots**.

### Scaling
|  |**Scale up**|**Scale out**|
|---|---|---|
|Changes|**Tier/size** (more CPU/RAM, unlocks features)|**Instance count**|
|Use when|An instance is maxed out or you need a feature (slots, VNet integration)|More users/traffic|

| Scale-out method      | Tiers             | How                                                                                     |
| --------------------- | ----------------- | --------------------------------------------------------------------------------------- |
| **Manual**            | Basic+            | Set instance count                                                                      |
| **Autoscale**         | **Standard+**     | Metric rules (CPU, memory, HTTP queue) and **schedules**; applies to the **whole plan** |
| **Automatic scaling** | **Premium v2–v4** | Platform scales on **HTTP traffic**, per-app max, prewarmed instances; no rules         |

* Use only **one** scaling method per plan.
* **Per-app scaling** (Standard+) caps how many plan instances a single app uses.

### Create an App Service
* Choose **publish** (Code or Container), **runtime stack**, **OS** (Windows/Linux), **region** and **plan**.
* Default hostname: `<app>.azurewebsites.net`.
* **Deployment Center:** GitHub Actions, Azure DevOps, Bitbucket, local Git, ZIP deploy, FTPS, container registry.
* **App settings** and **connection strings** are exposed to the app as **environment variables**.
* **Authentication ("Easy Auth")** — Entra ID, Google, GitHub etc. with no code changes.

### Custom Domains
|Domain|Records|
|---|---|
|**Root** (`contoso.com`)|**A** → app's IP **+ TXT `asuid`** → verification ID|
|**Subdomain** (`www.contoso.com`)|**CNAME** → `<app>.azurewebsites.net` **+ TXT `asuid.www`** (A record also works but the IP can change)|
|**Wildcard** (`*.contoso.com`)|**CNAME `*`** **+ TXT `asuid`**|

* The **`asuid` TXT** record proves ownership. Free tier can't use custom domains.
* Don't create both a CNAME and an A record for the same name.
* The same custom domain can't be added to two apps in the same scale unit ("stamp").

### Certificates & TLS
| Option                                  | Cost | Wildcard | Exportable | Notes                                                                                                                                      |
| --------------------------------------- | ---- | -------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **App Service managed certificate**     | Free | ❌        | ❌          | Auto-renewed. Basic+. Apex needs an **A record** and the app reachable (**no IP restrictions**); not with Traffic Manager apex; not in ASE |
| **App Service certificate**             | Paid | ✅        | ✅          | Stored in **your Key Vault**                                                                                                               |
| **Import from Key Vault**               | —    | ✅        | —          | Grant the App Service resource provider (`Microsoft.Azure.WebSites`) **Key Vault Certificate User** (RBAC) or Get on secrets/certs         |
| **Upload a private certificate** (.pfx) | —    | ✅        | —          | Bring your own                                                                                                                             |
| **Public certificate** (.cer)           | —    | —        | —          | For app code calling remote services, **not** for securing a domain                                                                        |

* **Bindings:** **SNI SSL** (many certificates share one IP; Basic+, the normal choice) or **IP-based SSL** (dedicated IP for old clients; **Standard+**).
* **HTTPS Only** redirects HTTP → HTTPS. **Minimum TLS version** (1.2 default).
* **Client certificates (mTLS)** can be required.

### Backup & Restore
|                             | **Automatic backups**              | **Custom backups**                                                       |
| --------------------------- | ---------------------------------- | ------------------------------------------------------------------------ |
| Tiers                       | Basic, Standard, Premium, Isolated | Basic, Standard, Premium, Isolated                                       |
| Setup                       | **None**                           | Storage account + container                                              |
| Frequency                   | **Hourly**, not configurable       | Scheduled (every 2 h min on Basic/Standard, 1 h on Premium) or on demand |
| Retention                   | **30 days**, fixed                 | 0–30 days or indefinite                                                  |
| Max size                    | **30 GB**                          | **10 GB** (incl. up to 4 GB linked DB)                                   |
| Downloadable                | ❌                                  | ✅ (blobs)                                                                |
| Linked database             | ❌                                  | Being removed — can't be added to new configs; ends **31 March 2028**    |
| Partial backups / over VNet | ❌                                  | ✅ (`_backup.filter`; firewalled storage via VNet integration)            |

* Custom backups need a storage account that allows **SAS**. Managed identity auth to storage **isn't supported** for backup.
* **Basic tier:** only the **production slot** can be backed up/restored.
* **Restore** to the same app, **a new app**, or **a slot**. Restoring **stops the target**, so restore into a **staging slot and swap** to minimise downtime.

### Networking
| Direction    | Feature                                   | What it does                                                                                                                                                                                                                                                                      |
| ------------ | ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Inbound**  | **Access restrictions**                   | Priority-ordered allow/deny rules (IP, service tag, service endpoint subnet, headers); separate rules for the **scm/Kudu** site. Don't apply to private endpoint traffic                                                                                                          |
| **Inbound**  | **Private endpoint**                      | App gets a **private IP in your VNet**; inbound only; Basic+                                                                                                                                                                                                                      |
| **Inbound**  | **App-assigned address**                  | Dedicated inbound IP (IP-based SSL)                                                                                                                                                                                                                                               |
| **Outbound** | **VNet integration**                      | App calls **into** a VNet (same region; peered/global-peered VNets, ExpressRoute, private endpoints). Basic+. **Dedicated subnet delegated to `Microsoft.Web/serverFarms`**, **/26 recommended** (portal minimum /27). NSG, UDR and **NAT gateway** on that subnet control egress |
| **Outbound** | **Hybrid Connections**                    | Reach a single **TCP host:port** on-prem via **Hybrid Connection Manager** over 443 — no VPN                                                                                                                                                                                      |
| **Both**     | **App Service Environment** (Isolated v2) | Single-tenant, deployed into your VNet                                                                                                                                                                                                                                            |

> [!warning] Direction trap
> **VNet integration is outbound only** — it doesn't let anything in the VNet reach the app. For private **inbound** access, use a **private endpoint** (or access restrictions for IP filtering).

### Deployment Slots
* **Standard: 5 slots**, **Premium/Isolated: 20**. Each slot is a live app at `<app>-<slot>.azurewebsites.net`. Slots share the plan's instances. No extra charge.
* **Swap:** settings from the target are applied to the source, the source **warms up**, then routing switches. **No downtime**, and swapping back is an **instant rollback**. Swap **with preview** pauses after applying settings so you can validate.
* **Auto swap** — deploy to staging and swap automatically.
* **Testing in production** — route a **percentage of traffic** to a slot.

|**Swapped** (follows the code)|**Not swapped** (stays with the slot)|
|---|---|
|Framework/runtime version, 32/64-bit, WebSockets|**Custom domain names**|
|**App settings & connection strings** (unless marked **Deployment slot setting**)|**Non-public certificates & TLS settings**, HTTPS Only, TLS version|
|Handler mappings, path mappings|**Scale settings**|
|Public certificates|**IP / access restrictions**|
|WebJobs content|**Always On**|
|Hybrid connections, service endpoints, CDN|**Diagnostic log settings**, **CORS**|
|Mounted storage (unless sticky)|**Managed identities**|
||**VNet integration**|
||Publishing endpoints, WebJobs schedulers|

* Tick **Deployment slot setting** to make an app setting or connection string **sticky** (e.g. a staging database connection string).

---
## 8. Related Topics — Lower Priority

* **Azure Backup for VMs** — Recovery Services vault, app-consistent (VSS) on Windows, instant restore snapshots. See [[Recovery Services Vault]], [[Backup Vault]].
* **Azure Site Recovery** — replicate VMs to another region for DR (also tested in the Monitor & Maintain area). See [[Azure Site Recovery]].
* **Azure Bastion** — browser RDP/SSH, covered in [[Virtual Networking Deep Dive]].
* **Proximity placement groups** — keep VMs physically close for the lowest latency.
* **Azure Compute Gallery** — share and version custom VM images across subscriptions/regions.
* **Azure Functions** — event-driven serverless code (Consumption, Flex Consumption, Premium, Dedicated plans).

---
## 9. Exam Scenario → Answer (and *why*)

| Scenario                                                                             | Answer                                                                                                                     | Why that one                                                          |
| ------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| "Preview what a template will change before deploying"                               | **What-if** (`az deployment group what-if` / `-WhatIf`)                                                                    | Shows creates/modifies/deletes without touching anything              |
| "Deployment must remove resources not defined in the template"                       | **Complete mode**                                                                                                          | Incremental (default) never deletes resources it doesn't know about   |
| "Get a reusable template for resources someone built by hand in the portal"          | **Export template** from the resource group                                                                                | History export only exists for template deployments                   |
| "Get the exact template and parameters used in last week's deployment"               | Export from **deployment history**                                                                                         | Resource export is a snapshot with hard-coded values                  |
| "Convert an existing ARM JSON template to Bicep"                                     | `az bicep decompile --file main.json`                                                                                      | `build` goes the other direction                                      |
| "Restrict a template parameter to three SKUs"                                        | `allowedValues` (JSON) / `@allowed([...])` (Bicep)                                                                         | Deployment fails validation on any other value                        |
| "Generate a globally unique storage name that's the same on every redeploy"          | `uniqueString(resourceGroup().id)`                                                                                         | Deterministic hash, so redeploys are idempotent                       |
| "Create 3 identical NICs from one resource definition"                               | `copy` loop (JSON) / `for` loop (Bicep)                                                                                    | Avoids duplicating the resource block                                 |
| "Encrypt the temp disk and disk caches without using VM CPU"                         | **Encryption at host**                                                                                                     | SSE doesn't cover temp disk/cache; ADE uses guest CPU and is retiring |
| "Enable encryption at host fails on an existing VM"                                  | **Register the `EncryptionAtHost` feature**, **deallocate** the VM, check the size supports it (and that it never had ADE) | All three are prerequisites                                           |
| "Control the keys used to encrypt managed disks"                                     | **Customer-managed keys** via a **Disk Encryption Set** + Key Vault                                                        | SSE uses platform keys by default                                     |
| "Move a VM to a different subscription — it fails"                                   | Move the **VM + disks + NIC + VNet + public IP + NSG together** from one RG (and same tenant)                              | Dependent resources must move in the same operation                   |
| "Move a VM to another region"                                                        | **Azure Resource Mover** (or Site Recovery)                                                                                | RG/subscription moves never change region                             |
| "Add an existing VM to an availability set"                                          | **Recreate** the VM from its disks in the set                                                                              | Availability set membership is creation-time only                     |
| "Survive a datacentre failure, 99.99% SLA"                                           | ≥ 2 VMs across **availability zones**                                                                                      | Availability sets stay inside one datacentre (99.95%)                 |
| "Highest SLA for a single VM"                                                        | **Premium SSD / Premium SSD v2 / Ultra** on all disks → 99.9%                                                              | Standard SSD 99.5%, Standard HDD 95%                                  |
| "Resize fails because the size isn't available"                                      | **Deallocate** the VM, then resize (in an availability set, maybe all VMs)                                                 | Deallocation lets Azure pick any cluster in the region                |
| "Need more IOPS on a Premium SSD without downtime"                                   | Raise the **performance tier** (or expand the data disk)                                                                   | Disk type changes need deallocation                                   |
| "Database needs 150,000 IOPS on a data disk"                                         | **Ultra Disk**                                                                                                             | Premium SSD v2 caps at 80,000; neither can be the OS disk             |
| "Scale set with a mix of VM sizes and Spot instances"                                | VMSS **Flexible**                                                                                                          | Uniform requires identical instances; mode can't be changed later     |
| "Update scale set instances in batches without downtime"                             | **Rolling** upgrade policy + health extension/probe                                                                        | Automatic may take all instances down                                 |
| "Stop autoscale removing a VM running a long job"                                    | **Instance protection — protect from scale-in**                                                                            | Scale-in policy only chooses *which* VM goes                          |
| "Autoscale keeps adding and removing instances"                                      | Widen the gap between thresholds / lengthen **cool down**                                                                  | Classic flapping                                                      |
| "Pull images from ACR without storing credentials"                                   | **Managed identity** with **AcrPull**                                                                                      | Admin user is a shared credential                                     |
| "Registry replicated to multiple regions / private endpoint"                         | ACR **Premium**                                                                                                            | Geo-replication and Private Link are Premium-only                     |
| "Build images in Azure when the base image updates"                                  | **ACR Tasks**                                                                                                              | No local Docker needed                                                |
| "Run a nightly batch container, billed only while running"                           | **ACI** with restart policy **OnFailure** (or Never)                                                                       | Always (default) would restart it forever                             |
| "Web container + logging sidecar on the same host"                                   | ACI **multi-container group** via **YAML/ARM** (Linux)                                                                     | Portal only creates single containers; Windows doesn't support groups |
| "Change an ACI container group's CPU from 1 to 2"                                    | **Delete and recreate** the group                                                                                          | CPU/memory aren't updatable in place                                  |
| "Microservice that scales to zero on idle and splits traffic 80/20 between versions" | **Container Apps**, **multiple revision mode**                                                                             | ACI has no autoscale or revisions                                     |
| "Scale a container app based on Service Bus queue length"                            | **Custom (KEDA) scale rule**                                                                                               | HTTP/TCP rules only count requests/connections                        |
| "Container app must always have one replica warm"                                    | **Min replicas = 1**                                                                                                       | Default min is 0 (scale to zero)                                      |
| "Web app needs autoscale and staging slots at lowest cost"                           | **Standard (S1)**                                                                                                          | Basic has neither                                                     |
| "Web app needs 25 instances"                                                         | **Premium v3/v4**                                                                                                          | Standard caps at 10                                                   |
| "Free TLS certificate for www.contoso.com"                                           | **App Service managed certificate** (Basic+)                                                                               | No wildcard, not exportable                                           |
| "Wildcard certificate for *.contoso.com"                                             | **App Service certificate** or **import from Key Vault/upload PFX**                                                        | Managed certificates don't support wildcards                          |
| "Map contoso.com (root) to a web app"                                                | **A record** + **TXT `asuid`**                                                                                             | CNAME isn't allowed at the root                                       |
| "Web app must call a SQL VM in a VNet"                                               | **VNet integration** (outbound)                                                                                            | Private endpoint is inbound only                                      |
| "Only users in the VNet may reach the web app"                                       | **Private endpoint** (and disable public access)                                                                           | VNet integration doesn't control inbound                              |
| "Web app must reach one on-prem SQL server without a VPN"                            | **Hybrid Connections**                                                                                                     | Reaches a single host:port over outbound 443                          |
| "Back up the app without configuring storage"                                        | **Automatic backups** (Basic+)                                                                                             | Custom backups need a storage account                                 |
| "Staging must use a different database connection string after the swap"             | Mark the connection string **Deployment slot setting**                                                                     | Otherwise it moves with the code                                      |
| "Custom domain disappeared from production after a swap"                             | It didn't — **custom domains don't swap**; check DNS/bindings                                                              | Domains and TLS bindings stay with the slot                           |

---
## 10. Portal Practice That Pays Off

- [x] Export a resource group's template as **Bicep** and as JSON; `az bicep decompile` the JSON and compare
- [x] Edit a template to add an `allowedValues` parameter and an output; deploy with **what-if** first
- [x] Deploy the same template in **Complete** mode to a test RG containing an extra resource and watch it get deleted
- [x] Register `EncryptionAtHost`, create a VM with encryption at host, then enable it on an existing VM (deallocate first)
- [x] Resize a VM; deallocate and resize to a size that wasn't offered while running
- [x] Add a data disk, expand it online, change an OS disk from Standard SSD to Premium SSD (deallocated), take an incremental snapshot
- [x] Move a VM and its dependencies to another resource group; try moving it alone and read the error
- [x] Create VMs in an availability set and in zones; compare fault/update domains
- [x] Create a **Flexible** scale set with autoscale rules and a scale-in policy; protect one instance from scale-in
- [x] Create an ACR, push/import an image, pull it from ACI using a managed identity with AcrPull
- [x] Deploy an ACI multi-container group from YAML with an Azure Files volume and `OnFailure` restart
- [x] Create a Container App with an HTTP scale rule (min 0, max 5), switch to multiple revisions and split traffic
- [x] Create a Standard App Service plan, add a staging slot with a sticky setting, swap, then swap back
- [x] Map a custom domain (A + TXT), add a managed certificate, enable HTTPS Only
- [x] Configure custom backups and restore to a new slot; add VNet integration and a private endpoint

---
## 11. One-Line Exam Facts

* ARM template required: **`$schema`, `contentVersion`, `resources`**
* **Incremental** = default, keeps extra resources; **Complete** = deletes extras
* **What-if** previews changes; works at RG, subscription, MG and tenant scope
* `az bicep decompile` = ARM → Bicep; `az bicep build` = Bicep → ARM
* Export from RG = current-state snapshot (JSON or Bicep in portal); from history = exact template (JSON only)
* Bicep dependencies are **implicit**; ARM needs `dependsOn`
* **Stopped** (OS shutdown) still billed; **deallocated** isn't; temp disk wiped on deallocate/resize
* Resizing a running VM **restarts** it; size not on cluster → **deallocate**
* `s` in a size name = **Premium SSD support**; `d` = local temp disk; `a` = AMD
* Ultra and Premium SSD v2 **can't be OS disks**; max 64 TiB
* Single VM SLA: **Premium/PremV2/Ultra 99.9%**, Standard SSD 99.5%, Standard HDD 95%
* Availability set **99.95%**, zones **99.99%** (≥ 2 VMs)
* Availability set: **3 FDs, 20 UDs (default 5)**, **creation-time only**
* Expand data disks online; **OS/shared disks need deallocation**; **can't shrink**
* Change disk type: **deallocate**, max **twice a day**
* **Encryption at host**: register `EncryptionAtHost`, deallocate existing VMs, **not if ADE was ever used**
* **ADE retires 15 Sept 2028**; unmanaged disks retired **31 March 2026**
* Cross-subscription moves: **same tenant**, **dependent resources together**, resource ID changes, VM keeps running
* Region moves: **Azure Resource Mover** / Site Recovery
* VMSS **orchestration mode can't be changed**; Flexible = mixed sizes + Spot; max **1,000** instances
* VMSS upgrade policy default **Manual**; Rolling needs health monitoring
* Scale-in policies: **Default, NewestVM, OldestVM**
* ACR: **Premium** = geo-replication + private endpoints + CMK; **AcrPull** for pulls; admin user off by default
* ACI: **multi-container = Linux only**, restart **Always** default, CPU/memory change = **recreate**, **no autoscale**
* ACA: min replicas **0**, max **10** default (up to 1,000); HTTP / TCP / custom KEDA rules
* ACA revisions: **Single** (default) vs **Multiple** (traffic splitting)
* App Service max instances: **Basic 3, Standard 10, Premium 30, Isolated 100**
* Autoscale **Standard+**; automatic scaling **Premium v2–v4**
* Slots: **Standard 5, Premium/Isolated 20**
* Custom domain: root = **A + TXT asuid**; subdomain = **CNAME + TXT asuid.sub**
* Managed certificate: **free, Basic+, no wildcard, not exportable**
* IP-based SSL **Standard+**; SNI SSL **Basic+**
* Backups: **Basic+**; automatic = hourly / 30 days / 30 GB / no storage; custom = 10 GB / storage account
* **VNet integration = outbound** (delegated subnet `Microsoft.Web/serverFarms`, /26 recommended); **private endpoint = inbound**
* Not swapped: **custom domains, TLS bindings, scale settings, IP restrictions, Always On, managed identities, VNet integration**
* Sticky app setting = **Deployment slot setting** checkbox
