A `ReadOnly` resource lock applied to a resource group prevents modifications to the **management plane**, but it does not prevent operations on the **data plane**.

- **Management plane:** Creating, deleting, or modifying Azure resources is blocked.
- **Data plane:** Reading, uploading, modifying, or deleting data inside supported services is not blocked by the resource lock.