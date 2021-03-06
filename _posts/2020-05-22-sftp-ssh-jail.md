---
layout: post
title: SFTP/SSH jail
date: 2020-05-22 23:45:19
author: Peter Stevenson
summary: Jail environment for SFTP only file transfer
categories: sysadmin
thumbnail:
tags:
 - Linux
 - security
---

# SFTP/SSH jail

The following sets up a jail environment for SFTP only file transfer for a specific user.

Based on `openssh-server` and a Ubuntu based OS.

## Create a new user home folder

`sudo mkdir /home/sftuser`

## Create the user

`sudo adduser --home /home/sftpuser/ sftpuser`

## Edit the openssh-server config

`sudo vim /etc/ssh/sshd_config`

## Make sure you have the following line in it

`Subsystem sftp internal-sftp`

In my case I commented the default out with a pound sign: 

`Subsystem sftp internal-sftp #/usr/lib/openssh/sftp-server`

## Append the following to end of the config

```
Match User sftuser
    ChrootDirectory /home/sftuser
    AllowTCPForwarding no
    X11Forwarding no
    ForceCommand internal-sftp
```

## Restart sshd service

`sudo service sshd restart`

## Create a writable directory

`sudo mkdir /home/sftuser/writable`

## Allow the user to own that writable folder

`sudo chown sftuser:sftuser /home/sftuser/writable`

## You should have the following folders

`adminuser` is our admin level account home folder. You will probably have a different name for your admin/sudoers level account. This account was the first user we setup during OS installation. The user we are currently using to SSH in.

```
ls -al /home
total 16
drwxr-xr-x  4 root      root      4096 Oct 28 10:23 .
drwxr-xr-x 23 root      root      4096 Oct 28 10:22 ..
drwxr-xr-x  5 adminuser adminuser 4096 Oct 28 10:50 adminuser
drwxr-xr-x  4 root      root      4096 Oct 28 10:44 sftpuser
```

```
ls -al /home/sftpuser/
total 16
drwxr-xr-x 4 root     root     4096 Oct 28 10:44 .
drwxr-xr-x 4 root     root     4096 Oct 28 10:23 ..
drwx------ 2 sftpuser sftpuser 4096 Oct 28 10:34 .cache
drwxr-xr-x 2 sftpuser sftpuser 4096 Oct 28 10:51 writeable
```

## Test it is secure

* FileZilla try escape directory.
* OpenSSH client try getting a bash shell.

## References

* [askubuntu.com/a/134442](https://askubuntu.com/a/134442)
* [howtoforge.com/restricting-users...](https://www.howtoforge.com/restricting-users-to-sftp-plus-setting-up-chrooted-ssh-sftp-debian-squeeze)
