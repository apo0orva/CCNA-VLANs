# CCNA-VLANs
Practice lab for CCNA exam with VLANs, DHCP IP addressing, switchport, port security, and OSPF area 1 routing.

# CCNA Network Configuration Lab

This repository contains the full Cisco IOS configurations for the network topology, including OSPF routing, VLAN management, VTP, and Layer 2 security.

## 🔐 Global Management Credentials
- **Enable Secret:** `CISCO`
- **VTY Password:** `CISCO`
- **Exec-Timeout:** `30`

---

## 🛰️ Router Configurations

### ISP
- **Loopback:** 8.8.8.8/32 
- **OSPF:** Router-ID 8.8.8.8, Area 0
- **Serial 0/3/0:** 10.0.0.1/30 (Clock Rate 128000)

### R1
- **OSPF:** Router-ID 1.1.1.1 
- **Interfaces:**
  - S0/3/0: 10.0.0.2/30
  - S0/3/1: 10.0.0.5/30 (Clock Rate 128000)
  - G0/1: 10.0.0.13/30 

### R2
- **OSPF:** Router-ID 2.2.2.2
- **Interfaces:**
  - S0/3/0: 10.0.0.6/30
  - S0/3/1: 10.0.0.9/30 
  - S0/2/0: 10.0.0.17/30 

### R3 (Server Gateway)
- **OSPF:** Passive interfaces on server VLANs
- **Sub-interfaces:**
  - G0/0.210 (Web): 210.99.99.1/30 
  - G0/0.211 (DNS): 210.99.99.5/30 

### R4 (Branch Office)
- **DHCP Pools:** HR (192.168.11.0), IT (192.168.21.0), SALES (192.168.31.0) 
- **Sub-interfaces:** G0/0.10, G0/0.20, G0/0.30 

---

## 🏢 Switch Configurations

### MS1 (Multilayer Switch)
- **VTP:** Server Mode, Domain `APOORVA`, Password `JADHAV`
- **Layer 3:** `ip routing` enabled with OSPF 
- **DHCP:** Pools for VLANs 10, 20, and 30 
- **STP:** Root Primary for VLANs 1, 10, 20, 30 

### SW1 - SW3, SW5 - SW6 (Access Switches)
- **VTP:** Client Mode
- **STP Security:** BPDUGuard and Portfast enabled on access ports
- **VLAN Access:**
  - Fa0/1-8: VLAN 10 (HR)
  - Fa0/9-16: VLAN 20 (IT)
  - Fa0/17-24: VLAN 30 (Sales)

### SW7 (Server Switch)
- **VLAN 210:** Web-Server
- **VLAN 211:** DNS-Server 

---

## 🛠️ Implementation Notes
1. **Routing:** All routers use OSPF Process 1.
2. **Encapsulation:** Sub-interfaces use `dot1q`.
3. **Redundancy:** Spanning Tree Rapid-PVST (`RAP`) is used across the switching fabric.

# CCNA Project Configuration Guide

This document contains the CLI configuration for all network devices in the topology.

---

## 🛰️ Router Configurations

### ISP Configuration
```cisco
EN
CONF T
HOSTNAME ISP
ENABLE SECRET CISCO
LINE VTY 0 5
 PASSWORD CISCO
 LOGIN
 EXEC-TIMEOUT 30
INT S0/3/0
 CLOCK RATE 128000
 IP ADD 10.0.0.1 255.255.255.252
 NO SHUT
INT L0
 IP ADD 8.8.8.8 255.255.255.255
ROUTER OSPF 1
 DEFAULT-INFORMATION ORIGINATE
 ROUTER-ID 8.8.8.8
 NETWORK 10.0.0.0 0.0.0.3 AREA 0
DO WR MEM
```


### R1 Configuration
```cisco
EN
CONF T
HOSTNAME R1
ENABLE SECRET CISCO
LINE VTY 0 5
 PASSWORD CISCO
 LOGIN
 EXEC-TIMEOUT 30
INT S0/3/0
 IP ADD 10.0.0.2 255.255.255.252
 NO SHUT
INT S0/3/1
 CLOCK RATE 128000
 IP ADD 10.0.0.5 255.255.255.252
 NO SHUT
INT G0/1
 IP ADD 10.0.0.13 255.255.255.252
 NO SHUT
ROUTER OSPF 1
 ROUTER-ID 1.1.1.1
 NETWORK 10.0.0.0 0.0.0.3 AREA 0
 NETWORK 10.0.0.4 0.0.0.3 AREA 0
 NETWORK 10.0.0.12 0.0.0.3 AREA 0
DO WR MEM
```

