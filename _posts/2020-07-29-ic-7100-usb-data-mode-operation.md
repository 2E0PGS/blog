---
layout: post
title: IC-7100 USB data mode operation
date: 2020-07-29 18:27:10
author: Peter Stevenson
summary: Using IC-7100 for data modes on a PC
categories: hamradio
thumbnail:
tags:
 - IC-7100
 - GQRX
---

# Using IC-7100 for data modes on a PC

## Connector settings explained

"SET"/"Connectors"/"ACC/USB Output Select": "AF" for data modes direct and "IF" for anything via GQRX. 

_"AF" is basically normal speaker audio without much/any DSP. "IF" (Intermediate Frequency) is basically raw IQ._

"SET"/"Connectors"/"DATA OFF MOD": "MIC,ACC"

_This is to choose fist microphone when not operating in "DATA" mode e.g. using "FM"._

"SET"/"Connectors"/"DATA MOD": "USB"

_This is to choose USB source as the microphone when operating in "DATA" mode._

"SET"/"Connectors"/"USB Audio SQL": "OFF (OPEN)" 

_This is to allow USB audio to always be present regardless of SQL level._

"ACC" is the accessory socket aka DIN socket.

## Connector settings summary

"DATA" mode seems to allow audio input via USB instead of using the microphone it also seems to change the RX bandwidth to a very small 1.2k or 500 and remove DSP.

So you can switch "ACC/USB Output Select" to IF and AF output depending if you need raw IQ or standard speaker audio into your software.

Probably don't bother with using the "DATA" mode as its too narrow AF output for SSTV etc. Instead switch "DATA OFF MOD" as required from "MIC,ACC" to "USB".

## Scanning notes

Scan function "ΔF (Delta Frequency)" can do + or - 1 MHz max freq sweep. The "SET" button allows for resume times etc.

## VirtualBox audio

If using a Linux host you can pass audio into a Windows VirtualBox VM for Windows only software data modes. You can pass inputs in seamlessly without the need for USB pass-through.

![virtualbox-audio.png](/blog/assets/2020-07-29/virtualbox-audio.png)

### MMSSTV Windows/QSSTV Linux

Here is an example of the VirtualBox audio being used to pass audio into MMSSTV inside a Windows VM. It tees the audio so you can also access the audio source on the host machine at the same time.

![sstv-win-linux.png](/blog/assets/2020-07-29/sstv-win-linux.png)

## QSSTV Linux

Sometimes the software would occasionally crash with the following error in console. This seems the be a unrelated bug in QSSTV: `Floating point exception (core dumped)`

"CAT (Data port)" control.

![qsstv-cat-control.png](/blog/assets/2020-07-29/qsstv-cat-control.png)

I found selecting "CAT (Voice port)" also works.

![qsstv-cat-control-also-works.png](/blog/assets/2020-07-29/qsstv-cat-control-also-works.png)

## WSJT-X Linux

Audio settings.

![wsjt-x-audio.png](/blog/assets/2020-07-29/wsjt-x-audio.png)

CAT control settings.

![wsjt-x-cat-control.png](/blog/assets/2020-07-29/wsjt-x-cat-control.png)

FT8 screen.

![wsjt-x-ft8.png](/blog/assets/2020-07-29/wsjt-x-ft8.png)

Enabling FT8/PSK reporting to [pskreporter.info/pskmap](https://www.pskreporter.info/pskmap.html)

![wsjt-x-psk-reporting-ft8.png](/blog/assets/2020-07-29/wsjt-x-psk-reporting-ft8.png)

WSPR screen.

![wsjt-x-wspr.png](/blog/assets/2020-07-29/wsjt-x-wspr.png)

## Screenshots of data mode usage

WSPR map on 20 m using Diamon CP-6.

![2020-06-07-wspr-band-20-m.png](/blog/assets/2020-07-29/2020-06-07-wspr-band-20-m.png)

WSPR map on 40 m using Diamon CP-6.

![2020-06-07-wspr-band-40-m.png](/blog/assets/2020-07-29/2020-06-07-wspr-band-40-m.png)

FT-8 PSK Reporter map on 20 m using Diamon CP-6.

![2020-06-09-ft-8-band-20-m.png](/blog/assets/2020-07-29/2020-06-09-ft-8-band-20-m.png)

FT-8 PSK Reporter map on 40 m using Diamon CP-6.

![2020-06-10-ft-8-band-40-m.png](/blog/assets/2020-07-29/2020-06-10-ft-8-band-40-m.png)

FT-8 PSK Reporter map on multiple bands using Diamon CP-6.

![2020-06-11-ft-8-multi-band.png](/blog/assets/2020-07-29/2020-06-11-ft-8-multi-band.png)

QSSTV images on 14.230 MHz 20 m using Diamon CP-6.

![2020-06-09-qsstv-f4hmp.png](/blog/assets/2020-07-29/2020-06-09-qsstv-f4hmp.png)

![2020-06-07-qsstv-iz3aoy.png](/blog/assets/2020-07-29/2020-06-07-qsstv-iz3aoy.png)

![2020-06-07-qsstv-ea9uv.png](/blog/assets/2020-07-29/2020-06-07-qsstv-ea9uv.png)

![2020-06-07-qsstv-ea3auw.png](/blog/assets/2020-07-29/2020-06-07-qsstv-ea3auw.png)