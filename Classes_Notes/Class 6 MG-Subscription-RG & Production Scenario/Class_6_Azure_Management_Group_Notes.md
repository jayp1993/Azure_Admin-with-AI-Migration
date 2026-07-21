# Class 6 -- Azure Management Group, Subscription, Resource Group & Microsoft Entra ID

> Based on the uploaded class diagram and expanded with real-time
> production concepts.

------------------------------------------------------------------------

# Azure Hierarchy

``` text
Microsoft Entra Tenant
│
└── Tenant Root Management Group
    ├── HR Management Group
    │   └── HR Subscription
    │       └── HR Resource Group
    │           └── Azure Resources
    ├── Account Management Group
    │   └── Account Subscription
    │       └── Account Resource Group
    └── Sales Management Group
        └── Sales Subscription
            └── Sales Resource Group
```

------------------------------------------------------------------------

# Real-Time Company Scenario

**XYZ Pvt Ltd** contains three departments:

  Department   Application
  ------------ --------------------
  HR           Salary Application
  Accounts     Tally
  Sales        Sales Portal

Each department receives its own Management Group, Subscription and
Resource Group for better governance, security and billing.

------------------------------------------------------------------------

# Microsoft Entra ID

Microsoft Entra ID is Azure's Identity and Access Management (IAM)
service.

It stores:

-   Users
-   Groups
-   Devices
-   Applications
-   Service Principals
-   Managed Identities

Responsibilities:

-   Authentication
-   Authorization
-   RBAC
-   Conditional Access
-   Identity Management

------------------------------------------------------------------------

# Tenant

A Tenant represents an organization inside Azure.

Example:

``` text
XYZ Pvt Ltd Tenant
```

A tenant can contain:

-   Multiple Users
-   Multiple Management Groups
-   Multiple Subscriptions

------------------------------------------------------------------------

# Tenant Root Management Group

The Root Management Group is automatically created for every Azure
tenant.

Features:

-   Highest level of Azure hierarchy
-   Cannot be deleted
-   Cannot be moved
-   Used for centralized governance
-   Policies and RBAC inherit to child resources

------------------------------------------------------------------------

# Management Group

Management Groups organize multiple subscriptions.

Benefits:

-   Centralized Policy Management
-   Centralized RBAC
-   Governance
-   Compliance
-   Department-wise Organization

Example:

``` text
Tenant Root MG
├── HR-MG
├── Account-MG
└── Sales-MG
```

------------------------------------------------------------------------

# Subscription

A Subscription is the billing and quota boundary.

Responsibilities:

-   Billing
-   Cost Management
-   Resource Limits
-   Access Control

Benefits of Multiple Subscriptions:

-   Separate invoices
-   Department-wise budgeting
-   Better security isolation
-   Easier cost tracking

------------------------------------------------------------------------

# Resource Group

A Resource Group is a logical container for Azure resources.

Example:

``` text
Sales-RG
├── Virtual Machine
├── Storage Account
├── Virtual Network
├── NSG
├── Disk
└── Public IP
```

Important Points:

-   One resource belongs to only one Resource Group.
-   One Resource Group can contain many resources.
-   Deleting a Resource Group deletes all contained resources.

------------------------------------------------------------------------

# RBAC (Role-Based Access Control)

RBAC defines **Who can perform What action on Which scope**.

Scopes:

1.  Management Group
2.  Subscription
3.  Resource Group
4.  Resource

Example:

Cloud Engineer → Contributor → Sales Subscription

The engineer can manage only Sales resources.

------------------------------------------------------------------------

# Azure Policy

Azure Policy defines **what resources are allowed**.

Example Policies:

-   Allow VM creation only in Central India and Central US
-   Allow only approved VM sizes
-   Require mandatory tags
-   Restrict resource types

------------------------------------------------------------------------

# RBAC vs Azure Policy

  RBAC                      Azure Policy
  ------------------------- -------------------------------
  Controls who can access   Controls what can be deployed
  Security                  Governance
  Permissions               Compliance

------------------------------------------------------------------------

# Permission Inheritance

``` text
Tenant Root MG
    ↓
Management Group
    ↓
Subscription
    ↓
Resource Group
    ↓
Resources
```

Permissions and policies applied at higher levels automatically flow to
lower levels.

------------------------------------------------------------------------

# Production Best Practices

-   Use Management Groups for departments.
-   Separate Production and Non-Production subscriptions.
-   Use Azure Policy for governance.
-   Use RBAC with least privilege.
-   Apply mandatory resource tags.
-   Separate billing by subscription.

------------------------------------------------------------------------

# Interview Questions

### What is Azure hierarchy?

Tenant → Management Group → Subscription → Resource Group → Resources

### Why use Management Groups?

To centrally manage multiple subscriptions using RBAC and Azure Policy.

### Why create multiple subscriptions?

-   Separate billing
-   Security isolation
-   Budget management
-   Department-wise administration

### Difference between Subscription and Resource Group?

-   Subscription = Billing boundary
-   Resource Group = Logical container

### Difference between RBAC and Azure Policy?

-   RBAC controls **who** can perform actions.
-   Azure Policy controls **what** resources are allowed.

### What happens if a Resource Group is deleted?

All resources inside that Resource Group are permanently deleted.

------------------------------------------------------------------------

# Summary

Azure hierarchy follows:

``` text
Tenant
   ↓
Management Group
   ↓
Subscription
   ↓
Resource Group
   ↓
Resources
```

This hierarchy provides centralized governance, security, compliance,
billing and access management for enterprise Azure environments.
