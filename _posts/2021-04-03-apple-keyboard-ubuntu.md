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

Both methods aim to allow the use of `F1-F12` function keys by setting the `fnmode` so they only act like media buttons when the `fn` key is held. They also aim to fix the layout to have the correct position for UK `<, >, @, #, ~, £, $` keys

## hid_apple.conf method

Create the file if it doesn't exist.

You can add the following lines to: `/etc/modprobe.d/hid_apple.conf`

```sh
$ cat /etc/modprobe.d/hid_apple.conf
options hid_apple fnmode=2
options hid_apple iso_layout=0
```

Then run `sudo update-initramfs -u -k all` and `sudo reboot`

Similar article: [Change Function Key behavior](https://help.ubuntu.com/community/AppleKeyboard#Change_Function_Key_behavior)

## rc.local method

This is the older method as newer distros don't have a rc.local.

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