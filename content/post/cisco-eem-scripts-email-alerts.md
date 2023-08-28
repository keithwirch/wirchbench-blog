---
title: "EEM Script: Email alert for Down EIGRP Neighbor"
date: 2023-08-28
tags: [
    "Networking",
    "Cisco",
]
draft: false
---

# Introduction
At times, the need arises to promptly receive email alerts directly from your Cisco Router. Cisco IOS-XE boasts a versatile attribute known as the Embedded Event Manager (EEM), which can be harnessed for various tasks, including the facilitation of email transmissions. In the forthcoming discussion, we shall delve into a script designed to promptly dispatch an alert in the event of a specific EIGRP Neighbor's disconnection.

# Script
```
event manager applet <name>
 event syslog pattern "Neighbor <ip_address> \(<interface name>\) is down" maxrun 60
 action 1.0 wait 3
 action 2.0 mail server "<smtp_server>" to "<destination_email>" from "<from_email>" subject "<email_subject>" body "<email_body>"
```
This script is tailored to detect a syslog entry indicating the disruption of an EIGRP Neighbor, coupled with a personalized email notification.  A 3 second wait is put into the script just incase routing updates need to happen as part of the EIGRP neighbor being down.  Please ensure to substitute placeholders enclosed within angle brackets "<>" with pertinent information. A comprehensive exemplar is illustrated below for your reference.

```
event manager applet neighbor_down
 event syslog pattern "Neighbor 192.168.1.1 \(GigabitEthernet0/0/0\) is down" maxrun 60
 action 1.0 wait 3
 action 2.0 mail server "smtp.example.com" to "network@example.org" from "MYROUTER@example.org" subject "EIGRP Neighbor Down" body "The EIGRP neighbor 192.168.1.1 is down."
```



