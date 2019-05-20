# SoftEther Linux client/server start on boot

Based on a Debian crontab.

The SoftEther client requires a manual DHCP client call after starting to get an IP.

```
@reboot /home/anon/vpnclient/vpnclient start

@reboot sleep 10; /sbin/dhclient vpn_vpn

@reboot sleep 20; /home/anon/vpnserver/vpnserver start
```
