---
layout: post
title: ESXi minimum RAM workaround
date: 2018-05-12 17:07:19
author: Peter Stevenson
summary: Workaround 4 GB RAM limit on VMware ESXi
categories: sysadmin
thumbnail:
tags:
 - VMware
---

# ESXi minimum RAM workaround

_Moved here from [original gist](https://gist.github.com/2E0PGS/9e56e67d72c31e849147b9e56e3ae383)_

You have 4 GB RAM installed in a new server and ESXi says you have 3.88 GB so it prevents you from continuing the install. This is a workaround for that.

Based upon this article but I had to make a few changes to get the permissions to take: [https://community.spiceworks.com/topic/411970-installing-esxi-5-5-with-4gb-ram-memory_size-error?page=1](https://community.spiceworks.com/topic/411970-installing-esxi-5-5-with-4gb-ram-memory_size-error?page=1)

1. Bootup VMware ESXi from live medium.
2. Wait for the "Welcome to VMWARE ESXi X.X.X installation" screen.
3. Press Alt+F1 to drop into a console.
4. Login with user root and blank password (just hit enter when prompted for password).
5. Change directory: `cd /usr/lib/vmware/weasel/util/`
6. Set permissions to allow any user to modify the install script: `chmod 777 upgrade_precheck.py`
7. Copy file to edit as it didn't like me editing it live: `cp upgrade_precheck.py temp.py`
8. Edit the file: `vi temp.py`
9. Edt the line with "MEM_MIN_SIZE" you can use / to search in vi.
10. Press i to enter insert mode in vim.
11. Change `4 * 1024 - 32` to `2 * 1024 - 32`
12. Press Esc to exit insert mode on vim.
13. Type :wq to write the changes to disk and close vim.
14. Copy the file back over the original: `cp temp.py upgrade_precheck.py`
15. Locate the installer process ID: `ps -c | grep weasel`
16. Kill the process using its ID: `kill PROCESSIDHERE`