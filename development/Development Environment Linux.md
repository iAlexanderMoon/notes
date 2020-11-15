# Linux Development Environment Setup (deb based)

## Basic Tool Install
```
sudo apt install vim
sudo apt install git
```

### Install Additional Tools
**APT**
````
sudo apt install chromium-browser
sudo apt install firefox
sudo apt install thunderbird
````

**Snaps seen slower**
````
sudo apt install snapd
sudo snap install chromium-browser
sudo snap install firefox
sudo snap install thunderbird
sudo snap install --classic vscode
````

### Snaps for the following tools are possible but seem slower:

#### Install vscode
https://code.visualstudio.com/docs/setup/linux

**Additional VSCode Plugins**
* markdownlint
* C# - C# for Visual Studio Code
* Vetur - VueJs
* sql-database extension

Most vscode postgres tools require psql to be installed on the dev machine.
This would be instead of using psql in a docker container to connect to the database which may be better...
However to WRITE sql files this might be better.

## Install dotnet core 2
````
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
sudo sh -c 'echo "deb[arch = amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-xenial-prod xenial main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt update
sudo apt install dotnet-sdk-2.1.4
````

#
# Install NodeJs 8

````
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt install -y nodejs
````

Make sure nodejs is version 8

````
nodejs -v 
````


### Install NPM
```
sudo apt install npm
```


### VueJs
```
sudo npm install -g vue-cli
```


## Install Docker CE

In elementary os we substituted xenial for the distro as it uses 16.04 Xenial as it's base
```
sudo apt-get remove docker docker-engine docker.io
sudo apt install apt-transport-https
sudo apt install ca-certificates
sudo apt install curl
sudo apt install software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable"
sudo apt update
sudo apt install docker-ce
```
Allow user to interact with docker without priviledge. Log out and log back in so that your group membership is re-evaluated. Maybe restart the system.
```
sudo groupadd docker
sudo usermod -aG docker $USER
```