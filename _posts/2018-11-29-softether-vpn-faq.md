---
layout: post
title: SoftEther VPN FAQ
date: 2018-11-29 17:07:19
author: Peter Stevenson
summary: Cheat sheet for SoftEther VPN
categories: networking
thumbnail:
tags:
 - SoftEther
---

# SoftEther VPN FAQ

_Moved here from [original gist](https://gist.github.com/2E0PGS/5ca5ee44cd942ca9943a87272f38e738)_

Cheat sheet for a few quirks I discovered in SoftEther VPN that are not immediately obvious.

## Routing and Remote Access

If you enable the "Routing and Remote Access" service in Windows (tested on 10) it causes the SoftEther VPN client running on that machine to drop packets.

## VPN concentrator

This is tested with SecureNAT enabled.

If you wish to run a SoftEther VPN client and a SoftEther VPN server on the same Linux or Windows host then you probably want the SoftEther VPN server to follow the system route table.

To achieve this you must forcefully prevent SoftEther VPN server from using `KernelModeSecureNAT` or `RawIpModeSecureNAT`. This is because they communicate using low level APIs directly to the drivers.

You need to open the "Edit Virtual Hub Extended Option List" and set `DisableKernelModeSecureNAT` and `DisableRawIpModeSecureNAT` to `1`.

This ensures the virtual SecureNAT uses "UserMode". More documentation available here in Japanese: [ja.softether.org/4-docs/3-kb/VPNFAQ036](https://ja.softether.org/4-docs/3-kb/VPNFAQ036)

The following issue helped pointed me to that page which I then translated using google: [SoftEtherVPN/issues/349#issuecomment-349570182](https://github.com/SoftEtherVPN/SoftEtherVPN/issues/349#issuecomment-349570182)

## Linux VPN client drops packets

Try setting the following setting under "Advanced Settings" of the connection properties dialogue `Disable UDP Acceleration`.