# Set dnsmasq's server port

1. Login to: [LuCI](192.168.1.1/cgi-bin/luci/)
2. Navigate to: ***Network > DHCP and DNS***
3. Click: **|Advanced Settings|**
3. Scrol down and fill out: **DNS Server Port** to be: `5353`

# Set (update persisting) kresd policy

1. Make the custom configuration file (persisting reboots and updates as it is placed in/etc/kresd/): root@turris:~# `echo -e "local lan_rule = policy.add(policy.suffix(policy.FORWARD('127.0.0.1@5353'),  policy.todnames({'lan'}))) \npolicy.del(lan_rule.id) \ntable.insert(policy.rules, 1, lan_rule)" > /etc/kresd/custom.conf`.

2. root@turris:~# `cp /etc/config/resolver /etc/config/resolver.bak && vim /etc/config/resolver` and replace line 22:

    ```
    #option include_config '/tmp/kresd.custom.conf'
    ```
    ...with (note the removal of the hash-tag and the explicit change of path!!):

    ```
    option include_config '/etc/kresd/custom.conf'
    ```
    
    > ALTERNATIVE (cli instead of vim editing): root@turris:~# `cp /etc/config/resolver /etc/config/resolver.bak && sed -i 's/#option include_config '\''\/tmp\/kresd\.custom\.conf'\''/option include_config '\''\/etc\/kresd\/custom.conf'\''/g' /etc/config/resolver`

3. root@turris:~# `/etc/init.d/kresd restart && /etc/init.d/dnsmasq restart`

    > REFERENCE: https://forum.turris.cz/t/dnsmasq-lan-domain-while-still-using-knot-resolver/1253
