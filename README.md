# CCNA-VLANs
Practice lab for CCNA exam with VLANs, DHCP IP addressing, switchport, port security, and OSPF area 1 routing.

# CCNA Network Configuration Lab

This repository contains the full Cisco IOS configurations for the network topology, including OSPF routing, VLAN management, VTP, and Layer 2 security.

## 🔐 Global Management Credentials
- [cite_start]**Enable Secret:** `CISCO` [cite: 5, 24, 49, 75, 107, 164, 229, 258, 287, 315, 341, 370, 399]
- [cite_start]**VTY Password:** `CISCO` [cite: 7, 26, 51, 77, 109, 166, 231, 260, 289, 317, 343, 372, 401]
- [cite_start]**Exec-Timeout:** `30` [cite: 9, 28, 53, 79, 111, 168, 233, 262, 291, 319, 345, 374, 403]

---

## 🛰️ Router Configurations

### ISP
- [cite_start]**Loopback:** 8.8.8.8/32 [cite: 15]
- [cite_start]**OSPF:** Router-ID 8.8.8.8, Area 0 [cite: 17-18]
- [cite_start]**Serial 0/3/0:** 10.0.0.1/30 (Clock Rate 128000) [cite: 10-12]

### R1
- [cite_start]**OSPF:** Router-ID 1.1.1.1 [cite: 40]
- **Interfaces:**
  - [cite_start]S0/3/0: 10.0.0.2/30 [cite: 30]
  - [cite_start]S0/3/1: 10.0.0.5/30 (Clock Rate 128000) [cite: 34]
  - [cite_start]G0/1: 10.0.0.13/30 [cite: 37]

### R2
- [cite_start]**OSPF:** Router-ID 2.2.2.2 [cite: 66]
- **Interfaces:**
  - [cite_start]S0/3/0: 10.0.0.6/30 [cite: 55]
  - [cite_start]S0/3/1: 10.0.0.9/30 [cite: 59]
  - [cite_start]S0/2/0: 10.0.0.17/30 [cite: 63]

### R3 (Server Gateway)
- [cite_start]**OSPF:** Passive interfaces on server VLANs [cite: 100-101]
- **Sub-interfaces:**
  - [cite_start]G0/0.210 (Web): 210.99.99.1/30 [cite: 90]
  - [cite_start]G0/0.211 (DNS): 210.99.99.5/30 [cite: 93]

### R4 (Branch Office)
- [cite_start]**DHCP Pools:** HR (192.168.11.0), IT (192.168.21.0), SALES (192.168.31.0) [cite: 133, 138, 143]
- [cite_start]**Sub-interfaces:** G0/0.10, G0/0.20, G0/0.30 [cite: 124, 127, 130]

---

## 🏢 Switch Configurations

### MS1 (Multilayer Switch)
- [cite_start]**VTP:** Server Mode, Domain `APOORVA`, Password `JADHAV` [cite: 179-181]
- [cite_start]**Layer 3:** `ip routing` enabled with OSPF [cite: 213-214]
- [cite_start]**DHCP:** Pools for VLANs 10, 20, and 30 [cite: 194, 199, 204]
- [cite_start]**STP:** Root Primary for VLANs 1, 10, 20, 30 [cite: 210]

### SW1 - SW3, SW5 - SW6 (Access Switches)
- [cite_start]**VTP:** Client Mode [cite: 238, 267, 295, 350, 379]
- [cite_start]**STP Security:** BPDUGuard and Portfast enabled on access ports [cite: 242-243, 271-272, 299-300, 354-355, 383-384]
- **VLAN Access:**
  - [cite_start]Fa0/1-8: VLAN 10 (HR) [cite: 246, 275, 303, 358, 387]
  - [cite_start]Fa0/9-16: VLAN 20 (IT) [cite: 249, 278, 306, 361, 390]
  - [cite_start]Fa0/17-24: VLAN 30 (Sales) [cite: 252, 281, 309, 364, 393]

### SW7 (Server Switch)
- [cite_start]**VLAN 210:** Web-Server [cite: 406-407]
- [cite_start]**VLAN 211:** DNS-Server [cite: 408-409]

---

## 🛠️ Implementation Notes
1. [cite_start]**Routing:** All routers use OSPF Process 1. [cite: 16, 39, 65, 94, 148, 214]
2. [cite_start]**Encapsulation:** Sub-interfaces use `dot1q`. [cite: 89, 92, 125, 128, 131]
3. [cite_start]**Redundancy:** Spanning Tree Rapid-PVST (`RAP`) is used across the switching fabric. [cite: 209, 234, 263, 292, 320, 346, 375]