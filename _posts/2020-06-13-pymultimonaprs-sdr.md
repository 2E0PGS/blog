---
layout: post
title: RTL-SDR pymultimonaprs iGate
date: 2020-06-13 14:31:10
author: Peter Stevenson
summary: Installing RTL-SDR pymultimonaprs iGate
categories: hamradio
thumbnail:
tags:
 - hamradio
 - RTL-SDR
 - SDR
 - Linux
 - APRS
---

# Installing RTL-SDR pymultimonaprs iGate

I am installing this onto a Ubuntu Server VM.

Three projects involved:

* [pymultimonaprs](https://github.com/asdil12/pymultimonaprs)
* [RTL-SDR](https://osmocom.org/projects/rtl-sdr/wiki/Rtl-sdr)
* [multimon-ng](https://github.com/EliasOenal/multimon-ng/)

## RTL-SDR installation

```
git clone git://git.osmocom.org/rtl-sdr.git

cd rtl-sdr/

mkdir build

cd build

sudo apt-get install cmake

sudo apt-get install libusb-1.0-0-dev

cmake ../ -DINSTALL_UDEV_RULES=ON -DDETACH_KERNEL_DRIVER=ON

make

sudo make install

sudo ldconfig
```

## multimon-ng installation

```
git clone https://github.com/EliasOenal/multimon-ng.git

cd multimon-ng/

mkdir build

cd build

cmake ..

// Ignore the pulse audio warning.

make

sudo make install

```

## pymultimonaprs installation

```
git clone https://github.com/asdil12/pymultimonaprs.git

cd multimon-ng/

sudo apt-get install python-minimal

sudo apt-get install python-pip

sudo pip install setuptools

sudo python2 setup.py install

sudo vim /etc/pymultimonaprs.json

sudo systemctl enable pymultimonaprs.service

sudo systemctl start pymultimonaprs.service

sudo systemctl status pymultimonaprs.service
```