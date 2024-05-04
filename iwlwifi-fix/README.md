# Fix for systems which use the Intel AX210 and suffer an iwlwifi crash where all resources are consumed by iwlwifi and the journalctl output contains 'Queue X is stuck'
This works for ASUS ROG STRIX X670E-E Gaming Wifi motherboard and Fedora 40. Has not been tested anywhere else. YMMV

This unit file implements a fix provided in the following Intel community forum post:
- https://community.intel.com/t5/Wireless/AX210-System-Crash-after-iwlwifi-Queue-Stuck-Error-Issue/m-p/1562147

# THIS WILL NOT WORK OUT OF THE BOX!!! The ExecStart command needs to be configured per-system.
To obtain the correct PCI device use the lspci command and look for 'Intel Corporation Wi-Fi 6E(802.11ax) AX210' or run the following command (assumes no other reference to AX210 in lspci):
```
lspci | grep AX210 | cut -d' ' -f1
```

## Service file content without comments - as works on my system
```
[Unit]
Description=Fix for iwlwifi locking up computer
Requires=network-online.target

[Service]
ExecStart=/usr/sbin/setpci -s 09:00.0 0x50.B=0x40
Type=simple

[Install]
WantedBy=graphical.target
```
