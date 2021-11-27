---
layout: post
title: ISM GSM band mesh networking LoRa
date: 2021-11-27 16:42:10
author: Peter Stevenson
summary: Bristol LoRa disaster.radio
categories: hamradio
thumbnail:
tags:
 - loRa
 - Mesh
---

# Bristol LoRa disaster.radio

This post has been sat around in my drafts for a long time (23/01/2020) before I posted it and therefore may no longer be relevant.

I thought I better get on and post this now as a flurry of videos about LoRa have surfaced again.

I ordered two of these in the 868 MHz variant from AliExpress: [LILYGO® TTGO Disaster-Radio LoRa32 V2.1 1.6 Version 433/868/915MHZ LoRa ESP-32 OLED 0.96 Inch SD Card Bluetooth WIFI Module](https://www.aliexpress.com/item/4000396836096.html)

They come pre flashed with the disaster.radio firmware.

Just did some range tests and had really impressive results of 700 m with one unit in hand on foot and the other on a shelf in the QTH second floor.

I suggested some improvements to the project. Below is a couple of my suggestions/bug reports along with related links:

* [Routing table error after long uptime or 3+ nodes #38](https://github.com/sudomesh/disaster-radio/issues/38)
* [disaster-radio-website/issues/4](https://github.com/sudomesh/disaster-radio-website/issues/4)
* [868 Mhz setting for Europe #43](https://github.com/sudomesh/disaster-radio/issues/43) Sam raised the ticket but this is something we discussed in the "Bristol LoRa" WhatsApp group.
	* [adds ability to configure lora frequency and spreading factor via con…](https://github.com/sudomesh/disaster-radio/commit/a94ca40b72bf207a8d4c7fca0d11128dada49459)
	* [add lora frequency as config option](https://github.com/sudomesh/disaster-radio/commit/93aafa36f6aff5abe6fe12e06d3e831ce188059e)
	* [expose LoRaFrequency and spreadingFactor in public functions](https://github.com/sudomesh/LoRaLayer2/commit/8509821d2a04b5edccaaaab7a6ef3260aa7b2cba)
* [releases/tag/0.2.0](https://github.com/sudomesh/disaster-radio/releases/tag/0.2.0)
* [Useful docs on the protocol](https://github.com/sudomesh/disaster-radio/wiki/Protocol)
* [What is lora?](https://www.link-labs.com/blog/what-is-lora)

## Also see

* [esptool for flashing the LoRa boards](https://github.com/espressif/esptool)
* [My blog post on Arduino package management mentioning platformio](https://2e0pgs.github.io/blog/programming/2021/03/07/arduino-package-management/)
* [Bristol HAMNET](https://www.facebook.com/groups/1783722755224874) and [2E0PGS - HSMM-MESH](https://2e0pgs.github.io/ham-radio/hsmm-mesh.html) since we're semi the same group.