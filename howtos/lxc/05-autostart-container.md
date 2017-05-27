# Auto start container

1. Open the lxc-auto file for editing: `vim /etc/config/lxc-auto`

2. Adapt the file to make it resemble as follows:

    ```shell
    config container
            option name my_first_vm
            option timeout 60

    config container
            option name my_second_vm
            option timeout 120
    ```

    > With `timeout` specifying how much time, in seconds, containers have for gracefull shutdowns before being killed (default: 300 s).