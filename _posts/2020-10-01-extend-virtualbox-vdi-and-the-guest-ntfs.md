---
layout: post
title: Extend VirtualBox VDI and the guest NTFS
date: 2020-10-01 21:04:10
author: Peter Stevenson
summary: Using Windows Disk Management to extend a NTFS partition
categories: sysadmin
thumbnail:
tags:
 - Windows
 - partitioning
 - VirtualBox
---

# Using Windows Disk Management to extend a NTFS partition

First, backup your data.

1. Shutdown the VirtualBox VM.
2. Open "File" then "Virtual Media Manager".
3. Select the VDI you want to expand.
![virtualbox-1](/blog/assets/2020-10-01/virtualbox-1.png)
4. Increase it's size.
![virtualbox-2](/blog/assets/2020-10-01/virtualbox-2.png)
5. Apply those changes and boot the VirtualBox VM back up.
6. Open Windows "Disk Management" by typing it into the start menu.
![disk-management-1](/blog/assets/2020-10-01/disk-management-1.png)
7. Right click on the "(C:)" partition and click "Extend Volume".
![disk-management-2](/blog/assets/2020-10-01/disk-management-2.png)
8. Use the arrows to max out the value for "Select the amount of space in MB:"
![disk-management-3](/blog/assets/2020-10-01/disk-management-3.png)
9. Hit next and finish.
![disk-management-4](/blog/assets/2020-10-01/disk-management-4.png)
10. You should now have a extended guest OS partition.
![disk-management-5](/blog/assets/2020-10-01/disk-management-5.png)

![my-computer.png](/blog/assets/2020-10-01/my-computer.png)