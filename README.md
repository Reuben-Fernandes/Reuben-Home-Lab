# 🛡️ Cybersecurity Homelab Deployment – Hands-On Security Engineering Lab

This project simulates a realistic enterprise network environment to develop skills in **network segmentation**, **log analysis**, and **security controls implementation**. Built on **Proxmox**, the lab includes multiple operating systems, offensive testing, and centralized log aggregation via Splunk.

---

## 🔧 Deployment Phases & Stack Overview

### Phase 1 – Network & Perimeter Security
- **pfSense Firewall**
  - 🔹 Manages **VLAN segmentation** across multiple subnets
  - 🔹 Provides **DHCP** to each VLAN (no reliance on Windows)
  - 🔹 Acts as **internal DNS resolver**
  - 🔹 Enforces **firewall rules** to simulate restricted zones
  - 🔹 Logs firewall traffic and events to **Splunk via syslog**

- **Kali Linux**
  - 🔹 Offensive testing tools: `Nmap`, `Hydra`, `Metasploit`, others
  - 🔹 Simulates attacker behavior to test detection across the environment
  - 🔹 Used to generate realistic log events and attack traffic

---

### Phase 2 – Log Aggregation & Visibility
- **Splunk (Free Tier)**
  - 🔹 Ingests logs from:
    - pfSense firewall
    - Windows Server & Windows 10
    - Docker containers (optional)
    - Nessus scans
  - 🔹 Built custom dashboards and correlation rules
  - 🔹 Use cases explored:
    - Port scanning
    - Failed login attempts
    - Privilege escalation
    - Suspicious PowerShell usage

---

### Phase 3 – Vulnerable Targets
- **Ubuntu Server (Docker Host)**
  - 🔹 Runs intentionally vulnerable web apps:
    - `WebGoat`, `DVWA`, `bWAPP`
  - 🔹 Containerized to practice Docker networking and isolation
  - 🔹 Used to simulate low-hanging fruit commonly found during assessments

- **Metasploitable 2**
  - 🔹 Classic vulnerable Linux VM
  - 🔹 Used to test exploit delivery, lateral movement, and enumeration

- **Nessus Essentials**
  - 🔹 Internal vulnerability scanning
  - 🔹 Validates configuration gaps
  - 🔹 Helps simulate client environment assessment scenarios

---

### Phase 4 – Windows Enterprise Setup
- **Windows Server 2022**
  - 🔹 Configured as Active Directory Domain Controller
  - 🔹 Services enabled:
    - Domain Services
    - Group Policy Management (GPO)
    - DNS (limited to domain lookups)
  - 🔹 Logs ingested into Splunk:
    - Logon events
    - PowerShell usage
    - Security and system logs

- **Windows 10 Workstation**
  - 🔹 Domain-joined client machine
  - 🔹 Simulates user behavior: logins, browsing, file access
  - 🔹 Used to test internal visibility and endpoint logging

---

## 🧪 Tools & Learning Focus

- **Nmap** – Network scanning and mapping
- **Hydra** – Brute-force attacks (SSH, RDP, web login)
- **Metasploit** – Exploit delivery and post-exploitation
- **Splunk** – Log analysis and detection rule development
- **pfSense** – Network control, segmentation, and traffic visibility
- **Windows Event Logs** – Correlating system behavior with real threats

---

## 💡 Why This Lab Exists

This is a practical, self-built lab designed to:

- Build intuition for how systems behave under real-world pressure
- See how common tools and attacks look in the logs
- Practice both attacker mindset and defensive architecture
- Go beyond certification — actually engineer, test, and learn

> Not designed for show — built to sharpen real-world awareness and skill.
