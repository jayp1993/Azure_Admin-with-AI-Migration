# Azure Management Hierarchy

> Based on the uploaded Class 5 notes.

## Azure Management Hierarchy

Azure Management Hierarchy is a logical structure used to organize,
manage, and secure Azure resources.

``` text
Tenant
│
├── Tenant Root Management Group
│
├── Management Group
│
├── Subscription
│
├── Resource Group
│
└── Resources
```

------------------------------------------------------------------------

# 1. Tenant (Microsoft Entra ID)

A **Tenant** is a dedicated instance of Microsoft Entra ID (formerly
Azure Active Directory).

### Features

-   Unique Tenant ID
-   Identity Management
-   Users & Groups
-   Authentication
-   Authorization

------------------------------------------------------------------------

# 2. Tenant Root Management Group

-   Automatically created by Azure
-   Top-level management scope
-   Used to apply organization-wide:
    -   RBAC
    -   Azure Policy
    -   Governance

------------------------------------------------------------------------

# 3. Management Group

Management Groups organize multiple Azure subscriptions.

Example:

``` text
Tenant Root
│
├── HR MG
├── Sales MG
└── Finance MG
```

### Benefits

-   Centralized management
-   Policy inheritance
-   RBAC inheritance
-   Governance

------------------------------------------------------------------------

# 4. Subscription

A Subscription is a billing and management boundary.

### Purpose

-   Billing
-   Resource isolation
-   Access control
-   Resource quotas

Example:

``` text
HR Subscription
Sales Subscription
Finance Subscription
```

------------------------------------------------------------------------

# 5. Resource Group (RG)

A Resource Group is a logical container for Azure resources.

Example:

``` text
Production-RG
├── Virtual Machine
├── Virtual Network
├── Storage Account
└── SQL Database
```

------------------------------------------------------------------------

# 6. Azure Resources

Examples:

-   Virtual Machine (VM)
-   Virtual Network (VNet)
-   Storage Account
-   SQL Database
-   Azure Firewall
-   Load Balancer
-   App Service
-   Key Vault
-   AKS

------------------------------------------------------------------------

# Azure RBAC Roles

  Role           Create   Modify   Delete   Assign Access
  ------------- -------- -------- -------- ----------------
  Owner            ✅       ✅       ✅           ✅
  Contributor      ✅       ✅       ✅           ❌
  Reader           ❌       ❌       ❌     ❌ (View Only)

## Owner

-   Full access
-   Can assign permissions

## Contributor

-   Can manage resources
-   Cannot assign permissions

## Reader

-   View-only access

------------------------------------------------------------------------

# Microsoft Entra ID Roles

-   Global Administrator
-   User Administrator
-   Security Administrator
-   Billing Administrator

> Entra ID roles manage identities, while Azure RBAC roles manage Azure
> resources.

------------------------------------------------------------------------

# Complete Azure Hierarchy

``` text
Tenant
│
└── Tenant Root Management Group
    │
    ├── HR Management Group
    │   └── HR Subscription
    │       └── HR Resource Group
    │           ├── Virtual Machine
    │           ├── Virtual Network
    │           ├── Storage Account
    │           └── SQL Database
    │
    ├── Sales Management Group
    └── Finance Management Group
```

------------------------------------------------------------------------

# Why Azure Management Hierarchy?

-   Centralized administration
-   Easier access management
-   Policy enforcement
-   Governance
-   Cost management
-   Better organization

------------------------------------------------------------------------

# Interview Questions

### What is Azure Management Hierarchy?

Azure Management Hierarchy organizes Azure resources using:

**Tenant → Management Group → Subscription → Resource Group →
Resources**

### What is a Tenant?

A Tenant is a dedicated Microsoft Entra ID instance for an organization.

### What is a Management Group?

A container used to organize multiple subscriptions and apply RBAC and
Policies.

### What is a Subscription?

A billing and resource management boundary.

### What is a Resource Group?

A logical container for Azure resources.

### Difference between Owner and Contributor?

-   **Owner:** Full control + assign access.
-   **Contributor:** Full resource management but cannot assign access.
