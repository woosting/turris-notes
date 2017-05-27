# Create container

> NOTE: On Turris OS the [lxc-snapshot command results in an rsync error][1]. This workaround employs BTRFS to make snapshots on filesystem level instead. Once lxc-snapshot is properly working the a regular (linux general) procedure can be used.

1. Create the BTRFS subvolume that will host the container: `btrfs subvolume create /srv/lxc/<containername>`

2. Create a container with the same name as the created btrfs subvolume: `lxc-create -n <containername> -t download -P /srv/lxc/ -- -d <distribution> -r <release> -a <architecture>`

<!-- REFERENCES -->
[1]:https://www.turris.cz/doc/en/howto/lxc