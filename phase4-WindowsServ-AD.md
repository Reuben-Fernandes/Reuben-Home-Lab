### Phase 4 – Windows Server Deployment

#### 🔥 Goal  
Deploy a Windows Server as Domain Controller to manage DHCP, DNS, and user authentication across the lab.

---

#### 🧱 Components

- **Windows Server 2022**
  - Role: Domain Controller
  - Services Installed:
    - Active Directory Domain Services
    - DHCP
    - DNS
  - Domain: `reuben.local`
  - VLAN: 20 (`10.10.20.0/24`)
  - DHCP Range: `10.10.20.100 – 10.10.20.120`
  - Gateway: `10.10.20.254` (pfSense)
  - Created Users:
    - `Reuben Fernandes` – Standard
    - `RF Admin` – Domain Admin
    - 7 x Test Users via PowerShell script

- **Windows 11 Workstation**
  - VLAN: 20
  - NIC Driver: Installed via second Proxmox CD-ROM ISO
  - Joined domain: `reuben.local`
  - Logged in as: `ReubenFernandes`
  - Installed: Splunk Universal Forwarder
  - Logs Monitored:
    - **Application**
    - **Security**
    - **Setup**
    - **System**
    - **Forwarded Events**

---

#### ⚙️ Setup Steps

1. **Create VM**
   - Used Microsoft Eval ISO for Server 2022
   - 2 vCPUs, 4 GB RAM (6 GB recommended), 45 GB disk
   - Attached secondary ISO to install VirtIO drivers for disk/network

2. **Initial Server Setup**
   - Set static IP: `10.10.20.10`
   - Renamed machine: `WIN-DC`
   - Installed DHCP and DNS via Server Manager
   - Promoted to Domain Controller
   - Created new forest: `reuben.local`
   - Function level: Server 2016

3. **Configure DHCP**
   - Disabled pfSense DHCP for VLAN 20
   - Created Scope `vlan20`
     - Range: `10.10.20.100 – 10.10.20.120`
     - Gateway: `10.10.20.254`

4. **Create Test Users via PowerShell**
   - Used script to bulk-create 7 users in `CN=Users,DC=reuben,DC=local`
   - Format includes full name, UPN, secure password, and set to not expire

5. **Install Splunk Universal Forwarder on Server**
   - Downloaded `.msi` from [Splunk Downloads](https://www.splunk.com/en_us/download/universal-forwarder.html)  
   - _Direct link removed to not unintentionally go against Splunk TOS_
   - Configured during setup:
     - Indexer: `10.10.1.51` (Ubuntu Splunk)
     - Port: `9997`
     - Index: `win`
     - Monitored Logs:
       - **Application**
       - **Security**
       - **Setup**
       - **System**
       - **Forwarded Events**

6. **Add Windows 11 Workstation**
   - Placed on VLAN 20 to get DHCP from Windows Server
   - Installed network driver via second CD-ROM (VirtIO)
   - Joined domain: `reuben.local`
   - Logged in as domain user: `ReubenFernandes`
   - Installed Splunk Universal Forwarder with same config as Server

---

> ✅ Active Directory and DHCP now managed from Windows Server. Windows clients are logging critical event data to Splunk for real-world visibility.
