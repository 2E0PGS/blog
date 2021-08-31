---
layout: post
title: WRT54G NVRAM WiFi drop outs
date: 2021-08-31 17:00:19
author: Peter Stevenson
summary: Flashing WRT54G with OpenWRT causes WiFi drop outs
categories: sysadmin
thumbnail:
tags:
 - OpenWRT
 - WRT54G
 - Linksys
---

# Flashing WRT54G with OpenWRT causes WiFi drop outs

When swapping between firmwares (stock Linksys, Tomato, DDWRT, OpenWRT) you may find the WiFi drops out. 

I thought it maybe a temperature issue or even hardware issue. I tried cooling fans and heat blown across the PCB and I tried various pressure across PCB in case there was a loose solder joint.

If you experience this try doing a full wipe of the NVRAM as flashing firmware doesn't fully clear down the NVRAM.

You can try the 30/30/30 reset but that never got me anywhere. I had to use a software method.

Look up firmware specific ways. Some use the GUI and some use the command line, see the below examples respectively:

"Erase all data in NVRAM memory (thorough)"

`erase nvram`

## WRT and router off topic notes

* WRT54G still useful as a 10/100 Mbps router for NAT separation or translation.
* BT routers don't seem to have the ability to use the WAN port with a local subnet gateway so WRT based stuff is still a good tool in the network engineer/sysadmin toolbox.
* Checkout the [GL.iNet GL-MT300N-V2](https://www.amazon.co.uk/GL-iNet-GL-MT300N-V2-Converter-Pre-installed-Performance/dp/B073TSK26W/) and various other GL.iNet products. They're useful as a mini firewall, router, data diode or WiFi AP and supports/has pre-installed OpenWRT.