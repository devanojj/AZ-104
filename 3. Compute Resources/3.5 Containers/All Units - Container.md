## 1. Containers vs. Virtual Machines
* **Virtual Machines (Hardware-level Virtualisation):**
  * Includes full Guest OS, kernel, binaries/libraries, and app.
  * Slower boot times (minutes), heavy resource footprint, higher management overhead.
* **Containers (OS-level Virtualisation):**
  * Share the host OS kernel; package only application code, runtimes, and dependencies.
  * Fast startup (seconds), lightweight footprint, high portability and consistency across environments.

---

## 2. Azure Container Instances (ACI) Overview
Serverless container solution running single or grouped containers without managing VMs or orchestrators.
* **Core Characteristics:**
  * **Fast startup:** Launch containers on-demand in seconds.
  * **Per-second billing:** Cost is computed based on vCPU and memory allocated per second.
  * **Hypervisor-level security:** Each container group runs isolated in a dedicated sandbox VM.
  * **Custom sizing:** Specify exact vCPU cores and GB memory required.
  * **Image sources:** Pull from public registries (Docker Hub) or private registries (Azure Container Registry - ACR).

---

## 3. Container Groups & Deployment
A **Container Group** is a collection of containers scheduled on the same host machine sharing the same lifecycle, local network, and storage volumes (analogous to a Pod in Kubernetes).

### Shared Group Resources
* **Lifecycle:** Scheduled, started, stopped, and restarted together.
* **Networking:** Share an assigned IP address, port namespace, and DNS name label.
  * Containers communicate internally via `localhost:<port>`.
* **Storage Volumes:** Mounted volumes (e.g., Azure Files shares) can be shared across containers in the group.

### Multi-Container Deployment
* Group definitions must be deployed using **ARM Templates**, **Bicep**, or **YAML** (single container simple deployments are supported via Azure Portal / CLI).
* **Common Pattern:** Sidecar container pattern (e.g., main web application container paired with a logging/metrics sidecar container).

---

## 4. Networking & Storage in ACI
* **Public Access:** Expose via a public IP and optional DNS name label (`<custom-label>.<region>.azurecontainer.io`).
* **VNet Integration:**
  * Deploy container groups directly into a delegated subnet in an Azure Virtual Network.
  * Provides private IP addressing and secure communication with internal VMs and PaaS services without exposing to the public internet.
* **Mounting Storage:**
  * Supports mounting **Azure Files Shares** (SMB) into Linux and Windows container groups.
  * Volume types: `AzureFile`, `emptyDir`, `gitRepo`, `secret`.

---

## 5. Restart Policies & Environment Variables
### Restart Policies
* **`Always` (Default):** Restarts container continuously if it stops/fails (ideal for long-running web APIs/daemons).
* **`Never`:** Runs once and never restarts (useful for one-off manual executions).
* **`OnFailure`:** Restarts only if the container process exits with a non-zero exit code (ideal for batch jobs / scheduled tasks).

### Environment Variables & Secure Values
* Pass configuration dynamically into container processes at runtime.
* Use **Secure Environment Variables** (`secureValue`) to protect sensitive secrets (passwords, connection strings) from being displayed in plain text or inspectable via CLI/API.

---

## 6. ACI vs. Azure Container Apps (ACA) vs. AKS
| Service | Best Used For | Key Capabilities |
| :--- | :--- | :--- |
| **Azure Container Instances (ACI)** | Isolated tasks, quick burst jobs, simple batch processing, CI/CD runners | Serverless, fast startup, no complex orchestration, per-second billing |
| **Azure Container Apps (ACA)** | Microservices, event-driven apps, background workers, web APIs | Built on Kubernetes (K8s) & KEDA; scale-to-zero; built-in ingress, traffic splitting, Dapr support |
| **Azure Kubernetes Service (AKS)** | Full-scale enterprise container orchestration | Full cluster management, fine-grained control over K8s APIs and node pools |

---

## 🎯 Exam Quick Reference (Cheat Sheet)

| Scenario / Requirement | Correct Configuration / Solution |
| :--- | :--- |
| **Run a short batch script or scheduled task** | ACI with restart policy set to **`OnFailure`**. |
| **Run a long-running web application in ACI** | ACI with restart policy set to **`Always`**. |
| **Deploy multi-container group with sidecar** | Deploy via **ARM Template / YAML** (Portal only creates single containers). |
| **Inter-container communication inside group** | Use **`localhost:<port>`**. |
| **Persist data between container restarts** | Mount an **Azure File Share** volume. |
| **Hide sensitive passwords/keys in container spec** | Pass as **Secure Environment Variables** (`secureValue`). |
| **Secure container communication with Azure VNets** | Deploy ACI with **VNet integration** into a dedicated delegated subnet. |
| **Microservices with scale-to-zero and ingress** | Choose **Azure Container Apps (ACA)** over ACI. |