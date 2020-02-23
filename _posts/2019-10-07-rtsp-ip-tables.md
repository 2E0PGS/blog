---
layout: post
title: RTSP IP tables
date: 2019-10-07 17:12:10
author: Peter Stevenson
summary: Using IP tables as a firewall for RTSP traffic
categories: networking
thumbnail:
tags:
 - firewall
 - security
 - ip-tables
 - Linux
 - networking
---

# RTSP forwarding

This is similar to a "data diode". 

For this example I am using a Raspberry Pi with two NICs. One onboard (primary network 192.168.1.40) and one USB (secondary network 192.168.50.1). 

The dest device is another host on the secondary network (192.168.50.2).

My PC I use to test the configuration is on the primary network (192.168.1.39).

This allows me to join two networks securely while only allowing TCP traffic over a specific port flowing in one direction.

## Enable IP forward in Linux.

`echo 1 > /proc/sys/net/ipv4/ip_forward`

## Allow traffic to be NAT to dest device.

We specify the port the firewall is listening on with `--dport`

Then we specify the device behind the firewall and what port it's listening on with `--to-detination`

`iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.50.2:80`

## Use MPV to try connect through firewall. Generally RTSP is TCP.

We use the firewalls IP here.

`mpv rtsp://admin:password@192.168.1.40:80/stream`
