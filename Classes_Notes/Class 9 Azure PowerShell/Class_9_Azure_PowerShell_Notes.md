# Azure PowerShell Notes

> **Course:** Azure Administrator with AI & Migration
> **Module:** Azure PowerShell
> **Level:** Beginner to Intermediate

---

# What is Azure PowerShell?

Azure PowerShell is a command-line scripting tool used to create, manage, automate, and monitor Azure resources.

It is built on top of **Windows PowerShell** and **PowerShell Core**.

Azure PowerShell uses **Az Module**, which contains hundreds of cmdlets for managing Azure resources.

Example:

```powershell
Get-AzVM
```

---

# Why Azure PowerShell?

- Automate repetitive tasks
- Manage Azure resources quickly
- Infrastructure Automation
- Easy scripting
- Bulk resource creation
- CI/CD Integration
- Azure Administration

---

# Azure PowerShell Architecture

```
PowerShell
      │
      ▼
Az Module
      │
      ▼
Azure Resource Manager (ARM)
      │
      ▼
Azure Resources
```

---

# Prerequisites

- Azure Subscription
- PowerShell 7+ (Recommended)
- Internet Connection
- Az Module

---

# Install Azure PowerShell

## Check PowerShell Version

```powershell
$PSVersionTable
```

---

## Install Az Module

```powershell
Install-Module Az -Scope CurrentUser
```

If prompted:

```
Y
```

---

## Update Az Module

```powershell
Update-Module Az
```

---

## Verify Installation

```powershell
Get-Module Az -ListAvailable
```

---

# Import Module

```powershell
Import-Module Az
```

---

# Login to Azure

Interactive Login

```powershell
Connect-AzAccount
```

---

# Login using Device Authentication

```powershell
Connect-AzAccount -UseDeviceAuthentication
```

---

# Logout

```powershell
Disconnect-AzAccount
```

---

# Check Current Account

```powershell
Get-AzContext
```

---

# List Azure Subscriptions

```powershell
Get-AzSubscription
```

---

# Select Subscription

```powershell
Set-AzContext -Subscription "Subscription Name"
```

or

```powershell
Set-AzContext -SubscriptionId "SubscriptionID"
```

---

# Azure PowerShell Naming Convention

Azure cmdlets follow:

```
Verb-AzNoun
```

Examples

```
Get-AzVM
New-AzVM
Remove-AzVM
Restart-AzVM
Start-AzVM
Stop-AzVM
```

---

# Common Verbs

| Verb | Purpose |
|-------|----------|
| Get | Read |
| New | Create |
| Remove | Delete |
| Set | Update |
| Start | Start |
| Stop | Stop |
| Restart | Restart |
| Add | Add Resource |
| Update | Modify |
| Export | Export Data |

---

# Help Commands

## Get Help

```powershell
Get-Help Get-AzVM
```

Detailed Help

```powershell
Get-Help Get-AzVM -Detailed
```

Examples

```powershell
Get-Help Get-AzVM -Examples
```

Online Help

```powershell
Get-Help Get-AzVM -Online
```

---

# Find Azure Commands

```powershell
Get-Command -Module Az
```

Search VM Commands

```powershell
Get-Command *VM*
```

---

# Resource Group Commands

## List Resource Groups

```powershell
Get-AzResourceGroup
```

---

## Create Resource Group

```powershell
New-AzResourceGroup `
-Name RG-Demo `
-Location "Central India"
```

---

## Delete Resource Group

