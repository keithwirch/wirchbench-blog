---
title: "Upgrade Cisco Nexus 9k Switches"
date: 2023-09-11
tags: [
    "Networking",
    "Cisco",
]
draft: false
---

# Navigating the Cisco Nexus Switch Upgrade Path

Upgrading Cisco Nexus switches is a crucial task to ensure your network infrastructure stays robust and secure. Following a recommended upgrade path is essential, and we have provided a useful tool to help you determine the right way to proceed.

[**Cisco Nexus Upgrade Matrix Tool**](https://www.cisco.com/c/dam/en/us/td/docs/dcn/tools/nexus-9k3k-issu-matrix/index.html)

This comprehensive tool offers valuable guidance for your upgrade journey, making it easier to make informed decisions.

## Uploading Upgrade Files Using SFTP

For a seamless upgrade process, we recommend using the Secure File Transfer Protocol (SFTP) to copy the necessary files to your Nexus switch. To facilitate this, you can use a utility like "freeFTPd" to set up a quick and efficient SFTP server.

Once you have your SFTP server ready, files can be effortlessly copied to the switch using the following command:

```
copy sftp://<username>:<password>/<path_to_upgrade_file> bootflash:
```

## Performing the Upgrade

With the files securely transferred to your Nexus switch, it's time to proceed with the installation. Below are the essential commands to execute the upgrade:

### 1. Show the Impact of the Update

Before initiating the installation, it's wise to assess the impact of the update. This helps you anticipate any potential challenges. Run the following command, replacing <filename> with the name of your upgrade file:

```
show install all impact nxos bootflash:<filename>
```

### 2. Perform the Installation

Once you've reviewed the impact and are ready to proceed, execute the installation using the following command:


```
install all nxos bootflash:<filename>
```
By following these steps and utilizing the provided resources, you'll ensure a smooth and successful upgrade of your Cisco Nexus switches, enhancing your network's performance and reliability.