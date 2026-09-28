
```
VM
 │
 │ VNet
 ▼
Private IP: 10.0.1.5
 │
 ▼
Storage Account
```

- Azure creates a **private IP address inside your VNet**.
- Your VM accesses the Azure service through that private IP.
- The service can be accessed privately from your VNet.
- You can disable the service's public network access if required.