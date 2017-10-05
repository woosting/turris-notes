# Create container

Create the container:

```
# lxc-create -B btrfs -n <containername> -t download -P /srv/lxc/ -- -d <distribution> -r <release> -a <architecture>
```

<!-- REFERENCES -->



<!-- NGREP ONELINERS

>>> Create LXC container from a template: # lxc-create -B btrfs -n <containername> -t download -P /srv/lxc/ -- -d <distribution> -r <release> -a <architecture>

-->
