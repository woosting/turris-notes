# Create container

> NOTE: This procedure employs BTRFS to make snapshots on filesystem level because regular [lxc-snapshot functionality results in an rsync error on Turris][1]. Once `lxc-snapshot` is working properly regular linux procedures can be used.

1. Create the BTRFS subvolume that will host the container: `btrfs subvolume create /srv/lxc/<containername>`

2. Create a container with the same name as the created btrfs subvolume: `lxc-create -n <containername> -t download -P /srv/lxc/ -- -d <distribution> -r <release> -a <architecture>`

<!-- REFERENCES -->
[1]:https://forum.turris.cz/t/lxc-snapshot-resulting-in-an-rsync-error/1849