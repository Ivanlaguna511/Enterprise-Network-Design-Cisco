# Corporate Network Architecture & Security Design

<div align="center">
  
  ![Cisco](https://img.shields.io/badge/Cisco-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white)
  ![Packet Tracer](https://img.shields.io/badge/Packet_Tracer-8.x-success?style=for-the-badge)
  ![OSPF](https://img.shields.io/badge/Routing-OSPF-orange?style=for-the-badge)
  ![Security](https://img.shields.io/badge/Security-ACLs_%7C_VPN_%7C_NAT-red?style=for-the-badge)
  ![Academic](https://img.shields.io/badge/3rd_Year-Network_Design-purple?style=for-the-badge)

</div>


> **ABOUT THIS PROJECT:**
> Comprehensive design, administration, and security implementation of a corporate LAN/WAN network for an insurance company. The architecture connects a central headquarters (Valladolid) with 8 regional branches across Castilla y León, ensuring high availability, security, and scalability.

<div align="center">
<img width="700" src="https://github.com/user-attachments/assets/298984af-7dcb-446e-ad97-90ee8e4832da"/>
</div>


## Architecture Overview

The network follows a hierarchical, modular design pattern to ensure scalability and ease of management:

* **Central Headquarters (Sede Central):** Houses the core network, Data Center (CPD), and common services.
* **Regional Branches (8 Delegaciones):** Connected via a simulated Fiber Optic WAN (1 Gbps for HQ, 300 Mbps for branches).
* **DMZ & Internet Edge:** Public-facing services (Web Server, Mobile App Backend) are isolated in a DMZ with strict firewall/ACL rules and NAT translation.
* **Data Center (CPD):** Centralized servers for ERP, CRM, and internal utilities, protected and highly available.

## Key Technologies & Protocols

* **Routing & Switching:**
    * **OSPF:** Used for dynamic, summarized routing to minimize routing table size and CPU/memory overhead. Passive interfaces are configured for security.
    * **RSTP & HSRP:** Implemented to provide Layer 2 loop prevention and Layer 3 default gateway redundancy, ensuring 99.999% availability.
* **Network Services:**
    * **DHCP & DNS:** Centralized IP management and name resolution.
    * **NAT/PAT:** Configured on the ISP edge router for secure internet access.
* **Security & Access:**
    * **ACLs (Access Control Lists):** Stateful firewall filtering at the ISP edge (e.g., blocking unestablished incoming traffic except for HTTP/HTTPS to the DMZ).
    * **VPN:** Secure remote access for employees working outside the office.
    * **WPA2-PSK & RADIUS:** Secure enterprise Wi-Fi for HQ employees and isolated guest access.
* **Management:**
    * **SNMPv3:** Secure network monitoring and fault management.

## VLANs & IP Addressing (VLSM)

The network is logically segmented using VLANs to separate departmental traffic (Accounting, Management, IT, Customer Service, and Guests). Private IP addressing (Class A for branches, Class C for DMZ/Core) is heavily subnetted:

| Location | VLAN | Subnet / Mask | Department |
| :--- | :--- | :--- | :--- |
| **Sede VA (HQ)** | 10 | `10.1.10.0/24` | Contabilidad |
| | 20 | `10.1.20.0/24` | Gestión |
| | 30 | `10.1.30.0/24` | Informática |
| | 40 | `10.1.40.0/24` | Atención al cliente |
| | 50 | `10.1.50.0/24` | Visitas (WIFI) |
| **Sede LE** | 10-40 | `10.2.x.0/24` | Branch Departments |
| **DMZ** | - | `192.168.3.0/24` | Public Servers |
| **Core** | - | `192.168.0.0/24` | Backbone |

*Note: The first 10 IP addresses of each subnet are reserved for static assignments (Servers, Routers, Default Gateways).*

## Security Policies Implemented

1.  **Internet Boundary Filtering:** Strict incoming traffic denial via ACLs on `RTRISP`. Only established TCP connections and specific ports (443 to Web Server) are permitted.
2.  **Intrusion Prevention Systems (IPS):** Deployed at the subnet level to detect and mitigate potential threats.
3.  **Encrypted Remote Access:** Implementation of Virtual Private Networks (VPN) for administrative and management staff.
4.  **Wireless Security:** Segregated Guest Wi-Fi and WPA2-Enterprise level security for staff.

## Hardware Blueprint

The physical topology is designed using enterprise-grade Cisco and Dell equipment:
* **Core/Distribution:** Cisco Catalyst 3650 Multilayer Switches.
* **Edge Routing:** Cisco ISR 4351 Routers.
* **Access Layer:** Cisco Catalyst 2960-L Switches.
* **Wireless:** Cisco Aironet 1832i Access Points.
* **Servers:** Dell PowerEdge R740 (Data Center) & R240 (Web/DMZ).

## How to Run

1. Download and install [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer).
2. Clone this repository or download the `Proyecto.pkt` file.
3. Open `Proyecto.pkt` in Cisco Packet Tracer.
4. Allow time for the STP (Spanning Tree Protocol) and OSPF to converge (link lights will turn green).
5. Use the PC terminals to test connectivity via `ping` (e.g., from PCVA10 to PCSO20) or test web access via the internal web browsers.

---
### Authors
* **Gonzalo Sánchez Maroto** - *Topology, Network Apps, Protocols & Testing*
* **Gabriel Ferrero Herrera** - *Tech Requirements, Addressing, Security & Management*
* **Iván Moro Cienfuegos** - *Executive Summary, Business Requirements, User Groups & Physical Design*
