---
layout: post
title: ESXi UPS shutdown script
date: 2019-01-13 13:29:34
author: Peter Stevenson
summary: How to shutdown ESXi safely when a UPS is on battery.
categories: programming
thumbnail:
tags:
 - UPS
 - ESXi
 - Linux
 - programming
 - shell
---

# ESXi UPS shutdown script

How to shutdown ESXi safely when a UPS is on battery.

## esxi-ups-shutdown.sh

Replace `"PASSWORD"` with the ESXi server password and replace `SERVERIP` with the ESXi servers IP.

```sh
#!/bin/bash

sshpass -p "PASSWORD" ssh -o StrictHostKeyChecking=no root@SERVERIP < esxi-ups-payload.sh
```

## esxi-ups-payload.sh

```sh
#!/bin/bash

echo "$(date) - UPS started ESXi shutdown" >> esxi-ups-log.txt
poweroff
```

The poweroff command is fine for ESXi version 4 and above: https://kb.vmware.com/s/article/1013193
