# mSATA SSD assembly

Application: Internal storage for LXC containers.

## Procedure

1. Assemble the new disk; by instructions (Youtube): [Turris Omnia: How to connect an mSATA disk][1].
2. Optional: Partition the new disk as you need by the `fdisk /dev/sda` via SSH.
3. Format the new drive: `mkfs.btrfs /dev/sda` or `mkfs.ext4 /dev/sda`
4. Mount the new filesystem to a mountpoint either via:
  - LuCI: _systeem > Mount Points_
  - SSH CLI:
    1. Find the UUID of chosen partition by `blkid /dev/sda1`.
    2. Edit the `/etc/config/fstab` file and insert new `config mount` section below the `config global` section:
      ```
      config mount
          option uuid 'your-partition-uuid-number-here'
          option enabled '1'
          option target '/srv/lxc'
      ```
    3. Save the file and restart the router.
5. Login to SSH and take a look at the output of the mount command. You should see your /dev/sdaX partition mounted on /srv/lxc, the place where router installs LXC containers by default. Also you should see it using LuCI under the menu System.
6. Create LXC containers as described in the [LXC howto][2] (are created in `/srv/lxc`, to which the mSATA drive is now mounted).


<!-- REFERENCES -->

[1]:https://www.youtube.com/watch?v=71_M2N3ga7s
[2]:https://www.turris.cz/doc/en/howto/lxc
