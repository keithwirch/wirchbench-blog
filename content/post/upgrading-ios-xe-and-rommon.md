---
title: "Upgrading IOS-XE and ROMMON"
date: 2022-04-02
tags: [
    "Networking",
    "Cisco",
]
draft: false
---

# Verify ROMMON Version

```
Wirchbench#show rom-monitor r0

System Bootstrap, Version 15.4(3r)S3, RELEASE SOFTWARE
Copyright (c) 1994-2014 by cisco Systems, Inc.

Wirchbench#sh platform
Chassis type: ISR4321/K9

Slot Type State Insert time (ago)
--------- ------------------- --------------------- -----------------
0 ISR4321/K9 ok 01:26:51
0/0 ISR4321-2x1GE ok 01:25:41
R0 ISR4321/K9 ok, active 01:26:51
F0 ISR4321/K9 ok, active 01:26:51
P0 PWR-4320-AC ok 01:26:34
P2 ACS-4320-FANASSY ok 01:26:34

Slot CPLD Version Firmware Version
--------- ------------------- ---------------------------------------
0 15030325 15.4(3r)S3
R0 15030325 15.4(3r)S3
F0 15030325 15.4(3r)S3
```

# Copy the ROMMON file and the software Image to Flash
```
Wirchbench#copy ftp://<username>:<password>@<IP Address>/isr4300-rommon.162-1r.pkg flash:
Destination filename [isr4300-rommon.162-1r.pkg]?
Accessing ftp://*:*@172.16.32.40/isr4300-rommon.162-1r.pkg...
Loading isr4300-rommon.162-1r.pkg !!!!!!!!!!!
[OK - 2646988/4096 bytes]

2646988 bytes copied in 1.295 secs (2044006 bytes/sec)

Wirchbench#copy ftp://<username>:<password>@<IP Address>/isr4300-universalk9.16.02.01.SPA.bin flash:
Destination filename [isr4300-universalk9.16.02.01.SPA.bin]?
Accessing ftp://*:*@172.16.32.40/isr4300-universalk9.16.02.01.SPA.bin...
Loading isr4300-universalk9.16.02.01.SPA.bin !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
[OK - 497494797/4096 bytes]

497494797 bytes copied in 199.583 secs (2492671 bytes/sec)
```
# Perform ROMMON Upgrade

```
Wirchbench#upgrade rom-monitor filename bootflash:isr4300-rommon.162-1r.pkg all
Chassis model ISR4321/K9 has a single rom-monitor.

Upgrade rom-monitor

Target copying rom-monitor image file
selected : 0
Booted : 0
Reset Reason: 0

Info: Upgrading entire flash from the rommon package
4259840+0 records in
4259840+0 records out
262144+0 records in
262144+0 records out
655360+0 records in
655360+0 records out
4194304+0 records in
4194304+0 records out
File is a FIPS ROMMON image
FIPS-140-3 Load Test on has PASSED.
Authenticity of the image has been verified.
Switching to ROM 1
8192+0 records in
8192+0 records out
Upgrade image MD5 signature is b702a0a59a46a20a4924f9b17b8f0887
4259840+0 records in
4259840+0 records out
4194304+0 records in
4194304+0 records out
4194304+0 records in
4194304+0 records out
262144+0 records in
262144+0 records out
Upgrade image MD5 signature verification is b702a0a59a46a20a4924f9b17b8f0887
Switching back to ROM 0
ROMMON upgrade complete.
To make the new ROMMON permanent, you must restart the RP.

Wirchbench#reload
```

# Verify that ROMMON upgrade was successful

```
Wirchbench#sh rom-monitor R0

System Bootstrap, Version 16.2(1r), RELEASE SOFTWARE
Copyright (c) 1994-2016 by cisco Systems, Inc.

Wirchbench#show platform
Chassis type: ISR4321/K9

Slot Type State Insert time (ago)
--------- ------------------- --------------------- -----------------
0 ISR4321/K9 ok 00:04:37
0/0 ISR4321-2x1GE ok 00:03:24
R0 ISR4321/K9 ok, active 00:04:37
F0 ISR4321/K9 ok, active 00:04:37
P0 PWR-4320-AC ok 00:04:20
P2 ACS-4320-FANASSY ok 00:04:20

Slot CPLD Version Firmware Version
--------- ------------------- ---------------------------------------
0 15030325 16.2(1r)
R0 15030325 16.2(1r)
F0 15030325 16.2(1r)
```

# Upgrade IOS-XE
Update boot variable to the new software image, save the configuration and reload the router
```
Wirchbench(config)#no boot system bootflash:isr4300-universalk9.03.16.00c.S.155-3.S0c-ext.SPA.bin
Wirchbench(config)#boot system bootflash:isr4300-universalk9.16.02.01.SPA.bin
Wirchbench#wr
Building configuration...
[OK]
Wirchbench#reload
```

# Verify software upgrade was successful
```
Wirchbench#sh ver
Cisco IOS XE Software, Version 16.02.01
Cisco IOS Software, ISR Software (X86_64_LINUX_IOSD-UNIVERSALK9-M), Version Denali 16.2.1, RELEASE SOFTWARE (fc1)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2016 by Cisco Systems, Inc.
Compiled Mon 28-Mar-16 03:45 by mcpre
```