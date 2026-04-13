# 02 — File System Permissions & NTFS Security

> *Lab 2 (Part A) · INFO 8461 · Windows Infrastructure and Security*

---

## 🎯 The Mission

Access control is the backbone of data security. In this lab, I moved beyond user creation and into the practical reality of **who can see what, and why**. Using NTFS permissions and Role-Based Access Control (RBAC), I built a layered permission model that mirrors what you'd find in a real corporate file server environment.

---

## 🔧 Environment

| Component | Value |
|-----------|-------|
| Domain Controller | `10.172.136.2` |
| File Server (Member Server) | `10.172.136.60` (FILESERVER) |
| Client VM | `10.172.136.10` |
| File System | NTFS on shared drives |

---

## 📋 Part 1 — Configure and Test NTFS File System Permissions

### The Setup

I created the following AD objects in a `Kitchener/Users` OU on the domain controller:

- **Users:** Jane Doe, John Jones, Bob Smith
- **Groups:** Group1 (John Jones), Group2 (Jane Doe), Group3

This gave me a controlled test environment to prove exactly how NTFS permissions behave — including some surprises.

### What I Discovered (The Interesting Parts)

**Observation 1: ACE Inheritance**

Opening the advanced security settings on `Data1`, I could see the full Access Control Entry (ACE) chain — including which entries were inherited from the parent folder vs. explicitly set. This distinction matters enormously when troubleshooting unexpected access.

**Observation 2: John Creates, Jane Can Read But Not Modify**

- Logged in as **John Jones** → created a file and folder in `Data1` ✅
- Logged in as **Jane Doe** → could *see* John's file and *read* its content ✅, but could **not** modify it ❌
- Root cause: The ACE with principal `Users (//FILESERVER/Users)` granted **Read and Execute** inherited from `E:\` — exactly enough to view, not enough to edit.

**Observation 3: The Deny Override**

When I created a **Deny** permission on `Data2`, it immediately overrode all existing Allow permissions for that principal. This is one of the most important — and most dangerous — behaviors in NTFS:

> ⚠️ **Deny always wins over Allow**, regardless of group membership or inheritance order.

After verifying this behavior, I removed the Deny ACE and proceeded with constructive access control.

**Observation 4: Group-Isolated Folders**

Created separate ACEs for Group1 and Group2 under `Data2`:
- **Group1 ACE** (Read, Write, Execute) → Jane Doe (Group2 member) had **no access** to Group1 folder
- **Group2 ACE** (Read, Write, Execute) → John Jones (Group1 member) had **no access** to Group2 folder

Finally, enabled **Access-Based Enumeration** — so users don't even *see* folders they can't access. Clean, professional, and secure.

### Screenshots

**Creation of users and groups in AD**

![Objects Created](assets/image1.png)

**Security permissions noted on Data1**

![Data1 Security Principals](assets/image2.PNG)

![Permission Details](assets/image3.PNG)

![More Permission Details](assets/image4.PNG)

**Advanced NTFS settings on Data1**

![Advanced Settings](assets/image5.PNG)

**ACE allowing Read and Execute for Users**

![Read Execute ACE](assets/image6.PNG)

**Effective permissions for John Jones**

![John Jones Effective Access](assets/image7.PNG)

**Special permissions on Users ACE**

![Special Permissions](assets/image8.PNG)

**John Jones creates file and folder in Data1**

![John Creates Files](assets/image9.PNG)

**Jane Doe's access — can view but not modify John's content**

![Jane Doe Access Test](assets/image10.PNG)

**Result of adding a Deny permission — overrides Allow**

![Deny Permission Result](assets/image11.PNG)

**Removal of the special access on Data2**

![Deny Removed](assets/image12.PNG)

**Group1 ACE created on Data2**

![Group1 ACE](assets/image13.PNG)

**Jane Doe's effective permissions after Group1 ACE — no access**

![Jane No Access to Group1](assets/image14.PNG)

**Group2 ACE created on Data2**

![Group2 ACE](assets/image15.PNG)

**John Jones' access after Group2 ACE — blocked**

![John No Access to Group2](assets/image16.PNG)

**Access-Based Enumeration enabled**

![ABE Enabled](assets/image17.PNG)

---

## 📋 Part 2 — RBAC on Publications Folder

### The Business Scenario

A more realistic setup: two departments (**HR** and **Operations**), each with their own publication folders, managed via a layered RBAC group model. This pattern — resource access groups nested in role groups — is how enterprise AD environments should be structured.

### The RBAC Model

```
Publications/
├── HR/          ← accessible by AC_HR_Pubs_RW
└── Operations/  ← accessible by AC_Operations_Pubs_RW

Role groups:
HR_Pubs_Maintainers        → member of → AC_HR_Pubs_RW
Operations_Pubs_Maintainers → member of → AC_Operations_Pubs_RW

Department groups:
HR_Staff         (HR user1, HR user2)
Operations_Staff (Ops user1, Ops user2)
```

**HR user1** → HR_Pubs_Maintainers → AC_HR_Pubs_RW → **Read/Write/Execute on HR/**

This nested group model means that when someone joins or leaves the maintenance role, you change one group membership — not 50 ACEs on 50 folders.

### Screenshots

**Groups created in AD**

![Objects Created](assets/image18.png)

**Inheritance disabled on Publications folder**

![Disable Inheritance](assets/image19.PNG)

**ACE created on Publications/HR folder**

![HR Folder ACE](assets/image20.PNG)

![HR Folder Permissions Detail](assets/image21.PNG)

![Operations ACE](assets/image22.PNG)

---

## 📋 Part 3 — RBAC on Teams

### Extending the Model

Applied the same RBAC pattern to a project-team structure, demonstrating that the model scales cleanly from departments to project teams without redesigning the permission architecture.

### Screenshots

![Team RBAC Setup](assets/image23.PNG)

![Team Permissions](assets/image24.PNG)

![Team Access Verified](assets/image25.PNG)

![Final RBAC Configuration](assets/image26.PNG)

---

## 💡 Key Takeaways

**NTFS mastery that translates directly to the job:**

1. **Deny overrides Allow** — always. Understanding this prevents catastrophic misconfiguration.

2. **Access-Based Enumeration** isn't just cosmetic — it prevents users from discovering the existence of resources they can't access, reducing information leakage.

3. **RBAC via nested groups** (Role Group → Access Control Group → ACE) is the enterprise standard. It makes permission changes a one-line operation instead of a folder-by-folder audit.

4. **Effective permissions matter more than individual ACEs** — I used the Effective Access tab consistently to verify real-world access, not just theoretical ACE assignments.

---

[← Lab 01: AD Foundations](../01-active-directory-foundations/README.md) | [Next: Group Policy →](../03-group-policy-management/README.md)
