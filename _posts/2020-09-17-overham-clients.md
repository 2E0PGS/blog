---
layout: post
title: Overham clients
date: 2020-09-17 18:09:10
author: Peter Stevenson
summary: Creating clients for the Overham API
categories: programming
thumbnail:
tags:
 - Overham
 - OverhamJ
 - dotnet
 - aspnet
 - Core
 - REST
 - API
 - .NET
 - ASP.NET
 - Node
 - NodeJS
 - Insomnia
 - MVC
---

# Creating clients for the Overham API

Overham API is written by my friend [2E0EOL](http://www.daybologic.co.uk/services.php?content=overham)

## IRC client

Around 2017‑03‑20 I created a IRC bot/client written in NodeJS [overham-irc](https://bitbucket.org/2E0PGS/overham-irc)

IRC overview of methods

![irc-overview.jpg](/blog/assets/2020-09-17/irc-overview.jpg)

IRC resistor lookup

![irc-resistor.jpg](/blog/assets/2020-09-17/irc-resistor.jpg)

## Insomnia tests

Around 2017-11-21 I did some Insomnia REST tests against the API.

Recording search

![insomnia-recording-search.jpg](/blog/assets/2020-09-17/insomnia-recording-search.jpg)

Recording status

![insomnia-recording-status.jpg](/blog/assets/2020-09-17/insomnia-recording-stats.jpg)

## Website client

Around 2018‑12‑15 I created a area in my [ASP.NET Core website](https://bitbucket.org/2E0PGS/core) to host a client web UI for Overham using my [.NET Standard client library for Overham](https://bitbucket.org/2E0PGS/overham-client).

Core overview of methods

![core-overview.jpg](/blog/assets/2020-09-17/core-overview.jpg)

Core resistor lookup

![core-resistor.jpg](/blog/assets/2020-09-17/core-resistor.jpg)

Core recording search

![core-recording-search.jpg](/blog/assets/2020-09-17/core-recording-search.jpg)
