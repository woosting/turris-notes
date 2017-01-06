# Set dnsmasq's server port

1. Login to: [LuCI](192.168.1.1/cgi-bin/luci/)
2. Navigate to: ***Network > DHCP and DNS ***
3. Click the hyperlinc-tab: **|Advanced Settings|**
3. Scrol down and fill out: **DNS Server Port** to be: `5353`

# Set (update persistent) kresd policy

1. `root@turris:~# echo "policy.add(policy.suffix(policy.FORWARD('127.0.0.1@5353'),  policy.todnames({'lan'})))" >> /etc/kresd.custom.conf` (persisting reboots and updates, as it is custom).

2. `root@turris:~# cp /etc/config/resolver /etc/config/resolver.bak`

3. `root@turris:~# vim /etc/config/resolver` and replace line 22:

  ```
  #option include_config '/tmp/kresd.custom.conf'
  ```
  ...with (note the removal of the hash-tag!):

  ```
  option include_config '/etc/kresd.custom.conf'
  ```
    
  > ALTERNATIVE (step 3 in one command):
  >
  > `root@turris:~# sed -i 's/#option include_config '\''\/tmp\/kresd\.custom\.conf'\''/option include_config '\''\/etc\/kresd.custom.conf'\''/g' /etc/config/resolver`

4. `root@turris:~# cp /etc/init.d/kresd /etc/init.d/kresd.bak`

5. `root@turris:~# vim /etc/init.d/kresd` and move the lines 134 & 135:

  ```
  # include custom kresd config
  include_custom_config
  ```

  ...right above line 122 & 123:

  ```
  config_get_bool do_forward "$section" forward_upstream 1
  if [ "$do_forward" = "1" ] ; then
  ```

  ... resulting in:

  ```
  # include custom kresd config
  include_custom_config
  config_get_bool do_forward "$section" forward_upstream 1

  if [ "$do_forward" = "1" ] ; then
  ```

6. `root@turris:~# /etc/init.d/kresd restart && /etc/init.d/dnsmasq restart`

> Also see: https://forum.turris.cz/t/dnsmasq-lan-domain-while-still-using-knot-resolver/1253/29
