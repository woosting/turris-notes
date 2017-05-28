# Backup container

0. Ensure a base backup target directory exists: `mkdir /srv/lxc/BACKUPS/`

0. Ensure the container is not running: `lxc-stop -n <containername>`

2. Make a migratable tarfile: `tar --numeric-owner -czvf /srv/lxc/BACKUPS/<yyyymmdd>t<hhmm>-<containername>.tar.gz -C /srv/lxc/ <containername>`

3. Grab a cup of coffee... (±5 minutes)