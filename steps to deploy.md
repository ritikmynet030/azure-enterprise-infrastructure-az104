


## ✅ Step 1:— Create Resource Group

1. Go to Azure Portal
2. Search **Resource Groups**
3. Click **Create:**
   | Setting | Value |
   |------|------|
   | Name | rg-enterprise-prod |
   | Region | Central India |


---

## ✅ Step 2:— Identity & Governance (IAM)

### Create Users (Optional)

Azure Active Directory → Users → New User

Create:
- auditor-user
- ops-user

---

### Assign RBAC Roles

Resource Group → Access Control (IAM)

| Role | Assignment |
|-----|-----|
| Reader | auditor-user |
| Virtual Machine Contributor | ops-user |

---

### Apply Azure Policy

Go to Policy → Assign Policy

Assign:

- Allowed Locations → Central India
- Require Tag → Environment = Production

---

## ✅ Step 3:— Hub-Spoke Networking

### Create HUB VNet:
  | Setting | Value |
  |-----|-----|
  | Name | vnet-hub |
  | Address space | 10.0.0.0/16 |

### Subnets:
  | Setting | Value |
  |-----|-----|
  | GatewaySubnet | 10.0.0.0/24 |
  | AzureBastionSubnet | 10.0.1.0/24 |
  
---

### Create SPOKE VNet:
  | Setting | Value |
  |-----|-----|
  | Websubnet | 10.1.1.0/24 |
  | Appsubnet | 10.1.2.0/24 |
  
---

## Configure VNet Peering

Create bidirectional peering:
- vnet-hub ↔ vnet-prod
- Enable traffic forwarding

---

## Create Network Security Groups

### nsg-web

Allow:
- SSH 22
- HTTP 80

### nsg-app

Allow:
- RDP 3389
- VirtualNetwork traffic

Associate NSGs with respective subnets.

---

## ✅ Step 4:— Deploy Virtual Machines

### Linux Web Server VM:
  | Setting | Value |
  |------|------|
  | Name | vm-web-linux |
  | Image | Ubuntu 22.04 LTS |
  | Size | B1s |
  | Subnet | Websubnet |
  | Public IP | Enabled (temporary) |

Connect using SSH and run:
- sudo apt update
- sudo apt install apache2 -y

Test:
- http://<<VM-public-ip>>

### Windows Application VM:
  | Setting | Value |
  |------|------|
  | Name | vm-app-win |
  | Image | Windows Server 2022 |
  | Size | B1s |
  | Subnet | Appsubnet |
  | Public IP | Enabled (temporary) |

Connect using RDP
Install IIS:
- Install-WindowsFeature -name Web-Server -IncludeManagementTools

---

## ✅ Step 5:- Create Azure Load Balancer
  | Setting | Value |
  |-----|-----|
  | Name | azure-vm-lb |
  | Type | Public |
  | SKU | Basic |
  | Backendpool | vm-web-linux |
  | Health Probe | Port 80 |
  | Rule | http port 80 |

### Test using Load Balancer Public IP.

---

## ✅ Step 6:- Create Storage & Backup

### Create Storage Account:
  | Setting | Value |
  |-----|-----|
  | Name | stenterprisebackup |
  | Perfromance | Standard |
  | Redundancy | LRS |

### Create Containers:
- VM-Backups
- logs

### Lifecycle Management

Rule:
- Hot → Cool after 30 days
- Cool → Archive after 90 days

### Enable Soft Delete:
Storage Account → Data Protection → Enable Blob Soft Delete.

### Configure Backup
Create Recovery Services Vault:
- Name: rsv-enterprise

Backup → Azure VM → Select both VMs → Daily backup policy.

---

## ✅ Step 7:— Monitoring & Alerts

### Create Log Analytics Workspace
- Name: law-enterprise

### Enable VM Insights

VM → Insights → Enable monitoring.

### Configure Diagnostic Settings
Send logs from:
- Virtual Machines
- NSG
- Load Balancer

Destinations:
- Log Analytics Workspace

## Create Alerts rule:
Azure Monitor → Alerts → Create Rule

### Condition:
- CPU Percentage > 80%

### Action:
- Email Notification

---

## ✅ Step 7:— Hybrid Connectivity (Simulation)

### Point-to-Site VPN

Create VPN Gateway:
  | Setting | Value |
  |-----|-----|
  | GatewaySKU | VpnGw1 |
  | Tunnel Type | IKEv2 |
  | Address Pool | 172.16.0.0/24 |

Download VPN client and connect.

Test: ping VM private IP

---

## ✅ Step 9:— Security Hardening

Enable:
- Defender for Cloud (Free Tier)
- Just-In-Time VM Access
- Remove Public IPs
- Access via VPN/Bastion only

---

## ✅ Step 10:— Network Troubleshooting
Use Network Watcher:
- IP Flow Verify
- Next Hop
- Connection Troubleshoot
- Packet Capture

---

## ✅ FINAL VALIDATION CHECKLIST

- Website accessible via Load Balancer
- VM backup successful
- Alerts triggered
- VPN connectivity working
- Policy compliance verified
- Monitoring enabled
