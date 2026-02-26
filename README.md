# 🏢 Campus Network Design — Four-Story Building

> A structured hierarchical campus network simulated in **Cisco Packet Tracer**, supporting 100 end-user devices, 20 IP phones, and 4 servers across four floors.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Network Topology](#network-topology)
- [Architecture](#architecture)
- [VLAN Design](#vlan-design)
- [IP Addressing](#ip-addressing)
- [Server Infrastructure](#server-infrastructure)
- [Key Configurations](#key-configurations)
- [Technologies Used](#technologies-used)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)

---

## Overview

This project implements a **hierarchical campus network** for a four-story building environment using Cisco Packet Tracer. The design prioritizes:

- ✅ Scalability
- ✅ Redundancy and fault tolerance
- ✅ Performance efficiency
- ✅ Structured traffic management (data + voice)

---

## Network Topology

![Campus Network Topology](topology.png)

The topology features:
- 1 Router (Core) with Router-on-a-Stick inter-VLAN routing
- 2 Distribution Switches (redundant backbone)
- 4 Access Switches (one per floor)
- 1 Dedicated IP Phone Switch
- 4 Servers (DNS, Web, FTP, Mail)

---

## Architecture

The network follows the **Hierarchical Campus Design Model**:

### 🔵 Core Layer
- Single router handling inter-VLAN routing, DHCP, and IP telephony services.

### 🟡 Distribution Layer
- Two distribution switches providing redundant backbone connectivity, VLAN trunk aggregation, and traffic distribution to access switches.

### 🟢 Access Layer
- Four access switches (one per floor), each supporting ~25 end-user PCs with floor-level segmentation.

### 📞 Voice Aggregation Layer
- A dedicated IP Phone switch physically isolates voice traffic from data traffic, simplifying troubleshooting and reducing configuration complexity.

---

## VLAN Design

| VLAN ID | Purpose        | Subnet               |
|---------|----------------|----------------------|
| 10      | Floor 1 Data   | 192.168.10.0/24      |
| 20      | Floor 2 Data   | 192.168.20.0/24      |
| 30      | Floor 3 Data   | 192.168.30.0/24      |
| 40      | Floor 4 Data   | 192.168.40.0/24      |
| 99      | Server Network | 192.168.99.0/24      |

---

## IP Addressing

Each VLAN uses a `/24` subnet. The router provides the default gateway via subinterfaces:

| VLAN | Gateway        |
|------|----------------|
| 10   | 192.168.10.1   |
| 20   | 192.168.20.1   |
| 30   | 192.168.30.1   |
| 40   | 192.168.40.1   |
| 99   | 192.168.99.1   |

---

## Server Infrastructure

All servers reside on **VLAN 99 (192.168.99.0/24)**:

| Server   | Service              | IP Address       |
|----------|----------------------|------------------|
| DNS      | Domain Name System   | 192.168.99.x     |
| Web      | HTTP/HTTPS           | 192.168.99.x     |
| FTP      | File Transfer        | 192.168.99.x     |
| Mail     | SMTP/POP3/IMAP       | 192.168.99.x     |

> Update IP addresses above with your actual server assignments.

---

## Key Configurations

### Router-on-a-Stick (Inter-VLAN Routing)

```cisco
interface g0/0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
```

### DHCP

```cisco
ip dhcp pool FLOOR1
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 dns-server 192.168.99.x
```

### IP Telephony

```cisco
telephony-service
 max-ephones 20
 max-dn 20
 ip source-address 10.0.0.1 port 2000
 auto assign 1 to 20
```

### Trunk Links

```cisco
interface g0/1
 switchport mode trunk
```

### Access Ports

```cisco
interface range fa0/1 - 24
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast
```

### Spanning Tree Protocol (STP)

**Distribution Switch 1 — Root for VLANs 10, 30, 99:**
```cisco
spanning-tree vlan 10,30,99 priority 4096
spanning-tree vlan 20,40 priority 8192
```

**Distribution Switch 2 — Root for VLANs 20, 40:**
```cisco
spanning-tree vlan 20,40 priority 4096
spanning-tree vlan 10,30,99 priority 8192
```

---

## Technologies Used

| Technology        | Purpose                          |
|-------------------|----------------------------------|
| Cisco Packet Tracer | Network simulation             |
| 802.1Q Trunking   | VLAN tagging across uplinks      |
| DHCP              | Dynamic IP assignment per VLAN   |
| VoIP / ePhone     | IP telephony services            |
| STP               | Loop prevention & redundancy     |
| DNS               | Domain name resolution           |
| HTTP/HTTPS        | Web services                     |
| FTP               | File transfer services           |
| SMTP/POP3         | Email services                   |

---

## Project Structure

```
campus-network/
├── README.md
├── topology.png
└── campus_network.pkt       # Cisco Packet Tracer file
```

---

## Getting Started

1. Install [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer)
2. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/campus-network.git
   ```
3. Open `campus_network.pkt` in Cisco Packet Tracer
4. Explore the topology and verify connectivity using `ping` between devices on different VLANs

---

## 📄 License

This project is for educational purposes.
