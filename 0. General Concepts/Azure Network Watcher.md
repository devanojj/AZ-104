**Connection troubleshoot** checks whether one Azure resource can connect to another and identifies where the connection is failing.

- Can test **TCP/ICMP** connectivity.
- Helps identify **NSG, routing, and connectivity problems**.
- Remember the direction: **VM1 → VM2**.
- Doesn't check reachability
- **NSG rules are stateful** — return traffic is automatically allowed when the initial connection is permitted.
- (Can these two things actually talk)


**Next hop** tool that to check the next routing hop (type, IP address, and route table ID) for traffic leaving a virtual machine. Next hop does not display routing decisions. (Where does this packet go next)


*Install AzureNetworkWatcherExtension*
*Use packet capture*

NSG flow logs, allows you to log information about IP traffic flowing through an NSG. 

Packet capture may help narrow down the scope of the issue, but it will not identify the specific NSG that prevents communication.