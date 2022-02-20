---
layout: post
title: Visual safety development
date: 2022-02-20 16:16:19
author: Peter Stevenson
summary: Visual warnings when developing on multiple environments
categories: programming
thumbnail:
tags:
 - API
 - SQL
---

# Visual warnings when developing on multiple environments

It is common for a developer or sysadmin to have access to multiple environments or servers such as production (live) (server-1), test (staging) and perhaps localhost (server-2).

The problem with this is that it's easy to forget which environment you're using.

One solution would be to have a mental pre-check of "am I on server X... yes I am" before running any commands that perform changes (writes/updates). However this is bound to fail you one day.

Another solution is making a visual clue appear which makes it clear consciously or subconsciously which server you're using.

## SQL Server Management Studio (SSMS)

Server 1 connection pane. In this example server-1 is our live production environment.

![server-1](/blog/assets/2022-02-20/2022-02-10-17-21-42-server-1.png)

Server 1 connection options. 

![server-1-settings](/blog/assets/2022-02-20/2022-02-10-17-22-12-server-1-settings.png)

Server 2 connection pane. In this example server-2 is our localhost environment.

![server-2](/blog/assets/2022-02-20/2022-02-10-17-22-53-server-2.png)

Server 2 connection options.

![server-2-settings](/blog/assets/2022-02-20/2022-02-10-17-23-08-server-2-settings.png)

Here is the server list on the left. Top one is localhost (server-2) and bottom is live production (server-1).

![server-list](/blog/assets/2022-02-20/2022-02-10-17-24-39-1-server-list.png)

Running a SQL query against server-1 shows us the nice warning at the bottom.

![server-1-query-pane](/blog/assets/2022-02-20/2022-02-10-17-24-58-1-server-1-query-pane.png)

Running a SQL query against server-2 shows us we're safe.

![server-2-query-pane](/blog/assets/2022-02-20/2022-02-10-17-25-08-1-server-2-query-pane.png)

## Insomnia REST

I use Insomnia which is like Postman for my API testing. I have various environment variables such as URIs which get swapped out based on environment.

Live environment shows red as a warning.

![insomnia-live](/blog/assets/2022-02-20/2022-02-10-17-44-36-insomnia-live.png)

Staging environment shows orange as a warning.

![insomnia-staging](/blog/assets/2022-02-20/2022-02-10-17-44-57-insomina-staging.png)

Localhost environment shows green as we're safe.

![insomnia-local](/blog/assets/2022-02-20/2022-02-10-17-45-11-insomnia-local.png)

## Linux SSH

From my `.bashrc` I deploy on remote servers and local machines.

```sh
if [ "$color_prompt" == yes ]; then
	if [ ! "$SSH_CLIENT" == "" ]; then
		# If this is a ssh session then change our PS1 colour to let them know it's remote.
		PS1='${debian_chroot:+($debian_chroot)}\[\033[01;33m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
	else
		# If this isn't a ssh connection then use our normal colour PS1.
		PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
	fi
else
	PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```

Green for local.

![local-ssh](/blog/assets/2022-02-20/2022-02-20-16-54-03-local-ssh.png)

Yellow for remote.

![remote-ssh](/blog/assets/2022-02-20/2022-02-20-16-54-43-remote-ssh.png)