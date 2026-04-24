# 🌐 1.6.1 Implement a Small Network — Cisco Packet Tracer Lab

> Full configuration of a small routed network including a router (RTA), two switches (SW1, SW2), and two PCs — covering hostnames, IP addressing, security hardening, and end-to-end connectivity verification.

---

## 📋 Overview

This lab demonstrates how to build and configure a small network from scratch using Cisco IOS. It covers router and switch initial setup, interface IP addressing, password security, banner messages, and full connectivity testing between all devices.

**File:** `1_6_1_Packet_Tracer_-_Implement_a_Small_Network.pka`  
**Platform:** Cisco Packet Tracer  
**Devices:** Router RTA, Switch SW1, Switch SW2, PC-1, PC-2

---

## 🖧 Network Topology

![Topology](ScreenShots/1777048297596_Topology.png)

| Device | Interface | IP Address | Subnet Mask | Default Gateway |
|--------|-----------|------------|-------------|-----------------|
| RTA | GigabitEthernet 0/0 | 10.10.10.1 | 255.255.255.0 | — |
| RTA | GigabitEthernet 0/1 | 10.10.20.1 | 255.255.255.0 | — |
| SW1 | VLAN 1 | 10.10.10.2 | 255.255.255.0 | 10.10.10.1 |
| SW2 | VLAN 1 | 10.10.20.2 | 255.255.255.0 | 10.10.20.1 |
| PC-1 | NIC | 10.10.10.10 | 255.255.255.0 | 10.10.10.1 |
| PC-2 | NIC | 10.10.20.10 | 255.255.255.0 | 10.10.20.1 |

---

## 🛠️ Configuration Steps

### Step 1 — Set Router Hostname

Enter global configuration mode on the default `Router` and assign the hostname `RTA`:

```
Router> enable
Router# configure terminal
Router(config)# hostname RTA
```

![Set Hostname RTA](ScreenShots/1777048319031_Config-HostName-RTA.png)

---

### Step 2 — Configure RTA Interface for SW1 (GigabitEthernet 0/0)

Assign a description and IP address to the interface facing SW1, then bring it up:

```
RTA(config)# interface gigabitEthernet 0/0
RTA(config-if)# description SW1 LAN
RTA(config-if)# ip address 10.10.10.1 255.255.255.0
RTA(config-if)# no shutdown
```

![RTA Interface for SW1](ScreenShots/1777048319032_Config-IP-Addressing-RTA_forSW1_.png)

![RTA Interface Description](ScreenShots/1777048297591_Config-Description-RTA.png)

---

### Step 3 — Configure RTA Interface for SW2 (GigabitEthernet 0/1)

Assign a description and IP address to the interface facing SW2, then bring it up:

```
RTA(config)# interface gigabitEthernet 0/1
RTA(config-if)# description SW2 LAN
RTA(config-if)# ip address 10.10.20.1 255.255.255.0
RTA(config-if)# no shutdown
```

![RTA Interface for SW2](ScreenShots/1777048319031_Config-IP-Address--Description-RTA_forSW2_.png)

---

### Step 4 — Set Enable Secret on RTA

Secure privileged EXEC mode with an encrypted enable secret:

```
RTA(config)# enable secret Ciscoenpa55
```

![RTA Enable Secret](ScreenShots/1777048297592_Config-EnableSecret-RTA.png)

---

### Step 5 — Configure Console and VTY Lines on RTA

Set passwords on the console and all VTY lines, and require login:

```
RTA(config)# line console 0
RTA(config-line)# password Ciscolinepa55
RTA(config-line)# login
RTA(config-line)# exit

RTA(config)# line vty 0 15
RTA(config-line)# password Ciscolinepa55
RTA(config-line)# login
```

![RTA Lines Configuration](ScreenShots/1777048297594_Config-Lines-RTA.png)

---

### Step 6 — Configure Banner MOTD on RTA

Set a message-of-the-day banner to warn unauthorized users:

