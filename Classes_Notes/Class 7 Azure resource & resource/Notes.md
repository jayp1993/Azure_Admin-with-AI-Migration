# Class 7: Azure Resources & Resource Group Creation (Portal + CLI)

## Learning Objectives

After this class, you will understand:

- What is an Azure Resource?
- What is a Resource Group?
- Different methods to deploy Azure resources
- Manual vs Automation deployment
- Imperative vs Declarative approach
- ARM Template
- Bicep
- Terraform
- How to create Resource Groups using Azure Portal & Azure CLI
- How to select Azure Region
- Production Best Practices

---

# What is an Azure Resource?

Any service created inside Azure is called an Azure Resource.

Examples:

- Virtual Machine
- Storage Account
- Virtual Network
- Network Security Group
- SQL Database
- Azure Firewall
- Load Balancer
- Application Gateway
- Public IP
- Key Vault
- Recovery Services Vault

Think of Azure Resource as an object that Azure manages.

---

# What is a Resource Group?

A Resource Group (RG) is a logical container that stores Azure Resources.

It helps you:

- Organize resources
- Manage permissions
- Monitor billing
- Delete resources together
- Apply policies
- Apply Tags
- Manage Lifecycle

Example:

Production Resource Group

```
RG-Production

│
├── VM01
├── VM02
├── Storage
├── VNET
├── NSG
├── Public IP
└── Backup Vault
```

---

# Why Resource Group?

Without Resource Group

```
Azure Subscription

VM
Storage
NSG
Database
VNET

Everything mixed together
```

With Resource Group

```
Subscription

Production-RG
Development-RG
Testing-RG
DR-RG
```

Everything becomes organized.

---

# Azure Resources Deployment Methods

Azure Resources can be deployed in two ways.

```
Azure Resource Deployment

│
├── Manual
│
└── Automation
```

---

# Manual Deployment

Resources are created manually.

Examples

- Azure Portal
- Azure Mobile App

Normally used by:

- Beginners
- Azure Administrators
- Small Environment
- Testing

Advantages

- Easy
- GUI Based
- No Coding

Disadvantages

- Time Consuming
- Human Errors
- Difficult to Repeat

---

# Automation Deployment

Resources are created using scripts or templates.

Advantages

- Fast
- Repeatable
- Version Control
- Infrastructure as Code
- Suitable for DevOps

---

# Types of Automation

Automation has two approaches.

```
Automation

│
├── Imperative
│
└── Declarative
```

---

# Imperative Method

You tell Azure HOW to create resources.

Example

Step 1

Create VNET

Step 2

Create Subnet

Step 3

Create VM

Step 4

Attach Disk

You write every command.

Examples

- Azure CLI
- Azure PowerShell

---

# Azure CLI

Azure CLI is a command-line tool.

Commands Example

```bash
az login

az account show

az group create \
--name Dev-RG \
--location centralindia
```

Supported Platforms

- Windows
- Linux
- macOS
- Cloud Shell

---

# Azure PowerShell

PowerShell is Microsoft's automation tool.

Example

```powershell
Connect-AzAccount

New-AzResourceGroup `
-Name Dev-RG `
-Location "Central India"
```

Mostly used by Windows Administrators.

---

# Declarative Method

You tell Azure WHAT you want.

Azure decides HOW to create it.

Example

Instead of writing 100 commands

You simply define

```
Create

1 VNET

2 VM

1 Storage

1 NSG
```

Azure automatically deploys everything.

---

# Declarative Tools

Azure Native

- ARM Template
- Bicep

Open Source

- Terraform

---

# ARM Template

ARM

Azure Resource Manager Template

Format

JSON

Example

```json
{
  "resources": []
}
```

Advantages

- Native Azure
- Microsoft Supported
- Free

Disadvantages

- JSON is lengthy
- Difficult to read
- Hard to maintain

---

# Bicep

Bicep is Microsoft's modern language.

Bicep converts into ARM Template automatically.

Example

```bicep
resource vm 'Microsoft.Compute/virtualMachines@2023-03-01' = {
  name: 'vm01'
}
```

Advantages

- Easy
- Clean Syntax
- Azure Native

---

# Terraform

Terraform is developed by HashiCorp.

Language

HCL

(HashiCorp Configuration Language)

Example

```hcl
resource "azurerm_resource_group" "rg" {

name = "Dev-RG"

location = "Central India"

}
```

Advantages

- Infrastructure as Code
- Multi Cloud
- Easy to Read
- Reusable Modules
- State File Management
- Version Control

Supports

- Azure
- AWS
- GCP
- VMware
- Kubernetes
- Oracle Cloud
- Alibaba Cloud

