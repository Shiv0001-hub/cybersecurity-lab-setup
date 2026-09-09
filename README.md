# Cybersecurity Lab Setup: VirtualBox & Kali Linux (WK1-PM1)

Welcome to my cybersecurity lab setup documentation! This repository covers the complete step-by-step setup of an isolated ethical hacking lab environment built on my personal laptop as part of the NetworkWalks Academy Week 1 Project Module.

---

## 📌 Project Information

| Field | Details |
|---|---|
| **Program Name** | Cybersecurity at Networkwalks |
| **Week / Module** | Week 01 - Project Module 01 (WK1-PM1) |
| **Project Title** | Cybersecurity & Pentesting Lab Setup |
| **Author** | **Shiv Das** (Batch B083) |

---

## 💻 Host Machine Specifications

I built and tested this lab on my personal laptop. Here are the exact hardware and OS specifications:

| Component | Details |
|---|---|
| **Device Name** | Pashupatastra |
| **Model** | HP Victus Gaming Laptop |
| **Host OS** | Fedora Linux 44 (Workstation Edition) |
| **Processor** | 13th Gen Intel® Core™ i5-13420H |
| **Memory (RAM)** | 16 GB |
| **Storage** | 512 GB NVMe SSD |
| **GPU** | NVIDIA GeForce RTX 3050 (6 GB) |
| **Virtualization** | Intel VT-x (Enabled in BIOS) |

---

## 🎯 Lab Objectives

The main objective of this lab is to create a segmented, isolated testing subnet where I can practice penetration testing safely without exposing my local home network or host machine.

### Key Requirements (WK1-PM1):
* **Hypervisor:** Oracle VirtualBox.
* **Attack Machine:** Kali Linux (configured with static IP `10.0.0.2/24`).
* **Subnet:** Custom VirtualBox NATNetwork (`10.0.0.0/24`).
* **Gateway:** `10.0.0.1` with DNS set to `8.8.8.8`.
* **VM Features:** Bidirectional shared clipboard & drag-and-drop enabled.
* **Host Integration:** Shared folder mapping host `/Downloads` to the VM.
* **Safety:** Snapshot taken after clean configuration for instant rollback.

---

## 🛠️ Step-by-Step Lab Setup & Screenshots

### Step 1: Host Preparation & Extracting Kali Linux
On Fedora Linux, I first made sure virtualization was active and installed `p7zip` to handle compressed archives. Then I downloaded the official Kali Linux VirtualBox image and extracted the `.7z` file using terminal commands.

![Installing 7zip on Fedora](Screenshots/Screenshot%20From%202026-09-09%2016-25-24.png)
*Figure 1: Installing p7zip utilities on Fedora Linux via terminal.*

![Extracting Kali Linux Archive](Screenshots/Screenshot%20From%202026-09-09%2016-25-59.png)
*Figure 2: Extracting the Kali Linux VirtualBox appliance using the terminal.*

---

### Step 2: Creating the NAT Network in VirtualBox
To ensure network isolation while allowing outbound internet access, I opened VirtualBox Global Tools → Network and created a custom **NAT Network** with prefix `10.0.0.0/24` and DHCP enabled.

![NAT Network Configuration](Screenshots/Screenshot%20From%202026-09-09%2016-36-28.png)
*Figure 3: VirtualBox NAT Network creation with 10.0.0.0/24 subnet prefix.*

---

### Step 3: Importing Extracted Kali Linux in VirtualBox
With the NATNetwork created, I imported the extracted Kali Linux virtual appliance into VirtualBox (`File → Import Appliance` or adding the extracted `.vbox` machine). This loaded the pre-built Kali Linux virtual machine into VirtualBox Manager, ready for hardware and network configuration.

---

### Step 4: Attaching Kali VM to the NAT Network
Next, I went into the imported Kali Linux VM settings and configured Network Adapter 1 to attach directly to `NATNetwork` with promiscuous mode set properly.

![Network Adapter Attached to NATNetwork](Screenshots/Screenshot%20From%202026-09-09%2016-38-47.png)
*Figure 4: Attaching Kali VM's network adapter to the custom NATNetwork.*

---

### Step 5: Allocating System Resources & VM Settings
To ensure Kali runs smoothly on my 16GB RAM laptop, I reviewed the VM settings, allocated 4GB of RAM, 2 vCPUs, and enabled hardware acceleration.

![Kali VM System Settings](Screenshots/Screenshot%20From%202026-09-09%2016-39-52.png)
*Figure 5: Kali Linux VM resource settings and hardware allocation.*

---

### Step 6: Enabling Bidirectional Clipboard & Drag'n'Drop
Under **General → Advanced**, I enabled both **Shared Clipboard** and **Drag'n'Drop** in **Bidirectional** mode so I can easily move commands, links, and snippets between my Fedora host and Kali Linux.

![Bidirectional Clipboard Setup](Screenshots/Screenshot%20From%202026-09-09%2017-41-25.png)
*Figure 6: Setting Shared Clipboard and Drag'n'Drop to Bidirectional.*

---

### Step 7: Setting Static IP & NetworkManager Configuration
Inside Kali Linux, I configured the wired connection with a static IP of `10.0.0.2`, netmask `24` (`255.255.255.0`), gateway `10.0.0.1`, and DNS `8.8.8.8`.

![Kali Static IP Configuration]
*Figure 7: Static IP address and DNS configuration inside Kali Linux.*

---

### Step 8: Configuring the Shared Folder
To easily transfer tools, scripts, and wordlists from my host machine into the VM, I set up a permanent Shared Folder pointing to my host's `Downloads` folder with Auto-Mount enabled.

