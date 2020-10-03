---
layout: post
title: Working the ISS using APRS
date: 2020-10-02 18:17:10
author: Peter Stevenson
summary: Using APRS to contact the ISS
categories: hamradio
thumbnail:
tags:
 - IC-7100
 - mpv
 - multimon-ng
 - Dire Wolf
 - APRS
 - ISS
 - ARISS
 - GQRX
 - RS0ISS
---

# Using APRS to contact the ISS

## Radios

Good enough for the ISS good enough for me. I was about to buy one of the following Kenwood radios.

TM-D710

TM-D100

## aprs-tx.sh

It's important you set path via `ariss` and not `WIDE1-1` or `WIDE2-1` as WIDE is for terrestrial APRS.

I had a sudden idea pop into my head on how to do a TX with my IC-7100.

My hack job script below got my packet on the map using a IC-7100 running 25 w into a collinear. Back to earth via DL8NDR iGate.

`aprs-2e0pgs-6.mp3` is a audio recording I made of a encoded APRS packet which includes my location and call sign.

I have a previous blog post on rigctl: [here](https://2e0pgs.github.io/blog/programming/2018/12/17/ic7100-hamlib/)

```
$ cat aprs-tx.sh 
#!/bin/bash

/usr/local/bin/rigctl -r /dev/ttyUSB0 -m 370 T 1
mpv aprs-2e0pgs-6.mp3 --audio-device='pulse/alsa_output.usb-Burr-Brown_from_TI_USB_Audio_CODEC-00.analog-stereo'
/usr/local/bin/rigctl -r /dev/ttyUSB0 -m 370 T 0
```

```
watch -n 60 ./aprs-tx.sh
```

![ariss-website-map](/blog/assets/2020-10-02/ariss-website-map.png)

![ariss-website-log](/blog/assets/2020-10-02/ariss-website-log.jpeg)

![aprs-fi](/blog/assets/2020-10-02/aprs-fi.png)

While I am at it we can decode via GQRX and UDP out to multimon-ng. It worked but was a bit hit and miss on the message probably due to doppler shift. Below is some of the decodes I had via multimon-ng. I later enabled `--timestamps` flag.

```
AFSK1200: fm ? to D3CZ5\-10 QSO Nr 11776 I15- pid=35
..D

AFSK1200: fm ? to :GDXU/-5 QSO Nr 6693 I26^ pid=26
.1.x

AFSK1200: fm ? to 4(,0P+-3 QSO Nr 3846 I54v pid=FF
....

AFSK1200: fm ? to R7=S16-1 QSO Nr 5165 RNR5v pid=B3
.q...39...

2020-06-12 14:04:44: AFSK1200: fm ? to '5(;+"-6 QSO Nr 9632 REJ1+ pid=93
2020-06-12 14:04:44: ....

2020-06-12 21:07:13: AFSK1200: fm ? to ]M/K'W-1 QSO Nr 2258 I66^ pid=E7
2020-06-12 21:07:13: ...;..
```

![gqrx-iss-doppler](/blog/assets/2020-10-02/gqrx-iss-doppler.png)

## Dire Wolf

I have played with Dire Wolf briefly in the past. I wasn't aware just how functionally complete it was!

### User Guide

The [PDF User Guide](https://github.com/wb2osz/direwolf/blob/master/doc/User-Guide.pdf) is very comprehensive see below useful headings.

* 9.6.3 SATgate example
* 9.10.5 SATgate mode
* 9.10.7 Log Everything Going In and Out

### Setup

Let's set up a more proper TX and RX APRS modem with iGate and SatGate function. I believe SatGate is just a fancy term for a iGate listening on satellite frequencies e.g. 145.825

```
sudo apt-get install libasound2-dev
git clone https://www.github.com/wb2osz/direwolf
cd direwolf
make
sudo make install
make install-conf
```

Use `aplay -l` to find hardware device ID of sound card. See [IC-7100 blog post](https://2e0pgs.github.io/blog/hamradio/2020/07/29/ic-7100-usb-data-mode-operation/) on `AF` output mode if applicable.

#### direwolf-satgate.conf

This is my current config file.

```
ADEVICE  plughw:4,0

ACHANNELS 1

CHANNEL 0

MYCALL 2E0PGS-6

MODEM 1200

PTT RIG 370 /dev/ttyUSB0 

AGWPORT 8000
KISSPORT 8001

PBEACON delay=1 every=1 overlay=S symbol="dish" lat=12^34.56N long=1^23.45W power=25 height=20 gain=5 comment="www.m3pgs.co.uk" via=ARISS

IGSERVER euro.aprs2.net 

IGLOGIN 2E0PGS-6 12345

IGTXLIMIT 6 10

FILTER 0 ig b/RS0ISS | d/RS0ISS

LOGFILE /home/peter/direwolf-satgate-rx.log
```

Running direwolf: `direwolf -a 100 -t 0 -d ii -d fff -c ~/direwolf-satgate.conf | tee -a ~/direwolf-satgate-console.log`

Test it decodes use another radio to send a packet over 145.825.

Test it beacons use another device with a decoder.

![laptop-decode-beacon](/blog/assets/2020-10-02/laptop-decode-beacon.png)

I have my SatGate beacon currently only going via RF to the ISS. Maybe I will have it use TCPIP beacons at some point. I see most hardcore SatGate operators have two IDs/radios one for a TCPIP beacon SatGate and one is a standalone RF only beacon so that way they advertise their SatGate 24/7 and still have a ID that shows only when the ISS received them.

### Related blog posts

* [g7vrd.co.uk/simple-hf-aprs-digipeater-howt](https://g7vrd.co.uk/simple-hf-aprs-digipeater-howto)
* [g7vrd.co.uk/simple-receive-only-hf-aprs-igate-howto](https://g7vrd.co.uk/simple-receive-only-hf-aprs-igate-howto)

### LOGFILE

These are full decodes.

```
chan,utime,isotime,source,heard,level,error,dti,name,symbol,latitude,longitude,speed,course,altitude,frequency,offset,tone,system,status,telemetry,comment
0,1592131941,2020-06-14T10:52:21Z,EI7BFB,RS0ISS,22(8/8),1,=,EI7BFB,/`,53.390833,-6.288333,,,,,,,,,,"EMAIL: EI7BFB1@gmail.com, Best Wishes & Good Health {UISS54}"
0,1592388330,2020-06-17T10:05:30Z,EA2AUK,RS0ISS,20(8/9),0,=,EA2AUK,/-,40.327833,-1.094333,,,,,,,,,,[Pedro en Teruel Via ISS 73]
0,1592388343,2020-06-17T10:05:43Z,IK1COA-1,RS0ISS,19(9/9),0,',IK1COA-1,\K,44.359333,9.223833,0.0,,,,,,Kenwood TM-D710,In Service,,73 DE IK1COA  Will End !
0,1592388347,2020-06-17T10:05:47Z,M0TDW-7,RS0ISS,22(8/9),1,`,M0TDW-7,/[,51.361000,-1.546333,0.0,322.0,141.0,,,,Mic-Emsg,In Service,,de Bill IO91fi 73 M0TDW_0
```
### Console

This is a console output that I `tee` to a log file for more verbose info about the IGate using flags: `-d ii -d fff`.

I was tweaking my config here and there so there are some errors. However I got some nice decodes coming in towards the end of my config alterations. The `LOGFILE` seems to not work with `~/` path so explicitly state the full path in the config.

```
Digipeater RS0ISS audio level = 24(9/9)   [SINGLE]   ____:____
[0.4] M0TDW-7>U1RQVV,RS0ISS*,WIDE2-1:`w<jl"x[/`"5K}de Bill IO91fi 73 M0TDW_0<0x0d>
MIC-E, Human, Mic-Emsg, In Service
N 51 21.6600, W 001 32.7800, 0 MPH, course 292, alt 469 ft
de Bill IO91fi 73 M0TDW_0

RS0ISS-1 audio level = 20(9/8)   [NONE]   ||||||___
[0.2] RS0ISS-1>EC5W-1:(RR res, n(r)=5, f=1)

RS0ISS-1 audio level = 20(9/9)   [NONE]   _|||||___
[0.3] RS0ISS-1>EC5W-1:(I cmd, n(s)=3, n(r)=6, p=0, pid=0xf0)<0x0d>

RS0ISS-1 audio level = 19(8/9)   [NONE]   ||||||___
[0.2] RS0ISS-1>EC5W-1:(I cmd, n(s)=4, n(r)=6, p=0, pid=0xf0)<0x0d>

RS0ISS-1 audio level = 18(9/9)   [NONE]   |||||||__
[0.3] RS0ISS-1>EC5W-1:(I cmd, n(s)=5, n(r)=6, p=0, pid=0xf0)<0x0d>

RS0ISS-1 audio level = 20(9/9)   [NONE]   |||||||__
[0.3] RS0ISS-1>EC5W-1:(I cmd, n(s)=6, n(r)=6, p=1, pid=0xf0)International Space Station<0x0d>

RS0ISS-1 audio level = 23(9/9)   [NONE]   ____|____
[0.4] RS0ISS-1>EC5W-1:(RR cmd, n(r)=6, p=1)

RS0ISS-1 audio level = 25(9/8)   [NONE]   ___||____
[0.3] RS0ISS-1>EC5W-1:(I cmd, n(s)=1, n(r)=6, p=0, pid=0xf0)<0x0d>

RS0ISS-1 audio level = 21(8/9)   [NONE]   ___|||___
[0.4] RS0ISS-1>EA2BJM-2:(RR cmd, n(r)=0, p=1)
[rx>ig] #

RS0ISS-1 audio level = 19(8/8)   [NONE]   |||||||__
[0.3] RS0ISS-1>EA2BJM-2:(RR cmd, n(r)=0, p=1)

RS0ISS audio level = 19(8/9)   [NONE]   ||||||___
[0.2] RS0ISS>CQ:Logged on to RS0ISS's Personal Message System<0x0d>
Unknown APRS Data Type Indicator "L"
Opening log file "~/direwolf-satgate-rx.log"
Can't open log file "~/direwolf-satgate-rx.log" for write.
No such file or directory

Rx IGate: Truncated information part at CR.
rx_to_ig_allow? 54969 "RS0ISS>CQ:Logged on to RS0ISS's Personal Message System"
rx_to_ig_allow? YES, no dedupe checking
[rx>ig] RS0ISS>CQ,qAO,2E0PGS-6:Logged on to RS0ISS's Personal Message System

RS0ISS audio level = 20(8/9)   [SINGLE]   _:::::___
[0.3] RS0ISS>CQ:on board the International Space Station<0x0d>
Unknown APRS Data Type Indicator "o"

RS0ISS audio level = 19(8/9)   [NONE]   ||||||:__
[0.3] RS0ISS>CQ:<0x0d>
Unknown APRS Data Type Indicator "^M"
Rx IGate: Truncated information part at CR.
Rx IGate: Information part length is zero.

RS0ISS audio level = 20(9/9)   [NONE]   ::::||___
[0.4] RS0ISS>CQ:You have mail waiting.<0x0d>
Unknown APRS Data Type Indicator "Y"
Rx IGate: Truncated information part at CR.
rx_to_ig_allow? 43383 "RS0ISS>CQ:You have mail waiting."
rx_to_ig_allow? YES, no dedupe checking
[rx>ig] RS0ISS>CQ,qAO,2E0PGS-6:You have mail waiting.

RS0ISS audio level = 19(9/8)   [NONE]   ||||||___
[0.2] RS0ISS>CQ:<0x0d>
Unknown APRS Data Type Indicator "^M"
Rx IGate: Truncated information part at CR.
Rx IGate: Information part length is zero.

RS0ISS audio level = 20(8/9)   [SINGLE]   _:::::___
[0.3] RS0ISS>CQ:Msg # Stat  Date     Time  To     From   @ BBS  Subject<0x0d>
Unknown APRS Data Type Indicator "M"

Digipeater RS0ISS audio level = 20(8/9)   [NONE]   :::|||___
[0.3] EA2AUK>CQ,RS0ISS*:=4019.67N/00105.66W-[Pedro en Teruel Via ISS 73]<0x0d>
Position, House
N 40 19.6700, W 001 05.6600
[Pedro en Teruel Via ISS 73]
Opening log file "/home/peter/direwolf-satgate-rx.log"
Rx IGate: Truncated information part at CR.
rx_to_ig_allow? 53197 EA2AUK>CQ:=4019.67N/00105.66W-[Pedro en Teruel Via ISS 73]
rx_to_ig_allow? YES, no dedupe checking
[rx>ig] EA2AUK>CQ,RS0ISS*,qAO,2E0PGS-6:=4019.67N/00105.66W-[Pedro en Teruel Via ISS 73]
[rx>ig] #

Digipeater RS0ISS audio level = 19(9/9)   [NONE]   ||||||___
[0.2] IK1COA-1>T4RQU6,RS0ISS*,APRSAT:'<0x7f>)Gl <0x1c>K\]73 DE IK1COA  Will End !=<0x0d>
MIC-E, Kenwood HT (W), Kenwood TM-D710, In Service
N 44 21.5600, E 009 13.4300, 0 MPH
73 DE IK1COA  Will End !
Rx IGate: Truncated information part at CR.
rx_to_ig_allow? 20902 "IK1COA-1>T4RQU6:')Gl K\]73 DE IK1COA  Will End !="
rx_to_ig_allow? YES, no dedupe checking
[rx>ig] IK1COA-1>T4RQU6,RS0ISS*,APRSAT,qAO,2E0PGS-6:'<0x7f>)Gl <0x1c>K\]73 DE IK1COA  Will End !=

Digipeater RS0ISS audio level = 22(8/9)   [SINGLE]   ____:____
[0.4] M0TDW-7>U1RQVV,RS0ISS*,WIDE2-1:`w<jl#2[/`"5I}de Bill IO91fi 73 M0TDW_0<0x0d>
MIC-E, Human, Mic-Emsg, In Service
N 51 21.6600, W 001 32.7800, 0 MPH, course 322, alt 463 ft
de Bill IO91fi 73 M0TDW_0
[rx>ig] #
```

## Successfully IGated packets

Where I been first to IGate something and no duplicate packets were rejected on server side.

```
00:00:10:46 : EA2AUK]CQ,RS0ISS*,qAO,2E0PGS-6:=4019.67N/00105.66W-[Pedro en Teruel Via ISS 73]
```

## SGATE path

* [amsat.org/pipermail/amsat-bb/2015-June/053382](https://www.amsat.org/pipermail/amsat-bb/2015-June/053382.html)
* [aprs.net.au/satellite/aprs-satellite/](http://www.aprs.net.au/satellite/aprs-satellite/)

## Closing notes

I am yet to have the ISS hear my Dire Wolf beacon so there is more tweaking required with Dire Worlf.

Dire Worlf is currently working for decoding as a iGate/SatGate though.

However I am happy the ISS did hear my hack job script beacon.

I should try the ISS cross band voice repeater.

I should also try recieve ISS SSTV images.

## Useful links

* [ISS Frequencies](https://issfanclub.eu/iss-frequencies/)
* [Contact the ISS](https://www.ariss.org/contact-the-iss.html)
* [Current Status of ISS Stations](https://www.ariss.org/current-status-of-iss-stations.html)
* [ARISS-SSTV images](https://ariss-sstv.blogspot.com/)
* [AMSAT Online Satellite Pass Predictions](https://www.amsat.org/track/)
* [APRS_via_SAT](http://www.aprs-dl.de/?APRS_Detailwissen:APRS_via_SAT%2FISS)
* [PSAT - APRS and a new PSK31 Approach](http://aprs.org/psat.html)
* [PCSAT-1, Amateur Radio Satellite NO-44](http://aprs.org/pcsat.html)
* [APRS Satellite Traffic and Reporting System](http://aprs.org/astars.html)
* [Amateur Radio Stations heard via Satellite](http://www1.findu.com/cgi-bin/pcsat.cgi)
* [isstracker.com](http://www.isstracker.com) (slightly inaccurate or slow to update)
* [AMSAT passtimes and tracking](http://amsat.org.ar/pass.htm) (pass times and sat tracking)