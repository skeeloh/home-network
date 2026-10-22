Purpose:
---
I am installing Proxmox to avoid the expense of purchasing new hardware where applicable. The purpose of Proxmox in this case is to host multiple virtual machines on this single repurposed device. This software is cool because it utilizes KVM to provide each VM with its own OS. Utilizing it, I can run various tools without stressing about compatibility, I.E., OPNsense or AdGuard, which will be added and explained in later renditions. By each instance being "Hosted Separately", I can also manage each service as an individual and remedy them as such. 

Steps taken:
---
The install is simple; I utilized Rufus to create a bootable drive of Proxmox. I wiped the laptop clean, thankfully did not need anything on it, and flashed the new OS.

Problems and fixes:
---
A small error on my part: I initially downloaded the ARM64 installer instead of the compatible AMD64. After this correction, I also had an issue with address assignment to the Proxmox instance. More specifically, forcing a bridge connection reference to the USB-C Ethernet adapter. Troubleshooting showed that the management IP and gateway were correct, but the bridge configuration referenced `nic0` instead of the USB Ethernet adapter’s actual interface name. I backed up `/etc/network/interfaces`, corrected the interface declaration and bridge reference, and applied the changes with `ifreload -a`. This restored access to the Proxmox web interface.

I also encountered an issue where closing the laptop lid suspended Proxmox and made the web interface unreachable. Reviewing the system logs confirmed that lid closure triggered suspension. The intended configuration file was missing because an extra space in the command prevented it from being saved at the correct path. I corrected this by creating `/etc/systemd/logind.conf.d/lid.conf` and setting the lid-switch actions to `ignore`. The remaining verification is to reboot and confirm that the web interface stays accessible with the lid closed.

Verification:
---
Verifying this step was straightforward. At this point, we are just checking for operability by remotely opening a shell, restarting the device, and seeing if it keeps functioning while the laptop lid is closed. All these boxes are checked after a few fixes, so I am good to continue. 
