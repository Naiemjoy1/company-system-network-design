# Company System Network Design

Enterprise network design and implementation for a **Trading Floor Support Centre** supporting approximately **600 users** using **Cisco Packet Tracer**.

The project follows Cisco's **Hierarchical Network Model** and implements redundancy, security, dynamic routing, wireless connectivity, centralized DHCP services, and dual ISP connectivity.

---

# Project Overview

The organization is relocating to a new three-floor building that currently has no existing network infrastructure.

This project presents a scalable, secure, and fault-tolerant network solution designed to satisfy current business requirements while supporting future growth.

### Implemented Features

- Hierarchical Network Architecture
- Redundant Core Layer
- Dual ISP Connectivity
- OSPF Routing
- Inter-VLAN Routing
- DHCP Relay
- Centralized DHCP Server
- SSH Remote Access
- NAT Overload (PAT)
- Port Security
- Wireless Access Points
- VLAN Segmentation
- Access Control Lists
- Server Infrastructure

---

# Network Topology

![Topology](images/topology.png)

---

# Building Layout

| Floor        | Department                        | Users | VLAN    | Network         |
| ------------ | --------------------------------- | ----- | ------- | --------------- |
| First Floor  | Sales & Marketing                 | 120   | VLAN 10 | 172.16.1.0/25   |
| First Floor  | HR & Logistics                    | 120   | VLAN 20 | 172.16.1.128/25 |
| Second Floor | Finance & Accounts                | 120   | VLAN 30 | 172.16.2.0/25   |
| Second Floor | Administration & Public Relations | 120   | VLAN 40 | 172.16.2.128/25 |
| Third Floor  | ICT                               | 120   | VLAN 50 | 172.16.3.0/25   |
| Third Floor  | Server Room                       | 12    | VLAN 60 | 172.16.3.128/28 |

---

# Subnetting Design

The company provided the base network **172.16.1.0**.

Subnetting was carried out using **VLSM** to accommodate all departments while minimizing address wastage.

### Host Requirements

| Department          | Hosts Needed | Prefix | Usable Hosts |
| ------------------- | ------------ | ------ | ------------ |
| Sales & Marketing   | 120          | /25    | 126          |
| HR & Logistics      | 120          | /25    | 126          |
| Finance & Accounts  | 120          | /25    | 126          |
| Administration & PR | 120          | /25    | 126          |
| ICT                 | 120          | /25    | 126          |
| Server Room         | 12           | /28    | 14           |

---

## Department Subnets

| VLAN | Department          | Network         | Mask            | Gateway      | Broadcast    |
| ---- | ------------------- | --------------- | --------------- | ------------ | ------------ |
| 10   | Sales & Marketing   | 172.16.1.0/25   | 255.255.255.128 | 172.16.1.1   | 172.16.1.127 |
| 20   | HR & Logistics      | 172.16.1.128/25 | 255.255.255.128 | 172.16.1.129 | 172.16.1.255 |
| 30   | Finance & Accounts  | 172.16.2.0/25   | 255.255.255.128 | 172.16.2.1   | 172.16.2.127 |
| 40   | Administration & PR | 172.16.2.128/25 | 255.255.255.128 | 172.16.2.129 | 172.16.2.255 |
| 50   | ICT                 | 172.16.3.0/25   | 255.255.255.128 | 172.16.3.1   | 172.16.3.127 |
| 60   | Server Room         | 172.16.3.128/28 | 255.255.255.240 | 172.16.3.129 | 172.16.3.143 |

---

## Point-to-Point Networks

| Connection        | Network         |
| ----------------- | --------------- |
| MLT-SW1 ↔ CORE-R1 | 172.16.3.144/30 |
| MLT-SW1 ↔ CORE-R2 | 172.16.3.148/30 |
| MLT-SW2 ↔ CORE-R1 | 172.16.3.152/30 |
| MLT-SW2 ↔ CORE-R2 | 172.16.3.156/30 |

---

## ISP Networks

| Link           | Network          |
| -------------- | ---------------- |
| ISP1 ↔ CORE-R1 | 195.136.17.0/30  |
| ISP2 ↔ CORE-R1 | 195.136.17.4/30  |
| ISP1 ↔ CORE-R2 | 195.136.17.8/30  |
| ISP2 ↔ CORE-R2 | 195.136.17.12/30 |

---

# Hierarchical Design

## Core Layer

### Devices

- CORE-R1
- CORE-R2

### Responsibilities

- OSPF Routing
- PAT/NAT
- Internet Connectivity
- Redundancy
- Default Route Management

---

## Distribution Layer

### Devices

- MLT-SW1
- MLT-SW2

### Responsibilities

- Inter-VLAN Routing
- OSPF
- DHCP Relay
- Redundant Connectivity

---

## Access Layer

### Devices

