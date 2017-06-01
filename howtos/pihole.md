# Install Pi-hole (router level add blocker)

## Procedure

1. Create a new LXC container.

    > This how-to assumes either: _Ubuntu xenial_ or _Debian Jessie_.

2. Have your assign a static IP address to the LXC container at *[LuCi: Network > DHCP and DNS >][1] Static Leases*:

    1. **Hostname**: `pihole`.
    2. **MAC-Address**: `xx:xx:xx:xx:xx:xx` open the lxc container's configuration file: *[LuCi: Services > LXC Containers][2] > target-container > more > configure* and search for:

        ```
        ...

        lxc.network.hwaddr = xx:xx:xx:xx:xx:xx

        ...
        ```

    3. **IPv4-Address**: Any address available in your network topology (e.g. `192.168.1.2`).

3. Make the containers startup automatically open the configuration file `/etc/config/lxc-auto` and include:

    ```shell
    config container
        option name pihole
        option timeout 30
    ```

    >Containers configured here will get started at boot and correctly be halted during shutdowns. Set timeout options specify how much time in seconds the containers have to gracefully shutdown before being killed (default: 300 seconds).

    > Or, with regular - non Turris - LXC containers, add the line: `lxc.start.auto = 1` to their configuration file.

4. Boot the container: `lxc-start -n pihole`

5. Enter the container: `lxc-attach -n pihole`

6. Install packages required to get the installer going: `apt install -y ca-certificates curl`

7. Install and configure Pi-hole itself (via an ncurses installer wizard): `$ curl -sSL https://install.pi-hole.net | bash`

    1. When asked for (ipv4) nameservers, select `custom` and enter your router's ip (typically: `192.168.1.1`).
    2. Other than that use common sence (applying defaults where unsure).
    3. Note the password at the end of the installation wizard (although it can be changed later, should you ever forget it: `pihole -a -p new_password`).

5. Exit the container: `exit`

8. Check if the Pi-hole server is properly running by browsing to (and logging into) the webinterface of Pi-hole (e.g. `http://192.168.1.2/admin`, using the password noted in the previous step).

9. Use the Pi-hole as a primary DNS and use the router's regular one as its fallback by changing the DHCP settings of the LAN interface; *[LuCi > Network > Interfaces][3] > Target interface (usually: LAN) > [Edit] > DHCP Server > |Advanced Settings|* to:

    - **DHCP-Options**: `6,192.168.1.2,192.168.1.1`

10. Renew your existing IP/leases on your (desktop) clients to make them aware of the new DNS server.
11. Clear browser cashes if nessesary.


# Notes

Hyperlinks used in this how-to are pointing to the LuCi interface of the open source Turris Omnia router.

Alternatively configure clients to use the Pi-hole as their DNS server statically instead of step 9 to 11 if preferred.


<!-- REFERENCES -->

[1]:http://192.168.1.1/cgi-bin/luci/admin/network/dhcp
[2]:http://192.168.1.1/cgi-bin/luci/admin/services/lxc
[3]:http://192.168.1.1/cgi-bin/luci/admin/network/network
