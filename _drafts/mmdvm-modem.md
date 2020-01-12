pi-star beta image for raspi3
pinouts
may need the ign pin of modem pulled high? car ignition for auto boot of pi?
sql pin of rig doesnt seem to be needed
rssi pin of modem doesnt seem to be needed
pi-star configuration/expert/mmdvmhost/txoffset is worth playing with try -100 or 100 or 200

txinvert was default 1, for some radios like mine you may need to change it to 0 however in my case I later found my radio was off frequency and upon correcting it (see ic-e208 calibration) I had to put txinvert back to original value.

ensure suffix of callsign for gateway is not just a space but its last char of input string: https://www.facebook.com/groups/pistarusergroup/414855285738167/?comment_id=414856705738025&reply_comment_id=415271039029925&notif_id=1554136716212493&notif_t=group_comment

# Useful links

* https://www.ebay.com/itm/MMDVM-DMR-Repeater-Open-Source-Multi-Mode-Digital-Voice-Modem-for-Raspberry-MJ-/163608363073?oid=143133159407
* http://www.radiomanual.info/schemi/ICOM_VU/IC-E208_user.pdf
* https://www.ebay.co.uk/itm/2018-latest-MMDVM-DMR-Repeater-Open-Source-Multi-Mode-Digital-Voice-Modem-Moto/352486107764?hash=item5211cf2a74:g:eJEAAOSwRE9bxBED
* https://wiki.brandmeister.network/index.php/Homebrew/MMDVM?fbclid=IwAR3wkTfMHb_fN2V6INoDoh30Li06tqzpZdKBPKN5aTUeyScjTOPN0jQ8aS0#Recommend_radios_for_homebrew_repeaters
* Robs pictures he took of the docs he received with his board.

See DVAP page on my website.