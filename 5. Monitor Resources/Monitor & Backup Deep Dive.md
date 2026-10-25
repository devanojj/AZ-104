
1. **Monitor resources in Azure** — interpret metrics, configure log settings, query and analyse logs, set up alert rules / action groups / alert processing rules, monitor VMs / storage / networks with **Azure Monitor Insights**, use **Network Watcher and Connection monitor**
2. **Implement backup and recovery** — create a **Recovery Services vault** and a **Backup vault**, create and configure a **backup policy**, back up and restore with **Azure Backup**, configure **Azure Site Recovery** for Azure resources, **fail over to a secondary region**, configure and interpret backup **reports and alerts**


> [!warning] Things that changed recently — older study material is wrong on these
> * The **Log Analytics agent (MMA/OMS) was retired 31 August 2024**. Guest data is collected with the **Azure Monitor Agent (AMA) + data collection rules (DCRs)**.
> * **VM Insights Map / Dependency agent** is deprecated (no new portal onboarding since 30 Sept 2025; retires 30 June 2028).
> * **Backup center** and **Azure Business Continuity Center** are now **Resiliency** in the Azure portal.
> * **NSG flow logs** can't be created any more — use **VNet flow logs** (see [[Virtual Networking Deep Dive]]).
> * Fired **alerts are kept for 30 days**; Log Analytics data defaults to **30 days** (31 included in the ingestion price).

---
## 1. Azure Monitor Data Types

See also [[Types of Monitor]], [[Monitor vs Watcher]].

| Data                     | What it is                                                                                       | Collected by default?                             | Retention                |
| ------------------------ | ------------------------------------------------------------------------------------------------ | ------------------------------------------------- | ------------------------ |
| **Platform metrics**     | Numeric time series from Azure resources (CPU %, network in/out, disk ops) every ~1 min          | ✅ no config, no agent                             | **93 days**              |
| **Activity log**         | **Control-plane** events: who created/deleted/changed what, service/resource health              | ✅ subscription level                              | **90 days**              |
| **Resource logs**        | **Data-plane** operations inside a resource (e.g. blob reads, Key Vault secret gets, NSG events) | ❌ needs a **diagnostic setting**                  | Wherever you send them   |
| **Guest OS data**        | Memory, logical disk free space, Windows events, Syslog, per-process                             | ❌ needs **AMA + DCR**                             | Log Analytics or Metrics |
| **Microsoft Entra logs** | Sign-ins, audit                                                                                  | Entra ID → diagnostic setting                     | Wherever you send them   |
| **Application data**     | Requests, dependencies, exceptions                                                               | Application Insights (SDK / auto-instrumentation) | Workspace retention      |

> [!important] Host vs guest
> Host metrics (agentless) **don't include memory used or free disk space inside the OS**. For those you need the **Azure Monitor Agent** and a **DCR**.

---
## 2. Metrics — Metrics Explorer

* Pick a **scope** (one or more resources of the same type/region), a **metric**, an **aggregation** and a **time range**.
* **Aggregations:** **Average, Minimum, Maximum, Sum (Total), Count**.
  * CPU % → use **Average** (or Max for spikes). Network bytes → **Sum**. Requests → **Sum/Count**.
* **Time granularity** (e.g. 1 min, 5 min, 1 h) — the bucket each point summarises.
* **Splitting** — one line per dimension value (e.g. split *Transactions* by **ResponseType** or **API name**; split disk metrics by **LUN**).
* **Filtering** — show only certain dimension values (e.g. only `ResponseType = ServerBusy`).
* Metrics are kept **93 days**, but a single chart shows **max 30 days** — pan to see older data. For longer, send metrics to Log Analytics via a diagnostic setting.
* Charts can be **pinned to dashboards**, added to **workbooks**, or used to **create an alert rule** directly.

---
## 3. Log Settings — Diagnostic Settings, Workspaces, DCRs

