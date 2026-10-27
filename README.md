# Enterprise Network Infrastructure Project
## Multi-Site Routing, Redundancy, Security, and Monitoring Architecture

## Overview

This project demonstrates the design and deployment of a highly available enterprise network supporting multiple locations, centralized services, network security controls, and infrastructure monitoring.

The environment includes headquarters and branch office connectivity over a routed WAN, gateway redundancy through HSRP, dynamic route advertisement using OSPF, segmented departmental VLANs, centralized DHCP and DNS services, DMZ-hosted public services, and comprehensive monitoring through Syslog, SNMP, and NTP.

The project was completed as a practical implementation of enterprise networking principles and serves as a portfolio demonstration of routing, switching, security, high availability, and troubleshooting skills.
 
### Key Technologies

- VLAN Segmentation
- Router-on-a-Stick (ROAS)
- Inter-VLAN Routing
- Multi-Site WAN
- OSPF
- HSRP
- DHCP
- DNS
- DMZ
- ACLs
- NAT/PAT
- EtherChannel (LACP)
- STP
- Syslog
- SNMP
- NTP

## Topology

The topology consists of a headquarters location, two branch offices, dual edge routers, a dedicated DMZ, centralized network services, and monitoring infrastructure connected through an OSPF-routed WAN.

<img width="1508" height="841" alt="Topology 2026-10-27 152106" src="https://github.com/user-attachments/assets/2a92683c-cd15-430e-b087-e66799c87f57" />


## Network Architecture

### Headquarters
- Dual HQ Routers
- Core Switch
- Access Switches
- DHCP Server
- DNS Server
- Monitoring Server

### Branch Office 1
- Router
- Switch
- Clients

### Branch Office 2
- Router
- Switch
- Clients

### Edge
- Dual Edge Routers
- ISP Connection

## Technologies Demonstrated

### Routing
- OSPF
- ROAS
- WAN Design

### Redundancy
- HSRP
- Dual Edge Design
- EtherChannel

### Security
- ACLs
- NAT/PAT
- DMZ
- Disabled Unused Ports

### Monitoring
- Syslog
- SNMP
- NTP

## VLAN Design

| VLAN | Purpose | Network |
|--------|----------|----------|
| 10 | IT | 192.168.10.0/28 |
| 20 | Sales | 192.168.20.0/28 |
| 30 | HR | 192.168.30.0/28 |
| 40 | Printers | 192.168.40.0/28 |
| 50 | Servers | 192.168.50.0/28 |
| 60 | DMZ | 172.16.60.0/24 |

## Core Network Services

| Service | IP Address |
|----------|------------|
| DHCP Server | 192.168.50.10 |
| DNS Server | 192.168.50.11 |
| Web Server | 192.168.50.12 |
| Monitoring Server | 192.168.50.13 |

## DMZ Implementation

A dedicated DMZ VLAN was created to host public-facing services while maintaining separation from internal enterprise resources.

### Benefits

- Isolation of public-facing systems
- Reduced attack surface
- Improved network segmentation

DMZ Network:


172.16.60.0/24

## WAN Design

The enterprise network consists of a headquarters location, two branch offices, dual edge routers, and an ISP connection.

OSPF Area 0 was deployed across the WAN infrastructure to dynamically exchange routes between all locations.

### WAN Backbone


HQ1      10.0.0.1
BR1      10.0.0.2
BR2      10.0.0.3
EDGE1    10.0.0.4
EDGE2    10.0.0.5
HQ2      10.0.0.6
 
## Implementation & Validationn

### OSPF

OSPF Area 0 was implemented to dynamically advertise routes between Headquarters, Branch Office 1, Branch Office 2, and the Edge routers. This removed the need for static routing and provided scalable WAN connectivity.

<img width="955" height="892" alt="HQ Router - 2 OSPF 2026-10-27 142048" src="https://github.com/user-attachments/assets/bb1a25c7-914f-468c-a823-bade35a2ef49" />

### HSRP

HSRP provides default gateway redundancy for all HQ VLANs. If the active router fails, the standby router automatically assumes gateway responsibilities.

<img width="681" height="879" alt="HQ Router - 2 (HSRP) 2026-10-27 143310" src="https://github.com/user-attachments/assets/55556a3b-3adf-428b-95d3-57aafd27c820" />

### ACLs

Extended ACLs were used to enforce security policies including restricting Sales and Branch users from accessing HR resources and preventing printer-initiated communication toward user VLANs.

<img width="528" height="873" alt="HQ Router - 1 (ACL) 2026-10-27 143936" src="https://github.com/user-attachments/assets/1721b27c-e415-4c0d-bdb5-0e29e9264341" />

### NAT/PAT

PAT was implemented on the Edge router to allow internal private networks to access external networks using a single public IP address.

<img width="662" height="878" alt="Edge Router - 1 (NAT) 2026-10-27 144133" src="https://github.com/user-attachments/assets/c99135d7-ffe6-4d82-af25-557f23a74900" />

<img width="415" height="875" alt="Edge Router1 Nat (statistics) 2026-10-27 152851" src="https://github.com/user-attachments/assets/24ac45a1-1348-46c9-ba90-9d07564a0c1f" />


### EtherChannel

