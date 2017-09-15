# Snapshot container

0. Ensure a base snapshot target directory exists:

	```
	# mkdir /srv/lxc/SNAPSHOTS/
	```

1. Create a container-specific snapshot target directory:

	```
	# mkdir /srv/lxc/SNAPSHOTS/<directoryname>
	```

2. Make a BTRFS snapshot of the container:

	```
	# btrfs subvolume snapshot /srv/lxc/<containername> /srv/lxc/SNAPSHOTS/<containername>/<date-time(iso_8601)_note>
	```

> NOTE: This procedure employs BTRFS to make snapshots on filesystem level because regular [lxc-snapshot functionality results in an rsync error on Turris][1]. Once `lxc-snapshot` is working properly regular GNU/Linux procedures can be used instead.

<!-- REFERENCES -->
[1]:https://forum.turris.cz/t/lxc-snapshot-resulting-in-an-rsync-error/1849


<!-- NGREP ONELINERS

>>> Make a BTRFS snapshot of an LXC container: # btrfs subvolume snapshot /srv/lxc/<containername> /srv/lxc/SNAPSHOTS/<containername>/<date-time(iso_8601)_note>

-->