### Diagnostic Settings
Route **resource logs**, **platform metrics** or the **activity log** somewhere you can keep or analyse them.

|Destination|Use for|
|---|---|
|**Log Analytics workspace**|Query with KQL, log alerts, workbooks, Insights|
|**Storage account**|Cheap long-term **archive** / compliance (same region as the resource)|
|**Event Hubs**|**Stream** to a SIEM or third-party tool|
|**Partner solution**|Integrated partner monitoring (e.g. Datadog, Elastic)|

* Up to **5 diagnostic settings per resource**. Pick categories (or the `allLogs` / `audit` groups) and `AllMetrics`.
* **Activity log** — create a **subscription-level** diagnostic setting (Monitor → Activity log → Export / Diagnostic settings) to keep it past 90 days or query it as the `AzureActivity` table.
* Can be enforced at scale with **Azure Policy** (DeployIfNotExists).

### Log Analytics Workspace
* Data stored in **tables** (`Heartbeat`, `Perf`, `Event`, `Syslog`, `AzureActivity`, `AzureDiagnostics`, `InsightsMetrics`, resource-specific tables…).
* **Retention:** default **30 days** (31 included in ingestion price; 90 for Sentinel/Application Insights). **Analytics retention** up to **2 years**; **total (long-term) retention** up to **12 years** — query old data with a **search job**. Set per workspace or **per table**.
* **Table plans:** **Analytics** (full features, alerts), **Basic** (cheaper ingestion, 30-day queries, billed per query), **Auxiliary** (cheapest, audit/verbose logs).
* **Access control mode:** workspace-context (permissions on the workspace) or **resource-context** (users see logs for resources they can access). Roles: **Log Analytics Reader / Contributor**, **Monitoring Reader / Contributor**.
* Cost breakdown: workspace → **Usage and estimated costs**. See [[Cost]].

### Azure Monitor Agent (AMA) & Data Collection Rules (DCRs)
* **AMA** is a VM extension (Windows/Linux, Azure VMs, scale sets and Arc servers). It authenticates with a **managed identity**. Replaces the retired Log Analytics agent, WAD/LAD and Telegraf.
* A **DCR** defines **what** to collect (performance counters, Windows event logs with XPath filters, Syslog facilities/levels, text logs), optional **transformations**, and **where** to send it (Log Analytics workspace, Azure Monitor Metrics).
* A VM can have **many DCRs**; one DCR can apply to **many VMs**. Associate in the portal, or at scale with **Azure Policy**.
* Creating a DCR in the portal **installs AMA** automatically on selected VMs. See [[DCR]].

---
## 4. Querying Logs with KQL

* Open **Logs** from a workspace (all data) or from a resource (scoped to that resource).
* KQL reads top to bottom with the pipe `|` passing results on.

|Operator|Does|
|---|---|
|`where`|Filter rows|
|`project`|Choose/rename columns|
|`extend`|Add a calculated column|
|`summarize ... by`|Aggregate (count, avg, max, dcount) per group|
|`bin(TimeGenerated, 5m)`|Bucket time|
|`sort by` / `order by`, `top`, `take`/`limit`|Order and limit rows|
|`join`, `union`|Combine tables|
|`render timechart`|Chart the result|
|`ago(1h)`|Relative time|

```kusto
// Average CPU per computer, 15-minute buckets, last 24 h
Perf
| where TimeGenerated > ago(24h)
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| summarize AvgCPU = avg(CounterValue) by Computer, bin(TimeGenerated, 15m)
| render timechart
```
```kusto
// VMs that stopped sending heartbeats in the last 10 minutes
Heartbeat
| summarize LastBeat = max(TimeGenerated) by Computer
| where LastBeat < ago(10m)
```
```kusto
// Who deleted resources in the last 7 days
AzureActivity
| where TimeGenerated > ago(7d)
| where OperationNameValue endswith "DELETE" and ActivityStatusValue == "Success"
| project TimeGenerated, Caller, ResourceGroup, _ResourceId
```
```kusto
// Failed Windows sign-ins (event ID 4625)
Event
| where EventID == 4625
| summarize Failures = count() by Computer
| top 10 by Failures
```

