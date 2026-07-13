# Class 2 Notes

## Traditional IT → Virtualization → Cloud Computing

## 1. Enterprise Architect

An **Enterprise Architect** designs the complete IT strategy of an
organization.

**Responsibilities** - Overall IT planning - Application and
infrastructure strategy - Technology selection - Security, scalability,
and cost optimization

> **Enterprise Architect = Complete IT Business Planner**

------------------------------------------------------------------------

## 2. Application Architect

An **Application Architect** designs how an application is built.

### Common Technologies

**Frontend** - ReactJS - AngularJS

**Backend** - Java - Python - .NET - PHP - Go

### Responsibilities

-   Application design
-   API design
-   Business logic
-   Database selection

------------------------------------------------------------------------

## 3. Infrastructure Architect

Infrastructure is the **home of an application**.

Responsibilities: - Servers - Storage - Networking - Firewall - Backup -
Disaster Recovery - Monitoring

------------------------------------------------------------------------

# 3-Tier Architecture

``` text
User
   ↓
Presentation Layer (Frontend)
   ↓
Business Logic Layer (Backend)
   ↓
Database Layer
```

### Presentation Layer

-   User Interface
-   ReactJS
-   AngularJS

### Business Logic Layer

-   Java
-   Python
-   .NET
-   PHP
-   Go

Responsibilities: - Authentication - Authorization - Business rules -
API processing

### Database Layer

Stores: - Users - Orders - Products - Transactions

Examples: - MySQL - SQL Server - Oracle - PostgreSQL - MongoDB

------------------------------------------------------------------------

# 1-Tier Architecture

Everything runs on one computer.

Examples: - Calculator - Notepad - Paint

------------------------------------------------------------------------

# 2-Tier Architecture

``` text
Frontend
   ↓
Backend + Database
```

Restaurant Analogy:

Customer → Waiter → Kitchen → Chef → Store Room → Tea

------------------------------------------------------------------------

# Traditional IT Infrastructure

A company purchases and manages its own hardware.

Typical Components: - Physical Servers - Router - SAN Storage - Cooling
Devices - Power Devices - Network

Problems: - High Cost - Hardware Maintenance - Power Consumption -
Difficult Scaling

------------------------------------------------------------------------

# Disaster Recovery (DR)

Primary Data Center (Noida)

↓

VPN

↓

Disaster Recovery Site (Pune)

Purpose: - Business Continuity - High Availability

------------------------------------------------------------------------

# CAPEX vs OPEX

## CAPEX

One-time investment. - Servers - Storage - Networking

## OPEX

Recurring operational expenses. - Electricity - Maintenance - Internet -
Staff

------------------------------------------------------------------------

# Virtualization

Virtualization allows multiple Virtual Machines (VMs) on one physical
server.

``` text
Physical Server
      ↓
 Hypervisor
      ↓
 VM1 VM2 VM3 VM4 VM5
```

Each VM has: - CPU - RAM - Disk - Operating System

## Hypervisors

### VMware

-   VMware Workstation
-   VMware ESXi

### Microsoft

-   Hyper-V

Benefits: - Better Hardware Utilization - Lower Cost - Fast Deployment -
Isolation - High Availability

------------------------------------------------------------------------

# On-Premises Infrastructure

Infrastructure owned and maintained by the organization.

Challenges: - High Initial Investment - Hardware Maintenance - Limited
Scalability

------------------------------------------------------------------------

# Cloud Computing

Instead of buying infrastructure, companies rent it from cloud
providers.

Major Providers: - Amazon Web Services (AWS) - Microsoft Azure - Google
Cloud Platform (GCP)

Advantages: - Pay-as-you-go - Fast Deployment - Easy Scaling - Global
Availability

------------------------------------------------------------------------

# Traditional IT vs Cloud

  Traditional IT               Cloud
  ---------------------------- -----------------------------------
  Buy Servers                  Rent Servers
  High CAPEX                   Lower Initial Cost
  Manual Scaling               Auto Scaling
  Company Maintains Hardware   Cloud Provider Maintains Hardware

------------------------------------------------------------------------

# Interview Questions

1.  What is an Enterprise Architect?
2.  Explain 3-Tier Architecture.
3.  Difference between 1-Tier, 2-Tier, and 3-Tier Architecture.
4.  What is Virtualization?
5.  What is a Hypervisor?
6.  VMware vs Hyper-V.
7.  CAPEX vs OPEX.
8.  What is Disaster Recovery?
9.  What is On-Premises Infrastructure?
10. Why do companies migrate to the Cloud?
