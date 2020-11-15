Windows 7 Environment... too combersome... going to linux VM 

VM Specs:
* GB Ram
20 GB VHD
Graphics Controller - VBoxSVGA

# Install Xubuntu 19.04
* Don't install updates during install... slows it down... do it after install

# Get current updates
apt update
apt upgrade

# Install VirtualBox Guest Additions Packages
apt install virtualbox-guest-utils virtualbox-guest-x11 virtualbox-guest-dkms

# VirtualBox
* Enable Bidrectional Clipboard Sharing. It Works!
* Enable Bidrectional Drag and Drop.
	* Copy a File from Xfce4 File Manager to Windows 7 File Manager. WORKS!
	* Copy a Folder from Xfce4 File Manager to Windows 7 File Manager. WORKS!
	* Copy a File from Xfce4 File Manager to Windows 7 File Manager. DOES NOT WORK!
	* Copy a Folder from Xfce4 File Manager to Windows 7 File Manager. DOES NOT WORK!
* Hide Menu Bar
	* Access it by hitting RIGHT CONTROL + Home
* Hide Status Bar
	* Can be renabled through the Menu Bar

	
# Corporate Certificate Authority: 
# available in the git procurement solution or can be exported from Windows
# 
sudo cp olymel.sec.crt /usr/local/share/ca-certificates/	
sudo update-ca-certificates
sudo reboot

## Install Docker & Docker Compose
```
apt install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
apt-key fingerprint 0EBFCD88
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
apt update
apt install docker-ce docker-ce-cli containerd.io
```

## SSH Server
sudo apt update
sudo apt install openssh-server

## Required Tools included in the Xubuntu base install
* vim
* make 

## Automated Install Additional Tools
```
apt install git
snap install --classic code
```

## Manual Install Azure Data Studio
Download the zipped tarball, extract, place, and link to the bin/
``` 
tar -xvf azuredatastudio.version.tar.gz 
sudo mv azuredatastudio.version /usr/local/lib/
sudo ln -s /usr/local/lib/azuredatastudio/bin/azuredatastudio /usr/local/bin/
```

How to add an icon and menu item in Xfce4?
How to add to Xfce4 Launcher
How to add Icon

' How hard to make a snap?... the vscode one works!



# Additional Configuration

## Generate SSH Keys
```
	 ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Add the SSH key to the git server as required.


## Docker Configuration 


### Add User to the Docker Group
```
sudo usermod -aG docker $USER
```

REBOOT Required

To verify docker is working as user. After remove the image
```
docker run hello-world
docker rmi --force hello-world
```


### Start Docker on boot
```
sudo systemctl enable docker
```

### Working Folder
Recommend working in a working folder... in your user directory
```
mkdir ~/working
```

Each project/solution can then be checked out into the working directory as required.




# Exposing http & https to the host desktop.

Windows 7. Xubuntu 19.04 Host.

## Setup additional static network adapter and network 
Unlike alpine and older versions of ubuntu... /etc/network/interfaces is not used for adding interfaces


/etc/netplan/01-network-managner-all.yaml

sets the renderer as NetworkMananger!

Manually configured a 2nd ethernet adapter through the xfce4 network gui:





Assuming the Windows Host network configuration

```
address 192.168.36.5
netmask 255.255.255.0
gateway 192.168.36.2
```

and 

```
address 192.168.36.6
netmask 255.255.255.0
gateway 192.168.36.2
```

Now services can be bound to different Ip's in docker compose.... thus each project can expose it's own ports and stay isolated!
So each isolated project SHOULD run it's own infrastructure: proxy, identity server, log viewer etc.

For Integrated Staging and Production areas a different deploy scripts should be created which pulls ONLY from docker hub or other registry.
These configurations may use different infrastructure pieces which are shared across multiple projects.

Solutions should NOT have to communicate between them... although... you may have to use a docker image that is created from another solution if it's not retrievable from the docker hub.


## Connecting to services:
* 192.168.36.5:9000	Portainer
* 192.168.36.5:9001 Traefik

### Configure Hosts Files

Configure the windows or linus host file entries with DNS names:

C:\Windows\System32\Drivers\etc\hosts
```
192.168.36.2	original
192.168.36.3	build
192.168.36.4	production
192.168.36.5	development
192.168.36.6	devorms
192.168.36.7	devopro
```

/etc/hosts
```
192.168.36.5	devoshared
192.168.36.6	devorms
192.168.36.7	devopro
192.168.36.8	devobs
```



Projects:

/home/moon/working/olywestids-test
 * an inmemory only instance of identity server for development purposes
 
/home/moon/working/olywestprocurement
 * crm etc.

 /home/moon/working/olywestriskmanagement
 * ui and api for risk management
 
 
1 Repository with many solutions... or many repositories?
Many repositories:

iAlexanderMoon / procurement
iAlexanderMoon / test-ids
iAlexanderMoon / riskmanagement
iAlexanderMoon / ids
# Install Docker

snap install azuredatastudio
sdfs


Xubuntu

# tilix terminal emulator
mousepad text editor... could it just autosave I'd like notepad++
what other editors are already available that are fast and do more and available packaged in ubuntu
I'm not a big fan of my notes being integrated into by desktop... mostly because I want them on all my devices.
Working notes I'd prefer in a seperate application ala notepad++

xubuntu mailing list
xubuntu chat channel




