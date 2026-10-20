# 🛡️ Day 02: Active Directory Architecture, Enterprise Logging & Shell Reconnaissance

---

## 📌 Executive Summary
Enterprise infrastructure relies heavily on Active Directory (AD) for identity management and centralized administrative control. For cybersecurity practitioners, understanding AD internals, authentication protocols, centralized logging (Syslog/Event Viewer), and native command-line auditing is critical for both offensive threat modeling and defensive threat hunting.

---

## 🏢 1. Local Architecture vs. Active Directory Domain Architecture

### 💻 Local Accounts & Standalone Systems
* **Storage:** Credentials and local security policies are stored independently on each endpoint within the local **Security Account Manager (SAM)** database (`C:\Windows\System32\config\SAM`).
* **Limitations:** Non-scalable in enterprise settings. Administrative changes, password resets, and policy updates must be applied manually on every machine.

### 🏛️ Active Directory Domain Architecture
* **Domain Controller (DC):** The central server running Active Directory Domain Services (ADDS). It hosts the **NTDS.dit** database containing identity credentials, policy configurations, and domain relationship maps.
* **Centralized Management:** Enables automated security patching, software distribution, and access control across thousands of domain-joined endpoints simultaneously.
* **Objects & OUs:** Users, computers, groups, and devices are represented as **Objects**, organized logically into **Organizational Units (OUs)** based on department structures (e.g., `IT`, `Finance`, `HR`).

---

## 🔑 2. Enterprise Authentication Protocols

### 🎟️ Kerberos (Modern & Secure Default)
* A ticket-based, stateless authentication protocol relying on a trusted third party—the **Key Distribution Center (KDC)** hosted on the Domain Controller.
* **Authentication Flow:**
  1. **Authentication Service (AS) Request:** Client sends encrypted timestamp using their password hash to the KDC.
  2. **TGT Issuance:** KDC validates the identity and responds with a **Ticket Granting Ticket (TGT)**.
  3. **TGS Request & Response:** Client uses the TGT to request a Service Ticket (ST) from the Ticket Granting Service (TGS).
  4. **Service Access:** Client presents the Service Ticket to access the target network resource.
* **Security Context:** Passwords are never sent across the wire in plaintext or raw hash form.

### 🔒 NTLM / NetNTLM (Legacy Protocol)
* A challenge-response authentication mechanism storing password representations as NTLM hashes.
* **Security Implications:** Highly vulnerable to credential dumping, relay attacks, and **Pass-the-Hash (PtH)** exploitation. Disabling NTLM in favor of Kerberos is a core hardening requirement.

---

## ⚔️ 3. Cybersecurity Perspective: Red Team vs. Blue Team Dynamics

### 🔴 Red Team (Offensive Angle)
* **Primary Target:** The Domain Controller is the ultimate Crown Jewel. Compromising the DC grants access to `NTDS.dit`, allowing attackers to extract all domain password hashes and achieve persistent enterprise dominance.
* **Common Attack Vectors:** Misconfigured Group Policies, Kerberoasting (harvesting service tickets), Pass-the-Hash attacks, and privilege escalation via over-privileged user accounts.

### 🔵 Blue Team (Defensive Angle)
* **DC Defense & Hardening:** Enforces strict Kerberos ticket lifespans, monitors anomalous domain logins, audits group membership changes (e.g., unexpected additions to `Domain Admins`), and applies Tiered Administration models.
* **Least Privilege Enforcement:** Restricts administrative access so users only possess permissions required for their specific organizational roles.

---

## 📊 4. Logging & Enterprise Monitoring Architecture

### 📑 Windows Event Viewer
* Built-in Windows security auditing tool that structures system, application, and security event logs.
* **Critical Windows Security Event IDs to Audit:**
  * **Event ID 4624:** Successful account logon.
  * **Event ID 4625:** Failed account logon (potential brute-force attempt).
  * **Event ID 4720:** A user account was created (potential backdoor account creation).
  * **Event ID 4728 / 4732:** A member was added to a security-enabled global/local group (e.g., Domain Admins).
  * **Event ID 4688:** A new process has been created (essential for process tracking).

### 🌐 Syslog & Centralized Security Information Management (SIEM)
* **Syslog Standard:** Standard protocol used by Linux servers, firewalls, routers, and switches to format and transmit event messages across a network.
* **Centralized Logging:** Forwarding logs from thousands of endpoints to a central logging server or SIEM (Security Information and Event Management) system.
* **Threat Hunting Value:** Allows Blue Teams to trace attacker entry points, visualize lateral movement across machines, detect persistence mechanisms, and establish full incident timelines.

---

## 💻 5. Native Shell Reconnaissance & System Auditing

Command-line utilities for quick endpoint investigation during incident response or security assessments:

### 🪟 Windows PowerShell / CMD Commands
* `whoami /all` — Displays current user identity, active security groups, and privilege tokens.
* `ipconfig /all` — Detailed network interface configurations, DNS servers, and MAC addresses.
* `Get-NetIPAddress` / `Get-NetRoute` — PowerShell cmdlets for routing table and IP audits.
* `netstat -ano` — Shows active network connections, listening ports, and corresponding Process IDs (PIDs).
* `Get-Process` / `tasklist` — Lists all currently running processes for behavior inspection.
* `Get-EventLog -LogName Security -Newest 20` — Retrieves recent security event logs via PowerShell.
* `net group "Domain Admins" /domain` — Enumerates all accounts with domain administrator privileges.

### 🐧 Linux Bash Commands
* `whoami` / `id` — Inspects user ID, group memberships, and current privilege context.
* `ip addr` / `ifconfig` — Displays network interfaces and assigned IP configurations.
* `ss -tuln` / `netstat -tuln` — Lists active listening TCP/UDP sockets and network bindings.
* `ps aux` — Displays detailed snapshot of all active system processes.
* `tail -f /var/log/auth.log` — Monitors live authentication attempts and SSH logins on Linux.

---

## 🎯 Key Takeaways
1. Active Directory provides centralized enterprise control, making the Domain Controller the primary focal point for both attackers and defenders.
2. Kerberos is ticket-based and secure; NTLM relies on challenge-response hashes and is vulnerable to lateral movement attacks.
3. Centralized logging (Syslog/SIEM) and event log tracking (Event Viewer) are vital for detecting unauthorized changes, account creation, and network intrusion.
