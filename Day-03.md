# 🌐 Day 03: Networking Fundamentals — IP Addresses, DHCP, Ports & Protocols

---

## 📌 Executive Summary
Networking is the backbone of all digital communication. Understanding how IP addresses are structured, how networks allocate IP blocks hierarchically, how DHCP automates connectivity using real-world mechanics, and how communication ports operate is essential for both system administration and cybersecurity monitoring.

---

## 🌍 1. Introduction to Internet & IP Addresses
* **Internet:** It is a "network of networks" — a massive global connection of smaller local networks communicating with each other.
* **Protocols:** A set of common rules and languages that allow different computers and devices to understand each other.
* **IP Address:** A unique numerical identifier assigned to every device or network so it can be located and communicated with.

---

## 🔢 2. IPv4 vs. IPv6

### 📌 IPv4
* Uses **32-bit** addresses.
* Divided into **4 parts (octets)** separated by dots (e.g., `192.168.1.1`).
* Supports broadcast traffic.
* **Current Status:** IPv4 addresses are mostly exhausted globally because the number of connected devices has exploded.

### 📌 IPv6
* Uses **128-bit** addresses.
* Uses **colons (:)** for separation.
* Does not rely on traditional broadcasting; uses more efficient **multicast/anycast** methods.

---

## 🏗️ 3. IP Classes & Allocation Hierarchy

### 🏷️ IP Address Classes (A, B, C, D, E)
* **Class A, B, and C:** Used for regular commercial, home, and enterprise networking. *(Class C is most common for small home/office setups).*
* **Class D & E:** Reserved for special purposes (Multicast and Experimental research).

### 📈 Allocation Hierarchy (How IPs Flow Down)
1. **IANA (Internet Assigned Numbers Authority):** The central global authority managing IP address pools.
2. **RIRs (Regional Internet Registries):** Regional bodies that take blocks from IANA and manage them for continents (e.g., APNIC, RIPE NCC).
3. **ISPs (Internet Service Providers):** Local telecom/internet companies that get blocks from RIRs and distribute them to end-users (homes and businesses).

---

## 🏠 4. Private vs. Public IP Addresses
* **Private IP:**
  * Used inside local environments like a home Wi-Fi or an office network.
  * Devices within the same local network use private IPs to talk to each other.
  * Cannot be accessed directly from the public internet.
* **Public IP:**
  * Assigned by your ISP to your router.
  * Globally unique and routable across the entire internet. Accessible from anywhere in the world.

---

## 🏘️ 5. Network ID, Host ID, Subnet Mask & Special IPs
*Real-world analogy: Think of a physical street and houses.*

* **Network ID (The Street Name):** Identifies the specific local network or segment (e.g., `192.168.1.0`).
* **Host ID (The House/Device Number):** Identifies the exact individual device inside that network (e.g., `.45` in `192.168.1.45`).
* **Subnet Mask (The Stencil):** A mask (like `255.255.255.0`) that tells the computer which part of the IP address is the Network ID (`255`) and which part is the Host ID (`0`).

### ⚙️ Special IPs in a Network Range:
* **Network Address (First IP):** Represents the network itself (e.g., `192.168.1.0`). Cannot be assigned to a device.
* **Broadcast Address (Last IP):** Used to send a message to all devices on the local network simultaneously (e.g., `192.168.1.255`). Cannot be assigned to a device.
* **Usable Hosts:** All the IP addresses lying between the first and last IP (e.g., `192.168.1.1` to `192.168.1.254`) that can be safely assigned to PCs, phones, and servers.

---

## ⚙️ 6. DHCP (Dynamic Host Configuration Protocol)
* **What is DHCP?** A network protocol that automatically assigns IP addresses, subnet masks, default gateways, and DNS settings to devices on a network so humans don't have to configure them manually.
* **Layman Example (The Restaurant Waiter):** Imagine you walk into a restaurant (the network) and sit at a table. You don't bring your own table number; instead, a waiter (DHCP Server) comes up to you and automatically assigns you an available table number (IP address) so you can place your order.

### 🔄 How DHCP Works (The D.O.R.A. Process)
* **D - Discover:** Your device shouts out on the network: *"Is there any DHCP server here? I need an IP address!"* (Broadcast)
* **O - Offer:** The DHCP Server replies: *"Yes, I am here! I have an IP address `192.168.1.15` available, do you want it?"*
* **R - Request:** Your device responds: *"Yes, please lock in `192.168.1.15` for me!"*
* **A - Acknowledge:** The DHCP Server confirms: *"Done! That IP is officially yours for the next 24 hours (Lease Time)."*

### ⚠️ Cybersecurity Perspective (Why DHCP Matters)
* **Rogue DHCP Server Attack:** A hacker connects a fake unauthorized DHCP server to the network. When new devices join, the fake server hands out malicious gateway addresses, routing all user internet traffic through the hacker's machine (leading to a **Man-in-the-Middle Attack**).
* **DHCP Starvation Attack:** An attacker floods the genuine DHCP server with fake connection requests until all available IP addresses are exhausted, denying legitimate users network access (**Denial of Service**).

---

## 🚪 7. Ports & Protocols
* **What is a Port?** A virtual communication endpoint. When a client wants to talk to a server, the client initiates contact from a random free port to the server's specific fixed service port.

### 📌 Port Ranges
* **Well-Known Ports (0 - 1023):** Reserved for standard core services (e.g., HTTP, HTTPS, SSH, FTP).
* **Registered Ports (1024 - 49151):** Assigned to specific vendor applications/software.
* **Dynamic / Private / Ephemeral Ports (49152 - 65535):** Randomly picked by the operating system for temporary client-side outbound communication.

### 🔗 The Communication 4-Tuple
Every active network connection requires 4 key pieces of information:
1. **Source IP** (Client's IP)
2. **Source Port** (Client's temporary random port)
3. **Destination IP** (Server's IP)
4. **Destination Port** (Server's well-known service port)

### 🔒 HTTP vs. HTTPS
* **HTTP (Port 80):** Unencrypted plain-text communication; data can be easily intercepted by eavesdroppers.
* **HTTPS (Port 443):** Encrypted secure communication (the 'S' stands for Secure/SSL/TLS) protecting data in transit.