* Save queries, pin results, and turn any query into a **log search alert**.

---
## 5. Alerts

See also [[Alert Types]], [[Alert Suppression]].

### Three Building Blocks
|Piece|Does|
|---|---|
|**Alert rule**|**Detects** — scope + signal + condition (+ severity)|
|**Action group**|**Responds** — who to notify and what to run|
|**Alert processing rule**|**Modifies fired alerts** — **suppress** notifications (e.g. during maintenance) or **add action groups** at scale, optionally on a **schedule**|

### Alert Rule Types
|Type|Signal|Use for|
|---|---|---|
|**Metric alert**|Platform/custom metrics, near-real-time (every 1 min). **Static** or **dynamic** (ML-learned) thresholds; multiple conditions; multi-resource|CPU > 80%, low available memory (via AMA metrics), storage availability drop|
|**Log search alert**|A **KQL query** on a workspace at a set frequency|Specific event ID, heartbeat loss, combined/complex logic|
|**Activity log alert**|A matching **activity log** event|VM deleted/deallocated, NSG changed, role assignment created|
|**Service Health alert**|Azure **service** incidents, planned maintenance, advisories in your regions|"Tell me when Azure has an outage affecting my services"|
|**Resource Health alert**|**Your resource** becomes unavailable/degraded|"Tell me when this VM is unavailable"|
|Smart detection / Prometheus|App Insights anomalies / PromQL|App and Kubernetes workloads|

