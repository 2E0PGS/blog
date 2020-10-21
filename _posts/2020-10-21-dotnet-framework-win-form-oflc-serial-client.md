---
layout: post
title: .NET Framework Win Form OFLC serial client
date: 2020-10-21 19:57:19
author: Peter Stevenson
summary: Updates to the OFLC client for Windows
categories: programming
thumbnail:
tags:
 - .NET
 - C#
 - Arduino
 - Raspberry Pi
---

# Updates to my OFLC .NET Framework Windows Form client

OFLC stands for Open Fusion LED Controller. [[Video]](https://www.youtube.com/watch?v=h-zARerEpG0) [[Main repo]](https://bitbucket.org/2E0PGS/open-fusion-led-controller-main) [[Win form repo]](https://bitbucket.org/2E0PGS/open-fusion-led-controller-win-remote)

## Original

Based on my old deprecated [Web Arduino](https://bitbucket.org/2E0PGS/open-fusion-led-controller-web-arduino) API.

Doesn't support USB serial.

![oflc-remote-running](/blog/assets/2020-10-21/oflc-remote-running.png)

![oflc-remote-solution](/blog/assets/2020-10-21/oflc-remote-solution.png)

## New

Previously I didn't have a native USB serial client. This now allows for a full feature set client for serial only applications with Windows. Anything using web based Raspberry Pi could already make use of my [web HTML client](https://bitbucket.org/2E0PGS/open-fusion-led-controller-raspberrypi/).

Supports full USB serial. Based on my [Arduino USB serial API](https://bitbucket.org/2E0PGS/open-fusion-led-controller-main/src/master/docs/arduino-usb-serial-api.md)

Will soon support my [Raspberry Pi Python Flask web API](https://bitbucket.org/2E0PGS/open-fusion-led-controller-main/src/master/docs/pi-python-flask-web-api.md)

![oflc-win-remote-running](/blog/assets/2020-10-21/oflc-win-remote-running.png)

![oflc-win-remote-solution](/blog/assets/2020-10-21/oflc-win-remote-solution.png)

## Also see

* My blog post on [Arduino USB serial]() TBA