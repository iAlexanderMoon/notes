# Docker Alpine Host with Windows 7 Desktop

## Git Bash Terminal
It seems the best bash terminal in Windows 7 is git-bash of which a portable version can be installed. 

_Windows 10 has a better command line experience or the Linux SubSystem which I haven't had occasion to try._

### SSH Keys
By setting ssh keys into our alpine host we won't have to put in a password anymore. Make sure you can already ssh to the alpine host.

In git-bash generate ssh keys (no passphrase)
```
ssh-keygen
```

* Copy public key to host
* ssh to host and concatenate the public key to the list of authorized keys
```
scp ~/.ssh/id_rsa.pub root@192.168.36.2:.
ssh user@192.168.36.2
> cat id_rsa.pub .ssh/authorized_keys
> rm id_rsa.pub
```

_now you should be able to _ssh to user@192.168.36.2 without a password with the git bash ssh client_

## Visual Studio Code
VS Code is available as a portable app.

### Configure Integrated Terminal to git-bash
Visual Studio Code integrated terminal to Git-Bash cygwin terminal change

* setting: terminal.integrated.shell. 
* C:\Windows\System32\cmd.exe to C:\Portable\GitPortable\git-bash.exe

## SQL Server Management Studio
* only SQL Server 2014 including management studio can be installed on Windows 7

## Azure Data Studio (formerly SQL Ops Studio)
Available as a Portable Application

# TODO:
* VSCODE intellisence doesn't work in the docker environment 
  * MUST you install dotnet core?
  * Can you remote into docker container over ssh
  * Can you remote into alpine host and connect to docker host?