* **Severity:** **Sev 0 (Critical)** → Sev 1 (Error) → Sev 2 (Warning) → Sev 3 (Informational) → **Sev 4 (Verbose)**.
* **Monitor condition** (set by the system): **Fired → Resolved**. Metric alerts are **stateful** by default (resolve automatically when the condition clears); log alerts can be stateful or stateless.
* **Alert state** (set by users): **New → Acknowledged → Closed**.
* Fired alerts are kept **30 days**.
* Limits: **100 activity log alert rules per subscription** (can't be raised — use log search alerts on `AzureActivity` instead); 5,000 metric and 5,000 log alert rules.

### Action Groups
* **Notifications:** email, **email Azure Resource Manager role** (e.g. all Owners), **SMS**, Azure mobile app **push**, **voice**.
* **Actions:** **Automation runbook**, **Azure Function**, **Logic App**, **webhook / secure webhook**, **ITSM** (ServiceNow etc.), **Event Hubs**.
* Reusable across many alert rules (and budgets — see the governance deep dive).
* **Rate limits:** **SMS and voice — max 1 every 5 minutes per number**; email — max 100 per hour per address. Runbooks, Functions, Logic Apps and webhooks **aren't rate limited**.

### Alert Processing Rules
* Scope: resources, resource groups or a subscription, with filters (severity, resource type, alert rule…).
* Action: **Suppress notifications** or **Apply action group**.
* Schedule: always, one-time window, or recurring (e.g. nightly 22:00–06:00, weekends).
* The alert still **fires and is recorded** — only the notifications/actions are suppressed. Suppression wins if both rule types match.
* Can take up to **30 minutes** to take effect. Limit: 1,000 per subscription.

---
## 6. Azure Monitor Insights

Curated monitoring experiences (built on workbooks) for specific resource types.

|Insight|Shows|Needs|
|---|---|---|
|**VM insights**|**Performance** (CPU, available memory, logical disk, network) across many VMs; **Map** of processes and connections (deprecated)|**AMA + VM insights DCR** → `InsightsMetrics` table; Map needs Dependency agent|
|**Storage insights**|**Capacity, availability, performance, transactions, failures** across many storage accounts at once|Nothing — built on **platform metrics**|
|**Network insights**|**Topology**, health of network resources, **connectivity** (Connection monitor), **traffic** (flow logs / traffic analytics)|Network Watcher; flow logs for traffic|
|**Container insights**|AKS/Arc Kubernetes nodes, pods, containers|AMA / managed Prometheus|
|**Application Insights**|App performance, failures, dependencies, availability tests|SDK or auto-instrumentation|
|Key Vault / Cosmos DB / others|Service-specific views|Mostly metrics|

* **Workbooks** combine text, KQL queries, metrics and parameters into interactive reports — Insights are pre-built workbooks. See [[Azure Workbooks]].
* **Boot diagnostics** (screenshot + serial log) and **Serial Console** are the tools for **unbootable VMs**, not Insights.

---
## 7. Network Watcher & Connection Monitor

Full tool-by-tool detail is in the networking deep dive (section 10) and [[Azure Network Watcher]].

|Need|Tool|
|---|---|
|Is this flow allowed, and **which NSG rule** decides?|**IP flow verify**|
|Where is this traffic **routed**?|**Next hop**|
|Point-in-time "can A reach B on port X?" with hop-by-hop faults|**Connection troubleshoot**|
|**Continuous** reachability/latency monitoring with alerts, Azure **and on-prem**|**Connection monitor**|
|Raw packets|**Packet capture**|
|Flow history / traffic analysis|**VNet flow logs** + **traffic analytics**|
|VPN gateway/connection diagnosis|**VPN troubleshoot**|
|Visual diagram|**Topology**|

### Connection Monitor
* **Test groups** = **sources** (Azure VMs/scale sets with the **Network Watcher Agent** extension, or on-prem/Arc machines with **AMA**) + **destinations** (VMs, FQDNs, URLs, IPs, Microsoft 365 endpoints) + **test configurations** (protocol **TCP / HTTP / ICMP**, port, frequency).
* Measures **checks failed (%)** and **round-trip time (ms)** against thresholds; results go to a **Log Analytics workspace** and can trigger **alerts**.
* Network Watcher is enabled **per region** automatically (resource group `NetworkWatcherRG`).

---
## 8. Azure Backup

See also [[MARS vs MABS]], [[MARS]], [[Recovery Services Vault]], [[Backup Vault]], [[Backup]], [[Instant Restore]].

### Which Vault?
|**Recovery Services vault**|**Backup vault**|
|---|---|
|**Azure VMs**|**Azure Disks** (managed disk snapshots)|
|**SQL Server in Azure VMs**, **SAP HANA in Azure VMs**|**Azure Blobs** (operational and vaulted)|
|**Azure Files**|**Azure Database for PostgreSQL**|
|On-premises via **MARS agent**, **MABS**, **DPM**|AKS (Kubernetes)|
|**Azure Site Recovery**||

* The vault must be in the **same region** as the VMs it protects (VMs can be in other subscriptions in the same tenant).
* Limits: up to **1,000 Azure VMs** per vault, **100 VMs per backup policy**, 500 vaults per subscription.
* Manage everything from **Resiliency** (formerly Backup center / Business Continuity Center): protection status, jobs, policies, alerts, reports across subscriptions and regions.

### Vault Storage Redundancy
* **LRS**, **ZRS** or **GRS** (default **GRS**).
* **Set it before protecting any items** — once backups exist you can't change it (you'd have to stop protection and delete the data).
* **Cross Region Restore (CRR)**: needs a **GRS** vault; lets you restore in the **paired region at any time** (drills, audits, outages) without waiting for Microsoft. Extra cost; data can take up to **48 hours** to appear in the secondary; can't be turned off once protection starts. Secondary-region RPO can be up to 36 hours.

