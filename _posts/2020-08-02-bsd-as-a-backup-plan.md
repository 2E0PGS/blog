---
layout: post
title: BSD as a backup plan
date: 2020-08-02 15:22:19
author: Peter Stevenson
summary: BSD as a backup plan to Linux
categories: sysadmin
thumbnail:
tags:
 - FreeBSD
 - Workstation
 - Linux
 - OpenBSD
 - GNU
 - TrueOS
 - GhostBSD
---

# BSD as a backup plan to Linux

Many open source advocates put a lot of trust in Linux.

Lets say Linux becomes infected by people will ill intention or the code base becomes unusable due to differences in opinion.

What other free open source operating systems with a non Linux kernel are there?

Well BSD is probably the first that comes to mind.

## OpenBSD or FreeBSD you may ask?

FreeBSD... You can roll your own with their installer and grab a desktop afterwards. I prefer it CLI only and use it on servers and IBM laptops.

The FreeBSD `pkg` package repos are quite extensive these days otherwise you can try FreeBSD ports.

The only two I recommend are FreeBSD and GhostBSD (based on FreeBSD/TrueOS) for desktop use. 

## GhostBSD

If you just want a graphical FreeBSD based distro that is the dogs, then look no further than GhostBSD. Yes I have tried many other distros such as DragonflyBSD etc.

GhostBSD has basically everything you need. Besides games and a couple programs that are Electron based. It's perfectly usable out the box.

I did find one odd bug with hot plugging my keyboard/mice while the system is running. I thought my USB switcher was borked but it seems GhostBSD didn't like me removing the keyboard/mouse and re-plugging while it was running. I forget what `lsusb` showed. I will test on my laptop shortly...

`var/log/syslog` showed connection changes but Xorg problem? No mouse after re-plug.

Anyway I have ran GhostBSD on Dell desktops and IBM laptops.

Oh its missing full file system encryption too. Not sure how to implement that yet.

## Others

Perhaps GNU Hurd will be more viable soon. Otherwise some open source XNU/Darwin based OS could also work. NetBSD I need to try again as it's been a long time since I last looked at it.