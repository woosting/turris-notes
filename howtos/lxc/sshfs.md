When running: user@container:~$ `sshfs -o idmap=user <user>@37.97.132.144:/remote/path /mnt/mountpoint`.

lxc-containers print: `fuse: device not found, try 'modprobe fuse' first`.

Although this would be good advice on a regular (OS on metal) system it should _not_ be executed inside lxc-containers, but on the host system (assumption: lxc-kernel usage related).
o:

So do the following instead:

1. root@turris:~# `modprobe fuse`
2. root@container:~# `mknod -m 666 /dev/fuse c 10 229`

> REFERENCE: https://forum.proxmox.com/threads/kernel-module-fuse-for-lxc.24855/page-2

> REFERENCE: http://linux.about.com/od/lsa_guide/a/gdelsa23t01.htm

> REFERENCE: https://github.com/lxc/lxc/issues/80
