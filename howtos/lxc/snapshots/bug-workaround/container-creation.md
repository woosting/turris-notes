1. root@turris:~# `btrfs subvolume create /srv/lxc/<containername>`
2. Login to: [LuCI](192.168.1.1/cgi-bin/luci/)
3. Navigate to: ***Services > LXC Containers***
4. Create the container having the same name as the just created btrfs subvolume.
5. root@turris:~# `~/import -c <containernaam> -u <username>` .. grab coffee ..
6. root@turris:~# `lxc-attach -n <containername>`
4. root@container:~# `passwd`
5. root@container:~# `<password>` (2x)
5. `root@container:~# vim /home/<username>/.ssh/authorized_keys` and populate it.
6. `root@container:~# exit`
7. `root@turris:~# lxc-stop -n <containername>`
8. `root@turris:~# btrfs subvolume  snapshot /srv/lxc/<containername> /srv/lxc/SNAPSHOTS./<containername>/<date-time(iso_8601)_note>`
9. `add private key (referral) to your ssh client of choice (e.g. putty on Microsoft Windows)
10. Connect via ssh
