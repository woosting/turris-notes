# Issue (bug?)

## SUSPECTED ROOT CAUSE:

**idmapd not starting** (suspected to be an (systemd) upstart issue in the Debian instance resulting from a faulty template).

Summary of [the discussion: 'nfs-common failing in LXC container (Debian template)'](https://forum.turris.cz/t/nfs-common-failing-in-lxc-container-debian-template/2689/8):

I seem to be getting similar errors during the installation of **nfs-common** (and nfs-kernel-server for that matter) inside a **Debian Jessie** LXC container:

    Creating config file /etc/idmapd.conf with new version
    Job for nfs-common.service failed. See 'systemctl status nfs-common.service' and 'journalctl -xn' for details.
    invoke-rc.d: initscript nfs-common, action "start" failed.
    dpkg: error processing package nfs-common (--configure):
     subprocess installed post-installation script returned error exit status 1
    Processing triggers for libc-bin (2.19-18+deb8u6) ...
    Processing triggers for systemd (215-17+deb8u5) ...
    Errors were encountered while processing:
     nfs-common
    E: Sub-process /usr/bin/dpkg returned an error code (1)

The command `systemctl status nfs-common.service` prints:

    ● nfs-common.service - LSB: NFS support files common to client and server
       Loaded: loaded (/etc/init.d/nfs-common)
       Active: failed (Result: exit-code) since Thu 2016-12-29 13:59:22 UTC; 2min 18s ago

    Dec 29 13:59:22 testserv rpc.idmapd[2506]: main: fcntl(/run/rpc_pipefs/nfs): Invalid argument
    Dec 29 13:59:22 testserv nfs-common[2495]: Starting NFS common utilities: statd idmapd failed!
    Dec 29 13:59:22 testserv systemd[1]: nfs-common.service: control process exited, code=exited status=1
    Dec 29 13:59:22 testserv systemd[1]: Failed to start LSB: NFS support files common to client and server.
    Dec 29 13:59:22 testserv systemd[1]: Unit nfs-common.service entered failed state.

The command `journalctl -xn` prints:

    -- Logs begin at Thu 2016-12-29 13:13:37 UTC, end at Thu 2016-12-29 13:59:23 UTC. --
    Dec 29 13:59:06 testserv systemd[1]: Reloading.
    Dec 29 13:59:21 testserv systemd[1]: Reloading.
    Dec 29 13:59:22 testserv systemd[1]: Reloading.
    Dec 29 13:59:22 testserv systemd[1]: Starting LSB: NFS support files common to client and server...
    -- Subject: Unit nfs-common.service has begun with start-up
    -- Defined-By: systemd
    -- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
    --
    -- Unit nfs-common.service has begun starting up.
    Dec 29 13:59:22 testserv rpc.idmapd[2506]: main: fcntl(/run/rpc_pipefs/nfs): Invalid argument
    Dec 29 13:59:22 testserv nfs-common[2495]: Starting NFS common utilities: statd idmapd failed!
    Dec 29 13:59:22 testserv systemd[1]: nfs-common.service: control process exited, code=exited status=1
    Dec 29 13:59:22 testserv systemd[1]: Failed to start LSB: NFS support files common to client and server.
    -- Subject: Unit nfs-common.service has failed
    -- Defined-By: systemd
    -- Support: http://lists.freedesktop.org/mailman/listinfo/systemd-devel
    --
    -- Unit nfs-common.service has failed.
    --
    -- The result is failed.
    Dec 29 13:59:22 testserv systemd[1]: Unit nfs-common.service entered failed state.
    Dec 29 13:59:23 testserv systemd[1]: Reloading.

The command: `rpc.idmapd -fv` (from the bug-report) prints:

    rpc.idmapd: libnfsidmap: using (default) domain: lan
    rpc.idmapd: libnfsidmap: Realms list: 'LAN'
    rpc.idmapd: libnfsidmap: loaded plugin /lib/arm-linux-gnueabihf/libnfsidmap/nsswitch.so for method nsswitch

    rpc.idmapd: Expiration time is 600 seconds.
    rpc.idmapd: nfsdopenone: Opening /proc/net/rpc/nfs4.nametoid/channel failed: errno 2 (No such file or directory)
    rpc.idmapd: main: fcntl(/run/rpc_pipefs/nfs): Invalid argument

This (taken from the output above): `Opening /proc/net/rpc/nfs4.nametoid/channel failed: errno 2 (No such file or directory)` seems to be related as the (marked) part indeed does not exist: /proc/net/rpc/`nfs4.nametoid/channel`

# Workarounds:

1. Use another container template (tested to work with 'Ubuntu Yakkety').

2. Use NSF3 (on server) and:

 - Turn off idmapd loading after installing 'nfs-common' (on the client):
    - root@container:~# `apt-get install nfs-common` (will semi-fail)
    - root@container:~# `vim /etc/default/nfs-common`
    - change `NEED_IDMAPD=` into `NEED_IDMAPD=no`
    - root@container:~# `apt-get upgrade -y`

 - [Change UIDs](https://askubuntu.com/questions/16700/how-can-i-change-my-own-user-id#16719) correspondingly with server if needed (*user can not be logged in during this change*):
    - root@container:~# `usermod -u <NEW_UID> <USERNAME>`
    - root@container:~# `find / -uid <OLD_UID> -exec chown -h <NEW_UID> {} +`

> REFERENCE: https://forum.turris.cz/t/nfs-common-failing-in-lxc-container-debian-template/2689/8