```
RTA(config-line)# banner motd &Unauthorized access is strictly prohibited&
```

![RTA Banner MOTD](ScreenShots/1777048297595_Massager-Banner-RTA.png)

---

### Step 7 — Save RTA Configuration

Persist the running configuration to startup so it survives a reboot:

```
RTA# copy running-config startup-config
```

![Save RTA Config](ScreenShots/1777048297596_To-Save-Config-RTA.png)

---

### Step 8 — Verify All RTA Accepted

Confirm the full configuration was accepted without errors:

![RTA Config Accepted](ScreenShots/1777048297596_to-check-all-accept-RTA.png)

---

### Step 9 — Configure SW1 Management Interface & Gateway

Set VLAN 1 IP address and default gateway on SW1 for remote management:

```
Switch> enable
Switch# configure terminal
Switch(config)# interface vlan 1
Switch(config-if)# ip address 10.10.10.2 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit
Switch(config)# ip default-gateway 10.10.10.1
```

![SW1 Gateway Config](ScreenShots/1777048319030_Config-Gateway-S1.png)

---

### Step 10 — Configure SW1 Enable Secret & Line Passwords

Apply the same security configuration to SW1:

```
Switch(config)# enable secret Ciscoenpa55
Switch(config)# line console 0
Switch(config-line)# password Ciscolinepa55
Switch(config-line)# login
Switch(config-line)# line vty 0 15
Switch(config-line)# password Ciscolinepa55
Switch(config-line)# login
```

![SW1 Enable Secret & Lines](ScreenShots/1777048297593_CoNFig-EnableSecret-SW1.png)

---

### Step 11 — Configure SW2 Management Interface & Gateway

Set VLAN 1 IP address and default gateway on SW2:

```
Switch> enable
Switch# configure terminal
Switch(config)# interface vlan 1
Switch(config-if)# ip address 10.10.20.2 255.255.255.0
Switch(config-if)# no shutdown
Switch(config-if)# exit
Switch(config)# ip default-gateway 10.10.20.1
```

![SW2 Gateway Config](ScreenShots/1777048319031_Config-Gateway-S2.png)

---

### Step 12 — Configure SW2 Enable Secret & Line Passwords

Apply the same security configuration to SW2:

```
Switch(config)# enable secret Ciscoenpa55
Switch(config)# line console 0
Switch(config-line)# password Ciscolinepa55
Switch(config-line)# login
Switch(config-line)# line vty 0 15
Switch(config-line)# password Ciscolinepa55
Switch(config-line)# login
```

![SW2 Enable Secret & Lines](ScreenShots/1777048297594_Config-EnableSecret-SW2.png)

---

### Step 13 — Configure PC-1 IP Settings

Set PC-1 to static IP addressing:

| Field | Value |
|-------|-------|
| IPv4 Address | 10.10.10.10 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 10.10.10.1 |

![PC-1 IP Configuration](ScreenShots/1777048297595_IP-Confing-PC1.png)

---

### Step 14 — Configure PC-2 IP Settings

Set PC-2 to static IP addressing:

| Field | Value |
|-------|-------|
| IPv4 Address | 10.10.20.10 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 10.10.20.1 |

![PC-2 IP Configuration](ScreenShots/1777048297595_IP-Config-PC2.png)

---

## ✅ Connectivity Verification

All tests are performed from **PC-2** to verify end-to-end reachability across the network.

---

### Test 1 — PC-2 → PC-1 (10.10.20.10)

Ping PC-1 to verify inter-VLAN routing through RTA:

```
C:\> ping 10.10.20.10
```

![Ping PC-2 to PC-1](ScreenShots/1777048319032_PIng_-PC2_-To-IP-OF-PC1.png)

> ✅ **Result:** 4/4 packets received — 0% loss

---

### Test 2 — PC-2 → Default Gateway of PC-2 (10.10.20.1 / RTA Gi0/1)

