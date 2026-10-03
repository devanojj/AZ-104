*Count Heart Beats*
Heartbeat
| where TimeGenerated > ago(30m)
| where Computer =~ "az104-vm0"
| summarize HeartbeatCount = count(), LastHeartbeat = max(TimeGenerated)
    by Computer, Category