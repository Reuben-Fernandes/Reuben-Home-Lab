### Phase 4 – Windows Server & Domain Infrastructure

#### 🔥 Goal  
Deploy Active Directory, DHCP, DNS, and integrate Windows systems into a working domain with log forwarding for identity monitoring.

---

#### 🧱 Components

- **Windows Server 2022**
  - Role: Domain Controller, DHCP, DNS
  - Hostname: `REUBEN-DC`
  - Static IP: `10.10.20.10`
  - Subnet: `VLAN 20` (`10.10.20.0/24`)

- **Windows 11 Pro**
  - Joined to domain `reuben.local`
  - IP via DHCP from Windows Server
  - Used for testing logon events and domain connectivity

- **Splunk Universal Forwarder (Windows)**
  - Installed on Domain Controller
  - Logs forwarded to Splunk Indexer (Ubuntu) on port `9997`
  - Custom Index: `win`
  - **Only Security logs enabled for now**

---

#### ⚙️ Setup Steps

1. **Install Windows Server 2022**
   - Used official Microsoft Eval ISO
   - Added Proxmox VirtIO driver ISO during setup
   - Specs: 45GB HDD, 2 vCPU, 4GB RAM

2. **Configure Windows Server**
   - Renamed host to `REUBEN-DC`
   - Set static IP: `10.10.20.10`
   - Installed roles:
     - **Active Directory Domain Services (AD DS)**
     - **DNS**
     - **DHCP Server**
   - Promoted to Domain Controller:
     - Forest/domain: `reuben.local`
     - Forest level: Windows Server 2016
   - Rebooted to complete setup

3. **Configure DHCP (Windows)**
   - Disabled pfSense DHCP on VLAN 20
   - Created DHCP Scope:
     - Name: `VLAN 20`
     - IP Range: `10.10.20.100 – 10.10.20.120`
     - Gateway: `10.10.20.254`
   - Scope activated via DHCP Manager

4. **Install and Join Windows 11 Pro**
   - Clean VM install with DHCP assigned IP
   - Joined to domain `reuben.local`
   - Validated domain logon with user accounts

5. **Create AD Users**
   - Manual:
     - `Reuben Fernandes` – Standard
     - `RF Admin` – Domain Admin
   - Scripted (via PowerShell):
     ```powershell
     Import-Module ActiveDirectory

     $users = @(
         @{Name="Alice Doe"; Username="adoe"; Password="A1ice@Lab"},
         @{Name="Bob Smith"; Username="bsmith"; Password="B0b@Lab23"},
         @{Name="Daisy Johnson"; Username="djohnson"; Password="D@1syL@b"},
         @{Name="Evan Lee"; Username="elee"; Password="E!vanLab3"},
         @{Name="Fiona Clark"; Username="fclark"; Password="F1ona@Lab"},
         @{Name="George Hall"; Username="ghall"; Password="G£eorge55"},
         @{Name="Hannah Lewis"; Username="hlewis"; Password="H@nnah89"}
     )

     $OU = "CN=Users,DC=reuben,DC=local"
     $domain = "reuben.local"

     foreach ($user in $users) {
         $securePass = ConvertTo-SecureString $user.Password -AsPlainText -Force

         New-ADUser `
             -Name $user.Name `
             -GivenName ($user.Name.Split(' ')[0]) `
             -Surname ($user.Name.Split(' ')[-1]) `
             -SamAccountName $user.Username `
             -UserPrincipalName "$($user.Username)@$domain" `
             -AccountPassword $securePass `
             -Path $OU `
             -Enabled $true `
             -ChangePasswordAtLogon $false `
             -PasswordNeverExpires $true

         Write-Host "✅ Created user:" $user.Username
     }
     ```

6. **Install Splunk Universal Forwarder**
   - Downloaded the `.msi` installer from [Splunk’s official downloads page](https://www.splunk.com/en_us/download/universal-forwarder.html)  
   - _Direct link removed to not unintentionally go against Splunk TOS_
   - Installed using GUI (Next > Next > etc.)
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

