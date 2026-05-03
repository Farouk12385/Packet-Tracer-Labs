# 🔀 4.3.8 Configure Layer 3 Switching and Inter-VLAN Routing — Cisco Packet Tracer Lab

> Inter-VLAN routing using a Multilayer Switch (MLS) with SVIs, a routed uplink to the cloud, and dual-stack IPv4/IPv6 support across VLANs 10, 20, 30, and 99.

---

## 📋 Overview

This lab demonstrates Layer 3 switching as an alternative to Router-on-a-Stick. A Multilayer Switch (MLS) performs inter-VLAN routing entirely in hardware using Switched Virtual Interfaces (SVIs) — one per VLAN. The MLS also has a routed (Layer 3) uplink port to connect to the cloud/internet. Both IPv4 and IPv6 are configured on all SVIs, and IPv6 unicast routing is enabled to support dual-stack end-to-end connectivity.

**File:** `4_3_8_Packet_Tracer_-_Configure_Layer_3_Switching_and_Inter-VLAN.pka`  
**Platform:** Cisco Packet Tracer  
**Devices:** Multilayer Switch (MLS) · Cisco Switches S1, S2, S3 · PC0–PC5 · Cloud

---

## 🖧 Network Topology

![Topology](Topology.png)

| VLAN | Name | IPv4 Gateway (SVI) | IPv6 Gateway (SVI) |
|---|---|---|---|
| 10 | Staff | 192.168.10.254 | 2001:db8:acad:10::1/64 |
| 20 | Student | 192.168.20.254 | 2001:db8:acad:20::1/64 |
| 30 | Faculty | 192.168.30.254 | 2001:db8:acad:30::1/64 |
| 99 | Native | 192.168.99.254 | — |

**Routed Uplink:** MLS G0/2 → Cloud · IPv4: 209.165.200.225/30 · IPv6: 2001:db8:acad:a::1/64

---

## 🛠️ Configuration Steps

### Step 1 — Add VLANs on MLS

Create and name the three data VLANs on the Multilayer Switch:

```
MLS(config)# vlan 10
MLS(config-vlan)# name Staff
MLS(config-vlan)# vlan 20
MLS(config-vlan)# name Student
MLS(config-vlan)# vlan 30
MLS(config-vlan)# name Faculty
```

![Add VLANs on MLS](Add-VLANs-on-MLS.png)

---

### Step 2 — Configure Trunking on MLS

Set MLS G0/1 (uplink to S1) to trunk mode with 802.1Q encapsulation and assign VLAN 99 as the native VLAN:

```
MLS(config)# interface g0/1
MLS(config-if)# switchport mode trunk
MLS(config-if)# switchport trunk encapsulation dot1q
MLS(config-if)# switchport trunk native vlan 99
```

The interface cycles down then back up as it transitions to trunk mode.

![Configure Trunking on MLS](Configure-Trunking-on-MLS.png)

---

### Step 3 — Configure Trunking on S1

Set S1's G0/1 uplink (to MLS) to trunk mode and set native VLAN 99 to match MLS. A CDP native VLAN mismatch warning appears initially — it clears once both ends are set to VLAN 99:

```
S1(config)# interface g0/1
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk native vlan 99
```

![Configure Trunking on S1](Configure-trunking-on-S1.png)

---

### Step 4 — Configure SVIs on MLS (IPv4)

Create a Switched Virtual Interface for each VLAN and assign it an IPv4 gateway address. Each SVI comes up automatically once the VLAN exists and the trunk is active:

```
MLS(config)# interface vlan 20
MLS(config-if)# ip address 192.168.20.254 255.255.255.0

MLS(config)# interface vlan 30
MLS(config-if)# ip address 192.168.30.254 255.255.255.0

MLS(config)# interface vlan 99
MLS(config-if)# ip address 192.168.99.254 255.255.255.0

MLS(config)# interface vlan 10
MLS(config-if)# ip address 192.168.10.254 255.255.255.0
```

![Configure SVI on MLS](Configure-SVI-on-MLS.png)

---

### Step 5 — Configure Layer 3 Switching (Routed Uplink) on MLS

Convert G0/2 from a switchport to a routed Layer 3 interface using `no switchport`, then assign it an IPv4 address for the uplink to the cloud:

```
MLS(config)# interface g0/2
MLS(config-if)# no switchport
MLS(config-if)# ip address 209.165.200.225 255.255.255.252
```

Verify connectivity to the cloud with a ping to 209.165.200.226 (80% success on first attempt while ARP resolves):

```
MLS# ping 209.165.200.226
```

![Configure Layer 3 Switching on MLS](Configure-Layer-3-Switching-on-MLS.png)

---

### Step 6 — Enable IP Routing on MLS

Enable Layer 3 routing so the MLS can route between SVIs and the routed uplink:

