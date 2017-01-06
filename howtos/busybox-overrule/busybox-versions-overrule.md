# Manually

`# /usr/lib/opkg/info/wget.postinst`

# Automated
`# echo -e '#!/bin/sh\n/usr/lib/opkg/info/less.postinst' > /etc/updater/hook_postupdate/04_hooks.sh`

See: https://forum.turris.cz/t/wget-busybox-vs-package-wget/2547/3
