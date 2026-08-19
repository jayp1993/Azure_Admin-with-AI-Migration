# Azure VNet-to-VNet Connection — Complete Notes

## 1. What is VNet-to-VNet Connection?

Azure VNet-to-VNet VPN connection is used to connect two Azure Virtual Networks through a private, encrypted VPN connection.

```text
VNet-1                           VNet-2
10.1.0.0/16                      10.2.0.0/16
    |                                |
Subnet                           Subnet
10.1.1.0/24                      10.2.1.0/24
    |                                |
   VM-1                            VM-2
10.1.1.4                         10.2.1.4
    |                                |
VPN Gateway-1  <==============> VPN Gateway-2
                 IPsec VPN
```

After the connection is established, VMs can communicate with each other using their private IP addresses.

---

## 2. Why do we use VNet-to-VNet?

Common production scenarios:

### Scenario 1 — Application and Database

```text
VNet-App
10.10.0.0/16
   |
App Servers
   |
VPN Connection
   |
VNet-DB
10.20.0.0/16
   |
Database Servers
```

Used when application workloads in one VNet need private connectivity to resources in another VNet.

### Scenario 2 — Different Azure Regions

```text
East US                         West Europe

VNet-A                          VNet-B
10.1.0.0/16                     10.2.0.0/16
   |                               |
VPN Gateway  ================= VPN Gateway
                 VPN
```

### Scenario 3 — Different Subscriptions

VNet-to-VNet connectivity can be established between VNets in different Azure subscriptions, provided the required permissions and networking configuration are available.

### Scenario 4 — Different Tenants

VNets associated with different Microsoft Entra tenants can also be connected using VNet-to-VNet VPN, provided the appropriate permissions and gateway configuration are available.

---

## 3. Important Components

The major components are:

1. VNet-1
2. VNet-2
3. GatewaySubnet in VNet-1
4. GatewaySubnet in VNet-2
5. VPN Gateway-1
6. VPN Gateway-2
7. Public IP-1
8. Public IP-2
9. Local Network Gateway-1
10. Local Network Gateway-2
11. VPN Connection-1
12. VPN Connection-2

---

## 4. GatewaySubnet

A VPN Gateway requires a dedicated subnet named `GatewaySubnet`.

Example:

```text
VNet-1
10.1.0.0/16

├── AppSubnet
│   └── 10.1.1.0/24
│
└── GatewaySubnet
    └── 10.1.255.0/27
```

Important points:

- The subnet name must be exactly `GatewaySubnet`.
- Do not deploy VMs or application workloads into GatewaySubnet.
- Keep the subnet sufficiently sized for the VPN Gateway requirements.

---

## 5. VPN Gateway

VPN Gateway provides the VPN connectivity between the VNets.

```text
VNet-1
   |
GatewaySubnet
   |
VPN Gateway-1
   |
VPN Tunnel
   |
VPN Gateway-2
   |
GatewaySubnet
   |
VNet-2
```

Typical configuration:

```text
Gateway Type = VPN
VPN Type     = Route-based
```

Important VPN Gateway concepts:

- Gateway Type
- VPN Type
- SKU
- Public IP
- Active-active configuration
- BGP, if required

---

## 6. Public IP

VPN Gateways use public IP addresses for their VPN endpoints.

Example:

```text
VPN-Gateway-1
Public IP: 20.x.x.x

VPN-Gateway-2
Public IP: 40.x.x.x
```

The gateways use these endpoints to establish the VPN tunnel.

---

## 7. Local Network Gateway

A Local Network Gateway can represent the remote network/gateway information used by the VPN connection.

Example:

### VNet-1 side

```text
Local Network Gateway-2

Remote Address Space:
10.2.0.0/16

Remote VPN Gateway Public IP:
40.x.x.x
```

### VNet-2 side

```text
Local Network Gateway-1

Remote Address Space:
10.1.0.0/16

Remote VPN Gateway Public IP:
20.x.x.x
```

---

## 8. Address Space Planning

Address spaces should not overlap.

### Correct

```text
VNet-1 = 10.1.0.0/16
VNet-2 = 10.2.0.0/16
```

### Incorrect

```text
VNet-1 = 10.1.0.0/16
VNet-2 = 10.1.0.0/16
```

Overlapping address spaces cause routing problems.

---

## 9. VPN Connection

The VPN connection connects the two VPN gateways.

