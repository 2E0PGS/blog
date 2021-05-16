---
layout: post
title: Arduino USB serial input
date: 2021-05-16 14:06:19
author: Peter Stevenson
summary: Parsing input commands
categories: programming
thumbnail:
tags:
 - Arduino
---

# Arduino USB serial input

Originally I was going to try use JSON coming from a API developer perspective but there doesn't seem to be much adoption of that for serial comms.

One of the few posts I found discussing JSON at the time (2019 odd as I sat on this post): [Sending JSON over serial in Python to Arduino](https://stackoverflow.com/questions/55698070/sending-json-over-serial-in-python-to-arduino)

In the end I decided KISS (Keep It Simple S...) and used comma separated values.

```
setcustom, 255, 255, 0
```

Return a ack/OK if all is good.

See the example: [open-fusion-led-controller-arduino.ino#lines-214](https://bitbucket.org/2E0PGS/open-fusion-led-controller-arduino/src/de2ba7c1c272b5f0cb387c5028f5e0474c83e7d4/open-fusion-led-controller-arduino.ino#lines-214)

The other option is new line (`\n`) only but you are limited to commands with no key value pairs e.g. `reboot` 

## Also see

* My blog pots on: [.NET Framework Win Form OFLC serial client](https://2e0pgs.github.io/blog/programming/2020/10/21/dotnet-framework-win-form-oflc-serial-client/)
* OFLC serial API documentation: [Arduino USB serial API](https://bitbucket.org/2E0PGS/open-fusion-led-controller-main/src/master/docs/arduino-usb-serial-api.md)