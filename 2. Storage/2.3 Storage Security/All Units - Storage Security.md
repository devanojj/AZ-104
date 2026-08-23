## 1. Storage Security Overview
* **Identity & Access:** Microsoft Entra ID (RBAC), Account Access Keys (Shared Key), Shared Access Signatures (SAS).
* **Encryption at Rest:** Storage Service Encryption (SSE) enabled by default (256-bit AES).
* **Encryption in Transit:** Enforce HTTPS via *Secure transfer required*, TLS 1.2 minimum.
* **Network Isolation:** Storage Firewalls, Virtual Network Service Endpoints, and Private Endpoints.

---

## 2. Storage Account Access Keys & Key Rotation
* **Account Keys:** Two 512-bit symmetric keys (`key1`, `key2`) providing root administrative access to data and control planes.
* **Key Rotation Workflow:**
  1. Switch applications to `key2`.
  2. Regenerate `key1`.
  3. Update applications to use `key1`.
  4. Regenerate `key2`.
* **Azure Key Vault:** Integrates to automate key rotation and secret storage.

> [!warning] Security Risk
> Account keys bypass least privilege. Prefer Microsoft Entra ID or SAS tokens.

---

## 3. Shared Access Signatures (SAS)
A signed URI granting delegated, fine-grained, time-limited access without exposing account keys.

### SAS Types
* **User Delegation SAS:** Secured with **Microsoft Entra ID** credentials (recommended for Blob/Queue; no account keys used).
* **Service SAS:** Secured with **Storage Account Key**; grants access to a single service (Blob, Queue, Table, or File).
* **Account SAS:** Secured with **Storage Account Key**; grants access across multiple services or account-level operations.

### Stored Access Policy
* Groups SAS parameters server-side for Service SAS on Blob containers, File shares, Queues, and Tables.
* **Key Advantage:** Allows altering permissions/expiry or **revoking access immediately** without rotating account keys.

---

## 4. URI Structure & SAS Parameters

```text
https://<storage-account>.blob.core.windows.net/<container>/<blob>?<sas-token>
````

|Parameter|Name|Description / Accepted Values|
|---|---|---|
|`sp`|Signed Permissions|`r` (read), `w` (write), `d` (delete), `l` (list), `a` (add), `c` (create)|
|`st`|Signed Start Time|UTC start time|
|`se`|Signed Expiry Time|UTC expiration time|
|`sip`|Signed IP Range|Allowed client IP / CIDR range|
|`spr`|Signed Protocol|`https` or `https,http` (always enforce `https`)|
|`sv`|Signed Version|Storage service API version|
|`sr`|Signed Resource|Target type: `b` (blob), `c` (container), `bs` (blob snapshot)|
|`sig`|Signature|HMAC-SHA256 signature hash|
|`si`|Stored Policy ID|References a Stored Access Policy name|

## 5. Storage Encryption

### Encryption at Rest (SSE)

- Automatically enabled for all storage services using **256-bit AES**.
    
- Cannot be disabled.
    

### Key Management Options

- **Microsoft-Managed Keys (MMK):** Default key management handled by Azure.
    
- **Customer-Managed Keys (CMK):**
    
    - Stored in **Azure Key Vault** or **Key Vault Managed HSM**.
        
    - Gives complete control over key creation, rotation, and access revocation.
        
    - Key Vault requires **Soft Delete** and **Purge Protection** enabled.
        
    - Storage account connects via **System-Assigned** or **User-Assigned Managed Identity**.
        

### Encryption Scopes (Blob Storage)

- Enforce distinct encryption settings at the **container** or **individual blob** level within the same storage account.
    
- Supports both MMK and CMK per scope.
    

### Infrastructure Encryption

- Applies a second independent layer of 256-bit AES encryption at the infrastructure/hardware level (Double Encryption).
    

## 6. Exam Tips & Best Practices

- **Identity Priority:** Use Microsoft Entra ID authorization where possible; block Shared Key access (`allowSharedKeyAccess = false`).
    
- **SAS Revocation:** Always bind Service SAS to a **Stored Access Policy** for instant revocation capability.
    
- **Network Hardening:** Restrict default network access to _Selected networks_ and use Private Endpoints.
    
- **Transport Security:** Reject unencrypted HTTP traffic and enforce minimum TLS 1.2.