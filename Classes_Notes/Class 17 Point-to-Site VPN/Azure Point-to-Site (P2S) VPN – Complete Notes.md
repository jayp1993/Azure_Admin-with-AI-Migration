# Azure Point-to-Site (P2S) VPN – Complete Notes

## 1. What is Point-to-Site VPN?

**Point-to-Site (P2S) VPN** allows an individual client computer/laptop to securely connect to an **Azure Virtual Network (VNet)** over an encrypted VPN tunnel.

It is mainly used when a user needs to access Azure private resources such as:

- Azure VMs
- Private IP addresses
- Internal applications
- Azure Storage private endpoints
- Internal databases
- Other resources deployed inside the VNet

### Simple Example

```text
Employee Laptop
      |
      | Encrypted VPN Tunnel
      |
   Internet
      |
      |
Azure VPN Gateway
      |
      |
Azure VNet
   |       |
  VM      Database
```

---

# 2. P2S vs S2S VPN

| Feature | Point-to-Site | Site-to-Site |
|---|---|---|
| Connection | Individual client → Azure VNet | On-premises network → Azure VNet |
| Client VPN software | Required | Usually not required |
| Users | Individual users | Entire office/network |
| VPN Gateway | Required | Required |
| On-prem VPN device | Not required | Required |
| Typical use | Remote employees | Branch/Datacenter connectivity |
| Example | Laptop → Azure VNet | Office → Azure VNet |

### Easy way to remember

**P2S = Person to Azure**

**S2S = Site to Azure**

---

# 3. Azure P2S VPN Components

A typical P2S VPN contains:

```text
                    Azure
        +-----------------------------+
        |                             |
Laptop  |      VPN Gateway            |
  |     |          |                  |
  |-----|----------|                  |
        |          |                  |
        |        VNet                 |
        |       /    \                |
        |      VM    Private App       |
        |                             |
        +-----------------------------+
```

Important components:

1. Azure Subscription
2. Resource Group
3. Virtual Network (VNet)
4. GatewaySubnet
5. Virtual Network Gateway
6. Point-to-Site Configuration
7. Address Pool
8. Authentication method
9. VPN Client
10. Client Certificate/RADIUS/Entra ID depending on authentication method

---

# 4. GatewaySubnet

The Azure VPN Gateway requires a dedicated subnet called:

```text
GatewaySubnet
```

Example:

```text
VNet: 10.0.0.0/16

Subnets:

GatewaySubnet       10.0.0.0/27
WebSubnet           10.0.1.0/24
AppSubnet           10.0.2.0/24
DBSubnet            10.0.3.0/24
```

### Important

Do not deploy VMs or normal workloads inside `GatewaySubnet`.

The VPN Gateway is deployed into this subnet.

---

# 5. P2S Address Pool

When a client connects through P2S VPN, Azure assigns the client a VPN IP address.

For example:

```text
P2S Address Pool:

172.16.100.0/24
```

Clients may receive:

```text
172.16.100.1
172.16.100.2
172.16.100.3
...
```

The P2S client address pool should **not overlap** with:

- Azure VNet address space
- On-premises network
- Other connected networks

### Example

```text
Azure VNet:

10.0.0.0/16

P2S Pool:

172.16.100.0/24
```

This is a good design.

---

# 6. P2S Authentication Methods

Azure P2S VPN supports different authentication approaches depending on the VPN protocol/gateway configuration.

Common approaches include:

### 1. Azure Certificate Authentication

```text
Client Certificate
        |
        v
Azure VPN Gateway
        |
        v
Azure VNet
```

The client proves its identity using a client certificate.

This is commonly used for labs and certificate-based P2S deployments.

---

### 2. Microsoft Entra ID Authentication

Users authenticate using their Microsoft Entra ID identity.

```text
User
 |
 | Sign in
 v
Microsoft Entra ID
 |
 | Authentication
 v
VPN Gateway
 |
 v
Azure VNet
```

Advantages:

- Centralized identity
- User-based authentication
- MFA/Conditional Access can be part of the identity architecture
- Easier user management than distributing individual certificates

---

### 3. RADIUS Authentication

RADIUS can be used when authentication needs to be integrated with an external authentication infrastructure.

Typical architecture:

```text
VPN Client
    |
    v
Azure VPN Gateway
    |
    v
RADIUS Server
    |
    v
Active Directory
```

---

# 7. VPN Protocols

Important P2S VPN protocols include:

## IKEv2

IKEv2 is an IPsec-based VPN protocol.

```text
Client
   |
IKEv2/IPsec
   |
VPN Gateway
```

Advantages:

- Strong security
- Native support on many operating systems
- Good performance
- Suitable for enterprise environments

