---
layout: post
title: IIS reverse proxy server
date: 2020-06-04 17:02:19
author: Peter Stevenson
summary: Create a reverse proxy with IIS
categories: sysadmin
thumbnail:
tags:
 - IIS
 - Proxy
 - Windows
 - ARR
---

# IIS reverse proxy server

Make sure you have "IIS URL Rewrite" module installed first. [Download link](http://www.iis.net/download/URLRewrite)

If you dont be careful of installing it in a shared config setup, you have to disable shared config then install rewrite module on both servers and then enable shared config.

Install ARRv3 for reverse proxy capabilities. You will probably run into same issue as above if using shared configs. [Download link](http://www.iis.net/download/ApplicationRequestRouting)

## Don't bother with per site reverse proxy settings for example

* [iis-support-blog/setup-iis-with-url...](https://techcommunity.microsoft.com/t5/iis-support-blog/setup-iis-with-url-rewrite-as-a-reverse-proxy-for-real-world/ba-p/846222#)
* [gunnarpeipman.com/plex-iis-reverse-proxy...](https://gunnarpeipman.com/plex-iis-reverse-proxy/)

## This is more along our lines of setup

* https://community.spiceworks.com/how_to/73336-so-you-need-to-share-port-443-iis-arr

Set it globally under server farm. You will need to close and open IIS Manager to see this new option.

Add a new farm. Put IP address of new server. Leave "Online" checked.

## Under proxy

Check "Reverse rewrite host in response headers".

## Under caching

Un check "Enable disk cache".

## Under Routing Rules

Un check "Enable SSL offloading".

## Under "Monitoring and Management"

Ensure "Request Distribution (%)" is showing 100%.

If not change setting under "Load Balance" to be custom distribution 100% to new farm.

## Summary

This setup works well with HTTP. It should also be working fine with HTTPS. 

You will need matching bindings on each server for each sites. 

_The sites will also need to be online each server. However it can be a blank site on the proxy server._

## Screenshots

The IP addresses have been masked for privacy reasons.

![server-list](/blog/assets/2020-06-04/server-list.png)

![server-status](/blog/assets/2020-06-04/server-status.png)