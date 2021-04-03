---
layout: post
title: Apple keyboard Ubuntu
date: 2021-04-03 23:53:10
author: Peter Stevenson
summary: UK Apple keyboard layout and fn key fix
categories: sysadmin
thumbnail:
tags:
 - Linux
---

# UK Apple keyboard layout and fn key fix for Ubuntu

I cannot recall which of the two methods I used most recently. I suspect the `hid_apple.conf` method is a better first bet. I provide two methods in case one works in the instance the other doesn't.

## hid_apple.conf method

You can add the following lines to: `/etc/modprobe.d/hid_apple.conf`

```sh
$ cat /etc/modprobe.d/hid_apple.conf
options hid_apple fnmode=2
options hid_apple iso_layout=0
```

## rc.local method

You can add the following lines to: `/etc/rc.local`

```sh
$ cat /etc/rc.local
# Apple keyboard fix.
echo "2" > /sys/module/hid_apple/parameters/fnmode
echo "0" > /sys/module/hid_apple/parameters/iso_layout

exit 0
```

## Check it worked

Give the system a reboot and cat the following to check those changes persisted after reboot.

```sh
$ cat /sys/module/hid_apple/parameters/fnmode
2
$ cat /sys/module/hid_apple/parameters/iso_layout
0
```