

```
VM
 │
 │ VNet → Azure backbone
 ▼
Storage Account
(public service endpoint)
```

- Traffic stays on the **Azure backbone**.
- The Storage Account is still accessed through its **public endpoint**.
- You restrict which **VNet/subnets** can access it using network rules.
- The Storage Account **doesn't get a private IP in your VNet**.