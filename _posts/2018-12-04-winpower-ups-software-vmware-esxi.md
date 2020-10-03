---
layout: post
title: Winpower UPS software VMware ESXi
date: 2018-12-04 17:07:19
author: Peter Stevenson
summary: Using a Winpower UPS with VMware ESXi
categories: sysadmin
thumbnail:
tags:
 - VMware
 - Winpower
 - UPS
 - vCLI
---

# Using a Winpower UPS with VMware ESXi

_Moved here from [original gist](https://gist.github.com/2E0PGS/6fe8094b91065d3a5aac17204f7493c1)_

This is mostly in reference to their Linux documentation: [Installation and configuration for Winpower in the VMware](http://www.ups-software-download.com/winpower/data/Winpower%20Manual%20VMwareESXi.pdf)

Winpower when installed using the Linux binaries includes a few extra perl scripts which supports shutting down VMware environments.

It does however also require you have the following perl lib installed on the system `VMware::VIRuntime`. Which can be acquired by installing VMware vCLI binaries.

The following documentation aids installing vCLI: [Installing vCLI](https://pubs.vmware.com/vsphere-50/index.jsp?topic=%2Fcom.vmware.vcli.getstart.doc_50%2Fcli_install.4.1.html)

You don't have to use vMA (vSphere Management Assistant) unless you don't want to manually install vCLI. However using vMA be warned that it didn't work with SMTP out the box and "vMA is deprecated and vMA 6.5 is the last release".

You probably need to accept the ESXi servers certificate and you should also test it's contactable.

`/usr/lib/vmware-vcli/vin/esxcli/esxcli --server <host ip address> vm process list`

This will return a error about not trusted cert with a finger print. Copy the finger print to place this into the next command.

`/usr/lib/vmware-vcli/apps/general/credstore_admin.pl add -s <host ip address> -t <finger print>`

It should say successfully added new entry. Now try running the first command again and you should have a successful vCLI response showing a list of VMs.

`/usr/lib/vmware-vcli/vin/esxcli/esxcli --server <host ip address> vm process list`

Now Winpower should be able to contact the ESXi server provided you added it's login details into the `hostlist` via `sudo ./config.pl` as per the documentation.