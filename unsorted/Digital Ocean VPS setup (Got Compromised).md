
Digital Ocean: Virtual Private Server:
-----------------------------------------------------------------------------
On the Onware Account

Created a Droplet: Marketplace Docker 5:19.03.1~3 on Ubuntu 18.04:
	* Docker CE 	18.06.1 	Apache 2
	* Docker Compose 	1.17.1 	Apache 2


$10 /month 2GB/1CPU 50 GB SSD 2TB Transfer 
in Toronto
added SSH key for EliteBook VM
hostname: orms-docker

--> Create Droplet


Additional Configuration:
-----------------------------------------------------------------------------
Connect using ssh account with ssh key: root@138.197.134.33

Firewall:

Droplet comes with Docker Daemon ports open. That's not great if we don't need it.

%> ufw delete allow 2375
%> ufw delete allow 2376
%> ufw enable


Will Likely need to open up 443 and maybe 80 as we get going?

%> ufw allow https
%> ufw enable

Make sure the default policy is to deny everything and check over the configuration:

%> ufw default deny incoming
%> ufw status verbose

Make is NOT installed.

%> apt install make

Git is already installed. 

Generate an ssh key for use with github:

%> ssh-keygen -t rsa -b 4096 -C "root@orms-docker"

Add ssh key to github repository:

%> git clone git@github.com:iAlexanderMoon/olywestrms.git
%> cd olywestrms
%> make dependencies
%> make production-images
%> docke

docker 

# Add dns entry to onware.org


Identity Server already in onware.
Add Clients to obs-ids.onware.org


Looks like the verbose from ufw says :

Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), allow (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere                  
22/tcp (v6)                ALLOW IN    Anywhere (v6)   

So how is the HTTP and HTTPS traffic getting through? Docker changes iptables. 

Docker actually bypasses UFW and directly alters iptables, such that a container can bind to a port. This means all those UFW rules you have set won't apply to Docker containers.
Reference: https://www.techrepublic.com/article/how-to-fix-the-docker-and-ufw-security-flaw/

Is it a flaw or a feature it changes the IPTABLES as per the docker services requiring external ports: 
%> iptables -S | grep DOCKER-N DOCKER

-N DOCKER-ISOLATION-STAGE-1
-N DOCKER-ISOLATION-STAGE-2
-N DOCKER-USER
-A FORWARD -j DOCKER-USER
-A FORWARD -j DOCKER-ISOLATION-STAGE-1
-A FORWARD -o br-9e1455b8ffa7 -j DOCKER
-A FORWARD -o br-47a01bd01ab3 -j DOCKER
-A FORWARD -o br-a3d2b7d98348 -j DOCKER
-A FORWARD -o br-4e26840a93c4 -j DOCKER
-A FORWARD -o docker0 -j DOCKER
-A DOCKER -d 192.168.48.2/32 ! -i br-9e1455b8ffa7 -o br-9e1455b8ffa7 -p tcp -m tcp --dport 5432 -j ACCEPT
-A DOCKER -d 192.168.32.2/32 ! -i br-47a01bd01ab3 -o br-47a01bd01ab3 -p tcp -m tcp --dport 443 -j ACCEPT
-A DOCKER -d 192.168.32.2/32 ! -i br-47a01bd01ab3 -o br-47a01bd01ab3 -p tcp -m tcp --dport 80 -j ACCEPT
-A DOCKER-ISOLATION-STAGE-1 -i br-9e1455b8ffa7 ! -o br-9e1455b8ffa7 -j DOCKER-ISOLATION-STAGE-2
-A DOCKER-ISOLATION-STAGE-1 -i br-47a01bd01ab3 ! -o br-47a01bd01ab3 -j DOCKER-ISOLATION-STAGE-2
-A DOCKER-ISOLATION-STAGE-1 -i br-a3d2b7d98348 ! -o br-a3d2b7d98348 -j DOCKER-ISOLATION-STAGE-2
-A DOCKER-ISOLATION-STAGE-1 -i br-4e26840a93c4 ! -o br-4e26840a93c4 -j DOCKER-ISOLATION-STAGE-2
-A DOCKER-ISOLATION-STAGE-1 -i docker0 ! -o docker0 -j DOCKER-ISOLATION-STAGE-2
-A DOCKER-ISOLATION-STAGE-1 -j RETURN
-A DOCKER-ISOLATION-STAGE-2 -o br-9e1455b8ffa7 -j DROP
-A DOCKER-ISOLATION-STAGE-2 -o br-47a01bd01ab3 -j DROP
-A DOCKER-ISOLATION-STAGE-2 -o br-a3d2b7d98348 -j DROP
-A DOCKER-ISOLATION-STAGE-2 -o br-4e26840a93c4 -j DROP
-A DOCKER-ISOLATION-STAGE-2 -o docker0 -j DROP
-A DOCKER-ISOLATION-STAGE-2 -j RETURN
-A DOCKER-USER -j RETURN










