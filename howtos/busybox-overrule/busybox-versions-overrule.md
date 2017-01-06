# Manually
1. root@turris:~# `/usr/lib/opkg/info/wget.postinst`
2. root@turris:~# `/usr/lib/opkg/info/less.postinst`

> ALTERNATIVE (step 1 and 2 in one command): `root@turris:~# /usr/lib/opkg/info/wget.postinst; /usr/lib/opkg/info/less.postinst`

# Automated (by calling the postinst after every update)
1. root@turris:~# `vim /etc/updater/hook_postupdate/04_hooks.sh`
2. Populate it with:

  ```
  #!/bin/sh
  /usr/lib/opkg/info/less.postinst
  ```
  
> ALTERNATIVE (step 1 and 2 in one command): `root@turris:~# echo -e '#!/bin/sh\n /usr/lib/opkg/info/less.postinst\n /usr/lib/opkg/info/wget.postinst' > /etc/updater/hook_postupdate/04_hooks.sh`

> REFERENCE: https://forum.turris.cz/t/wget-busybox-vs-package-wget/2547/3
