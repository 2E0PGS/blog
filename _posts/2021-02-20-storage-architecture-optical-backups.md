---
layout: post
title: Storage architecture optical backups
date: 2021-02-20 19:18:19
author: Peter Stevenson
summary: Backing up data using optical media
categories: sysadmin
thumbnail:
tags:
 - Linux
---

## Backing up data using optical media

* When looking to purchase a Blu-ray burner watch out for Blu-ray drives which only read BD but write DVD and CD.

## Burning software

* [Disc spanning](https://en.wikipedia.org/wiki/Disc_spanning)
* Had some issues getting FLAC onto a CD-R audio project without codecs.
* Proxmox supports cd-rom passthrough but I had issues where it seemed the guest didn't have full control.
* VirtualBox works flawlessly so far.
	* Passing through a optical disc drive to guest for burning inside a VM: [forums.virtualbox.org/viewtopic.php?f=2&t=28871](https://forums.virtualbox.org/viewtopic.php?f=2&t=28871)

![virtualbox-passthrough](/blog/assets/2021-02-20/virtualbox-passthrough.jpg)

### Brasero

* Will create a MD5 checksum for you.
* Works on CD-R and DVD-R but not BD-R discs.
* Enabling the `burn the image directly without saving it to disc` option will skip making a local ISO cache before burning to disc in `/tmp`
* CD-R write speeds are one of the fastest I have tested so far as high as 34x. Using Pioneer, VM passthrough Windows host -> Linux guest and Sony 700 MB.
* Had it ask if I want to rename files for full Windows compatibility, I press **okay** it creates a checksum and then says do I want to continue **without** full Windows compatibility **OK** or **Cancel**. Normally this works fine so there must be some edge case.
* Crashes on audio projects often.

### K3B 

* You need to be careful with adding files up to the limit as you may get "mkisofs crashed" error.
* [Long File-Name Support for Burning Data Discs in Ubuntu with K3b](https://ubuntugenius.wordpress.com/2010/06/09/long-file-name-support-for-burning-data-discs-in-ubuntu-with-k3b/)
* Works on CD-R, DVD-R and BD-R discs.
* You will need to create your own checksum.
* Enabling `Create image` option will make a local ISO cache before burning to disc in `/tmp`.
* You will need to manually enable verify disc. This should really be on by default.
* DB-R write speeds only seen as high as 3x but have seen it dip to 1.7x. Using Pioneer, VM passthrough Windows host -> Linux guest and Verbatim 25 GB 6x.
* ["cdrecord has no permission to open the device"](https://askubuntu.com/questions/1032903/problem-with-k3b-in-18-04-releases-cdrecord-has-no-permission-to-open-the-devi)
* Calls mkisofs

### Windows Explorer

* [Live File System vs. Mastered Disc Formats in Windows](https://www.howtogeek.com/126547/htg-explains-live-file-system-vs.-mastered-disc-formats-in-windows/)
* Works on CD-R?, DVD-R? and BD-R discs.
* You will need to create your own checksum.
* Caches files into a temp `%AppData%` folder before burning.
* Doesn't show current burn speed.

### ImgBurn

* Works on CD-R?, DVD-R and BD-R discs.
* Gives some automatic file system recommendation that better suits MP3 BD-R data discs.
* You will need to create your own checksum. TODO: Check settings further.
* Produces a graph output which can be read in [DVDINFOPro](http://www.dvdinfopro.com/).
* [Possible to span large image across CD's?](https://forum.imgburn.com/index.php?/topic/6060-possible-to-span-large-image-across-cds/)
* [How to split a large file in 2 small ones](https://forum.imgburn.com/index.php?/topic/17001-how-to-split-a-large-file-in-2-small-ones/)
* DB-R 8.1x write speed. Uses 318 Mbps pulling from a NAS at those write speeds. Using Pioneer, VM passthrough Windows host -> Windows guest and Verbatim 25 GB 6x.
* DVD-R 13.4x write speed. Using Pioneer, VM passthrough Windows host -> Windows guest and Philips 4.7 GB 12x.

### Nero

* [SPLITTING LARGE FILES WITH NERO BURNING ROM](http://www.steves-digicams.com/knowledge-center/how-tos/video-software/splitting-large-files-with-nero-burning-rom.html)
* [How to Use Disc Span in Nero Burning ROM](https://youtu.be/JsMr7CHU0ZA)

## Write speed tests

| Id | Date       | Disc brand | Disc type | Write software | File source | IO type                              | Project type         | File type               | Write device | Write speed max | Write speed average | Result                      |
|----|------------|------------|-----------|----------------|-------------|--------------------------------------|----------------------|-------------------------|--------------|-----------------|---------------------|-----------------------------|
| 1  | 2020-12-12 | Verbatim   | BD-R      | K3B            | Local cache | Live Linux USB direct                | Data                 | MP4, MOV                | BDR-212M     | 3.3x            |                     | OK                          |
| 2  | 2020-12-12 | Verbatim   | BD-R      | K3B            | Remote SMB  | Windows -> VM passthrough -> Linux   | Data                 | MP4, MOV, JPG, TNL, MTS | BDR-212M     |                 |                     | ERROR Ran out of disc space |
| 3  | 2020-12-12 | Verbatim   | BD-R      | Windows        | Local cache | Windows direct                       | Data                 | MP4/MOV                 | BDR-212M     | N/A             | N/A                 | OK                          |
| 4  | 2020-12-12 | Verbatim   | BD-R      | K3B            | Local cache | Live Linux USB direct                | Data (FS Linux only) | MP4/MOV                 | BDR-212M     | 3.3x            |                     | OK                          |
| 5  | 2020-12-12 | Verbatim   | BD-R      | K3B            | Remote SMB  | Windows -> VM passthrough -> Linux   | Data                 | MP3                     | BDR-212M     | 2.4x            |                     | OK                          |
| 6  | 2020-12-14 | Verbatim   | BD-R      | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data                 | MP3                     | BDR-212M     | 8.1x            |                     | OK                          |
| 7  | 2020-12-14 | Sony       | CD-R      | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data                 | MP3                     | BDR-212M     | N/A             | N/A                 | ERROR                       |
| 8  | 2020-12-14 | Sony       | CD-R      | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data                 | MP3                     | BDR-212M     | N/A             | N/A                 | ERROR                       |
| 9  | 2020-12-14 | Philips    | DVD-R     | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data                 | MP3                     | BDR-212M     | 13.4x           |                     | OK                          |
| 10 | 2020-12-14 | Philips    | DVD-R     | K3B            | Local cache | Windows -> VM passthrough -> Linux   | Data                 | MP3                     | BDR-212M     | 15x             |                     | OK                          |
| 11 | 2020-12-14 | Philips    | DVD-R     | Brasero        | Local cache | Windows -> VM passthrough -> Linux   | Data                 | MP3                     | BDR-212M     | 16x             | 10.9x               | OK                          |
| 12 | 2020-12-14 | Sony       | CD-R      | Brasero        | Local cache | Windows -> VM passthrough -> Linux   | Data                 | MP3                     | BDR-212M     | 34x             | 24.9x               | OK                          |
| 13 | 2020-12-14 | Sony       | CD-R      | Windows        | Local cache | Windows direct                       | Data                 | MP3                     | BDR-212M     | N/A             | N/A                 | OK                          |
| 14 | 2020-12-07 | Sony       | CD-R      | Brasero        | USB3        | Linux direct                         | Data                 | MP3                     | SU-208GB     |                 |                     | OK                          |
| 15 | 2020-12-06 | Philips    | DVD-R     | Brasero        | USB3        | Linux direct                         | Data                 | MP3                     | SU-208GB     |                 |                     | OK                          |
| 16 | 2020-12-07 | Philips    | DVD-R     | K3B            | USB3        | Linux direct                         | Data                 | MP4                     | DU-8A5LH     |                 |                     | OK                          |

* BDR-212M is a Pioneer drive.
* SU-208GB is a TSSTcorp drive.
* DU-8A5LH is a drive commonly found in DELL laptops.
* Write software `Windows` refers to `Windows Explorer` but is kept short for brevity. 
* Some items maybe missing write speed if the software doesn't show it like in the case of `Windows Explorer`, otherwise I may have not recorded the speed at the time.

## Compatibility/playback tests

| WriteId | Read device | Result           |
|---------|-------------|------------------|
| 1       | PS3         | OK               |
| 1       | Xbox One S  | Error 0x80820002 |
| 3       | PS3         | OK               |
| 3       | Xbox One S  | Error 0x80820002 |
| 4       | PS3         | OK               |
| 4       | Xbox One S  | Error 0x80820002 |
| 5       | PS3         | OK               |
| 5       | Xbox One S  | Error 0x80820002 |
| 6       | PS3         | OK               |
| 6       | Xbox One S  | Error 0x80820002 |
| 9       | PS3         | OK               |
| 9       | Xbox One S  | Error 0x80820002 |
| 9       | LG DVX392H  | OK               |
| 10      | PS3         | OK               |
| 10      | Xbox One S  | Error 0x80820002 |
| 10      | LG DVX392H  | OK               |
| 11      | PS3         | OK               |
| 11      | Xbox One S  | Error 0x80820002 |
| 11      | LG DVX392H  | OK               |
| 12      | PS3         | OK               |
| 12      | Xbox One S  | Error 0x80820002 |
| 12      | LG DVX392H  | OK               |
| 13      | PS3         | OK               |
| 13      | Xbox One S  | Error 0x80820002 |
| 13      | LG DVX392H  | OK               |
| 14      | PS3         | OK               |
| 14      | Xbox One S  | Error 0x80820002 |
| 14      | LG DVX392H  | OK               |
| 15      | PS3         | OK               |
| 15      | Xbox One S  | Error 0x80820002 |
| 15      | LG DVX392H  | OK               |
| 16      | PS3         | OK               |
| 16      | Xbox One S  | Error 0x80820002 |
| 16      | LG DVX392H  | Err              |

* 1, 3 and 4 the PS3 wouldn't find the MOV files but this is a separate issue relating to PS3 codecs not a medium issue. It did however play MP4 just fine.

## Optical medium models

* [The Digital FAQ - Blank DVD Media Quality Review](http://www.digitalfaq.com/reviews/dvd-media.htm)
* [Guide to the Best Blank Blu-ray Discs](https://nerdtechy.com/best-blank-blu-ray-discs)
* [4. How Long Can You Store CDs and DVDs and Use Them Again?](https://www.clir.org/pubs/reports/pub121/sec4/)

### Verbatim

The Verbatim product line can be a bit confusing and their main website could do with updating. The EU website is a bit more modern but lacking clear comparisons. Luckily I finally found the information I was after here: [Verbatim Optical Media July 2020 Brochure](https://www.verbatim-marcom.com/media_files/news-media/files/Optical%20Media_July_2020_EN.pdf)

* Annoyingly you cannot get the same "SURFACE" across all medium, in addition it's specific to the "CONFIGURATION" too.
* "Wide Printable No-ID" seems to be the only "SURFACE" which is common across nearly all medium. Second to that would be "Matt Silver" of which CD-R and BD-R doesn't come in.
* Their professional line is all "No-ID" annoyingly. They intend you to print them however.
* The "CRYSTAL" surface is specific to CD-R AZO only.
* Be warned that "PART NO" 43791 under the "Datalife" branding is "Non-AZO". "PART NO" 97693 under the "Life Series" is also "Non-AZO".
* \> Archival Life: Extra Protection - up to 40 years; AZO - up to 100 years.

Below are some "consistent" "SURFACE" configurations you could get. Otherwise it's a "mix" of surfaces for decent prices:

| Type          | Configuration | Surface              | Product name  | Product Id |
|---------------|---------------|----------------------|---------------|------------|
| Consistent    | 50pk Spindle  | Wide Printable No-ID | CD-R AZO      | 43438      |
| Consistent    | 50pk Spindle  | Wide Printable No-ID | DVD-R AZO     | 43533      |
| Consistent    | 50pk Spindle  | Wide Printable No-ID | BD-R Datalife | 43812      |
| Consistent    | 25pk Spindle  | Wide Printable ID    | CD-R AZO      | 43439      |
| Consistent    | 25pk Spindle  | Wide Printable ID    | DVD-R AZO     | 43538      |
| Mixed         | 50pk Spindle  | Crystal              | CD-R AZO      | 43343      |
| Mixed         | 50pk Spindle  | Matt Silver          | DVD-R AZO     | 43548      |
| Mixed         | 50pk Spindle  | Wide Printable No-ID | BD-R Datalife | 43812      |
| Supplementary | 25pk Spindle  | Matt Silver          | DVD+R DL AZO  | 43757      |
| Supplementary | 25pk Spindle  | Wide Printable No-ID | DVD+R DL AZO  | 43667      |
| Supplementary | 10pk Spindle  | White/Blue           | BD-R DL       | 43746      |

#### Technologies

* "MediDisk"
* "Datalife"
* "HARD COAT"
* "SUPER HARD COAT"
* "AZO"
* "SERL"
* "EXTRA PROTECTION"
* "M.A.B.L"
* "CRYSTAL"
* "M-DISC"

#### Surfaces

* "Mat Silver"
* "Inkjet Printable"
* "White/Blue"
* "Wide Printable No-ID"
* "Wide Print No-ID" (inconsistency)
* "Wide Printable ID"
* "5 Colours"
* "Colour" (possible inconsistency)
* "Crystal"
* "Extra Protection"
* "Silver"
* "Thermal No-ID"
* "DataLifePlus Wide Thermal No-ID"
* "DataLifePlus Wide Printable No-ID"
* "DataLifePlus Waterproof Printable No-ID"

#### Extra links

* [Verbatim® Life Series DVD Media](http://cdn.cnetcontent.com/59/45/594513c7-59ed-4fd7-979f-48e6a5091f22.pdf)
* [Optical for Professionals](https://www.bechtle.com/shop/medias/57e386bd9ce9693335a88171.pdf?context=bWFzdGVyfHJvb3R8MTM2NjU4OHxhcHBsaWNhdGlvbi9wZGZ8aDk1L2g4OS8xMTM1MTQ0OTk2MDQ3OC5wZGZ8Mjc4MDhmNTcwMDlhYjAyMThhMzA2ZDk0YjkyNTdjOWZjNTI1MGRiMDJjNDY1NjA5MzkzZTJiMmYzNmNiOGI4Yw)
* [CD-R AZO vs CD-R Extra Protection](https://forum.imgburn.com/index.php?/topic/23204-quality-cd-blanks-in-this-day-and-age/&do=findComment&comment=154294)
* [Is there a significant difference in recording quality between these and the regular Verbatim CD-R. Where does the difference come in??](https://www.amazon.com/ask/questions/Tx48WCG7HZSC6C/)

### Philips

Can't seem to find a data sheet or product brochure unfortunately. So no way to quantify what a "very long archive lifespan" is.

* [DVD+R DL x 25 DR8I8B25F](https://www.amazon.co.uk/Philips-DR8I8B25F-DVD-DL-25-8-5/dp/B0010S8WTW) although several bad reviews.
* [DVD-R x 100 DM4S6B00F/00](https://www.amazon.co.uk/gp/product/B000E0LLCM/)

## File systems

* K3B `Linux/Unix only` seems to read back just fine on Windows 10. It looks to be ISO 9660 Level 3 + Rock Ridge extensions.
* K3B `Linux/Unix + Windows` is ISO 9660 Level 3 + Rock Ridge extensions + Joliet extensions. 
* ~~ISO 1999?~~
* [ISO 9660](https://wiki.osdev.org/ISO_9660) file system
* [Joliet](https://wiki.osdev.org/Joliet) extension to ISO 9660.
* [Rock Ridge](https://wiki.osdev.org/ISO_9660#Rock_Ridge_and_Joliet) extension to ISO 9660.
* [UDF](https://wiki.osdev.org/UDF) file system
* [UDF with K3B](https://dirkmittler.homeip.net/blog/archives/4120)
* ImgBurn UDF 1.02
* ImgBurn ISO 9660 + Joliet: MP3 data disks
* ImbBurn ISO 9660 + Joliet: DivX video disc
* [ImgBurn ISO 9660 + UDF - Bridge disc](https://forum.imgburn.com/index.php?/topic/11599-difference-between-udf-102-and-iso-9660udf-102/) TODO: Same as UDF structures on K3B?
* [ISO 9660 + Joliet + UDF - vs doing each separate](https://forum.imgburn.com/index.php?/topic/17600-iso9660jolietudf-vs-separate-of-them/)
* Brasero uses ISO 9660 Level 2? But will switch to Level 3 when you add files over 2 GB and you accept the warning message.

## Scripts

### ffmpeg

* [How can I use ffmpeg to split MPEG video into 10 minute chunks?](https://unix.stackexchange.com/a/212518/220886)
* Below is a adaptation I used for DVD splits of long 6 hour MP4 video files. Some work is needed still since this causes issues on the time stamps.
* One downside with this is using this for data disc project wont make use of the disks full capacity as the disc can fit more MP4 runtime than 120 mins in DVD VIDEO_TS mode.
* 1: `ffmpeg -i "$1" -c copy -map 0 -segment_time 01:18:00 -f segment %03d"$1"`
* That creates a file around: 3.6 GB (3604783722 bytes) in size although it will depend how compressed your source file is. 
* 2: `ffmpeg -i "$1" -c:v copy -c:a copy -segment_time 01:18:00 -f segment %03d"$1"`
	* `ffmpeg -i 001out.mp4 -c:v copy -c:a copy 001out-reprocessed.mp4` this will fix the timestamps must be some flag im missing on original line.

### Tar compress, split and GPG

* `split -b 500MB backup.tar.gz backup.tar.gz.`
* Uses base 2 (`G`, `M`) or base 10 (`GB`, `MB`) size suffixes.
* TODO: Include our scripts mentioned here: [TODO: Some kinda what tool for what job consolidation](#todo-some-kinda-what-tool-for-what-job-consolidation)

### rsync

* `rsync -a --progress`
* `rsync -a --delete --progress`
* `rsync -a --delete --progress --dry-run`

### tar tape length

* `tar -M -L 500M -cvf backup.tar.1 *`
* Uses base 2 size suffixes (`G`, `M`): `SIZE x 1024` use the bytes suffix (`c`) for anything else.
* [gnu.org/software/tar/manual/html_section/tar_78.html](https://www.gnu.org/software/tar/manual/html_section/tar_78.html)
	* \> Sure enough, to extract a split member you would need all volumes its parts reside on.
* tar has interactive mode so you can swap tapes or other medium and specify it's path when it reaches the end of tape 1.
* Useful for large files which would span more than one medium on their own.

### 7-Zip

1. Add to archive
2. Compression level use `Store` for a `tar` like substitute otherwise use default for compressible files.
3. Split into blocks of, you can choose DVD or select one from the list then manually change the size.

### dirsplit

* [linux.die.net/man/1/dirsplit](https://linux.die.net/man/1/dirsplit)
* Brasero seems to work with soft or hard links.
* ImgBurn only works with hard links from what I tested over SMB.
* To see all the flags you want this command: `$ dirsplit --longhelp`
* Uses base 2 (`G`, `M`) or base 10 (`g`, `m`) size suffixes.

#### Example of a file too large to fit onto one medium

```
$ dirsplit -s 200m -L -S camera/
Building file list, please wait...
Too large object(s) (15574072797) for the given max size: /media/peter/camera/large-file-1.mp4 (maybe coalesced in arrays, check manually)
```

#### mkisofs catalog example

```
$ dirsplit -s 24g -S camera/

$ ls
camera  vol_1.list  vol_2.list
```

#### Soft link example

```
$ dirsplit -s 24g -l -S camera/`

$ ls
camera  vol_1  vol_2
```

#### Hard link example

```
$ dirsplit -s 24g -L -S camera/`

$ ls
camera  vol_1  vol_2
```

### File size suffix and file managers

* My file manager uses base 10.
* `du` uses base 2 unless specifying the `--si` flag.
* [pubs.opengroup.org/onlinepubs/009604499/utilities/df.html#tag_04_36_10](https://pubs.opengroup.org/onlinepubs/009604499/utilities/df.html#tag_04_36_10)
	* \> In the following list, all quantities expressed in 512-byte units (1024-byte when -k is specified) shall be rounded up to the next higher unit.
* So `df` and presumably `du` round up. Must be to prevent confusion of not being able to fit a file onto a medium of fixed size if it were rounded using the common method in maths or science.
* [Random ASCII - Base Ten For (Almost) Everything](https://randomascii.wordpress.com/2016/02/13/base-ten-for-almost-everything/)
	* Windows Explorer uses base 2. 

#### Base 2 file

`tar -M -L 6G -cvf 1-tar *`

My file manager reports it as: `6.4 GB (6442455040 bytes)`

```
$ du  *
6291464	1-tar
```

```
$ du -hs *
6.1G	1-tar
```

```
$ du --si *
6.5G	1-tar
```

```
$ bc -l
6442455040/1024
6291460.00000000000000000000
6291460/1024
6144.00390625000000000000
6144/1024
6.00000000000000000000
```

#### Base 10 file

`split -b 500MB backup.tar.gz backup.tar.gz.`

My file manager reports it as: `500.0 MB (500000000 bytes)`

```
$ du *
488284	backup.tar.gz.aa
```

```
$ du -hs *
477M	backup.tar.gz.aa
```

```
$ du --si *
501M	backup.tar.gz.aa
```

```
$ bc -l
500000000/100
5000000.00000000000000000000
5000000/100
50000.00000000000000000000
50000/100
500.00000000000000000000
```

### Other

* [Parchive](https://en.wikipedia.org/wiki/Parchive)
* [Using the Linux Command, dirsplit, to Dynamically Backup a Directory Over Multiple DVDs](http://www.phphosts.org/blog/2012/02/one-liner-using-the-linux-command-dirsplit-to-dynamically-backup-a-directory-over-multiple-dvds/)
* [How should I divide my data for backup media?](https://www.reddit.com/r/linuxadmin/comments/4m8jk6/how_should_i_divide_my_data_for_backup_media/d3uh68o?utm_source=share&utm_medium=web2x&context=3)
* [difference between mkisofs and genisoimage](https://askubuntu.com/questions/557910/difference-between-mkisofs-and-genisoimage)
* [mass-to-discs](https://pastebin.com/5asZ4tzp?fbclid=IwAR2B-nPP10VpquXRLSpcvr0GpfRT5PENc4TuRVILmLJBvK9NAy9pIEwqzP8) by 2E0EOL, it's similar to `dirsplit`.
* [chksum-all-recursive.sh](https://pastebin.com/S3tBzFbK?fbclid=IwAR13VcR2n8jLe9B3QTSka_OvfXhOFrOdrN4R1sFTbYo9LUE4wk7obQXhvhY) by 2E0EOL, parallel checksum using multiple algorithms.
* `eject` and `eject -t` are useful for controlling the CD tray opening and closing via CLI.
* `less /proc/sys/dev/cdrom/info` list cdrom info under Linux.
* `dmesg | grep "CD-ROM"` list model of cdrom drive under Linux.
* `md5sum * > md5.txt` create a md5 checksum of all files in the current directory.

### TODO: Some kinda what tool for what job consolidation

* `backup-dirsplit-public.sh` for music and videos public domain non sensitive? Data disc. No compression needed here and no encryption as containers are already compressed.
* `backup-tar-split-gpg-mv-bd-private.sh` for pretty much anything personal.
* `backup-tar-split-mv-bd-public.sh` for pretty much anything public domain large data set like public code backups.
* ~~Tar length for HDD backups or something. No compression needed here and no encryption as it can be done at entire disk level.~~

### What to include on a disc backup label

* Volume name
* Date of burning
* Disc number of how many in the backup set
* Any notes of special file exclusions or inclusions.
* _Encrypted or unencrypted?_
* _Owner_
* _Method of backup e.g. tar, split or is this obvious from file extensions?_
* _File system type e.g. ISO 9660_
* _Write software_
* _Alphabetic range if storing music dumps e.g. A-E_
* _Unique int id number which can then be stored in a central index e.g. spreadsheet or CSV along with other metadata._

### Disc vs Disk

* Disc, discus, round, optical disc, compact disc
* Disk, diskette, floppy disk, hard disk drive

## Related playback video links

* [PS1 original as a audio CD player in 2020](https://www.youtube.com/watch?v=JIKKKWyUSqs)
* [PS1 slim as a audio CD player in 2020](https://www.youtube.com/watch?v=ytwOKjF9yDI)
* [PS2 original as a audio CD player in 2020](https://www.youtube.com/watch?v=KgzL2z1fAik)
* [Nintendo Wii as a m4a music player in 2020](https://www.youtube.com/watch?v=q3RHqpq-tIo)

## Related blog posts

* [NetworkProfile - Archiving pictures to 1000 year M-DISC](https://blog.networkprofile.org/archiving-pictures-to-m/)
* [NetworkProfile - My Backup Plan & Lessons learned](https://blog.networkprofile.org/my-backup-plan-lessons-learned/)
* [NetworkProfile - Archiving data to LTO Tape for long term storage and backups](https://blog.networkprofile.org/archiving-data-to-lto-tape-for-long-term-storage-and-backups/)

## Colour rings

I couldn't find much on Blu-ray/Blueray colour/color rings/bands burn line colour.

[forum.imgburn.com/index.php?/profile/33189-e5frog/&do=content&all_activity=1&page=2](https://forum.imgburn.com/index.php?/profile/33189-e5frog/&do=content&all_activity=1&page=2)

> e5frog replied to e5frog's topic in ImgBurn Support
> 
> But the log says OPC: No, and isn't it supposed to do that when writing the lead-in, once... Tried 8x, fail after 78%, same kind of bands in the written area. The successful write before it also has some of these bands of different color (it's very hard to get a good picture of it). Trying another 8x write with OPC on...

> e5frog replied to e5frog's topic in ImgBurn Support
>
> Sorry, I didn't save the log and I went through five discs that burned up to 90% before write error occured, all had increasingly lighter "bands" where they were written to. Sixth disc worked right away, no strange patterns on the disc, same looking surface on all the burnt area. So, I guess it's the discs, but thanks for the feedback, I'll see to it that I upgrade again then.

### CAV vs CLV

[DVD Burning and Media Quality Concepts](http://www.digitalfaq.com/guides/media/dvd-media-concepts.htm)

> With the advent of 8x media, Z-CLV (zonal constant linear velocity) and P-CAV (partial constant angular velocity) were introduced. Z-CLV starts at a speed like 4x, then at a certain point in the media, jumps to 6x, then 8x, etc., until it reaches the maximum speed. Sometimes a 16x Z-CLV burn will only burn a few hundred MB at 16x, which is why “x” speeds mean almost nothing anymore. P-CAV is similar, but does not jump speed. It increases velocity from 4x to 4.5x to 5x, etc., until it reaches it’s top speed. Much like Z-CLV, it may not hit max speed until the last 30 seconds worth of burning. This is why a 12x burn is almost an identical wait time to the speed of a 16x burn.

> This CLV burn is the same color from beginning to end.

> Z-CLV burning methods leave a mark on the dye, as the burn speed alters color slightly. These zones are perfect circles with a hard edge lines. Not to be confused with imperfect-shaped dye rings caused from inferior media. P-CAV may gradually change colors.

[Moisés Cardona - VERBATIM 4X BDXL 100GB BLANK MEDIA](https://moisescardona.me/verbatim-4x-bdxl-100gb-blank-media/)

> Here, we can see the written disc with its Z-CLV zones:

![](https://moisescardona.me/wp-content/uploads/2020/04/Verbatim-4x-BDXL-media-4-Copy-1024x768.jpg)

My BDR-212M burner seems to be CAV as per: [pioneer.jp/device_e/product-e/ibs/device_e/pdf/BDR-212M_Medialist.pdf](https://pioneer.jp/device_e/product-e/ibs/device_e/pdf/BDR-212M_Medialist.pdf)

This Polish website goes above and beyond with testing it: 

* [cdrinfo.pl - BDR212 - Introduction](https://www.cdrinfo.pl/artykuly/artykulPioneerBDR212S12X12English/index.php)
* [cdrinfo.pl - BDR212 - Record Blu-Ray](https://www.cdrinfo.pl/artykuly/artykulPioneerBDR212S12X12English/strona15.php)

Interestingly my SU-208GB drive supports DVD DL and this website shows it using a different tracking for DVD DL [mydvddrives.com/tsstcorp-cddvdw-su-208gb-su208gb](https://www.mydvddrives.com/tsstcorp-cddvdw-su-208gb-su208gb-sata-cd-dvd-rw-drive-burner-105313.html)

> * DVD+R 8x ( CAV )
> * DVD+R DL 6x ( ZCLV )

* [DVD-Writer-vs-Recorder-CAV-vs-CLV](https://forum.videohelp.com/threads/214141-DVD-Writer-vs-Recorder-CAV-vs-CLV)
* [Which purpose has P-CAV and Z-CLV?](https://forum.videohelp.com/threads/385866-Disc-writing-speeds-Which-purpose-has-P-CAV-and-Z-CLV)
* [wikipedia.org/wiki/Constant_angular_velocity](https://en.wikipedia.org/wiki/Constant_angular_velocity)
* [wikipedia.org/wiki/Constant_linear_velocity](https://en.wikipedia.org/wiki/Constant_linear_velocity)
* [wikipedia.org/wiki/Zone_bit_recording](https://en.wikipedia.org/wiki/Zone_bit_recording)

Below are some tests I did with Verbatim BD-R Datalife 43812 to try reproduce colour some rings by toggling local image cache before write to hopefully change buffer times.

### Create image test

Before write

![2021-01-08-create-image-test-before-write.png](/blog/assets/2021-02-20/2021-01-08-create-image-test-before-write.png)

Cover

![2021-01-08-create-image-test-cover.png](/blog/assets/2021-02-20/2021-01-08-create-image-test-cover.png)

After write with local image cache

![2021-01-08-create-image-test-k3b-write.png](/blog/assets/2021-02-20/2021-01-08-create-image-test-k3b-write.png)

### Identical test

Before write

![2021-01-08-identical-test-before-write.png](/blog/assets/2021-02-20/2021-01-08-identical-test-before-write.png)

Cover

![2021-01-08-identical-test-cover.png](/blog/assets/2021-02-20/2021-01-08-identical-test-cover.png)

After write with no local image cache image 1

![2021-01-08-identical-test-k3b-write-1.png](/blog/assets/2021-02-20/2021-01-08-identical-test-k3b-write-1.png)

After write with no local image cache image 2

![2021-01-08-identical-test-k3b-write-2.png](/blog/assets/2021-02-20/2021-01-08-identical-test-k3b-write-2.png)

### Side by side

![2021-01-08-side-by-side.png](/blog/assets/2021-02-20/2021-01-08-side-by-side.png)

![2021-01-08-ring-vs-no-ring.png](/blog/assets/2021-02-20/2021-01-08-ring-vs-no-ring.png)

## Imperfections

I noticed a few imperfections so I now check the blank medium before using it.

![imperfection-1.png](/blog/assets/2021-02-20/imperfection-1.png)

![imperfection-2.png](/blog/assets/2021-02-20/imperfection-2.png)

![imperfection-3.png](/blog/assets/2021-02-20/imperfection-3.png)

![imperfection-4.png](/blog/assets/2021-02-20/imperfection-4.png)

![imperfection-5.png](/blog/assets/2021-02-20/imperfection-5.png)

## Further compatibility testing

### Data set 1 write

| Id | Date       | Disc brand | Disc type | Write software | File source | IO type                              | Project type | File system         | File type                                                                       | Write device | Write speed max | Write speed average | Result |
|----|------------|------------|-----------|----------------|-------------|--------------------------------------|--------------|---------------------|---------------------------------------------------------------------------------|--------------|-----------------|---------------------|--------|
| 18 | 2021-02-27 | Kodak      | DVD+R DL  | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | UDF 1.02            | MP4                                                                             | BDR-212M     | 10.5x           | 8x                  | OK     |
| 19 |            | Philips    | DVD-R     | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | UDF 1.02            | MP4                                                                             | BDR-212M     |                 |                     | OK     |
| 20 |            | Philips    | DVD-R     | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | ISO 9660+UDF        | MP4                                                                             | BDR-212M     |                 |                     | OK     |
| 21 |            | Philips    | DVD-R     | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | ISO 9660+Joliet+UDF | MP4                                                                             | BDR-212M     |                 |                     | OK     |
| 22 | 2021-02-27 | Kodak      | DVD+R DL  | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | ISO 9660+Joliet     | MP3                                                                             | BDR-212M     |                 |                     | OK     |
| 23 |            | Philips    | DVD-R     | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | ISO 9660+Joliet+UDF | FLAC, TXT, WAV, M4A, AIFF, JPG, PNG, GIF, MKV, MP4, AVI, MP3, WEBM              | BDR-212M     |                 |                     | OK     |
| 24 |            | Philips    | DVD-R     | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | ISO 9660+Joliet     | FLAC, TXT, WAV, M4A, AIFF, JPG, PNG, GIF, MKV, MP4, AVI, MP3, WEBM              | BDR-212M     |                 |                     | OK     |
| 25 |            | Philips    | DVD-R     | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | ISO 9660+UDF 1.02   | FLAC, TXT, WAV, M4A, AIFF, JPG, PNG, GIF, MKV, MP4, AVI, MP3, WEBM              | BDR-212M     |                 |                     | OK     |
| 26 |            | Philips    | DVD-R     | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | UDF 1.02            | FLAC, TXT, WAV, M4A, AIFF, JPG, PNG, GIF, MKV, MP4, AVI, MP3, WEBM              | BDR-212M     |                 |                     | OK     |
| 27 |            | Kodak      | DVD+R DL  | Brasero        | Remote SMB  | Linux direct                         | Data         | ISO 9660            | JPG, MP3, 3PG, OGG, PNG, FLV, WAV, M4A, FLAC, MOV, MP4, MKV, AVI, AAC, UMX, MD5 | SU-208GB     |                 |                     | OK     |

* "Data set 1" is a continuation of the data set mentioned in [Write speed tests](#write-speed-tests) and [Compatibility/playback tests](#compatibilityplayback-tests)
* 23, 24, 26, 27 are the most useful test disks.
* TODO: Need a BB-R and BD-R DL multi file test disk.
* 25 was a accidental burn because I wasn't planning on testing that FS combo. However I thought id test it anyway.

### Data set 1 read

| WriteId | Read device | Result | Readable file types |
|---------|-------------|--------|---------------------|
| 18      | PS3         | OK     | MP4                 |
| 18      | LG DVX392H  | Err    |                     |
| 19      | LG DVX392H  | Err    |                     |
| 20      | LG DVX392H  | Err    |                     |
| 21      | LG DVX392H  | Err    |                     |
| 22      | LG DVX392H  | OK     | MP3                 |
| 23      | LG DVX392H  | OK     | MP3, JPG, AVI       |
| 24      | LG DVX392H  | OK     | MP3, JPG, AVI       |
| 25      | LG DVX392H  | OK     | MP3, JPG, AVI       |
| 26      | LG DVX392H  | OK     | MP3, JPG, AVI       |
| 27      | LG DVX392H  | OK     | MP3, JPG, AVI       |

### Data set 2 write

| Id | Date       | Disc brand | Disc type | Write software | File source | IO type                              | Project type | File system     | File type | Write device | Write speed max | Write speed average | Result |
|----|------------|------------|-----------|----------------|-------------|--------------------------------------|--------------|-----------------|-----------|--------------|-----------------|---------------------|--------|
| 65 | 2021-03-03 | MediaRange | BD-R DL   | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | ISO 9660+Joliet | binary    | BDR-212M     | 8x              | 6.6x                | OK     |

### Data set 2 read

N/A

### Data set 3 write

| Id | Date       | Disc brand | Disc type | Write software | File source | IO type                              | Project type | File system     | File type | Write device | Write speed max | Write speed average | Result |
|----|------------|------------|-----------|----------------|-------------|--------------------------------------|--------------|-----------------|-----------|--------------|-----------------|---------------------|--------|
| 1  | 2021-02-24 | Verbatim   | BD-R      | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | ISO 9660+Joliet | MP3       | BDR-212M     |                 |                     | OK     |
| 2  | 2021-02-24 | Verbatim   | BD-R      | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | UDF 1.02        | MP4       | BDR-212M     |                 |                     | OK     |
| 3  | 2021-02-24 | Philips    | DVD-R     | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | UDF 1.02        | MKV       | BDR-212M     |                 |                     | OK     |
| 5  | 2021-02-24 | Philips    | DVD-R     | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | ISO 9660+Joliet | MP3       | BDR-212M     |                 |                     | OK     |
| 6  | 2021-02-24 | Philips    | DVD-R     | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | ISO 9660+Joliet | MP3       | BDR-212M     |                 |                     | OK     |
| 7  | 2021-02-24 | Philips    | DVD-R     | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | ISO 9660+Joliet | MP3       | BDR-212M     |                 |                     | OK     |
| 8  | 2021-03-08 | Kodak      | DVD+R DL  | ImgBurn        | Remote SMB  | Windows -> VM passthrough -> Windows | Data         | ISO 9660+Joliet | MP3       | BDR-212M     |                 |                     | OK     |

### Data set 3 read

| WriteId | Read device    | Result | Readable file types | Notes                                                                                           |
|---------|----------------|--------|---------------------|-------------------------------------------------------------------------------------------------|
| 1       | Sony BDP-S1500 | OK     | MP3                 |                                                                                                 |
| 2       | Sony BDP-S1500 | OK     | MP4                 | One MP4 file didn't work: "The file is corrupt or unsupported.". The rest were working perfect. |
| 3       | Sony BDP-S1500 | OK     | MKV                 | Videos with no audio present: "Audio format not supported."                                     |
| 5       | Sony BDP-S1500 | OK     | MP3                 |                                                                                                 |
| 6       | Sony BDP-S1500 | OK     | MP3                 |                                                                                                 |
| 7       | Sony BDP-S1500 | OK     | MP3                 |                                                                                                 |
| 8       | Sony BDP-S1500 | OK     | MP3                 |                                                                                                 |

### Sony BDP-S1500

#### Initial notes

* [Sony - Supported File Formats for DLNA® and USB Compatible Blu-ray Disc™ Players and Network Media Players](https://www.sony.com/electronics/support/articles/S1F0959)
	* They don't confirm what formats work on disc however.
	* In my testing it seems to support the same as USB compatible formats.
* Data set 3 Id:2 seems to be due to file size or runtime however it's only 17G and a 11G file worked.
* Data set 3 Id:3 seems to be due to MKV opus 5.1 instead of using MP3, MP2 (MPEG), AC3 or AAC audio. 
	* AC3 apparently works.
		* My friend 2E0EOL says tsMuxer warned him to use AC3.
	* MP4 seems to mostly use AAC audio and avc1 video in my sample set.
	* MKV is mostly opus audio in my sample set.

With regards to finding a format for a friends player:

* MP3 only I would fire out: `ISO 9660+Joilet`
* Anything else like MP4, MKV I would fire out `UDF`.
* If you're really worried about compatibility then just use: `ISO 9660+Joliet+UDF` if that doesn't work its probably not worth debugging further as you will expend 8 discs for each combination of file type and file system type if you wanted a unique combination test.
* It seems most systems these days are not worried about file system type but are more worried about codec.

#### Things to watch out for

* File system
	* ISO 9660
		* Version 3 for certain size files (Brasero)? macOS doesn't work with version 3?
	* UDF
		* File size limit big.
* File type
	* Container
		* .mp4
		* .mkv
	* Codec video
		* h.264
	* Codec audio
		* AAC
		* AC3
		* Opus		
	* Video frame rate
		* 25 fps
		* 30 fps
		* 50 fps
		* 60 fps
		* variable
	* Video colour space
		* PAL, NTSC
		* [Color Hell: Ffmpeg Transcoding and Preserving BT.601](https://kdenlive.org/en/project/color-hell-ffmpeg-transcoding-and-preserving-bt-601/)
		* `yuvj420p(pc, bt470bg/bt470bg/smpte170m)`
		* `yuv420p(tv, bt709)`
		* `yuvj420p(pc)`
		* `yuv420p`
		* `yuvj420p(pc, progressive)`
		* `yuv420p(tv, bt709, progressive),`
	* Video aspect ratio
		* `DAR 16:9`
		* `DAR 9:16`
			* Doesn't work. Use transpose: [Rotating videos with FFmpeg](https://stackoverflow.com/questions/3937387/rotating-videos-with-ffmpeg)
	* Audio/video bit rates
	* Audio channels
		* 5.1 surround
		* Stereo
		* Mono
* ffmpeg
	* Re-encode for correcting timestamps on TS streams.
		* See heading: [ffmpeg](#ffmpeg)
	* `-report` flag
	* `-map` choose the right stream index?
	* `-c` specify codec for streams or specific with `a` then `copy` it or use `aac` etc
	* TODO: List all the other transforms and useful no video flags.
* ffprobe

#### Script for mass checking codecs

ffprobe and ffmpeg you have to redirect the stdout in order to pipe it.

```sh
echo "MP4"
for i in *.mp4; do 
	echo "$i"
	ffprobe -i "$i" 2>&1 | grep Video --color
	ffprobe -i "$i" 2>&1 | grep Audio --color
done

echo "MKV"
for i in *.mkv; do
	echo "$i"
	ffprobe -i "$i" 2>&1 | grep Video --color
	ffprobe -i "$i" 2>&1 | grep Audio --color
done
```

#### Extended testing notes

* Vertical vs horizontal trays... Go horizontal... Xbox, writing etc
* It seems to expose the USB the same as a BD or DVD. 
	* This allowed me to move to testing via USB instead of disc for these edge case issues I wanted to narrow down to the specific cause. 
	* Confirmed results on DVD on refined test data set.
	* I could use rewritable media as 2E0EOL points out.
	* It's still important to retain a directory listing of the files so you can refer back to them if further analysis is required. Not a problem if you burn everything to a set of test disks like I mostly did.
* moto-g test was crap due to vertical video 9:16 issue. Re test it I guess using the GC video.
* Colour space doesn't appear to be an issue.	
* [2E0EOL convert script](https://pastebin.com/MWWm0PE4) so his script seems mostly fine provided original video doesn't have a bad aspect ratio.
* FPS didn't seem to be a issue either.
* AC3 playback is louder than AAC.
* The "Return" button works on audio and pictures but only the "Stop" button will work on videos.
* The following codecs all work on both MP4 and MKV with 5.1 _and presumably stereo_: MP3, MP2 (MPEG), AC3, AAC
* MP3 playback is louder than MP2 (MPEG)
* OBS files worked as I dont have remux to MP4 turned on. So they're AAC MKVs.
* Use pen only, indellible markers. No labels due to weight imbalance.
* Bit rate doesn't seem to be a issue nor 48000 Hz vs 44100 Hz.
* ogg != opus but you can use libvorbis and libopus. Both can be stored in `.ogg` but `.opus` is recommended file extension for the latter.
* `opus in MP4 support is experimental, add '-strict -2' if you want to use it.` so no Opus MP4 tested.
* Vorbis (libvorbis) 2 channel stereo works on MKV but not MP4 where it gives silent video. 
	* 5.1 doesn't work on either video.
	* It works audio only too however 5.1 doesn't but stereo does work.
* Opus (libopus) 2 channel stereo works on MKV.
	* 5.1 doesn't work but 2 channel stereo does.
	* It wont play audio only in `.ogg` or `.opus`
* MKV isn't subject to the below MP4 file size/runtime issues.

#### MP4 file size or runtime issue

Generated using the following by swapping the `-t` flag value.

```
ffmpeg -i "gatecrasher on rapture tv nec 2001.mp4" -c:v copy -c:a copy -t 22400 gatecrasher-rapture-22400.mp4 -report
```

* The following path is a local HDD for scratch space: `/media/peter/bulk-drive/temp-burning-2`
* The following path is a USB I was using for testing on the player: `/media/peter/testing`
* I did `md5sum` on the USB to ensure it matched the source file after playback test to rule out copy corruption or playback corruption.
	* I did have one test get thrown out as a corrupt file however `md5sum` confirmed it was actually corrupted upon copy not related to the test case.
* \> Units are K,M,G,T,P,E,Z,Y (powers of 1024) or KB,MB,... (powers of 1000).
* Could it be a file name length or spaces issue... Nope tried that, it's not file name.

##### Not working

```
peter@desktop:/media/peter/bulk-drive/temp-burning-2$ md5sum gatecrasher-rapture-22800.mp4
1b5f8ade378963ced05e1f1ff9cbade9  gatecrasher-rapture-22800.mp4

peter@desktop:/media/peter/testing$ md5sum gatecrasher-rapture-22800.mp4 
1b5f8ade378963ced05e1f1ff9cbade9  gatecrasher-rapture-22800.mp4

peter@desktop:/media/peter/bulk-drive/temp-burning-2$ du -B KB gatecrasher-rapture-22800.mp4
16722961kB	gatecrasher-rapture-22800.mp4
peter@desktop:/media/peter/bulk-drive/temp-burning-2$ du -B K gatecrasher-rapture-22800.mp4
16331016K	gatecrasher-rapture-22800.mp4

peter@desktop:/media/peter/bulk-drive/temp-burning-2$ ffprobe -i gatecrasher-rapture-22800.mp4
Duration: 06:20:00.02, start: 0.000000, bitrate: 5867 kb/s
```

The below was tested twice.

```
peter@desktop:/media/peter/bulk-drive/temp-burning-2$ md5sum gatecrasher-rapture-22600.mp4 
90df26b7186c7da02dee5da88217a934  gatecrasher-rapture-22600.mp4

peter@desktop:/media/peter/testing$ md5sum gatecrasher-rapture-22600.mp4 
90df26b7186c7da02dee5da88217a934  gatecrasher-rapture-22600.mp4

peter@desktop:/media/peter/bulk-drive/temp-burning-2$ du -B KB gatecrasher-rapture-22600.mp4
16567682kB	gatecrasher-rapture-22600.mp4
peter@desktop:/media/peter/bulk-drive/temp-burning-2$ du -B K gatecrasher-rapture-22600.mp4
16179376K	gatecrasher-rapture-22600.mp4

peter@desktop:/media/peter/bulk-drive/temp-burning-2$ ffprobe gatecrasher-rapture-22600.mp4
Duration: 06:16:40.02, start: 0.000000, bitrate: 5864 kb/s
```

##### Working

```
peter@desktop:/media/peter/bulk-drive/temp-burning-2$ md5sum gatecrasher-rapture-22400.mp4 
7ae86b0b637c94740147c3ca2e1d8ce4  gatecrasher-rapture-22400.mp4

peter@desktop:/media/peter/testing$ md5sum gatecrasher-rapture-22400.mp4 
7ae86b0b637c94740147c3ca2e1d8ce4  gatecrasher-rapture-22400.mp4

peter@desktop:/media/peter/bulk-drive/temp-burning-2$ du -B KB gatecrasher-rapture-22400.mp4 
16420013kB	gatecrasher-rapture-22400.mp4
peter@desktop:/media/peter/bulk-drive/temp-burning-2$ du -B K gatecrasher-rapture-22400.mp4 
16035168K	gatecrasher-rapture-22400.mp4

peter@desktop:/media/peter/bulk-drive/temp-burning-2$ ffprobe gatecrasher-rapture-22400.mp4
Duration: 06:13:20.02, start: 0.000000, bitrate: 5864 kb/s
```

The below was tested twice.

```
peter@desktop:/media/peter/bulk-drive/temp-burning-2$ md5sum gatecrasher-rapture-22200.mp4
b7503a44e34798f8a518e7da43a241b3  gatecrasher-rapture-22200.mp4

peter@desktop:/media/peter/testing$ md5sum gatecrasher-rapture-22200.mp4 
b7503a44e34798f8a518e7da43a241b3  gatecrasher-rapture-22200.mp4

peter@desktop:/media/peter/bulk-drive/temp-burning-2$ du -B KB gatecrasher-rapture-22200.mp4
16267891kB	gatecrasher-rapture-22200.mp4
peter@desktop:/media/peter/bulk-drive/temp-burning-2$ du -B K gatecrasher-rapture-22200.mp4
15886612K	gatecrasher-rapture-22200.mp4

peter@desktop:/media/peter/bulk-drive/temp-burning-2$ ffprobe -i gatecrasher-rapture-22200.mp4
Duration: 06:10:00.02, start: 0.000000, bitrate: 5862 kb/s
```
