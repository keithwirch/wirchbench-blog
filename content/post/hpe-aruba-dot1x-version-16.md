---
title: "Setting Up 802.1x on an Aruba/HPE Switch (Version 16 Code)"
date: 2024-06-10
tags: ["networking", "802.1x", "Aruba", "HPE", "Switch"]
---

In this guide, I'll walk you through the steps to set up 802.1x authentication on an Aruba/HPE Switch running version 16 code. 802.1x is a network access control protocol that provides an authentication mechanism to devices wishing to attach to a LAN or WLAN.  This configuration will support:

- Certificate/credential authentication
- Mac address based authentication
- Downloadable ACLs

## Prerequisites

Before we begin, ensure you have the following:
- Aruba/HPE switch running version 16 code.
- RADIUS server configured with user credentials.
- Basic knowledge of switch configuration.

## Step-by-Step Guide

### 1. Setup RADIUS Servers

First, configure the RADIUS server settings.

```plaintext
switch# configure terminal
switch(config)# radius-server host <radius_server_ip> auth-port <int> acct-port <int> key <secret>
```

### 2. Add RADIUS Servers to a Group

```plaintext
switch(config)# aaa server-group radius <radius_group_name> host <radius_server_ip>
```

### 3. Enable Port-Access and MAC-Based Authentication

Configure the switch to use the RADIUS group for 802.1x and MAC-based authentication.

```plaintext
switch(config)# aaa port-access authenticator active
switch(config)# aaa authentication port-access eap-radius server-group <radius_group_name>
switch(config)# aaa authentication mac-based chap-radius server-group <radius_group_name>
```

### 5. Interface Configuration

Enable the authenticator and MAC-based authentication on specific ports.

```plaintext
switch(config)# aaa port-access authenticator <port>  # Enable the authenticator on the port
switch(config)# aaa port-access authenticator <port> client-limit 1  # Set a client limit on the port
switch(config)# aaa port-access mac-based <port>  # Enable MAB on the port
switch(config)# aaa port-access mac-based <port> addr-limit 3  # Set an address limit on the port
```

### 6. Configure Reauthentication (Optional)

Optionally, configure reauthentication on the port.

```plaintext
switch(config)# aaa port-access authenticator <port> reauth-period 3600
switch(config)# aaa port-access mac-based <port> reauth-period 3600
```

### 7. Show Commands

Verify the configuration using the following commands.
```plaintext
switch# show authentication  # Show the configured authentication groups for each service
switch# show port-access authenticator  # Show ports enabled with authenticator
switch# show port-access mac-based  # Show ports enabled with MAC-based authentication
switch# show access-list radius <port> # show any access list enable on a port.
```
