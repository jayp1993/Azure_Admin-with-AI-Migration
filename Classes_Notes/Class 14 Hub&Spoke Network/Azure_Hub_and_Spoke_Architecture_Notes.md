# Azure Hub & Spoke Architecture

## 1. What is Hub & Spoke?

**Hub & Spoke** is an Azure network architecture where a central **Hub VNet** provides shared connectivity, security, and services to multiple **Spoke VNets**.

```text
                    HUB VNET
                10.0.0.0/16
                     │
          ┌──────────┼──────────┐
          │          │          │
       Peering    Peering    Peering
          │          │          │
          ▼          ▼          ▼
      Spoke-1     Spoke-2     Spoke-3
       Prod         Dev        Test
```

---

## 2. Hub VNet

The Hub is the central network.

Common services deployed in the Hub:

- Azure Firewall
- VPN Gateway
- ExpressRoute Gateway
- Azure Bastion
- DNS services
- Shared management services
- Monitoring services
- Network Virtual Appliances (NVA)

Example:

```text
                    HUB VNET
                 10.0.0.0/16
                      │
       ┌──────────────┼──────────────┐
       │              │              │
    Firewall      VPN Gateway     Bastion
       │
       │
   Shared Services
```

---

## 3. Spoke VNet

Spoke VNets are workload or environment-specific networks.

Examples:

```text
Hub
 │
 ├── Production Spoke
 ├── Development Spoke
 ├── Testing Spoke
 └── Data Spoke
```

Spokes may contain:

- Web VMs
- Application VMs
- Databases
- AKS
- Application Gateway
- Private Endpoints

---

## 4. Why Use Hub & Spoke?

The main purpose is **centralized networking and security**.

Common reasons:

- Multiple VNets need connectivity
- Centralized firewall/security is required
- Multiple environments exist
- Shared services need to be accessed
- VPN or ExpressRoute connectivity is required
- Centralized governance is required
- Enterprise-scale network architecture is needed

---

## 5. Real Production Example

Suppose a company has:

```text
Hub VNet
10.0.0.0/16

Production
10.10.0.0/16

Development
10.20.0.0/16

Testing
10.30.0.0/16
```

Architecture:

```text
                    HUB
                 10.0.0.0/16
                      │
          ┌───────────┼───────────┐
          │           │           │
          ▼           ▼           ▼
        PROD         DEV         TEST
     10.10.0.0/16 10.20.0.0/16 10.30.0.0/16
```

Each Spoke VNet is commonly connected to the Hub using **VNet Peering**.

---

## 6. Hub & Spoke + Azure Firewall

Azure Firewall can be deployed in the Hub for centralized traffic inspection.

```text
                 HUB VNET
                     │
               Azure Firewall
                     │
          ┌──────────┼──────────┐
          │          │          │
          ▼          ▼          ▼
        PROD        DEV        TEST
```

Example:

```text
Spoke-1 → Azure Firewall → Spoke-2
```

Firewall policies can be used to control traffic.

---

## 7. Hub & Spoke + On-Premises

For hybrid connectivity, the Hub can contain VPN Gateway or ExpressRoute Gateway.

```text
              On-Premises
                   │
             VPN / ExpressRoute
                   │
                   ▼
               HUB VNET
                   │
             Azure Firewall
                   │
       ┌───────────┼───────────┐
       ▼           ▼           ▼
     PROD         DEV         TEST
```

The Hub becomes the centralized connectivity point.

---

## 8. Hub & Spoke + VPN Gateway

```text
On-Prem
   │
   │ Site-to-Site VPN
   │
   ▼
Hub VNet
   │
   ├── Spoke-Prod
   ├── Spoke-Dev
   └── Spoke-Test
```

Spokes can use the Hub's gateway when the appropriate peering and gateway-transit configuration is enabled.

---

## 9. Gateway Transit

Gateway Transit is commonly used in Hub-and-Spoke architectures.

Typical configuration:

```text
Hub VNet
→ Allow gateway transit = Enabled

Spoke VNet
→ Use remote gateways = Enabled
```

Concept:

```text
             On-Prem
                │
           VPN Gateway
                │
               HUB
             /                 /                Spoke     Spoke
```

The Spokes can use the Hub's VPN/ExpressRoute gateway when configured appropriately.

---

## 10. Is Hub & Spoke Transitive?

**VNet Peering itself is non-transitive.**

Example:

```text
Spoke-A
   │
   ▼
  Hub
   │
   ▼
Spoke-B
```

A-Hub peering and Hub-B peering do **not automatically create direct A-B connectivity**.

```text
A ↔ Hub
Hub ↔ B

A ↛ B automatically
```

If Spoke-to-Spoke communication is required, use an appropriate architecture with Azure Firewall/NVA and routing.

---

## 11. Hub & Spoke + NSG

Network Security Groups still apply.

Example:

```text
VM-A
   │
   ▼
NSG
   │
   ▼
VNet Peering
   │
   ▼
VM-B
```

Even if peering is connected, an NSG rule can block the required traffic.

---

## 12. Hub & Spoke + Route Tables

For advanced traffic flows, User Defined Routes (UDRs) can be used.

Example:

