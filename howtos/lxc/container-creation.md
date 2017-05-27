# Container creation


## Create clean container

1. Create the BTRFS subvolume that will host the container: `btrfs subvolume create /srv/lxc/<containername>`

2. Create a container with the same name as the created btrfs subvolume: `lxc-create -n <containername> -t download -P /srv/lxc/ -- -d <distribution> -r <release> -a <architecture>`

3. Start the container: `lxc-start -n <containername>`


## Populate the container

1. Populate the container with basic tooling and create a regular user:`cimport -c <containername> -u <username>`

    > In case you do not have the script yet:
    > 1. Download the script (create the target directory if required): `git clone https://github.com/woosting/cimports.git /srv/scripts/cimports`
    > 3. Make the script executable: `chmod 755 /srv/scripts/cimports/cimport.sh`
    > 4. Place a symbolic link in the path to make it available from any location: `ln -s /srv/scripts/cimports/cimport.sh /usr/bin/cimport`

2. ...Grab a cup of coffee (±10 minutes)...

3. Enter the container: `lxc-attach -n <containername>`

4. Change the password of root (interactive script; follow instructions on screen): `passwd`

5. Change the hostname of the container: `hostnamectl set-hostname <new-hostname>`

6. Leave the container: `exit`


## Snapshot the container

1. Stop the container: `lxc-stop -n <containername>`

2. Create a container-specific snapshot target directory (create the base `SNAPSHOTS` directory dirst if not present yet): mkdir /srv/lxc/SNAPSHOTS/<directoryname>

2. Make a BTRFS snapshot of the container: `btrfs subvolume snapshot /srv/lxc/<containername> /srv/lxc/SNAPSHOTS/<containername>/<date-time(iso_8601)_note>`

3. Start the container: `lxc-start -n <containername>`


## Backup the container

1. Stop the container: `lxc-stop -n <containername>`

2. Make a migratable tarfile (create the base `BACKUPS` directory dirst if not present yet): `tar --numeric-owner -czvf /srv/lxc/BACKUPS/<yyyymmdd>t<hhmm>-<containername>.tar.gz -C /srv/lxc/ <containername>`

3. ...Grab a cup of coffee (±5 minutes)...

4. Start the container: `lxc-start -n <containername>`


## Situational

### Make the container start at (re)boot

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

    > Note: `timeout` specifies how much time, in seconds, containers have for gracefull shutdowns before being killed (default: 300 s).


### RSA-key based login

1. Enter the container: `lxc-attach -n <containername>`

2. Open the authorized keys\_file for editing: `vim /home/<username>/.ssh/authorized_keys`

3. Populate it with the **public** RSA-keys you wish to grand access to the container (change with your keys):

    ```shell
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAEAQDIYPme3EYXldsqUnV8l6+L3ObnEphusSM3Xs8GxjMxVHVd1Lo5LUi949SR7onsVKN+ac7A5CuZHA5cT0FEjIVTa31mrasdaId3gItyPqh+bV+RAcGmqo4VefQdAr1asdaRHl1Kb6wnVXWRMLpCyw5I3N3v4i1tXLm3SWvEVwIgh8Pjtjrtjr3Nyo+tOfus3kTwRWOvDfVHguhrV8dfgHLT8X+kWPzxJWIn/pL/Mvu6LhKslXhuGU/xnsda7sNiJWFK8QPQ5zvKRE06DOD1ba+nY7SAKLHjjxq4wltQttXoPtlMjg0KA1TTTePsLeVfVq/a4ERHFpr2ZsWdsrfhUzrqvyGv4CvaN4t14t5Dvn6x45e2YEtsBsdafrhwK6DQ9q8+ya1DtEaZadgCAofwEtBhHyDm9rRo4rB7sjd65uJBwO1bF93sSv1RldfdlTMuNsf9hxQnmIUVl+drl/unyKzagT7p2K47nt55Q8P6DcsdFvCsddF2m+IHt5n+3bCzQD58McaFHAt29DDhRkjyB3SQI26wk5jrtj+pW2G+YuDmesfgjAXumj50W/YL6tHsppqKNNQky7HpljxSjQ49dqxSymZ89b97k2FcNdOHasdYorarcIMJt9kFYezxYjYWqbFnrt5uZ0674W435uZgJ+M7br5tg84f3weddwx5efkrg6h7ig4osKbPTAMJz67d+6z/lHXVg7YadrZDYjR1uZQ9b5XBpgmuZIasdNQ/aeH9FYNoLUjkxghjNfttbuw54JwqF4bzSEzcKJHGwyjroaskdfg0r7i0ISs/kD5j7EpGo1gHdtf4bp7C5Vo6qT8ix6eu3aUZucyc1n+E36aZdA2v1y66D8Dfs+dSwMsdSTQ+Kz5CIF8CiBsEiECCcecEnHW1xGXQgARtj+MUyAA3cl4NT0Ee7YpD7Jpaft7jc56rgR67gjk9ilC656yhecvwdzwu5PjxV8b54b3bba457bd45bg2346A2346xb877P77qM4HsSohhqaa6XdM3H3lV3kgrdfd4sbRNRWh1px6pCT9j6T7j8csQ6fO7UFlCZ6aIxoanvMQXbof56SzDimikVmHWDydwU5LmpajdDoasH8VSnmJ9gF0SBiwdIDyjNyl3Ptpe7s42d1Y32DA9nDBRq2f6kT8krbHsXIsj7vryemVt4aK9Te3ni6U33kCuMlN/6fL04V7+hWMkTFP33oe2EWwuaOQwkq1qCobS+woLOzuLnF0Rhj7+YWGLyI2TdLpjJd8J4XshTSQIskju6PsG+8+ZUi2Toqm7FFiTwVKZuxS7JPglSLAdasdakAaNZkA5gSg/w11Pk6M5t0RQIyq1NgBPPXitBnFs0d3kKs2hmz user@host
    ```

4. Leave the container: `exit`

5. Instruct your ssh client (e.g. putty on Windows) to use the corresponding private key when connecting to the container.

6. Connect via ssh using the user's account (not root; you can `su` to super user from within the container if required).


# References

- [LXC [Project: Turris]][1]: Official Turris  documentation


<!-- REFERENCES -->
[1]:https://www.turris.cz/doc/en/howto/lxc