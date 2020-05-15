

headers etc

# A state of tuners

Using free and open-source software.

CCTV, Satellite, Terrestrial and amateur TV.

## Software

vlc

mpv

w_scan

[kaffeine](https://kde.org/applications/multimedia/org.kde.kaffeine)

[SDRangel](https://github.com/f4exb/sdrangel)

## Modes

### Digital

DVB-T

DVB-T2

DVB-S

DVB-S2

DVB-C

### Analog

PAL-I

## Adapters

You will need a range of adapters if you wish to use RTL devices.

75 Ohm coax.

Bias Tee for LNA power.

## Polarisation

### Single LNB or Yagi

Horizontal

Vertical

### Two LNBs

LH (Left Horizontal)

RH (Right Horizontal)

### Special

Circular

### Quad LNB

High Horizontal

Low Horizontal

High Vertical

Low Vertical

## Amps

TV terrestrial distribution amplifier

## Antenna systems

UHF Yagi.

VHF Yagi.

LNB dish.

## Test signals

578 MHz aka 578000 KHz UK broadcast TV

1.316 GHz aka 1316 MHz aka 1316000 KHz GB3ZZ

## Bus speeds for tuner devices

USB 2.0 revised = 60 MBps 

PCI standard 32 bit = 133 MBps

PCI-E x4 = 800 MBps (am I using 3.0?)

## Linux paths

Analog video sources: `/dev/video0`

Digital TV sources: `/dev/dvb/adapter0`

## Other experiments

Using a TV antenna for ham radio band reception.

DAB decoding using RTL dongle and FOSS software.

## TODO

Include various GQRX screenshots comparing SNR/db level.

Include VLC codec screenshots.

PPM offsets. Megasymbols. etc

## Device table

| Type                 | Brand                 | Model            | Chipset           | Frequency range | Modes                | Inputs                                                | OS             | Driver           | Software support    | PPM offset | Docs                                                                                   | Notes                        |
|----------------------|-----------------------|------------------|-------------------|-----------------|----------------------|-------------------------------------------------------|----------------|------------------|---------------------|------------|----------------------------------------------------------------------------------------|------------------------------|
| PCI-E                | Hauppauge             | WinTV-HVR-1200   | NXP TDA10048HN    |                 | DVB-T                | Belling-Lee, F-type, RCA, 3.5 mm jack and S-Video DIN | Linux          | V4L2             | Kaffeine, Tvheadend |            | [linuxtv](https://linuxtv.org/wiki/index.php/Hauppauge_WinTV-HVR-1200)                 |                              |
| PCI universal 32 bit | AsusTek               | LNA Tiger Hybrid | Philips TDA10046H | 51 - 858 MHz    | DVB-T, Analog        | Belling-Lee                                           | Windows, Linux | DirectShow, V4L2 | VLC, Tvheadend      |            |                                                                                        | Firmware required for Linux. |
| USB 2                | EasyCAP               |                  |                   |                 | Analog               | RCA                                                   |                |                  |                     |            |                                                                                        | Designed for CCTV DVR.       |
| USB 2                | NewGen                | RTL2832          |                   | 25 - 1760 MHz   | DVB-T                | SMA                                                   | Linux          | V4L2             | VLC, Tvheadend      | 0.5        |                                                                                        |                              |
| USB 2                | DVB-T+FM+DAB          | 820T2            |                   |                 | DVB-T                | MCX                                                   | Linux          | V4L2             | VLC, Tvheadend      |            |                                                                                        |                              |
| USB 2                | Dual                  | RTL              |                   |                 | DVB-T                | SMA, SMA                                              | Linux          | V4L2             | VLC, Tvheadend      |            |                                                                                        |                              |
| USB 2                | Xbox One              | Digital TV Tuner | Panasonic MN88472 |                 | DVB-T, DVB-T2, DVB-C | Belling-Lee                                           | Linux          | V4L2             | Kaffeine, Tvheadend |            | [linuxtv](https://www.linuxtv.org/wiki/index.php/Xbox_One_Digital_TV_Tuner)            | Firmware required for Linux. |
| PCI universal 32 bit | Pinnacle PCTV Systems | 4000i            |                   |                 | DVB-S                | F-type                                                | Windows        |                  |                     |            | [linuxtv](https://www.linuxtv.org/wiki/index.php/Pinnacle_PCTV_Dual_Sat_Pro_PCI_4000I) | No Linux support as of yet.  |
| PCI-E                | TBS                   | TBS-6980         |                   |                 | DVB-S, DVB-S2        | F-type, F-type                                        | Linux          | V4L2             |                     |            | [linuxtv](https://linuxtv.org/wiki/index.php/TBS6980)                                  |                              |

## AsusTek - LNA Tiger Hybrid signal issues

Windows VLC: Not good signal at all. Very blocky. For some reason will decode in DVB-T2 but I don't believe it really is doing V2. See screenshots of VLC codec info.

Linux VLC: No decode as of yet. Sometimes syncs but never a image or error under `-vvv` console output.

## Firmware

## AsusTek LNA Tiger Hybrid

```
wget https://github.com/OpenELEC/dvb-firmware/blob/master/firmware/dvb-fe-tda10046.fw?raw=true -O dvb-fe-tda10046.fw
```

## Xbox One Digital TV Tuner

```
wget http://palosaari.fi/linux/v4l-dvb/firmware/MN88472/02/latest/dvb-demod-mn88472-02.fw
cp dvb-demod-mn88472-02.fw /lib/firmware
sudo reboot
```

## Tvheadend tests

Tests are carried out using the following two "Pre-defined muxes":

"--Generic--: auto-With167kHzOffsets" adds 155 muxes

"United Kingdom: uk-Mendip" brings us up to 157 muxes total to scan

~~Make sure you check and un-check enabled against each `Autorecs` item after changes to channel mappings for the auto recordings to work properly. Otherwise it seems to miss some with `Status:Invalid`.~~

~~Make sure you don't have any `Autorecs` with channel specific filters. They will be lost when removing channels for remapping.~~

"Recording"/"Digital Video Recorder Profiles" sensible format string: `$t/%F-%R-$t$-e$n.$x` 

Example path created: `Beat-The-Chasers/2020-04-29-23-45-Beat-The-Chasers.ts` 

Example duplicate path created: `Beat-The-Chasers/2020-04-29-23-45-Beat-The-Chasers-1.ts`

It's a shame its not lowercase but it's as good as I can hope.
