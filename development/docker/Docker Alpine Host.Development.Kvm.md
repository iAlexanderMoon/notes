# Alpine Docker Development Host Setup

## KVM & Alpine Docker Host
* install libvirtd
* install virtualization tools

*Ubuntu & Debian?*
 
```
apt install qemu-kvm libvirt-bin
apt install libguestfs-tools
```

### Get Alpine Virtual Machine Image
Download alpine image created for virualization... version may need to be updated.
```
wget http://dl-cdn.alpinelinux.org/alpine/v3.8/releases/x86_64/alpine-virt-3.8.2-x86_64.iso 
```

### Create instance
Requirements of the image will depend on what is going to be run in the docker host, mssql takes more resources than postgres. Modify as required:

```
sudo virt-install --name=dockerhost \
    --vcpus=1 \
    --ram=2048 \
    --disk path=/var/lib/libvirt/images/dockerhost.img,size=8 \
    --os-type linux \
    --os-variant=generic \
    --network bridge:virbr0,model=virtio \
    --graphics none \
    --console pty,target_type=serial \
    --cdrom=alpine-virt-3.8.2-x86_64.iso
```

### Confirm default network is running (active)
```
virsh net-list --all
virsh net-autostart default
virsh net-start default
```

### Connect to Console
```
virsh console <name>
```
_To disconnect once connected: ^]_


### List running and all machines
```
virsh list
virst list --all
```

### Cloning
Alternatively, if you have a good guest template you can reuse it by cloning and modifying it.

```
 virt-clone \
		--original dockerhost \
		--name $(name) \
       	--file /var/lib/libvirt/images/$(name).img,size=8

```

After cloning you should probably do some image clean up. On your own systems leaving the SSH keys and passwords the same is PROBABLY the desired result. However, there are some things we likely want to change.

You can also run scripts during sysprep on the filesystem but in the context of the HOST machine --script.

Or copy and run scripts in the context of the machine on first boot --firstboot

```
virt-sysprep -d <name> --list-operations
```

* tmp-files: remove temp files
* logfiles: remove logfiles
* dhcp-client-state: clean dhcp
* crash-data: remove any crash-data
* udev-persistent-net: new mac address will be on eth0 instead of adding a new eth
* net-hostname: supposed to change the hostname from interface



```
sudo virt-sysprep -d <name> --enable tmp-files,logfiles,dhcp-client-state,crash-data,udev-persistent-net,net-hostname --hostname openlever
```

Unfortunately, I could not get virt-sysprep to work to change the hostname

__You probably want to add your new VM to your hosts /etc/hosts file so it is known to your development machine__

Really we could create a brand new one everytime or clone an existing one... if we can easily create a new one there is less reason to clone and easier to stay up to date.

So we could create a new one and use virt-sysprep to run a new program

```
sudo sed -i -e 's/dockerhost/openlever/' /etc/hostname 
```
* change /etc/hostname
* update /etc/network/interfaces dhcp hostname
* reboot
* set /etc/hosts with new dhcp


### Remove Virtual Machine and associated storage

```
virsh undefine <name> --remove-all-storage
```


## Alpine Docker Host Setup
### Install additional Basic Tools
````
apk add make
apk add vim
apk add sudo
apk add pv
````

### Sudo access to sudo group

Configure users in sudo group access to all commands by editing sudoers file: 
```
visudo
```

### Create Development User
Create a default development user and add to sudo group
```
adduser <username>
addgroup sudo
adduser <username> sudo
```

### Add DNS Entry to Host System
Edit /etc/hosts with ip address and name as appropriate

``` 
192.168.122.228	<hostname>
```

### Setup ssh key authentication to new user
Connect over ssh to ensure it is working. Use ifconfig to get the address of the vm! Copy public keys to new machine using openssh command ssh-copy-id for simplicity.
```
ssh-copy-id <username>@<hostname>
```

and now ssh using hostname without a password

```
ssh <username>@<hostname>
```

### Docker Install
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

# Shared Folder Guest to Host
For hosting with KVM is it easier to map the directory into the guest and will hot module reload and everything else work as expected... or to have the guest share files to the host as is required in the windows environment?

## Sharing Guest to Host over sshfs 
On the guest create a working directory
```
sudo mkdir /working
sudo chown <username>:<username> /working
```

On the host try and connect with sshfs
```
sudo apt install sshfs
mkdir <solution>
sshfs -o idmap=user,IdentityFile=~/.ssh/id_rsa moon@<hostname>:/working <solution>
```

Unmount
```
fusermount -u <solution>
```

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