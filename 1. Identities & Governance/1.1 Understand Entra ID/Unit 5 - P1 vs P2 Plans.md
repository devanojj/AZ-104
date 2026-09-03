
| Feature Area                           | Free                               | Microsoft Entra ID P1                                   | Microsoft Entra ID P2                                   |
| -------------------------------------- | ---------------------------------- | ------------------------------------------------------- | ------------------------------------------------------- |
| **SLA**                                | No financially-backed SLA          | 99.9% uptime SLA                                        | 99.9% uptime SLA                                        |
| **Self-Service Password Reset (SSPR)** | Cloud-only users                   | Cloud + On-premises writeback                           | Cloud + On-premises writeback                           |
| **Group Management**                   | Basic static groups                | Dynamic groups, self-service group management           | Dynamic groups, self-service group management           |
| **Hybrid Identity & Sync**             | Microsoft Entra Connect sync       | Entra Connect Health + Microsoft Identity Manager (MIM) | Entra Connect Health + Microsoft Identity Manager (MIM) |
| **Conditional Access**                 | Security defaults (all or nothing) | Granular (by user, device, location, app)               | Risk-based (real-time user and sign-in risk)            |
| **Cloud App Discovery**                | Not included                       | Included (basic SaaS discovery)                         | Included                                                |
| **Identity Protection**                | Not included                       | Not included                                            | Full (ML-driven automated risk remediation)             |
| **Privileged Access (PIM)**            | Not included                       | Not included                                            | Full (Just-In-Time role activation, approval flows)     |
| **Access Reviews & Governance**        | Not included                       | Not included                                            | Full (automated recurring access recertification)       |

**Microsoft Entra ID Free**
- Foundation-level directory for Microsoft 365 or Azure subscriptions.
- Supports user/group creation, cloud-only authentication, basic SSO across 10 apps per user, and security defaults.
- Lacks granular security controls, dynamic assignment, and automated identity governance.

**Microsoft Entra ID P1 (Workforce Security & Hybrid Foundation)**
- **Granular Conditional Access:** Enforces MFA and access policies based on trusted locations, device compliance (Intune), or specific user groups.
- **Hybrid Enablement:** Supports self-service password reset with on-premises Active Directory writeback, Microsoft Identity Manager (MIM), and Entra Connect Health monitoring.
- **Advanced Administration:** Introduces dynamic user/device groups and delegated self-service group management.

**Microsoft Entra ID P2 (Zero Trust & Privileged Governance)**
- **Microsoft Entra ID Protection:** Uses machine learning to evaluate real-time sign-in risk (e.g., anonymous IP, atypical travel) and user risk (e.g., leaked credentials) to automate self-remediation (blocking access or forcing password reset).
- **Privileged Identity Management (PIM):** Eliminates standing admin privileges by providing time-bound, just-in-time (JIT) role elevation, multi-party approval workflows, and audit trails.
- **Access Reviews & Lifecycle Workflows:** Automates periodic audits of external guests, group memberships, and privileged roles to prevent entitlement drift.