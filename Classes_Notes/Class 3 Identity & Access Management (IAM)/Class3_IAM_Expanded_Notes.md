# Class 3: Identity & Access Management (IAM)

> **Azure Administrator with AI & Migration -- Class 3 Notes**

## Learning Objectives

After completing this class you will understand: - Identity & Access
Management (IAM) - Microsoft Entra ID (Azure AD) - Authentication vs
Authorization - Token-based Authentication (JWT) - Azure Portal Sign-in
Flow - Manual vs Infrastructure Automation - Azure Automation Tools

------------------------------------------------------------------------

# Azure Administrator Roadmap

1.  Billing & Management Groups
2.  Identity & Access Management (IAM)
3.  Project Onboarding
4.  Networking (VNet & Subnet)
5.  Compute (VM, VMSS, AKS, App Service)
6.  Storage (Blob, Files, Queue, Table)
7.  Security (Key Vault, NSG, Firewall)
8.  Monitoring (Azure Monitor)
9.  Backup & Disaster Recovery
10. Cost Optimization
11. Governance (Azure Policy)

------------------------------------------------------------------------

# What is IAM?

Identity and Access Management (IAM) is the security system responsible
for:

-   User Identity
-   Authentication
-   Authorization
-   Access Control
-   Resource Protection
-   Audit & Compliance

**Goal:** Ensure the right user gets the right access to the right
resource at the right time.

------------------------------------------------------------------------

# Real-world Example

## Bank Example

-   Customers access Internet Banking.
-   Employees have different permissions.
-   Admin manages all identities.

The Domain Controller / Microsoft Entra ID stores: - Users - Groups -
Passwords - Policies - Permissions

------------------------------------------------------------------------

# Authentication vs Authorization

## Authentication

**Question:** Who are you?

Examples: - Username & Password - OTP - MFA - Face ID - Fingerprint

## Authorization

**Question:** What can you access?

Examples: - Read - Write - Delete - Virtual Machine Contributor -
Storage Blob Reader

------------------------------------------------------------------------

# Token-Based Authentication

After successful login:

1.  User enters credentials.
2.  Entra ID validates identity.
3.  JWT Access Token is generated.
4.  Token is sent to Azure services.
5.  Azure verifies the token.
6.  Resource access is granted based on RBAC.

Benefits: - Password isn't sent repeatedly. - Faster authentication. -
More secure. - Supports Single Sign-On (SSO).

------------------------------------------------------------------------

# Microsoft Entra ID

Microsoft Entra ID provides:

-   Identity Management
-   Authentication
-   Authorization
-   Single Sign-On
-   Multi-Factor Authentication
-   Conditional Access
-   Application Registration
-   RBAC

------------------------------------------------------------------------

# Azure Login Flow

User ↓ portal.azure.com ↓ login.microsoftonline.com ↓ Microsoft Entra ID
↓ JWT Token ↓ Azure Resource Manager ↓ Azure Resources

------------------------------------------------------------------------

# Azure Portal

Portal: https://portal.azure.com

Login Service: https://login.microsoftonline.com

------------------------------------------------------------------------

# Authentication Flow

User → Entra ID → JWT Token → Azure Resource → Access Granted

------------------------------------------------------------------------

# Azure Resource Protection

Think of Azure like a secured building:

-   Gate = Authentication
-   Security Guard = Authorization
-   Rooms = Azure Resources
-   Token = Entry Pass

Without a valid token, access is denied.

------------------------------------------------------------------------

# Azure Subscription

To create Azure resources:

-   Microsoft Account
-   Credit/Debit Card
-   Visa/MasterCard
-   International transactions enabled

Free Trial: - Around 30 days (subject to Microsoft offers) - Limited
free credits

------------------------------------------------------------------------

# Infrastructure Deployment Methods

## Manual

Portal - Easy - Slow - Error-prone

## Imperative

-   Azure CLI
-   Azure PowerShell

You specify HOW infrastructure should be created.

## Declarative

-   ARM Templates
-   Bicep
-   Terraform

You specify WHAT infrastructure should exist.

Recommended for enterprise environments.

------------------------------------------------------------------------

# Infrastructure as Code (IaC)

Benefits:

-   Repeatable deployments
-   Version control
-   Faster provisioning
-   Standardization
-   Disaster Recovery
-   CI/CD Integration

------------------------------------------------------------------------

# Automation Tools

Microsoft: - ARM Templates - Bicep - Azure CLI - Azure PowerShell

Open Source: - Terraform

------------------------------------------------------------------------

# Interview Questions

1.  What is IAM?
2.  Difference between Authentication and Authorization?
3.  What is Microsoft Entra ID?
4.  What is JWT Token?
5.  Explain Azure Login Flow.
6.  Manual vs Imperative vs Declarative deployment?
7.  ARM vs Bicep vs Terraform?
8.  What is Infrastructure as Code?

------------------------------------------------------------------------

# Key Takeaways

-   IAM secures Azure identities.
-   Microsoft Entra ID is Azure's Identity Provider.
-   Authentication verifies identity.
-   Authorization determines permissions.
-   JWT tokens enable secure access.
-   Terraform is the most popular open-source IaC tool.
-   Enterprise environments prefer automation over manual deployment.
