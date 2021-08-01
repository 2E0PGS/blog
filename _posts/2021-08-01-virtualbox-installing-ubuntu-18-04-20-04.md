---
layout: post
title: VirtualBox installing Ubuntu 18.04/20.04
date: 2021-08-01 17:40:10
author: Peter Stevenson
summary: How to install VirtualBox and kernel drivers
categories: sysadmin
thumbnail:
tags:
 - Linux
 - VirtualBox
---

# VirtualBox installing Ubuntu 18.04/20.04

https://www.virtualbox.org/wiki/Linux_Downloads

> Ubuntu 19.10 / 20.04 / 20.10 / 21.04
> Ubuntu 18.04 / 18.10 / 19.04

20.04: https://download.virtualbox.org/virtualbox/6.1.22/virtualbox-6.1_6.1.22-144080~Ubuntu~eoan_amd64.deb

18.04: https://download.virtualbox.org/virtualbox/6.1.22/virtualbox-6.1_6.1.22-144080~Ubuntu~bionic_amd64.deb

```sh
wget https://download.virtualbox.org/virtualbox/6.1.22/virtualbox-6.1_6.1.22-144080~Ubuntu~eoan_amd64.deb

sudo dpkg -i virtualbox-6.1_6.1.22-144080~Ubuntu~eoan_amd64.deb

sudo apt install -f

sudo /sbin/vboxconfig

sudo usermod -a -G vboxusers $USER
```

This download it, installs it, fixes dependencies, installs kernel drivers (DKMS) and finally adds our user to `vboxusers` group for USB support etc.

https://www.virtualbox.org/wiki/Downloads

> Oracle VM VirtualBox Extension Pack
> All supported platforms

https://download.virtualbox.org/virtualbox/6.1.22/Oracle_VM_VirtualBox_Extension_Pack-6.1.22.vbox-extpack

Download, double click and install.