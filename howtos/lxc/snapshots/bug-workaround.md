1. root@turris:~# `btrfs subvolume create /srv/lxc/<containername>`
2. Login to: [LuCI](192.168.1.1/cgi-bin/luci/)
3. Navigate to: ***Services > LXC Containers***
5. Create the container having the same name as the just created btrfs subvolume.
6. Start the container (eitehr in LuCI or by: `lxc-start -n <container-name>`.
7. root@turris:~# `~/import -c <containernaam> -u <username>` .. grab coffee ..
8. root@turris:~# `lxc-attach -n <containername>`
9. root@container:~# `passwd`
10. root@container:~# `<password>` (2x)
11. root@container:~# `vim /home/<username>/.ssh/authorized_keys` and populate it with your *public* RSA-keys.
12. root@container:~# `exit`
13. root@turris:~# `lxc-stop -n <containername>`
15. root@turris:~# `btrfs subvolume  snapshot /srv/lxc/<containername> /srv/lxc/SNAPSHOTS./<containername>/<date-time(iso_8601)_note>`
16. Add private key (referral) to your ssh client of choice (e.g. putty on Microsoft Windows)
17. Connect via ssh
