**Connection troubleshoot** checks whether one Azure resource can connect to another and identifies where the connection is failing.

- Can test **TCP/ICMP** connectivity.
- Helps identify **NSG, routing, and connectivity problems**.
- Remember the direction: **VM1 → VM2**.
- Doesn't check reachability
- **NSG rules are stateful** — return traffic is automatically allowed when the initial connection is permitted.


**Next hop** tool that to check the next routing hop (type, IP address, and route table ID) for traffic leaving a virtual machine. Next hop does not display routing decisions.


*Install AzureNetworkWatcherExtension*
*Use packet capture*

NSG flow logs, allows you to log information about IP traffic flowing through an NSG. 

