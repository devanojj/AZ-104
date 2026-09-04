**SSPR** can be rolled out to a **selected security group** first, then expanded to all users.

---
#### Prerequisites
- **Microsoft Entra ID P1/P2** trial or license.
- **Authentication Policy Administrator** account.
- **Non-administrative test user** with valid license.
- **Security group** containing test users.

---
#### SSPR Scope
- **None** → SSPR disabled.
- **Selected** → only specified security group can use SSPR.
- **All** → all users can use SSPR.

**Exam Tip**
- Use **Selected** for a trial/pilot deployment.

---
#### Configure SSPR
_Steps_
**Microsoft Entra ID → Manage → Password reset**
1. **Properties** → Enable for **Selected** users/group or **All**.
2. **Authentication methods** → Choose 1 or 2 methods.
3. **Registration** → Configure user registration/reconfirmation.
4. **Notifications** → Configure password-reset notifications.
5. **Customisation** → Add help email/URL.