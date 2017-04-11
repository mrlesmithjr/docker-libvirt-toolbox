Repo Info
=========
This repo is for creating a usable [Docker] image for running [libvirt] related
tasks from. This image uses [Alpine] Linux as the base image.
- Works great from [macOS]

Building
--------

Build image:
```
docker build -t libvirt-toolbox .
```

Usage
-----
When you launch a container from this image you will be logged in as the user
`remote` within the `/home/remote` directory.
To run without building:
```
docker run -it mrlesmithjr/libvirt-toolbox
```
To run after building:
```
docker run -it libvirt-toolbox
```

`/home/remote/.ssh` volume has been added as a possible volume mount that you
may want to utilize as well for things such as:
* ssh keys
* ssh known hosts
* etc.

Mounting a volume for ssh related files within the current users `.ssh` directory:
```
docker run -it -v ~/.ssh:/home/remote/.ssh mrlesmithjr/libvirt-toolbox
```
Now you can generate your SSH keys and add hosts to known_hosts to persist
even if the container is destroyed.

Manage remote KVM/QEMU system(s):
```
virsh -c qemu+ssh://remote@nas01/system list
 Id    Name                           State
----------------------------------------------------
 1     lb-01a                         running
 2     jumpbox                        running
 3     gitlab-runner                  running
```

Connect to remote VM console:
```
virsh -c qemu+ssh://nas01/system console maas
...
Connected to domain maas
Escape character is ^]

Ubuntu 16.04.1 LTS localhost ttyS0

localhost login:
Password:
Welcome to Ubuntu 16.04.1 LTS (GNU/Linux 4.4.0-57-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

administrator@localhost:~$
```

Clone an existing VM/template to another VM:
```
virt-clone --connect qemu+ssh://nas01/system -o ubuntu1604 -n maas -f /home/remote/maas.qcow2
...
Allocating 'maas.qcow2'                                                                                                                                    |  36 GB  00:00:15

Clone 'maas' created successfully.
```

License
-------

BSD

Author Information
------------------

Larry Smith Jr.
- @mrlesmithjr
- http://everythingshouldbevirtual.com
- mrlesmithjr [at] gmail.com

[Alpine]: <https://alpinelinux.org/>
[Docker]: <https://www.docker.com/>
[libvirt]: <https://libvirt.org/>
[macOS]: <www.apple.com/macos>
