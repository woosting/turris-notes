# Container creation

1. Create the BTRFS subvolume that will host the container:

    ```shell
    btrfs subvolume create /srv/lxc/<containername>
    ```

2. Create a container with the same name as the created btrfs subvolume:

	```shell
    lxc-create -n <containername> -t download -P </path/to/container/directory> -- -d <distribution> -r <release> -a <architecture>
    ```
	> *Alternatives:*
	> 1. *Via interactive shell script: `lxc-create -n <containername> -t download -P </path/to/container/directory>`*
	> 2. *Via web-interface: [LuCI](192.168.1.1/cgi-bin/luci/) > Services > LXC Containers*

3. Start the container:

    ```shell
    lxc-start -n <containername>
    ```

    > *Alternative: Via web-interface: [LuCI](192.168.1.1/cgi-bin/luci/) > Services > LXC Containers*

4. Populate the container with basic tooling and create a regular user:

    ```shell
    cimport -c <containernaam> -u <username>
    ```

    > Info: [Cimport](https://github.com/woosting/cimports) is a three-stage cascading script. In the first stage an initialisation script is copied into the container. During the second stage that script is executed to downloads a [base-install](https://github.com/woosting/baseInst) script. During the third stage the base-install script actually populates the container with basic applications (using apt) and creates the user with the requested username.
    
    Grab a cup of coffee (this may take over 10 minutes)...
    
5. Enter the container:

    ```shell
    lxc-attach -n <containername>
    ```
    
6. Change the password of the logged in user (root) by an interactive script:

    ```shell
    passwd
    ```
    
7. Change the hostname of the container:

    ```shell
    hostnamectl set-hostname <new-hostname>
    ```
    
8. Leave the container:

    ```shell
    exit
    ```

9. Stop the container:

    ```shell
    lxc-stop -n <containername>
    ```
	> *Alternative: Via web-interface: [LuCI](192.168.1.1/cgi-bin/luci/) > Services > LXC Containers*

10. Make a BTRFS snapshot of the container:

    ```shell
    btrfs subvolume snapshot /srv/lxc/<containername> /srv/lxc/SNAPSHOTS/<containername>/<date-time(iso_8601)_note>
    ```

9. Start the container:

    ```shell
    lxc-start -n <containername>
    ```
	> *Alternative: Via web-interface: [LuCI](192.168.1.1/cgi-bin/luci/) > Services > LXC Containers*

## Optionals

### Create migratable backup

1. Install the script (if not done allready):

	1. Download the lxc-backup script:
        ```shell
        wget https://raw.githubusercontent.com/woosting/lxc-backup/master/lxc-backup.sh -P </path/to/scripts>
        ```

	2. Make the script executable:
        ```shell
        chmod 755 </path/to/scripts>/lxc-backup.sh
        ```

	3. Place a symbolic link in the path:
        ```shell
        ln -s </path/to/scripts>/lxc-backup.sh /usr/bin/lxc-backup.sh
        ```

2. Execute the script:
    ```shell
    lxc-backup.sh
	```




### RSA-key based login
1. root@container:~# `vim /home/<username>/.ssh/authorized_keys` and populate it with your **public** RSA-keys.
2. Add private key (referral) to your ssh client of choice (e.g. putty on Microsoft Windows).
3. Connect via ssh using the user's account (not root; you can `su` to super user from within the container).


### Automatically start the container at Turris Omnia (re)boot

1. root@turris:~# `vim /etc/config/lxc-auto` and adapt the file as such:

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
