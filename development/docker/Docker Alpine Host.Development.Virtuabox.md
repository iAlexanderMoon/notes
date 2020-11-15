# Alpine Docker Development Host Setup

## Virtuabox Setup VM:
alpine-linux
20 GB Disk
4 GB Ram
run setup
* reset the root password
* reboot

## Additional Tools and Basic Setup

Install make, vim, sudo, pv
````
apk add make
apk add vim
apk add sudo
apk add pv
````

Configure users in sudo group access to all commands by editing sudoers file: 
````
visudo
````

Configure User
````
adduser <username>
addgroup sudo
adduser <username> sudo
````


## Bridge interface for local machine access
By default NAT will be enabled on eth0 connection to host and can't be ssh'd or samba into it
Add a second bridge interface bridge on eth1 on a different network:

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
192.168.36.3	development

## SSH Login by Keys
* see Windows or Linux for generating keys and isntalling terminal

scp ~/.ssh/id_rsa.pub root@development:.
ssh root@development
> cat id_rsa.pub >> .ssh/authorized_keys

login again without the need of a password


## Docker
Install Docker in alpine and configure it to start on boot
Docker is only available in the community packages
* Give full access to docker by adding user to docker group
````
vim /etc/apk/repositories
apk upgrade --update-cache --available
apk add docker
rc-update add docker boot
adduser <username> docker

````

Reboot and make sure docker starts and user has access to docker
````
ps ax | grep docker
docker images
````

## Samba Access
* add samba
* add user to samba
* Set samba to start on reboot
````
apk add samba
smbpasswd -a <username>
rc-update add samba
````

##### Create a work folder /working
````
mkdir /working
````

##### Append working share to samba configuration
* /etc/samba/smb.conf

	[working]
		path = /working
		public = yes
		only guest = no
		writable = yes
		printable = no

#### test samba parameters
````
testparm
````

#### add root as a user
````
smbpasswd -a <username>
````

# start/top/restart
/etc/init.d/samba restart


## Swarm and Stack:

### Initialize Swarm
* Single Node in Swarm Mode
* Since we added a 2nd interface docker needs to know the default to bind to
````
docker swarm init --avertise-addr 192.168.36.3
````


### Initialize Stack

__! You can't run 2 stacks bound on the same ports at the same time !__
* default to production exposed range: [7000,7999]
* default to development exposed range: [8000,8999]
* default to tools exposed range: 9000 [9000,9999]

_ALTERNATIVE: Run multiple virtual machines one for production and one for development_

Instead of using

* Secrets:
* Configs:
* Proxy:

### Portainer
* Community Edition License: portainer.io Opensource License
* Web Interactive Interface use for local docker examination instead of command line
* Run directly on docker machine exposing docker.sock

Portainer allows you to easily view the logs and containers etc.

### Traefik


### Verdaccio
* License: MIT
* Create your npm registry
* Should a assign a host entry dns name
Configure npm to use local registry
```
npm config set registry http://registry.npmjs.org/
```


### What about running a nuget registry

### What about running a docker registry