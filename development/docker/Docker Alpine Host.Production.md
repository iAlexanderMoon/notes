# Alpine Docker Development Host Setup

## Virtuabox Setup VM:
alpine-linux
10 GB Disk
4 GB Ram
run setup
* reset the root password
* reboot

## Software and User Configuration
Enable community APK repository for docker apk
```
vi /etc/apk/repositories
apk upgrade --update-cache --available
```

Install make, sudo, docker and set docker on default boot
````
apk add make
apk add sudo
apk add docker
apk add docker
rc-update add docker default
````

Configure user for access to sudo and docker, docker group is created while adding docker apk.
````
adduser <username>
addgroup sudo
adduser <username> sudo
adduser <username> docker
````

Configure users in sudo group access to all commands by editing sudoers file: 
````
visudo
````

## Networking
eth0 NAT interface
eth1 Host Ony interface

#### Virtuabox VM Secondary Network Adapter
In virtuabox enable 2nd network adapter: Host Only
 
#### Configure Windows Host Only Network Adapter
* Configure Windows Adapter in Control Panel 
* IPv4 Address: 192.168.36.1 
* Gateway: 192.168.36.2

#### Configure Linux Adapter eth1 in /etc/network/interfaces
* address can be added in the range of the Windows Host Only Network 192.168.36.*
````
auto eth1
iface eth1 inet static
        address 192.168.36.3
        netmask 255.255.255.0
        network 192.168.36.0
````

#### Add Windows Host Record
* C:\Windows\System32\Drivers\etc\hosts
192.168.36.4	production

## SSH Login by Keys
* see Windows or Linux for generating keys and isntalling terminal

scp ~/.ssh/id_rsa.pub root@docker:.
ssh root@docker
> cat id_rsa.pub >> .ssh/authorized_keys

login again without the need of a password
