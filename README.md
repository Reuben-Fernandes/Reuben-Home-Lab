# 🧪 Cybersecurity Homelab – Proxmox Edition

A hands-on cybersecurity lab designed to simulate real-world networks, attacks, and defenses — built from scratch using Proxmox, Docker, Kali, PFsense, and more.

## 🛰️ Lab Overview

This homelab replicates a small enterprise network with vulnerable machines, segmentation via VLANs, IDS/IPS inspection, and SIEM logging. The aim is to sharpen blue team skills, test red team tactics, and build practical experience in secure network design and incident response.

## 🛠️ Core Infrastructure

| Component        | Description |
|------------------|-------------|
| **Proxmox VE**   | Virtualization platform managing all VMs and networks |
| **Ubuntu Server**| Docker host running Webgoat, DVWA, and BWAPP via Portainer |
| **Docker & Portainer** | Containerized vulnerable apps for web exploitation training |
| **PFsense**      | Firewall/router providing VLAN separation and traffic inspection |
| **Kali Linux**   | Offensive testing machine for enumeration, scanning, and exploitation |
| **Metasploitable 2** | Intentionally vulnerable target machine for exploit validation |

### 🔜 Upcoming Additions
- **Nessus Essentials** – For internal vulnerability scanning and asset enumeration  
- **Windows Server & Client VMs** – To simulate AD environments, workstation traffic, and lateral movement

## 🔐 Features

- ✅ VLAN-based network segmentation
- ✅ Web app exploitation targets via Docker
- ✅ Live firewall traffic analysis via PFsense
- ✅ Offensive tooling via Kali (Metasploit, Nmap, Burp)
- ✅ Infrastructure built to support future SIEM/IDS integrations (Splunk, Suricata, Wazuh)

## 📸 Sneak Peek (Add Screenshots Here)
- Proxmox topology
- Portainer containers
- PFsense dashboard
- Kali in action

## 💡 Learning Objectives

- 🧠 Understand how attacks behave on a segmented network  
- 🕵️‍♂️ Practice scanning, enumeration, and exploitation  
- 🛡️ Build and test defensive configurations (IDS, logging, firewall rules)  
- 📊 Lay the foundation for full SIEM integration (e.g. Splunk or ELK)

## 🔄 Setup (Coming Soon)

Detailed steps for deploying this lab — including network diagrams, setup scripts, and config files — will be added soon.

---

> Built by someone who's not just studying cybersecurity — but living it.  
> _"If it breaks, I learn. If it works, I log it."_  
