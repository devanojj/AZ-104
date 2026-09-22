
|               | **Azure Front Door**                             | **Azure ExpressRoute**                        |
| ------------- | ------------------------------------------------ | --------------------------------------------- |
| Main purpose  | Improve **web application access**               | Connect **on-premises networks to Azure**     |
| Type          | Global Layer 7 service                           | Private network connection                    |
| Traffic       | Internet → Azure/web app                         | On-premises ↔ Azure                           |
| Connection    | Uses the **public internet**                     | Uses a **private dedicated connection**       |
| Main features | CDN, global load balancing, WAF, TLS termination | Private connectivity, predictable performance |
| Example       | Users worldwide accessing `mycompany.com`        | Company HQ connecting to Azure VNet           |
| OSI focus     | **Layer 7 (HTTP/HTTPS)**                         | Primarily **Layer 3 (IP routing)**            |


### Exam tip
If the question says:

- **Global web application / HTTP / HTTPS / WAF / load balancing** → **Azure Front Door**
- **On-premises to Azure / private connection / dedicated circuit / no public internet** → **ExpressRoute**

Also, **ExpressRoute is not a VPN**. An Azure **VPN Gateway** creates an encrypted connection over the internet, whereas ExpressRoute provides a private connection through a connectivity provider.