### VM Backup Policies
|  |**Standard**|**Enhanced**|
|---|---|---|
|Frequency|**Once a day** (or weekly)|**Multiple per day** — every **4, 6, 8, 12 or 24 h**|
|Instant restore (snapshot) retention|**1–5 days** (default 2)|**1–30 days** (default 7)|
|Supports **Trusted Launch**, **Ultra Disk**, **Premium SSD v2**, shared disks|❌ (Trusted Launch only via newer CLI/PS/API)|✅|
|ZRS snapshots|❌|✅|
|Switch back|—|**Can't switch Enhanced → Standard**|

* **Retention:** daily / weekly / monthly / yearly points (GFS), up to **99 years**. Monthly/yearly points can go to the **Archive tier**.
* **Tiers:** **Snapshot** (instant restore, fastest) → **Vault-Standard** → **Vault-Archive** (long-term, cheapest, slow restore).
* **On-demand backup** ("Backup now") uses a custom retention you set, independent of the policy.
* **Consistency:** **application-consistent** (Windows **VSS**; Linux with **pre/post scripts**), **file-system consistent** (Linux default), **crash-consistent** (VM off or VSS failed).
* Backup works even if the VM is **stopped** (crash-consistent) and needs no explicit outbound connectivity. Enabling backup installs the **VM backup extension**.

### Restore Options (Azure VM)
|Option|Result|Use when|
|---|---|---|
|**Create new VM**|A new VM from the recovery point|Quick full rebuild alongside the original|
|**Restore disks**|Managed disks + an **ARM template** to customise|You need to change size, network, settings before creating the VM|
|**Replace existing**|Swaps the disks on the **existing VM**|Keep the same VM identity and network config|
|**File recovery (item-level)**|Script mounts the recovery point as drives over **iSCSI** for **12 hours**|Recover a few files/folders|
|**Cross Region Restore**|Any of the above in the **paired region**|GRS + CRR enabled|

* **Instant restore** from snapshots is fastest (no data transfer from the vault).

### Other Workloads
* **Azure Files** — share snapshots or **vaulted** backup in a Recovery Services vault; restore whole share or individual files.
* **SQL Server / SAP HANA in VMs** — full, differential and **log backups every 15 minutes** → **point-in-time restore**.
* **On-premises** — **MARS agent** (files, folders, system state, directly to Azure) or **MABS/DPM** (VMs, apps, central backup server). See [[MARS vs MABS]].

