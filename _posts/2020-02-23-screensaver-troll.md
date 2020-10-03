---
layout: post
title: Screensaver troll
date: 2020-02-23 16:07:10
author: Peter Stevenson
summary: Gnome screensaver leave a message
categories: sysadmin
thumbnail:
tags:
 - Linux
 - security
---

# Screensaver troll

My work colleagues pranked me but they reviled a security bug.

Both `mate-screensaver` and `gnome-screensaver` have a feature to leave a message on the lock screen. However it doesn't leave an audit trail.

What happened was my work colleagues left a message saying: "you have been hacked".

I login and see a `notify-send` message with no context and no way of knowing what triggered it. 

Eventually I figured out the message must be local and I started questioning my colleagues. Once I found out the full story I was actually grateful because it helped my improve my security.

Disable the button on the lock screen using ini: `/usr/share/mate-screensaver/lock-dialog-default.ui`

Change the line `<property name="visible">True</property>` to `<property name="visible">False</property>` in the following block like so.

```
<child>
	<object class="GtkButton" id="auth-note-button">
		<property name="label" translatable="yes">_Leave Message</property>
		<property name="visible">False</property>
		<property name="can_focus">True</property>
		<property name="focus_on_click">False</property>
		<property name="can_default">True</property>
		<property name="receives_default">False</property>
		<property name="use_underline">True</property>
	</object>
	<packing>
		<property name="expand">False</property>
		<property name="fill">False</property>
		<property name="position">0</property>
	</packing>
</child>
```

## Screenshots

Lock screen

![lock-screen](/blog/assets/2020-02-23/lock-screen.png)

Lock screen message

![lock-screen-message](/blog/assets/2020-02-23/lock-screen-message.png)

Notify send message

![notify-send-message](/blog/assets/2020-02-23/notify-send-message.png)

Lock screen after patching

![lock-screen-patched](/blog/assets/2020-02-23/lock-screen-patched.png)