# 🔐 Break-Glass Account Architecture (Microsoft Entra ID)
**Focus:** Emergency Access • Tenant Lockout Prevention • Monitoring & Alerts • Zero Trust IAM  
**Platform:** Microsoft Entra ID (Azure AD)  
**Level:** Enterprise / Tier-0 Identity Security

<img width="1536" height="1024" alt="ChatGPT Image Jan 30, 2026 at 03_21_40 PM" src="https://github.com/user-attachments/assets/f865f7ef-27cf-4690-8ba2-2c9ad8339af1" />

---

## 📌 Project Summary
This project documents the design and implementation of **Break-Glass (Emergency Access) accounts** in Microsoft Entra ID.

Break-glass accounts are **Tier-0 identities** used only during emergencies such as:
- Conditional Access misconfiguration
- MFA or authentication outages
- Tenant-wide lockout scenarios
- Identity service disruptions

The objective of this lab is to ensure:
- Emergency access is **always available**
- Usage is **highly visible and monitored**
- Access is **auditable and intentional**
- Normal security controls do **not** accidentally block recovery

This architecture follows Microsoft and enterprise IAM best practices.

---

## 🎯 What You’ll Build
✅ Two dedicated break-glass admin accounts  
✅ Conditional Access exclusions (intentionally scoped)  
✅ Strong but outage-safe authentication strategy  
✅ Continuous monitoring using Entra logs  
✅ Clear alerting and investigation process  
✅ Interview-ready IAM evidence pack  

---

## 🧰 Requirements
- Microsoft Entra ID tenant
- Ability to manage Conditional Access
- Ability to assign Global Administrator role
- Optional (bonus):
  - Entra ID P2 (Identity Protection / risk detections)
  - Microsoft Sentinel

---

# 🧱 PHASE 0 — Emergency Account Design
**Purpose:** Ensure tenant recovery is always possible.

### Step 0.1 — Create Break-Glass Accounts
Create **two** cloud-only users:

- `BreakGlass-Admin-01`
- `BreakGlass-Admin-02`

Reason for two accounts:
- One may be unavailable, compromised, or misconfigured
- Redundancy is a Tier-0 best practice

📸 Screenshot:

![Image 1-30-26 at 15 14](https://github.com/user-attachments/assets/64b6ad16-348f-4c29-931a-b25680387d77)

---

### Step 0.2 — Assign Global Administrator Role
Assign **Global Administrator** to both accounts.

Important:
- These accounts **do NOT use PIM**
- They must always be able to recover the tenant

📸 Screenshot:

![Image 1-30-26 at 15 23](https://github.com/user-attachments/assets/ed862b98-ee69-460e-aa31-a8262218cf02)

---

# 🔐 PHASE 1 — Authentication Strategy
**Purpose:** Prevent lockout during MFA or auth outages.

### Step 1.1 — Configure Authentication
For each break-glass account:
- Use a **very long password** (30+ characters)
- Store credentials securely (offline password vault or sealed documentation)

Intentional design:
- ❌ No Conditional Access MFA
- ❌ No Authentication Strength enforcement
- ✅ Availability > convenience

📸 Screenshot:

![2](https://github.com/user-attachments/assets/81760b5b-9832-4bdd-886b-9bf792c8597d)

---

# 🧠 PHASE 2 — Conditional Access Exclusions
**Purpose:** Ensure break-glass accounts bypass all CA policies.

### Step 2.1 — Update Conditional Access Policies
For **every Conditional Access policy** in the tenant:
- Exclude:
  - `BreakGlass-Admin-01`
  - `BreakGlass-Admin-02`

This includes:
- MFA enforcement policies
- Phishing-resistant authentication policies
- Risk-based Conditional Access
- Session control policies

📸 Screenshots:

![3](https://github.com/user-attachments/assets/206e4f7b-0877-4608-a105-90289c9facb6)

---

# 🔎 PHASE 3 — Monitoring & Alerts
**Purpose:** Break-glass access must be rare, visible, and investigated immediately.

---

## Step 3.1 — Monitor Sign-In Logs (Primary Detection)
**Path:**  
Entra admin center → **Identity** → **Monitoring** → **Sign-in logs**

Filter:
- **User** = `BreakGlass-Admin-01` or `BreakGlass-Admin-02`

Review:
- Location
- IP address
- Client app
- Authentication method

📸 Screenshots:

![4](https://github.com/user-attachments/assets/11df38d6-bd56-435e-afe5-953ed943ac5d)

---

## 👤 Standard Admin User
**Account:** `Felipe-Admin`

**Expected Behavior:**
- Sign-in is **restricted by Conditional Access**
- Requires MFA / phishing-resistant authentication

**Validation:**
- Attempt admin portal access without PIM activation → **Access denied or limited**
- After PIM activation → **Access granted**

📸 Screenshot:

![5](https://github.com/user-attachments/assets/005c17f4-dd4b-4c52-81ef-dfbdd6f9f592)

![6](https://github.com/user-attachments/assets/fe3a4d01-385c-486b-ae18-78bfdddadc8b)

--- 

## 🚨 Break-Glass Admin
**Accounts:**  
- `BreakGlass-Admin-01`  
- `BreakGlass-Admin-02`

**Expected Behavior:**
- **Bypasses all Conditional Access policies**
- No MFA or auth strength enforcement
- Used **only during emergency scenarios**

**Validation:**
- Sign in directly to admin portal → **Access granted**
- Sign-in logs show:
  - Conditional Access: **Not applied (Excluded)**
  - Full audit visibility in sign-in and audit logs
### Conditional Access Validation
Open a sign-in event → **Conditional Access** tab.

📸 Screenshot:

![7](https://github.com/user-attachments/assets/e6402f27-5894-47f8-a106-ae6caaa05adb)

![8](https://github.com/user-attachments/assets/bf172270-a859-4db7-b704-7387ed33cec9)

Expected result:
- **Result:** Not applied  
- **Reason:** User excluded  

This confirms:
- Access worked **by design**, not misconfiguration

📸 Screenshot:

![9](https://github.com/user-attachments/assets/9aee6675-e578-4284-8fe3-a0989e78f710)

![10](https://github.com/user-attachments/assets/5cfb9456-8f9a-4bc7-b84c-5b69ba0ee18d)

![11](https://github.com/user-attachments/assets/ed0e190a-78fb-4beb-8955-26a9283f25dc)

![12](https://github.com/user-attachments/assets/663a9da4-bb0e-47ad-a6b5-354ebff75999)

---

# ✅ Results
This project proves the ability to:
- Design Tier-0 identity architecture
- Prevent tenant lockout
- Balance security with availability
- Monitor emergency access effectively
- Document IAM decisions clearly
