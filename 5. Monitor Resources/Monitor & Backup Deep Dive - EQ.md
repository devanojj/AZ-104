Practice questions for [[Monitor & Backup Deep Dive]]. Sections mirror that note so you can jump back to whatever you miss.

---
## 1. Azure Monitor Data Types

**Which Azure Monitor data records control-plane operations such as who deleted a VM?**
- Resource logs
- *The activity log (correct)*
- Platform metrics

**Which data must you enable with a diagnostic setting before it's collected?**
- *Resource logs (correct)*
- Platform metrics
- The activity log

**You need to see each read operation against blobs in a storage account. Which data type contains this?**
- Platform metrics
- The activity log
- *Resource logs (correct)*

**How long are platform metrics retained in Azure Monitor Metrics?**
- 30 days
- *93 days (correct)*
- 90 days

**How long is the activity log retained by default?**
- 30 days
- 93 days
- *90 days (correct)*

**You need to monitor available memory inside a Windows VM. The metric doesn't appear among the VM's platform metrics. What should you do?**
- *Install the Azure Monitor Agent and associate a data collection rule (correct)*
- Enable boot diagnostics
- Increase the VM size

**Which agent should be used to collect guest OS data from Azure VMs?**
- *The Azure Monitor Agent (correct)*
- The Log Analytics agent (MMA)
- The Dependency agent

---
## 2. Metrics

**In Metrics explorer, you want to view one line for each response type of a storage account's Transactions metric. What should you use?**
- Filtering
- A different aggregation
- *Splitting (correct)*

**You want a chart to show only throttled storage requests. What should you use?**
- Splitting by ResponseType
- A Sum aggregation
- *A filter on ResponseType (correct)*

**Which aggregation is most appropriate for total bytes sent over a VM's network interface in each interval?**
- *Sum (correct)*
- Average
- Minimum

**You select a time range of 60 days in Metrics explorer and the chart is blank. Why?**
- *A single chart can show at most 30 days; pan to view older data (correct)*
- Metrics are only kept for 30 days
- The VM must be running

**You need to analyse VM metrics from 6 months ago. What should you have configured?**
- *A diagnostic setting sending platform metrics to a Log Analytics workspace (correct)*
- A longer metric retention in Metrics explorer
- A workbook

---
## 3. Log Settings

**Which destination should you choose to query resource logs with KQL and create log alerts?**
- A storage account
- An event hub
- *A Log Analytics workspace (correct)*

**Resource logs must be streamed in near real time to a third-party SIEM. Which destination should you use?**
- A Log Analytics workspace
- A storage account
- *An event hub (correct)*

**Logs must be archived cheaply for seven years for compliance, without frequent querying. Which destination is most cost-effective?**
- An event hub
- *A storage account (correct)*
- Azure Monitor Metrics

**What is the maximum number of diagnostic settings per resource?**
- *5 (correct)*
- 1
- 10

**You need to keep the activity log for two years and query it with KQL. What should you do?**
- Increase the activity log retention to two years
- Export the activity log to CSV monthly
- *Create a diagnostic setting for the activity log that sends it to a Log Analytics workspace with sufficient retention (correct)*

**What is the default data retention of a Log Analytics workspace?**
- *30 days (correct)*
- 7 days
- 90 days

**What is the longest total retention you can configure for a table in a Log Analytics workspace?**
- *12 years (correct)*
- 2 years
- 7 years

**You need to collect Windows Application and System event logs from 50 VMs consistently. What should you create?**
- A diagnostic setting on each VM
- A workbook
- *A data collection rule associated with the VMs (correct)*

**How does the Azure Monitor Agent authenticate to Azure Monitor?**
- With a workspace ID and key
- *With a managed identity (correct)*
- With a storage account SAS token

**Which statement about data collection rules is correct?**
- Each VM can be associated with only one DCR
- *A VM can have multiple DCRs and a DCR can apply to multiple VMs (correct)*
- Each DCR can be associated with only one VM

**Where in a Log Analytics workspace can you see a breakdown of data ingestion costs?**
- Agents
- Tables
- *Usage and estimated costs (correct)*

---
## 4. Querying Logs (KQL)

**Which KQL operator filters rows?**
- *where (correct)*
- project
- summarize

**Which KQL operator selects or renames the columns to return?**
- *project (correct)*
- extend
- where

**You need the count of events per computer. Which operator should you use?**
- *summarize (correct)*
- extend
- project

**What does `bin(TimeGenerated, 15m)` do in a query?**
- Filters data to the last 15 minutes
- *Groups records into 15-minute time buckets (correct)*
- Deletes records older than 15 minutes

**Which table should you query to find VMs that have stopped reporting to the workspace?**
- Perf
- *Heartbeat (correct)*
- Event

