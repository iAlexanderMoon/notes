apk add samba

# set workgroup and server string
	workgroup = ALPINEDOCKER
	server string = Alpine Samba Server

# Create a work folder  /work 
mkdir /work

# Add work share export
/etc/samba/smb.conf

[working]
	path = /working
	public = yes
	only guest = yes
	writable = yes
	printable = no

# test samba parameters
testparm

# test local share
smbclient -L 192.168.36.2

# add root as a user
smbpasswd -a root

# start/top/restart
/etc/init.d/samba restart


# We could also add a host record for our guest OS.
# that way we can use the host browser
# C:\Windows\System32\Drivers\etc\hosts
192.168.36.2	dockerdev

# Access the shares
\\192.168.36.2
\\dockerdev

# Map the share to windows
\\192.168.36.2\working
\\dockerdev


# didn't have permission to access as root?

https://superuser.com/questions/258026/using-samba-to-share-a-folder-from-a-linux-guest-with-a-windows-host-in-virtualb
https://askubuntu.com/questions/281466/samba-how-can-i-access-a-share-on-a-virtualbox-guest-in-nat-mode


# Visual Studio Code integrated terminal to Git-Bash cygwin terminal
# setting: terminal.integrated.shell.
C:\Windows\System32\cmd.exe to C:\Portable\GitPortable\git-bash.exe

# bash needs to be able to ssh to the alpine instance to work with the command line
# by default root can't ssh in enable it in /etc/ssh/sshd_config
PermitRootLogin Yes
# This will allow interactive access to root (would be better to use keys only)

# Login to Host-Only interface
ssh root@192.168.36.2

# use ssh keys only
# generate keys on local user (git-bash) don't use a password
ssh-keygen
scp .ssh/id_rsa.pub root@192.168.36.2:.
ssh root@192.168.36.2
> cat id_rsa.pub .ssh/authorized_keys

# now you should be able to ssh to root@192.168.36.2 without a password on the windows client
# change sshd settings so SSH ROOT ONLY access via keys in /etc/ssh/sshd_config
PermitRootLogin prohibit-password



## 
## Well VSCODE intellisence doesn't work in the docker environment unless you install dotnet core
## My Guess is intellisence doesn't work in the docker environment unless you install as well?
