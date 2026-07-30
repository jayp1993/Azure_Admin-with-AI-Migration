# Class 8 -- Azure CLI Notes

> Based on the uploaded Class 8 Azure CLI PDF.

## What is Azure CLI?

Azure CLI (Azure Command Line Interface) is a command-line tool used to
manage Azure resources from the terminal.

-   Open Source
-   Cross-platform (Windows, Linux, macOS)
-   Command-based interface
-   Used for Azure automation

------------------------------------------------------------------------

# Ways to Deploy Azure Resources

## 1. Manual Deployment

-   Azure Portal (GUI)
-   Best for beginners
-   Suitable for small environments

## 2. Automation Deployment

### Imperative Method

You tell Azure **how** to perform each step.

**Tools** - Azure CLI - Azure PowerShell

### Declarative Method

You describe **what** you want, and Azure provisions it.

**Tools** - ARM Template (JSON) - Bicep - Terraform (HCL)

------------------------------------------------------------------------

# Infrastructure as Code (IaC)

Infrastructure is defined using code instead of manual configuration.

**Benefits** - Automation - Reusability - Version Control - Reduced
Human Error

------------------------------------------------------------------------

# ARM Template

-   Azure Native
-   JSON Format
-   Declarative

# Bicep

-   Azure Native
-   Simpler than ARM Templates
-   Declarative

# Terraform

-   Open Source
-   HCL Language
-   Multi-Cloud Support
-   State File Management

------------------------------------------------------------------------

# Azure CLI Installation

Azure CLI can be used from:

-   Windows Command Prompt (CMD)
-   Windows PowerShell

------------------------------------------------------------------------

# Azure CLI Help Commands

``` bash
az --help
az group --help
az group create --help
```

------------------------------------------------------------------------

# Azure CLI Command Structure

``` bash
az <service> <operation> [arguments]
```

Example:

``` bash
az group create -l centralindia -n CloudTechHacks
```

  Parameter   Description
  ----------- ------------------------
  `az`        Azure CLI
  `group`     Resource Group Service
  `create`    Create operation
  `-l`        Location
  `-n`        Resource Group Name

------------------------------------------------------------------------

# Arguments

## Required

Mandatory values required by the command.

## Optional

Additional parameters that modify the command behavior.

------------------------------------------------------------------------

# Common Azure CLI Commands

## Login

``` bash
az login
```

## Show Current Account

``` bash
az account show
```

## List Resource Groups

``` bash
az group list
```

## Create Resource Group

``` bash
az group create -l centralindia -n CloudTechHacks
```

## Delete Resource Group

``` bash
az group delete -n CloudTechHacks
```

## List Azure Locations

``` bash
az account list-locations
```

## Logout

``` bash
az logout
```

------------------------------------------------------------------------

# Interview Questions

## What is Azure CLI?

Azure CLI is a command-line tool used to create, manage, and automate
Azure resources.

## Azure Portal vs Azure CLI

  Azure Portal        Azure CLI
  ------------------- ------------------------
  GUI                 Command Line
  Manual              Automation
  Beginner Friendly   Administrator Friendly

## Imperative vs Declarative

  Imperative    Declarative
  ------------- --------------
  Azure CLI     Terraform
  Defines how   Defines what

## What is IaC?

Infrastructure as Code is the practice of provisioning infrastructure
using code.

## Terraform Language

Terraform uses **HCL (HashiCorp Configuration Language)**.

------------------------------------------------------------------------

# Key Takeaways

-   Azure resources can be deployed manually or using automation.
-   Azure CLI follows the Imperative approach.
-   ARM Template, Bicep, and Terraform are Declarative tools.
-   Terraform supports multiple cloud providers.
-   Learn help commands before using Azure CLI.
