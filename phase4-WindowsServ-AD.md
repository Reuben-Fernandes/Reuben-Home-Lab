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
   - Use this PowerShell script to add 7 more users:
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
     $OU = "OU=Users,DC=reuben,DC=local"
     foreach ($user in $users) {
       $pass = ConvertTo-SecureString $user.Password -AsPlainText -Force
       New-ADUser -Name $user.Name -SamAccountName $user.Username `
         -UserPrincipalName "$($user.Username)@reuben.local" `
         -Path $OU -AccountPassword $pass -Enabled $true `
         -PasswordNeverExpires $true -ChangePasswordAtLogon $false
     }
     ```

---

> 🔍 Validate: Ensure DHCP leases from VLAN 20 are issued by the Windows Server. Confirm test users can authenticate in the domain `reuben.local`.