### Backup Security
* **Soft delete** — deleted backup data kept **14 days** by default at no cost. **Enhanced soft delete**: customise **14–180 days** and make it **always-on** (can't be disabled).
* **Immutable vault** — recovery points can't be deleted before they expire; can be **locked** (irreversible).
* **Multi-user authorisation (MUA)** — a **Resource Guard** (owned by a different admin/tenant) must approve critical operations (disable soft delete, reduce retention, delete protection).
* **Security PIN** for critical operations from MARS; RBAC roles **Backup Contributor / Backup Operator / Backup Reader**; private endpoints; CMK encryption.

### Backup Reports & Alerts
* **Reports:** configure a **diagnostic setting** on each vault to send data (resource-specific tables) to a **Log Analytics workspace**, then view **Backup reports** in Resiliency (Summary, Backup items, Usage, Jobs, Policies, Optimise). First data can take **24 hours**; start viewing after ~2 days. Data retention follows the workspace (30 days default).
* **Alerts:** **built-in Azure Monitor alerts** fire automatically for job failures, security events (e.g. soft delete disabled) — no alert rule needed. Route them with an **alert processing rule + action group**. **Metric alerts** (backup health) need an alert rule. Classic backup alerts are retired.
* Monitor jobs in the vault → **Backup jobs**, or in Resiliency.

---
## 9. Azure Site Recovery (ASR)

See also [[Azure Site Recovery]], [[BCDR]].

### Backup vs Site Recovery
|  |**Azure Backup**|**Azure Site Recovery**|
|---|---|---|
|Protects against|Data loss/corruption, accidental deletion, ransomware|**Region or site outage**|
|How|Scheduled **recovery points** kept for a long time|**Continuous replication** of VMs to another region|
|RPO|Hours (daily or every 4 h)|**Minutes** (crash-consistent points every 5 min)|
|RTO|Hours (restore)|**Minutes** (failover)|
|Result|Restore data/VMs|**Fail over** the running workload|

### Azure-to-Azure Architecture
1. Create a **Recovery Services vault in the target (secondary) region** — so you can still manage failover if the source region is down.
2. **Enable replication** on the VM: choose target region, resource group, VNet, availability options, replication policy.
3. ASR **automatically installs the Mobility service extension** on the VM.
4. Disk writes go to a **cache storage account in the source region**, then are processed into **replica managed disks** in the target region.
5. Target resources (VNet, etc.) can be created automatically or mapped to existing ones.

### Replication Policy
|Setting|Default|Notes|
|---|---|---|
|**Recovery point retention**|**1 day**|How far back you can fail over; up to **15 days** (managed disks)|
|**App-consistent snapshot frequency**|**Off** (0 hours)|Minimum **1 hour**; must be less than retention|
|**Crash-consistent points**|Every **5 minutes**|Fixed, can't change|
* **Multi-VM consistency** (replication groups) for apps spanning several VMs — shared recovery points, performance cost.

### Failover Workflow
1. **Test failover** — creates the VM in an **isolated, non-production VNet** in the target region; **no impact on production or replication**. Run **Cleanup test failover** afterwards. Do this regularly (DR drills).
2. **Failover** — choose a recovery point:
   * **Latest processed** — lowest **RTO** (no processing wait)
   * **Latest** — lowest **RPO** (processes all pending data first)
   * **Latest app-consistent**
   * **Custom** (single VM only)
   Optionally **shut down the source VM** first to avoid data loss.
3. **Commit** — finalises the failover (removes other recovery points).
4. **Re-protect** — starts replicating from the secondary back to the primary.
5. **Failback** — fail over again to the primary region, then re-protect again.

* **Recovery plans** group VMs into ordered **boot groups** with scripts, runbooks and manual actions, so a whole app fails over in the right order.
* ASR doesn't use the vault's storage redundancy setting — no need for a GRS vault for ASR.

---
## 10. Related

* **Azure Advisor** (cost/reliability/security recommendations) — see [[Azure Advisor]] and the governance deep dive.
* **App Service diagnostic logging** — application logging to **Blob** for long retention (filesystem logging turns off after 12 h); severity levels. See [[Logging]].
* **Service Health** (Azure-wide incidents) vs **Resource Health** (your specific resource).

---
## 11. Exam Scenario → Answer (and *why*)

|Scenario|Answer|Why that one|
|---|---|---|
|"Alert when a VM's CPU exceeds 80% for 5 minutes"|**Metric alert**|Numeric platform metric, near-real-time|
|"Alert on low available memory / free disk space in the guest"|Install **AMA + DCR**, then a metric or log alert|Host metrics don't include in-guest memory/disk|
|"Alert when anyone deletes a VM"|**Activity log alert** on the delete operation|Deletion is a control-plane event|
|"Alert when event ID 4625 appears 10 times in 5 minutes"|**Log search alert** (KQL on `Event`)|Needs log data and query logic|
|"Alert when a VM hasn't sent a heartbeat for 10 minutes"|**Log search alert** on `Heartbeat`|Heartbeat is a log table|
|"Be told when Azure has an outage in my regions"|**Service Health alert**|Platform-wide incidents|
|"Be told when my specific VM becomes unavailable"|**Resource Health alert**|Your resource's health|
|"Threshold should adapt to normal daily patterns"|Metric alert with **dynamic thresholds**|ML-learned baseline|
|"Stop alert emails during Saturday maintenance"|**Alert processing rule** — suppress, scheduled|Alerts still fire and are logged; notifications don't|
|"Add the on-call action group to every alert in a subscription"|**Alert processing rule** — apply action group|No need to edit each alert rule|
|"Restart a service automatically when an alert fires"|Action group → **Automation runbook** (or Function/Logic App)|Notifications alone don't remediate|
|"SMS notifications are being dropped"|SMS rate limit — **1 per 5 minutes per number**; use webhook/Logic App for volume|Rate limiting|
|"Keep activity log data for 2 years and query it"|**Diagnostic setting** on the activity log → **Log Analytics** (set retention)|Activity log alone keeps 90 days|
|"Keep storage resource logs for 7 years cheaply"|Diagnostic setting → **storage account** (or workspace long-term retention)|Archive destination|
|"Send resource logs to a third-party SIEM in real time"|Diagnostic setting → **Event Hubs**|Streaming destination|
|"Collect Windows Application/System event logs from 50 VMs"|**DCR** + **AMA**, assigned at scale (or via Azure Policy)|One DCR, many VMs|
|"Chart blob transactions by response type"|Metrics explorer with **splitting** by ResponseType|Splitting = one line per value|
|"Compare capacity and availability of all storage accounts"|**Storage insights**|Multi-account view, no agent|
|"See performance of all VMs in one place"|**VM insights**|Curated multi-VM performance|
|"Continuously monitor latency from on-prem servers to an Azure VM"|**Connection monitor**|Continuous, hybrid, alerts|
|"Is port 3389 to this VM blocked, and by which rule?"|**IP flow verify**|Returns allow/deny + rule|
|"Back up Azure VMs, SQL in VMs and Azure Files"|**Recovery Services vault**|Those workloads live there|
|"Back up Azure Blobs, managed disks or PostgreSQL"|**Backup vault**|Newer workloads|
|"VM backups every 4 hours" or "back up a Trusted Launch VM"|**Enhanced** backup policy|Standard = once daily; Trusted Launch/Ultra need Enhanced|
|"Restore VMs in the paired region during an audit, any time"|**GRS** vault + **Cross Region Restore**|Plain GRS only allows secondary access after Microsoft declares a disaster|
|"Change vault from GRS to LRS — it's greyed out"|Items are already protected; **stop protection and delete data**, or use a new vault|Redundancy is fixed once backups exist|
|"Recover a few deleted files from a VM backup"|**File recovery** (iSCSI script, 12 hours)|No full restore needed|
|"Customise size/network before recreating a VM from backup"|**Restore disks** (+ ARM template)|Create new VM doesn't allow that customisation|
|"Restore in place keeping the same VM and IP"|**Replace existing**|Swaps disks on the original VM|
|"Fastest possible restore"|From an **instant restore snapshot**|No vault data transfer|
|"Someone deleted backup data yesterday"|**Undelete** within the **soft delete** window (14 days default)|Soft delete keeps it free|
|"Prevent a rogue admin disabling soft delete or deleting backups"|**MUA with Resource Guard** (+ immutable vault / always-on soft delete)|Needs a second approver|
|"Linux VM backups must be application-consistent"|**Pre- and post-scripts**|Linux defaults to file-system consistent|
|"Back up on-prem Windows files and folders directly to Azure"|**MARS agent**|No backup server needed|
|"Show backup storage used and failed jobs over 6 months"|Vault **diagnostic settings → Log Analytics**, then **Backup reports**|Reports are built on workspace data|
|"Get notified of backup job failures"|Built-in **Azure Monitor alerts** + **alert processing rule** + **action group**|Alerts already exist; route them|
|"VMs must keep running in another region if a region fails, RPO in minutes"|**Azure Site Recovery**|Backup RPO is hours|
|"Where should the ASR vault be?"|In the **target (secondary) region**|Still manageable when the source region is down|
|"Validate DR without affecting production"|**Test failover** into an **isolated VNet**, then cleanup|Replication continues|
|"Fail over with the least data loss"|Recovery point **Latest**|Processes all pending data|
|"Fail over as fast as possible"|Recovery point **Latest processed**|Lowest RTO|
|"After failover, replicate back to the original region"|**Commit**, then **Re-protect**|Reverses replication direction|
|"Several VMs must fail over in a defined order"|**Recovery plan** with boot groups|Orchestrated failover|

---
## 12. Portal Practice That Pays Off

- [ ] Open Metrics explorer on a VM and storage account; change aggregation, split and filter; pin to a dashboard
- [ ] Create a **diagnostic setting** for a storage account and for the activity log → Log Analytics
- [ ] Create a **DCR** collecting Windows event logs + memory counters; watch AMA install
- [ ] Run the KQL queries above (`Perf`, `Heartbeat`, `AzureActivity`, `Event`) and render a timechart
- [ ] Create a **metric alert** (CPU > 80%), a **log search alert** (heartbeat), and an **activity log alert** (VM delete), all using one **action group**
- [ ] Create an **alert processing rule** that suppresses notifications on a schedule
- [ ] Enable **VM insights** and open **Storage insights** and **Network insights**
- [ ] Create a **Connection monitor** test from a VM to an FQDN
- [ ] Create a **Recovery Services vault** (set redundancy first, enable CRR) and a **Backup vault**
- [ ] Create **Standard** and **Enhanced** VM backup policies; back up a VM; run **Backup now**
- [ ] Restore a VM using **Restore disks**, and recover a file with **File recovery**
- [ ] Delete a backup item and **undelete** it from soft delete
- [ ] Configure vault diagnostics and open **Backup reports**
- [ ] Enable **Site Recovery** for a VM to another region, run a **test failover**, clean up

---
## 13. One-Line Exam Facts

* Platform metrics: **93 days**; one chart shows **30 days** max
* Activity log: **90 days**, control plane, collected automatically
* Resource logs: data plane, need a **diagnostic setting**
* Diagnostic setting destinations: **Log Analytics, storage, Event Hubs, partner**; max **5** per resource
* Memory and in-guest disk space need **AMA + DCR**; Log Analytics agent retired **Aug 2024**
* Workspace retention default **30 days**; Analytics up to **2 years**; total up to **12 years**
* Metric = numbers, near real time; Log search = KQL; Activity log = control-plane events
* Service Health = Azure incidents; Resource Health = your resource
* Severity **0 (Critical) → 4 (Verbose)**
* Alert processing rules **suppress** or **add action groups**; alerts still fire
* SMS/voice: **1 per 5 minutes per number**; runbooks/webhooks not rate limited
* Fired alerts kept **30 days**; **100** activity log alert rules per subscription
* Storage insights = metrics, no agent; VM insights = AMA + DCR
* Connection monitor = **continuous**, hybrid, TCP/HTTP/ICMP, results in Log Analytics
* RSV: **VMs, SQL/SAP HANA in VMs, Azure Files, MARS/MABS/DPM, ASR**
* Backup vault: **Disks, Blobs, PostgreSQL, AKS**
* Vault in **same region** as the VMs; redundancy fixed once items are protected
* **CRR needs GRS**; restore in paired region any time
* Standard policy: **daily**, instant restore **1–5 days**; Enhanced: **every 4 h+**, **1–30 days**, Trusted Launch/Ultra
* Can't switch **Enhanced → Standard**
* Soft delete **14 days** default; enhanced **14–180 days**, always-on
* **MUA + Resource Guard** protects critical backup operations
* File recovery mounts via **iSCSI** for **12 hours**
* Linux app-consistent = **pre/post scripts**
* Backup reports need **vault diagnostic settings → Log Analytics**
* ASR vault in **target region**; Mobility service auto-installed; **cache storage account** in source region
* ASR crash-consistent every **5 min**; app-consistent min **1 h**; retention default **1 day**, max **15 days**
* Test failover → isolated VNet → **cleanup**; Failover → **Commit** → **Re-protect** → Failback
* **Latest processed** = lowest RTO; **Latest** = lowest RPO
