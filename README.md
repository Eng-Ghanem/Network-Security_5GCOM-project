# Enterprise Network Design & CyberOps Security Architecture (5GCOM)

An enterprise-grade campus network architecture and cybersecurity implementation designed for the **5GCOM** telecommunications provider using **Cisco Packet Tracer**. The project models a resilient multi-building topology segmented across departments, incorporating dynamic OSPF routing, Inter-VLAN routing, core network services (DHCP, DNS, Syslog, NTP, Web), and switchport-level perimeter security.

---

## Features

- **Multi-Building Campus Topology**:
  - Two interconnected corporate buildings with 3 functional departmental zones each: **IT**, **Human Resources (HR)**, and **Operations**.
- **Virtual LAN (VLAN) Segmentation**:
  - Granular traffic isolation restricting broadcast domains across departments and server clusters.
- **Dynamic & Inter-VLAN Routing**:
  - **Inter-VLAN Routing**: Layer 3 gateway switching enabling controlled cross-department communication.
  - **Open Shortest Path First (OSPF)**: Dynamic interior gateway protocol providing automated metric-based path computation and fast failure recovery.
- **Enterprise Network Services**:
  - **DHCP**: Centralized dynamic IP address allocation across segmented subnets.
  - **DNS**: Internal domain name resolution for company services.
  - **Syslog**: Centralized logging server capturing system events, audit trails, and interface state transitions.
  - **NTP**: Network Time Protocol synchronization ensuring unified audit log timestamps across routers and switches.
  - **HTTP/Web Server**: Internal intranet portal hosting.
- **Security & Threat Mitigation**:
  - **Access Control Lists (ACLs)**: Standard and Extended ACLs enforcing zero-trust traffic filters between sensitive subnets (e.g., isolating HR financial data from guest/operational nodes).
  - **Switch Port Security**: MAC address binding (`switchport port-security mac-address sticky`) and violation shutdown rules mitigating rogue device access and MAC flooding attacks.
  - **Secure Management**: Encrypted SSH administrative access replacing legacy Telnet.

---

## Network Architecture Topology

```mermaid
flowchart TD
    subgraph WAN ["WAN / ISP Gateway"]
        ISP["Edge Gateway / Internet"]
    end

    subgraph Core ["Enterprise Core & Server Farm"]
        CR["Core Router (OSPF Area 0)"]
        ISP --- CR
        subgraph Servers ["Data Center Services"]
            DHCP["DHCP Server"]
            DNS["DNS Server"]
            Syslog["Syslog Server"]
            NTP["NTP Server"]
            Web["Web Server"]
        end
        CR --- Servers
    end

    subgraph B1 ["Building 1 (Distribution & Access)"]
        SW1["Distribution Switch 1"]
        CR === SW1
        VLAN10["VLAN 10: IT Department"]
        VLAN20["VLAN 20: HR Department"]
        VLAN30["VLAN 30: Operations"]
        SW1 --- VLAN10
        SW1 --- VLAN20
        SW1 --- VLAN30
    end

    subgraph B2 ["Building 2 (Distribution & Access)"]
        SW2["Distribution Switch 2"]
        CR === SW2
        VLAN40["VLAN 40: IT Department"]
        VLAN50["VLAN 50: HR Department"]
        VLAN60["VLAN 60: Operations"]
        SW2 --- VLAN40
        SW2 --- VLAN50
        SW2 --- VLAN60
    end
```

---

## Configuration & Implementation Summary

| Subsystem | Implemented Protocols & Technologies | Purpose |
|---|---|---|
| Routing | OSPF (Open Shortest Path First), Inter-VLAN | Dynamic path selection and inter-subnet routing |
| Switching | 802.1Q Trunking, PortFast, Port Security | Layer 2 broadcast containment and port hardening |
| Addressing | IPv4 Subnetting, DHCP Relay / Helper Addresses | Optimized address utilization and automated client lease |
| Infrastructure Services | DNS, NTP, Syslog, HTTP | Name resolution, time synchronization, and central telemetry |
| Access Control | Extended IP Access Control Lists (ACLs) | Restricting unauthorized cross-department packet flow |
| Device Hardening | Sticky MAC, Maximum MAC limit = 1, Shutdown violation | Preventing CAM overflow and unauthorized physical port connection |

---

## Project Structure

```text
Network-Security_5GCOM-project/
├── final-11.pkt                           # Cisco Packet Tracer simulation topology and device configs
├── Final-Project 5GCOM presentation.pdf   # Architectural presentation and security review
└── README.md                              # Project documentation
```

---

## Simulation Verification & Media

Detailed topology layout and configuration verification screenshots:

- **Enterprise Topology Overview**:
  ![Topology Overview](https://github.com/Ghanem-MO/Network-Security_5GCOM-project/blob/8268531c852d7a3d53f013c57374b7ae1ec00bd6/Screenshot%202024-11-22%20112326.png)
- **Subnet & IP Configuration**:
  ![IP Configuration](https://github.com/Ghanem-MO/Network-Security_5GCOM-project/blob/fe09995472b97f135410f5a6b1bca78615957583/Screenshot%202024-11-22%20112618.png)
- **Server Farm Services (DHCP/DNS/Syslog)**:
  ![Server Farm](https://github.com/Ghanem-MO/Network-Security_5GCOM-project/blob/85c8206f61dd1d8938510a10933cdb90b6fbdbe5/Screenshot%202024-11-22%20112711.png)
- **Switch Port Security Status**:
  ![Port Security](https://github.com/Ghanem-MO/Network-Security_5GCOM-project/blob/5e80a43f5b74a672518a92070810dd216c8383d0/Screenshot%202024-11-22%20112800.png)
- **End-to-End Connectivity & Ping Verification**:
  ![Ping Verification](https://github.com/Ghanem-MO/Network-Security_5GCOM-project/blob/59498c30a553e0655cba6d63d993be3dc7cf551e/Screenshot%202024-11-22%20112842.png)

---

## How to Run the Simulation

### Prerequisites
- [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer) (version 8.0 or newer)

### Steps
1. Clone the repository:
   ```bash
   git clone https://github.com/Eng-Ghanem/Network-Security_5GCOM-project.git
   cd Network-Security_5GCOM-project
   ```
2. Open [`final-11.pkt`](final-11.pkt) in Cisco Packet Tracer.
3. Observe real-time OSPF neighbor relationships establish across routers.
4. Test inter-VLAN ping from client terminals in Building 1 to services in the core server farm.

---

## Documentation

Full project slides and design rationale are available in [Final-Project 5GCOM presentation.pdf](Final-Project%205GCOM%20presentation.pdf).

---

## Author

- **Mohamed Ghanem** - [Eng-Ghanem](https://github.com/Eng-Ghanem)
