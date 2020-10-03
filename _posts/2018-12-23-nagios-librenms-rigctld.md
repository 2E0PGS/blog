---
layout: post
title: Nagios LibreNMS rigctld
date: 2018-12-23 14:02:10
author: Peter Stevenson
summary: Using LibreNMS and rigctld from Hamlib to graph my radio.
categories: programming
thumbnail:
tags:
 - IC-7100
 - Hamlib
 - Linux
---

# LibreNMS and rigctld

Using LibreNMS and rigctld from [Hamlib](https://github.com/Hamlib/Hamlib) to graph my radio.

I wrote a script for Nagios/LibreNMS a while back which polls rigctld (a Hamlib tool).

It allows me to graph various elements of my IC-7100. The graph titles are incorrect in places but the data is correct.

Link to my script: [check_rigctld](https://bitbucket.org/2E0PGS/nagios-plugins/src/master/check_rigctld)

## Screenshot from my LibreNMS

![screenshot](/blog/assets/2018-12-23/nagios-librenms-rigctld-screenshot.png)