### R2 Configuration
```cisco
EN
CONF T
HOSTNAME R2
ENABLE SECRET CISCO
LINE VTY 0 5
 PASSWORD CISCO
 LOGIN
 EXEC-TIMEOUT 30
INT S0/3/0
 IP ADD 10.0.0.6 255.255.255.252
 NO SHUT
INT S0/3/1
 CLOCK RATE 128000
 IP ADD 10.0.0.9 255.255.255.252
 NO SHUT
INT S0/2/0
 CLOCK RATE 128000
 IP ADD 10.0.0.17 255.255.255.252
 NO SHUT
ROUTER OSPF 1
 ROUTER-ID 2.2.2.2
 NETWORK 10.0.0.4 0.0.0.3 AREA 0
 NETWORK 10.0.0.8 0.0.0.3 AREA 0
 NETWORK 10.0.0.16 0.0.0.3 AREA 0
DO WR MEM
```


### R3 Configuration
```cisco
EN
CONF T
HOSTNAME R3
ENABLE SECRET CISCO
LINE VTY 0 5
 PASSWORD CISCO
 LOGIN
 EXEC-TIMEOUT 30
INT S0/3/0
 IP ADD 10.0.0.10 255.255.255.252
 NO SHUT
INT S0/3/1
 IP ADD 10.0.0.21 255.255.255.252
 NO SHUT
INT G0/0
 NO SHUT
INT G0/0.210
 ENCAP DOT1Q 210
 IP ADD 210.99.99.1 255.255.255.252
INT G0/0.211
 ENCAP DOT1Q 211
 IP ADD 210.99.99.5 255.255.255.252
ROUTER OSPF 1
 ROUTER-ID 3.3.3.3
 NETWORK 10.0.0.8 0.0.0.3 AREA 0
 NETWORK 10.0.0.20 0.0.0.3 AREA 0
 NETWORK 210.99.99.0 0.0.0.3 AREA 0
 NETWORK 210.99.99.4 0.0.0.3 AREA 0
 PASSIVE-INTERFACE G0/0.210
 PASSIVE-INTERFACE G0/0.211
DO WR MEM
```

### R4 Configuration
```cisco
EN
CONF T
HOSTNAME R4
ENABLE SECRET CISCO
LINE VTY 0 5
 PASSWORD CISCO
 LOGIN
 EXEC-TIMEOUT 30
INT G0/2
 IP ADD 10.0.0.26 255.255.255.252
 NO SHUT
INT S0/3/0
 IP ADD 10.0.0.18 255.255.255.252
 NO SHUT
INT S0/3/1
 CLOCK RATE 128000
 IP ADD 10.0.0.22 255.255.255.252
 NO SHUT
INT G0/0
 NO SHUT
INT G0/0.10
 ENCAP DOT1Q 10
 IP ADD 192.168.11.1 255.255.255.0
INT G0/0.20
 ENCAP DOT1Q 20
 IP ADD 192.168.21.1 255.255.255.0
INT G0/0.30
 ENCAP DOT1Q 30
 IP ADD 192.168.31.1 255.255.255.0
IP DHCP POOL HR
 DEFAULT-ROUTER 192.168.11.1
 NETWORK 192.168.11.0 255.255.255.0
 DNS-SERVER 210.99.99.6
 IP DHCP EX 192.168.11.1 192.168.11.5
IP DHCP POOL IT
 DEFAULT-ROUTER 192.168.21.1
 NETWORK 192.168.21.0 255.255.255.0
 DNS-SERVER 210.99.99.6
 IP DHCP EX 192.168.21.1 192.168.21.5
IP DHCP POOL SALES
 DEFAULT-ROUTER 192.168.31.1
 NETWORK 192.168.31.0 255.255.255.0
 DNS-SERVER 210.99.99.6
 IP DHCP EX 192.168.31.1 192.168.31.5
ROUTER OSPF 1
 ROUTER-ID 4.4.4.4
 NETWORK 10.0.0.16 0.0.0.3 AREA 0
 NETWORK 10.0.0.20 0.0.0.3 AREA 0
 NETWORK 10.0.0.24 0.0.0.3 AREA 0
 NETWORK 192.168.11.0 0.0.0.255 AREA 0
 NETWORK 192.168.21.0 0.0.0.255 AREA 0
 NETWORK 192.168.31.0 0.0.0.255 AREA 0
 PASSIVE-INTERFACE G0/0.10
 PASSIVE-INTERFACE G0/0.20
 PASSIVE-INTERFACE G0/0.30
DO WR MEM
```

### MS1 Configuration
```cisco

```

### SW1 Configuration
```cisco

```

### SW2 Configuration
```cisco

```

### SW3 Configuration
```cisco

```

### SW4 Configuration
```cisco

```

### SW5 Configuration
```cisco

```

### SW6 Configuration
```cisco

```

### SW7 Configuration
```cisco

```
