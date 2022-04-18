---
title: "Upgrading A10 ACOS with VRRP and no aVCS"
date: 2022-04-18
tags: [
    "Networking",
]
draft: false
---

# Backup Configurations and Show Tech
```
backup system use-mgmt-port ftp://<username>@<ipaddress>/<hostname>-<date>-backup.tgz
show techsupport export use-mgmt-port ftp://<username>@1<ipaddress>/<hostname>-<date>-tech.tgz
```

# Collect information if needed later, have this recorded
```
show bootimage
show interface brief
show mac-address-table
show ip route
show arp
```
# Change VRRP-A to make the node secondary (optional)
Check the current VRRP State
```
show vrrp-a
```
Failover VRRP to the other node before starting maintenance on this node if needed.  Default VRRP-A Priority is 150.  Change is instant.
```
vrrp-a vrid 0 
  blade-parameters 
    priority <number lower than whatever the peer is>
```

# Upgrade the SSD to have the updated image.
A10 Devices have two SSDs in them to boot from.  You can have the same image on both drives but duing the upgrade process it can be wise to have the old image hanging on a drive incase of an upgrade failure so you can easily revert back.  You can use the following command to view the image installed on each SSD.
```
show bootimage
```
You may need to set the startup configuration to use whatever drive you upgraded.  Use the following command to do so:
```
link startup-config <profile-name or default> <primary or secondary>
```
Use the commmand below to upgrade either drive.
```
upgrade hd <pri or sec> use-mgmt-port ftp://<username>@<ipaddress>/<filename>.upg
```
At the end of the upgrade proceedure you will be prompted to reboot your device.  You can answer yes or manually reboot later.

# Verify successful upgrade
```
show version
show bootimage
show start all
```
Because aVCS is not configured.  You will need to repeat the steps on the secondary A10.  aVCS auto updates if you decide to start setting it up.


# Change VRRP-A to make the node primary (optional)
Check the current VRRP State
```
show vrrp-a
```
Failover VRRP to the other node before starting maintenance on this node if needed.  Default VRRP-A Priority is 150.  Change is instant.
```
vrrp-a vrid 0 
  blade-parameters 
    priority <number higher than whatever the peer is>
```
---
<font size="1">A more in depth video that does a great job showing the process can be [found here](https://www.youtube.com/watch?v=kNMEpT9zqmA) but does include aVCS.</font>