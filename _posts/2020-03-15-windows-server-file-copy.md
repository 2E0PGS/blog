---
layout: post
title: Windows server file copy
date: 2020-03-15 19:57:10
author: Peter Stevenson
summary: Moving lots of files between two Windows servers
categories: sysadmin
thumbnail:
tags:
 - Windows
 - RDP
 - Robocopy
---

# Moving lots of files between two Windows servers

Lots of small files 100k+

I have done this many times now. This is the best method besides using SFTP/SSH.

## Destination server

RPD into the destination server. Use it's RDP client to connect to the source server but make sure to specify device attachment of local HDD. You can find this setting under the "Local Resources" tab of the RDP connection window.

This is more reliable than a drag and drop or copy-paste from local to RDP window. You often get a unspecific error crop up with files larger than a GB.

## Now on the source RDP

Don't use windows right click paste file copy between local disk and remote network mounted disk in "My computer". This has some odd memory hogging behaviour also it doesn't preserve the "Date modified".

Robocopy allows us to update those files incrementally. Useful if you are migrating bulk of the historical data now. Then when you are ready to migrate server before final move you can refresh the new server with the latest files.

Use: `robocopy "D:\myfiles" "\\tsclient\D\myfiles" /MIR`

Warning it will remove files in the destination dir where they don't exist on the source.
