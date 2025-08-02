### Phase 1 – Network & Perimeter Security

#### 🔥 Goal
Establish core network infrastructure with segmented VLANs, firewall rules, and a foundation for monitoring and threat simulation.

---

#### 🧱 Components

- **pfSense Firewall**
  - Gateway for all VLANs
  - Manages:
    - **VLAN segmentation**
        - VLAN 1: `10.10.1.0/24` (Monitoring/Infra)
        - VLAN 10: `10.10.10.0/24` (Target zone)
        - VLAN 20: `10.10.20.0/24` (Reserved/Windows)
        - VLAN 30: `10.10.30.0/24` (Docker apps)
    - **DHCP ranges** for each subnet
    - **DNS Resolver** across VLANs
    - **Firewall policies** – initially set to "Allow All" for lab setup
    - **Syslog forwarding** to Splunk (UDP 514)

- **Kali Linux**
  - IP: `10.10.1.50` (via DHCP)
  - Tools installed: `nmap`, `hydra`, `metasploit-framework`
  - Used for:
    - Recon and scanning
    - Password attacks
    - Exploitation tests
  - Logs and activity observed in Splunk

---

#### ⚙️ Setup Steps

1. **Install pfSense**
   - Deploy in Proxmox with 2 NICs (WAN + LAN)
   - Set static IP on LAN: `10.10.1.254`

2. **Access pfSense Web UI**
   - Browser: `https://10.10.1.254`
   - Login and complete initial setup wizard

3. **Enable DHCP for VLAN 1**
   - Range: `10.10.1.50 – 10.10.1.100`

4. **Install Kali Linux**
   - Connect NIC to lab VLAN bridge (e.g. `vmbr1`)
   - Boot and verify DHCP lease (should be `10.10.1.50`)

5. **Configure VLANs in pfSense**
   - Add VLAN 10, 20, 30 on same physical interface
   - Assign interfaces: `VLAN10`, `VLAN20`, `VLAN30`
   - Set gateways: `.254` for each

6. **Enable DHCP for all VLANs**
   - Use `.50 – .100` ranges in each

7. **Set Firewall Rules**
   - For now, allow all traffic within and between VLANs
   - (To tighten later once everything is logging)

---

> 🔍 Validate: Ensure Kali can ping other VLANs, and that Splunk will see syslog traffic from pfSense.
