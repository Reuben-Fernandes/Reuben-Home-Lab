# Phase 3 – Log Aggregation & Visibility

### 🎯 Goal
Centralize logs from critical systems (pfSense, Ubuntu) into **Splunk Free Tier** for threat detection, misconfiguration analysis, and visibility across your lab.

---

### 🧱 Components

- **Splunk Enterprise (Free Tier)**
  - Runs on Ubuntu Server: `10.10.1.51`
  - Installed using `.deb` package from Splunk site
  - Listens on:
    - TCP `9997` for Universal Forwarders
    - UDP `514` for pfSense syslog
  - Use cases:
    - Port scanning detection
    - Failed login attempts
    - Privilege escalation
    - Suspicious PowerShell usage
  - Custom dashboards and correlation rules

- **Nessus Essentials**
  - Internal vulnerability scanning
  - Tests misconfigurations across lab hosts
  - Validates detection capabilities
  - Exported results viewable in Splunk (manually or via log monitoring)

---

### ⚙️ Setup Steps

---

#### 🔹 Step 1: Install Splunk Enterprise on Ubuntu

1. **Deploy Ubuntu Server**
   - VLAN 1
   - Static IP: `10.10.1.51`

2. **Download & Install `.deb`**
   - From [splunk.com](https://www.splunk.com/en_us/download.html) after login
   - wget -O splunk-10.0.0-e8eb0c4654f8-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/10.0.0/linux/splunk-10.0.0-e8eb0c4654f8-linux-amd64.deb"
   - Install:
     ```bash
     sudo dpkg -i splunk-<version>-linux-2.6-amd64.deb
     sudo /opt/splunk/bin/splunk start --accept-license
     sudo /opt/splunk/bin/splunk enable boot-start
     ```

3. **Configure Splunk Web**
   - Access: `https://10.10.1.51:8000`
   - Set admin credentials
   - Add Receiving Inputs:
     - TCP `9997`
     - UDP `514` (for pfSense syslog)

---

#### 🔹 Step 2: Configure pfSense Syslog Forwarding

1. **Navigate to:**
   `Status → System Logs → Settings`

2. **Set Remote Syslog Server:**
   ```
   10.10.1.51
   Port: 514
   Transport: UDP
   ```

3. **Enable log forwarding for:**
   - Firewall events
   - DHCP logs
   - System-level logs

4. **On Splunk Web**, verify logs appear with source IP `10.10.1.254`.

---

#### 🔹 Step 3: Install Splunk Universal Forwarder (Ubuntu - Docker Host)

1. **Download `.tar.gz` from Splunk**
   - Use wget or curl from [Splunk Downloads](https://www.splunk.com/en_us/download/universal-forwarder.html)
   - Example:
     ```bash
     wget https://download.splunk.com/products/universalforwarder/releases/<ver>/linux/splunkforwarder-<ver>-Linux-x86_64.tgz
     ```

2. **Install & Start Forwarder**
   ```bash
   tar -xvzf splunkforwarder-<ver>.tgz
   mv splunkforwarder /root/splunkforwarder
   cd /root/splunkforwarder
   ./bin/splunk start --accept-license
   ./bin/splunk enable boot-start
   ```

3. **Configure Forwarder to Send Logs**
   ```bash
   ./bin/splunk add forward-server 10.10.1.51:9997
   ./bin/splunk add monitor /var/log
   ./bin/splunk restart
   ```

4. **Verify Ingestion**
   - Search: `index=* host=10.10.30.50`

---

#### 🔹 Step 4: Install Nessus Essentials

1. **Download via `curl`**
   ```bash
   curl --request GET \
     --url https://www.tenable.com/downloads/api/v2/pages/nessus/files/Nessus-10.7.2-ubuntu1404_amd64.deb \
     --output Nessus.deb
   ```

2. **Install Nessus**
   ```bash
   sudo dpkg -i Nessus.deb
   sudo systemctl start nessusd.service
   ```

3. **Initial Setup**
   - Open browser: `https://<host-ip>:8834`
   - Choose: **Nessus Essentials**
   - Register and activate
   - Wait for plugins to update
   - Create admin login

4. **Run Initial Scan on Metasploitable**
   - Target: `10.10.10.50`
   - Confirm vulnerabilities are detected
   - Export report as `.nessus` or monitor scan results with Splunk (optional)

---

> ✅ With this phase complete, core systems are feeding log data into Splunk. The lab is now ready for detection tuning, correlation, and red/blue scenario testing.
