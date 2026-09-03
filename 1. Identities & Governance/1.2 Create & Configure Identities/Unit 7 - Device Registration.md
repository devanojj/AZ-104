
## Microsoft Entra Registered
> **Best for:** Bring Your Own Device (BYOD) / Personal devices

### Key Features
- Personal device registered with Microsoft Entra ID
- User signs in with **local account** (not work account)
- Work account added for access to company resources
- Supports **Conditional Access**
- Can be managed with **Microsoft Intune (MDM)**

### Device Ownership
- User-owned (or organisation-owned)

### Management
- Microsoft Intune (MDM)
- Mobile Application Management (MAM)

### Examples
- Home PC accessing Microsoft 365
- Personal Android/iPhone
- Personal Mac

### Remember
- **Local sign-in + Work account attached**
- Ideal for **BYOD**

---

## Microsoft Entra Joined

> **Best for:** Cloud-first / Cloud-only organizations

### Key Features
- Device joined directly to Microsoft Entra ID
- User signs in using **organizational account**
- SSO to **cloud and on-premises resources**
- Supports:
  - Conditional Access
  - Windows Hello
  - Self-Service Password Reset (SSPR)
- Managed with **Microsoft Intune**

### Device Ownership
- Organization-owned

### Deployment
- Out of Box Experience (OOBE)
- Windows Autopilot
- Bulk enrollment

### Remember
- **Work device**
- **Cloud-native**
- No on-prem AD required

---

## Hybrid Microsoft Entra Joined

> **Best for:** Organizations with on-premises Active Directory

### Key Features
- Joined to:
  - On-premises Active Directory
  - Microsoft Entra ID
- User signs in with organizational account
- SSO to cloud and on-premises resources
- Supports:
  - Conditional Access
  - Windows Hello for Business
  - Self-Service Password Reset

### Management
- Group Policy
- Configuration Manager (SCCM)
- Microsoft Intune (Co-management)

### When to Use
- Existing Active Directory environment
- Legacy Win32 applications
- Still using Group Policy
- Existing imaging solutions

### Remember
- **AD + Entra ID**
- Best for hybrid environments

---

## Device Comparison

| Feature | Registered | Joined | Hybrid Joined |
|---------|------------|---------|---------------|
| Device Ownership | Personal (BYOD) | Organization | Organization |
| Sign-in | Local account + Work account | Work account | Work account |
| On-prem AD | ❌ | ❌ | ✅ |
| Microsoft Entra ID | ✅ | ✅ | ✅ |
| Intune Support | ✅ | ✅ | ✅ |
| Group Policy | ❌ | ❌ | ✅ |
| Best For | BYOD | Cloud-only | Hybrid environments |

---

## Cloud Kerberos Trust

- Replaces **Device Writeback** (deprecated)
- Enables Microsoft Entra joined and Hybrid joined devices to authenticate to on-premises resources
- Supports:
  - On-premises SSO
  - Windows Hello for Business

> **Exam Tip:** Device Writeback is **no longer recommended**.

---

## Quick Memory Trick

- 📱 **Registered** = **Personal device (BYOD)**
- ☁️ **Joined** = **Cloud-only work device**
- 🏢 **Hybrid Joined** = **On-prem AD + Cloud**