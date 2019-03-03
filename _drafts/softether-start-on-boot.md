how to have softether client start on boot linux?

@reboot /home/anon/vpnclient/vpnclient start
@reboot sleep 10; dhclient

@reboot /home/anon/vpnserver/vpnserver start
