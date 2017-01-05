In the guest (container):

1. `cp /lib/systemd/system/getty@.service /etc/systemd/system`

2. `vim /lib/systemd/system/getty@.service`

3. Comment-out the line:

	```
	ConditionPathExists=/dev/tty0
	```

	to become:

	```
	#ConditionPathExists=/dev/tty0
	```

Source: https://wiki.debian.org/LXC