```powershell
Remove-AzResourceGroup `
-Name RG-Demo
```

---

# Azure Regions

```powershell
Get-AzLocation
```

---

# Virtual Machine Commands

## List VMs

```powershell
Get-AzVM
```

---

## VM Status

```powershell
Get-AzVM -Status
```

---

## Start VM

```powershell
Start-AzVM `
-Name VM1 `
-ResourceGroupName RG-Demo
```

---

## Stop VM

```powershell
Stop-AzVM `
-Name VM1 `
-ResourceGroupName RG-Demo
```

---

## Restart VM

```powershell
Restart-AzVM `
-Name VM1 `
-ResourceGroupName RG-Demo
```

---

## Delete VM

```powershell
Remove-AzVM `
-Name VM1 `
-ResourceGroupName RG-Demo
```

---

# Storage Account Commands

List Storage Accounts

```powershell
Get-AzStorageAccount
```

Create Storage Account

```powershell
New-AzStorageAccount `
-ResourceGroupName RG-Demo `
-Name mystorage12345 `
-Location "Central India" `
-SkuName Standard_LRS
```

---

# Virtual Network Commands

List VNets

```powershell
Get-AzVirtualNetwork
```

---

# Network Security Group

List NSGs

```powershell
Get-AzNetworkSecurityGroup
```

---

# Public IP

List Public IPs

```powershell
Get-AzPublicIpAddress
```

---

# Resource Commands

List All Resources

```powershell
Get-AzResource
```

Delete Resource

```powershell
Remove-AzResource
```

---

# Tags

List Tags

```powershell
Get-AzTag
```

---

# Azure PowerShell Variables

```powershell
$name="AzureVM"
```

Print Variable

```powershell
$name
```

---

# Arrays

```powershell
$vm=("VM1","VM2","VM3")
```

Access

```powershell
$vm[0]
```

---

# Loops

ForEach

```powershell
foreach($item in $vm)
{
Write-Host $item
}
```

---

# If Statement

```powershell
if($a -eq 10)
{
Write-Host "Equal"
}
```

---

# Pipeline

```powershell
Get-AzVM | Select Name
```

Example

```powershell
Get-AzVM | Stop-AzVM
```

---

# Export Data

CSV

```powershell
Get-AzVM | Export-Csv vm.csv
```

---

# Formatting Output

Table

```powershell
Get-AzVM | Format-Table
```

List

```powershell
Get-AzVM | Format-List
```

---

# Measure Command

```powershell
Get-AzVM | Measure-Object
```

---

# Filtering

```powershell
Get-AzVM | Where-Object {$_.Location -eq "Central India"}
```

---

# Sorting

```powershell
Get-AzVM | Sort-Object Name
```

---

# Select Specific Properties

```powershell
Get-AzVM | Select Name,Location
```

---

# PowerShell Execution Policy

View Policy

```powershell
Get-ExecutionPolicy
```

Allow Script Execution

```powershell
Set-ExecutionPolicy RemoteSigned
```

---

# Common Az Modules

| Module | Purpose |
|---------|----------|
| Az.Accounts | Authentication |
| Az.Resources | Resource Groups |
| Az.Compute | Virtual Machines |
| Az.Network | Networking |
| Az.Storage | Storage |
| Az.KeyVault | Key Vault |
| Az.Sql | Azure SQL |
| Az.Websites | App Service |
| Az.Monitor | Monitoring |
| Az.RecoveryServices | Backup |

---

# Best Practices

- Use PowerShell 7+
- Keep Az module updated
- Use Resource Groups properly
- Follow naming conventions
- Use Variables
- Use Scripts instead of manual tasks
- Test scripts before production
- Use Least Privilege (RBAC)
- Never hardcode secrets
- Use Managed Identity or Key Vault for credentials

---

# Azure PowerShell vs Azure CLI

| Azure PowerShell | Azure CLI |
|------------------|-----------|
| PowerShell Based | Bash Style |
| Object Output | JSON Output |
| Windows Friendly | Cross Platform |
| Great for Automation | Great for Scripting |
| Uses Az Module | Uses az Commands |

Example:

PowerShell

```powershell
Get-AzVM
```

Azure CLI

```bash
az vm list
```

---

# Frequently Used Commands

```powershell
Connect-AzAccount

Get-AzContext

Get-AzSubscription

Set-AzContext

Get-AzResourceGroup

New-AzResourceGroup

Get-AzVM

Start-AzVM

Stop-AzVM

Restart-AzVM

Remove-AzVM

Get-AzStorageAccount

Get-AzVirtualNetwork

Get-AzLocation

Get-AzResource

Get-AzTag

Disconnect-AzAccount
```

---

# Interview Questions

### Q1. What is Azure PowerShell?

Azure PowerShell is a collection of PowerShell cmdlets used to manage Azure resources.

---

### Q2. What is Az Module?

Az Module is Microsoft's official PowerShell module for Azure management.

---

### Q3. Difference between Azure CLI and Azure PowerShell?

- Azure CLI returns JSON.
- Azure PowerShell returns PowerShell Objects.
- Azure CLI is Bash-friendly.
- Azure PowerShell is PowerShell-based.

---

### Q4. Which command logs into Azure?

```powershell
Connect-AzAccount
```

---

### Q5. Which command lists all VMs?

```powershell
Get-AzVM
```

---

### Q6. How do you switch subscriptions?

```powershell
Set-AzContext
```

---

### Q7. Which command creates a Resource Group?

```powershell
New-AzResourceGroup
```

---

### Q8. Which command shows current Azure login context?

```powershell
Get-AzContext
```

---

# Summary

Azure PowerShell provides a powerful and efficient way to manage Azure resources using scripts and automation. It is widely used by Azure Administrators, DevOps Engineers, and Cloud Engineers for day-to-day Azure management and infrastructure automation.