**Which table contains activity log records sent to a Log Analytics workspace?**
- AzureDiagnostics
- *AzureActivity (correct)*
- Syslog

**Which table contains Windows event log records collected by the Azure Monitor Agent?**
- Syslog
- *Event (correct)*
- Heartbeat

**Which operator displays query results as a line chart over time?**
- summarize
- *render timechart (correct)*
- top

---
## 5. Alerts

**You need an alert when a VM's average CPU exceeds 80% for 5 minutes. Which alert type should you use?**
- Activity log alert
- *Metric alert (correct)*
- Log search alert

**You need an alert whenever someone deletes a virtual machine. Which alert type should you use?**
- Metric alert
- *Activity log alert (correct)*
- Resource Health alert

**You need an alert when Windows event ID 4625 is logged more than 10 times in 5 minutes. Which alert type should you use?**
- Metric alert
- Activity log alert
- *Log search alert (correct)*

**You need to be notified when an Azure service outage affects the regions you use. What should you configure?**
- A Resource Health alert
- *A Service Health alert (correct)*
- A metric alert on each resource

**You need to be notified when a specific VM becomes unavailable because of a platform problem. What should you configure?**
- *A Resource Health alert (correct)*
- A Service Health alert
- A log search alert on Heartbeat

**Web traffic varies by time of day, and a fixed CPU threshold causes false alerts. What should you use?**
- A log search alert
- An alert processing rule
- *A metric alert with dynamic thresholds (correct)*

**Which alert severity is the most critical?**
- Sev 4
- Sev 1
- *Sev 0 (correct)*

**What does an action group define?**
- *Who is notified and which automated actions run when an alert fires (correct)*
- The condition that fires an alert
- When notifications are suppressed

**When an alert fires, a Windows service must be restarted automatically. What should the action group include?**
- An email to the operations team
- *An Automation runbook (or Azure Function/Logic App) action (correct)*
- An SMS notification

**Alert notifications must go to everyone with the Owner role on the subscription, even as membership changes. Which notification type should you use?**
- Email to a distribution list
- *Email Azure Resource Manager role (correct)*
- SMS to each Owner

**An on-call engineer misses several SMS alerts sent in quick succession. What is the most likely reason?**
- *SMS notifications are rate limited to one every five minutes per phone number (correct)*
- SMS isn't supported in action groups
- The alert rule was disabled

**Notifications must be suppressed every Saturday from 22:00 to 06:00 for a resource group under maintenance. What should you create?**
- *An alert processing rule with a recurring schedule that suppresses notifications (correct)*
- A new alert rule with a schedule
- A disabled action group

**During a suppression window from an alert processing rule, what happens to alerts?**
- They aren't created
- They're deleted after the window
- *They still fire and are recorded, but notifications and actions are suppressed (correct)*

**You need to add a new on-call action group to every alert in a subscription without editing each alert rule. What should you use?**
- Azure Policy
- A workbook
- *An alert processing rule that applies the action group (correct)*

**Which alert state is set by users rather than by the system?**
- Fired
- Resolved
- *Acknowledged (correct)*

**How long are fired alerts retained?**
- 7 days
- 90 days
- *30 days (correct)*

**You need 150 activity log alert rules in one subscription. What should you do?**
- Request a quota increase
- *Send the activity log to Log Analytics and use log search alerts, because the 100-rule limit can't be raised (correct)*
- Create them across multiple resource groups

---
## 6. Azure Monitor Insights

**You need a single view of capacity, availability and transactions across all storage accounts in a subscription. What should you use?**
- *Storage insights (correct)*
- VM insights
- Metrics explorer for each account

**What is required for Storage insights?**
- The Azure Monitor Agent
- A diagnostic setting on each account
- *Nothing extra; it's based on platform metrics (correct)*

**You need to compare guest CPU, memory and disk performance across many VMs in one view. What should you enable?**
- Storage insights
- *VM insights (correct)*
- Boot diagnostics

**What does VM insights require on each VM?**
- *The Azure Monitor Agent with a VM insights data collection rule (correct)*
- The Log Analytics agent
- Boot diagnostics

**You need to visualise the topology and health of network resources and connectivity tests. What should you use?**
- Storage insights
- *Network insights (correct)*
- Application Insights

**A VM shows a black screen and won't boot. Which tool should you use first?**
- VM insights
- *Boot diagnostics (screenshot and serial log) (correct)*
- Connection monitor

**You need an interactive report combining KQL queries, metrics charts and parameters for your team. What should you create?**
- *A workbook (correct)*
- An alert processing rule
- An action group

---
## 7. Network Watcher & Connection Monitor

