To provide guests with a DNS (as they can not reach turris' one after completing the [openWRT (LuCI) recipe for making a guest WiFi network](https://wiki.openwrt.org/doc/recipes/guest-wlan-webinterface)):

0. Login to: [LuCI](192.168.1.1/cgi-bin/luci/)
1. Navigate to: ***Network > Interfaces***
2. Click (at the guest network): **[edit]**
3. Scroll down to: **DHCP Server**
4. Click: **|Advanced Settings|**
5. Fill out: **DHCP-Options**: `6,192.168.2.1`

> NOTE: The `6` (in step 5) has something to do the data-length of the DHCP option (for more info refer to [networksorcery.com](http://www.networksorcery.com/enp/protocol/bootp/options.htm)).

> REFERENCE: https://wiki.openwrt.org/doc/recipes/guest-wlan-webinterface

> REFERENCE: https://wiki.openwrt.org/doc/recipes/guest-wlan
