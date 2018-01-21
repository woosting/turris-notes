# mSATA SSD assembly

Add internal storage to the Turris Omnia (for LXC containers):

1. Assemble the new disk:
[Turris Omnia: How to connect an mSATA disk][1].
2. Partition the new disk as you deem fit via SSH:
	```
	# fdisk /dev/sda
	```
3. Format the new drive:
	```
	# mkfs.btrfs /dev/sda1
	```
	> ALTERNATIVE:
	> ```
	> # mkfs.ext4 /dev/sda1
	> ```
4. Mount the new filesystem to a mountpoint: LuCI: _systeem > Mount Points_
	SSH CLI:
	> ALTERNATIVE: Mount by CLI:
	> 1. Find the UUID of chosen partition:
	>		```
	>		# blkid /dev/sda1
	>		```
	> 2. Open the fstab file for editing:
	>		```
	>		# vim /etc/config/fstab
	>		```
	>		Insert a new `config mount` section below the `config global` section:
	>
	>		```shell
	>		config mount
	>			option uuid
	>			'your-partition-uuid-number-here'
	>			option enabled '1'
	>			option target '/srv/lxc'
	>		```
	> 3. Save the file and restart the router.


> FUNCTIONALITY CHECK: Login to SSH and take a look at the output of the mount command. You should see your /dev/sdaX partition mounted on /srv/lxc, the place where router installs LXC containers by default. Also you should see it using LuCI under the menu System.

> APPLICATION (example): Create LXC containers as described in the [LXC howto][2] (are created in `/srv/lxc`, to which the mSATA drive is now mounted).


<!-- REFERENCES -->

[1]:https://www.youtube.com/watch?v=71_M2N3ga7s
[2]:https://www.turris.cz/doc/en/howto/lxc


<!-- NGREP ONELINERS

>>> Find the UUID of a block device or partition: # blkid /dev/sda<n>

-->
