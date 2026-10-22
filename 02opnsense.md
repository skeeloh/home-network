Purpose:
I am configuring OPNsense to manage traffic between my home network and the devices connected behind my virtual firewall. Hosting it in Proxmox allows me to use the laptop I already repurposed instead of purchasing a separate firewall. For now, my everyday PC is connected directly to the second Ethernet adapter. The goal at this stage is to get normal internet access working through OPNsense before adding services such as AdGuard Home and separate networks for testing.
Steps taken:
With OPNsense installed and its web interface accessible, I completed the setup wizard. This included setting the hostname, domain, time zone, DNS settings, and administrator password. I kept the WAN connection on DHCP so it receives an address from my existing router. On the LAN side, I kept a separate subnet and enabled DHCP so OPNsense can automatically assign addresses to connected devices. I also enabled the Unbound DNS resolver to handle DNS requests.

Since OPNsense sits behind my existing home router, I unchecked the WAN setting that blocks private networks. Private addresses are expected on this connection. I then installed the available OPNsense updates and rebooted the firewall.

For the virtual machine configuration, I confirmed that hardware checksum offloading, TCP segmentation offloading, and large receive offloading were disabled. I also disabled VLAN hardware filtering and turned off IPv6 to keep this initial setup focused on IPv4. After applying these settings, I rebooted OPNsense again.

I reviewed the firewall rules and kept the default IPv4 rule allowing devices on the LAN to initiate connections to other destinations. I considered blocking access to the household network, but my everyday PC still needs access to those resources and Proxmox management. I will add restrictions when I create separate networks for experimental VMs and guest devices.
Problems and fixes:
One concern was the amount of RAM allocated to OPNsense. Proxmox had shown high memory usage, which made me question whether 4 GB was enough. Checking inside OPNsense showed approximately 18% usage at that point, so I kept the existing allocation. This was a useful reminder to check memory usage inside the VM before assuming it needs more resources.

Another point to work through was how restrictive the firewall rules should be. Blocking the entire household subnet would also block access to devices I still want to use. I kept the current LAN trusted for now. Having a separate subnet gives me a place to build out the network, but it does not automatically isolate experimental devices from my PC or household resources.

No additional connectivity issue showed up during this stage. OPNsense is currently behind my home router, so the setup uses double NAT. Normal browsing works, while automatic device discovery or services needing incoming connections may require additional configuration later.
Verification:
Verifying this step was straightforward. I confirmed that the OPNsense web interface was accessible after rebooting and that the WAN and LAN had the expected addressing. I then turned off Wi-Fi on my PC so the test would use the Ethernet connection through OPNsense. Websites loaded, and a speed test reached my expected full connection speed.

Basic routing is working, and the current LAN rule supports the access I want for my everyday PC. A fresh configuration backup and automatic VM startup still need to be confirmed. The next step is installing AdGuard Home and testing DNS filtering before directing devices to use it.
