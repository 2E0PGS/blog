# SoftEther Linux client start on boot

Based on a Debian crontab.

```
@reboot /home/anon/vpnclient/vpnclient start

@reboot sleep 10; /sbin/dhclient vpn_vpn

@reboot sleep 20; /home/anon/vpnserver/vpnserver start
```