![Shared Folder Configuration in VirtualBox](Screenshots/Screenshot%20From%202026-09-09%2017-46-11.png)
*Figure 8: Mapping host Downloads directory as an auto-mounted shared folder.*

---

### Step 9: Shared Folder Working Verification
I verified that the shared folder was successfully mounted and accessible inside Kali's file manager and terminal, allowing smooth file exchange between my Fedora host and Kali VM.

![Shared Folder Working in Kali](Screenshots/Screenshot_2026-09-09_08_03_53.png)
*Figure 9: Accessing and verifying the shared folder files from within Kali Linux.*

---

### Step 10: Network Connectivity & Tool Verification
I opened the Kali root terminal to thoroughly test network connectivity, DNS resolution, and security tool readiness:

1. `ip addr show` — Confirmed the static IP `10.0.0.2/24` on `eth0`.
2. `ip route show` — Confirmed the default route via `10.0.0.1`.
3. `ping -c 4 10.0.0.1` — Confirmed local gateway connectivity.
4. `ping -c 4 google.com` — Confirmed active outbound internet access and valid DNS resolution.
5. `nmap --version` — Confirmed Nmap scanner is installed and operational.
6. `nslookup networkwalks.com` — Confirmed DNS lookups resolve external domain names properly.

```bash
┌──(root㉿kali)-[~]
└─# nmap --version                                   
Nmap version 7.99 ( https://nmap.org )
Platform: x86_64-pc-linux-gnu
Compiled with: liblua-5.4.8 openssl-3.6.3 libssh2-1.11.1 libz-1.3.1 libpcre2-10.46 libpcap-1.10.6 nmap-libdnet-1.18.0 ipv6

┌──(root㉿kali)-[~]
└─# nslookup networkwalks.com    
Server:         192.168.122.1
Address:        192.168.122.1#53

Non-authoritative answer:
Name:   networkwalks.com
Address: 192.232.216.135
```

![Terminal Verification Tests](Screenshots/Screenshot_2026-09-09_07_42_44.png)
*Figure 10: Terminal output showing valid IP, default gateway route, and successful ping tests.*

---

### Step 11: Taking a Clean VM Snapshot
After completing and verifying all configurations, I took a clean snapshot titled `Snapshot 1` (Base Clean Setup). This allows me to roll back in seconds if any future experiment breaks the environment.

![Taking VM Snapshot](Screenshots/Screenshot%20From%202026-09-09%2017-52-41.png)
*Figure 11: Clean state VirtualBox snapshot for easy recovery.*

---

## ⚡ Challenges & Troubleshooting

### Issue: Kali Linux Internet Connectivity (VirtualBox v7 DAD Conflict)
* **Symptom:** After setting the static IP `10.0.0.2`, Kali could ping the local gateway (`10.0.0.1`) but could not reach external IPs (`8.8.8.8` or domain names).
* **Cause:** NetworkManager in newer Kali Linux releases experiences an IPv4 Duplicate Address Detection (DAD) delay/conflict when initializing static network connections inside VirtualBox NATNetworks.
* **Resolution:**
  ```bash
  # Permanent fix: Disable DAD timeout on the connection
  sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
  
  # Restart interface to apply changes immediately
  sudo nmcli connection down "Wired connection 1"
  sudo nmcli connection up "Wired connection 1"
  ```
  Once applied, internet access was restored immediately.

---

## 🎓 What I Learned

Through this project, I learned how to create and configure a virtual environment for cybersecurity practice. The most important concepts I learned include:

### 1. NAT vs NAT Network
A standard NAT configuration and a NAT Network serve different purposes. A NAT Network allows multiple VMs connected to the same virtual network to communicate with one another while providing network address translation for external connectivity. This makes it ideal for building a multi-machine cybersecurity laboratory.

### 2. Virtual Machine Networking
I learned how VirtualBox virtual network adapters connect virtual machines to different types of networks (NAT, NAT Network, Host-Only, Bridged, and Internal) and how network configuration affects communication between machines.

### 3. Static IP Configuration & Network Management
I learned how to configure and verify IPv4 addressing, subnet masks, default gateways, and DNS settings in Kali Linux using command-line utilities like `nmcli` and `ip`.

### 4. VM Snapshots & Lab Safety
I learned that a clean snapshot should always be created before performing risky or experimental activities. This provides a known-good recovery point for future cybersecurity exercises.

### 5. Professional Documentation
I learned that documenting commands, system configurations, visual screenshots, problems encountered, and resolutions is an essential skill for any professional cybersecurity engineer.

---

## 🔐 Security & Ethical Use

> **Disclaimer:** This laboratory environment is designed and intended strictly for **educational and authorized ethical hacking practice only**. All testing and tools should only be used in controlled environments with explicit permission.

---

## 🔗 Tools & Resources

* [7-Zip Official Downloads](https://7-zip.org/download.html)
* [Oracle VirtualBox Downloads](https://virtualbox.org/wiki/Downloads)
* [Kali Linux Official Images](https://kali.org/get-kali)
* [Fedora Linux](https://fedoraproject.org/)
* [NetworkWalks Academy](https://www.networkwalks.com)

---

## 👤 Author

**Shiv Das**  
*Cybersecurity Enthusiast & Practitioner (Batch B083)*  
* **LinkedIn:** [linkedin.com/in/shiv-das-1071b2347](https://www.linkedin.com/in/shiv-das-1071b2347)

---
*Created as part of NetworkWalks Cybersecurity Training | Module WK1-PM1*
