# 🏢 Nexora Technologies — Enterprise Active Directory Lab

![Windows Server](https://img.shields.io/badge/Windows%20Server-2016-blue?style=flat-square&logo=windows)
![Active Directory](https://img.shields.io/badge/Active%20Directory-Domain%20Services-0078D4?style=flat-square&logo=microsoft)
![VirtualBox](https://img.shields.io/badge/VirtualBox-Lab%20Environment-183A61?style=flat-square&logo=virtualbox)
![Status](https://img.shields.io/badge/Status-Complete-success?style=flat-square)

---

## Project Overview

This project simulates a **fully functional enterprise IT environment** for a fictional company — **Nexora Technologies** — built entirely on a MacBook using Oracle VirtualBox. The goal was to replicate real-world System Administration tasks including domain setup, user management, security policy enforcement, and access control — all documented professionally.

This lab demonstrates hands-on proficiency in

**Windows Server 2016 administration**, 
**Active Directory Domain Services**, 
**Group Policy management**,
**network configuration** — 

core skills required for Junior SysAdmin and IT Infrastructure roles.

---

## Environment Architecture

```
┌─────────────────────────────────────────────────────┐
│              NEXORA TECHNOLOGIES NETWORK            │
│                  192.168.10.0/24                    │
│                                                     │
│  ┌─────────────────┐      ┌─────────────────┐      │
│  │  NEXORA-DC01    │      │ NEXORA-CLIENT01  │      │
│  │  Windows Server │◄────►│  Windows 10 Pro  │      │
│  │  2016           │      │                  │      │
│  │  192.168.10.1   │      │  192.168.10.51   │      │
│  │                 │      │                  │      │
│  │  • AD DS        │      │  • Domain joined │      │
│  │  • DNS          │      │  • GPOs applied  │      │
│  │  • DHCP         │      │  • Policy tested │      │
│  │  • File Server  │      │                  │      │
│  └─────────────────┘      └─────────────────┘      │
│                                                     │
│              VirtualBox Internal Network            │
│                    (NexoraNet)                      │
└─────────────────────────────────────────────────────┘
```

---

## ⚙️ Technical Specifications

| Component | Details |
|---|---|
| **Server OS** | Windows Server 2016 Standard Evaluation |
| **Client OS** | Windows 10 Pro |
| **Virtualisation** | Oracle VirtualBox |
| **Domain Name** | `nexoratech.com` |
| **NetBIOS Name** | `NEXORATECH` |
| **Domain Controller** | NEXORA-DC01 |
| **Client Machine** | NEXORA-CLIENT01 |
| **Network Range** | 192.168.10.0/24 |
| **DC IP Address** | 192.168.10.1 (Static) |
| **Client IP Address** | 192.168.10.51 (Static) |
| **DNS Server** | 192.168.10.1 (DC) |
| **UPN Suffix** | @nexoratech.com |

---

## 🌐 IP Addressing Scheme

| IP Range | Reserved For |
|---|---|
| `192.168.10.1 – 10` | Network Infrastructure |
| `192.168.10.11 – 30` | Servers (Mail, File, Print) |
| `192.168.10.31 – 50` | Printers & Peripherals |
| `192.168.10.51 – 200` | DHCP Pool (Workstations) |
| `192.168.10.201 – 254` | Reserved for Future Use |

### Device Assignments

| Device | Hostname | IP Address | Role |

| Domain Controller | `NEXORA-DC01` | `192.168.10.1` | AD DS, DNS, DHCP |

| Client Workstation | `NEXORA-CLIENT01` | `192.168.10.51` | Employee Workstation |

---

## 🏢 Company Structure — Nexora Technologies

### Organisational Units (OUs)

```
nexoratech.com
├── 📁 Nexora-IT
├── 📁 Nexora-HR
├── 📁 Nexora-Finance
├── 📁 Nexora-Engineering
└── 📁 Nexora-Sales
```

### Employee Accounts (10 Users)

| Full Name | Username | Email | Department | Role |
|---|---|---|---|---|
| Joel Addo | joel.addo | joel.addo@nexoratech.com | IT | IT Administrator |
| Akosua Dankwa | akosua.dankwa | akosua.dankwa@nexoratech.com | IT | IT Support |
| Sarah Mensah | sarah.mensah | sarah.mensah@nexoratech.com | HR | HR Manager |
| Abena Osei | abena.osei | abena.osei@nexoratech.com | HR | HR Officer |
| Kwame Asante | kwame.asante | kwame.asante@nexoratech.com | Finance | Finance Officer |
| Yaw Frimpong | yaw.frimpong | yaw.frimpong@nexoratech.com | Finance | Accountant |
| Ama Boateng | ama.boateng | ama.boateng@nexoratech.com | Engineering | Software Engineer |
| Efua Tetteh | efua.tetteh | efua.tetteh@nexoratech.com | Engineering | DevOps Engineer |
| Kofi Darko | kofi.darko | kofi.darko@nexoratech.com | Sales | Sales Rep |
| Nana Amponsah | nana.amponsah | nana.amponsah@nexoratech.com | Sales | Sales Manager |

### Security Groups

| Group Name | Department | Members |
|---|---|---|
| `IT-Team` | Nexora-IT | joel.addo, akosua.dankwa |
| `HR-Team` | Nexora-HR | sarah.mensah, abena.osei |
| `Finance-Team` | Nexora-Finance | kwame.asante, yaw.frimpong |
| `Engineering-Team` | Nexora-Engineering | ama.boateng, efua.tetteh |
| `Sales-Team` | Nexora-Sales | kofi.darko, nana.amponsah |

---

## 🔒 Group Policy Objects (GPOs)

Four GPOs were created and linked to `nexoratech.com` domain:

### GPO 1 — Nexora - Block USB Storage
| Setting | Value |
|---|---|
| Policy | All Removable Storage classes: Deny all access |
| Configuration | Computer Configuration |
| Status | **Enabled** |
| Purpose | Prevents data theft via USB devices |

### GPO 2 — Nexora - Restrict Control Panel
| Setting | Value |
|---|---|
| Policy | Prohibit access to Control Panel and PC Settings |
| Configuration | User Configuration |
| Status | **Enabled** |
| Purpose | Prevents users from changing system settings |

### GPO 3 — Nexora - Password Policy
| Setting | Value |
|---|---|
| Minimum password length | 10 characters |
| Password complexity | Enabled |
| Maximum password age | 90 days |
| Password history | 5 passwords remembered |
| Purpose | Enforces strong password standards |

### GPO 4 — Nexora - Screen Lock Policy
| Setting | Value |
|---|---|
| Policy | Interactive logon: Machine inactivity limit |
| Value | 600 seconds (10 minutes) |
| Configuration | Computer Configuration |
| Status | **Enabled** |
| Purpose | Locks unattended workstations automatically |

---

## 📁 Shared Folders & NTFS Permissions

Departmental shared folders created on DC01 with strict NTFS permissions:

```
C:\NexoraShares\
├── 📁 IT-Share          → IT-Team (Modify)
├── 📁 HR-Share          → HR-Team (Modify)
├── 📁 Finance-Share     → Finance-Team (Modify)
├── 📁 Engineering-Share → Engineering-Team (Modify)
└── 📁 Sales-Share       → Sales-Team (Modify)
```

**Access Control Principle:** Each Security Group can only access their department's folder. Cross-department access is explicitly denied — implementing the **Least Privilege Principle.**

---

## ✅ Testing & Verification

### Test 1 — Domain Join
```
Result: NEXORA-CLIENT01 successfully joined nexoratech.com ✅
Verified: "Welcome to the nexoratech.com domain" message received
```

### Test 2 — Domain User Login
```
User: nexoratech\joel.addo
Result: Successful domain authentication ✅
Profile: Created at C:\Users\joel.addo
```

### Test 3 — Control Panel Restriction (GPO 2)
```
Action: Attempted to open Control Panel as joel.addo
Result: "This operation has been cancelled due to
        restrictions in effect on this computer" ✅
GPO enforcement confirmed working
```

### Test 4 — Password Policy (GPO 3)
```
Action: Attempted to set weak password for Ama Boateng
Result: "Windows cannot complete the password change.
        The password does not meet the password policy
        requirements" ✅
Complexity enforcement confirmed working
```

### Test 5 — Screen Lock (GPO 4)
```
Action: Left CLIENT01 idle
Result: Machine locked after inactivity timeout ✅
Requires domain credentials to unlock
```

### Test 6 — NTFS Permissions
```
Action: HR user attempted to access IT-Share
Result: Access Denied ✅
Action: HR user accessed HR-Share
Result: Access Granted ✅
Least privilege principle confirmed working
```

---

## 📸 Screenshots

### 1. OU Structure — Nexora Technologies Domain
![OU Structure](screenshots/OUs_created.png)

### 2. Users Added to Organisational Units
![Users in OUs](screenshots/Users_added_to_OUs.png)

### 3. Security Group Members
![Security Groups](screenshots/members_added_to_GPO.png)

### 4. User Created with Corporate Email
![User with Email](screenshots/user_with_nexoratech_com.png)

### 5. Domain Join — Welcome Message
![Domain Join](screenshots/Domian_Joined.png)

### 6. Client Machine — Domain Verified
![Client Logged In](screenshots/client-user_logged_in.png)

### 7. GPO 1 — Linked and Enabled
![GPO Verified](screenshots/GP01_verify.png)

### 8. GPO 2 — Control Panel Blocked
![Control Panel Blocked](screenshots/GP02-working.png)

### 9. GPO 3 — Password Policy Settings
![Password Policy](screenshots/Password_policy_enabled.png)

### 10. GPO 3 — Password Policy Enforced
![Password Rejected](screenshots/Password_change_working.png)

### 11. GPO 4 — Screen Lock Configured
![Screen Lock](screenshots/lockscreen_enabled.png)

---

## 🛠️ Skills Demonstrated

```
✅ Windows Server 2016 Administration
✅ Active Directory Domain Services (AD DS)
✅ Organisational Unit (OU) Design
✅ User & Group Account Management
✅ UPN Suffix Configuration
✅ Group Policy Object (GPO) Creation & Linking
✅ GPO Testing & Verification
✅ NTFS Permissions & Access Control
✅ Network Shared Folder Configuration
✅ IP Addressing & Network Planning
✅ DNS Configuration & Management
✅ Domain Join & Authentication
✅ Least Privilege Security Principle
✅ IT Documentation & Technical Writing
✅ VirtualBox VM Deployment & Management
```

---

## Key Learnings

> "Building Nexora Technologies from scratch gave me deep practical understanding of how enterprise IT environments are structured. Configuring GPOs and verifying their enforcement taught me the importance of testing every policy change before rolling it out to production. Planning the IP addressing scheme before touching any device showed me why documentation-first thinking is critical in IT infrastructure work."
>
> — Joel Bless Addo

---

## Repository Structure

```
nexora-ad-lab/
├── README.md                    ← This file
├── DOCUMENTATION.md             ← Full technical documentation
├── IP-Addressing-Scheme.md      ← Network IP plan
└── screenshots/
    ├── OUs_created.png
    ├── Users_added_to_OUs.png
    ├── members_added_to_GPO.png
    ├── user_with_nexoratech_com.png
    ├── Domian_Joined.png
    ├── client-user_logged_in.png
    ├── GP01_verify.png
    ├── GP02-working.png
    ├── Password_policy_enabled.png
    ├── Password_change_working.png
    └── lockscreen_enabled.png
```

---

## 👤 Author

**Joel Bless Addo**
IT Support Specialist | Junior System Administrator
📍 Accra, Ghana
📧 addojoel283@gmail.com
🔗 [LinkedIn](https://www.linkedin.com/in/addojoelbless/)

---

*Built with dedication as part of a structured SysAdmin learning journey — targeting a cloud engineering role by 2027* 🚀
