---
title: "A10 aFlex script to limit by source address"
date: 2023-08-16
tags: [
    "Networking",
    "A10",
]
draft: false
---

In the A10 platform you designate different behavior based on the source IP of the client, such as whitelisting a Load Balaner VIP by the source IP.  You will need a Class List with the Source IPs in the list.

Here is the example script and Class List configuration:

```
# A10 Class List configuration
class-list my_class_list ipv4 
  1.1.1.0.24 
  2.2.2.2/32 
  3.3.3.3/32 
```

aFlex Script:
```
# Purpose:  Limit external availability to a Endpoint based on source IP
# Author:  <YOUR NAME>

when RULE_INIT {
  # Configured in "Shared Objects" -> "Class Lists or in the CLI
  set ::MYCLASSLIST "my_class_list"
}

when HTTP_REQUEST {
    # If not in the ukg_appointments class-list and tries to access /mapi
    if { not [ CLASS::match [IP::client_addr]  $::MYCLASSLIST ip ] } {
        HTTP::respond 401 content "Unauthorized\n"
}

```