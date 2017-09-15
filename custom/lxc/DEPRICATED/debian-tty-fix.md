# POSSIBLY DEPDRICATED (fixed in container template).

In the guest (container):

1. root@container:~# `cp /lib/systemd/system/getty@.service /etc/systemd/system`

2. root@container:~# `vim /lib/systemd/system/getty@.service` and comment-out the line:

    ```
	ConditionPathExists=/dev/tty0
	```

	to become:

	```
	#ConditionPathExists=/dev/tty0
	```

> REFERENCE: https://wiki.debian.org/LXC
