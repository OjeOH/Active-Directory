# Active Directory Foundations

> * Windows Infrastructure and Security*

---

## 🎯 The Mission

Every enterprise Windows environment begins with one critical decision: **setting up the Domain Controller**. This lab depicts the first process with that foundational task, building a working Active Directory domain from a blank Windows Server 2019 VM, then expanding it by joining member servers.

Think of this as laying the concrete slab that every other lab in this portfolio stands on.

---

## 🔧 Environment Setup

| Component | Value |
|-----------|-------|
| Domain Name | `ooje8461Labs.local` |
| Domain Controller IP | `10.172.136.2` |
| Default Gateway | `10.172.136.1` |
| Subnet | `255.255.255.0` |
| Server 02 IP | `10.172.136.3` |
| Server 03 IP | `10.172.136.23` |
| Platform | VMware vSphere / Windows Server 2019 |

---

## 📋 Part 1 — Build the Domain Controller

### What Was Done

Starting from a blank Windows Server 2019 VM, I:

1. Installed the **Active Directory Domain Services (AD DS)** role via Server Manager
2. Promoted the server to a **Domain Controller** for the new domain `ooje8461Labs.local`
3. Designed and created a logical **OU hierarchy** under the domain:

```
ooje8461Labs.local
└── Accounts
    ├── Service Accounts
    │   └── [Group] 8461Labs-joiners
    │   └── [User]  joiner01
    ├── Administrators
    │   └── [User]  ooje-da  ← Domain Admin account
    └── Employees
        └── [User]  ooje
```

4. Added `ooje-da` to the **Domain Admins** built-in group — following the principle of separation between a personal account and an administrative account.

### Why This Structure Matters

Separating users into OUs by function (service accounts vs. administrators vs. employees) isn't just tidy — it's a prerequisite for applying targeted Group Policy Objects, delegating control safely, and auditing access effectively. A well-designed OU structure is the difference between a manageable domain and an administrative nightmare.

### Screenshots

**Installing Active Directory Domain Services**

![AD DS Installation](assets/image1.png)

**Server 01 promoted to Domain Controller**

![Domain Controller Promoted](assets/image2.png)

**Account OU created under the domain**

![Account OU Creation](assets/image3.PNG)

**Service Accounts, Administrators, and Employees OUs created**

![Sub-OUs Created](assets/image4.png)

**Global Security Group created in Service Accounts OU**

![Security Group Creation](assets/image5.PNG)

**8461Labs-joiners group visible under Service Accounts**

![Group in Service Accounts](assets/image6.png)

**User joiner01 created**

![joiner01 User Creation](assets/image7.png)

**joiner01 added to 8461Labs-joiners group**

![User Added to Group](assets/image8.png)

**Employee user account (ooje) created**

![Employee User Created](assets/image9.png)

**Administrative user (ooje-da) created and added to Domain Admins**

![Admin User Created](assets/image10.png)

---

## 📋 Part 2 — Join a Member Server

### What Was Done

With the domain running, I deployed a second VM (`Server 02` at `10.172.136.3`) and joined it to the domain — simulating how real organizations expand their infrastructure.

Key steps:
- Created a **local account** called `Service Desk` on Server 02
- Changed Server 02's **DNS server to point to the DC** (10.172.136.2) — critical for domain resolution
- Joined Server 02 to the domain using the **service account `joiner01`** (demonstrating the purpose of that account from Part 1)
- Verified login using both **domain accounts** and the **local account**

> 💡 **Key Lesson:** Using a dedicated service account (`joiner01`) to join machines to the domain — rather than a full Domain Admin — follows the **principle of least privilege**. The joiner account only has the rights needed to join computers, nothing more.

### Screenshots

**Local Service Desk account created on Server 02**

![Local Account Creation](assets/image11.png)

**DNS changed to point to the Domain Controller**

![DNS Configuration](assets/image12.png)

**Domain resolution confirmed**

![Resolve Confirmation](assets/image13.PNG)

**Server 02 joined to domain using joiner01 service account**

![Domain Join](assets/image14.png)

**Login to Server 02 with domain account (post-reboot)**

![Domain Account Login](assets/image15.png)

**Login to Server 02 with second domain account**

![Second Domain Login](assets/image16.png)

**Login to Server 02 with local account**

![Local Account Login](assets/image17.png)

---

## 📋 Part 3 — Second Member Server

### What Was Done

Repeated the domain-join process for a third VM (`ooje8461Labs-03` at `10.172.136.23`) — reinforcing the workflow and verifying the domain's ability to support multiple member servers.

### Screenshots

**Third VM domain join and configuration**

![Server 03 Config](assets/image18.png)

![Server 03 Login](assets/image19.png)

![Server 03 Local Account](assets/image20.png)

![Server 03 Domain Resolution](assets/image21.PNG)

![RDP Access to Domain](assets/image22.png)

---

## 💡 Key Takeaways

**What I learned that will make me effective on day one:**

1. **AD DS deployment** — I can provision a Windows Server domain from scratch, including role installation, DC promotion, and DNS configuration.

2. **OU architecture** — I understand why flat AD structures become unmanageable at scale and how to design hierarchical OUs that support RBAC, GPO targeting, and delegation.

3. **Service accounts** — Using `joiner01` to join machines (rather than an admin account) isn't just best practice — it's auditable, revocable, and follows least-privilege principles.

4. **Troubleshooting** — I encountered and resolved login errors when accessing the member server, which required research into remote access permissions. Real experience beats a textbook every time.

---

[← Back to Portfolio](../README.md) | [Next: File System Permissions →](../02-file-system-permissions/README.md)
