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
 - dotnet
 - core
 - REST
 - API
 - netstandard2.0
 - netcore2.0
---

# Creating a custom status page using the LibreNMS API

A while back in 2019‑12‑07 I created a area in my [dotnet core website](https://bitbucket.org/2E0PGS/core) to host a status page for LibreNMS using my [dotnet standard client library for LibreNMS](https://bitbucket.org/2E0PGS/librenms-client).

This allows me to have a public status page for my hosted services.

Devices

![devices.png](/blog/assets/2020-09-16/devices.png)

Services

![services.png](/blog/assets/2020-09-16/services.png)

Services in error state

![red-services.png](/blog/assets/2020-09-16/red-services.png)

See also: [My custom LibreNMS/Nagios plugins](https://bitbucket.org/2E0PGS/nagios-plugins)