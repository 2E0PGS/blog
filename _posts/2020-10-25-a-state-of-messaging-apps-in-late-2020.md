---
layout: post
title: A state of messaging apps in late 2020
date: 2020-10-25 13:30:10
author: Peter Stevenson
summary: A wide breadth of alternative messaging apps
categories: personal
thumbnail:
tags:
 - Android
---

# A state of messaging apps in late 2020

* Occasionally I checkout the latest messaging apps for fun. 
* Over the years I have tried pretty much every app that looks usable.
* I may include a few apps/services which aren't strictly IM (Instant Messaging) apps.
* There is quite a wide breadth of alternative messaging apps many may not be aware of.

## Common

* WhatsApp - WhatsApp Web is great.
	* [Open Whisper Systems partners with WhatsApp to provide end-to-end encryption](https://signal.org/blog/whatsapp/)
	* [WhatsApp's Signal Protocol integration is now complete](https://signal.org/blog/whatsapp-complete/)
	* \> Media and messages you back up aren't protected by WhatsApp end-to-end encryption while in iCloud.
	* \> Media and messages you back up aren't protected by WhatsApp end-to-end encryption while in Google Drive.
* WeChat - More commonly used in Asia.
* SMS/MMS - It's a useful backup.
* Email - Probably hit its peak a long time ago, now becoming outdated.
* Skype
	* [Signal partners with Microsoft to bring end-to-end encryption to Skype](https://signal.org/blog/skype-partnership/)
* Facebook Messenger
	* [Facebook Messenger deploys Signal Protocol for end-to-end encryption](https://signal.org/blog/facebook-messenger/)
* Snapchat - False sense of privacy.
* Kik
* Viber
* LINE
* iMessage - Apple specific.
* Android Messages - Android specific.
	* [Google is rolling out end-to-end encryption for RCS in Android Messages beta](https://www.theverge.com/2020/11/19/21574451/android-rcs-encryption-message-end-to-end-beta)
* Steam chat - Steam specific.
* Xbox Live - Xbox specific.
* PSN aka PlayStation Messages - PSN specific.

## Common team chats

* House party
* Zoom
* Webex
* Microsoft Teams - Typing latency but decent VOIP.
* Google Meets
* Google Hangouts
	* Could also mention Google Allo and Google Duo.
	* [Open Whisper Systems partners with Google on end-to-end encryption for Allo](https://signal.org/blog/allo/)
* Slack - The VOIP sucks but chat is great.
* Gitter - Specific to OSS development for the most part.
* Discord - Also implements a half decent VOIP. Although it's webRTC based which can cause issues with routing.
	* [Discord message encryption plugin - SimpleDiscordCrypt](https://gitlab.com/An0/SimpleDiscordCrypt#)

## Early IM

* IRC
* ICQ
* AIM
* Yahoo Messenger
* Pidgin/Libpurple
* MSN Messenger aka Windows Live Messenger
* eBuddy

## Early VOIP

* Mumble - Open source and self hosted.
* TeamSpeak - You can host your own secure server, the only downside is the text chat is basic and it's not open source.
* Asterisk - Great VOIP software but has a bit of a learning curve unless using FreePBX.

## Decentralised focused team chats

* Riot.im aka Matrix aka Element.io - They boast a huge selection of bridges/connectors which allow you to link various chat systems.
* rocket.chat - Self hosted Discord/Slack alternative.
* Jami
* Mattermost
* Jitsi - Open source Zoom alternative.
* Zulip

## Privacy focused

* Signal - Probably the forerunner in this category.
* Silence.im - Fork of Signal focusing on SMS and no Google push service dependency.
* FireChat - Mesh networking.
* Briar - A modern version of FireChat imo.
* Telegram
* BCM - Blockchain messenger.
* DeltaChat - IM style PGP email. 
	* Uses Signal's user interface classes so looks similar.
	* [Does Delta Chat support end-to-end-encryption?](https://delta.chat/en/help#does-delta-chat-support-end-to-end-encryption)
	* \> Delta Chat implements the Autocrypt Level 1 standard
* Session - Very cool as it only requires a GUID to chat, although their servers had uptime issues.
* Keybase - The social network for PGP key owners.
	* [Keybase Crypto Documents - saltpack](https://book.keybase.io/docs/crypto#saltpack-message-format)
	* [saltpack GitHub repo](https://github.com/keybase/saltpack)
* Tox - Not tested.
* PGP/GPG email - A classic but rather involved setup email encryption solution.
	* [SKS poisoning, keys.openpgp.org / Hagrid and other non-solutions](https://blogs.gentoo.org/mgorny/2019/07/04/sks-poisoning-keys-openpgp-org-hagrid-and-other-non-solutions/)
	* [SKS Keyserver Network Under Attack](https://gist.github.com/rjhansen/67ab921ffb4084c865b3618d6955275f#the-consequences)
* Wire - Not tested.
* Threema - Not tested.