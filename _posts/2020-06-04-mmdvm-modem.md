---
layout: post
title: MMDVM modem hat
date: 2020-06-04 17:02:19
author: Peter Stevenson
summary: MMDVM_pog hat to make a GMSK DStar gateway
categories: hamradio
thumbnail:
tags:
 - MMDVM_pog
 - MMDVM
 - D-Star
 - GMSK
---

# MMDVM Modem Hat

I am using a Icom IC-E208 and a "MMDVM_pog" hat to make a GMSK DStar gateway.

![mmdvm-modem-overview](/blog/assets/2020-06-04/mmdvm-modem-overview.jpg)

My fiend 2E0EOL has written a article on a similar setup: [daybologic GMSK Gateway](http://www.daybologic.co.uk/articles.php?content=gmsk)

## Software installing

Requires Pi-Star beta image for Raspberry Pi 3B+ at time of writing.

UPDATE 07/04/2020: "Raspberry Pi 3B+ out of beta".

## Pinouts

For some reason my PI wont boot with the hat on. I haven't figured out why yet. See: [wojciechk8/MMDVM_pog/issues/4](https://github.com/wojciechk8/MMDVM_pog/issues/4#issuecomment-482480582)

The "sql" pin of rig doesn't seem to be needed for basic setup.

The "rssi" pin of modem doesn't seem to be needed for basic setup.

### Icom IC-E208

![mmdvm-modem-hat](/blog/assets/2020-06-04/mmdvm-modem-hat.jpg)
![mmdvm-modem-breadboard](/blog/assets/2020-06-04/mmdvm-modem-breadboard.jpg)

## Software configuration 

Pi-Star web UI page configuration/expert/mmdvmhost you will find the following fields.

`txoffset` is worth playing with try `-100` or `100` or `200`.

`txinvert` was default `1`, for some radios like mine you may need to change it to `0` however in my case I later found my radio was off frequency and upon correcting it (see: [IC-E208 Calibration](https://2e0pgs.github.io/blog/hamradio/2019/05/25/ic-e208-calibration/)) I had to put `txinvert` back to original value.

Ensure suffix of call sign for gateway is not just a space but its last char of input string.

I had some issues changing the gateways call sign without re-flashing.

Thread: [facebook.com/groups/pistarusergroup/414855285738167](https://www.facebook.com/groups/pistarusergroup/414855285738167/?comment_id=414856705738025&reply_comment_id=415271039029925&notif_id=1554136716212493&notif_t=group_comment)

## Hardware calibration

There is a couple trim pots on the board which allow you to adjust the mic gain. Use the ACL meter on the radio to gauge this.

## Useful links

* [Connection images from eBay listing](https://www.ebay.com/itm/MMDVM-DMR-Repeater-Open-Source-Multi-Mode-Digital-Voice-Modem-for-Raspberry-MJ-/163608363073)
* [IC-e208 manual](http://www.radiomanual.info/schemi/ICOM_VU/IC-E208_user.pdf)
* [More connection images from eBay listing](https://www.ebay.co.uk/itm/2018-latest-MMDVM-DMR-Repeater-Open-Source-Multi-Mode-Digital-Voice-Modem-Moto/352486107764)
* [Radio compatibility tests](https://wiki.brandmeister.network/index.php/Homebrew/MMDVM?fbclid=IwAR3wkTfMHb_fN2V6INoDoh30Li06tqzpZdKBPKN5aTUeyScjTOPN0jQ8aS0#Recommend_radios_for_homebrew_repeaters)

Also see the DVAP page on my website: [ham-radio/dvap](https://2e0pgs.github.io/ham-radio/dvap.html)

## Useful documentation

My friend Rob (2E0RPT) took pictures of the document he received with his board. I didn't receive any such documentation with mine.

![mmdvm-modem-settings-guide](/blog/assets/2020-06-04/mmdvm-modem-settings-guide.jpg)