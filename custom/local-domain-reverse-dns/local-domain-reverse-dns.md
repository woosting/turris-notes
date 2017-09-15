# Local domain reverse DNS

## Set dnsmasq's server port

1. Login to: [LuCI](192.168.1.1/cgi-bin/luci/)
2. Navigate to: ***Network > DHCP and DNS***
3. Click: **| Advanced Settings |**
3. Scrol down and fill out: **DNS Server Port**: `5353`

## Set kresd policy (persisting)

1. Make the custom configuration file:

	```
	# echo -e "local lan_rule = policy.add(policy.suffix(policy.FORWARD('127.0.0.1@5353'),  policy.todnames({'lan'}))) \npolicy.del(lan_rule.id) \ntable.insert(policy.rules, 1, lan_rule)" > /etc/kresd/custom.conf
	```

	> NOTE: The intended contents are echoed and subsequently redirected to a file.

	> NOTE: This file persists reboots and - more importantly - updates, as it is placed 'in/etc/kresd/'.

2. Make a backup of the resolver config and open it for editing:

	```
	# cp /etc/config/resolver /etc/config/resolver.bak && vim /etc/config/resolver
	```

3. Replace line 22:

    ```shell
	...
    #option include_config '/tmp/kresd.custom.conf'
	...
	```
    with:

    ```shell
	...
    option include_config '/etc/kresd/custom.conf'
	...
	```

	> NOTE: The `#` (hash-tag character) removed and path change from `/tmp/` to `/etc/`)

3.  Restart 'kresd' and 'dnsmasq':

	```
	# /etc/init.d/kresd restart && /etc/init.d/dnsmasq restart
	```

> ALTERNATIVE: Combine all steps in one CLI command:
>
> ```
> # echo -e "local lan_rule = policy.add(policy.suffix(policy.FORWARD('127.0.0.1@5353'),  policy.todnames({'lan'}))) \npolicy.del(lan_rule.id) \ntable.insert(policy.rules, 1, lan_rule)" > /etc/kresd/custom.conf && cp /etc/config/resolver /etc/config/resolver.bak && sed -i 's/#option include_config '\''\/tmp\/kresd\.custom\.conf'\''/option include_config '\''\/etc\/kresd\/custom.conf'\''/g' /etc/config/resolver && /etc/init.d/kresd restart && /etc/init.d/dnsmasq restart
> ```
> CAUTION: This 'one-command' still needs testing!


## References

> Adapted from: Turris Omnia forum:
> [Dnsmasq lan domain while still usinge knot resolver][1]

<!-- REFERENCES -->

[1]:https://forum.turris.cz/t/dnsmasq-lan-domain-while-still-using-knot-resolver/1253