```
MLS(config)# ip routing
```

Verify the routing table with `show ip route` — connected routes for all VLAN subnets and the uplink subnet should appear:

![Enable Routing](Enable-routing.png)

---

### Step 7 — Configure SVIs for IPv6 on MLS

Add IPv6 addresses to each VLAN SVI to support dual-stack hosts:

```
MLS(config)# interface vlan 10
MLS(config-if)# ipv6 address 2001:db8:acad:10::1/64

MLS(config)# interface vlan 20
MLS(config-if)# ipv6 address 2001:db8:acad:20::1/64

MLS(config)# interface vlan 30
MLS(config-if)# ipv6 address 2001:db8:acad:30::1/64
```

![Configure SVI for IPv6 on MLS](Configure-SVI-for-IPv6-on-MLS.png)

---

### Step 8 — Configure G0/2 with IPv6 on MLS

Add an IPv6 address to the routed uplink port connecting MLS to the cloud:

```
MLS(config)# interface g0/2
MLS(config-if)# ipv6 address 2001:db8:acad:a::1/64
```

Verify the IPv6 routing table with `show ipv6 route` — connected routes for all VLAN and uplink prefixes should appear:

![Configure G0/2 with IPv6 on MLS](Configure-G0-2-with-IPv6-on-MLS.png)

---

### Step 9 — Enable IPv6 Routing on MLS

Enable IPv6 unicast routing so the MLS can forward IPv6 traffic between SVIs and the uplink:

```
MLS(config)# ipv6 unicast-routing
```

![Enable IPv6 Routing](Enable-IPv6-routing.png)

---

## ✅ Verification

### Verify End-to-End Connectivity

From a PC on VLAN 10, ping another host in the same VLAN (192.168.10.2) and the SVI gateway (192.168.10.254):

```
C:\>ping 192.168.10.2
C:\>ping 192.168.10.254
```

![Verify End-to-End Connectivity](Verify-end-to-end_connectivity.png)

Both pings succeed with **0% packet loss**, confirming that the SVIs are up, Layer 3 routing is active, and hosts can reach their default gateway and each other within the same VLAN.

---

## 📌 Key Concepts

| Concept | Detail |
|---|---|
| **Layer 3 Switch (MLS)** | A switch capable of routing between VLANs in hardware without an external router |
| **SVI (Switched Virtual Interface)** | A virtual Layer 3 interface on a multilayer switch, one per VLAN, acting as the default gateway |
| **`ip routing`** | Enables IPv4 routing between SVIs and routed ports on the MLS |
| **`ipv6 unicast-routing`** | Enables IPv6 routing between SVIs and routed ports on the MLS |
| **`no switchport`** | Converts a switchport to a routed Layer 3 interface for direct IP assignment |
| **Routed uplink** | G0/2 is configured as a Layer 3 port (not a trunk) to connect the MLS directly to the cloud |
| **802.1Q encapsulation** | Required on MLS trunk ports (`switchport trunk encapsulation dot1q`) before setting trunk mode |
| **Dual-stack** | Both IPv4 (192.168.x.254) and IPv6 (2001:db8:acad:x::1) are configured on each SVI |
| **Native VLAN 99** | Set on both MLS G0/1 and S1 G0/1 to prevent native VLAN mismatch warnings |

---

## 📁 Repository Structure

```
.
├── 4_3_8_Packet_Tracer_-_Configure_Layer_3_Switching_and_Inter-VLAN.pka
├── README.md
└── ScreenShots/
    ├── Topology.png
    ├── Add-VLANs-on-MLS.png
    ├── Configure-Trunking-on-MLS.png
    ├── Configure-trunking-on-S1.png
    ├── Configure-SVI-on-MLS.png
    ├── Configure-Layer-3-Switching-on-MLS.png
    ├── Enable-routing.png
    ├── Configure-SVI-for-IPv6-on-MLS.png
    ├── Configure-G0-2-with-IPv6-on-MLS.png
    ├── Enable-IPv6-routing.png
    └── Verify-end-to-end_connectivity.png
```

---

## 🚀 Getting Started

1. Open Cisco Packet Tracer
2. Load `4_3_8_Packet_Tracer_-_Configure_Layer_3_Switching_and_Inter-VLAN.pka`
3. Create VLANs 10, 20, 30 on MLS and configure G0/1 as a trunk with native VLAN 99
4. Mirror the native VLAN 99 trunk setting on S1 G0/1
5. Create SVIs for each VLAN with IPv4 gateway addresses
6. Convert MLS G0/2 to a routed port and assign the uplink IP
7. Enable `ip routing` and verify the IPv4 routing table
8. Add IPv6 addresses to all SVIs and G0/2, then enable `ipv6 unicast-routing`
9. Ping between PCs and to the SVI gateway to confirm end-to-end connectivity
