# BISB Enterprise Network

# Project Overview

This project presents the design and implementation of a secure, scalable, resilient, and highly available enterprise network for Bahrain Islamic Bank (BISB).

The network connects the BISB Head Office in Manama with branches in Riyadh and Kuwait through a simulated Batelco WAN.

The project focuses on enterprise network design, IP addressing, VLAN segmentation, dynamic routing, WAN connectivity, network security, wireless networking, redundancy, high availability, and network management.

The complete network topology and device configurations were implemented using Cisco Packet Tracer.

# Network Architecture

The enterprise network consists of three main locations:

- Manama Head Office
- Riyadh Branch
- Kuwait Branch

The sites are interconnected through a simulated Batelco WAN using point-to-point serial connections.

The Manama Head Office also provides Internet connectivity and contains the main web and email server networks.

The network architecture was designed to provide:

- Secure communication between sites
- Department-based network segmentation
- Dynamic routing
- Redundant network paths
- Gateway redundancy
- Controlled Internet access
- Secure device management
- Wireless connectivity
- High availability
- Network scalability

# Network Topology

The Cisco Packet Tracer topology contains:

- Cisco 2911 routers
- Cisco Catalyst switches
- End-user PCs
- Web server
- Email server
- Wireless router
- WAN routers
- Internet connectivity
- Redundant network connections

The Manama, Riyadh, and Kuwait sites are connected through the simulated Batelco WAN.

Switch-to-switch and switch-to-router connections use trunk links where required, while end devices are connected through access ports assigned to their respective VLANs.

# IP Addressing

A private IPv4 addressing scheme based on RFC1918 addressing was used for the internal enterprise network.

Each site was assigned a separate /16 address block:

| Site | Address Range |
|---|---|
| Manama HQ | 10.1.0.0/16 |
| Riyadh Branch | 10.2.0.0/16 |
| Kuwait Branch | 10.3.0.0/16 |

Department VLANs use /24 subnets.

The third octet corresponds to the VLAN ID, making the addressing structure easier to identify and manage.

Examples:

- 10.1.10.0/24 - Manama VLAN 10
- 10.1.20.0/24 - Manama VLAN 20
- 10.2.10.0/24 - Riyadh VLAN 10
- 10.3.10.0/24 - Kuwait VLAN 10

The WAN uses /30 point-to-point networks from the 10.10.0.0/24 address space.

# VLAN Design

VLANs were implemented to separate departments and functional areas into different broadcast domains.

The VLAN structure is consistent across the main sites.

| VLAN | Purpose |
|---|---|
| VLAN 10 | IT Department |
| VLAN 20 | Finance Department |
| VLAN 30 | HR Department |
| VLAN 40 | Operations |
| VLAN 50 | Sales / Spare Department |
| VLAN 60 | IT Management / Network Administration |
| VLAN 70 | Guest / Corporate Wireless |
| VLAN 80 | Future Expansion / Branch Connectivity |
| VLAN 90 | Email Server DMZ - HQ |
| VLAN 100 | Web Server DMZ - HQ |

VLAN 90 and VLAN 100 are used at the Manama Head Office for the email and web server DMZ networks.

# Inter-VLAN Routing

Inter-VLAN routing was implemented using Router-on-a-Stick.

Router subinterfaces are configured for the different VLANs, allowing communication between VLANs through the router.

802.1Q encapsulation is used to identify VLAN traffic across trunk links.

The router acts as the Layer 3 gateway for the VLANs and provides a central point where access control policies can be applied.

# VLAN Trunking

802.1Q trunking is used between:

- Switches
- Switches and routers
- Multiple switches within the same site

Trunk links allow multiple VLANs to travel across a single physical connection.

This allows devices connected to different switches to remain within the same logical VLAN.

# VTP

Cisco VLAN Trunking Protocol (VTP) was used to simplify VLAN management within the sites.

The VTP domain is configured as:

BISB

VTP server and client roles are used to distribute VLAN information between switches within a site.

