Defines **compliance conditions** and the **effect** applied when conditions are met.

- Written in **JSON**.
- Contains:
  - **if** → Conditions to evaluate.
  - **then** → Effect to apply.

---

#### Policy Definition Properties

- **displayName** → Policy name.
- **description** → Policy purpose.
- **policyType** → Built-in, Custom, or Static.
- **mode** → Determines which resource types/properties are evaluated.
- **parameters** → Makes policies reusable with different values.
- **policyRule** → Contains `if` + `then`.

---

#### Policy Conditions

*Logical Operators*

- **allOf** → All conditions must be true (**AND**).
- **anyOf** → One or more conditions must be true (**OR**).
- **not** → Reverses the condition.

*Condition Types*

- **Field** → Evaluates resource properties.
- **Value** → Evaluates a value.
- **Count** → Counts matching items in an array.

*Common Operators*

- `equals` / `notEquals`
- `in` / `notIn`
- `like` / `notLike`
- `contains` / `notContains`
- `exists`
- `greater` / `greaterOrEquals`
- `less` / `lessOrEquals`

---

#### Policy Effects

| Effect | Purpose |
|---|---|
| **deny** | Blocks non-compliant resource requests. |
| **audit** | Logs non-compliance without blocking. |
| **modify** | Adds, updates, or removes properties/tags. |
| **append** | Adds properties during creation/update; largely obsolete. |
| **deployIfNotExists** | Deploys a related resource if missing. |
| **auditIfNotExists** | Audits when a related resource/configuration is missing. |
| **denyAction** | Blocks specific actions; currently **DELETE**. |
| **disabled** | Disables the policy. |
| **manual** | Requires manual compliance attestation. |

---

#### Policy Evaluation

- **Synchronous** → `deny`, `modify`, `append`, `denyAction`, `disabled`.
- **Asynchronous** → `audit`, `auditIfNotExists`, `deployIfNotExists`.
- **Manual** → `manual`.

**Multiple policies** → Effects are **cumulative most restrictive**.

---

#### AZ-104 Exam Facts

- **Policy Definition** = `if` condition + `then` effect.
- **allOf** = AND.
- **anyOf** = OR.
- **not** = Inverts condition.
- **deny** = Prevents non-compliant deployment.
- **audit** = Reports non-compliance without blocking.
- **modify** = Changes properties/tags.
- **deployIfNotExists** = Deploys missing related resources.
- **auditIfNotExists** = Audits missing related resources.
- **manual** = Manual compliance attestation.
- **Parameters** = Reuse one policy with different values.
- Multiple policies → **most restrictive result wins**.