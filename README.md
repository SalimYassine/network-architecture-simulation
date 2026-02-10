# Virtualized Network Interconnection Infrastructure (Docker/FRR) 

> **Note:** This repository serves as a technical showcase and documentation for a network engineering project. The source code (docker-compose) is archived, but the architecture and technical implementation details are fully documented below.

## Overview
Design and simulation of a scalable ISP and Enterprise network interconnection. The project leverages **Docker** containers to simulate routers and endpoints, using **FRRouting (FRR)** for dynamic routing (OSPF) and Linux-based firewalls.

**Context:** Engineering Project at ENSEEIHT - Telecommunications & Networks.

## Network Architecture
The infrastructure is segmented into three isolated zones:
1.  **OSPF Backbone:** Core network using Border and Internal routers running OSPFv2.
2.  **Enterprise LAN:** A secure zone behind a Debian-based Firewall/NAT, hosting internal Web and DNS services.
3.  **Home LAN:** A domestic network simulation with dynamic addressing (DHCP) for end-users.

## Technical Implementation

### 1. Virtualization Strategy
Instead of heavy VMs, we used **Docker** containers sharing the host kernel to simulate complex topology with low overhead.
* **Routers:** Alpine Linux images with FRRouting enabled.
* **End-Devices:** Busybox/Debian containers for testing connectivity.

### 2. NAT & Security Configuration
The Enterprise edge router acts as a firewall using `iptables`.
**DNAT (Port Forwarding):** HTTP traffic entering the public IP is redirected to the internal web server (192.168.197.131).
```bash
# Redirection of incoming HTTP traffic to internal server
iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.197.131:80
# Outgoing traffic masquerading
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```
### 3. Dynamic Routing (OSPF)
The backbone uses FRRouting to exchange routes dynamically. Routers are configured via vtysh.

## Validation Tests
The infrastructure was validated through several connectivity tests:
**DNS Resolution**: Split-horizon DNS successfully resolving www.entreprise.com to the Box's public IP.
**HTTP Connectivity**: Successful retrieval of the web page from the Home LAN through the NAT.

## Authors
**Yassine Salim**, Aloïs Meurisse, Cédric Gironcel, Aurélien Rakotoarison, Romain Costi, Yahya Mazouari, Mathéo Lin-Teng-Shee.
