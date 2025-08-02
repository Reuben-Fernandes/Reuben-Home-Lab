# Phase 2 – Vulnerable Targets: Ubuntu + Metasploitable

### 🎯 Goal
Deploy vulnerable machines to simulate real-world attack surfaces and observe attacker behavior from the perspective of host-level visibility.

---

### 🧱 Components

- **Ubuntu Server (Docker Host)**
  - IP: `10.10.30.50` (VLAN 30)
  - Hosts vulnerable web applications:
    - `DVWA` – PHP/MySQL app with classic OWASP flaws
    - `bWAPP` – Multi-vulnerability training app
    - `WebGoat` – Java-based OWASP app for insecure coding scenarios
  - Managed via **Portainer**
  - Docker logs not forwarded — focus is currently on system-level deployment

- **Metasploitable 2**
  - IP: `10.10.10.50` (VLAN 10)
  - Intentionally vulnerable Linux VM
  - Used for:
    - Enumeration
    - Exploitation (planned)
    - Lateral movement testing (future phase)

---

### ⚙️ Setup Steps

#### 🔹 Ubuntu Docker Host

1. **Deploy Ubuntu Server**
   - Connect NIC to VLAN 30 bridge in Proxmox
   - Get IP from pfSense DHCP or assign reserved IP: `10.10.30.50`

2. **Install Docker (Official Apt Repository)**
   - Followed official Docker docs:  
     https://docs.docker.com/engine/install/ubuntu/

   ```bash
   sudo apt-get update
   sudo apt-get install ca-certificates curl
   sudo install -m 0755 -d /etc/apt/keyrings
   sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
   sudo chmod a+r /etc/apt/keyrings/docker.asc

   echo \
     "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
     $(. /etc/os-release && echo \"${UBUNTU_CODENAME:-$VERSION_CODENAME}\") stable" | \
     sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

   sudo apt-get update
   sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

   sudo docker run hello-world
   ```

3. **Install Portainer**
   ```bash
   docker volume create portainer_data
   docker run -d -p 8000:8000 -p 9443:9443 \
     --name=portainer \
     --restart=always \
     -v /var/run/docker.sock:/var/run/docker.sock \
     -v portainer_data:/data \
     portainer/portainer-ce:lts
   ```

   Optional legacy port:
   ```bash
   -p 9000:9000
   ```

   Access Portainer at:
   ```
   https://10.10.30.50:9443
   ```

4. **Deploy Vulnerable Apps via Portainer GUI**
   - Images used:
     - `vulnerables/web-dvwa`
     - `raesene/bwapp`
     - `webgoat/webgoat`
   - Created a Docker network inside Portainer for containers to receive their own IPs within VLAN 30

---

#### 🔹 Metasploitable 2

1. **Proxmox Deployment Process**
   - Create VM in Proxmox (no ISO)
   - Create VM folder:
     ```bash
     mkdir /var/lib/vz/images/205
     ```
   - Download from SourceForge:
     ```bash
     wget https://netix.dl.sourceforge.net/project/metasploitable/Metasploitable2/metasploitable-linux-2.0.0.zip?viasf=1 -O metasploitable-linux-2.0.0.zip
     ```
   - Unzip:
     ```bash
     unzip metasploitable-linux-2.0.0.zip
     cd metasploitable*
     ```
   - Convert VMDK to QCOW2:
     ```bash
     qemu-img convert -f vmdk Metasploitable.vmdk -O qcow2 Metasploitable.qcow2
     mv Metasploitable.qcow2 /var/lib/vz/images/205/
     ```
   - Edit VM config:
     ```bash
     nano /etc/pve/qemu-server/205.conf
     ```
     Add:
     ```
     scsi0: local:205/Metasploitable.qcow2
     ```
   - Start VM and assign NIC to VLAN 10 bridge
   - Confirm IP from pfSense: `10.10.10.50`

---

> 🧠 This phase introduces real attacker behavior and real misconfigurations — but focuses on host-level visibility, not container deep dives (yet).
