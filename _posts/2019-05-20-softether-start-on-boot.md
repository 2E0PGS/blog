---
layout: post
title: SoftEther Linux client/server start on boot
date: 2019-05-20 09:37:19
author: Peter Stevenson
summary: Starting SoftEther on Linux and getting an IP on boot.
categories: networking
thumbnail:
tags:
 - SoftEther
 - Linux
---

# How to start SoftEther Linux client/server on boot

Based on a Debian crontab.

The SoftEther client requires a manual DHCP client call after starting to get an IP.

```sh
@reboot /home/peter/vpnclient/vpnclient start

@reboot sleep 10; /sbin/dhclient vpn_vpn

@reboot sleep 20; /home/peter/vpnserver/vpnserver start
```
