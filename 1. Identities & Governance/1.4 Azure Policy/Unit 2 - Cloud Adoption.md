
#### Cloud Adoption Framework (CAF)
Microsoft framework for planning, adopting, governing, and managing Azure.
Provides **best practices, documentation, and tools** for cloud adoption.

---
#### Cloud Governance
Managing Azure usage to ensure **security, compliance, cost control, and operational consistency**.

- Reduce risks.
- Enforce standards.
- Align cloud usage with business goals.

*Steps*
1. **Build governance team** → Define, manage, report governance.
2. **Assess risks** → Compliance, security, cost, operations, data, AI.
3. **Document policies** → Rules and standards.
4. **Enforce policies** → Automation + manual oversight.
5. **Monitor governance** → Continuous compliance.

> **Exam Tip:** Governance is a **continuous process** (repeat steps 2–5).

---

#### Governance Policy Considerations

- **Business Risk** → Identify risks & risk tolerance.
- **Policy & Compliance** → Convert risks into policies.
- **Process** → Monitor compliance & policy violations.

---

#### Five Governance Disciplines

- **Cost Management** → Control and optimise cloud spending.
- **Security Baseline** → Apply minimum security standards.
- **Resource Consistency** → Standardise resource configuration.
- **Identity Baseline** → Enforce RBAC & identity standards.
- **Deployment Acceleration** → Standardise deployments using templates.

---

#### Azure Policy
Azure governance service that **enforces rules** and **checks compliance** across resources

- Built-in **policies** and **initiatives**.
- Compliance dashboard.
- Centralized compliance reporting.
- Evaluates **existing** and **new** resources.
- Can automatically remediate non-compliant resources.
- Supports **policy exceptions**.
- Integrates with **Azure DevOps**.

*Common Uses*

- Restrict **Azure regions**.
- Restrict **VM sizes**.
- Enforce **resource tags**.
- Require **diagnostic logs**.
- Recommend **system updates**.
- Require **MFA**.
- Enforce **geo-replication**.

*Azure Policy Evaluation*

- Evaluates:
  - Existing resources.
  - New resource deployments.
  - Resource changes (e.g., VM resize).

- Can:
  - Audit.
  - Deny.
  - Remediate.
  - Exempt specific resources.

---

# AZ-104 Exam Facts

- **CAF** = Framework for Azure adoption.
- **Cloud Governance** = Ongoing management of cloud resources.
- **Azure Policy** = Governance & compliance tool.
- **Governance = Continuous process.**
- **Goal:** Balance **control & compliance** with **speed & productivity**.