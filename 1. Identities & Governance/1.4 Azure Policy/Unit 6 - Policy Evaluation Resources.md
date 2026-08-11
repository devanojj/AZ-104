Determines whether resources are **compliant** with assigned policies.

---
#### Evaluation Triggers
Policy evaluation occurs when:

- Policy/initiative is **assigned or updated**.
- Resource is **created or updated**.
- Policy **exemption** is created, updated, or deleted.
- Subscription is created/moved within a management group.
- **Standard compliance scan** runs.
- **On-demand scan** is triggered.
---
#### Evaluation Timing

- **Automatic full scan** → Every **24 hours**.
- **Manual scan** → Can be triggered with `az policy state trigger-scan`.
- New policy assignment can take up to **30 minutes** to propagate.

> **Exam Tip:** Policy scans are not necessarily immediate.

---
#### Compliance States

- **Compliant** → Meets policy requirements.
- **Non-compliant** → Doesn't meet requirements.
- **Error** → Policy evaluation/template error.
- **Conflicting** → Policies have contradictory effects.
- **Protected** → Covered by `denyAction`.
- **Exempted** → Resource is exempt.
- **Unknown** → Default for `manual` policies before attestation.
---
#### Enforcement Mode
Controls whether a policy **effect is enforced**.

**Enabled (Default)**
- Policy effect is enforced.
- Activity log entries generated.

**Disabled (`DoNotEnforce`)**
- Policy is evaluated but effect **isn't enforced**.
- No Activity Log entry from the effect.
- Used for **What-If testing**.

> **Exam Tip:** `enforcementMode` ≠ `disabled` effect.
>
> **Disabled effect** → Policy isn't evaluated.  
> **DoNotEnforce** → Policy is evaluated but not enforced.

---
#### Safe Policy Deployment

1. Set `enforcementMode` → **Disabled**.
2. Test compliance.
3. Deploy using **deployment rings**.
4. Start with **non-production**.
5. Check compliance + application health.
6. Enable enforcement.
7. Gradually deploy to **production**.

---
#### Azure Policy Events

**Azure Policy → Event Grid → Event Handler**

- **Event Grid** → Routes policy state change events.
- Event handlers:
  - Azure Functions
  - Logic Apps
  - Webhooks
  - Custom HTTP endpoints

---
#### AZ-104 Exam Facts
- **Full compliance scan** → Every **24 hours**.
- **DoNotEnforce** → Evaluate without enforcing.
- **Disabled effect** → No evaluation.
- **Deployment rings** → Safely roll out policies.
- **Event Grid** → React to Azure Policy state changes.
- **ARM Request Sequence:** RBAC ("Can you?") → Azure Policy ("Should you?") → Resource Provider ("Build it").
- **Deny Policy:** Evaluates pre-creation; blocks non-compliant requests before reaching the Resource Provider.