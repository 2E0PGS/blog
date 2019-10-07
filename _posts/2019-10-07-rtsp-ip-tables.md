---
layout: post
title: RTSP IP tables
date: 2019-10-07 17:12:10
author: Peter Stevenson
summary: Using IP tables as a firefwall for RTSP traffic
categories: networking
thumbnail:
tags:
 - firewall
 - security
 - ip-tables
 - Linux
 - networking
---

### RTSP forwarding

#### Enable IP forward in Linux.

`echo 1 > /proc/sys/net/ipv4/ip_forward`

#### Allow traffic to be NAT to dest device.

We specify the port the firewall is listening on with `--dport`

Then we specify the device behind the firewall and what port it's listening on with `--to-detination`

`iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.50.2:80`

#### Use MPV to try connect through firewall. Generally RTSP is TCP.

We use the firewalls IP here.

`mpv rtsp://admin:password@192.168.1.40:80/stream`