The sites remain separated at Layer 2 because the WAN connections operate at Layer 3.

# Routing

OSPF was selected as the dynamic routing protocol for the enterprise network.

OSPF provides:

- Dynamic route discovery
- Fast convergence
- Redundant path selection
- Scalability
- Automatic route adjustment
- Support for multi-site enterprise networks

OSPF Process ID:

1

OSPF Area:

Area 0

The routers and WAN links participate in OSPF Area 0.

OSPF allows the network to dynamically adjust routing when WAN links or paths become unavailable.

# WAN Connectivity

The three BISB sites are connected through a simulated Batelco WAN.

Point-to-point serial links use /30 subnets from the 10.10.0.0/24 address space.

PPP is used on the WAN serial connections.

The WAN architecture provides multiple paths between sites to support redundancy and failover.

OSPF operates across the WAN to dynamically exchange routing information.

# HSRP

Hot Standby Router Protocol (HSRP) is used to provide gateway redundancy.

HSRP allows two routers to share a virtual default gateway for a VLAN.

The virtual gateway uses the .254 address.

For example:

10.x.x.254

If the active router becomes unavailable, the standby router can take over the virtual gateway role.

This reduces the impact of router failure and helps prevent a single point of failure at the gateway.

# EtherChannel

EtherChannel is used to combine multiple physical links into a logical connection.

This provides:

- Increased bandwidth
- Link redundancy
- Improved network availability
- Reduced dependency on a single physical link

# Spanning Tree Protocol

Spanning Tree Protocol (STP) is implemented to prevent Layer 2 switching loops.

PVST is used to manage spanning-tree instances for VLANs.

STP provides protection against network loops while maintaining redundant physical connections.

# Network Security

Security was implemented at multiple layers of the network.

The security design includes:

- VLAN segmentation
- Access Control Lists
- SSH management
- Wireless encryption
- Guest network isolation
- NAT
- DMZ segmentation
- Server access restrictions
- Internet access restrictions
- Disabled unused switch ports
- STP/BPDU protection

The network uses VLAN segmentation to isolate departments and reduce unnecessary Layer 2 access between users.

# Access Control Lists

ACLs are used to control traffic between different network segments.

The security policies include restrictions for:

- Inter-VLAN communication
- HR Internet access
- Finance Internet access
- Server access
- Management access
- External access to internal services

ACLs provide an additional security layer by allowing or denying traffic based on defined rules.

# Secure Device Management

Network devices are configured for secure remote management using SSH.

SSH is used instead of insecure remote management methods.

Management access is restricted to authorized network administration traffic.

A dedicated management VLAN is also used for network administration.

# DMZ Network

The Manama Head Office contains separate DMZ networks for the main servers.

## Email Server

VLAN 90:

192.168.90.0/24

Gateway:

192.168.90.1

The email server is isolated from normal user VLANs.

## Web Server

VLAN 100:

192.168.100.0/24

Gateway:

192.168.100.1

The web server is placed in a separate DMZ network to control access between the server and internal networks.

# NAT

Network Address Translation is used to allow private internal addresses to communicate with external networks.

NAT/PAT is used for outbound Internet connectivity.

Static NAT is used where controlled external access to internal services is required.

This allows private internal addressing while reducing the need for public IPv4 addresses.

# Wireless Network

Wireless connectivity is provided using a Linksys WRT300N wireless router.

The wireless design includes:

- BISB-Corp
- BISB-Guest
- WPA2 security
- Guest isolation
- DHCP
- NAT

The guest network is separated from corporate resources to reduce the risk of unauthorized access to internal systems.

# DHCP

DHCP is used to dynamically provide IP configuration to wireless clients.

The wireless DHCP range includes:

192.168.0.100 - 192.168.0.149

DHCP provides clients with the required network configuration without manually assigning addresses.

# Network Management

Several network management technologies are included in the implementation.

## SNMP

SNMP is used for network monitoring and management.

