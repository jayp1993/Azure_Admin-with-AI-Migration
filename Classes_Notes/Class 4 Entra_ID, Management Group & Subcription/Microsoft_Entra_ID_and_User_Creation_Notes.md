# Class Notes: Microsoft Entra ID & User Creation

## Beginner Friendly Notes (Simple Language)

------------------------------------------------------------------------

# Microsoft Entra ID

## What is Microsoft Entra ID?

Microsoft Entra ID (formerly **Azure Active Directory (Azure AD)**) is
Microsoft's **Identity and Access Management (IAM)** service.

It is used to:

-   Store Users
-   Store Groups
-   Manage Authentication
-   Manage Authorization
-   Control Access to Azure Resources
-   Enable Single Sign-On (SSO)
-   Apply Security Policies

> **Old Name:** Azure Active Directory (Azure AD)\
> **New Name:** Microsoft Entra ID

------------------------------------------------------------------------

# Real-Life Example

Imagine a company:

``` text
Company
│
├── HR
├── IT
├── Finance
└── Sales
```

Every employee has:

-   Username
-   Password
-   Employee ID

When an employee signs in:

1.  Microsoft Entra ID verifies the identity.
2.  Checks the password or MFA.
3.  Confirms permissions.
4.  Grants access if everything is valid.

------------------------------------------------------------------------

# Identity vs Authentication vs Authorization

## Identity

**Who are you?**

Example:

-   Name: Rahul Sharma
-   Email: rahul@company.com

------------------------------------------------------------------------

## Authentication

**How do you prove your identity?**

Examples:

-   Username & Password
-   OTP
-   Microsoft Authenticator
-   Fingerprint

------------------------------------------------------------------------

## Authorization

**What are you allowed to access?**

Example:

-   ✅ Read Storage Account
-   ❌ Delete Virtual Machines

------------------------------------------------------------------------

# Authentication Flow

``` text
User
   │
Username + Password
   │
Microsoft Entra ID
   │
Authentication
   │
Authorization
   │
Access Granted
```

------------------------------------------------------------------------

# Types of Identities

## 1. User Identity

A real person.

Example:

`rahul@company.onmicrosoft.com`

## 2. Service Principal

Used by applications.

Examples:

-   Azure DevOps
-   Terraform
-   GitHub Actions

## 3. Managed Identity

Used by Azure resources.

Examples:

-   Virtual Machine
-   App Service
-   Function App

No password management is required.

------------------------------------------------------------------------

# Microsoft Entra ID Portal

Navigate to:

``` text
Azure Portal
    ↓
Microsoft Entra ID
```

Available sections:

-   Users
-   Groups
-   Roles & Administrators
-   Enterprise Applications
-   App Registrations
-   Devices
-   Licenses

------------------------------------------------------------------------

# Users

A **User** represents a person who can sign in to Azure.

Each user has:

-   Display Name
-   User Principal Name (UPN)
-   Password
-   Role
-   Email Address

------------------------------------------------------------------------

# User Types

## Member

Internal employee.

Example:

`rahul@company.com`

## Guest

External user.

Examples:

-   Vendor
-   Client
-   Consultant

Example login:

`abc@gmail.com`

------------------------------------------------------------------------

# Ways to Create Users

1.  Azure Portal
2.  Azure CLI
3.  PowerShell
4.  Microsoft Graph API

------------------------------------------------------------------------

# Create User Using Azure Portal

1.  Open **portal.azure.com**
2.  Go to **Microsoft Entra ID**
3.  Select **Users**
4.  Click **+ New User**
5.  Select **Create New User**
6.  Enter:
    -   User Principal Name
    -   Display Name
    -   Password
7.  Click **Review + Create**
8.  Click **Create**

------------------------------------------------------------------------

# Required User Information

  Field                 Example
  --------------------- -------------------------------
  User Principal Name   rahul@company.onmicrosoft.com
  Display Name          Rahul Sharma
  Password              Auto Generated
  Account Enabled       Yes

------------------------------------------------------------------------

# User Principal Name (UPN)

The UPN is the user's login ID.

Example:

`rahul@cloudtechhacks.onmicrosoft.com`

------------------------------------------------------------------------

# Account Status

## Enabled

The user can sign in.

## Disabled

The user cannot sign in.

------------------------------------------------------------------------

# Assign Azure Roles

Common roles:

-   Owner
-   Contributor
-   Reader
-   User Access Administrator

Example:

Rahul → Contributor

-   Can manage Azure resources.
-   Cannot assign permissions.

------------------------------------------------------------------------

# Reset User Password

Microsoft Entra ID → Users → Select User → **Reset Password**

------------------------------------------------------------------------

# Delete User

Microsoft Entra ID → Users → Select User → **Delete**

Deleted users remain in **Deleted Users** for a limited retention period
(typically 30 days).

------------------------------------------------------------------------

# Best Practices

-   Use strong passwords.
-   Enable Multi-Factor Authentication (MFA).
-   Follow the Principle of Least Privilege.
-   Assign only the required roles.
-   Review inactive users regularly.
-   Remove unused accounts.

------------------------------------------------------------------------

# Interview Questions

### 1. What is Microsoft Entra ID?

Microsoft Entra ID is Microsoft's cloud-based Identity and Access
Management (IAM) service used to manage identities and secure access to
Azure resources.

### 2. What was the old name of Microsoft Entra ID?

Azure Active Directory (Azure AD).

### 3. What is Authentication?

Authentication verifies the identity of a user.

### 4. What is Authorization?

Authorization determines what a user is allowed to access.

### 5. What is a User Principal Name (UPN)?

The UPN is the user's sign-in name, such as:

`rahul@company.onmicrosoft.com`

### 6. What are the two main user types?

-   Member
-   Guest

------------------------------------------------------------------------

# Summary

-   Microsoft Entra ID is an IAM service.
-   It manages users, groups, devices, and applications.
-   It performs Authentication and Authorization.
-   Users can be Members or Guests.
-   Users can be created through Portal, CLI, PowerShell, or Graph API.
-   The UPN is used for login.
-   MFA and least privilege improve security.
