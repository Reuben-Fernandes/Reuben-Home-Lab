### Phase 4 – Windows Server Deployment

#### 🔥 Goal
Deploy a Windows Server as Domain Controller to manage **DHCP**, **DNS**, and user authentication across the lab.

---

#### 🧱 Components

- **Windows Server 2022 Eval**
  - Connected to VLAN 20 (`10.10.20.0/24`)
  - Static IP: `10.10.20.10`
  - Renamed host to `WIN-DC01`
  - Installed roles:
    - **DHCP Server**
    - **DNS Server**
    - **Active Directory Domain Services (ADDS)**
  - Promoted to Domain Controller:
    - Forest: `reuben.local`
    - Functional level: Server 2016

- **DHCP (VLAN 20)**
  - Scope Name: `VLAN 20`
  - Range: `10.10.20.100 – 10.10.20.120`
  - Gateway: `10.10.20.254`
  - Replaced pfSense DHCP on this VLAN

- **AD Users**
  - Manually created:
    - `Reuben Fernandes` (Standard)
    - `RF Admin` (Domain Admin)
  - Bulk created via PowerShell (7 test users)

---

#### ⚙️ Setup Steps

1. **Install Windows Server**
   - Download Eval ISO from Microsoft
   - Create VM in Proxmox:
     - 2 vCPUs, 4–6 GB RAM, 45 GB Disk
   - Use VirtIO ISO to load drivers

2. **Initial Configuration**
   - Set static IP: `10.10.20.10`
   - Rename computer: `WIN-DC01`
   - Disable pfSense DHCP on VLAN 20

3. **Install Server Roles**
   - Add via Server Manager:
     - DHCP
     - DNS
     - Active Directory Domain Services
   - Restart system

4. **Promote to Domain Controller**
   - Launch ADDS wizard
   - Create new forest: `reuben.local`
   - Functional level: Server 2016
   - Complete setup and restart

5. **Configure DHCP**
   - Create scope: `VLAN 20`
   - IP Range: `10.10.20.100 – 10.10.20.120`
   - Set router: `10.10.20.254`
   - Activate the scope

6. **Add AD Users**
   - Manually add `Reuben Fernandes` and `RF Admin`
   - Use the following PowerShell script to bulk-create lab users:
     ```powershell
     # Import Active Directory module
     Import-Module ActiveDirectory

     # User array (excluding Reuben Fernandes and RF Admin)
     $users = @(
         @{Name="Alice Doe"; Username="adoe"; Password="A1ice@Lab"; Role="Test User"},
         @{Name="Bob Smith"; Username="bsmith"; Password="B0b@Lab23"; Role="Test User"},
         @{Name="Daisy Johnson"; Username="djohnson"; Password="D@1syL@b"; Role="Test User"},
         @{Name="Evan Lee"; Username="elee"; Password="E!vanLab3"; Role="Test User"},
         @{Name="Fiona Clark"; Username="fclark"; Password="F1ona@Lab"; Role="Test User"},
         @{Name="George Hall"; Username="ghall"; Password="G£eorge55"; Role="Test User"},
         @{Name="Hannah Lewis"; Username="hlewis"; Password="H@nnah89"; Role="Test User"}
     )

     # Target container
     $OU = "CN=Users,DC=reuben,DC=local"
     $domain = "reuben.local"

     # Loop to create users
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

7. **Install Splunk Universal Forwarder**
   - Download MSI installer using:
     ```powershell
     wget -O splunkforwarder-10.0.0-e8eb0c4654f8-windows-x64.msi "https://download.splunk.com/products/universalforwarder/releases/10.0.0/windows/splunkforwarder-10.0.0-e8eb0c4654f8-windows-x64.msi"
     ```
   - Install and configure to forward logs to Splunk indexer (`10.10.1.51`, port `9997`)

---

> 🔍 Validate: Ensure DHCP leases from VLAN 20 are issued by the Windows Server. Confirm test users can authenticate in the domain `reuben.local`. Verify Splunk forwarder is phoning home successfully.