**You need continuous monitoring of latency and reachability between on-premises servers and an Azure VM, with alerts. What should you use?**
- IP flow verify
- *Connection monitor (correct)*
- Connection troubleshoot

**Which protocols can a Connection monitor test configuration use?**
- *TCP, HTTP and ICMP (correct)*
- RDP and SSH
- UDP and GRE

**What must be installed on an Azure VM used as a Connection monitor source?**
- *The Network Watcher Agent extension (correct)*
- The Dependency agent
- The VM backup extension

**Where are Connection monitor results stored for analysis and alerting?**
- A storage account
- *A Log Analytics workspace (correct)*
- An event hub

**You need a one-time test to see whether a VM can reach a database on port 1433, including the hops and any NSG fault. What should you use?**
- *Connection troubleshoot (correct)*
- Connection monitor
- Topology

**You need to know which NSG rule is blocking inbound RDP to a VM. What should you use?**
- Next hop
- *IP flow verify (correct)*
- Packet capture

---
## 8. Azure Backup

**Which vault type should you create to back up Azure VMs?**
- Backup vault
- *Recovery Services vault (correct)*
- Key Vault

**Which vault type should you create to back up Azure Blobs and managed disks?**
- Recovery Services vault
- *Backup vault (correct)*
- Storage account

**You need to back up Azure Files shares and SQL Server databases running in Azure VMs. Which vault should you use?**
- Backup vault
- *Recovery Services vault (correct)*
- Two Backup vaults, one per workload

**You need to back up an Azure Database for PostgreSQL server. Which vault should you use?**
- Recovery Services vault
- Site Recovery vault
- *Backup vault (correct)*

**A Recovery Services vault is in East US. Which VMs can you configure for backup to it?**
- *VMs in East US (correct)*
- VMs in any region
- VMs in East US and its paired region

**You want to change a Recovery Services vault's storage redundancy from GRS to LRS, but the setting is greyed out. Why?**
- LRS isn't supported for Recovery Services vaults
- Only Owners can change redundancy
- *Items are already protected; redundancy can only be changed before backups exist (correct)*

**You need to restore Azure VMs in the paired secondary region at any time, for example during an audit. What must be configured?**
- *A GRS vault with Cross Region Restore enabled (correct)*
- An LRS vault with Site Recovery
- A second vault in the paired region

**What is the default storage redundancy for a new Recovery Services vault?**
- LRS
- *GRS (correct)*
- ZRS

**VMs must be backed up every 4 hours. Which policy type should you use?**
- *Enhanced (correct)*
- Standard
- An on-demand backup every 4 hours

**You need to back up a Trusted Launch VM from the Azure portal. Which policy type should you use?**
- Standard
- Archive
- *Enhanced (correct)*

**What is the instant restore snapshot retention range for a Standard VM backup policy?**
- 1–30 days
- 7–90 days
- *1–5 days (correct)*

**A VM is protected with an Enhanced policy. Can you switch it back to a Standard policy?**
- *No, a VM using an Enhanced policy can't move to a Standard policy (correct)*
- Yes, by editing the policy type
- Yes, after the next backup completes

**Which backup tier provides the fastest restore?**
- Vault-Standard
- *Snapshot (instant restore) (correct)*
- Vault-Archive

**Monthly and yearly recovery points must be kept for 10 years at the lowest cost. Where should they be stored?**
- The snapshot tier
- Vault-Standard
- *Vault-Archive (correct)*

**You need to restore a VM from backup but first change its size and virtual network. Which restore option should you choose?**
- *Restore disks (with the generated ARM template) (correct)*
- Create new VM
- Replace existing

**A VM's OS is corrupted. You want to restore it in place, keeping the same VM, NIC and IP. Which option should you choose?**
- Create new VM
- Restore disks
- *Replace existing (correct)*

**A user deleted a few files from a VM yesterday. What is the quickest way to recover them from backup?**
- Restore the whole VM
- Restore disks and attach them
- *File recovery, which mounts the recovery point as drives (correct)*

**How long does the mounted recovery point from file recovery remain available?**
- 1 hour
- *12 hours (correct)*
- 24 hours

**Linux VM backups must be application-consistent. What should you configure?**
- An Enhanced policy
- VSS on the Linux VM
- *Pre-scripts and post-scripts (correct)*

**A VM is stopped (deallocated) when its scheduled backup runs. What happens?**
- *The VM is backed up with a crash-consistent recovery point (correct)*
- The backup is skipped
- The VM is started for the backup

**Backup data for a VM was deleted by mistake two days ago. What should you do?**
- Restore from the paired region
- *Undelete it from the soft-deleted items (correct)*
- Open a support ticket

