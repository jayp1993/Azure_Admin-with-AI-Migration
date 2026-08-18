# Azure Regional VNet Peering

## 1. What is Regional VNet Peering?

**Regional VNet Peering** connects two Azure Virtual Networks (VNets) located in the **same Azure region** using Microsoft's private backbone network.

### Example

```text
Azure Region: Central India

VNet-A                         VNet-B
10.0.0.0/16                   10.1.0.0/16
    │                             │
    └──────── VNet Peering ───────┘
```

After peering, resources in both VNets can communicate using private IP addresses, subject to routing and security rules.

---

## 2. Why Do We Use Regional VNet Peering?

Common use cases:

- Connect different VNets
- Enable application-to-application communication
- Access shared services
- Build Hub-and-Spoke architecture
- Provide private connectivity between VNets
- Connect separate environments such as Dev, Test, and Production

### Example

```text
VNet-Production
       │
       │ Peering
       │
VNet-Shared-Services
       │
       ├── DNS
       ├── Monitoring
       └── Management Services
```

---

## 3. Regional VNet Peering vs Global VNet Peering

| Feature | Regional Peering | Global Peering |
|---|---|---|
| VNet locations | Same Azure region | Different Azure regions |
| Example | Central India ↔ Central India | Central India ↔ East US |
| Connectivity | Private | Private |
| Azure Backbone | Yes | Yes |
| Common use | Same-region architecture | Multi-region architecture |

### Regional Example

```text
Central India
 ├── VNet-A
 └── VNet-B
       ↕
    Peering
```

### Global Example

```text
Central India                    East US
   VNet-A  ───── Global Peering ───── VNet-B
```

---

## 4. Important Requirement: Non-Overlapping Address Spaces

The address spaces of peered VNets should **not overlap**.

### ❌ Incorrect

```text
VNet-A → 10.0.0.0/16
VNet-B → 10.0.0.0/16
```

### ✅ Correct

```text
VNet-A → 10.0.0.0/16
VNet-B → 10.1.0.0/16
```

Overlapping address spaces create routing conflicts.

---

## 5. How Regional VNet Peering Works

Suppose:

```text
VNet-A
10.0.0.0/16
     │
     │ Peering
     │
VNet-B
10.1.0.0/16
```

VM-A:

```text
10.0.1.4
```

VM-B:

```text
10.1.1.4
```

Traffic can travel between the VNets over Microsoft's backbone network.

```text
VM-A
10.0.1.4
   │
   ▼
VNet-A
   │
   ▼
Microsoft Azure Backbone
   │
   ▼
VNet-B
   │
   ▼
VM-B
10.1.1.4
```

---

## 6. VNet Peering is Bidirectional

VNet peering involves peering configuration on both sides.

```text
VNet-A
  │
  ├── Peering → VNet-B
  │
VNet-B
  │
  └── Peering → VNet-A
```

In the Azure Portal, you should see the peering as **Connected** on both VNets.

---

## 7. Important Peering Settings

### Allow virtual network access

Normally enabled.

It allows resources in the peered VNets to communicate.

---

### Allow forwarded traffic

Useful when traffic is being forwarded through an NVA, firewall, or similar network appliance.

Example:

```text
VNet-A
   │
   ▼
Azure Firewall / NVA
   │
   ▼
VNet-B
```

---

### Allow gateway transit

Commonly used in Hub-and-Spoke architectures.

Example:

```text
             Hub VNet
           VPN Gateway
                │
        ┌───────┴───────┐
        │               │
     Spoke-1          Spoke-2
```

The Hub can allow its gateway to be used by connected spoke VNets.

---

### Use remote gateways

A spoke VNet can use the Hub VNet's VPN/ExpressRoute gateway when the peering is configured appropriately.

Typical configuration:

```text
Hub VNet
→ Allow gateway transit = Enabled

Spoke VNet
→ Use remote gateways = Enabled
```

---

## 8. Regional Peering + NSG

Peering alone does not guarantee that traffic will be allowed.

**Network Security Groups (NSGs)** can allow or block the traffic.

Example:

```text
VM-A
10.0.1.4
   │
   ▼
NSG
   │
   ▼
VNet Peering
   │
   ▼
VM-B
10.1.1.4
```

