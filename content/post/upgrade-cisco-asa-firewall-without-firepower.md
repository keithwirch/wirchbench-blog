---
title: "Upgrading Cisco ASA Firmware without Firepower"
date: 2022-03-22T13:19:55-05:00
tags: [
    "Networking",
    "Cisco",
]
draft: false
---

This article is taken mainly from my favorite subnet calculator spot:  

https://www.tunnelsup.com/how-to-upgrade-a-cisco-asa-firewall/



# Understanding ASA Versions

ASA Firmware images include all the needed software and then licenses are used to enable/disable features.  OS Images will look like one of these 3:

`asa933-8-lfbff-k8.SPA`

`asa924-6-smp-K8.bin`

`asa924-3-k8.bin`

The numbers indicate the version. For instance the first file here is for ASA OS Version 9.3(3)8.

The lfbff and SPA indicates it has FirePower IPS included in the image and this image is digitally signed which makes it tamper resistant.

The smp indicates the image is for a multi-core ASA (check how many cores using show ver).

The 3rd one is for old ASAs that have a single core.

The k8 tag indicates this image supports DES encryption. With a license, you can make the ASA support AES and 3DES.

These images aren’t tied to a model number, so the image downloaded for a 5512x can also be used on a 5516x.

# Download Location

Software can be downloaded from Cisco's Support download site.  Firmware does require entitlement.

https://software.cisco.com/download/home


# Getting software on an ASA

There are a variety of ways to copy the ASA code to the box.  Easiest is pulling down from an external source.  Logging into the ASA and running the following commands will work:

`copy http flash`

`copy ftp flash`

`copy tftp flash`

Verify the software downlaoded  file is good with `verify disk0:/<filename>`.  Compare the checksum with what is on Cisco's Download Site.  It's near the download button.

# Changing the boot flag

Changing the firmware on the ASA is as simple as just changing the boot flag in the saved configuration.

See the current bootflag:

`show run | boot`

`show bootvar`

To change the bootflag:
```
conf t
no boot system disk0:/<filename from show run>
boot system disk0:/<filename>
write mem
```

# Reboot the ASA

Once the bootvar is set, you can reboot the ASA with the `reload` command in EXEC mode.  Once the ASA comes back, check the version with EXEC command:  `show version`.