SNMPv2c is used in the implementation.

## Syslog

Syslog is used to collect and monitor device-generated log messages.

This assists with network troubleshooting and monitoring.

## NTP

Network Time Protocol is used to synchronize device clocks.

Accurate time synchronization is important for:

- Logging
- Troubleshooting
- Monitoring
- Security investigation

# Redundancy and High Availability

The network was designed with multiple redundancy mechanisms.

These include:

- HSRP
- Redundant WAN links
- OSPF failover
- EtherChannel
- STP
- Multiple network paths
- Router redundancy
- Switch redundancy

These mechanisms help maintain connectivity when a network component or link fails.

# Network Testing

The implemented network was tested to verify connectivity, routing, security, redundancy, wireless connectivity, and network management.

Testing included:

- End-to-end connectivity
- Inter-VLAN connectivity
- OSPF routing
- HSRP failover
- WAN link failover
- STP operation
- EtherChannel operation
- SSH access
- SSH access restrictions
- Server connectivity
- Server ICMP restrictions
- HR Internet restrictions
- Finance Internet restrictions
- External service filtering
- Wireless connectivity
- Guest wireless isolation
- NAT
- SNMP
- Syslog
- NTP

# Fault Tolerance

The network includes mechanisms designed to reduce the effect of network failures.

For example, OSPF can dynamically recalculate routes when a WAN path becomes unavailable, while HSRP provides an alternative gateway when the active gateway router fails.

STP and EtherChannel provide additional protection against link and Layer 2 failures.

# Network Scalability

The addressing and VLAN design allows additional departments, devices, and branches to be added without redesigning the entire network.

Each site has a dedicated /16 address block, while departments use separate /24 VLAN networks.

The structure also provides a clear method for identifying the location and department associated with an IP address.

# Technologies Used

- Cisco Packet Tracer
- Cisco 2911 Routers
- Cisco Catalyst Switches
- VLAN
- 802.1Q
- Router-on-a-Stick
- VTP
- OSPF
- HSRP
- EtherChannel
- PVST
- STP
- PPP
- NAT
- PAT
- DHCP
- ACL
- SSH
- SNMP
- Syslog
- NTP
- WPA2
- Wireless Networking
- Private IPv4 Addressing
- WAN Networking
- DMZ

# Skills Demonstrated

- Enterprise Network Design
- Network Architecture
- IPv4 Addressing
- Subnetting
- VLAN Design
- Inter-VLAN Routing
- Dynamic Routing
- OSPF Configuration
- WAN Configuration
- PPP
- HSRP
- Network Redundancy
- High Availability
- EtherChannel
- Spanning Tree Protocol
- Access Control Lists
- NAT and PAT
- Wireless Security
- SSH Configuration
- Network Management
- Network Troubleshooting
- Cisco Packet Tracer

# Project Files

| File | Description |
|---|---|
| `README.md` | Complete project documentation |
| `BISB-Enterprise-Network-Topology.pkt` | Complete Cisco Packet Tracer topology and device configurations |
| `BISB-Enterprise-Network-Report.docx` | Complete project report and documentation |

# Packet Tracer File

The Packet Tracer file contains the implemented network topology and configurations.

It includes:

- Routers
- Switches
- Servers
- PCs
- Wireless devices
- WAN connections
- VLAN configuration
- OSPF configuration
- HSRP configuration
- ACLs
- NAT
- DHCP
- SSH
- Network management configuration
- Redundant connections

**Packet Tracer File:**

[BISB Enterprise Network Topology](BISB-Enterprise-Network-Topology.pkt)

# Project Report

The complete project report provides detailed documentation of the network design, planning, implementation, configuration, security, redundancy, and testing.

**Project Report:**

[BISB Enterprise Network Report](BISB-Enterprise-Network-Report.docx)

# Project Structure

```text
BISB-Enterprise-Network/
├── README.md
├── BISB-Enterprise-Network-Topology.pkt
└── BISB-Enterprise-Network-Report.docx
