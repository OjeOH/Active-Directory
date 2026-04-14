# 🖥️ Windows Infrastructure & Active Directory Project

> **Oje Opeoluwa Hezekiah** — Windows Systems Engineer | Active Directory | Security Hardening | Enterprise Infrastructure
>
> * Windows Infrastructure and Security*

---

## 👋 About This Portfolio

This repository documents a hands-on journey through building, securing, and administering enterprise-grade Windows infrastructure from the ground up. Every lab, every screenshot, and every configuration you see here was performed live on virtual machines, no simulation, no theory-only exercises.

From provisioning a bare Windows Server 2019 VM all the way through WSUS patch management, disaster recovery, and Microsoft Security Compliance Toolkit hardening, this portfolio represents 7 interconnected projects that mirror real-world IT engineering responsibilities.

If you're an employer, hiring manager, or fellow engineer — **welcome**. This is what I do.

---

## 🗺️ Project Map

| # | Module | Core Skills Demonstrated |
|---|--------|--------------------------|
| [01](./01-active-directory-foundations/) | **Active Directory Foundations** | DC promotion, OU design, user/group management, domain joining |
| [02](./02-file-system-permissions/) | **File System Permissions & NTFS** | NTFS ACLs, RBAC design, access-based enumeration, deny overrides |
| [03](./03-group-policy-management/) | **Group Policy Management** | GPO creation, linking, enforcement, precedence, security filtering |
| [04](./04-wsus-patch-management/) | **WSUS Patch Management** | WSUS installation, computer groups, update approvals, IIS integration |
| [05](./05-ad-backup-and-recovery/) | **AD Backup & Authoritative Restore** | System state backup, safe mode, ntdsutil, OU recovery |
| [06](./06-capstone-enterprise-project/) | **Capstone: Enterprise AD Deployment** | End-to-end multi-VM domain, role groups, project-scoped permissions |
| [07](./07-security-hardening-exam/) | **Security Hardening Exam** | Microsoft MSCT toolkit, CIS-aligned GPOs, password policy, endpoint hardening |

---

## 🛠️ Technology Stack

```
Operating System   →  Windows Server 2019
Virtualization     →  VMware vSphere
Directory Services →  Active Directory Domain Services (AD DS)
Policy Management  →  Group Policy Management Console (GPMC)
Patch Management   →  Windows Server Update Services (WSUS)
File Services      →  NTFS, SMB Shares, Access-Based Enumeration
Security Toolkit   →  Microsoft Security Compliance Toolkit (MSCT)
Backup & Recovery  →  Windows Server Backup, ntdsutil, Safe Mode Restore
```

---

## 🏗️ Infrastructure Overview

All labs were built on an isolated virtual network (`10.172.136.0/24`) in VMware vSphere, with multiple interconnected Windows Server 2019 VMs playing distinct roles:

```
┌─────────────────────────────────────────────────────────┐
│                  ooje8461Labs.local Domain               │
│                                                          │
│  ┌───────────────────┐     ┌───────────────────────────┐│
│  │  Domain Controller │     │     Member Servers        ││
│  │  10.172.136.2      │────▶│  .3  Server 02            ││
│  │  (AD DS, DNS,      │     │  .23 ooje8461Labs-03      ││
│  │   GPMC, Backup)    │     │  .60 FILESERVER           ││
│  └───────────────────┘     │  .45 WSUS DC              ││
│                             │  .65 WSUS Member Server   ││
│                             └───────────────────────────┘│
│                                                          │
│  ┌───────────────┐                                       │
│  │  Client VM    │                                       │
│  │  10.172.136.10│                                       │
│  └───────────────┘                                       │
└─────────────────────────────────────────────────────────┘
```

---

## 📊 Skills Matrix

| Skill | Evidence |
|-------|----------|
| Active Directory Design | Labs 01, 06 |
| Group Policy Objects  | Labs 03, 07 |
| NTFS / RBAC Permissions  | Lab 02 |
| WSUS / Patch Management  | Lab 04 |
| Disaster Recovery  | Lab 05 |
| Security Hardening (MSCT)  | Lab 07 |
| Windows Server Administration  | All labs |
| VMware vSphere  | All labs |

---

## 📂 Repository Structure

```
📁 ad-portfolio/
├── 📄 README.md                          ← You are here
├── 📁 01-active-directory-foundations/
│   ├── 📄 README.md
│   └── 📁 assets/  (24 screenshots)
├── 📁 02-file-system-permissions/
│   ├── 📄 README.md
│   └── 📁 assets/  (26 screenshots)
├── 📁 03-group-policy-management/
│   ├── 📄 README.md
│   └── 📁 assets/  (16 screenshots)
├── 📁 04-wsus-patch-management/
│   ├── 📄 README.md
│   └── 📁 assets/  (8 screenshots)
├── 📁 05-ad-backup-and-recovery/
│   ├── 📄 README.md
│   └── 📁 assets/  (14 screenshots)
├── 📁 06-capstone-enterprise-project/
│   ├── 📄 README.md
│   └── 📁 assets/  (32 screenshots)
└── 📁 07-security-hardening-exam/
    ├── 📄 README.md
    └── 📁 assets/  (11 screenshots)
```

---

## 📬 Contact

**Oje Opeoluwa Hezekiah**
- 📧 opeoluwaoje1@gmail.com | [Linkeldn](www.linkedin.com/in/opeoluwaoje)
- 🎓 Conestoga College
- 📍 Waterloo, Ontario, Canada
  

---

*All configurations, screenshots, and reflections in this portfolio represent original work completed in a live lab environment.*
