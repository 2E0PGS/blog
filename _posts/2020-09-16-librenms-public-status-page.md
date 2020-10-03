---
layout: post
title: LibreNMS public status page
date: 2020-09-16 22:57:10
author: Peter Stevenson
summary: Creating a custom status page using the LibreNMS API
categories: programming
thumbnail:
tags:
 - LibreNMS
 - C#
 - Core
 - REST
 - API
 - .NET
 - ASP.NET
 - MVC
---

# Creating a custom status page using the LibreNMS API

A while back in 2019‑12‑07 I created a area in my [ASP.NET Core website](https://bitbucket.org/2E0PGS/core) to host a status page for LibreNMS using my [.NET Standard client library for LibreNMS](https://bitbucket.org/2E0PGS/librenms-client).

This allows me to have a public status page for my hosted services.

Devices

![devices.png](/blog/assets/2020-09-16/devices.png)

Services

![services.png](/blog/assets/2020-09-16/services.png)

Services in error state

![red-services.jpg](/blog/assets/2020-09-16/red-services.jpg)

## See also

* [My custom LibreNMS/Nagios plugins repo](https://bitbucket.org/2E0PGS/nagios-plugins)
* [nagios-plugin-minecraft](https://github.com/2E0PGS/nagios-plugin-minecraft)