If an NSG denies the required traffic, communication can fail even when peering is connected.

---

## 9. Regional Peering + Route Tables

VNet peering provides reachability between the VNet address spaces.

Example:

```text
VNet-A → 10.0.0.0/16
VNet-B → 10.1.0.0/16
```

For advanced routing scenarios involving NVAs, Azure Firewall, or custom routes, **UDR (User Defined Routes)** may also be required.

---

## 10. Real Production Example

Suppose a company has:

```text
Production VNet
10.10.0.0/16

Development VNet
10.20.0.0/16

Shared Services VNet
10.30.0.0/16
```

All VNets are in the same Azure region.

Architecture:

```text
              Shared Services
               10.30.0.0/16
                     │
                     │ VNet Peering
                ┌────┴────┐
                │         │
                ▼         ▼
          Production   Development
          10.10.0.0/16 10.20.0.0/16
```

This allows private communication between required resources while keeping environments logically separated.

---

## 11. Important Considerations

### 1. Avoid overlapping address spaces

```text
❌ 10.0.0.0/16
   10.0.0.0/16
```

Use unique address spaces:

```text
✅ 10.0.0.0/16
   10.1.0.0/16
```

### 2. VNet peering is non-transitive

This is a very important interview point.

Suppose:

```text
VNet-A
   │
   ▼
VNet-B
   │
   ▼
VNet-C
```

If A is peered with B and B is peered with C, **A does not automatically communicate with C through B**.

```text
A ↔ B
B ↔ C

A ↛ C automatically
```

For transitive connectivity, an appropriate architecture using a hub, firewall/NVA, routing, or other connectivity mechanism is required.

---

## 12. Hub-and-Spoke with Regional Peering

A common production architecture is:

```text
                    HUB VNET
                 10.0.0.0/16
                       │
          ┌────────────┼────────────┐
          │            │            │
       Peering      Peering      Peering
          │            │            │
          ▼            ▼            ▼
      Spoke-1       Spoke-2       Spoke-3
       Prod           Dev           Test
```

The Hub may contain:

- Azure Firewall
- VPN Gateway
- ExpressRoute Gateway
- Azure Bastion
- DNS services
- Shared management services

---

## 13. Regional Peering vs VPN Gateway

| Feature | VNet Peering | VPN Gateway |
|---|---|---|
| Connectivity | Azure backbone | VPN tunnel |
| Typical use | VNet-to-VNet | Azure-to-on-prem / VPN connectivity |
| Performance | Generally high | Depends on gateway SKU |
| Latency | Generally low | Higher than direct peering |
| Encryption | Not IPsec encryption by default | IPsec VPN |
| Gateway required | No | Yes |

---

## 14. Interview Questions

### Q1. What is Regional VNet Peering?

**Answer:**

> Regional VNet Peering connects two Azure VNets located in the same Azure region over Microsoft's private backbone network.

### Q2. Can two peered VNets have overlapping address spaces?

**Answer:**

> No. Overlapping address spaces can cause routing conflicts.

### Q3. Is VNet peering transitive?

**Answer:**

> No. VNet peering is non-transitive by default.

### Q4. Can VNets in different Azure regions be connected?

**Answer:**

> Yes. Global VNet Peering can connect VNets in different Azure regions.

### Q5. Does VNet peering require a VPN Gateway?

**Answer:**

> No. VNet peering does not require a VPN Gateway.

### Q6. Can resources in peered VNets communicate using private IPs?

**Answer:**

> Yes, provided the required routing and security rules allow the traffic.

### Q7. Can VNet Peering be used for Hub-and-Spoke architecture?

**Answer:**

> Yes. Hub-and-Spoke architectures commonly use VNet Peering to connect the Hub VNet with Spoke VNets.

---

## 15. One-Line Interview Definition

> **Regional VNet Peering is a private, low-latency connection between two Azure VNets in the same region using Microsoft's backbone network.**

---

## Quick Revision

```text
Regional VNet Peering
        │
        ├── Same Azure Region
        ├── Private connectivity
        ├── Microsoft backbone
        ├── No VPN Gateway required
        ├── Non-overlapping IP ranges
        ├── Non-transitive
        ├── NSG rules still apply
        ├── Can support Hub-and-Spoke
        └── Different regions → Global VNet Peering
```
