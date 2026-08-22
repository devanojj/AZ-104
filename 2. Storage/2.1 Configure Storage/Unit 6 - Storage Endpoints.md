Each Azure Storage service has a unique **endpoint URL** based on the storage account name.

---
#### Default Endpoints
For storage account `mystorageaccount`:

| Service   | Endpoint                                  |
| --------- | ----------------------------------------- |
| **Blob**  | `mystorageaccount.blob.core.windows.net`  |
| **Table** | `mystorageaccount.table.core.windows.net` |
| **Queue** | `mystorageaccount.queue.core.windows.net` |
| **File**  | `mystorageaccount.file.core.windows.net`  |

**Object URL:**

```
<endpoint>/<container>/<object>
```

Example:

```
mystorageaccount.blob.core.windows.net/mycontainer/myblob
```
---
#### Custom Domains
- Can map a custom domain to **Blob Storage**
- Example: `blobs.contoso.com`
- Uses a DNS **CNAME** record
- CNAME points the custom subdomain to the storage account's Blob endpoint

**Exam Tip**
- **Custom domain → CNAME → Storage endpoint**