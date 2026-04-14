#  Capstone: Enterprise Active Directory Deployment

> * Windows Infrastructure and Security*
> 

---

## 🎯 The Mission

This was the culminating project of the course — designing and deploying a **complete enterprise Active Directory environment** that mirrors a real software development company. Everything from domain architecture to project-team-scoped file permissions was implemented from scratch, collaboratively, on live virtual infrastructure.

This project demonstrates not just technical depth, but the ability to **translate a business org chart into a working AD design**.

---

## 🏢 The Business Case

**Company:** A software firm with multiple development teams  
**Teams:** Android App team, iOS App team  
**Roles:** Software Developers, System Integrators, Database Analysts, System Architects, Administrators, Software Development Director

The challenge: design an AD environment where users can access exactly what they need for their role and project — and nothing more.

---

## 🔧 Infrastructure

| Component | VM | Role |
|-----------|-----|------|
| Domain Controller | VM1 | AD DS, DNS, GPMC |
| File Server | VM2 | SMB Shares, NTFS Permissions |
| Domain | `8917862-8461A2.Local` | |
| File Server Drive | E: (new virtual disk) | Project folders |

VM2 was joined to the domain by setting its DNS to point to VM1 — a clean, deliberate configuration.

---

## 📋 Task 1 — Job Function AD Objects

### Organizational Unit Design

```
8917862-8461A2.Local
└── Employees OU
    ├── Users
    │   ├── devuser1, devuser2, devuser3, devuser4  (Software Developers)
    │   ├── sysuser1, sysuser2                      (System Integrators)
    │   ├── datauser1                               (Database Analyst)
    │   ├── archuser1                               (System Architect)
    │   ├── group-da                                (Administrator)
    │   └── dev-director                            (Software Development Director)
    └── Groups
        ├── 8917862-Software Developers
        ├── 8917862-System Integrators
        ├── 8917862-Database Analyst
        ├── 8917862-System Architect
        └── 8917862-software director
```

Each user was placed in their corresponding role group. The student number prefix in group names ensures uniqueness in shared lab environments and demonstrates naming convention discipline.

### Why Role Groups Matter

> Adding users directly to file share permissions is a maintenance nightmare. When devuser3 leaves, someone has to manually audit every folder they had access to. With role groups, you remove them from one group — and their access disappears everywhere, instantly.

### Screenshots

**Organizational Unit structure created**

![OU Creation](assets/image1.PNG)

**Software Developers group created**

![Software Developers Group](assets/image2.png)

**Database Analyst and System Architect groups created**

![DB and Architect Groups](assets/image3.png)

**System Integrators users created**

![System Integrators](assets/image4.png)

**All 5 role groups visible in AD**

![All Groups](assets/image5.png)

**datauser1 added to Database Analyst group**

![DB Analyst Group Membership](assets/image6.PNG)

**sysuser1/2 added to System Integrators group**

![System Integrator Membership](assets/image7.png)

**devuser1-4 added to Software Developers group**

![Developer Group Membership](assets/image8.PNG)

---

## 📋 Task 2 — Project Team AD Objects

### Project-Scoped Groups

Two project teams were created, each drawing from the role groups:

| Group | Members |
|-------|---------|
| **Android Project** | devuser1, devuser2 (Developers) · sysuser1 (Integrator) · archuser1 (Architect) · datauser1 (DB Analyst) |
| **iOS Project** | devuser3, devuser4 (Developers) · sysuser2 (Integrator) · archuser1 (Architect) · datauser1 (DB Analyst) |

Note: `archuser1` and `datauser1` are in **both** project groups — reflecting the business reality that senior technical roles (System Architect, Database Analyst) support multiple projects simultaneously.

### Screenshots

**Android Project group created**

![Android Group Creation](assets/image9.png)

**iOS Project group created**

![iOS Group Creation](assets/image10.png)

**Users added to Android project**

![Android Members](assets/image11.PNG)

**Users added to iOS project**

![iOS Members](assets/image12.PNG)

**Software Developer group structure confirmed**

![Developer Group Confirmed](assets/image13.PNG)

---

