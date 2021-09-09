---
layout: post
title: DV4 CCTV file format conversion
date: 2021-09-09 20:16:10
author: Peter Stevenson
summary: How I processed CCTV custom DV4 file format into MP4
categories: sysadmin
thumbnail:
tags:
 - Linux
---

# How I processed CCTV custom DV4 file format into MP4

## Exporting

* I found my DVR required a FAT32 MBR formatted file system USB stick or USB HDD.

## Part files

* The files when exported from the CCTV will split at 2.1 GB into part files once the clip exceeds 2.1 GB in size.
* You will need to reassemble them before making a MP4.
* Turns out these can simply be concatenated together into one large blob: `cat filename*.dv4 > filename.cat.dv4`
* Real example: `cat CH04_2021JAN01000000_1.dv4 CH04_2021JAN01000000_1.part02.dv4 > CH04_2021JAN01000000_1.cat.dv4`
* _VLC didn't work on raw DV4 files. mpv does and so does ffplay._

## Conversion

* After reassembling the part files you need to then do a container copy: `ffmpeg -i filename.cat.dv4 -c:v copy -c:a copy -y filename.cat.dv4.mp4`
* Real example: `ffmpeg -i CH04_2021JAN01000000_1.cat.dv4 -c:v copy -c:a copy -y CH04_2021JAN01000000_1.cat.dv4.mp4`

## bulk-process.sh

Below is just a script to bulk process the concatenated files in a batch as it's rather time consuming if you have more than one camera channel or date range exported.

```sh
#!/bin/bash

for i in *.cat.dv4; do
		ffmpeg -i "$i" -c:v copy -c:a copy -y "$i".mp4
done
```