# Busybox version overrule

## Manual

Manually call after every update:

- Overrule 'wget':

	```
	# /usr/lib/opkg/info/wget.postinst
	```

- Overrule 'less':

 	```
	# /usr/lib/opkg/info/less.postinst
	```

>	ALTERNATIVE: Both in one command:
>	```
>	# /usr/lib/opkg/info/wget.postinst; /usr/lib/opkg/info/less.postinst
>	```

## Automated

Automatically called after every update:

1. Open the post update hook file for editing:

	```
	# vim /etc/updater/hook_postupdate/04_hooks.sh
	```

2. Populate it with:

	```shell
	#!/bin/sh
	/usr/lib/opkg/info/less.postinst
	```

> ALTERNATIVE: Both in one command:

```
# echo -e '#!/bin/sh\n /usr/lib/opkg/info/less.postinst\n /usr/lib/opkg/info/wget.postinst' > /etc/updater/hook_postupdate/04_hooks.sh
```

## REFERENCES

> Adapted from: Turris Omnia forum
> [Wget busybox v.s. package wget][1]


[1]:https://forum.turris.cz/t/wget-busybox-vs-package-wget/2547/3
