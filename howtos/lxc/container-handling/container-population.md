# Populate the container

1. Populate the container with basic tooling and create a regular user:`cimport -c <containername> -u <username>`

    > In case you do not have the script yet:
    > 1. Download the script (create the target directory if required): `git clone https://github.com/woosting/cimports.git /srv/scripts/cimports`
    > 3. Make the script executable: `chmod 755 /srv/scripts/cimports/cimport.sh`
    > 4. Place a symbolic link in the path to make it available from any location: `ln -s /srv/scripts/cimports/cimport.sh /usr/bin/cimport`

2. Grab a cup of coffee... (±10 minutes)

3. Enter the container: `lxc-attach -n <containername>`

4. Change the password of root (interactive script; follow instructions on screen): `passwd`

5. Change the hostname of the container: `hostnamectl set-hostname <new-hostname>`

6. Leave the container: `exit`