---

## OpenVPN

OpenVPN uses SSL/TLS.

```text
Client
   |
SSL/TLS
   |
VPN Gateway
```

Advantages:

- Flexible
- Works well across different networks
- Useful when IKEv2 isn't suitable
- Supports certificate and identity-based authentication depending on configuration

---

## SSTP

SSTP uses SSL/TLS over TCP.

It is mainly associated with Windows-based VPN connectivity.

```text
Client
   |
SSL/TLS
   |
VPN Gateway
```

---

# 8. Which Protocol Should You Use?

| Protocol | Typical Use |
|---|---|
| IKEv2 | Secure IPsec-based VPN connectivity |
| OpenVPN | Cross-platform and flexible VPN connectivity |
| SSTP | Windows-oriented scenarios |
| WireGuard | Not the standard Azure VPN Gateway P2S protocol |

### Practical recommendation

For a modern Azure P2S deployment:

**OpenVPN + Microsoft Entra ID** can be a good choice for user-based authentication.

For certificate-based P2S labs:

**OpenVPN/IKEv2 + Certificate Authentication** can be used depending on the selected configuration.

---

# 9. P2S VPN Architecture

```text
                         Internet
                            |
                            |
                     +-------------+
                     | VPN Client  |
                     |   Laptop    |
                     +-------------+
                            |
                     Encrypted Tunnel
                            |
                            |
                  +--------------------+
                  | Azure VPN Gateway  |
                  +--------------------+
                            |
                      GatewaySubnet
                            |
              +-------------+-------------+
              |                           |
        +-----------+               +-----------+
        | Web VM     |               | App VM    |
        |10.0.1.4    |               |10.0.2.4   |
        +-----------+               +-----------+
                            |
                       Azure VNet
```

---

# 10. How P2S VPN Works

Step-by-step flow:

### Step 1

User starts the VPN client on their laptop.

### Step 2

VPN client connects to the Azure VPN Gateway public IP.

### Step 3

Authentication takes place.

For example:

```text
Certificate
```

or

```text
Microsoft Entra ID
```

or

```text
RADIUS
```

### Step 4

VPN Gateway validates the authentication.

### Step 5

A VPN tunnel is established.

### Step 6

The client receives an IP from the P2S address pool.

Example:

```text
Client IP:

172.16.100.10
```

### Step 7

The client can access permitted private Azure resources.

Example:

```text
172.16.100.10
       |
       | VPN
       |
10.0.0.4 Azure VM
```

---

# 11. Prerequisites

Before configuring P2S VPN, prepare:

- Azure Subscription
- Resource Group
- Azure VNet
- GatewaySubnet
- Azure VPN Gateway
- Public IP
- P2S address pool
- VPN authentication method
- VPN client

---

# 12. Create VNet

Go to:

```text
Azure Portal
    ↓
Virtual Networks
    ↓
Create
```

Example:

```text
VNet Name:

P2S-VNet

Address Space:

10.0.0.0/16
```

Create application subnet:

```text
Subnet:

AppSubnet

Address:

10.0.1.0/24
```

---

# 13. Create GatewaySubnet

Inside the VNet:

```text
Subnets
   ↓
Add subnet
```

Use:

```text
Subnet name:

GatewaySubnet

Address range:

10.0.255.0/27
```

### Important

The subnet name must be:

```text
GatewaySubnet
```

Do not name it:

```text
VPNSubnet
Gateway
MyGatewaySubnet
```

Use the standard name:

```text
GatewaySubnet
```

---

# 14. Create Public IP

Go to:

```text
Public IP addresses
      ↓
Create
```

Example:

```text
Name:

P2S-VPN-PIP
```

The VPN Gateway uses this public IP to receive VPN connections from clients.

---

# 15. Create Virtual Network Gateway

Go to:

```text
Azure Portal
   ↓
Virtual Network Gateways
   ↓
Create
```

Configure:

```text
Name:
P2S-VPN-Gateway

Region:
Same region as VNet

Gateway type:
VPN

SKU:
Select an appropriate supported SKU

Generation:
Select according to requirement

Virtual network:
P2S-VNet

Public IP:
P2S-VPN-PIP
```

Then create the gateway.

### Important

VPN Gateway deployment can take several minutes.

---

# 16. Configure Point-to-Site

After the VPN Gateway is created:

```text
VPN Gateway
    ↓
Point-to-site configuration
```

Configure:

### Address Pool

Example:

```text
172.16.100.0/24
```

This pool provides IP addresses to VPN clients.

---

# 17. Configure Authentication

Select the authentication method according to the requirement.

For certificate authentication:

```text
Tunnel type:
IKEv2 / OpenVPN

Authentication:
Azure Certificate
```

