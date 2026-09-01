# Secure Multi-Branch Corporate Network

## TechNova Solutions

A secure and scalable corporate network designed and implemented in **Cisco Packet Tracer** for a company with a Head Office and Branch Office.

The project addresses common enterprise networking problems such as flat network architecture, excessive broadcast traffic, unrestricted inter-department communication, and lack of centralized network access control.

---

## Problem Statement

TechNova Solutions has two offices:

* Head Office
* Branch Office

Each office contains multiple departments:

* HR
* Finance
* IT
* Guest Wi-Fi

The original flat network allowed devices from different departments to communicate freely. This created security and performance risks.

### Major Problems

* No network segmentation
* Guest users could potentially access internal resources
* Excessive broadcast traffic
* No department-level access control
* No secure network-device management
* Difficult to scale as the company grows

---

# Project Objectives

The main objectives of this project are:

1. Segment departments using VLANs.
2. Implement subnetting for efficient IP address allocation.
3. Enable communication between authorized VLANs using inter-VLAN routing.
4. Connect Head Office and Branch Office using a WAN.
5. Implement OSPF dynamic routing.
6. Provide automatic IP addressing using DHCP.
7. Restrict unauthorized traffic using Extended ACLs.
8. Secure network-device management using SSH.
9. Prevent unauthorized device connections using port security.
10. Verify the complete network using Cisco Packet Tracer simulation and CLI commands.

---

# Network Architecture

```text
                         TECHNOVA SOLUTIONS
                                  |
                    +-------------+-------------+
                    |                           |
               HEAD OFFICE                 BRANCH OFFICE
                    |                           |
                 HO-L3                       BR-L3
                    |                           |
                 HO-R1                       BR-R1
                    |                           |
                    +-------- WAN / OSPF -------+
```

Each branch contains four logical VLANs:

```text
VLAN 10 → HR
VLAN 20 → Finance
VLAN 30 → IT
VLAN 40 → Guest
```

---

# Technologies Used

| Technology          | Purpose                    |
| ------------------- | -------------------------- |
| Cisco Packet Tracer | Network simulation         |
| VLAN                | Department segmentation    |
| 802.1Q Trunking     | VLAN transport             |
| Layer-3 Switching   | Inter-VLAN routing         |
| IPv4 Subnetting     | Network addressing         |
| DHCP                | Automatic IP configuration |
| OSPF                | Dynamic routing            |
| WAN                 | Branch connectivity        |
| Extended ACL        | Traffic filtering          |
| SSH                 | Secure device management   |
| Port Security       | Access-port protection     |
| HTTP/FTP            | Internal server testing    |

---

# IP Addressing Plan

## Head Office

| VLAN | Department | Network         | Gateway      |
| ---: | ---------- | --------------- | ------------ |
|   10 | HR         | 192.168.0.0/27  | 192.168.0.1  |
|   20 | Finance    | 192.168.0.32/27 | 192.168.0.33 |
|   30 | IT         | 192.168.0.64/27 | 192.168.0.65 |
|   40 | Guest      | 192.168.0.96/27 | 192.168.0.97 |

## Branch Office

| VLAN | Department | Network         | Gateway      |
| ---: | ---------- | --------------- | ------------ |
|   10 | HR         | 192.168.1.0/27  | 192.168.1.1  |
|   20 | Finance    | 192.168.1.32/27 | 192.168.1.33 |
|   30 | IT         | 192.168.1.64/27 | 192.168.1.65 |
|   40 | Guest      | 192.168.1.96/27 | 192.168.1.97 |

## WAN

```text
Network: 10.0.0.0/30

HO-R1 → 10.0.0.1
BR-R1 → 10.0.0.2
```

---

# VLAN Design

### VLAN 10 — HR

Used for HR department devices.

```text
Network: 192.168.x.0/27
```

### VLAN 20 — Finance

Used for Finance department devices.

```text
Network: 192.168.x.32/27
```

### VLAN 30 — IT

Used for IT users and internal infrastructure.

```text
Network: 192.168.x.64/27
```

### VLAN 40 — Guest

Used for guest and visitor devices.

```text
Network: 192.168.x.96/27
```

The Guest VLAN is isolated from internal corporate networks using ACL policies.

---

# Routing Architecture

Inter-VLAN routing is implemented using **Cisco 3560 multilayer switches**.

The Layer-3 switches provide gateway interfaces for each VLAN.

