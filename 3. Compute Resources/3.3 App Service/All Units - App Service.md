An **App Service Plan (ASP)** represents the set of compute resources (VM instances) allocated to run one or more Azure Web Apps, API Apps, Mobile Apps, or Function Apps.

### Key Plan Attributes
* **Region:** Datacenter location hosting the compute VMs.
* **Operating System:** Windows or Linux.
* **Instance Size:** Number and size of VM compute instances (CPU cores, RAM, local storage).
* **Pricing Tier:** Determines available PaaS features, limits, and scaling capabilities.

> [!important] Exam Note: Multi-App Density & Resource Contention
> All apps assigned to the same App Service Plan share the same compute instances and storage. If one app experiences high CPU/memory usage, it impacts performance for all other apps in that plan.

---

## 2. App Service Plan Pricing Tiers

| Tier Category | Available Tiers | Compute / Hosting Type | Max Scale-Out | Key Features & Exam Capabilities |
| :--- | :--- | :--- | :--- | :--- |
| **Shared Compute** | **Free (F1), Shared (D1)** | Multi-tenant shared VMs | 1 instance (No scale) | 1 GB storage, 60 CPU min/day (Free) / 240 CPU min/day (Shared). Custom domains allowed on D1 only. No SSL, no SLA. |
| **Dedicated Compute (Basic)** | **Basic (B1–B3)** | Dedicated multi-tenant VMs | Up to 3 instances | Custom domains, SSL certificates, manual scale only. No deployment slots, no autoscale, no traffic routing. |
| **Dedicated Compute (Standard)** | **Standard (S1–S3)** | Dedicated multi-tenant VMs | Up to 10 instances | **Production baseline:** Autoscale, **5 Deployment Slots**, daily backups, custom domains & SSL, 99.95% SLA. |
| **Dedicated Compute (Premium)** | **Premium v2 / v3 (P1v3–P3v3)** | Dedicated modern hardware (SSD) | Up to 30 instances | **20 Deployment Slots**, autoscale, VNet Integration, Private Endpoints, **Zone Redundancy (multi-AZ)**. |
| **Isolated Compute** | **Isolated v2 (I1v2–I6v2)** | Dedicated private hardware (ASE) | Up to 100 instances | Runs in an **App Service Environment (ASE)**; complete network isolation, direct VNet injection, maximum scale and compliance. |

---

## 3. Scaling Options: Scale Up vs. Scale Out

### Vertical Scaling (Scale Up / Down)
* **Action:** Change the hardware tier/pricing SKU (e.g., upgrading from `Basic B1` $\rightarrow$ `Premium P1v3`).
* **Effect:** Adds more CPU cores, RAM, disk space, or unlocks features (Slots, VNet Integration).
* **Impact:** May cause brief connection interruptions during VM reconfiguration.

### Horizontal Scaling (Scale Out / In)
* **Action:** Increase or decrease the number of identical VM instances serving the plan.
* **Effect:** Distributes incoming HTTP/HTTPS traffic across multiple worker instances behind Azure's built-in Layer 7 load balancer.
* **Impact:** Zero application downtime.

---

## 4. Autoscale Rules & Triggers

Autoscale automatically adjusts instance counts to meet workload demand and minimize idle costs.

### Autoscale Dimensions
1. **Metric-Based Scaling:** Scales dynamically based on resource telemetry (CPU Percentage, Memory Percentage, HTTP Queue Length, Disk Queue, Data In/Out).
2. **Schedule-Based Scaling:** Scales to specific instance counts during fixed time ranges (e.g., scale out to 10 instances on Monday at 08:00 UTC, scale in on Friday at 18:00 UTC).

### Autoscale Rule Architecture
* **Instance Limits:**
  * `Minimum:` Lowest baseline instances (always running).
  * `Maximum:` Ceiling limit (budget protection).
  * `Default:` Fallback capacity if metric telemetry fails.
* **Rule Components:**
  * **Metric Aggregation:** Average, Minimum, Maximum, Total over a duration (time grain: 5–10 mins).
  * **Threshold Operator:** Greater than (`>`), Less than (`<`), etc.
  * **Cool-down Period:** Cooldown window (e.g., 5–10 minutes) before the engine evaluates another scale event (prevents **flapping/thrashing**).

> [!tip] Exam Rule: Paired Scale Rules
> Always define both **Scale-Out** and **Scale-In** rules with appropriate buffer between thresholds (e.g., Scale Out when CPU > 75%, Scale In when CPU < 25%) to prevent rapid oscillation.

---

## 5. High Availability & Zone Redundancy

* **Availability Zones (Multi-AZ):** Premium (v2/v3) and Isolated v2 plans support zone redundancy across 3 availability zones in supported regions.
* **Minimum Instances:** Enabling zone redundancy requires a minimum of **2 worker instances** (3 instances recommended for balanced distribution across all 3 zones).
* **Stamp / Scale Unit Constraint:** If the underlying infrastructure scale unit doesn't support zones, you must create a new Resource Group and deploy the plan with zone redundancy enabled at creation.

---

## 6. Exam Quick Reference (Cheat Sheet)

| Concept | Key Point / Exam Trigger |
| :--- | :--- |
| **Minimum Tier for Autoscale** | **Standard (S1)** tier (Basic only supports manual scale). |
| **Minimum Tier for Deployment Slots** | **Standard (S1)** tier (up to 5 slots) or Premium (up to 20 slots). |
| **Minimum Tier for Custom Domain + SSL** | **Basic (B1)** tier (Shared allows custom domain, but no SSL bindings). |
| **App Service Environment (ASE)** | Requires **Isolated (I-series)** tier; dedicated hypervisor and direct VNet injection. |
| **Shared Plan Resource Contention** | Multiple apps in 1 ASP share CPU/RAM limits; separate ASPs to isolate critical workloads. |
| **Autoscale Flapping** | Resolved by adjusting threshold margins and increasing the **cool-down period**. |
| **Move Web App to New Plan** | Both App Service Plans must reside in the **same Resource Group and geographical region/webspace**. |
```