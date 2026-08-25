## 1. App Service Overview & Architecture
* **PaaS Compute:** Fully managed platform for hosting web apps, REST APIs, and mobile backends (.NET, Java, Node.js, PHP, Python).
* **Hosting Options:** Code deployment or custom Linux/Windows Docker containers.
* **Key Benefits:** Automatic OS/runtime patching, built-in high availability, global infrastructure, automated scaling, and integrated security.

---

## 2. Continuous Deployment & Sources
Azure App Service integrates with source control systems for continuous integration and automated deployment (CI/CD).
* **Automated CI/CD:** GitHub Actions, Azure DevOps Pipelines, Bitbucket.
* **Manual / Local Deployment:** Local Git, Cloud Git (GitHub/Bitbucket without CI), FTPS / FTP, Zip deploy / WAR deploy.
* **Deployment Center:** Central hub in Azure Portal to configure source control, branch mappings, and build providers.

---

## 3. Deployment Slots & Swapping (High Exam Priority)
Deployment slots are live apps with their own hostnames (e.g., `app-staging.azurewebsites.net`). Available in **Standard**, **Premium**, and **Isolated** tiers.

### Key Benefits
* **Zero Downtime:** Staging instance is warmed up before traffic is routed.
* **Instant Rollback:** Swap production and staging back if issues occur.
* **Testing in Production (Traffic Routing):** Route a percentage of live traffic to a specific slot.

### Swap Mechanism: What Swaps vs. What is Slot-Specific (Sticky)
| Swapped (Follows the Code) | NOT Swapped (Remains Sticky to the Slot) |
| :--- | :--- |
| **General App Settings** *(unless sticky)* | **Custom Domain Names** |
| **Connection Strings** *(unless sticky)* | **SSL / TLS Certificates & Bindings** |
| **Framework Version & Runtime Stack** | **Scale Settings (Autoscale rules & instance count)** |
| **Handler Mappings** | **IP Restrictions & Access Restrictions** |
| **Virtual Applications / Directories** | **Publishing Endpoints & Credentials** |
| **WebSockets Settings** | **Diagnostic Logging & WebJob Configuration** |

> [!important] Exam Rule: Sticky Setting Flag
> Any App Setting or Connection String can be made slot-specific by checking the **Deployment slot setting** (sticky) checkbox in the portal.

---

## 4. Custom Domains & SSL/TLS Configuration
Default URL: `<app-name>.azurewebsites.net`

### Custom Domain Verification
* **CNAME Record:** Maps subdomain (e.g., `www.contoso.com` $\rightarrow$ `contoso.azurewebsites.net`).
* **A Record + TXT Record:** Maps root domain / apex (e.g., `contoso.com` $\rightarrow$ App Service IP). Must create a `TXT` record (`asuid.contoso.com` containing Verification ID) to prove ownership.
* **Minimum Tier:** **Basic (B1)** for custom domain + SSL (Shared allows custom domain but no SSL).

### SSL/TLS Bindings
* **IP-based SSL:** Associates certificate with a dedicated public IP address (legacy, incurs extra cost).
* **SNI-based SSL (Server Name Indication):** Multiple hostnames share the same single IP address (standard, recommended).
* **Enforce HTTPS:** Set **HTTPS Only** to `On` to automatically redirect all unencrypted HTTP traffic (port 80 $\rightarrow$ 443).
* **Minimum TLS Version:** Recommended TLS 1.2+.

---

## 5. App Service Security & Authentication
* **Built-in Authentication (Easy Auth):** Turnkey authentication with zero code changes.
  * Identity Providers: **Microsoft Entra ID**, Google, Facebook, Apple, GitHub, Twitter/X, OpenID Connect.
  * Can restrict access to the whole app or pass claims to application code.
* **Network Security & Access Restrictions:**
  * **Inbound Access Restrictions:** Priority-based allow/deny rules based on IPv4/IPv6 ranges or Azure Virtual Network Service Endpoints.
  * **Private Endpoints:** Exposes the Web App securely via a private IP inside an Azure VNet.
  * **VNet Integration:** Enables the App Service to make **outbound** calls to resources inside an Azure VNet or on-premises over VPN/ExpressRoute.
* **App Service Authentication Scope:** Configurable independently per deployment slot.

---

## 6. Backup & Restore
* **Requirements:**
  * Minimum Tier: **Standard (S1)**, Premium, or Isolated.
  * Requires an **Azure Storage Account** (Blob container) with a configured SAS URL or Managed Identity.
* **What is Backed Up:**
  * App Configuration (App settings, connection strings).
  * File System Content (HTML, PHP, JS, binaries under `site/wwwroot`).
  * Connected SQL Database or Azure Database for MySQL (up to limits, if linked).
* **Limits:**
  * Max backup size: **10 GB** of app and database content.
  * Automated scheduled backups can run up to multiple times per day.
* **Restore:** Can restore to the original app, an existing staging slot, or create a brand-new app.

---

## 7. Azure Application Insights & Diagnostic Logging
* **Application Insights (APM):** Application performance monitoring; tracks request rates, response times, failure rates, live metrics, exceptions, and distributed tracing.
* **App Service Logs (Diagnostic Logging):**
  * **Application Logging:** Standard stdout/stderr or application code logs (stored to file system or Azure Blob storage).
    * *File system logs* turn off automatically after **12 hours** to preserve disk.
  * **Web Server Logging:** Detailed HTTP error messages, Failed Request Tracing (FREB), Web server logging (W3C format).

---

## 🎯 Exam Quick Reference (Cheat Sheet)

| Scenario / Exam Question | Correct Answer / Action |
| :--- | :--- |
| **Zero downtime deployment** | Use **Deployment Slots** (Standard/Premium tier); test on staging $\rightarrow$ Swap. |
| **Settings that stay on the slot during swap** | Custom domains, SSL certificates, Autoscale/scale settings, IP restrictions. |
| **Verify root domain (`contoso.com`)** | Create **A record** (points to IP) + **TXT record** (domain verification ID). |
| **Verify subdomain (`www.contoso.com`)** | Create **CNAME record** pointing to `<app-name>.azurewebsites.net`. |
| **Redirect all HTTP traffic to HTTPS** | Enable **HTTPS Only** setting in App Service Configuration. |
| **Authenticate users without changing code** | Enable **App Service Authentication (Easy Auth)** with Microsoft Entra ID. |
| **Web App needs to access private VM in VNet** | Configure **VNet Integration** (outbound). |
| **Restrict Web App to be accessible only from internal VNet** | Configure **Private Endpoint** or **Access Restrictions** (inbound). |
| **Minimum Tier for Backup & Restore** | **Standard (S1)** tier + Azure Storage Account container. |
| **App Service file system logs turned on** | Automatically disable after **12 hours** (Blob storage logs persist). |