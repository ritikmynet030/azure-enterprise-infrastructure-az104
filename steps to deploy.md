


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
- http://<VM-public-ip>
