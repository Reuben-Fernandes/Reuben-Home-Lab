
# **Home Lab Project**

## **Purpose**
The goal of this home lab project is to create a versatile, self-contained network environment for **learning, testing, and experimentation** with cybersecurity, networking, and IT infrastructure concepts. The lab will serve as a sandbox for building technical skills, simulating real-world scenarios, and testing tools and configurations in a safe and controlled setting.

---

## **Key Objectives**
1. **Learn Networking Fundamentals**:
   - Build and configure isolated network environments.
   - Understand how firewalls, routers, NAT, and DNS function.
   - Work with segmented networks (e.g., VLANs) to isolate and control traffic.

2. **Enhance Cybersecurity Skills**:
   - Explore how to secure networks using tools like **pfSense**.
   - Simulate attacks and test defensive mechanisms (e.g., firewalls, intrusion detection systems).
   - Practice monitoring and analyzing network traffic.

3. **Develop Practical IT Skills**:
   - Gain experience with systems administration by deploying and managing multiple VMs.
   - Learn how to install and configure services like VPNs, web servers, and databases.
   - Experiment with virtualization platforms (e.g., VMware, VirtualBox).

4. **Test and Experiment with New Tools**:
   - Set up tools like SIEMs (e.g., Security Onion), IDS/IPS (e.g., Suricata, Snort), and monitoring systems (e.g., Zabbix).
   - Use the lab to evaluate open-source and commercial cybersecurity tools.

---

## **Lab Design Overview**

### **Virtualized Networking**
Using VMware Workstation:
- **VMnet8 (NAT)**: Provides WAN connectivity for pfSense.
- **VMnet1 (Host-only)**: LAN network managed by pfSense for internal traffic segmentation.

### **Core Components**
- **pfSense VM**: Acts as the router and firewall for the lab.
- **Client VMs**: Workstations or servers for testing configurations and connectivity.
- **Attack/Defense VMs**: For testing penetration testing tools (e.g., Kali Linux) and defensive strategies.

### **Expandability**
- The lab can scale by adding new VMs or integrating external devices.
- Future configurations may include VLANs, VPNs, and cloud-based components (e.g., AWS, Azure).

---

## **Initial Use Cases**
- Build a **basic network** with internet access for internal clients.
- Simulate real-world network security scenarios (e.g., malware containment, intrusion detection).
- Configure and test firewall rules, NAT policies, and DNS settings.
- Explore and troubleshoot networking issues in a hands-on environment.

---

## **Long-term Vision**
### **1. Mastering Cybersecurity Tools**
- Testing advanced configurations with IDS/IPS systems (e.g., Snort, Suricata).
- Implementing SIEMs for centralized monitoring and alerting.
- Learning forensic techniques for post-attack analysis.

### **2. Certifications and Career Readiness**
- Preparing for certifications like **CompTIA Security+**, **CEH**, or **CCNA**.
- Building a foundational understanding of IT infrastructure to support career growth.

### **3. Continuous Learning**
- Experimenting with emerging technologies (e.g., container security, zero trust models).
- Using the lab as a playground for testing new tools and methodologies.

---

## **Progress**

### **Current Status**
- **Networking Configuration**: ✅ Completed
- **Firewall/Router Setup (pfSense)**: ✅ Completed
- **LAN Client Setup**: ⚙️ In Progress
- **Internet Access and DNS Configuration**: ✅ Completed
- **Firewall Rule Tuning**: ⏳ Pending
- **Attack Simulations (e.g., Kali Linux)**: 🚀 Planned
- **SIEM/IDS Integration**: 🚀 Planned

---

## **Technical Environment**
### **Hypervisor**
- VMware Workstation

### **Virtual Networks**
- **WAN**: NAT network (VMnet8) - connects the lab to the internet.
- **LAN**: Host-only network (VMnet1) - managed by pfSense.

### **Core VMs**
1. **pfSense Firewall**:
   - Gateway for LAN, providing DHCP, DNS, and internet routing.
2. **Ubuntu Workstation**:
   - A client machine for testing LAN configurations and connectivity.
3. **Future VMs**:
   - Kali Linux for penetration testing.
   - Windows Server for Active Directory experiments.
   - Zabbix or ELK for monitoring and log analysis.

---

## **Next Steps**
1. Finalize internet access and DNS resolution for all VMs.
2. Add a penetration testing VM (e.g., Kali Linux) and explore network vulnerabilities.
3. Configure advanced features in pfSense:
   - Port forwarding for services.
   - Intrusion detection with Snort or Suricata.
4. Document key configurations for troubleshooting and reuse.

---

### **Repository Structure**
```
├── README.md                # Project Documentation
├── images/                  # Screenshots of configurations
└── configurations/          # Exported pfSense configuration backups



### Progress

- **Networking Configuration**: ✅ Completed
    - Virtual networks configured using VMware:
        - VMnet8 (NAT): Configured as WAN (`192.168.150.0/24`).
        - VMnet1 (Host-only): Configured as LAN (`192.168.170.0/24`).
    - pfSense installed and routing LAN-to-WAN traffic successfully.
    - DNS issues resolved using Google DNS (`8.8.8.8`, `8.8.4.4`).

- **Firewall/Router Setup (pfSense)**: ✅ Completed
    - Configured pfSense with WAN and LAN interfaces.
    - Enabled DHCP for the LAN with IP range `192.168.170.100 - 192.168.170.150`.
    - Verified LAN-to-WAN connectivity and internet access.

- **LAN Client Setup**: ✅ Completed
    - Ubuntu VM connected to pfSense LAN and received IP via DHCP.
    - Verified internet access from the Ubuntu VM and tested DNS resolution.
