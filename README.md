# Cybersecurity Lab Setup: VirtualBox and Kali Linux (WK1-PM1)

This repository documents my Week 1 Project Module for NetworkWalks. I built a small cybersecurity practice lab on my own laptop using Fedora Linux, Oracle VirtualBox, and Kali Linux.

The purpose of the lab is to provide a controlled environment for learning and practising ethical hacking. All testing in this lab is intended for systems that I own or have permission to test.

## Project Information

| Item | Details |
|---|---|
| Program | Cybersecurity at NetworkWalks |
| Week and module | Week 01 - Project Module 01 (WK1-PM1) |
| Project | Cybersecurity and Pentesting Lab Setup |
| Author | Shiv Das |
| Batch | B083 |
| LinkedIn | [Shiv Das](https://www.linkedin.com/in/shiv-das-1071b2347) |


## Host Machine Specifications

I completed the setup on the following laptop:

| Component | Details |
|---|---|
| Hardware model | HP Victus by HP Gaming Laptop 15-fa1xxx |
| Host operating system | Fedora Linux 44 (Workstation Edition) |
| Processor | 13th Gen Intel Core i5-13420H |
| Memory | 16.0 GiB (16 GB) |
| Storage | 512.1 GB SSD |
| Graphics | NVIDIA GeForce RTX 3050 6GB Laptop GPU |

## Lab Requirements and Configuration

The important configuration values for this lab are:

| Item | Configuration |
|---|---|
| Hypervisor | Oracle VirtualBox |
| Guest / attack machine | Kali Linux |
| NAT Network | `10.0.0.0/24` |
| Kali static address | `10.0.0.2/24` |
| Gateway | `10.0.0.1` |
| DNS configured for the connection | `8.8.8.8` |
| Clipboard | Bidirectional |
| Drag and drop | Bidirectional |
| Shared folder | Host `Downloads` folder, auto-mounted in Kali |
| Snapshot | Clean baseline snapshot after setup and testing |

The custom NAT Network allows the virtual machines to communicate on the same private virtual network while VirtualBox provides outbound network address translation. This is different from using ordinary NAT for each VM separately.

## Step-by-Step Setup

### Step 1: Install 7-Zip and extract the Kali image

I used Fedora's terminal to install the 7-Zip utilities. I then extracted the Kali Linux VirtualBox archive so that the virtual appliance could be imported into VirtualBox.

![Installing 7-Zip on Fedora](Screenshots/Screenshot%20From%202026-09-09%2016-25-24.png)

*Figure 1: Installing the 7-Zip utilities on Fedora Linux.*

![Extracting the Kali Linux archive](Screenshots/Screenshot%20From%202026-09-09%2016-25-59.png)

*Figure 2: Extracting the Kali Linux archive from the Fedora terminal.*

### Step 2: Create the NAT Network

After opening VirtualBox, I created a custom NAT Network with the prefix `10.0.0.0/24`. DHCP was enabled in the VirtualBox network settings.

![NAT Network configuration](Screenshots/Screenshot%20From%202026-09-09%2016-36-28.png)

*Figure 3: Creating the VirtualBox NAT Network with the `10.0.0.0/24` prefix.*

### Step 3: Import the extracted Kali Linux appliance

I imported the extracted Kali Linux virtual appliance into VirtualBox using the appliance import option. This added the pre-built Kali virtual machine to VirtualBox Manager so that I could configure its resources and network adapter.

The screenshots in this repository focus on the configuration after the import rather than the import progress window.

### Step 4: Attach Kali Linux to the NAT Network

In the Kali VM's Network settings, I attached Adapter 1 to the custom `NATNetwork`. This connects Kali to the private `10.0.0.0/24` virtual network.

![Kali network adapter attached to NATNetwork](Screenshots/Screenshot%20From%202026-09-09%2016-38-47.png)

*Figure 4: Attaching the Kali Linux network adapter to the custom NAT Network.*

### Step 5: Configure the VM resources

I reviewed the Kali VM settings and allocated resources suitable for my laptop: 4 GB of RAM and 2 virtual CPUs. I also checked the display and hardware acceleration settings so that the VM would run smoothly.

![Kali VM settings](Screenshots/Screenshot%20From%202026-09-09%2016-39-52.png)

*Figure 5: Kali Linux virtual machine settings and resource allocation.*

### Step 6: Enable bidirectional clipboard and drag and drop

In the VirtualBox General → Advanced settings, I set Shared Clipboard and Drag and Drop to **Bidirectional**. This makes it easier to transfer commands and small pieces of text between Fedora and Kali while working in the lab.

![Bidirectional clipboard and drag and drop](Screenshots/Screenshot%20From%202026-09-09%2017-41-25.png)

*Figure 6: Shared Clipboard and Drag and Drop configured as Bidirectional.*

### Step 7: Configure the Kali static IP

Inside Kali Linux, I configured the wired connection with the following IPv4 values:

```text
Address: 10.0.0.2/24
Gateway: 10.0.0.1
DNS configured for the connection: 8.8.8.8
```

### Step 8: Configure the shared folder

I added my host's `Downloads` folder as a VirtualBox shared folder and enabled auto-mount. This allows me to move permitted lab files between the Fedora host and Kali VM without repeatedly configuring a new transfer method.

![Shared folder configuration](Screenshots/Screenshot%20From%202026-09-09%2017-46-11.png)

*Figure 8: The host Downloads folder configured as a VirtualBox shared folder.*

### Step 9: Verify the shared folder inside Kali

After starting Kali, I checked that the shared folder was mounted and that a file could be viewed from inside the VM. This confirmed that the host-to-guest file-sharing configuration was working.

![Shared folder verification](Screenshots/Screenshot_2026-09-09_08_03_53.png)

*Figure 9: The shared folder and its file visible inside Kali Linux.*

### Step 10: Verify networking and security tools

I used the Kali terminal to check the address, routing table, gateway connectivity, Internet access, DNS resolution, and the availability of Nmap:

```bash
ip addr show
ip route show
ping -c 4 10.0.0.1
ping -c 4 google.com
nmap --version
nslookup networkwalks.com
```

The `ping -c 4 google.com` test confirmed both outbound connectivity and name resolution. The Nmap command returned version 7.99, confirming that Nmap was installed. The `nslookup` command resolved `networkwalks.com` successfully. In my output, the active resolver reported by `nslookup` was `192.168.122.1`; the important result for this check was that the DNS query completed successfully.

```text
Nmap version 7.99 ( https://nmap.org )

Server:         192.168.122.1
Address:        192.168.122.1#53

Non-authoritative answer:
Name:   networkwalks.com
Address: 192.232.216.135
```

![Network verification tests](Screenshots/Screenshot_2026-09-09_07_42_44.png)

*Figure 10: Kali terminal output showing the IP, route, and connectivity checks.*

### Step 11: Take a clean snapshot

Once the configuration and checks were complete, I created a clean VirtualBox snapshot. This gives me a known-good restore point before I begin future security exercises or make changes to the VM.

![Clean Kali snapshot](Screenshots/Screenshot%20From%202026-09-09%2017-52-41.png)

*Figure 11: Clean baseline snapshot of the Kali Linux VM.*

## Troubleshooting

### Kali initially had an Internet connection problem

After configuring the static address, the Kali VM did not have Internet access even though the local virtual network was configured. I restarted the NetworkManager connection and then applied the DAD timeout setting that was provided for this VirtualBox/Kali setup:

```bash
# Restart the connection as a temporary fix
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"

# Apply the setting so the problem does not return after restarting
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

After restarting the connection, I was able to complete the gateway and Internet checks. This was the most useful troubleshooting part of the lab because it made me work through the network configuration instead of only following the setup steps.

## What I Learned

This project gave me my first complete walkthrough of building a small virtual cybersecurity lab. My main takeaways were:

### NAT and NAT Network are not the same

At first, I thought NAT and NAT Network were just two names for the same setting. I learned that a normal NAT connection gives a VM outbound access independently, while a NAT Network creates a shared virtual subnet. That shared subnet is more useful when several practice machines need to communicate with one another.

### Virtual network settings affect everything else

The Kali VM could not communicate correctly until the VirtualBox adapter, NAT Network, IP address, gateway, and DNS settings all matched. I learned to check those layers one at a time instead of changing several settings at once.

### Static IP configuration needs verification

I learned how to set a static IPv4 address and how to confirm it with `ip addr` and `ip route`. The ping and `nslookup` checks also showed me that having an IP address is not enough; the route and name resolution must work as well.

### Snapshots make experimentation safer

Taking a clean snapshot before starting security exercises gives me a known-good point to return to. That is especially useful in a lab where future tools or configuration changes may break the VM.

### Documentation is part of the technical work

Writing down the commands, settings, screenshots, and the Internet connectivity problem made the setup easier to review. It also gives me a reproducible record that I can use when I build another lab machine.

## Security and Ethical Use

This laboratory is for education and authorized ethical hacking practice only. I will use the tools and techniques in this repository only against systems that I own or have explicit permission to test.

## Tools and Resources

- [7-Zip official downloads](https://7-zip.org/download.html)
- [Oracle VirtualBox downloads](https://virtualbox.org/wiki/Downloads)
- [Kali Linux official downloads](https://kali.org/get-kali)
- [Fedora Linux](https://fedoraproject.org/)
- [NetworkWalks Academy](https://www.networkwalks.com)

## Author

**Shiv Das**  
Cybersecurity student, Batch B083  
[LinkedIn profile](https://www.linkedin.com/in/shiv-das-1071b2347) 

---

*Created as part of NetworkWalks Cybersecurity Training - WK1-PM1.*
