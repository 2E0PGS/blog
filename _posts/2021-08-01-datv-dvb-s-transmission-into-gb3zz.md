---
layout: post
title: DATV DVB-S transmission into GB3ZZ
date: 2021-08-01 19:11:19
author: Peter Stevenson
summary: Focus on VLC RTP, GB3ZZ and DATV transmission.
categories: hamradio
thumbnail:
tags:
 - ATV
 - SDR
---

# DATV DVB-S transmission into GB3ZZ

This post is a continuation of [A state of tuners](https://2e0pgs.github.io/blog/hamradio/2020/07/25/a-state-of-tuners/) but with a focus on VLC RTP, GB3ZZ and DATV transmission. I suggest reading my previous post first.

## Current setup

* See comment/description of video: [Working GB3ZZ 437 MHz input 2021-07-14](https://www.youtube.com/watch?v=-eicPTC1YCQ)
* 1280 x 720 progressive 30 fps MJPG
	* Could effect bandwidth or CPU usage.

## Videos

<div align="center">
	<iframe width="419" height="236" src="https://www.youtube.com/embed/4vg6mT37uoU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

	<iframe width="419" height="236" src="https://www.youtube.com/embed/-eicPTC1YCQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

## VLC

* Useful command to check VLC ports: `watch -d "sudo lsof -i -P -n  | grep vlc"`
* <https://wiki.videolan.org/Documentation:Streaming_HowTo/Command_Line_Examples>

### Local

It wouldn't accept dvb-adapter on the same colon spaced so had to be a separate flag.

```sh
vlc dvb-s://frequency=1316000000:srate=4000000:dvb-fec=1:voltage=0 --dvb-adapter=1
```

Program has to have a space before.

```sh
vlc dvb-s://frequency=1316000000:srate=4000000:dvb-fec=1:voltage=0 :program=2 --dvb-adapter=1

vlc dvb-s://frequency=1316000000:srate=4000000:dvb-fec=1:voltage=0 :program=1 --dvb-adapter=1
```

I eventually found I could use colons for adapter provided its also spaced.

```sh
vlc dvb-s://frequency=1316000000:srate=4000000:dvb-fec=1:voltage=0 :program=2 :dvb-adapter=1

vlc dvb-s://frequency=1316000000:srate=4000000:dvb-fec=1:voltage=0 :program=1 :dvb-adapter=1
```

The following two commands did not work for me.

```sh
vlc dvb-s://frequency=1316000000:srate=4000000:dvb-fec=1:voltage=0 :program=2 :dvb-adapter=1 --sout rtp/ts://localhost:9090

vlc dvb-s://frequency=1316000000:srate=4000000:dvb-fec=1:voltage=0 :program=2 :dvb-adapter=1 --sout http/avi://localhost:9090
```

### Remote

Got this one to work for one channel.

```sh
vlc dvb-s://frequency=1316000000:srate=4000000:dvb-fec=1:voltage=0 :program=2 :dvb-adapter=1 --sout '#rtp{dst=192.168.1.173,port=1234,sdp=rtsp://192.168.1.137:8080/test.sdp}'
```

These work for multi channels although I found the latter works without the need for `--sout-all`

```sh
vlc dvb-s://frequency=1316000000:srate=4000000:dvb-fec=1:voltage=0 :dvb-adapter=1 --sout '#rtp{dst=192.168.1.173,port=1234,sdp=rtsp://192.168.1.137:8080/test.sdp}' --sout-all

vlc dvb-s://frequency=1316000000:srate=4000000:dvb-fec=1:voltage=0 :dvb-adapter=1 --sout '#rtp{dst=192.168.1.173,port=1234,sdp=rtsp://192.168.1.137:8080/test.sdp}' 
```

Client command: `vlc rtsp://192.168.1.137:8080/test.sdp`

I could also do multicast but I am sticking to unicast as I only intend to watch it on one machine and want to save bandwidth on my LAN.

## Notes

* RTL-SDR wont work due to 2.4 MHz band width limitation for GB3ZZ as ZZ TXs 4 MSps which is around 5 MHz bandwidth.
	* I can however use a RTL-SDR + SDRangel for local TX monitoring of my 1 MSps signal.
* I wonder if HackRF libraries are what slow down SDR angel.
* TODO: Try pluto with SDRangel.
* Blazer HD4000 will go down to 1 MSps.
	* It will also go down to 437 MHz and a bit lower.
	* 10000 (Mhz lnb freq in software) + 437 (Mhz freq we want to receiver) = 10437 is the frequency we input into the box so it can account for lnb down shift.
	* However its input sensitivity is rather bad that low down.
* It's worth monitoring the TX output spectrum of your TX device, in my case Pluto SDR.
	* I found sometimes it transmits a bit wonky, e.g. broken up signal. In that instance DATV Express needs restarting to fix it.
* I can get 2 MSps with h262 but not h264 on my Pluto.
	* h265 I hit CPU limitations on my host PC.
* Blazer works on h262 and h264.
* I haven't yet gotten 333 KSps to work yet locally. 1 MSps TX works best however I should check the 333 issue isn't a DATV Express restart issue. Otherwise it maybe just SDRAngel.
* _Ryde receiver no dual channel support?_
* Observed setups
	* TX: DATV Express -> LimeSDR. RX: FreeSAT.
	* TX: FR Systems
	* TX: DTX 1. RX: FreeSAT.
* Ports down
	* A semi umbrella term for a project which can consist of different transmitters.
	* Multiple versions of ports down exist see links section.
	* LimeSDR
	* Pluto SDR
	* DATV Express TX board
	* MiniTiouner receiver board
	* Raspberry PI running custom OS
* The handheld "Satellite Signal Finder" I been informed go down to 1 MSps and 437 MHz.
* h262/MPEG2 are the same.

## GB3ZZ extended details

Note this information is subject to change and should only be viewed as a snapshot of the current setup at time of writing.

* The 1249 input is more sensitive because the aerials are much higher and there is a hot pre-amp up the mast.
* Big wheel omnidirectional horizontal antenna for 437 MHz input.
	* Height is same as the 2 m talk back antenna approx 2-5 ft off the roof of the building.
* Height for 23 cm input antenna is 20 ft off the roof.
* 08:45 BATC streamer reboots and then again at 16:45. Reboot takes approx 5 mins.
* _The big wheel antenna is the only working one at the moment? The Yagi heading towards Minehead doesn't work?_
* There is an automatic antenna switcher for 437 MHz input.
	* 10:57 - 12:57 Yagi facing Minehead is active.
	* 17:24 - 19:54 Yagi facing Minehead is active.
	* The above times are in effect Tuesday to Saturday.
	* The rest of the time big wheel is active.
	* At all times 23 cm input overrides this so you can operate 23 cm input at any time of day.
	* In other words
		* When pictures of Bristol are showing on GB3ZZ the big wheel antenna is active.
		* When GB3ZZ test cards are showing on GB3ZZ the Yagi facing Minehead is active.
* Ryde receivers only active from 07:00 - 23:00 during summer time. They're subject to BST.
* _23 cm input and output faces?_
* _23 cm output height off roof?_
* The 144.750 MHz VHF FM vertical is for talk back. This gets fed into the TS (Transport Stream) of GB3ZZ output.
* Rydes can take 30 seconds to switch as they're scanning multiple modes.
* Output is two channel multiplexed. Channel 1 is the main channel and has audio left and right separate sources. Channel 2 has no audio normally and is the engineering channel.
* **Update: 2021-08-16:** 70 cm input is no longer active.
* **Update: 2021-09-01:** GB3ZZ site decommissioned. TODO: Find exact date.
* **Update: 2022-01-10:** GB3ZZ recommissioned on adjacent building. Exact heights, bearings and ranges TBA. Currently running in TX only beacon mode with no RX enabled.

| Band  | Direction | Name      | Availability  | Frequency   | Standard | Symbolrate                 | Video codec    | FEC | Antenna             | Polarisation | Device        | Notes |
|-------|-----------|-----------|---------------|-------------|----------|----------------------------|----------------|-----|---------------------|--------------|---------------|-------|
| 23 cm | Output    | Primary   | 24/7          | 1316 MHz    | DVB-S    | 4 MSps                     | h262           | 1/2 | Yagi                | Horizonal    |               |       |
| 23 cm | Input     | Primary   | 24/7          | 1249 MHz    | DVB-S    | 4 MSps                     | h262           |     | Yagi                | Horizonal    | Satellite box |       |
| 23 cm | Input     | Secondary | 06:00 - 22:00 | 1249 MHz    | DVB-S/S2 | 2 MSps, 1 MSps or 333 KSps | h262/h264/h265 |     | Yagi                | Horizonal    | Ryde reciever |       |
| 70 cm | Input     | Primary   | 24/7          | 437 MHz     | DVB-S    | 2 MSps                     | h262           |     | Wheel/Yagi switched | Horizonal    | Satellite box |       |
| 70 cm | Input     | Secondary | 06:00 - 22:00 | 437 MHz     | DVB-S/S2 | 1 MSps or 333 KSps         | h262/h264/h265 |     | Wheel/Yagi switched | Horizonal    | Ryde reciever |       |
| 2 m   | Input     | Primary   | 24/7          | 145.750 MHz | FM       | N/A                        | N/A            | N/A | Collinear           | Vertical     |               |       |

![antennas](http://www.stvg.co.uk/images/GB3ZZ%202013%20006a.jpg)

## Working the 23 cm input

Above I mention working the 70 cm input. I also worked the 23 cm input a few days later. TODO: Video coming soon.

## Links and diagram

* <https://www.qsl.net/g8ymm/gb3zz.htm> 
* [Interlacing and HD for DATV Express - GW8BVI](https://www.cardiffars.org.uk/events/2020_Phil%20Longhurst_Interlacing%20and%20HD%20for%20DATV%20Express.pdf)
* <https://www.camsecure.co.uk/GB3ZZ.html>
	* <https://www.camsecure.co.uk/437.html>
* <https://batc.org.uk/live/gb3zz>
* <http://www.stvg.co.uk/gb3zz.html>
* <https://wiki.batc.org.uk/GB3ZZ>
* <https://github.com/BritishAmateurTelevisionClub>
* <https://github.com/davecrump/portsdown-buster>
* <https://wiki.batc.org.uk/Portsdown_software>
* <https://wiki.batc.org.uk/Use_With_a_DTX-1>
* <https://wiki.batc.org.uk/Lean_DVB_receiver>
* <https://wiki.batc.org.uk/Portsdown_2019>
* <https://wiki.batc.org.uk/23cms_ATV>
* [Bristol Minicat19, BATC](https://youtu.be/krVbt_BBdWM)
* <https://github.com/F5OEO/rpidatv>
* <https://wiki.batc.org.uk/Ryde_Receiver>
* <https://wiki.microwavers.org.uk/Langstone_Project>
* <https://wiki.batc.org.uk/Portsdown_4_Pluto>
* <https://wiki.batc.org.uk/Portsdown_2020>
* <https://wiki.batc.org.uk/DATV_transmitting_Equipment>
* <https://wiki.batc.org.uk/The_Portsdown_DATV_transceiver_system>
* <https://wiki.batc.org.uk/Portsdown_4>

![Portsdown 4 diagram](https://wiki.batc.org.uk/images/thumb/f/fd/Portsdown_4_Block.PNG/600px-Portsdown_4_Block.PNG)











