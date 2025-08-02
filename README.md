# 🛡️ Cybersecurity Homelab Deployment – Hands-On Security Engineering Lab

This project simulates a realistic enterprise network environment to develop skills in **network segmentation**, **log analysis**, and **security controls implementation**. Built on **Proxmox**, the lab includes multiple operating systems, offensive testing, and centralized log aggregation via Splunk.

---

## 🔧 Deployment Phases & Stack Overview

### Phase 1 – Network & Perimeter Security
📘 [Phase 1 – Network & Perimeter Setup Guide](./phase-1-network.md)
- **pfSense Firewall**
  - 🔹 Manages **VLAN segmentation** across multiple subnets
  - 🔹 Provides **DHCP** to each VLAN (no reliance on Windows)
  - 🔹 Acts as **internal DNS resolver**
  - 🔹 Enforces **firewall rules** to simulate restricted zones
  - 🔹 Logs firewall traffic and events to **Splunk via Syslog**

- **Kali Linux**
  - 🔹 Offensive testing tools: `Nmap`, `Hydra`, `Metasploit`, others
  - 🔹 Simulates attacker behavior to test detection across the environment
  - 🔹 Used to generate realistic log events and attack traffic

---

### Phase 2 – Vulnerable Targets
📘 [Phase 2 – Vulnerable Targets Setup Guide](./phase-2-vulnerable-targets.md)
- **Ubuntu Server (Docker Host)**
  - 🔹 Runs intentionally vulnerable web apps:
    - `WebGoat`, `DVWA`, `bWAPP`
  - 🔹 Containerized to practice Docker networking and isolation
  - 🔹 Used to simulate low-hanging fruit commonly found during assessments

- **Metasploitable 2**
  - 🔹 Classic vulnerable Linux VM
  - 🔹 Used to test exploit delivery, lateral movement, and enumeration

---

### Phase 3 – Log Aggregation & Visibility
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

> Built to sharpen real-world awareness and skill.

```mermaid
graph TD
  Internet --> pfSense
  pfSense --> VLAN1[VLAN 1: 10.10.1.0/24]
  pfSense --> VLAN10[VLAN 10: 10.10.10.0/24]
  pfSense --> VLAN20[VLAN 20: 10.10.20.0/24]
  pfSense --> VLAN30[VLAN 30: 10.10.30.0/24]

  VLAN1 --> Kali(Kali: .50)
  VLAN1 --> Splunk(Splunk: .51)
  VLAN1 --> Nessus(Nessus: .53)

  VLAN10 --> Metasploit(Metasploit: .50)

  VLAN30 --> Ubuntu(Host: .50)
  Ubuntu --> DVWA
  Ubuntu --> bWAPP
  Ubuntu --> WebGoat