## 📋 Task 3 — Folder Structure and Network Shares

### The File Server Layout

```
E:\
└── Projects\                    ← Shared at \\FILESERVER\Projects
    ├── Android\
    │   └── Brainstorming\
    └── IOS\
        └── Brainstorming\
```

A dedicated `Brainstorming` subfolder under each project folder allows team members to collaborate on early-stage ideas in a separate space from the main project files — a clean organizational pattern.

**Access-Based Enumeration** was enabled on the Projects share. This means:
- Android team members see only the Android folder
- iOS team members see only the iOS folder
- Neither team sees the other's folder at all — not even as a grayed-out entry

### Screenshots

**Projects folder created on E: drive**

![Projects Folder](assets/image14.png)

**Android and iOS subfolders created**

![Subfolders Created](assets/image15.png)

**Projects folder shared across the network**

![Network Share](assets/image16.PNG)

**Brainstorming folder created under Android**

![Android Brainstorming](assets/image17.PNG)

**Brainstorming folder created under iOS**

![iOS Brainstorming](assets/image18.PNG)

**Access-Based Enumeration enabled on Projects share**

![ABE Enabled](assets/image19.PNG)

---

## 📋 Task 4 — NTFS Permissions

### The Permission Matrix

| Principal | Android Folder | iOS Folder |
|-----------|---------------|------------|
| 8917862-Android Project | Read/Write/Execute | ❌ No Access |
| 8917862-iOS Project | ❌ No Access | Read/Write/Execute |
| 8917862-software director | Read Only | Read Only |

The Software Development Director has read-only access to both projects — appropriate for executive oversight without the ability to accidentally modify project files.

### Screenshots

**NTFS permissions on Projects folder**

![NTFS Permissions](assets/image20.png)

**Android folder permissions — Android group only**

![Android Folder Perms](assets/image21.png)

**iOS folder permissions — iOS group only**

![iOS Folder Perms](assets/image22.PNG)

**Director read access to both folders**

![Director Access](assets/image23.png)

**NTFS inheritance and ACE configuration**

![ACE Config](assets/image24.PNG)

**Final permission verification**

![Final Verification](assets/image25.PNG)

---

## 📋 Task 5 — Advanced Share Configurations

Additional configurations applied to complete the enterprise setup:

- Share permissions aligned with NTFS (NTFS is the effective control)
- UNC path access verified from client machines
- Folder ownership and audit settings reviewed

### Screenshots

![Share Config 1](assets/image26.PNG)

![Share Config 2](assets/image27.png)

![Share Config 3](assets/image28.png)

![Final Share Verification](assets/image29.png)

![Advanced Config](assets/image30.png)

![Config Complete](assets/image31.png)

![Project Summary](assets/image32.png)

---

## 📊 Architecture Summary

```
Employees OU
  │
  ├── Role Groups (5)           ← Job function identity
  │     └── Project Groups (2) ← Project-scoped access
  │
File Server (VM2)
  │
  └── E:\Projects\
        ├── Android\  ← ACE: Android Project group R/W/X
        │                 Director group: Read
        └── IOS\      ← ACE: iOS Project group R/W/X
                          Director group: Read

Access-Based Enumeration: ON (users see only their project)
```

---

## 💡 What This Project Demonstrates

1. **End-to-end AD design** — from business requirements to working infrastructure, translated cleanly into AD objects.

2. **Nested group strategy** — role groups nest into project groups, providing flexibility as people join/leave teams or projects change.

3. **NTFS + Share permission alignment** — NTFS is the effective security layer; Share permissions are set to "Allow Everyone Full Control" at the share level, with NTFS handling actual access control. This is the correct enterprise pattern.

4. **Access-Based Enumeration** — applied professionally to prevent information leakage about projects users aren't part of.

5. **Scalability** — this architecture scales. Adding a new project? Create a group, create a folder, set ACE. Adding a new developer? Add them to a role group and optionally a project group. Nothing else needs to change.

---

[← Lab 05: AD Backup](../05-ad-backup-and-recovery/README.md) | [Next: Security Hardening Exam →](../07-security-hardening-exam/README.md)
