# 📄 Nexora Technologies — Full Technical Documentation

**Project:** Enterprise Active Directory Home Lab
**Author:** Joel Bless Addo
**Date:** May 2026
**Version:** 1.0

---

## Table of Contents

1. [Project Goals](#1-project-goals)
2. [Pre-Installation Planning](#2-pre-installation-planning)
3. [Virtual Machine Setup](#3-virtual-machine-setup)
4. [Windows Server 2016 Installation](#4-windows-server-2016-installation)
5. [Active Directory Installation](#5-active-directory-installation)
6. [Domain Configuration](#6-domain-configuration)
7. [Organisational Units](#7-organisational-units)
8. [User Account Creation](#8-user-account-creation)
9. [Security Groups](#9-security-groups)
10. [Group Policy Objects](#10-group-policy-objects)
11. [Shared Folders & NTFS Permissions](#11-shared-folders--ntfs-permissions)
12. [Client Machine Setup](#12-client-machine-setup)
13. [Testing & Verification](#13-testing--verification)
14. [Troubleshooting Log](#14-troubleshooting-log)
15. [Lessons Learned](#15-lessons-learned)

---

## 1. Project Goals

The primary objective of this project was to simulate a real-world enterprise IT environment from scratch using free tools available on a personal MacBook. The specific goals were:

- Deploy a Windows Server 2016 Domain Controller with Active Directory
- Create a realistic company structure with departments, users, and security groups
- Enforce security policies using Group Policy Objects
- Implement least-privilege access control using NTFS permissions
- Join a client workstation to the domain and verify all configurations
- Document everything professionally for a GitHub portfolio

---

## 2. Pre-Installation Planning

### 2.1 Tools Required

| Tool | Version | Cost | Source |
|---|---|---|---|
| Oracle VirtualBox | Latest | Free | virtualbox.org |
| Windows Server 2016 | Evaluation (180 days) | Free | Microsoft Evaluation Center |
| Windows 10 Pro | Evaluation | Free | Microsoft |
| MacBook | Host machine | — | Personal device |

### 2.2 IP Address Plan

Planned before touching any device — professional practice.

```
Network:       192.168.10.0/24
Subnet Mask:   255.255.255.0
Gateway:       192.168.10.1
DNS:           192.168.10.1 (DC)

Infrastructure:  192.168.10.1  – 192.168.10.10
Servers:         192.168.10.11 – 192.168.10.30
Printers:        192.168.10.31 – 192.168.10.50
DHCP Pool:       192.168.10.51 – 192.168.10.200
Reserved:        192.168.10.201– 192.168.10.254
```

### 2.3 Company Structure Plan

Designed before building:

```
Company:      Nexora Technologies
Domain:       nexoratech.com
Email Format: firstname.lastname@nexoratech.com
Departments:  IT, HR, Finance, Engineering, Sales
Total Users:  10 employees
```

---

## 3. Virtual Machine Setup

### 3.1 NEXORA-DC01 (Domain Controller)

| Setting | Value |
|---|---|
| VM Name | NEXORA-DC01 |
| OS Type | Windows Server 2016 (64-bit) |
| RAM | 2048 MB |
| Storage | 50 GB (Dynamic) |
| Network | Internal Network — NexoraNet |

### 3.2 NEXORA-CLIENT01 (Workstation)

| Setting | Value |
|---|---|
| VM Name | NEXORA-CLIENT01 |
| OS Type | Windows 10 Pro (64-bit) |
| RAM | 2048 MB |
| Storage | 40 GB (Dynamic) |
| Network | Internal Network — NexoraNet |

### 3.3 Network Configuration

Both VMs placed on VirtualBox **Internal Network** named `NexoraNet` — acting as a virtual switch connecting both machines privately.

---

## 4. Windows Server 2016 Installation

### 4.1 Installation Steps

1. Attached Windows Server 2016 ISO to VM
2. Selected **Windows Server 2016 Standard Evaluation (Desktop Experience)**
3. Chose **Custom Installation** on clean disk
4. Set Administrator password: (secure — not documented publicly)
5. Renamed server to `NEXORA-DC01` via System Properties

### 4.2 Static IP Configuration

Configured before promoting to Domain Controller:

```
IP Address:        192.168.10.1
Subnet Mask:       255.255.255.0
Default Gateway:   192.168.10.1
Preferred DNS:     192.168.10.1 (points to itself)
```

**Reason for static IP:** Domain Controllers must have fixed IPs. If the IP changed, all domain-joined devices would lose connectivity to the domain.

---

## 5. Active Directory Installation

### 5.1 Installing AD DS Role

1. Opened **Server Manager → Add Roles and Features**
2. Selected **Active Directory Domain Services**
3. Accepted all required features
4. Completed installation

### 5.2 Promoting to Domain Controller

1. Clicked yellow notification flag in Server Manager
2. Selected **"Promote this server to a domain controller"**
3. Chose **"Add a new forest"**
4. Set Root domain name: `nexoratech.com`
5. Set DSRM password (Directory Services Restore Mode)
6. Allowed NetBIOS name to auto-populate as `NEXORATECH`
7. Server restarted automatically after promotion

**Post-restart login:** `NEXORATECH\Administrator`

---

## 6. Domain Configuration

### 6.1 UPN Suffix Configuration

Added `nexoratech.com` as an alternative UPN suffix so users could log in with their corporate email format.

**Path:** Active Directory Domains and Trusts → Right-click top node → Properties → Alternative UPN Suffixes

**Value added:** `nexoratech.com`

**Result:** Users can now log in as `joel.addo@nexoratech.com` instead of `joel.addo@nexoratech.local`

### 6.2 DNS Verification

Verified DNS was functioning correctly:
- Forward Lookup Zone for `nexoratech.com` created automatically
- A records for NEXORA-DC01 present
- PTR records configured

---

## 7. Organisational Units

### 7.1 OU Creation

Created 5 OUs to mirror Nexora Technologies' department structure:

**Path:** Active Directory Users and Computers → Right-click `nexoratech.com` → New → Organizational Unit

| OU Name | Purpose |
|---|---|
| `Nexora-IT` | IT department users and resources |
| `Nexora-HR` | Human Resources users and resources |
| `Nexora-Finance` | Finance department users and resources |
| `Nexora-Engineering` | Engineering department users and resources |
| `Nexora-Sales` | Sales department users and resources |

### 7.2 Why OUs Matter

OUs allow Group Policies to be applied at department level, users to be organised logically, and delegation of admin rights to be scoped to specific departments — exactly how enterprise environments operate.

---

## 8. User Account Creation

### 8.1 User Provisioning Process

For each user:
1. Right-clicked target OU → New → User
2. Filled in First name, Last name, Full name
3. Set User logon name (e.g. `joel.addo`)
4. Selected UPN domain `@nexoratech.com`
5. Set password: `Nexora@2024!`
6. Unchecked "User must change password at next logon"
7. Checked "Password never expires" (lab environment only)
8. Added email address in General tab

### 8.2 All User Accounts

| Name | Logon | Email | OU |
|---|---|---|---|
| Joel Addo | joel.addo | joel.addo@nexoratech.com | Nexora-IT |
| Akosua Dankwa | akosua.dankwa | akosua.dankwa@nexoratech.com | Nexora-IT |
| Sarah Mensah | sarah.mensah | sarah.mensah@nexoratech.com | Nexora-HR |
| Abena Osei | abena.osei | abena.osei@nexoratech.com | Nexora-HR |
| Kwame Asante | kwame.asante | kwame.asante@nexoratech.com | Nexora-Finance |
| Yaw Frimpong | yaw.frimpong | yaw.frimpong@nexoratech.com | Nexora-Finance |
| Ama Boateng | ama.boateng | ama.boateng@nexoratech.com | Nexora-Engineering |
| Efua Tetteh | efua.tetteh | efua.tetteh@nexoratech.com | Nexora-Engineering |
| Kofi Darko | kofi.darko | kofi.darko@nexoratech.com | Nexora-Sales |
| Nana Amponsah | nana.amponsah | nana.amponsah@nexoratech.com | Nexora-Sales |

---

## 9. Security Groups

### 9.1 Group Creation Process

For each department:
1. Right-clicked target OU → New → Group
2. Set Group name (e.g. `IT-Team`)
3. Group scope: **Global**
4. Group type: **Security**
5. Added department members via Members tab

### 9.2 Groups & Members

| Group | Scope | Type | Members |
|---|---|---|---|
| `IT-Team` | Global | Security | joel.addo, akosua.dankwa |
| `HR-Team` | Global | Security | sarah.mensah, abena.osei |
| `Finance-Team` | Global | Security | kwame.asante, yaw.frimpong |
| `Engineering-Team` | Global | Security | ama.boateng, efua.tetteh |
| `Sales-Team` | Global | Security | kofi.darko, nana.amponsah |

### 9.3 Why Security Groups

Security Groups allow permissions to be assigned to the group rather than individual users. When a new employee joins HR, 
simply adding them to `HR-Team` gives them all the correct access automatically — this is enterprise best practice.

---

## 10. Group Policy Objects

### 10.1 GPO Creation Process

All GPOs created using:
Right-click `nexoratech.com` → **"Create a GPO in this domain and link it here"**

This automatically creates AND links the GPO to the domain simultaneously.

### 10.2 GPO 1 — Nexora - Block USB Storage

**Purpose:** Prevent data theft and malware introduction via removable storage.

**Path:**
```
Computer Configuration → Policies → Administrative Templates
→ System → Removable Storage Access
→ All Removable Storage classes: Deny all access → Enabled
```

**Verification:** GPO showed as Linked: Yes, Status: Enabled in Group Policy Management console.

### 10.3 GPO 2 — Nexora - Restrict Control Panel

**Purpose:** Prevent standard users from modifying system settings.

**Path:**
```
User Configuration → Policies → Administrative Templates
→ Control Panel
→ Prohibit access to Control Panel and PC Settings → Enabled
```

**Verification:** Attempted to open Control Panel on CLIENT01 as `joel.addo` — received error: 
*"This operation has been cancelled due to restrictions in effect on this computer. Please contact your system administrator."*

### 10.4 GPO 3 — Nexora - Password Policy

**Purpose:** Enforce strong password standards across all domain accounts.

**Path:**
```
Computer Configuration → Policies → Windows Settings
→ Security Settings → Account Policies → Password Policy
```

**Settings configured:**

| Policy | Value |
|---|---|
| Minimum password length | 10 characters |
| Password complexity | Enabled |
| Maximum password age | 90 days |
| Password history | 5 passwords |

**Verification:** Attempted to set weak password for Ama Boateng — received error: *"The password does not meet the password policy requirements."*

### 10.5 GPO 4 — Nexora - Screen Lock Policy

**Purpose:** Automatically lock unattended workstations to prevent unauthorised access.

**Path:**
```
Computer Configuration → Policies → Windows Settings
→ Security Settings → Local Policies → Security Options
→ Interactive logon: Machine inactivity limit → 600 seconds
```

**Verification:** Set to 60 seconds for testing — machine locked automatically after idle period. Reset to 600 seconds (10 minutes) for production use.

---

## 11. Shared Folders & NTFS Permissions

### 11.1 Folder Structure Created

```
C:\NexoraShares\
├── IT-Share
├── HR-Share
├── Finance-Share
├── Engineering-Share
└── Sales-Share
```

### 11.2 NTFS Permission Configuration

For each folder:
1. Right-clicked folder → Properties → Security → Advanced
2. Clicked **Disable Inheritance** → Convert to explicit permissions
3. Removed **Users** group
4. Added corresponding Security Group (e.g. `HR-Team`)
5. Granted **Modify** permission
6. Applied changes

### 11.3 Network Share Configuration

For each folder:
1. Properties → Sharing → Advanced Sharing
2. Checked **Share this folder**
3. Added Security Group with **Full Control**

### 11.4 Permission Matrix

| Folder | IT-Team | HR-Team | Finance-Team | Engineering-Team | Sales-Team |
|---|---|---|---|---|---|
| IT-Share | ✅ Modify | ❌ Denied | ❌ Denied | ❌ Denied | ❌ Denied |
| HR-Share | ❌ Denied | ✅ Modify | ❌ Denied | ❌ Denied | ❌ Denied |
| Finance-Share | ❌ Denied | ❌ Denied | ✅ Modify | ❌ Denied | ❌ Denied |
| Engineering-Share | ❌ Denied | ❌ Denied | ❌ Denied | ✅ Modify | ❌ Denied |
| Sales-Share | ❌ Denied | ❌ Denied | ❌ Denied | ❌ Denied | ✅ Modify |

---

## 12. Client Machine Setup

### 12.1 Windows 10 Pro Installation

1. Created NEXORA-CLIENT01 VM in VirtualBox
2. Attached Windows 10 Pro ISO
3. Selected **Windows 10 Pro** edition
4. Set up as **Organisation** device
5. Created local account: `NexoraUser`

### 12.2 Static IP Configuration

```
IP Address:      192.168.10.51
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
Preferred DNS:   192.168.10.1
```

**Critical:** DNS must point to the Domain Controller so the client can resolve `nexoratech.com` during domain join.

### 12.3 Connectivity Test

Ran ping test before domain join:
```
ping 192.168.10.1

Reply from 192.168.10.1: bytes=32 time<1ms TTL=128
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss) ✅
```

### 12.4 Domain Join Process

1. Right-click Start → System → Rename this PC (Advanced)
2. Clicked **Change**
3. Selected **Domain** → typed `nexoratech.com`
4. Entered Administrator credentials
5. Received: **"Welcome to the nexoratech.com domain!"**
6. Restarted CLIENT01

### 12.5 First Domain Login

```
Username: nexoratech\joel.addo
Password: Nexora@2024!
Result:   Successful ✅
Profile:  Created at C:\Users\joel.addo
Domain:   Confirmed as nexoratech.com in System Properties
```

---

## 13. Testing & Verification

### 13.1 Test Results Summary

| Test | Expected Result | Actual Result | Status |
|---|---|---|---|
| Domain Join | Welcome message | Received ✅ | Pass |
| Domain Login | Desktop loads | Loaded as joel.addo | Pass |
| Control Panel Block | Error message | "Cancelled due to restrictions" | Pass |
| Weak Password | Rejected | Password policy error shown | Pass |
| Screen Lock | Locks after timeout | Locked after 60 seconds | Pass |
| HR accessing HR-Share | Access granted | Granted ✅ | Pass |
| HR accessing IT-Share | Access denied | Denied ❌ | Pass |
| GPO Linked in console | Shows as Enabled | Enabled ✅ | Pass |

### 13.2 GPO Verification Commands Used

```powershell
# Check all applied GPOs on client
gpresult /r

# Force immediate GPO refresh
gpupdate /force

# Detailed GPO report
gpresult /r /scope computer
```

---

## 14. Troubleshooting Log

### Issue 1 — VMs Not Communicating
**Problem:** Ping between DC01 and CLIENT01 failing
**Cause:** Both VMs on different network types (NAT vs Internal)
**Solution:** Changed both VM adapters to Internal Network → NexoraNet
**Lesson:** Both VMs must be on identical internal network name

### Issue 2 — GPO Not Applying Immediately
**Problem:** Control Panel restriction not showing after GPO creation
**Cause:** Group Policy applies on login or at set intervals
**Solution:** Ran `gpupdate /force` on CLIENT01 and signed out/in
**Lesson:** Always run gpupdate /force after GPO changes during testing

### Issue 3 — Domain Join Failing
**Problem:** "nexoratech.com domain could not be contacted"
**Cause:** CLIENT01 DNS was pointing to wrong server
**Solution:** Changed CLIENT01 DNS to 192.168.10.1 (DC IP)
**Lesson:** DNS must always point to Domain Controller for domain join

---

## 15. Lessons Learned

### Technical Lessons

```
1. Plan your IP addressing scheme BEFORE configuring any device.
   Changing IPs mid-project causes domain connectivity issues.

2. DNS is the backbone of Active Directory.
   Every problem that seems like an AD problem is often DNS.

3. Always run gpupdate /force when testing GPOs.
   Waiting for natural policy refresh wastes time in a lab.

4. Security Groups are more powerful than direct user permissions.
   Assign permissions to groups, add users to groups — never
   assign permissions directly to individual user accounts.

5. Document as you build — not after.
   Memory fades fast. Screenshots taken during configuration
   are more accurate than recreating them later.
```

### Professional Lessons

```
1. The lab environment is only as good as its documentation.
   A perfectly configured AD with no documentation is worthless
   to any team member who inherits it.

2. Test every change immediately after making it.
   Don't configure 10 things then test — you won't know
   which one caused the problem.

3. Least privilege is not just a concept — it's a discipline.
   Every permission granted should be questioned.
   Every unnecessary right removed.

4. Real enterprise environments are just bigger versions of this.
   The same GPOs, the same OU structures, the same NTFS
   permissions logic — just at larger scale.
```

---

## 📞 Contact

**Joel Bless Addo**
IT Support Specialist | Junior System Administrator
📍 Accra, Ghana
📧 addojoel283@gmail.com
🔗 [LinkedIn](https://www.linkedin.com/in/addojoelbless/)
🐙 [GitHub](https://github.com/JoelBlessAddo/)

---

*This documentation was written following professional IT documentation standards. Every configuration step, test result, and lesson learned has been recorded to demonstrate not just technical ability but the discipline of clear, thorough technical writing.*
