---
title: "Configuring an HP/Aruba Switch"
date: 2021-07-27T18:25:55-05:00
draft: false
---

# Introduction

HP and Aruba switches that run on the ArubaOS platform are fairly simple to configure and setup if you are a little familiar with the Cisco IOS/IOS-XE style of doing things.  It is worth noting that HP/Aruba are phasing out the ArubaOS in favor of the ArubaCX platform.  This article does not cover ArubaCX which is very different.

Here are a couple of typical tasks that may need when configuring a switch.

## Saving Configuration
`write memory`

## Enter Configuration Mode
`configure`

## Enter Interface Configuration Mode
This is done from Configuration Mode.

`configure interface`

## Set logging desination
`logging <ip address>`

## Setup DNS
```
ip dns domain-name "wirchbench.local"
ip dns server-address priority 1 <ip address>
ip dns server-address priority 2 <ip address>
```

## Setup SNTP and time
```
sntp unicast
sntp server priority 1 <ip address/FQDN>
sntp server priority 2 <ip address/FQDN>
time daylight-time-rule continental-us-and-canada
time timezone -360
```

## Setup a management default gateway

If your ssh/snmp traffic needs to take a different gateway, you can use this command to designate that default gateway.  This is not the same as you setting a default gateway via `ip route`.

`ip default-gateway <ip address>`

## VLANs

VLANs on HP/Aruba are treated very differently than Cisco.  They use a tagged/untagged model.  In Cisco switches, you create vlan from global configuration mode and then you tell the interface which vlan it is in if it is a access port.  If it is trunk port, you say what vlans are allowed (defaults to all.)  As shown below.

```
# Create VLANs
vlan 100
 name my-vlan

vlan 101
 name native-vlan

# Access Port
interface Gigabitethernet0/1
 switchport mode access
 switchport access vlan 100

# Trunk Port with a Native (untagged vlan) of 101
interface Gigabitethernet0/2
 switchport mode trunk
 switchport trunk native vlan 101
 switchport trunk allowed vlan 100
```
On HP/Aruba Switches you create the VLAN and then tell the VLAN what ports you will tag traffic, and what ports you will NOT tag traffic, like a native VLAN.

Note:  HP/Aruba just number their ports (1-48) instead of designating the port speed in the interface name.

```
# Create VLANs
vlan 100
 name "my-vlan"
 tagged 1-2,4
 untagged 3

vlan 101
 name "native-vlan"
 untagged 1-2,4
```
In the configuration above, ports 1,2, and 4 will accept tagged traffic on vlan 100 but anything not tagged on port 3 will be put onto vlan 100 as it is the untagged (native) vlan on that port.  Untagged (native) traffic on vlan 1,2, and 4 will go on blan 101.