For Entra ID:

```text
Authentication:
Microsoft Entra ID
```

For RADIUS:

```text
Authentication:
RADIUS
```

The exact available combinations depend on the VPN Gateway configuration and Azure-supported options.

---

# 18. Certificate-Based Authentication

Certificate authentication generally involves:

```text
Root Certificate
       |
       +---- Client Certificate 1
       |
       +---- Client Certificate 2
       |
       +---- Client Certificate 3
```

The VPN Gateway trusts the configured root certificate.

The client uses a certificate signed by that trusted root.

---

# 19. Certificate Authentication Flow

```text
Client Laptop
     |
     | Client Certificate
     |
     v
Azure VPN Gateway
     |
     | Validate certificate
     |
     v
Trusted Root Certificate
     |
     v
Authentication Successful
     |
     v
VPN Tunnel Established
```

---

# 20. Microsoft Entra ID Authentication Flow

```text
User
 |
 | VPN connection
 v
Azure VPN Gateway
 |
 | Authentication request
 v
Microsoft Entra ID
 |
 | User authentication
 v
Authentication Success
 |
 v
VPN Tunnel
 |
 v
Azure VNet
```

This is useful when you want centralized identity-based access.

---

# 21. Download VPN Client

After P2S configuration:

```text
VPN Gateway
    ↓
Point-to-site configuration
    ↓
Download VPN client
```

Azure generates the appropriate VPN client configuration.

Download it to your local machine.

Extract the downloaded ZIP file.

---

# 22. Install VPN Client

For Windows, depending on the selected authentication/protocol, Azure provides the relevant VPN client configuration.

After installation/configuration:

```text
Windows
   ↓
Network & Internet
   ↓
VPN
   ↓
Azure VPN
   ↓
Connect
```

If Entra ID authentication is configured, the user may be prompted to sign in.

---

# 23. Test the VPN Connection

After connecting, verify the VPN client IP.

On Windows:

```powershell
ipconfig
```

Check for the VPN adapter.

Then test the Azure VM private IP:

```powershell
ping 10.0.1.4
```

For Linux:

```bash
ip addr
```

and:

```bash
ping 10.0.1.4
```

---

# 24. Important: Ping May Not Work

If VPN shows:

```text
Connected
```

but:

```text
ping 10.0.1.4
```

fails, it does not necessarily mean the VPN is broken.

Check:

1. NSG
2. Windows Firewall
3. Linux firewall
4. Routing
5. VM private IP
6. VPN client routes
7. Network security configuration

---

# 25. ICMP and Ping

Ping uses:

```text
ICMP
```

It does **not** use TCP port 80 or TCP port 443.

For example:

```text
ping 10.0.1.4
```

uses ICMP Echo Request.

If you want to test HTTP:

```text
curl http://10.0.1.4
```

or:

```text
Test-NetConnection 10.0.1.4 -Port 80
```

---

# 26. NSG Consideration

Suppose VM has:

```text
Private IP:
10.0.1.4
```

P2S client:

```text
172.16.100.10
```

NSG must allow the required traffic from the P2S client address range if NSG rules restrict it.

Example conceptual rule:

```text
Source:
172.16.100.0/24

Destination:
10.0.1.0/24

Protocol:
Required protocol

Action:
Allow
```

---

# 27. Common Troubleshooting

## Problem 1: VPN Client Cannot Connect

Check:

- VPN Gateway status
- Public IP
- VPN client configuration
- Authentication configuration
- Certificate validity
- Entra ID configuration
- Firewall/network restrictions

---

## Problem 2: Authentication Failed

For certificate authentication, check:

- Root certificate
- Client certificate
- Certificate expiration
- Certificate private key
- Certificate installed in correct certificate store
- Subject/SAN/configuration as required

For Entra ID, check:

- Tenant configuration
- Audience/issuer configuration
- User authentication
- VPN application configuration
- Required permissions

---

## Problem 3: VPN Connected but VM Not Reachable

Check:

```text
VPN Connection
      ↓
Client IP
      ↓
Route
      ↓
NSG
      ↓
VM Firewall
      ↓
Application
```

---

# 28. P2S Routing

When the VPN connects, the client needs a route to the Azure VNet.

Example:

```text
Client:

172.16.100.10

Azure VNet:

10.0.0.0/16
```

Traffic:

```text
172.16.100.10
      |
      | VPN Tunnel
      v
Azure VPN Gateway
      |
      v
10.0.0.0/16
```

---

# 29. P2S vs VNet Peering

| Feature | P2S VPN | VNet Peering |
|---|---|---|
| Connects | User → VNet | VNet → VNet |
| Encryption | VPN tunnel | Azure backbone |
| Client VPN | Required | No |
| Internet-based | Yes | No |
| Main use | Remote access | Network-to-network connectivity |

