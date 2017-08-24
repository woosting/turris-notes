# Install Pi-hole

> NOTE Pi-hole is a router-level add blocker (thus protecting the entire network).


> NOTE: For _Ubuntu xenial_ or _Debian Jessie_.

1. Create a new LXC container.

2. Assign a static IP address to the LXC container at *[LuCi: Network > DHCP and DNS >][1] Static Leases*:

    1. Set **hostname**: `pihole`
    2. Set **MAC-Address**: `xx:xx:xx:xx:xx:xx` open the lxc container's configuration file: *[LuCi: Services > LXC Containers][2] > target-container > more > configure* and search for:

        ```shell
        lxc.network.hwaddr = xx:xx:xx:xx:xx:xx
        ```

    3. **IPv4-Address**: Any address available in your network topology (e.g. `192.168.1.2`).

3. Make the containers startup automatically open the configuration file `/etc/config/lxc-auto` and include:

    ```shell
    config container
        option name pihole
        option timeout 30
    ```

    > NOTE: Containers configured here will get started at boot and correctly be halted during shutdowns. Set timeout options specify how much time in seconds the containers have to gracefully shutdown before being killed (default: 300 seconds).

    > ALTERNATIVE: Regular - non Turris - LXC containers, need to have  line: `lxc.start.auto = 1` to their configuration file.

4. Boot the container:

	```
	# lxc-start -n pihole
	```

5. Enter the container:

	```
	# lxc-attach -n pihole
	```

6. Install packages required to get the installer going:

	```
	# apt install -y ca-certificates curl
	```

7. Install and configure Pi-hole itself (via an ncurses installer wizard):

	```
	# curl -sSL https://install.pi-hole.net | bash
	```

    1. When asked for (ipv4) nameservers, select `custom` and enter your router's ip (typically: `192.168.1.1`).
    2. Other than that use common sence (applying defaults where unsure).
    3. Note the password at the end of the installation wizard (although it can be changed later, should you ever forget it: `pihole -a -p new_password`).

5. Exit the container:

	```
	# exit
	```

8. Check if the Pi-hole server is properly running by browsing to (and logging into) the webinterface of Pi-hole (e.g. `http://192.168.1.2/admin`, using the password noted in the previous step).

9. Set the DHCP to the Pihole IP address and use the router's regular one as its fallback: *[LuCi > Network > Interfaces][3] > Target interface (usually: LAN) > [Edit] > DHCP Server > | Advanced Settings |* to: **DHCP-Options**: `6,192.168.1.2,192.168.1.1`

	> ALTERNATIVE: Configure clients to use the Pi-hole as their DNS server statically.

10. Renew your existing IP/leases on your (desktop) clients to make them aware of the new DNS server.

11. Clear browser caches if necessary.


> NOTE: Hyperlinks used in this how-to are pointing to the LuCi interface of the open source Turris Omnia router.


<!-- REFERENCES -->

[1]:http://192.168.1.1/cgi-bin/luci/admin/network/dhcp
[2]:http://192.168.1.1/cgi-bin/luci/admin/services/lxc
[3]:http://192.168.1.1/cgi-bin/luci/admin/network/network
