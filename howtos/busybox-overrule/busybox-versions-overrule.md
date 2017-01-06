# Manually

`root@turris:~# /usr/lib/opkg/info/wget.postinst; /usr/lib/opkg/info/less.postinst`

# Automated (by calling the postinst after every update)
`root@turris:~# echo -e '#!/bin/sh\n /usr/lib/opkg/info/less.postinst\n /usr/lib/opkg/info/wget.postinst' > /etc/updater/hook_postupdate/04_hooks.sh`

See: https://forum.turris.cz/t/wget-busybox-vs-package-wget/2547/3