Verify PC-2 can reach its own default gateway:

```
C:\> ping 10.10.20.1
```

![Ping PC-2 to its own Gateway](ScreenShots/1777048319032_PIng-PC2-To-Defualt-Getaway-Of-PC2.png)

> ✅ **Result:** 4/4 packets received — 0% loss

---

### Test 3 — PC-2 → Default Gateway of PC-1 (10.10.10.1 / RTA Gi0/0)

Verify PC-2 can reach the far-side interface of RTA:

```
C:\> ping 10.10.10.1
```

![Ping PC-2 to PC-1 Gateway](ScreenShots/1777048319032_PIng-PC2-To-Defualt-Getaway-Of-PC1.png)

> ✅ **Result:** 4/4 packets received — 0% loss

---

### Test 4 — PC-2 → SW1 Management IP (10.10.10.2)

Verify reachability to SW1's management interface across the routed boundary:

```
C:\> ping 10.10.10.2
```

![Ping PC-2 to SW1](ScreenShots/1777048319032_Ping-PC2-To-SW1.png)

> ⚠️ **Result:** 2/4 packets received — 50% loss  
> *Normal — first packets timeout during ARP resolution. Subsequent pings succeed.*

---

### Test 5 — PC-2 → SW2 Management IP (10.10.20.2)

Verify reachability to SW2's local management interface:

```
C:\> ping 10.10.20.2
```

![Ping PC-2 to SW2](ScreenShots/1777048319033_Ping-PC2-To-SW2.png)

> ⚠️ **Result:** 3/4 packets received — 25% loss  
> *Normal — first packet times out during ARP resolution. Subsequent pings succeed.*

---

## 📌 Key Concepts

| Concept | Detail |
|---------|--------|
| **Enable Secret** | `Ciscoenpa55` — encrypted with MD5, stronger than `enable password` |
| **Line Password** | `Ciscolinepa55` — applied to console and VTY lines |
| **Inter-VLAN Routing** | RTA routes between 10.10.10.0/24 and 10.10.20.0/24 |
| **Switch Management** | VLAN 1 SVI used for remote access on both switches |
| **Default Gateway** | Required on switches for management traffic to be routed |
| **Banner MOTD** | Warns unauthorized users before login |
| **ARP Delay** | First ping may fail as devices resolve MAC addresses — expected behavior |
| **`copy run start`** | Saves configuration across reboots |

---

## 📁 Repository Structure

```
.
├── 1_6_1_Packet_Tracer_-_Implement_a_Small_Network.pka
├── README.md
└── ScreenShots/
    ├── Topology.png
    ├── Config-HostName-RTA.png
    ├── Config-IP-Addressing-RTA_forSW1_.png
    ├── Config-Description-RTA.png
    ├── Config-IP-Address--Description-RTA_forSW2_.png
    ├── Config-EnableSecret-RTA.png
    ├── Config-Lines-RTA.png
    ├── Massager-Banner-RTA.png
    ├── To-Save-Config-RTA.png
    ├── to-check-all-accept-RTA.png
    ├── Config-Gateway-S1.png
    ├── CoNFig-EnableSecret-SW1.png
    ├── Config-Gateway-S2.png
    ├── Config-EnableSecret-SW2.png
    ├── IP-Confing-PC1.png
    ├── IP-Config-PC2.png
    ├── PIng_-PC2_-To-IP-OF-PC1.png
    ├── PIng-PC2-To-Defualt-Getaway-Of-PC2.png
    ├── PIng-PC2-To-Defualt-Getaway-Of-PC1.png
    ├── Ping-PC2-To-SW1.png
    └── Ping-PC2-To-SW2.png
```

---

## 🚀 Getting Started

1. Open Cisco Packet Tracer
2. Load `1_6_1_Packet_Tracer_-_Implement_a_Small_Network.pka`
3. Follow the configuration steps above in order
4. Run all ping tests from PC-2 to verify full connectivity
