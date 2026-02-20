


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
