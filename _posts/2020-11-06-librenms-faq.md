---
layout: post
title: LibreNMS FAQ
date: 2020-11-06 17:23:19
author: Peter Stevenson
summary: Cheat sheet for LibreNMS
categories: sysadmin
thumbnail:
tags:
 - LibreNMS
---

# LibreNMS FAQ

## Graphs stop showing

I had this occur if memory overfills or so...

SSH in and run: `./poller.php -h localhost -d -f -m os`

## Move services in bulk to another host

Services don't get monitored if a device is down. So if you're already hosting those services elsewhere it wont get checked until you move them on LibreNMS. 

See my community suggestion: [Services that are not checked colour](https://community.librenms.org/t/services-that-are-not-checked-colour/11117)

SQL to assign them to another device id:

```sh
sudo su

mysql
```

```sql
use librenms

select * from services where device_id = 44;

update services set device_id = 41 where device_id = 44;
```

## You need to monitor HTTPS/HTTP and certificates separately

* For HTTP I suggest: `-t 30` 
* For HTTPS I suggest: `-S --sni -t 30` 
* For certificate I suggest: `-C 27 --sni -t 30`

From the man page: [man/check_http](https://www.monitoring-plugins.org/doc/man/check_http.html)

```
 -C, --certificate=INTEGER[,INTEGER]
    Minimum number of days a certificate has to be valid. Port defaults to 443
    (when this option is used the URL is not checked.)
```

## Slow or delayed alert notifications

* Check the config using the web UI config validator.
* Check the `php.ini` config timezone, the configs path maybe something like this: `/etc/php/7.0/fpm/php.ini`