Conceptually:

```text
VPN Gateway-1
     |
     | VPN Connection
     |
VPN Gateway-2
```

Typical configuration:

```text
Connection Type = VNet-to-VNet
Shared Key      = Same on both sides
```

Example:

```text
Gateway-1 PSK = MySecureKey123
Gateway-2 PSK = MySecureKey123
```

The pre-shared key must match on both sides.

---

## 10. How VNet-to-VNet Traffic Works

Suppose:

```text
VM-1 = 10.1.1.4
VM-2 = 10.2.1.4
```

VM-1 sends traffic to VM-2:

```text
10.1.1.4
    |
    | Packet
    ↓
Azure Routing
    |
    ↓
VPN Gateway-1
    |
    | Encrypted IPsec Tunnel
    ↓
VPN Gateway-2
    |
    ↓
10.2.1.4
```

The response follows the reverse path.

---

## 11. Routing

Routing is one of the most important parts of VNet-to-VNet connectivity.

Azure must know that:

```text
10.2.0.0/16
```

is reachable through the appropriate VPN gateway.

Similarly, VNet-2 must know that:

```text
10.1.0.0/16
```

is reachable through the appropriate VPN gateway.

The VPN Gateway connection handles route exchange/propagation according to the configured routing model.

---

## 12. NSG Configuration

Even if the VPN tunnel is connected, VM traffic can still be blocked by an NSG.

Example:

```text
VM-1
10.1.1.4
   |
NSG
   |
VPN
   |
VM-2
10.2.1.4
```

If ICMP is blocked, this can fail:

```bash
ping 10.2.1.4
```

Therefore, check NSG rules on the source and destination sides.

---

## 13. ICMP

ICMP is a network protocol, not a TCP/UDP port.

For ping:

```text
Protocol = ICMP
Port     = Not applicable
```

Therefore, you do not specify a TCP/UDP port for ICMP ping.

The operating system firewall can also block ICMP.

---

## 14. Complete Architecture

```text
                 Azure
─────────────────────────────────────────────

        VNet-1                  VNet-2
    10.1.0.0/16              10.2.0.0/16

   ┌─────────────┐          ┌─────────────┐
   │     VM-1    │          │     VM-2    │
   │ 10.1.1.4    │          │ 10.2.1.4    │
   └──────┬──────┘          └──────┬──────┘
          │                         │
   GatewaySubnet              GatewaySubnet
          │                         │
   ┌──────▼──────┐          ┌──────▼──────┐
   │ VPN Gateway │          │ VPN Gateway │
   │      GW1    │          │      GW2    │
   └──────┬──────┘          └──────┬──────┘
          │                         │
          └──── IPsec VPN Tunnel ──┘
```

---

## 15. Portal Configuration — High-Level Steps

### Step 1 — Create VNet-1

```text
Name: VNet-1
Address Space: 10.1.0.0/16
```

Create:

```text
AppSubnet
10.1.1.0/24
```

Create:

```text
GatewaySubnet
10.1.255.0/27
```

### Step 2 — Create VNet-2

```text
Name: VNet-2
Address Space: 10.2.0.0/16
```

Create:

```text
AppSubnet
10.2.1.0/24
```

Create:

```text
GatewaySubnet
10.2.255.0/27
```

### Step 3 — Create Public IPs

Create:

```text
Public-IP-GW1
Public-IP-GW2
```

### Step 4 — Create VPN Gateway 1

Configure:

```text
VNet: VNet-1
Gateway Type: VPN
VPN Type: Route-based
Public IP: Public-IP-GW1
```

### Step 5 — Create VPN Gateway 2

Configure:

```text
VNet: VNet-2
Gateway Type: VPN
VPN Type: Route-based
Public IP: Public-IP-GW2
```

### Step 6 — Configure Remote Network/Gateway Information

Configure the remote VNet address space and VPN gateway information for each side.

### Step 7 — Create VPN Connections

Configure:

```text
Connection Type: VNet-to-VNet
Shared Key: Same PSK on both sides
```

### Step 8 — Validate Connectivity

From VM-1:

```bash
ping 10.2.1.4
```

From VM-2:

```bash
ping 10.1.1.4
```

---

## 16. Troubleshooting — Ping Request Timeout

If the VPN connection shows `Connected` but:

```bash
ping 10.2.1.4
```

returns:

```text
Request timeout
```