**What is the default soft delete retention for Azure Backup?**
- 7 days
- 30 days
- *14 days (correct)*

**You must stop a compromised admin account from disabling soft delete or reducing backup retention. What should you configure?**
- *Multi-user authorisation with a Resource Guard owned by a different admin (correct)*
- A CanNotDelete lock on the vault
- Azure Policy with a Deny effect

**You need recovery points that can't be deleted before they expire, and this protection must be irreversible. What should you configure?**
- Soft delete
- Cross Region Restore
- *A locked immutable vault (correct)*

**You need to back up files and folders from an on-premises Windows server directly to Azure, without a backup server. What should you use?**
- MABS
- The VM backup extension
- *The MARS agent (correct)*

**What gets installed on an Azure VM when you enable Azure Backup?**
- The MARS agent
- *The VM backup extension (correct)*
- The Azure Monitor Agent

**SQL Server databases in Azure VMs must support point-in-time restore with very low data loss. What does Azure Backup provide?**
- Daily full backups only
- *Log backups as often as every 15 minutes (correct)*
- VM snapshots every 4 hours

**What is the maximum number of VMs that can be assigned to one backup policy?**
- 50
- *100 (correct)*
- 1,000

**Management wants reports of backup storage consumption and failed jobs over the last six months. What must you configure first?**
- An alert processing rule
- A Cross Region Restore vault
- *Diagnostic settings on the vaults that send data to a Log Analytics workspace (correct)*

**You configured backup reports an hour ago and they show no data. What is the most likely reason?**
- Reports only work for Backup vaults
- The vault must use GRS
- *The initial data push can take up to 24 hours (correct)*

**You need email notifications when backup jobs fail. Built-in backup alerts are already firing. What should you configure?**
- A new metric alert rule for each VM
- A log search alert on the vault
- *An alert processing rule that routes the alerts to an action group (correct)*

**Where can you manage backups and protection status across subscriptions and regions in one place?**
- *Resiliency (formerly Backup center) (correct)*
- Azure Advisor
- Azure Policy

---
## 9. Azure Site Recovery

**VMs must keep running in another region during a regional outage, with an RPO measured in minutes. What should you use?**
- *Azure Site Recovery (correct)*
- Azure Backup with an Enhanced policy
- Cross Region Restore

**Where should you create the Recovery Services vault for replicating Azure VMs with Site Recovery?**
- *In the target (secondary) region (correct)*
- In the source region
- In any third region

**What does Site Recovery install on Azure VMs when you enable replication?**
- The MARS agent
- *The Mobility service extension (correct)*
- The Azure Monitor Agent

**Where does Site Recovery first write replicated disk changes for an Azure VM?**
- *A cache storage account in the source region (correct)*
- Directly to the replica managed disks
- The Recovery Services vault

**How often does Site Recovery create crash-consistent recovery points for Azure VMs?**
- Every hour
- Every 15 minutes
- *Every 5 minutes (correct)*

**What is the minimum frequency for app-consistent snapshots in Site Recovery?**
- 5 minutes
- *1 hour (correct)*
- 15 minutes

**What are the default replication policy settings for Azure-to-Azure Site Recovery?**
- 15 days retention, app-consistent every 4 hours
- *1 day retention, app-consistent snapshots off (correct)*
- 7 days retention, app-consistent every hour

**You need to validate disaster recovery without affecting production or replication. What should you do?**
- *Run a test failover into an isolated, non-production virtual network (correct)*
- Run a planned failover to the target region
- Pause replication and start the replica disks

**After a test failover is validated, what should you do?**
- *Clean up the test failover (correct)*
- Commit the failover
- Re-protect the VM

**You must fail over with the lowest possible data loss. Which recovery point should you choose?**
- Latest processed
- Latest app-consistent
- *Latest (correct)*

**You must fail over as quickly as possible. Which recovery point should you choose?**
- Latest
- Custom
- *Latest processed (correct)*

**After a failover to the secondary region, which step starts replicating the VMs back to the primary region?**
- Test failover
- Cleanup
- *Re-protect (correct)*

**What is the correct order of operations for failing over to the secondary region and later returning?**
- *Failover, commit, re-protect, then failback (correct)*
- Re-protect, failover, commit, failback
- Failover, failback, commit, re-protect

**A three-tier app must fail over with the database first, then the app tier, then the web tier. What should you configure?**
- A replication policy per tier
- *A recovery plan with ordered boot groups (correct)*
- Multi-VM consistency

**Which statement best distinguishes Azure Backup from Azure Site Recovery?**
- Backup replicates VMs continuously; Site Recovery keeps long-term recovery points
- Both provide the same RPO
- *Backup restores data from recovery points; Site Recovery fails over workloads to another region (correct)*
