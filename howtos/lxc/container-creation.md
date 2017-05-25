# Container creation

## 1. Create clean container

1. Create the BTRFS subvolume that will host the container:

  ```shell
  btrfs subvolume create /srv/lxc/<containername>
  ```

2. Create a container with the same name as the created btrfs subvolume:

  ```shell
  lxc-create -n <containername> -t download -P </path/to/container/directory> -- -d <distribution> -r <release> -a <architecture>
  ```

3. Start the container:

  ```shell
  lxc-start -n <containername>
  ```

## 2. Populate the container

1. Populate the container with basic tooling and create a regular user:

  ```shell
  cimport -c <containernaam> -u <username>
  ```

  Info: Grab a cup of coffee (this may take over 10 minutes)..

  > 1. Download the script should it not be present yet:
  >     ```shell
  >     git clone https://github.com/woosting/cimports.git </path/to/scripts/>cimports
  >     ```
  >
  > 3. Make the script executable:
  >     ```shell
  >     chmod 755 </path/to/scripts/>cimports/cimport.sh
  >     ```
  > 4. Place a symbolic link in the path to make it available from any location:
  >    ```shell
  >    ln -s </path/to/scripts/>cimports/cimport.sh /usr/bin/cimport
  >    ```

5. Enter the container:

  ```shell
  lxc-attach -n <containername>
  ```
    
6. Change the password of root by an interactive script (follow instructions on screen):

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

## 3. Snapshot the container

9. Stop the container:

  ```shell
  lxc-stop -n <containername>
  ```

10. Make a BTRFS snapshot of the container:

  ```shell
  btrfs subvolume snapshot /srv/lxc/<containername> /srv/lxc/SNAPSHOTS/<containername>/<date-time(iso_8601)_note>
  ```

9. Start the container:

  ```shell
  lxc-start -n <containername>
  ```

## 4. Create migratable backup

1. Execute the script (may take over 20 minutes):

  ```shell
  lxc-backup
  ```

> SITUATIONAL: In case you do not have the script yet
>
> 1. Download the script:
>     ```shell
>     git clone https://github.com/woosting/lxc-backup.git /srv/scripts/lxc-backup
>     ```
> 
> 3. Make the script executable:
>     ```shell
>     chmod 755 /srv/scripts/lxc-backup/lxc-backup.sh
>     ```
>     
> 4. Place a symbolic link in the path to make it available from any location:
>    ```shell
>    ln -s /srv/scripts/lxc-backup/lxc-backup.sh /usr/bin/lxc-backup
>    ```


### 4. RSA-key based login

1. Edit your authorized_keys file:

  ```shell
  vim /home/<username>/.ssh/authorized_keys
  ```
  
2. Populate it with the **public** RSA-keys you wish to grand access to the container (change with your keys):

  ```shell
  ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAEAQDIYPme3EYXldsqUnV8l6+L3ObnEphusSM3Xs8GxjMxVHVd1Lo5LUi949SR7onsVKN+ac7A5CuZHA5cT0FEjIVTa31mrasdaId3gItyPqh+bV+RAcGmqo4VefQdAr1asdaRHl1Kb6wnVXWRMLpCyw5I3N3v4i1tXLm3SWvEVwIgh8Pjtjrtjr3Nyo+tOfus3kTwRWOvDfVHguhrV8dfgHLT8X+kWPzxJWIn/pL/Mvu6LhKslXhuGU/xnsda7sNiJWFK8QPQ5zvKRE06DOD1ba+nY7SAKLHjjxq4wltQttXoPtlMjg0KA1TTTePsLeVfVq/a4ERHFpr2ZsWdsrfhUzrqvyGv4CvaN4t14t5Dvn6x45e2YEtsBsdafrhwK6DQ9q8+ya1DtEaZadgCAofwEtBhHyDm9rRo4rB7sjd65uJBwO1bF93sSv1RldfdlTMuNsf9hxQnmIUVl+drl/unyKzagT7p2K47nt55Q8P6DcsdFvCsddF2m+IHt5n+3bCzQD58McaFHAt29DDhRkjyB3SQI26wk5jrtj+pW2G+YuDmesfgjAXumj50W/YL6tHsppqKNNQky7HpljxSjQ49dqxSymZ89b97k2FcNdOHasdYorarcIMJt9kFYezxYjYWqbFnrt5uZ0674W435uZgJ+M7br5tg84f3weddwx5efkrg6h7ig4osKbPTAMJz67d+6z/lHXVg7YadrZDYjR1uZQ9b5XBpgmuZIasdNQ/aeH9FYNoLUjkxghjNfttbuw54JwqF4bzSEzcKJHGwyjroaskdfg0r7i0ISs/kD5j7EpGo1gHdtf4bp7C5Vo6qT8ix6eu3aUZucyc1n+E36aZdA2v1y66D8Dfs+dSwMsdSTQ+Kz5CIF8CiBsEiECCcecEnHW1xGXQgARtj+MUyAA3cl4NT0Ee7YpD7Jpaft7jc56rgR67gjk9ilC656yhecvwdzwu5PjxV8b54b3bba457bd45bg2346A2346xb877P77qM4HsSohhqaa6XdM3H3lV3kgrdfd4sbRNRWh1px6pCT9j6T7j8csQ6fO7UFlCZ6aIxoanvMQXbof56SzDimikVmHWDydwU5LmpajdDoasH8VSnmJ9gF0SBiwdIDyjNyl3Ptpe7s42d1Y32DA9nDBRq2f6kT8krbHsXIsj7vryemVt4aK9Te3ni6U33kCuMlN/6fL04V7+hWMkTFP33oe2EWwuaOQwkq1qCobS+woLOzuLnF0Rhj7+YWGLyI2TdLpjJd8J4XshTSQIskju6PsG+8+ZUi2Toqm7FFiTwVKZuxS7JPglSLAdasdakAaNZkA5gSg/w11Pk6M5t0RQIyq1NgBPPXitBnFs0d3kKs2hmz user@host
  ```

3. Instruct your ssh client of choice (e.g. putty on Microsoft Windows) to use the corresponding private when connecting to the container.

4. Connect via ssh using the user's account (not root; you can `su` to super user from within the container if required).


### 5. Make the container start at (re)boot

1. Edit your lxc-auto file:
  ```shell
  vim /etc/config/lxc-auto
  ```

  and adapt the file as such:

  ```
  config container
          option name my_first_vm
          option timeout 60

  config container
          option name my_second_vm
          option timeout 120
  ```

> Note: `timeout` specifies how much time, in seconds, containers have for gracefull shutdowns before being killed (default: 300 s).


# References

- [LXC [Project: Turris]][1]: Official Turris  documentation

<!-- REFERENCES -->
[1]:https://www.turris.cz/doc/en/howto/lxc