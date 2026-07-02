# company-system-network-design

Enterprise network design and implementation for a **Trading Floor Support Centre** supporting approximately **600 users** using **Cisco Packet Tracer**.

The project follows Cisco's **Hierarchical Network Model** and implements redundancy, security, dynamic routing, wireless connectivity, centralized DHCP, and dual ISP connectivity.

---

## Project Overview

The company is relocating to a new three-floor building that currently has no network infrastructure. This project provides a scalable, resilient, and future-proof network design capable of supporting current business requirements and future expansion.

The design includes:

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
- Access Control Lists (ACL)
- Server Room Infrastructure

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

# Hierarchical Design

## Core Layer

Devices:

- CORE-R1
- CORE-R2

Responsibilities:

- Internet Connectivity
- OSPF Routing
- NAT/PAT
- Redundancy
- Default Route Management

---

## Distribution Layer

Devices:

- MLT-SW1
- MLT-SW2

Responsibilities:

- Inter-VLAN Routing
- DHCP Relay
- OSPF
- Redundant Links

---

## Access Layer

Devices:

- SALE-SW
- HR-SW
- FINANCE-SW
- ADMIN-SW
- ICT-SW
- SERVERROOM-SW

Responsibilities:

- End Device Connectivity
- Wireless Connectivity
- Port Security
- VLAN Assignment

---

# IP Addressing Scheme

## Department Networks

| VLAN | Department         | Subnet          | Gateway      |
| ---- | ------------------ | --------------- | ------------ |
| 10   | Sales & Marketing  | 172.16.1.0/25   | 172.16.1.1   |
| 20   | HR & Logistics     | 172.16.1.128/25 | 172.16.1.129 |
| 30   | Finance & Accounts | 172.16.2.0/25   | 172.16.2.1   |
| 40   | Admin & PR         | 172.16.2.128/25 | 172.16.2.129 |
| 50   | ICT                | 172.16.3.0/25   | 172.16.3.1   |
| 60   | Server Room        | 172.16.3.128/28 | 172.16.3.129 |

---

## Router Links

| Connection        | Network         |
| ----------------- | --------------- |
| MLT-SW1 ↔ CORE-R1 | 172.16.3.144/30 |
| MLT-SW1 ↔ CORE-R2 | 172.16.3.148/30 |
| MLT-SW2 ↔ CORE-R1 | 172.16.3.152/30 |
| MLT-SW2 ↔ CORE-R2 | 172.16.3.156/30 |

---

## ISP Networks

| Link           | Subnet           |
| -------------- | ---------------- |
| ISP1 ↔ CORE-R1 | 195.136.17.0/30  |
| ISP2 ↔ CORE-R1 | 195.136.17.4/30  |
| ISP1 ↔ CORE-R2 | 195.136.17.8/30  |
| ISP2 ↔ CORE-R2 | 195.136.17.12/30 |

---

# Technologies Implemented

- Cisco Packet Tracer
- Hierarchical Network Design
- VLAN Segmentation
- Inter-VLAN Routing
- OSPF Routing
- DHCP Server
- DHCP Relay
- SSH
- NAT Overload (PAT)
- ACLs
- Port Security
- Wireless Access Points
- Redundant ISP Connections
- Dynamic IP Allocation
- Static Server Addressing

---

# Security Features

### SSH

Implemented on:

- Core Routers
- Layer 3 Switches

Features:

- RSA Key Generation
- Local User Authentication
- SSH Version 2

---

### Port Security

Configured on the Finance Department switch.

Security settings:

- Maximum MAC Address = 1
- Sticky MAC Learning
- Violation Mode = Shutdown

---

### Black Hole VLAN

Unused ports assigned to:

```text
VLAN 99
```

Unused interfaces are administratively disabled.

---

# Routing

Dynamic routing protocol used:

### OSPF

```cisco
router ospf 10
```

Single area implementation:

```cisco
Area 0
```

Advertised Networks:

- VLAN Networks
- Point-to-Point Links
- ISP Connections

---

# DHCP

Dedicated DHCP Server deployed inside:

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

ACL:

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

Each department includes:

- Access Point
- Laptop
- Tablet Device

Wireless clients obtain IP addresses dynamically from the centralized DHCP server.

---

# Verification and Testing

````md
# Screenshots

## Topology

![Topology](images/topology.png)

## OSPF Neighbor Status

![OSPF](images/ospf-neighbor.png)

## DHCP Server

![DHCP Server](images/dhcp-server.png)

## Dynamic IP Allocation

![DHCP Client](images/ip-from-dhcp.png)

## DNS Server

![DNS Server](images/dns-server.png)

## SSH Access

![SSH](images/ssh-access.png)

## NAT Translation Table

![NAT](images/nat-translation.png)

## Port Security

![Port Security](images/sticky-check.png)

## Connectivity Testing

![Ping Test](images/ping-test.png)

---

# Validation Performed

✔ DHCP Address Assignment

✔ Inter-VLAN Communication

✔ OSPF Adjacencies

✔ SSH Access

✔ NAT Translation

✔ PAT Verification

✔ Wireless Connectivity

✔ Port Security

✔ End-to-End Ping

✔ Redundant ISP Connectivity

---

# Project Structure

```text
Hotel-Network-Design
│
├── README.md
├── Hotel-Network.pkt
│
├── images
│   ├── topology.png
│   ├── ospf.png
│   ├── sticky.png
│   └── internal-ping.png
│
├── configs
│   ├── CORE-R1.txt
│   ├── CORE-R2.txt
│   ├── MLT-SW1.txt
│   ├── MLT-SW2.txt
│   └── Access-Switches.txt
│
└── docs
    └── subnetting.xlsx
```
````

---

# Software Used

- Cisco Packet Tracer 8.x
- Cisco IOS
- GitHub

---

# Author

**Naiem Hasan**

Network Engineering Student

Cisco Packet Tracer Enterprise Network Design Project

---

## Screenshots

### Network Topology

```markdown
![Topology](images/topology.png)
```

### OSPF Verification

```markdown
![OSPF](images/ospf.png)
```

### Port Security

```markdown
![Port Security](images/sticky.png)
```

### End-to-End Ping

```markdown
![Ping](images/internal-ping.png)
```
