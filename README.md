# SOHO Network  Simulation

![Network Topology Status](https://img.shields.io/badge/Status-Complete-success)
![Platform](https://img.shields.io/badge/Platform-Cisco%20Packet%20Tracer-blue)

## 📖 Project Overview
This project implements a fully segmented **Small Office/Home Office (SOHO)** network using Cisco Packet Tracer. The design includes **three VLANs**, **Router-On-A-Stick (ROAS)** for inter-VLAN routing, and centralized **DHCP services** provisioned through the router. The repository includes complete documentation and topology diagrams suitable for training and demonstration.

**Key Objectives:**

* Reduce broadcast congestion through VLAN segmentation.
* Demonstrate proficiency in Layer 2 switching and Layer 3 routing concepts.

## 🏗️ Network Topology
![Network Topology Diagram](./topology.png)

### Design Specifications
The network is divided into three distinct VLANs to enforce logical separation:

| VLAN ID | Name | Subnet |
| :--- | :--- | :--- |
| **10** | HR Department | `192.168.10.0/24` |  
| **20** | Sales Department | `192.168.20.0/24` | 
| **30** | IT Department | `192.168.30.0/24` | 

## 🛠️ Technologies & Protocols Used
* **Core Networking:** IPv4 Addressing, Subnetting, Default Routing
* **Switching (Layer 2):** VLANs (Virtual LANs)
    * 802.1Q Trunking (Tagging)
    * Access Ports vs. Trunk Ports
* **Routing (Layer 3):** Router-on-a-Stick (Subinterfaces)
    * SVIs (Switched Virtual Interfaces for L3 Switch version)
    * Static Routing
* **Services:** DHCP (Dynamic Host Configuration Protocol) with excluded addresses (Default gateway address in each VLAN)

## ⚙️ Configuration Highlights
Below are snippets of the core configurations applied to the devices.

### **Router-on-a-Stick (Inter-VLAN Routing) for Layer 2 Switch**

**Network/End Devices**: PCs and 2 Cisco 2960 switches for each department, a Cisco 1941 router

#### **1. Connect each device to match the topology. (use Copper Straight-Through)**

#### *(Optional) Rename each device to match its purpose.

![Diagram](./img/L2-1.png)

#### 2. Configuring subinterfaces on the Gateway Router to allow communication between isolated VLANs.

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
```

#### 2. Configuring subinterfaces on the Gateway Router to allow communication between isolated VLANs.

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
```

### **SVIs (Switch Virtual Interfaces) for Layer 3 Switch**
