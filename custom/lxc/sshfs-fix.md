# sshfs fix

> SYMPTOM: When running: `$ sshfs -o idmap=user <user>@37.97.132.144:/remote/path /mnt/mountpoint` and lxc-containers' bash print: `fuse: device not found, try 'modprobe fuse' first`.
> Although using 'fuse' would be good advice on a regular (OS on metal) system it should _not_ be executed inside lxc-containers, but on the host system (assumption: lxc-kernel usage related).

1. Issue 'modprobe fuse' _on the host system (Turris)_:

	```
	# modprobe fuse
	```

2. Issue the following from _within the container_:

	```
	$ mknod -m 666 /dev/fuse c 10 229
	```

## References

> Adapted from: Proxmox
> [Kernel module fuse for lxc][1]

> Also see: About Linux
> [LSA guide][2]

> Also see: github
> [LXC issues][3]


<!-- REFERENCES -->

[1]:https://forum.proxmox.com/threads/kernel-module-fuse-for-lxc.24855/page-2

[2]:http://linux.about.com/od/lsa_guide/a/gdelsa23t01.htm

[3]: https://github.com/lxc/lxc/issues/80


<!-- NGREP ONELINERS

>>> fix `fuse: device not found, try 'modprobe fuse' first`: 1) host/~# modprobe fuse, 2) container/~$ mknod -m 666 /dev/fuse c 10 229

-->
