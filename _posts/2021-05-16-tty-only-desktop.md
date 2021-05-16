---
layout: post
title: tty only desktop
date: 2021-05-16 14:54:10
author: Peter Stevenson
summary: Useful tools for a tty only desktop.
categories: sysadmin
thumbnail:
tags:
 - FreeBSD
 - Linux
 - command-line
---

# tty only desktop

Sometimes you don't need a full X server setup + window manager. Below are some of the tools I use for my tty only desktops.

* `mpv video.mp4 --vo=drm` Play MPV videos direct without a window manager or X.
* `mpv video.mp4 --no-video` Play MPV video audio only.
* `mpv audio.mp3` Play MPV audio files.
* `vim` Text editor.
* `bc -l` Best Calculator.
* `ranger` File manager.
* `mocp` TUI (Text User Interface) music player.
* `apm` FreeBSD check laptop power.
* `cat /sys/class/power_supply/*/capacity` Linux check laptop power.
* `sensors` Check system temperature sensors.
* `htop` Task manager.
* `tmux`Terminal multiplexer.
* `irssi`IRC chat.
* `cal` Calendar.
* `youtube-dl` Downloading content to play on MPV.
* `speedtest-cli` TUI for speedtest.net
* `mps-youtube` TUI for YouTube.
* `stig` TUI for Transmission torrent client.

## Also see

* My blog post: [Everyday Termux uses](https://2e0pgs.github.io/blog/android/2019/10/11/everyday-termux-uses/)

