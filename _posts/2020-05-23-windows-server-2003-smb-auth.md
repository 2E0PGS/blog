---
layout: post
title: Windows Server 2003 SMB auth
date: 2020-05-23 23:03:19
author: Peter Stevenson
summary: Fix unknown user name or bad password error
categories: sysadmin
thumbnail:
tags:
 - Windows Server
---

# Windows Server 2003 SMB auth

Windows Server 2003 SMB not allowing other devices on the network to authenticate or access the share.

> Logon failure: unknown user name or bad password.

Try the following fault finding steps. Test in-between.

1. Ensure system time is up to date and within a few seconds accuracy. Manually update it then do a sync with time servers in date and time properties.
2. Ensure LAN network DNS is correct. Use local router IP as the primary DNS server and Google DNS as the secondary.
3. Ensure you are entering the username with the domain aka the servers name. For example: `dellserver\user1`
4. "Map network drive" instead of adding a "Network Location" or accessing via "Networks". Ensure you check "Connect using different credentials".
5. Use the servers IP for mapping.
6. Include the share name in the path. For example: `\\192.168.1.6\share1`
7. Remove previous share for folder if exists. Create a new one on a folder instead of the entire drive.
8. Create a new user. Set simple password for testing. Add and assign user "Full Control" under "Sharing and Security..." of shared folder.
9. If possible try connect using a different OS.