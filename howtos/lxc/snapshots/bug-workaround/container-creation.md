1. Turris cli: `btrfs subvolume create /srv/lxc/<containername>`
2. Luci: Create the container having the same name as the just created btrfs subvolume.
2. Turris cli: `~/import -c <containernaam> -u <username>` .. grab coffee ..
3. Turris cli: `lxc-attach -n <containername>`
4. Container cli: `passwd`
5. Container cli: `<password>` (2x)
5. Container cli: `vim /home/<username>/.ssh/authorized_keys` and populate it.
6. Container cli: `exit`
7. Turris cli: `lxc-stop -n <containername>`
8. Turris cli: `btrfs subvolume  snapshot /srv/lxc/<containername> /srv/lxc/SNAPSHOTS./<containername>/<date-time(iso_8601)_note>`
9. Possibly: add private key referral to ssh client (e.g. putty).
10. Connect via ssh and enjoy!
