# Auto start container

1. Open the lxc-auto config file for editing:

	```
	# vim /etc/config/lxc-auto
	```

2. Adapt the file to make it resemble as follows:

    ```shell
    config container
            option name <my_first_vm>
            option timeout <xxx>

    config container
            option name <my_second_vm>
            option timeout <xxx>
    ```

    > `option timeout <xxx>` The time containers get for graceful shutdowns before being killed (default: 300 seconds).
