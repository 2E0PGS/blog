---
layout: post
title: Arduino package management
date: 2021-03-07 16:12:19
author: Peter Stevenson
summary: How I backup Arduino IDE and libraries on Linux
categories: programming
thumbnail:
tags:
 - Ardunio
 - Bash
---

# How I backup Arduino IDE and libraries on Linux

Below is the rsync script I use. I rsync the home folder Arduino libraries to a network mount (NAS). This is because changing a library version independent of the compiler or other code in a project can cause breakages. Normally libraries would be versioned in a project but Arduino doesn't have such project management natively at time of writing

I also rsync the IDE itself so the IDE/Compiler version is stored against the current library versions. Notice my IDE is running from a folder in `downloads`.

You will need to adjust these paths for your source and destination folders. The rsync command flags explained are: `-a` for archive and mirror directory structure, `--progress` show the ETA for the transfer then you just specify the `<src>` and `<dst>` directories.

## arduino-backup.sh

```sh
#!/bin/bash

rsync -a --progress ~/Arduino/libraries /mnt/foo/backups/arduino/

rsync -a --progress ~/downloads/arduino-1.8.8 /mnt/foo/backups/arduino/

```

I have seen other large Arduino based projects on GitHub doing essentially the same by using a shell script called `./fetch_deps.sh` with `git clone` hardcoded `git checkout` commit id strings. See: [disaster-radio/fetch_deps.sh](https://github.com/sudomesh/disaster-radio/blob/5ee98264895dea021dd5ac8b95eb3fadec50965e/fetch_deps.sh)

## Alternatives

The best alternative I have seen is a actual package manager/project manager for IoT which aims to unify the tool chain for embedded systems development and works with Arduino, see: [PlatformIO](https://platformio.org/install/cli)

## Notes

This post has been a draft a while and is relevant to Arduino IDE versions around: 1.8.8, 1.8.12

A friend of mine recently ran into a library versioning issue so I thought I better finally get this post out.
 
Seems the new Ardunio IDE version 2.0.0-beta.3 is based on VS Code. Everything is slowing going this way it seems. Something I touched on in my blog post: [A state of text editors in late 2019](https://2e0pgs.github.io/blog/programming/2019/12/07/a-state-of-text-editors-in-late-2019/)