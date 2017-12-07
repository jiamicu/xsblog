---
title: docker更改启动目录
date: 2017-12-04 18:34:29
tags: docker linux
---
You can change Docker's storage base directory (where container and images go) using the -g option when starting the Docker daemon.

Ubuntu/Debian: edit your /etc/default/docker file with the -g option: DOCKER_OPTS="-dns 8.8.8.8 -dns 8.8.4.4 -g /mnt"

Fedora/Centos: edit /etc/sysconfig/docker, and add the -g option in the other_args variable: ex. other_args="-g /var/lib/testdir". If there's more than one option, make sure you enclose them in " ". After a restart, (service docker restart) Docker should use the new directory.

Using a symlink is another method to change image storage.

Caution - These steps depend on your current /var/lib/docker being an actual directory (not a symlink to another location).

1) Stop docker: service docker stop. Verify no docker process is running ps faux
2) Double check docker really isn't running. Take a look at the current docker directory: ls /var/lib/docker/
2b) Make a backup - tar -zcC /var/lib docker > /mnt/pd0/var_lib_docker-backup-$(date +%s).tar.gz
3) Move the /var/lib/docker directory to your new partition: mv /var/lib/docker /mnt/pd0/docker
4) Make a symlink: ln -s /mnt/pd0/docker /var/lib/docker
5) Take a peek at the directory structure to make sure it looks like it did before the mv: ls /var/lib/docker/ (note the trailing slash to resolve the symlink)
6) Start docker back up service docker start
7) restart your containers