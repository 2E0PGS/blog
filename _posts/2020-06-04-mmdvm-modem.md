---
layout: post
title: MMDVM modem HAT
date: 2020-06-04 17:02:19
author: Peter Stevenson
summary: MMDVM_pog HAT to make a GMSK DStar gateway
categories: hamradio
thumbnail:
tags:
 - MMDVM_pog
 - MMDVM
 - D-Star
 - GMSK
---

# MMDVM modem HAT

I am using a Icom IC-E208 and a "MMDVM_pog" HAT to make a GMSK DStar gateway.

![mmdvm-modem-overview](/blog/assets/2020-06-04/mmdvm-modem-overview.jpg)

My fiend 2E0EOL has written a article on a similar setup: [daybologic GMSK Gateway](http://www.daybologic.co.uk/articles.php?content=gmsk)

## Software installing

Requires Pi-Star beta image for Raspberry Pi 3B+ at time of writing.

UPDATE 07/04/2020: "Raspberry Pi 3B+ out of beta".

## Pinouts

I connected the modem pins "GND", "TX", "RX" and "PTT" accordingly to my radio. You need a radio with a "DATA IN" jack accepting 9600 bps GMSK. The radio must also support 2.5 kHz FM narrow deviation.

The "SQL" pin of rig doesn't seem to be needed for basic setup.

The "RSSI" pin of modem doesn't seem to be needed for basic setup.

### Icom IC-E208

![mmdvm-modem-hat](/blog/assets/2020-06-04/mmdvm-modem-hat.jpg)
![mmdvm-modem-breadboard](/blog/assets/2020-06-04/mmdvm-modem-breadboard.jpg)

### Icom ID-E880

The pinout is the same for a ID-E880 which is nice! Picture is of a setup with the HAT I now recommend see the below heading "Issues".

![id-e880-mmdvm](/blog/assets/2020-06-04/id-e880-mmdvm.jpeg)

## Software configuration 

Pi-Star web UI page "Configuration"/"Expert"/"MMDVMHost" you will find the following fields.

`TXOffset` is worth playing with try `-100` or `100` or `200`.

`TXInvert` was default `1`, for some radios like mine you may need to change it to `0` however in my case I later found my radio was off frequency and upon correcting it I had to put `txinvert` back to original value.

"Radio/Modem Type" either options worked for me:

"MMDVM_HS_Hat (DB9MAT & DF2ET) for Pi (GPIO)"

"STM32-DVM / MMDVM_HS - Raspberry Pi Hat (GPIO)"

## Hardware calibration

There is a couple trim pots on the board which allow you to adjust the mic gain. Use the ACL meter on the radio to gauge this.

Use a SDR or spectrum analyser to check the radio is on frequency see: [IC-E208 Calibration](https://2e0pgs.github.io/blog/hamradio/2019/05/25/ic-e208-calibration/)

## Issues

### Pi wont boot with the HAT on

I had to power up the Pi with no HAT then add the HAT once the Pi has booted a bit. Not ideal because it wouldn't easily recover from power outage. Also causes more potential for pins to misalign or make contact at different times with power/data.

MMDVM_pog HAT: [wojciechk8/MMDVM_pog/issues/4](https://github.com/wojciechk8/MMDVM_pog/issues/4#issuecomment-482480582)

I tried testing fresh install of Raspian lite. This didn't help.

I tried changing the `cmdline.txt` to remove the `serial0` console in case the HAT was firing some serial command at boot. This didn't help.

```
console=serial0,115200 console=tty1 root=PARTUUID=2fed7fee-02 rootfstype=ext4 elevator=deadline fsck.repair=yes rootwait quiet init=/usr/lib/raspi-config/init_resize.sh
```

```
console=tty1 root=PARTUUID=2fed7fee-02 rootfstype=ext4 elevator=deadline fsck.repair=yes rootwait quiet init=/usr/lib/raspi-config/init_resize.sh
```

I noticed my friend Rob (2E0RPT) had a different looking PCB layout on his MMDVM HAT he bought from eBay. He confirmed his Pi 3B+ cold booted just fine with the HAT on. So I wondered if this was a hardware problem with my Pi or the HAT. Rob actually offered to send me his HAT to compare. After testing with his HAT the Pi booted just fine however the PCB lacks version or author information. Perhaps shop for one that looks the same, see the below image of the board I now recommend instead of the board I show above with two blue POTs:

![mmdvm-hs](/blog/assets/2020-06-04/mmdvm-hs.jpeg)

### Gateway call sign

Ensure suffix of call sign for gateway is not just a space but its last char of input string.

I had some issues changing the gateways call sign without re-flashing.

Thread: [facebook.com/groups/pistarusergroup/414855285738167](https://www.facebook.com/groups/pistarusergroup/414855285738167/?comment_id=414856705738025&reply_comment_id=415271039029925&notif_id=1554136716212493&notif_t=group_comment)

## Useful links

* [Connection images from eBay listing](https://www.ebay.com/itm/MMDVM-DMR-Repeater-Open-Source-Multi-Mode-Digital-Voice-Modem-for-Raspberry-MJ-/163608363073)
* [IC-e208 manual](http://www.radiomanual.info/schemi/ICOM_VU/IC-E208_user.pdf)
* [More connection images from eBay listing](https://www.ebay.co.uk/itm/2018-latest-MMDVM-DMR-Repeater-Open-Source-Multi-Mode-Digital-Voice-Modem-Moto/352486107764)
* [Radio compatibility tests](https://wiki.brandmeister.network/index.php/Homebrew/MMDVM?fbclid=IwAR3wkTfMHb_fN2V6INoDoh30Li06tqzpZdKBPKN5aTUeyScjTOPN0jQ8aS0#Recommend_radios_for_homebrew_repeaters)

Also see the DVAP page on my website: [ham-radio/dvap](https://2e0pgs.github.io/ham-radio/dvap.html)

## Useful documentation

My friend Rob (2E0RPT) took pictures of the document he received with his board. I didn't receive any such documentation with mine.

![mmdvm-modem-settings-guide](/blog/assets/2020-06-04/mmdvm-modem-settings-guide.jpg)