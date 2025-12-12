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

## Router-on-a-Stick (Inter-VLAN Routing) for Layer 2 Switch

**Network & End Devices**: PCs and 2 Cisco 2960 switches, a Cisco 1941 router

**1. Connect each device to match the topology. (use Copper Straight-Through)**

*(Optional) Rename each device to match its purpose.

![Diagram](./img/L2-1.png)


**2. Create VLANs on both switches. (global config)**
```cisco
vlan 10
 name HR
vlan 20 
 name Sales
vlan 30
 name IT
```


**3. Configure VLANs on both switches to match the topology.**

Ex. On SW1
```cisco
interface range FastEthernet0/1-3
 switchport mode access
 switchport access vlan 10
interface range FastEthernet0/4-5
 switchport mode access
 switchport access vlan 20
```
*Tip: You can also use contractions, switches and routers will complete the command. Also, if you did not create VLAN before configuring them, it will be created on the spot!*

```cisco
int ra f0/1-3
 sw mode ac
 sw ac vlan 10 
int ra f0/4-5
 sw mode ac
 sw ac vlan 20
```

*Use 'show vlan brief' ('do show' - interface config mode)*
![Diagram](./img/show_vlan.png)


**4. Configure IPs on PCs to match their subnets (Static Configuration) and use 'ping' in command prompt another host on the same VLAN to test connectivity.**

*Note: 192.168.xx.1 will be reserve for default gateway IPs*

![Diagram](./img/ping.png)

**5. Configure trunk port on both switch and on the SW2, set the interface connecting to the router to be a trunk port.**
```cisco
interface FastEthernet0/24
 switchport mode trunk
interface GigabitEthernet0/1
 swtichport mode trunk
```
*Use 'show interfaces trunk' ('do show' - interface config mode) and ping vlan20 on another switch to test connectivity*

![Diagram](./img/show_trunk.png)

**6. Configure subinterfaces on the Gateway Router to allow communication between isolated VLANs.**

```cisco
no shutdown 
!
interface GigabitEthernet0/0.10
 description Gateway for HR VLAN
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0
!
interface GigabitEthernet0/0.20
 description Gateway for Sales VLAN
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0
!
interface GigabitEthernet0/0.30
 description Gateway for IT VLAN
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0
```
**7. Configure the appropiate default gateway on each PC and ping test.**

*Note: ping may be failed for the first time due to ARP*
![Diagram](./img/L2-7.png)

### **SVIs (Switch Virtual Interfaces) for Layer 3 Switch**
