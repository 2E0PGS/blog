---
layout: post
title: youtube-dl Termux
date: 2019-10-07 18:15:10
author: Peter Stevenson
summary: Download YouTube videos on Android with Termux.
categories: Android
thumbnail:
tags:
 - Android
 - Termux
 - youtube-dl
---

# youtube-dl on Android with Termux

1. Install Termux from the Google Play Store or FDroid store.
2. Open Termux.
3. Run the command: `pkg install python`
4. Run the command: `pip install youtube-dl`

you now have youtube-dl on Android!

You may want to enable Termux storage access to rest of Android system: `termux-setup-storage`

## Example video download

`youtube-dl https://www.youtube.com/watch?v=abcdefg`

## Example audio download

You may need ffmpeg installed for this.

`youtube-dl -x https://www.youtube.com/watch?v=abcdefg`
