## 1. VM Monitoring Architecture & Data Tiers

Azure Monitor collects telemetry across four distinct layers of the virtual machine stack:

```text
Monitoring Data Tiers
├── 1. Host (Hypervisor Tier) ──► Agentless. Platform metrics (CPU, Disk IO, Network).
├── 2. Guest OS / Client Tier ──► Requires Azure Monitor Agent (AMA) + Data Collection Rule (DCR).
├── 3. Boot & Diagnostic Tier ──► Serial Console logs + OS boot screenshot (failed boot diagnostics).
└── 4. Subscription / Activity ──► Control plane operations (Start, Stop, Resize, RBAC assignments).
````

### Host vs. Guest OS Monitoring Comparison

|Feature / Dimension|Host-Level Metrics|Guest OS / Client-Level Telemetry|
|---|---|---|
|**Agent Requirement**|**Agentless** (built into hypervisor)|**Requires Agent** (Azure Monitor Agent - AMA)|
|**Data Scope**|Hypervisor view: CPU Percentage, Network In/Out, Disk Read/Write Bytes & Ops|In-OS view: Available Memory, Page file usage, logical disk free space, per-process stats|
|**Log Collection**|Activity Logs only|Windows Event Logs (`Application`, `System`), Linux `Syslog`, custom text logs|
|**Sampling & Retention**|1-minute frequency; retained for **93 days** in Azure Monitor Metrics|Configurable via DCR; ingested into **Log Analytics Workspace** (retained 30–730+ days)|
|**Cost**|Standard metric collection included at no charge|Ingestion and data retention billed per GB|

> [!important] Exam Core Distinction **Memory usage** (RAM consumed/available) and **logical disk free space**are **NOT** available from host metrics. You **must** install the Azure Monitor Agent (AMA) to collect memory and in-guest disk metrics.

## 2. Boot Diagnostics & Serial Console

- **Boot Diagnostics:** Captures serial console text logs and VM screen snapshots at startup to diagnose unbootable VMs (e.g., BSOD, kernel panic, corrupted bootloader).
    
- **Storage Options:**
    
    - **Managed Storage Account (Default / Recommended):** Azure-managed storage; zero configuration overhead.
        
    - **Custom Storage Account:** User-managed storage account; used when compliance requires customer-controlled storage keys.
        
- **Serial Console:** Provides direct, out-of-band interactive terminal/CMD access over COM1 port, independent of VM networking or OS health.
    

## 3. Azure Monitor Agent (AMA) & Data Collection Rules (DCR)

### Azure Monitor Agent (AMA)

- Unified agent replacing legacy agents (Log Analytics Agent / MMA, Telegraf, WAD, LAD).
    
- Installed as a VM extension (`AzureMonitorWindowsAgent` / `AzureMonitorLinuxAgent`).
    
- Authenticates securely using VM **System-Assigned or User-Assigned Managed Identity**.
    

### Data Collection Rules (DCR)

- Declarative JSON rules specifying:
    
    1. **Data Sources:** Performance counters, Windows event logs, or Linux syslogs to collect.
        
    2. **Data Sampling / Filter:** Frequency (e.g., 60s) and XPath event filters (`Application!*[System[(Level=1 or Level=2)]]`).
        
    3. **Destinations:** Where data streams (Log Analytics workspace, Azure Monitor Metrics, Event Hub).
        
- **Multi-homing:** A single VM can be associated with multiple DCRs; a single DCR can apply across multiple VMs via Azure Policy or direct association.
    

## 4. Azure Monitor VM Insights

Turn-key monitoring solution delivering pre-built performance dashboards, dependency mapping, and health diagnostics.

### Core Capabilities

- **Performance Tab:** Pre-configured charts for CPU, available memory, disk space, IOPS, and network throughput across individual VMs or entire fleets.
    
- **Map Tab (Dependency Agent):**
    
    - Discovers running processes and active inbound/outbound TCP connections.
        
    - Maps communication dependencies between VMs, servers, and external endpoints.
        
- **Processes Tab:** Real-time visibility into running processes, memory consumption, and network throughput per process.
    

## 5. Metrics Explorer & Data Visualization

- **Aggregation Types:** `Average`, `Minimum`, `Maximum`, `Total`, `Count`.
    
- **Splitting:** Segregates metrics across dimensions (e.g., split network traffic by Network Interface or disk IO by LUN ID).
    
- **Filtering:** Restricts chart display based on dimension attributes (e.g., display only `Drive C:` logical disk space).
    

## 6. Alerts & Action Groups

### Alert Types

|Alert Category|Evaluation Mechanism|Best Used For|
|---|---|---|
|**Metric Alerts**|Evaluates numeric time-series values against static/dynamic thresholds in near real-time (every 1 min).|High CPU (> 80%), Low Memory (< 10%), High Network In.|
|**Log Search Alerts (KQL)**|Runs scheduled KQL queries against Log Analytics tables (`Heartbeat`, `Event`, `Perf`, `Syslog`).|Specific error events (Event ID 4625), process crashes, heartbeat loss.|
|**Activity Log Alerts**|Triggers on subscription-level control-plane events.|VM deallocated, VM deleted, NSG modified.|

### Action Groups & Automation

- Reusable notification and automated response containers linked to alert rules.
    
- **Notifications:** Email, SMS, Azure Mobile App Push, Voice call.
    
- **Automated Actions:**
    
    - **Azure Automation Runbooks:** Remediate issues (e.g., restart a stopped service).
        
    - **Azure Functions / Logic Apps:** Trigger custom operational workflows.
        
    - **Webhooks / ITSM:** Create tickets in ServiceNow or send messages to Teams/Slack.
        

## 🎯 Exam Quick Reference (Cheat Sheet)

|Scenario / Exam Question Trigger|Correct Answer / Action|
|---|---|
|**Monitor VM CPU & Network without installing software**|Use **Host Platform Metrics** in Azure Monitor (Agentless).|
|**Monitor VM Available Memory / Free Disk Space**|Deploy **Azure Monitor Agent (AMA)** + create a **Data Collection Rule (DCR)**.|
|**Troubleshoot a VM stuck in "Starting" or black screen**|Check **Boot Diagnostics** (Screenshot and Serial Log).|
|**Map TCP dependencies and connected processes between VMs**|Enable **Azure Monitor VM Insights** (Performance + Map).|
|**Trigger alert when a specific Windows Event ID occurs**|Create a **Log Search Alert** with a KQL query on the `Event` table.|
|**Trigger alert when a VM is stopped/deallocated by a user**|Create an **Activity Log Alert** for `Microsoft.Compute/virtualMachines/deallocate/action`.|
|**Automate service restart upon alert firing**|Attach an **Action Group** configured with an **Azure Automation Runbook** or **Azure Function**.|
|**Legacy Agent Migration Path**|Replace Log Analytics Agent (MMA) with **Azure Monitor Agent (AMA)**.|