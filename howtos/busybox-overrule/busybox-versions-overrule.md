# Manually

1. `# /usr/lib/opkg/info/wget.postinst`

# Automated

1. `# vim /etc/updater/hook_postupdate/04_hooks.sh`
2. Populate it with:

  ```
  #!/bin/sh
  /usr/lib/opkg/info/less.postinst
  ```
See: https://forum.turris.cz/t/wget-busybox-vs-package-wget/2547/3
