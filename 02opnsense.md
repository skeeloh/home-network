Purpose:
---
I am installing OPNsense to serve as the router and firewall for my lab network. Running it as a virtual machine in Proxmox lets me reuse the Framework laptop instead of purchasing a dedicated firewall appliance. This gives me a place to practice routing, DHCP, firewall rules, and eventually VLAN segmentation. The Cox router continues serving the household network, while OPNsense will control traffic for the devices connected behind it.

Steps taken:
---
I downloaded the AMD64 OPNsense ISO and uploaded it to Proxmox’s local storage. I created a VM with 2 CPU cores, 4GB of RAM, and a 32GB virtual disk.

I configured two virtual network interfaces. The WAN interface connects to `vmbr0`, which reaches the Cox router through the first USB Ethernet adapter. The LAN interface connects to `vmbr1`, a VLAN-aware bridge attached to the second adapter. This provides separate upstream and lab connections.

After correcting the interface assignments, I installed OPNsense onto its virtual disk and configured a root password. I detached the installer ISO and booted from the installed system. I then reconnected WAN and connected my home PC directly to the second Ethernet adapter, allowing it to act as a physical test device without a switch.

Problems and fixes:
---
The second USB Ethernet adapter initially did not appear in Proxmox. I checked device detection using `lsusb` and `ip -br link`, then confirmed both adapters were recognized before assigning the second adapter to the LAN bridge.

OPNsense initially assigned LAN to `vtnet0` and WAN to `vtnet1`, which was opposite to the intended Proxmox bridge connections. I temporarily disconnected the VM’s upstream network interface to prevent its LAN DHCP service from reaching the household network. Through the OPNsense console, I reassigned WAN to `vtnet0` and LAN to `vtnet1`, then verified the mapping before reconnecting WAN.

The console initially displayed no WAN address after reconnection. Running `ifconfig vtnet0` confirmed that the interface was active and had received an address from the Cox router.

Verification:
---
I confirmed that OPNsense boots with the installer ISO detached and retains the correct WAN and LAN assignments. WAN received an address on the household subnet, while LAN remained on its separate subnet.

After connecting my PC to the LAN adapter, I checked `ipconfig` and confirmed the expected address, subnet mask, and gateway. I also successfully signed in to the OPNsense web interface using the password created during installation.

This verifies installation, interface mapping, LAN addressing, and management access. Completing the setup wizard, testing internet access with Wi-Fi disabled, and configuring and testing isolation rules are the next steps.