do not assume that the VPN Gateway is the problem.

Check the following.

### Check 1 — Address Spaces

```text
VNet-1 = 10.1.0.0/16
VNet-2 = 10.2.0.0/16
```

Make sure they do not overlap.

### Check 2 — VPN Connection Status

Expected:

```text
Connected
```

### Check 3 — NSG

Check whether the destination VM's NSG allows the required traffic.

### Check 4 — Linux Firewall

On Linux:

```bash
sudo ufw status
```

Also check:

```bash
sudo iptables -L
```

Depending on the Linux distribution, `firewalld` may also be relevant:

```bash
sudo firewall-cmd --state
```

### Check 5 — Windows Firewall

If the destination is Windows, verify that the appropriate inbound ICMP rule is enabled.

### Check 6 — Effective Routes

Check the VM's effective routes.

Expected destination:

```text
10.2.0.0/16
```

The route should point toward the appropriate VPN connectivity path.

### Check 7 — IP Flow Verify

Use Azure Network Watcher IP flow verify to determine whether NSG rules are allowing or denying the traffic.

### Check 8 — OS Firewall

A connected VPN tunnel does not automatically mean that the operating system firewall will allow ICMP.

---

## 17. VNet Peering vs VNet-to-VNet VPN

| Feature | VNet Peering | VNet-to-VNet VPN |
|---|---|---|
| Connectivity | Azure private backbone | VPN tunnel |
| VPN Gateway required | No | Yes |
| IPsec encryption | No | Yes |
| Performance | Generally higher | Depends on VPN Gateway SKU |
| Cost | Peering/data transfer charges | VPN Gateway + data transfer charges |
| Cross-region | Yes | Yes |
| Use case | Azure VNet connectivity | Encrypted VPN connectivity |

### Simple Rule

```text
Simple Azure-to-Azure private connectivity
                ↓
          VNet Peering

Encrypted VPN/IPsec requirement
                ↓
       VNet-to-VNet VPN
```

---

## 18. Production Scenario

Suppose a company has:

```text
Production VNet
10.10.0.0/16

DR VNet
10.20.0.0/16
```

Production:

```text
Web
App
DB
```

DR:

```text
DR Web
DR App
DR DB
```

The company requires encrypted connectivity between the production and DR environments.

Architecture:

```text
Production VNet
10.10.0.0/16
       |
 VPN Gateway
       |
       | IPsec VPN
       |
 VPN Gateway
       |
DR VNet
10.20.0.0/16
```

Production applications can then communicate with DR resources through their private IP addresses.

---

# 19. Interview Questions

### Q1. What is VNet-to-VNet VPN?

It connects two Azure VNets through an encrypted VPN tunnel.

### Q2. Does VNet-to-VNet require VPN Gateway?

Yes. Both VNets generally require VPN Gateways for this architecture.

### Q3. Can two VNets have overlapping address spaces?

No. Overlapping address spaces cause routing problems.

### Q4. Which VPN type is commonly used?

Route-based VPN.

### Q5. Is ICMP a port?

No. ICMP is a protocol, not a TCP/UDP port.

### Q6. VPN connection is Connected but ping fails. Why?

Possible reasons:

- NSG
- OS firewall
- Incorrect route
- Incorrect address space
- Routing issue
- ICMP blocked
- Application/service not listening

### Q7. Can VNet-to-VNet connect VNets in different Azure regions?

Yes.

### Q8. Can VNets in different subscriptions be connected?

Yes, with the required permissions and configuration.

### Q9. VNet Peering vs VNet-to-VNet VPN — which should you choose?

It depends on the requirement:

```text
High-performance Azure private connectivity
              ↓
        VNet Peering

Encrypted VPN/IPsec requirement
              ↓
      VNet-to-VNet VPN
```

### Q10. What is the purpose of GatewaySubnet?

It is the dedicated subnet used for deploying the VPN Gateway.

---

# 20. Key Interview Takeaway

> A VPN Gateway showing `Connected` confirms that the VPN tunnel is established, but it does not guarantee end-to-end VM connectivity.

For VM-to-VM troubleshooting, always validate:

```text
1. Address Space
2. VPN Connection
3. Routes
4. NSG
5. OS Firewall
6. ICMP/Application Port
7. Network Watcher / IP Flow Verify
```

This is the most important troubleshooting approach for a real-time Azure VNet-to-VNet VPN scenario.
