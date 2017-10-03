# Create container

1. Create the container:

	```
	# lxc-create -B btrfs -n <containername> -t download -P /srv/lxc/ -- -d <distribution> -r <release> -a <architecture>
	```

2. Snapshot the base container:

	```
	# lxc-snapshot -n <containername>
	```


<!-- REFERENCES -->



<!-- NGREP ONELINERS

>>> Create LXC container from a template: # lxc-create -B btrfs -n <containername> -t download -P /srv/lxc/ -- -d <distribution> -r <release> -a <architecture>

>>> Snapshot LXC container: # lxc-snapshot -n <containername>

-->
