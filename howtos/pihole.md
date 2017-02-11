# Install Pi-hole (router level add blocker)

## Procedure

1. Create a new LXC container (this how-to assumes either: _Ubuntu xenial_ or _Debian Jessie_).
2. Have your [router's DHCP server][1] assign a static IP address to the LXC container:
    1. **Hostname**: `pihole`.
    2. **MAC-Address**: found at:
      - Regular: The lxc container's configuration file (this remains to be tested!!!).
      - Turris Omnia: **[Services/Containers][2] > container > more > configure > ``lxc.network.hwaddr``**.
    3. **IPv4-Address**: Any address available in your network topology (e.g. `192.168.1.2`).
4. Make the containers startup automatically:
  - Regular: Add the line `lxc.start.auto = 1` to the container's configuration file.
  - Turris omnia: Edit configuration file `/etc/config/lxc-auto` to include:

    ```shell
    config container
      option name pihole
      option timeout 30
    ```
    Containers configured here will get started at the boot and each of them will be correctly halted during the shutdown. Set timeout option to specify how much time in seconds do the containers have to gracefully shutdown before being killed (by default: 300s.).
5. Boot the container and gain entrance:

  ```shell
$ lxc-start -n pihole
$ lxc-attach -n pihole
```
6. Install packages required to get the installer going:

  ```shell
$ apt-get install -y ca-certificates curl
```
7. Install and configure pi-hole itself (via an ncurses installer wizard):

  ```shell
$ curl -sSL https://install.pi-hole.net | bash
```
  1. When asked for (ipv4) nameservers, select `custom` and enter your router's ip (typically: `192.168.1.1`).
  2. Other than that use common sence (applying defaults where unsure).
  3. Note the password at the end of the installation wizard (although it can be changed later, should you ever forget it: `pihole -a -p new_password`).
8. Check if the Pi-hole server is properly running by logging into it; e.g `http://192.168.1.2/admin` (using the password noted in the previous step).
9. Change [DHCP settings] of the LAN interface to use the Pi-hole as a primary DNS, and the router's as its fallback:
  - Turris Omnia: 
    1. **[LuCi][3] > Interface (usually: LAN) > Edit > DHCP Server > Advanced Settings**
    2. **DHCP-Options**: `6,192.168.1.2,192.168.1.1`
10. Renew your existing IP/leases on your (desktop) clients to make them aware of the new DNS server.
11. Clear browser cashes if nessesary.

> Alternatively configure clients themselves to use the pihole as their DNS server statically.

ENJOY ADD-FREE BROWSING

But please consider donating registering for a subscriptions at content providers you deem worthy of your money!


# Notes

Hyperlinks used in this how-to are pointing to the LuCi interface of the open source Turris Omnia router.


<!-- REFERENCES -->

[1]:http://192.168.1.1/cgi-bin/luci/admin/network/dhcp
[2]:http://192.168.1.1/cgi-bin/luci/admin/services/lxc
[3]:http://192.168.1.1/cgi-bin/luci/admin/network/network
