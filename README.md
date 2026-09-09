Cybersecurity Lab Setup: VirtualBox & Kali Linux
Hi everyone! This repository documents my journey setting up a cybersecurity testing lab on my personal laptop. I created it as part of my Week 1 Project Module for ethical hacking practice.
My Setup
I'm running this lab on my main laptop. Here are my specs so you know what hardware I'm working with:
·	Machine: HP Victus Gaming Laptop (Pashupatastra)
·	Operating System: Fedora Linux 44
·	Processor: 13th Gen Intel Core i5-13420H
·	RAM: 16 GB
·	Storage: 512 GB NVMe SSD
·	Graphics: NVIDIA RTX 3050 (6 GB)
The Goal
The objective was to create a safe, isolated environment where I could practice penetration testing. To do this, I needed to:
1.	Install VirtualBox as the hypervisor.
2.	Set up a Kali Linux virtual machine to act as the primary attacking machine.
3.	Configure an isolated NAT Network in the 10.0.0.0/24 subnet.
4.	Set a static IP (10.0.0.2) on the Kali machine and ensure it could reach the internet safely.
How I Did It
I started by getting VirtualBox up and running on my Fedora system. Since I already had VT-x virtualization enabled in my BIOS, the installation went pretty smoothly.
Next, I opened VirtualBox manager and created a new NAT Network. I assigned it the network prefix 10.0.0.0/24 and made sure DHCP was enabled. This step is crucial because it keeps the VM isolated from my home network while still allowing it to talk to the internet.
After setting up the network, I grabbed the latest VirtualBox image of Kali Linux directly from their official website. I imported the .ova file, and before booting it up, I went into the VirtualBox settings to connect the VM's network adapter strictly to the NAT Network I just created.


Once Kali was booted, I had to configure the network from the terminal. I used the nmcli command-line tool to set the static IP address to 10.0.0.2 and point the DNS to 8.8.8.8 so I could browse the web.
The Problems I Ran Into (And How I Fixed Them)
The setup wasn't entirely perfect on the first try. The main roadblock I hit was the internet connection inside Kali.
The Internet Connectivity Issue:

Right after I set my static IP and applied the settings, I couldn't ping google.com. The gateway (10.0.0.1) was reachable, but there was absolutely zero internet access. After some troubleshooting, I found out this is a known quirk with newer versions of Kali Linux running on VirtualBox v7. NetworkManager runs into an IPv4 Duplicate Address Detection (DAD) conflict that basically hangs the routing on static NATNetwork connections.
Here is how I fixed it. First, I ran a quick temporary fix just by cycling the connection:
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"

To make sure it didn't happen again on every reboot, I applied a permanent fix by disabling the DAD timeout entirely:
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"

After running those commands, ping 8.8.8.8 started working perfectly!
The Performance Issue:

I also noticed Kali was a little bit laggy when I first started using it. The default VM profile only gave it 2GB of RAM and a single CPU core. Since my laptop has 16GB of RAM and a fast processor, I went back into the VirtualBox system settings, bumped the VM up to 4GB of RAM and 2 virtual CPUs, and turned on 3D acceleration. It ran incredibly smoothly after that.
Verification
To prove everything was working, I ran a few tests in the terminal:
·	ip addr show confirmed my IP was 10.0.0.2/24
·	ping 10.0.0.1 confirmed I could reach the VirtualBox gateway
·	ping 8.8.8.8 confirmed I had outside internet access


Final Thoughts
Setting this up taught me a lot about how virtual networking works behind the scenes. Honestly, fixing the internet bug was the best part of the lab because it forced me to dig into nmcli and understand what the network adapter was actually doing.
I'm feeling really good about this setup and I'm ready to start using it for the upcoming ethical hacking modules!
