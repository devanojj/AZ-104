
*Distribution modes*

| Hash Based       |
| ---------------- |
| Source IP        |
| Source Port      |
| Destination Port |
| Destination IP   |
| Protocol Type    |

*Hash Based*
Traffic to healthy backend, only persistent connection during sessions 
New sessions creates new backend instance 


*Client IP 2 Tuple*
Traffic from same IP routed to the same backend 

*Client IP + Protocol 3 Tuple*
Traffic from same IP + Protocol going to the same backend 



