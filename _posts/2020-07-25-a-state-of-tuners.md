---
layout: post
title: A state of tuners
date: 2020-07-25 13:02:19
author: Peter Stevenson
summary: DATV, IPTV and tuners
categories: hamradio
thumbnail:
tags:
 - Tvheadend
 - ATV
---

# A state of tuners

PCI tuners, SDR, SAT>IP, IPTV and open source software.

Covering digital modes of satellite, terrestrial and amateur TV.

## Software

The following software is useful for testing.

* [VLC](https://www.videolan.org)
* [mpv](https://mpv.io/)
* [w_scan](https://linuxtv.org/wiki/index.php/W_scan)
* [kaffeine](https://kde.org/applications/multimedia/org.kde.kaffeine)
* [SDRangel](https://github.com/f4exb/sdrangel)
* [Tvheadend](https://github.com/tvheadend/tvheadend) See: [Tvheadend installation](#tvheadend-installation)

## Modes

### Digital

* DVB-T
* DVB-T2
* DVB-S
* DVB-S2
* DVB-C

### Analog

* PAL-I

## Adapters and accessories

You will need a range of adapters if you wish to use RTL devices.

* 75 Ohm coax.
* Bias Tee for LNB power.
* LNB DC power blocker. Used for connecting a passive device like a Yagi to a tuner which supplies LNB power. Do not trust the LNB power option inside the set top box settings. Sometimes there isn't even a option. See eBay listing: [F Screw Type DC Power Blocking Blocker Inline In Line Connector Coupler 100994](https://www.ebay.co.uk/itm/F-Screw-Type-DC-Power-Blocking-Blocker-Inline-In-Line-Connector-Coupler-100994/112227310735)

## Polarisation

### Yagi

* Horizontal
* Vertical

### Special

* Circular

### Universal LNBs

This is specific to europe.

Ref: [wiki/Low-noise_block](https://en.wikipedia.org/wiki/Low-noise_block_downconverter#Universal_LNB_(%22Astra%22_LNB))

| Voltage |  Tone  | Polarization |     Frequency band    |
|---------|--------|--------------|-----------------------|
| 13 V    | 0 kHz  | Vertical     | 10.70–11.70 GHz, low  |
| 18 V    | 0 kHz  | Horizontal   | 10.70–11.70 GHz, low  |
| 13 V    | 22 kHz | Vertical     | 11.70–12.75 GHz, high |
| 18 V    | 22 kHz | Horizontal   | 11.70–12.75 GHz, high |

## Antenna systems

* UHF Yagi. Most common for modern terrestrial DVB-T, DVB-T2 Freeview. If doing DATV DVB-S you must use a LNB DC power blocker.
* VHF Yagi. Old analog TV.
* Universal LNB + dish. Most common for satellite DVB-S, DVB-S2 Freesat. Also can be used for: QO-100/Es'hail-2

## Test signals

| Station                     | Type  | GHz    | MHz   | KHz      | Hz          | Polarity      | Megasymbols/s | Kilosymbols/s | Symbols/s | FEC |
|-----------------------------|-------|--------|-------|----------|-------------|---------------|---------------|---------------|-----------|-----|
| GB3ZZ                       | DVB-S | 1.316  | 1316  | 1316000  | 1316000000  | Horizonal     | 4             | 4000          | 4000000   | 1/2 |
| Broadcast Freeview ITV      | DVB-T | 0.578  | 578   | 578000   | 578000000   | Horizonal     |               |               |           |     |
| Broadcast Freesat Channel 4 | DVB-S | 10.714 | 10714 | 10714000 | 10714000000 | Low Horizonal | 22            | 22000         | 22000000  | 5/6 |

## Bus speeds for tuner devices

Important to ensure we have the bandwidth for video.

* USB 2.0 revised = 60 MBps 
* PCI standard 32 bit = 133 MBps
* PCI-E x4 = 800 MBps (am I using PCI 3.0?)

## Linux paths

* Analog video sources: `/dev/video0`
* Digital TV sources: `/dev/dvb/adapter0`
* Useful Linux command to list DVB devices: `dmesg | grep adapter`

## Other experiments

* Using a TV antenna for ham radio band reception.
* DAB decoding using RTL dongle and FOSS software using: [welle.io](https://github.com/AlbrechtL/welle.io)

## Device table

| Type                 | Brand                 | Model            | Chipset           | Frequency range | Modes                | Inputs                                                | OS             | Driver           | Software support    | Docs                                                                                   | Notes                        |
|----------------------|-----------------------|------------------|-------------------|-----------------|----------------------|-------------------------------------------------------|----------------|------------------|---------------------|----------------------------------------------------------------------------------------|------------------------------|
| PCI-E                | Hauppauge             | WinTV-HVR-1200   | NXP TDA10048HN    |                 | DVB-T                | Belling-Lee, F-type, RCA, 3.5 mm jack and S-Video DIN | Linux          | V4L2             | Kaffeine, Tvheadend | [linuxtv](https://linuxtv.org/wiki/index.php/Hauppauge_WinTV-HVR-1200)                 |                              |
| PCI universal 32 bit | AsusTek               | LNA Tiger Hybrid | Philips TDA10046H | 51 - 858 MHz    | DVB-T, Analog        | Belling-Lee                                           | Windows, Linux | DirectShow, V4L2 | VLC, Tvheadend      |                                                                                        | Firmware required for Linux. |
| USB 2                | EasyCAP Somagic       | SMI-2021CBE      |                   |                 | Analog               | RCA                                                   |                |                  |                     | [linuxtv](https://linuxtv.org/wiki/index.php/Easycap)                                  | Designed for CCTV DVR.       |
| USB 2                | NewGen                | RTL2832          |                   | 25 - 1760 MHz   | DVB-T                | SMA                                                   | Linux          | V4L2             | VLC, Tvheadend      | [linuxtv](https://www.linuxtv.org/wiki/index.php/RealTek_RTL2832U)                     |                              |
| USB 2                | DVB-T+FM+DAB          | 820T2            |                   |                 | DVB-T                | MCX                                                   | Linux          | V4L2             | VLC, Tvheadend      |                                                                                        |                              |
| USB 2                | Dual                  | RTL              |                   |                 | DVB-T                | SMA, SMA                                              | Linux          | V4L2             | VLC, Tvheadend      |                                                                                        |                              |
| USB 2                | Xbox One              | Digital TV Tuner | Panasonic MN88472 |                 | DVB-T, DVB-T2, DVB-C | Belling-Lee                                           | Linux          | V4L2             | Kaffeine, Tvheadend | [linuxtv](https://www.linuxtv.org/wiki/index.php/Xbox_One_Digital_TV_Tuner)            | Firmware required for Linux. |
| PCI universal 32 bit | Pinnacle PCTV Systems | 4000i            |                   |                 | DVB-S                | F-type                                                | Windows        |                  |                     | [linuxtv](https://www.linuxtv.org/wiki/index.php/Pinnacle_PCTV_Dual_Sat_Pro_PCI_4000I) | No Linux support as of yet.  |
| PCI-E                | TBS                   | TBS-6980         |                   |                 | DVB-S, DVB-S2        | F-type, F-type                                        | Linux          | V4L2             | VLC, Tvheadend      | [linuxtv](https://linuxtv.org/wiki/index.php/TBS6980)                                  | Firmware required for Linux. |

Note: Generic RTL devices may require a PPM offset outside their intended use center frequency. For example the 820T2 mentioned above requires a PPM offset of 0.5 for UHF amateur bands.

## VLC DVB-T

* Quick and simple command to view TV on VLC using a RTL dongle: `vlc dvb-t://frequency=578000000:bandwidth=0`
* The docs state it should be in KHz but I get: `dtv stream error: 578000 Hz carrier frequency is too low.` so it looks like it's actually using Hz.
* Record `cvlc` transport stream headless: `cvlc dvb-t://frequency=578000000:bandwidth=0 :program=8384 --sout "#file{dst=/home/anon/recording.ts,no-overwrite}"`

## VLC DVB-S

* I found I had to specify polarity voltage and FEC: `vlc dvb-s://frequency=10714000000:srate=22000000:dvb-fec=5:voltage=18`
* The docs state it should be in KHz but I get: `dtv stream error: 10714000 Hz carrier frequency is too low.` so it looks like it's actually using Hz.

## AsusTek - LNA Tiger Hybrid signal issues

* Windows VLC: Not good signal at all. Very blocky. For some reason will decode in DVB-T2 but I don't believe it really is doing V2. See screenshots of VLC codec info.
* Linux VLC: No decode as of yet. Sometimes syncs but never a image or error under `-vvv` console output.

## Firmware

### AsusTek - LNA Tiger Hybrid

```sh
wget https://github.com/OpenELEC/dvb-firmware/blob/master/firmware/dvb-fe-tda10046.fw?raw=true -O dvb-fe-tda10046.fw
sudo cp dvb-fe-tda10046.fw /lib/firmware
sudo reboot
```

### Xbox One - Digital TV Tuner

```sh
wget http://palosaari.fi/linux/v4l-dvb/firmware/MN88472/02/latest/dvb-demod-mn88472-02.fw
sudo cp dvb-demod-mn88472-02.fw /lib/firmware
sudo reboot
```

### TBS - TBS-6980

Ref: [github.com/ljalves/linux_media](https://github.com/ljalves/linux_media/wiki/CX24117-firmware)

```sh
wget http://www.tbsdtv.com/download/document/common/tbs-linux-drivers_v130901.zip
unzip -p tbs-linux-drivers_v130901.zip linux-tbs-drivers.tar.bz2 | tar jxOf - linux-tbs-drivers/v4l/tbs6981fe_driver.o.x86_64 | dd bs=1 skip=10144 count=55486 of=dvb-fe-cx24117.fw
sudo cp dvb-fe-cx24117.fw /lib/firmware
sudo reboot
```

![pci-cards-internal.jpg](/blog/assets/2020-07-25/pci-cards-internal.jpg)

## Tvheadend installation

* I noticed it's easier to setup tuners if the tuner is already present with required firmware before starting Tvheadend for the first time. This is because the wizard guides you through based on the type of tuner.
* [Install Tvheadend on Ubuntu 18.04 server](https://lintut.com/install-tvheadend-on-ubuntu-18-04-server/)

## Tvheadend found services test

Tests are carried out using the following two "Pre-defined muxes":

`--Generic--: auto-With167kHzOffsets` adds 155 muxes

`United Kingdom: uk-Mendip` brings us up to 157 muxes total to scan

My adapters listed.

![tv-adapters.png](/blog/assets/2020-07-25/tv-adapters.png)

Comparison of networks found by each with the same test muxes to scan.

![networks.png](/blog/assets/2020-07-25/networks.png)

## Freeview DVB-T HD channels

To get HD services I had to manually add the mux. For some reason the predefined muxes don't include HD.

![tvheadend-dvb-t-hd-channels-mux.png](/blog/assets/2020-07-25/tvheadend-dvb-t-hd-channels-mux.png)

Scanning the custom mux pulls in the following HD services.

![tvheadend-dvb-t-hd-channels-services.png](/blog/assets/2020-07-25/tvheadend-dvb-t-hd-channels-services.png)

I did find when watching a HD channel it seemed to mess up my EPG. Not sure why.

## Tvheadend recordings

~~Make sure you check and un-check enabled against each `Autorecs` item after changes to channel mappings for the auto recordings to work properly. Otherwise it seems to miss some with `Status:Invalid`.~~ Seems you just need to wait for the EPG to update itself and recordings match up again.

Make sure you don't have any `Autorecs` with channel specific filters. They will be lost when removing channels for remapping.

"Recording"/"Digital Video Recorder Profiles" sensible format string: `$t/%F-%R-$t$-e$n.$x` 

Example path created: `Beat-The-Chasers/2020-04-29-23-45-Beat-The-Chasers.ts` 

Example duplicate path created: `Beat-The-Chasers/2020-04-29-23-45-Beat-The-Chasers-1.ts`

It's a shame it's not lowercase but it's as good as I can hope for.

## RTSP in Tvheadend

```sh
pipe:///usr/bin/ffmpeg -loglevel fatal -i rtsp://admin:password@192.168.1.40:80/stream -vcodec copy -acodec copy -metadata service_provider=cctv -metadata service_name=ch01 -f mpegts -tune zerolatency pipe:1
```

Here I reference my [RTSP blog post](https://2e0pgs.github.io/blog/networking/2019/10/07/rtsp-ip-tables/) but this time im using Tvheadend instead of a desktop running MPV as a client.

* [How_to_add_a_stream_to_Streaming_server](https://www.tbsiptv.com/download/moipro-amd/How_to_add_a_stream_to_Streaming_server.pdf)
* [tvheadend.org/boards/5/topics/26121?r=27302](https://tvheadend.org/boards/5/topics/26121?r=27302)

## SDRangel DATV

Before I bought a set top box I thought I would try decoding GB3ZZ using SDRagels built in DVB-S decoder along with my HackRF. No LNB DC power blocker required here as the SDR doesn't output any bias normally. 

Overview with the wideband reception so you can see how the signal looks.

![datv-sdrangel-gb3zz-wide-band.png](/blog/assets/2020-07-25/datv-sdrangel-gb3zz-wide-band.png)

It proved very CPU intensive and I only just about got data decoded when I turned off spectrum waterfalls and reduced my bandwidth to only that of what I need to decode to save on CPU.

You can see the QSPK constellation is aligning up nicely.

![datv-sdrangel-gb3zz-narrow-band.png](/blog/assets/2020-07-25/datv-sdrangel-gb3zz-narrow-band.png)

This is the best decode I could get so far. I need to try a very fast clock for single thread performance.

![datv-sdrangel-gb3zz-decode-2.png](/blog/assets/2020-07-25/datv-sdrangel-gb3zz-decode-2.png)

## Blazer set top box DATV

I bought a cheap set top box from Amazon (Blazer HD4000). Most of the cheap chinese made ones allow you to set custom LNB settings.

![datv-blazer-hd400.png](/blog/assets/2020-07-25/datv-blazer-hd400.png)

Overview of the setup with Yagi and LNB DC power blocking.

![datv-blazer-gb3zz-overview.jpg](/blog/assets/2020-07-25/datv-blazer-gb3zz-overview.jpg)

### LNB frequency and blind scans

Initial LNB settings.

![datv-blazer-old-lnb-if.jpeg](/blog/assets/2020-07-25/datv-blazer-old-lnb-if.jpeg)

Performing a blind scan it finds GB3ZZ.

![datv-blazer-blind-scan.jpeg](/blog/assets/2020-07-25/datv-blazer-blind-scan.jpeg)

After completion it finds GB3ZZ on both horizontal and vertical which is to be expected given we are using a Yagi with no LNB polarization switching.

![datv-blazer-blind-scan-complete.jpg](/blog/assets/2020-07-25/datv-blazer-blind-scan-complete.jpg)

11916 MHz is the frequency the blind scan found GB3ZZ on. 1316 MHz is the actual TX frequency of GB3ZZ.

`11916 MHz - 1316 MHz = 10600 MHz`

Which corresponds with high band universal LNB local oscillator frequency on Wikipedia: 10.60 GHz.

So it looks like the set top box frequency display takes into account LNB local oscillator frequency.

We can if we want change our software LNB frequency to make the actual frequency calculation easier `- 10000` 10 GHz.

![datv-blazer-new-lnb-if.jpg](/blog/assets/2020-07-25/datv-blazer-new-lnb-if.jpg)

### GB3ZZ reception

The channel list shows the two services (GB3ZZ Ch1, GB3ZZ Ch2) within the one mux along with frequency and symbol rate.

![datv-blazer-gb3zz-channel-1.jpg](/blog/assets/2020-07-25/datv-blazer-gb3zz-channel-1.jpg)

GB3ZZ displays images when there are no active repeater users.

![datv-blazer-gb3zz-1.jpg](/blog/assets/2020-07-25/datv-blazer-gb3zz-1.jpg)

![datv-blazer-gb3zz-2.jpg](/blog/assets/2020-07-25/datv-blazer-gb3zz-2.jpg)

## Tvheadend DATV

After playing with Freeview and Freesat on Tvheadend and DATV on the set top box I thought it's time to combine the two and have DATV in Tvheadend.

TBS-6980 + LNB DC power blocker.

Creating a custom mux for GB3ZZ.

![tvheadend-gb3zz-mux-edit.png](/blog/assets/2020-07-25/tvheadend-gb3zz-mux-edit.png)

I did a test and noticed we need to account for the assumed 10.60 GHz universal LNB frequency.

![tvheadend-gb3zz-mux-list.png](/blog/assets/2020-07-25/tvheadend-gb3zz-mux-list.png)

Scanning that mux we find our two services (GB3ZZ Ch1, GB3ZZ Ch2).

![tvheadend-gb3zz-services.png](/blog/assets/2020-07-25/tvheadend-gb3zz-services.png)

Mapping those to channel list.

![tvheadend-gb3zz-channels.png](/blog/assets/2020-07-25/tvheadend-gb3zz-channels.png)

Signal strength isn't bad for my indoor antenna.

![tvheadend-signal-strength.png](/blog/assets/2020-07-25/tvheadend-signal-strength.png)

There are several benefits. I can see both channels at once unlike with most set top boxes. I can stream the DATV over IPTV to anywhere on my network.

![tvheadend-gb3zz-dual.png](/blog/assets/2020-07-25/tvheadend-gb3zz-dual.png)

## Useful links

* [en.SatExpat.com - List of Television Channels on Satellites Astra 2E/2F/2G 28.2° East](https://en.satexpat.com/sat/east/28.2/)
* [TP 10714 H — Satellite Astra 2E 28.2° East](https://en.satexpat.com/tp/28.2e/10714H)
* [VLC Documentation - Streaming a DVB Channel](https://wiki.videolan.org/Documentation:Streaming_HowTo/Stream_a_DVB_Channel/)

## TODO

* Include VLC codec screenshots.
* Megasymbols. etc
* DVBlast
* DATV QO-100/Es'hail-2 seperate blog post