Example:

```text
VLAN 10 → 192.168.0.1
VLAN 20 → 192.168.0.33
VLAN 30 → 192.168.0.65
VLAN 40 → 192.168.0.97
```

The two branch routers are connected through a WAN serial link.

**OSPF Area 0** is used for dynamic routing between the branch routers and Layer-3 switches.

---

# Security Design

Security is one of the primary goals of this project.

## Extended ACLs

ACL policies are designed to:

* Prevent Guest users from accessing corporate VLANs.
* Restrict Finance resources to authorized departments.
* Protect internal IT resources.
* Control communication between departments.
* Allow authorized traffic while denying unauthorized traffic.

Example security model:

```text
Guest → HR        DENY
Guest → Finance   DENY
Guest → IT        DENY
Guest → Server    DENY

HR → Finance      ALLOW
IT → Finance      ALLOW
IT → Server       ALLOW
```

---

# SSH Security

Network-device management is secured using SSH instead of Telnet.

SSH provides encrypted remote management between administrators and network devices.

The configuration includes:

```text
Local username
Privilege level
RSA keys
SSH version 2
VTY authentication
SSH-only access
```

---

# Port Security

Port security is enabled on access ports to restrict unauthorized devices.

Configuration includes:

```text
Maximum MAC addresses
Sticky MAC addresses
Violation restriction
Access-mode ports
```

This helps prevent unauthorized devices from being connected to corporate access ports.

---

# DHCP

DHCP is configured separately for each VLAN.

Example:

```text
HO-HR
HO-FINANCE
HO-IT
HO-GUEST

BR-HR
BR-FINANCE
BR-IT
BR-GUEST
```

DHCP automatically provides:

* IP address
* Subnet mask
* Default gateway
* DNS server

---

# Internal IT Server

An internal server is placed inside the IT VLAN.

Example:

```text
IP Address: 192.168.0.71
Subnet Mask: 255.255.255.224
Gateway: 192.168.0.65
```

HTTP/FTP services can be enabled to demonstrate access-control policies.

---

# Network Verification

The following Cisco IOS commands are used to verify the configuration:

### VLAN Verification

```cisco
show vlan brief
```

### Interface Verification

```cisco
show ip interface brief
```

### Routing Verification

```cisco
show ip route
```

### OSPF Verification

```cisco
show ip ospf neighbor
```

### ACL Verification

```cisco
show access-lists
```

### DHCP Verification

```cisco
show ip dhcp binding
show ip dhcp pool
```

### Trunk Verification

```cisco
show interfaces trunk
```

### Port Security

```cisco
show port-security
show port-security interface fa0/1
```

### SSH

```cisco
show ip ssh
```

---

# Testing Scenarios

The following tests are performed in Cisco Packet Tracer.

| Test                                | Expected Result |
| ----------------------------------- | --------------- |
| HR → Finance                        | Allowed       |
| IT → Finance                        | Allowed       |
| IT → Internal Server                | Allowed       |
| Guest → HR                          | Blocked       |
| Guest → Finance                     | Blocked       |
| Guest → IT                          | Blocked       |
| Guest → Internal Server             | Blocked       |
| Head Office → Branch Office         | Allowed       |
| Branch Office → Head Office         | Allowed       |
| SSH administration                  | Allowed       |
| Unauthorized device on secured port | Restricted    |

---

# Before vs After

## Before

```text
                    FLAT NETWORK
                         |
        +----------------+----------------+
        |        |       |       |        |
       HR     Finance    IT    Guest    Server

        All devices share one network
```

### Problems

No segmentation
Poor security
Large broadcast domain
Guest can access internal resources
No department-level control

---

## After

```text
                    SECURE NETWORK
                         |
             +-----------+-----------+
             |                       |
        HEAD OFFICE             BRANCH OFFICE
             |                       |
       +-----+-----+           +-----+-----+
       |     |     |           |     |     |
      HR    FIN   IT Guest    HR    FIN   IT Guest
       |     |     |     |     |     |     |     |
       +-----+-----+     |     +-----+-----+     |
             |           |           |           |
            ACL        ISOLATED     ACL        ISOLATED
             |                       |
             +------ OSPF / WAN -----+
```

### Improvements

VLAN segmentation
Reduced broadcast domains
Controlled inter-VLAN communication
Guest isolation
Dynamic routing
Secure SSH management
Port security
Automatic DHCP addressing
Scalable architecture

