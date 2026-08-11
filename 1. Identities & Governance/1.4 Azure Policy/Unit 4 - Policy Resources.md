#### Azure Policy Resources
Azure Policy uses resources to **define, assign, group, exempt, attest, and remediate** policies.

[![Screenshot of six policy resources that are available in Azure and multiple concepts apply to these Azure policy resources.|511](https://learn.microsoft.com/en-gb/training/modules/sovereignty-policy-initiatives/media/policy-resources.png)](https://learn.microsoft.com/en-gb/training/modules/sovereignty-policy-initiatives/media/policy-resources.png#lightbox)

---

#### Policy Definition
Defines:
- **Compliance conditions** → What is evaluated.
- **Effect** → What happens when conditions are met.

- Can be stored at a **management group** or **subscription**.
- Definition location determines where it can be assigned.

---

#### Initiative
Also called a **Policy Set**.

Groups multiple **policy definitions** into one manageable unit.

- Simplifies policy assignment.
- Simplifies compliance management.
- Used for broader goals such as **regulatory compliance**.

**Built-in Initiative** → Collection of built-in policies.

**Custom Initiative** → Collection of custom policies.

#### Built-in vs Custom Policy

- **Built-in Policy** → Provided by Azure Resource Providers.
- **Custom Policy** → Created when no built-in policy meets requirements.

---

#### Policy Assignment
Defines **which resources are evaluated** by a policy or initiative.

Can be assigned at:
- Management group
- Subscription
- Resource group

*Key Options*

- **Resource selectors** → Gradual rollout by resource type/location.
- **Overrides** → Change policy effect without changing definition.
- **enforcementMode** → Disable enforcement for **what-if** scenarios.
- **Excluded scopes** → Exclude resources/containers.
- **Parameters** → Set policy parameter values.
- **Noncompliance messages** → Custom compliance messages.
- **Managed identity** → Required for remediation with `deployIfNotExists`.

---

#### Policy Exemption
Excludes resources or resource hierarchies from **policy evaluation**.

- Created **after assignment**.
- Exempt resources still count toward **overall compliance**.
- Similar effect to excluded scopes.

*Types*

- **Mitigated** → Compliance achieved through another method.
- **Waiver** → Temporary acceptance of noncompliance.

---

#### Policy Attestation
Used to manually set a resource/scope's **compliance state**.

- Used with **manual policies**.
- Each applicable resource requires an attestation for each manual policy assignment.

---

#### Policy Remediation
Brings **non-compliant resources into compliance**.

- Used with `modify`.
- Used with `deployIfNotExists`.
- New/updated applicable resources can be **automatically remediated**.

---

#### AZ-104 Exam Facts

- **Definition** → Defines compliance conditions + effect.
- **Initiative** → Groups policies.
- **Assignment** → Determines what resources are evaluated.
- **Exemption** → Excludes resources from evaluation.
- **Attestation** → Manually establishes compliance.
- **Remediation** → Fixes non-compliant resources.
- **Built-in** → Provided by Azure.
- **Custom** → Created by the organisation.
- **enforcementMode disabled** → Test/what-if without enforcing the policy.
- `modify` / `deployIfNotExists` → Can use **remediation**.