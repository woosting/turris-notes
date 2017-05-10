# Container installation procedure

1. root@turris:~# `btrfs subvolume create /srv/lxc/<containername>`
2. Browse to: [LuCI](192.168.1.1/cgi-bin/luci/) ***Services > LXC Containers***
5. Create the container having the same name as the just created btrfs subvolume.
6. Start the container (either in LuCI or by: `lxc-start -n <container-name>`.
7. root@turris:~# `~/import -c <containernaam> -u <username>` .. grab coffee ..
8. root@turris:~# `lxc-attach -n <containername>`
9. root@container:~# `passwd`
10. root@container:~# `<password>` (2x)
11. root@container:~# `vim /home/<username>/.ssh/authorized_keys` and populate it with your *public* RSA-keys.
12. root@container:~# `hostnamectl set-hostname <new-hostname>`.
12. root@container:~# `exit`
13. root@turris:~# `lxc-stop -n <containername>`
15. root@turris:~# `btrfs subvolume  snapshot /srv/lxc/<containername> /srv/lxc/SNAPSHOTS/<containername>/<date-time(iso_8601)_note>`
16. Add private key (referral) to your ssh client of choice (e.g. putty on Microsoft Windows)
17. root@turris:~# `lxc-start -n <containername>`
18. Connect via ssh


## Optionally

root@turris:~# `vim /etc/config/lxc-auto` and adapt the file as such:

```
config container
        option name my_first_vm
        option timeout 60
        
config container
        option name my_second_vm
        option timeout 120
```
> Note: Set timeout option to specify how much time in seconds do the containers have to gracefully shutdown before being killed. The default value is 300. 


# References

[LXC [Project: Turris]][1]

<!-- REFERENCES -->
[1]:https://www.turris.cz/doc/en/howto/lxc