```text
Spoke
  │
  ▼
UDR
  │
  ▼
Azure Firewall
  │
  ▼
Destination
```

UDRs are commonly used when traffic needs to pass through a firewall or NVA.

---

## 13. IP Address Planning

Hub and Spoke address spaces should **not overlap**.

### Correct

```text
Hub  → 10.0.0.0/16
Prod → 10.10.0.0/16
Dev  → 10.20.0.0/16
Test → 10.30.0.0/16
```

### Incorrect

```text
Hub  → 10.0.0.0/16
Prod → 10.0.0.0/16
```

Overlapping address spaces create routing problems.

---

## 14. Hub & Spoke vs Normal VNet

| Feature | Normal VNet | Hub & Spoke |
|---|---|---|
| Architecture | Simple | Centralized |
| Network management | Distributed | Centralized |
| Security | Per VNet | Centralized possible |
| Firewall | Individual/optional | Central Hub possible |
| VPN Gateway | Based on requirement | Common Hub gateway |
| Scalability | Suitable for simple environments | High |
| Enterprise use | Smaller/simple workloads | Large environments |

---

## 15. When Should We Use a Normal VNet?

For a small or simple application, a normal VNet may be enough.

Example:

```text
VNet
 │
 ├── Web Subnet
 ├── App Subnet
 └── DB Subnet
```

If centralized networking is not required, Hub & Spoke may add unnecessary complexity.

---

## 16. When Should We Use Hub & Spoke?

Use Hub & Spoke when:

- Multiple VNets exist
- Multiple environments need connectivity
- Central Firewall is required
- Central VPN/ExpressRoute connectivity is required
- Shared services are required
- Centralized security is required
- Enterprise-scale architecture is needed
- Spoke-to-Spoke traffic requires centralized inspection/control

---

## 17. Hub & Spoke with Shared Services

Common shared services can be hosted in the Hub.

```text
                     HUB
                      │
        ┌─────────────┼─────────────┐
        │             │             │
      DNS          Firewall       Bastion
        │
   Shared Services
        │
   Monitoring
```

Spokes can consume the shared services.

---

## 18. Complete Production Architecture

```text
                         INTERNET
                            │
                            ▼
                    Azure Firewall
                            │
                       HUB VNET
                      10.0.0.0/16
                            │
          ┌─────────────────┼─────────────────┐
          │                 │                 │
          ▼                 ▼                 ▼
      PROD SPOKE        DEV SPOKE         TEST SPOKE
      10.10.0.0/16     10.20.0.0/16      10.30.0.0/16
          │                 │                 │
      Web/App/DB         Web/App           Web/App
```

---

## 19. Advantages

### Centralized Security

Firewall and security controls can be managed centrally.

### Scalability

New Spoke VNets can be added without redesigning the entire network.

```text
New Spoke
    │
    ▼
Hub Peering
```

### Centralized Connectivity

VPN Gateway and ExpressRoute Gateway can be placed in the Hub.

### Shared Services

DNS, Bastion, monitoring, and other common services can be centralized.

### Better Governance

Network architecture and security controls can be managed centrally.

### Cost Optimization

Common services can be shared instead of deploying duplicate services in every VNet.

---

## 20. Disadvantages

Hub & Spoke also introduces some challenges:

- Architecture is more complex
- Routing needs careful design
- Firewall/NVA management may be required
- Hub becomes a critical network component
- IP address planning becomes important
- Additional networking components can increase cost
- Troubleshooting can be more complex

---

## 21. Important Interview Questions

### Q1. What is Hub & Spoke architecture?

> Hub & Spoke is a network architecture where a central Hub VNet provides shared connectivity, security, and services to multiple Spoke VNets.

### Q2. Why do we use Hub & Spoke?

> To centralize networking, security, connectivity, and shared services while keeping workloads separated into different VNets.

### Q3. What is a Hub VNet?

> The central VNet that hosts shared networking and security services such as Azure Firewall, VPN Gateway, ExpressRoute Gateway, and Bastion.

### Q4. What is a Spoke VNet?

> A workload-specific VNet connected to the Hub, commonly used for Production, Development, Testing, or individual applications.

### Q5. How are Hub and Spokes connected?

> Typically using Azure VNet Peering.

### Q6. Is VNet Peering transitive?

> No. VNet Peering is non-transitive.

### Q7. Can Spokes communicate with each other?

> Not automatically just because both are connected to the Hub. Appropriate routing and security architecture is required.

### Q8. Where can Azure Firewall be deployed?

> In many Hub-and-Spoke designs, Azure Firewall is deployed in the Hub for centralized traffic inspection and control.

---

## 22. Quick Revision

```text
                 HUB
          ┌──────┼──────┐
          │      │      │
       Firewall VPN   Bastion
          │
     ┌────┼────┐
     │    │    │
   PROD  DEV  TEST
  Spoke Spoke Spoke
```

### Remember

> **Hub = Centralized Networking & Security**

> **Spoke = Application/Workload Network**

> **VNet Peering = Hub ↔ Spoke Connectivity**

> **Azure Firewall = Centralized Traffic Inspection**

> **VPN/ExpressRoute = Hybrid Connectivity**

> **VNet Peering = Non-Transitive**
