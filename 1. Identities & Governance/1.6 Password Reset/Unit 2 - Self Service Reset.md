**SSPR** = allows users who can't sign in to reset their Microsoft Entra ID password themselves.

- Can be used from a **web browser** or **Windows sign-in screen**.
- Reduces **help-desk workload** and user downtime.

---
#### How SSPR Works
_Steps_

1. User opens the **password-reset portal** or selects **Can't access your account**.
2. User enters their **username** and completes CAPTCHA.
3. User verifies their identity using configured authentication methods.
4. User creates a **new password**.
5. User receives a **notification** confirming the reset.

---
#### Authentication Methods
SSPR supports **6 methods**:
- **Mobile app notification** → Approve/deny Authenticator notification.
- **Mobile app code** → Enter code from Authenticator.
- **Email** → Enter code sent to external email.
- **Mobile phone** → SMS code or automated call.
- **Office phone** → Automated call; press `#`.
- **Security questions** → Answer registered questions.

**Exam Tip**
- Configure **2 or more authentication methods**.
- **Mobile app notification/code** → recommended primary methods.
- **Mobile phone** → not recommended due to SMS fraud risks.
- **Security questions** → least recommended.

---
#### Authentication Requirements
- Admins can require users to register **1 or 2 authentication methods**.
- Security questions can have separate requirements for:
    - Number of questions registered.
    - Number of questions answered during reset.
- Trial Microsoft Entra organisations don't support **phone call** options.

**Important**
- Administrator accounts always require a **two-method authentication policy**.
- **Security questions** aren't available for administrator accounts.

---
#### SSPR Notifications
Administrators can enable:
- **Notify users on password resets**
    - Sends notification to primary and secondary email addresses.
- **Notify all admins when other admins reset their password**
    - Alerts administrators when another admin resets their password.

---
#### License Requirements
- **Microsoft Entra ID P1/P2** → SSPR for users who aren't signed in.
- Signed-in users can change their password **regardless of edition**.
- SSPR is also available with **Microsoft 365 Apps for business**.

**Exam Tip**
- **Password writeback** → available with **Microsoft Entra ID P1/P2**.

---
#### Password Writeback
**Password writeback** = writes cloud password changes back to the on-premises **Active Directory**.

- Used in **hybrid identity** environments.
- Supported through:
    - **Microsoft Entra Connect**
    - **Cloud sync**

---
#### SSPR Deployment Options
- **Microsoft Entra Connect**
    - Supports password writeback to existing on-premises AD.
- **Cloud sync**
    - Alternative for synchronising users/passwords.
    - Can provide **higher availability** by avoiding reliance on a single Microsoft Entra Connect instance.
- Both options can be deployed **side-by-side** for different domains/users.