---

# 30. P2S vs S2S vs VNet Peering

| Feature | P2S | S2S | VNet Peering |
|---|---|---|---|
| User → Azure | Yes | No | No |
| Network → Azure | No | Yes | No |
| VNet → VNet | No | Possible via VPN architecture | Yes |
| VPN Gateway | Yes | Yes | No |
| VPN client | Yes | No | No |
| Encryption tunnel | Yes | Yes | Azure backbone |

---

# 31. Real-Time Scenario

### Scenario

A company has:

```text
Azure VNet:

10.0.0.0/16
```

A Linux VM is:

```text
10.0.1.4
```

Employee is working from home.

The employee needs to access the VM without exposing the VM's private service directly to the public Internet.

Solution:

```text
Employee Laptop
      |
      | P2S VPN
      |
Internet
      |
Azure VPN Gateway
      |
Azure VNet
      |
Linux VM
10.0.1.4
```

The employee connects through P2S VPN and accesses:

```text
10.0.1.4
```

without requiring the VM itself to have a public IP.

---

# 32. Important Interview Questions

### Q1. What is Azure P2S VPN?

P2S VPN provides secure connectivity from an individual client device to an Azure VNet over an encrypted VPN tunnel.

### Q2. Does P2S require an on-premises VPN device?

No.

### Q3. What is GatewaySubnet?

A dedicated subnet used by the Azure VPN/ExpressRoute gateway.

### Q4. What is the P2S address pool?

It is the private IP address range from which connected VPN clients receive their VPN IP addresses.

### Q5. Can P2S use Microsoft Entra ID?

Yes, Microsoft Entra ID authentication is supported for appropriate P2S configurations.

### Q6. What is the difference between P2S and S2S?

P2S connects an individual client to Azure, while S2S connects an entire network/site to Azure.

### Q7. Does P2S require a public IP?

The Azure VPN Gateway requires a public IP for Internet-based client connectivity.

### Q8. Can P2S clients access Azure VMs?

Yes, provided routing, NSGs, firewalls, and application access rules permit the traffic.

### Q9. What protocol does ping use?

ICMP.

### Q10. VPN is connected but ping is failing. What will you check?

Check:

- Route
- NSG
- VM firewall
- ICMP rules
- Correct private IP
- VPN client configuration
- Network configuration

---

# 33. Complete P2S Lab Flow

```text
Step 1
Create Resource Group
        ↓
Step 2
Create VNet
        ↓
Step 3
Create Application Subnet
        ↓
Step 4
Create GatewaySubnet
        ↓
Step 5
Create Public IP
        ↓
Step 6
Create Virtual Network Gateway
        ↓
Step 7
Configure Point-to-Site
        ↓
Step 8
Configure Address Pool
        ↓
Step 9
Configure Authentication
        ↓
Step 10
Configure VPN Protocol
        ↓
Step 11
Download VPN Client
        ↓
Step 12
Install/Configure VPN Client
        ↓
Step 13
Connect VPN
        ↓
Step 14
Verify Client IP
        ↓
Step 15
Test Azure Private IP
        ↓
Step 16
Troubleshoot NSG/Firewall if required
```

---

# 34. Quick Revision

```text
P2S
=
Individual Client → Azure VNet

Required:

VNet
+
GatewaySubnet
+
VPN Gateway
+
Public IP
+
P2S Address Pool
+
Authentication
+
VPN Client
```

### Authentication

```text
Certificate
Microsoft Entra ID
RADIUS
```

### Common VPN protocols

```text
IKEv2
OpenVPN
SSTP
```

### Most important point

```text
P2S = Remote User Access

S2S = Site/Network Access

VNet Peering = VNet-to-VNet Connectivity
```

# 35. Interview-Level Architecture

```text
                         INTERNET
                            |
                            |
                    +---------------+
                    | Remote Client |
                    |    Laptop     |
                    +---------------+
                            |
                    Encrypted VPN
                            |
                            v
                 +----------------------+
                 | Azure VPN Gateway    |
                 |                      |
                 | P2S Configuration    |
                 +----------------------+
                            |
                     GatewaySubnet
                            |
                            v
                 +----------------------+
                 |      Azure VNet      |
                 |      10.0.0.0/16     |
                 |                      |
                 |  +---------------+   |
                 |  | Linux VM      |   |
                 |  | 10.0.1.4      |   |
                 |  +---------------+   |
                 +----------------------+
```

**Key takeaway:** Azure P2S VPN is primarily designed for **secure remote-user access to Azure private resources**, without requiring a site-to-site VPN device at the user's location.