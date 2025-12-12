# SOHO Network Architecture Simulation

![Network Topology Status](https://img.shields.io/badge/Status-Complete-success)
![Platform](https://img.shields.io/badge/Platform-Cisco%20Packet%20Tracer-blue)
![License](https://img.shields.io/badge/License-MIT-green)

## 📖 Project Overview
This project simulates a scalable **Small Office/Home Office (SOHO)** network infrastructure designed to address common security and performance issues found in flat networks. 

Moving beyond a standard "plug-and-play" setup, this architecture implements **VLAN segmentation** to isolate broadcast domains and traffic types. It utilizes **Router-on-a-Stick (ROAS)** for efficient Inter-VLAN routing and centralizes address management via **DHCP**.

**Key Objectives:**
* Improve network security by isolating sensitive traffic from guest/IoT devices.
* Reduce broadcast congestion through VLAN segmentation.
* Demonstrate proficiency in Layer 2 switching and Layer 3 routing concepts.

## 🏗️ Network Topology
*(Place a screenshot of your Packet Tracer topology here. Save the image as `topology.png` in your repo)*
![Network Topology Diagram](./topology.png)

### Design Specifications
The network is divided into three distinct VLANs to enforce logical separation:

| VLAN ID | Name | Subnet | Description |
| :--- | :--- | :--- | :--- |
| **10** | Management | `192.168.10.0/24` | Network Devices & Admin Access |
| **20** | Employee | `192.168.20.0/24` | Staff workstations and printers |
| **30** | Guest/IoT | `192.168.30.0/24` | Untrusted devices (Internet access only) |

## 🛠️ Technologies & Protocols Used
* **Core Networking:** IPv4 Addressing, Subnetting, Default Routing
* **Switching (Layer 2):** * VLANs (Virtual LANs)
    * 802.1Q Trunking (Tagging)
    * Access Ports vs. Trunk Ports
* **Routing (Layer 3):** * Router-on-a-Stick (Subinterfaces)
    * SVIs (Switched Virtual Interfaces - L3 Switch variant)
    * Static Routing
* **Services:** * DHCP (Dynamic Host Configuration Protocol) with excluded addresses

## ⚙️ Configuration Highlights
Below are snippets of the core configurations applied to the devices.

### 1. Router-on-a-Stick (Inter-VLAN Routing)
Configuring subinterfaces on the Gateway Router to allow communication between isolated VLANs.

```cisco
interface GigabitEthernet0/0.10
 description Gateway for Management VLAN
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface GigabitEthernet0/0.20
 description Gateway for Employee VLAN
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
