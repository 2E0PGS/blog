---
layout: post
title: LVM extend using GParted
date: 2020-01-26 23:04:10
author: Peter Stevenson
summary: Using GParted to extend a LVM partition
categories: sysadmin
thumbnail:
tags:
 - GParted
 - partitioning
 - VMWare
 - Linux
 - LVM
---

# Using GParted to extend a LVM partition

First, backup your data.

## Expand disk

You may have migrated from a smaller to larger physical disk.

If using VMWare power off the VM and expand the virtual disk in the VM settings menu.

## GParted disk image

Insert the live bootable disk into the host. 

If using VMWare use a virtual disk and ensure "connected" is checked.

## Boot options

F12 for one time boot menu on most physical machines.

If using VMWare you need to boot into the VMWare BIOS via F2 and change the boot order to prioritise the CD drive.

Choose GParted CD/ISO.

## Partitioning

"Deactivate" the partition you wish to extend via right click menu.

### On some setups you may have a two step

1. Right click "Expand" extended volume
2. Right click "Expand" LV (Logical Volume)

### On others it's simply

1. Right click "Expand" LV (Logical Volume)

Apply actions.

## Shutdown and remove GParted ISO/CD

I found the desktop icon on to be hit and miss for some odd reason. You may want to run `sudo poweroff`

## Boot back into Ubuntu

Run the following commands.

Check `/dev` for the correct device names. It normally includes the initial system hostname.

```
sudo lvextend -l+100%FREE /dev/minecraft-vg/root

sudo resize2fs /dev/mapper/minecraft--vg-root
```
