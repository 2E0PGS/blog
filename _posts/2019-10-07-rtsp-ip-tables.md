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
 - Raspberry Pi
---

# RTSP forwarding

This is similar to a "data diode". 

For this example I am using a Raspberry Pi with two NICs. On board NIC (secondary network 192.168.50.1) and USB NIC (primary network 192.168.1.40). 

The dest device is another host on the secondary network (192.168.50.2).

My PC I use to test the configuration is on the primary network (192.168.1.39).

This allows me to join two networks securely while only allowing TCP traffic over a specific port flowing in one direction.

## Pre setup

Image raspbian lite to a SD card.

Add `ssh` file to boot partiton.

## Edit before putting imaged SD card into Pi

```
sudo nano /etc/dhcpcd.conf 

interface eth0
static ip_address=192.168.50.1

interface eth1
static ip_address=192.168.1.40
```

## Edit after the Pi has booted for first time using SSH

`ssh pi@192.168.1.40`

### Enable IP forward in Linux

Uncomment the following line in the file

```
sudo nano /etc/sysctl.conf

net.ipv4.ip_forward = 1
```

### Optionally change hostname

`sudo nano /etc/hosts` and `sudo nano /etc/hostname`

### Allow traffic to be NAT to dest device

We specify the port the Pi(firewall) is listening on with `--dport`

Then we specify the device behind the Pi(firewall) and what port it's listening on with `--to-detination`

`iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.50.2:80`

Reboot the Pi(firewall).

## Test use MPV to try connect through the Pi(firewall)

Generally RTSP is TCP.

We use the Pi(firewall) IP here.

`mpv rtsp://admin:password@192.168.1.40:80/stream`

### Flow of traffic

PC desktop (192.168.1.39) --> Pi primary NIC (192.168.1.40) --> NAT and IP forwarding --> Pi secondary NIC (192.168.50.1) --> Dest RTSP server (192.168.50.2)

test
