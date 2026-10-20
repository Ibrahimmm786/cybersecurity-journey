# 🛠️ Day 01: OS Architecture, File Systems & Registry Hives

---

## 📌 Executive Summary
Understanding system low-level internals is the absolute core foundation of cybersecurity. Before leveraging high-level tools, a security analyst or threat hunter must understand how the underlying Operating System manages execution rings, process isolated spaces, file system paths, and system registry configurations.

---

## 🧠 1. OS Architecture & Execution Hierarchy

An Operating System manages execution privileges via CPU hardware protection rings:

* **Hardware & Firmware (UEFI/BIOS):** Initial bootstrap phase initializing essential devices and system control.
* **Kernel Space (Ring 0):** Highest execution privilege. Has unfiltered, direct access to CPU memory, device drivers, and system hardware.
* **User Space (Ring 3):** Isolated execution mode for non-administrative user apps. Applications cannot directly access hardware; they must issue **System Calls (Syscalls)** to request kernel-level operations.

### 🐧 Linux vs. 🪟 Windows Core Architecture
* **Windows:** A single integrated operating system binary built around the Windows NT Kernel architecture.
* **Linux:** Linux is strictly a open-source kernel. Distributions (Distros) like Kali Linux or Ubuntu bundle this core kernel with GNU user utilities and tools.

---

## 📁 2. File System Structure & System Paths

### 🪟 Windows File System Structure
* Path delimiter: Backslash (`\`)
* Base notation: Drive letters (`C:\`, `D:\`)
* **Key Directories:**
  * `C:\Windows\System32\` — Core OS system binaries and DLL libraries.
  * `C:\Program Files` — 64-bit application binaries.
  * `C:\Program Files (x86)` — 32-bit legacy application binaries.
  * `C:\Users\<Username>\AppData\` — Hidden per-user application storage and configurations.

### 🐧 Linux File System Structure (FHS Standard)
* Path delimiter: Forward slash (`/`)
* Base notation: Unified root tree starting at `/`
* **Key Directories:**
  * `/` — The Root directory of the entire system hierarchy.
  * `/etc` — Critical system configurations and local credentials storage.
  * `/var` — Variable data logs (e.g., `/var/log` for system log audits during incident response).
  * `/bin` & `/sbin` — Essential user and system administration command binaries.
  * `/root` — Home directory for the superuser (root account).

---

## 🔑 3. Windows Registry Hives & Security Implication

The Windows Registry acts as a hierarchical system database storing OS, software, driver, and active user configurations.

### Core Registry Hives:
1. **HKLM (HKEY_LOCAL_MACHINE):** Global machine configuration settings applicable across all users.
2. **HKCU (HKEY_CURRENT_USER):** Environment configurations specific to the currently logged-in user.
3. **HKCR (HKEY_CLASSES_ROOT):** Application file association bindings (e.g., `.txt` to Notepad).
4. **HKU (HKEY_USERS):** Loaded profiles for all active accounts on the machine.
5. **HKCC (HKEY_CURRENT_CONFIG):** Active hardware profile settings at runtime.

### ⚠️ Security Relevance: Persistence Attack Vectors
Threat actors often leverage startup registry keys to maintain access across machine reboots without detection:
* `HKLM\Software\Microsoft\Windows\CurrentVersion\Run`
* `HKCU\Software\Microsoft\Windows\CurrentVersion\Run`

*Auditing these specific registry paths is a fundamental step in system hardening and digital forensics.*
