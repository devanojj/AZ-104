*Domain Name System*
Maps IP address to domain name

*SOA - State of authority*
Contains authoritative information about DNS 
Primary DNS / Zone Administrator / Serial Number

*NS - Name Server*
Shows DNS server for zones, (handles questions for DNS)
gatehouse.com 

*A - Address*
Maps domain name to IPv4 address
gatehouse.systems $\rightarrow$ 54.33.177.89


To create a subdomain, create NS record in the "gatehouse.com" for example put "ticket" for "ticket.gatehouse.com"


### Azure-provided name resolution
- **Built-in DNS** provided by Azure.
- Automatically available to VMs/resources in a **VNet**.


### Azure Private DNS
- Used for **private DNS zones** within Azure.
- Resolves **private IP addresses**.
- For private name resolution between Azure virtual networks

*Azure public DNS provides DNS for public access, such as name resolution for a publicly accessible website.* 