- SALE-SW
- HR-SW
- FINANCE-SW
- ADMIN-SW
- ICT-SW
- SERVERROOM-SW

### Responsibilities

- End Device Access
- Wireless Connectivity
- VLAN Assignment
- Port Security

---

# Technologies Implemented

- Cisco Packet Tracer
- VLAN Segmentation
- OSPF Routing
- DHCP Services
- DHCP Relay
- SSH
- NAT Overload
- PAT
- ACLs
- Wireless LAN
- Port Security
- Inter-VLAN Routing
- Hierarchical Design
- Redundant ISP Connectivity

---

# Security Features

## SSH

Implemented on:

- CORE-R1
- CORE-R2
- MLT-SW1
- MLT-SW2

Features:

- RSA Key Generation
- Local Authentication
- SSH Version 2

---

## Port Security

Configured in the Finance Department.

Features:

- Sticky MAC Learning
- Maximum One Device
- Violation Mode Shutdown

---

## Black Hole VLAN

Unused interfaces assigned to:

```text
VLAN 99
```

Unused ports are administratively disabled.

---

# OSPF Routing

Dynamic routing protocol used:

```cisco
router ospf 10
```

Single Area Design:

```cisco
Area 0
```

Advertised Networks:

- VLANs
- Router Links
- ISP Links

---

# DHCP

Dedicated DHCP server deployed in:

**VLAN 60 – Server Room**

DHCP relay configured using:

```cisco
ip helper-address 172.16.3.130
```

---

# NAT Configuration

PAT configured on both Core Routers.

Example:

```cisco
ip nat inside source list 1 interface Serial0/2/0 overload
```

ACL Used:

```cisco
access-list 1 permit 172.16.1.0 0.0.0.127
access-list 1 permit 172.16.1.128 0.0.0.127
access-list 1 permit 172.16.2.0 0.0.0.127
access-list 1 permit 172.16.2.128 0.0.0.127
access-list 1 permit 172.16.3.0 0.0.0.127
access-list 1 permit 172.16.3.128 0.0.0.15
```

---

# Wireless Network

Each department contains:

- Access Point
- Laptop
- Tablet Device

Wireless clients receive IP addresses dynamically from the centralized DHCP server.

---

# Verification and Testing

## OSPF Neighbor Verification

Demonstrates successful OSPF adjacency establishment.

![OSPF Neighbor](images/ospf-neighbor.png)

---

## DHCP Server Configuration

Dedicated DHCP services for all VLANs.

![DHCP Server](images/dhcp-server.png)

---

## Dynamic IP Assignment

Clients successfully obtaining IP addresses dynamically.

![DHCP Client](images/ip-from-dhcp.png)

---

## DNS Server Verification

DNS services configured in Server VLAN.

![DNS Server](images/dns-server.png)

---

## SSH Access

Secure remote administration using SSH.

![SSH Access](images/ssh-access.png)

---

## NAT Translation Verification

PAT translations generated successfully.

![NAT Translation](images/nat-translation.png)

---

## Port Security Verification

Finance Department sticky MAC configuration.

![Port Security](images/sticky-check.png)

---

## End-to-End Connectivity Test

Inter-VLAN communication verified successfully.

![Ping Test](images/ping-test.png)

---

# Validation Checklist

✔ VLAN Segmentation

✔ Inter-VLAN Routing

✔ DHCP Address Allocation

✔ OSPF Neighbor Formation

✔ SSH Connectivity

✔ PAT Translation

✔ DNS Resolution

✔ Wireless Connectivity

✔ Port Security

✔ End-to-End Communication

✔ Redundant ISP Connectivity

---

# Repository Structure

```text
company-system-network-design
│
├── README.md
├── company-system-network-design.pkt
│
├── configs
│   ├── ADMIN-SW.txt
│   ├── company-system-network-design.txt
│   ├── CORE-R1.txt
│   ├── CORE-R2.txt
│   ├── FINANCE-SW.txt
│   ├── HR-SW.txt
│   ├── ICT-HR.txt
│   ├── ISP1.txt
│   ├── ISP2.txt
│   ├── MLT-SW1.txt
│   ├── MLT-SW2.txt
│   ├── SALE-SW.txt
│   └── SERVERROOM-SW.txt
│
├── images
│   ├── topology.png
│   ├── dhcp-server.png
│   ├── dns-server.png
│   ├── ip-from-dhcp.png
│   ├── nat-translation.png
│   ├── ospf-neighbor.png
│   ├── ping-test.png
│   ├── ssh-access.png
│   └── sticky-check.png
│
└── docs
```

---

# Software Used

- Cisco Packet Tracer 8.x
- Cisco IOS
- GitHub

---

# Author

**Naiem Hasan**

Network Engineering Student

Enterprise Network Design using Cisco Packet Tracer

---