LACP-based EtherChannels were configured between the Core Switch and Access Switches. EtherChannel combines multiple physical links into a single logical connection, providing increased bandwidth and redundancy while simplifying Spanning Tree Protocol operations.

<img width="697" height="876" alt="Core Switch - Etherchannel 2026-10-27 143500" src="https://github.com/user-attachments/assets/1f8a6782-7c0f-4c86-a22b-862957c83c18" />


### STP

Spanning Tree Protocol (STP) was implemented to prevent Layer 2 switching loops. The Core Switch was configured as the root bridge, ensuring predictable traffic paths throughout the switching infrastructure.

<img width="550" height="872" alt="Core Switch -  STP(Spanning-tree) 2026-10-27 143657" src="https://github.com/user-attachments/assets/99b1d395-b822-4395-82e0-0bfbd902b19e" />

### DNS

The DNS server provides hostname-to-IP address resolution for internal network resources. This allows users to access services using hostnames rather than memorizing IP addresses.

<img width="1014" height="459" alt="DNS 2026-10-27 155813" src="https://github.com/user-attachments/assets/34a38181-994b-48cc-bd55-e5be5f8c6191" />

### DHCP Lease

Dynamic Host Configuration Protocol (DHCP) was implemented to automatically assign IP addresses, subnet masks, default gateways, and DNS server information to clients. DHCP reduces administrative overhead and ensures consistent network addressing across all locations.

<img width="1014" height="546" alt="DHCP Lease 2026-10-27 153358" src="https://github.com/user-attachments/assets/ded38076-7989-4f9e-b63a-82739eddeb66" />

### WEB Service Access/DMZ

A dedicated DMZ VLAN was created to host public-facing services while maintaining separation from internal enterprise resources. This segmentation reduces the exposure of internal systems and improves overall network security.

<img width="1033" height="707" alt="Web Service 2026-10-27 154043" src="https://github.com/user-attachments/assets/eb516d61-febf-450c-a3bc-2790e3e7f1a9" />

### Syslog

Syslog was deployed to centralize event logging from routers and switches. Network events, interface status changes, and configuration notifications are forwarded to the Monitoring Server for troubleshooting and operational visibility.

<img width="1002" height="697" alt="Monitoring Server (Syslog) 2026-10-27 144241" src="https://github.com/user-attachments/assets/6eed07cc-115e-4cf2-9a17-d623fcece583" />

### SNMP

Simple Network Management Protocol (SNMP) was configured to provide network monitoring capabilities. SNMP enables centralized visibility into device status and supports future integration with network management platforms.

<img width="318" height="93" alt="SNMP 2026-10-27 160226" src="https://github.com/user-attachments/assets/e1a615aa-2a57-4001-ae01-740cbe621462" />

### NTP

Network Time Protocol (NTP) was configured to synchronize time across all network infrastructure devices. Consistent timestamps improve troubleshooting accuracy and event correlation within Syslog data

<img width="288" height="38" alt="NTP 2026-10-27 160624" src="https://github.com/user-attachments/assets/afe055d3-c14d-4709-89a6-0dfa8a239d96" />

<img width="1256" height="101" alt="NTP Sync 2026-10-27 172259" src="https://github.com/user-attachments/assets/775ea2bb-6039-4d8c-9730-e8fa56f99bd1" />

## Validation Results

| Test | Result |
|--------|--------|
| VLAN Segmentation | ✅ Pass |
| Inter-VLAN Routing | ✅ Pass |
| OSPF Neighbor Formation | ✅ Pass |
| Branch Connectivity | ✅ Pass |
| HSRP Failover | ✅ Pass |
| DHCP Address Assignment | ✅ Pass |
| DNS Resolution | ✅ Pass |
| Web Server Access | ✅ Pass |
| ACL Enforcement | ✅ Pass |
| NAT/PAT Translation | ✅ Pass |
| DMZ Segmentation | ✅ Pass |
| EtherChannel Operation | ✅ Pass |
| STP Root Bridge Election | ✅ Pass |
| Syslog Collection | ✅ Pass |
| SNMP Configuration | ✅ Pass |
| NTP Synchronization | ✅ Pass |

## Lessons Learned

This project strengthened my understanding of enterprise network design, routing, switching, redundancy, security, and infrastructure monitoring.

Key concepts reinforced throughout the project included:


- OSPF deployment and route advertisement
- HSRP gateway redundancy and failover
- DHCP relay configuration
- ACL design and traffic filtering
- NAT/PAT implementation
- DMZ network architecture
- EtherChannel deployment using LACP
- STP root bridge selection
- Centralized monitoring through Syslog, SNMP, and NTP
- Network troubleshooting and validation

## Skills Demonstrated

- Enterprise Network Design
- WAN Architecture Design and Implementation
- Cisco IOS Configuration
- VLAN Segmentation
- Router-on-a-Stick (ROAS)
- Inter-VLAN Routing
- OSPF Dynamic Routing
- HSRP High Availability
- DHCP & DHCP Relay
- DNS Services
- NAT/PAT
- ACL-Based Security
- DMZ Design
- EtherChannel (LACP)
- Spanning Tree Protocol (STP)
- Syslog Monitoring
- SNMP Monitoring
- NTP Time Synchronization
- Enterprise Troubleshooting
- Network Documentation
