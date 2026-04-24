# 🔐 1.3.6 Configure SSH — Cisco Packet Tracer Lab

> Secure remote access configuration on a Cisco switch using SSH, RSA key generation, and password encryption.

---

## 📋 Overview

This lab demonstrates how to configure SSH on a Cisco switch (S1) to replace insecure Telnet access. It covers domain name setup, RSA key generation, password encryption, and saving the configuration.

**File:** `1_3_6_Packet_Tracer_-_Configure_SSH.pka`  
**Platform:** Cisco Packet Tracer  
**Device:** Cisco Switch S1

---

## 🖧 Network Topology

![Topology](ScreenShots/Topology.png)

A single PC (**PC1**) is connected directly to switch **S1**.

---

## 🛠️ Configuration Steps

### Step 1 — Set Enable Password

Enter global configuration mode and set the enable password:

```
S1(config)# enable password cisco
```

At this point the password is stored in **plaintext** in the running config:

![Enable Password](ScreenShots/enable-password.png)

---

### Step 2 — Set Domain Name

A domain name is required before RSA keys can be generated:

```
S1(config)# ip domain-name netacad.pka
```

---

### Step 3 — Generate RSA Keys

Generate the RSA crypto keys to enable SSH:

```
S1(config)# crypto key generate rsa
```

When prompted, enter **1024** for the modulus size. The key name will be `S1.netacad.pka`.

![Generate RSA Keys](ScreenShots/encrypt-administartor-byrsa.png)

![RSA Key Confirmation](ScreenShots/encrypt-way.png)

---

### Step 4 — Enable Password Encryption

Encrypt all plaintext passwords stored in the configuration:

```
S1(config)# service password-encryption
```

The enable password is now stored as a **Type 7** encrypted hash instead of plaintext:

![Encrypted Enable Password](ScreenShots/encrypted-enable-password.png)

---

### Step 5 — VTY Line Passwords

**Before** encryption — passwords appear in plaintext:

![VTY Plaintext Password](ScreenShots/line-cons0-pass.png)

**After** `service password-encryption` — all VTY passwords are hashed:

![VTY Encrypted Password](ScreenShots/line-cons0-encryptedpass.png)

---

### Step 6 — Save the Configuration

Persist the configuration so it survives a reboot:

```
S1# copy running-config startup-config
```

Confirm the destination filename when prompted:

![Save Config](ScreenShots/Save-Config.png)

---

## 📌 Key Concepts

| Concept | Detail |
|---|---|
| **SSH requirement** | Domain name + RSA keys must be set before SSH works |
| **RSA key size** | 1024 bits recommended for this lab |
| **Password type 7** | Cisco proprietary encryption — reversible, basic protection |
| **VTY lines** | Lines `0 4` and `5 15` handle remote Telnet/SSH sessions |
| **`service password-encryption`** | Encrypts all plaintext passwords in the running config |
| **`copy run start`** | Saves running config to startup config across reboots |

---

## 📁 Repository Structure

```
.
├── 1_3_6_Packet_Tracer_-_Configure_SSH.pka
├── README.md
└── ScreenShots/
    ├── Topology.png
    ├── enable-password.png
    ├── encrypt-administartor-byrsa.png
    ├── encrypt-way.png
    ├── encrypted-enable-password.png
    ├── line-cons0-pass.png
    ├── line-cons0-encryptedpass.png
    └── Save-Config.png
```

---

## 🚀 Getting Started

1. Open Cisco Packet Tracer
2. Load `1_3_6_Packet_Tracer_-_Configure_SSH.pka`
3. Follow the steps above in order
4. Verify SSH access from PC1 to S1 when complete