---

# Imperative vs Declarative

| Imperative | Declarative |
|------------|-------------|
| Tell How | Tell What |
| Command Based | Template Based |
| Manual Steps | Desired State |
| CLI | Terraform |
| PowerShell | ARM |
| Sequential | Automatic |

---

# Manual vs Automation

| Manual | Automation |
|----------|-------------|
| Azure Portal | Terraform |
| Slow | Fast |
| Human Error | Consistent |
| GUI | Code |
| Small Infra | Enterprise |

---

# Production Recommendation

Development

Portal

Testing

Portal

Production

Terraform

Reason

- Repeatable
- Version Control
- Team Collaboration
- Easy Rollback

---

# Infrastructure as Code (IaC)

Infrastructure is created using code instead of clicking.

Example

Instead of

Portal

Click → Next → Next → Create

We write

Terraform Code

Git Repository

Pipeline

Deploy

Benefits

- Audit
- Automation
- Reusability
- Disaster Recovery
- CI/CD Integration

---

# State File in Terraform

Terraform stores infrastructure information inside

```
terraform.tfstate
```

Purpose

- Track Resources
- Detect Changes
- Prevent Duplicate Creation

Production

Store State File

- Azure Storage Account
- AWS S3
- Terraform Cloud

Never keep production state locally.

---

# Azure Region

Every Azure Resource must be deployed inside a Region.

Examples

- Central India
- South India
- East US
- West Europe
- Japan East

---

# How to Select Azure Region?

Consider:

### 1. Lowest Latency

Closer region = Better performance

Example

Meerut

Choose

Central India

instead of

West Europe

---

### 2. Compliance

Some companies require data inside India.

Choose

Central India

South India

---

### 3. Service Availability

Not every Azure service exists in every Region.

Always verify before deployment.

---

### 4. Disaster Recovery

Production

Central India

Backup

South India

---

### 5. Pricing

Some Azure Regions cost more than others.

Always estimate using Azure Pricing Calculator.

---

# Real-Time Production Scenario

Company

ABC Bank

Users

Delhi

Noida

Meerut

Agra

Best Region

Central India

Reason

- Lowest latency
- Better customer experience
- Lower network delay

---

# Azure Portal Demo

Create Resource Group

Portal

↓

Resource Groups

↓

Create

↓

Subscription

↓

Resource Group Name

↓

Region

↓

Review + Create

↓

Create

---

# Azure CLI Demo

Login

```bash
az login
```

Create Resource Group

```bash
az group create \
--name Production-RG \
--location centralindia
```

List Resource Groups

```bash
az group list --output table
```

Delete Resource Group

```bash
az group delete --name Production-RG
```

---

# Naming Convention

Recommended

```
RG-DEV-APP01

RG-PROD-WEB

RG-UAT-ERP

RG-NETWORK

RG-BACKUP
```

---

# Best Practices

- Keep Production and Development separate.
- Use meaningful Resource Group names.
- Use Tags (Owner, Project, Environment, CostCenter).
- Select the nearest Azure Region.
- Use Terraform for Production deployments.
- Store Terraform State remotely.
- Follow least privilege using RBAC.
- Avoid deleting Resource Groups without verification.

---

# Interview Questions

### Q1. What is an Azure Resource?

A manageable service in Azure such as a VM, Storage Account, or Virtual Network.

### Q2. What is a Resource Group?

A logical container used to organize and manage Azure resources.

### Q3. Difference between Resource Group and Subscription?

Subscription is the billing boundary, while a Resource Group is used to organize and manage related resources within a subscription.

### Q4. Manual vs Automation Deployment?

Manual uses the Azure Portal, while Automation uses code or templates like Azure CLI, ARM, Bicep, or Terraform.

### Q5. Imperative vs Declarative?

Imperative defines **how** to perform tasks step by step, whereas Declarative defines the **desired end state**, and Azure determines how to achieve it.

### Q6. Why is Terraform popular?

Because it supports Infrastructure as Code (IaC), is cloud-agnostic, uses reusable modules, and provides state management for consistent deployments.

### Q7. Which deployment method is preferred in production?

Terraform or Bicep (IaC), as they provide repeatability, version control, automation, and easier collaboration.

### Q8. Why do we use Tags?

Tags help categorize resources for cost management, ownership tracking, automation, and governance.

### Q9. Can resources in one Resource Group belong to different regions?

Yes. A Resource Group has a metadata location, but the resources inside it can be deployed across different Azure regions.

### Q10. What happens if you delete a Resource Group?

All resources within that Resource Group are permanently deleted. Always verify before deleting an RG.