---

# Project Structure

Recommended GitHub repository structure:

```text
secure-multi-branch-corporate-network/
│
├── README.md
│
├── packet-tracer/
│   └── TechNova_Secure_MultiBranch_Network.pkt
│
├── documentation/
│   ├── network-topology.png
│   ├── ip-addressing-table.png
│   ├── vlan-configuration.png
│   ├── ospf-verification.png
│   ├── acl-testing.png
│   ├── ssh-verification.png
│   └── port-security.png
│
├── configurations/
│   ├── HO-R1.txt
│   ├── HO-L3.txt
│   ├── HO-SW1.txt
│   ├── HO-SW2.txt
│   ├── BR-R1.txt
│   ├── BR-L3.txt
│   ├── BR-SW1.txt
│   └── BR-SW2.txt
│
└── screenshots/
    ├── topology.png
    ├── vlan-test.png
    ├── ospf-test.png
    ├── dhcp-test.png
    ├── acl-block.png
    └── ssh-test.png
```

---

# Recommended Screenshots

Add screenshots showing:

### 1. Complete topology

Your complete Packet Tracer network.

### 2. VLAN configuration

```cisco
show vlan brief
```

### 3. IP interfaces

```cisco
show ip interface brief
```

### 4. OSPF

```cisco
show ip ospf neighbor
```

### 5. Routing table

```cisco
show ip route
```

### 6. ACL

```cisco
show access-lists
```

### 7. Successful communication

Example:

```text
IT PC → Internal Server
```

### 8. Blocked communication

Example:

```text
Guest PC → Finance PC
```

Capture this using Packet Tracer **Simulation Mode**.

### 9. SSH

Show a successful SSH connection from an authorized IT PC.

### 10. Port Security

Show:

```cisco
show port-security interface fa0/1
```

---

# 💼 Resume Description

**Secure Multi-Branch Corporate Network | Cisco Packet Tracer**

* Designed and simulated a secure two-branch enterprise network using Cisco Packet Tracer with VLAN segmentation for HR, Finance, IT, and Guest departments.
* Implemented IPv4 subnetting, Layer-3 inter-VLAN routing, DHCP, WAN connectivity, and OSPF dynamic routing.
* Configured Extended ACLs to isolate Guest traffic and restrict access to sensitive Finance and IT resources.
* Secured network-device management using SSH and protected access ports using MAC-based port security.
* Verified routing, VLAN segmentation, DHCP, ACL enforcement, OSPF adjacency, and inter-branch connectivity using Cisco IOS commands and Packet Tracer Simulation Mode.

---

# 🎤 Interview Explanation

### "Tell me about your project."

> I designed and simulated a secure multi-branch corporate network for a fictional company called TechNova Solutions using Cisco Packet Tracer. The company had a flat network where all departments could communicate with each other, creating security and scalability issues.
>
> I redesigned the network using VLANs for HR, Finance, IT, and Guest users. I used /27 subnetting to provide separate networks for each department and implemented inter-VLAN routing using Cisco 3560 multilayer switches.
>
> I connected the Head Office and Branch Office using a WAN link and configured OSPF for dynamic routing. DHCP was implemented for automatic IP address assignment.
>
> For security, I used Extended ACLs to isolate the Guest VLAN and control access to Finance and IT resources. I also configured SSH for secure network-device management and port security on access ports.
>
> Finally, I verified the design using ping tests, routing tables, OSPF neighbor information, ACL counters, DHCP bindings, and Packet Tracer Simulation Mode.

---

# ⭐ Key Skills Demonstrated

```text
Networking
├── VLAN
├── Trunking
├── Inter-VLAN Routing
├── IPv4
├── Subnetting
├── DHCP
├── WAN
└── OSPF

Security
├── Extended ACL
├── SSH
├── Port Security
└── Network Segmentation

Tools
└── Cisco Packet Tracer
```

---

# 📌 Project Status

```text
Architecture       ✅
VLANs              ✅
Subnetting         ✅
Inter-VLAN Routing ✅
WAN                ✅
OSPF               ✅
DHCP               ✅
ACL Security       🔄
SSH                🔄
Port Security      🔄
Server             🔄
Testing             🔄
Documentation      🔄
```

> **Note:** Mark the remaining items as complete in GitHub only after you actually configure and test them in Packet Tracer.
