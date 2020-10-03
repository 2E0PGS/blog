---
layout: post
title: Mount VMDK in Ubuntu
date: 2020-06-04 17:12:19
author: Peter Stevenson
summary: Mount NTFS VMDK file in Ubuntu desktop
categories: sysadmin
thumbnail:
tags:
 - VMware
 - Linux
---

# Mount NTFS VMDK file in Ubuntu desktop

Using `vmware-mount` which seemed to come with VMware player.

## List partitions

`sudo vmware-mount -p mydisk.vmdk`

```
Nr Start Size      Type Id System
1  2024  947890173 BIOS 7  HPFS/NTFS
```

## Create mount folder

`sudo mkdir /mnt/myfiles`

## Mount partition in that folder

`sudo vmware-mount mydisk.vmdk 1 /mnt/myfiles`
