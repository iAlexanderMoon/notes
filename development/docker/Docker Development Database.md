# Database

## SQL Server
* MS SQL Server 2017 is available for linux.
* Can use SQL Server Management Tools from Windows Host.
* Export our data files 
* 2GB of RAM as of 2017-CU2 (Assign enough RAM to Docker Host VM... maybe 4GB for the machine)
* expose the default port 1433 to the development machine via a mapped port: 91433:1433 
	* in case you have a local server bound to that port
	* in case you want multiple database servers running (test, dev etc)
* make sure only 1 instance in swarm
* persist data by using a volume -v: 
   * _host directory_:/var/opt/mssql
* this will only work on a single machine!
  
Pull the current cumulative update from docker:
```
docker pull mcr.microsoft.com/mssql/server:2017-CU12-ubuntu
```

The image has the following settings:
* ACCEPT_EULA=Y
* SA_PASSWORD=_your_strong_password_
* MSSQL_PID=Express

### Run Manually
* use express edition licensed for fee use
* persist files locally
* the password must be 8 chars and include upper, lower and a digit or symbol
* run daemonized... -it will only show logs at startup anyway

```
docker run --name dev_sql --rm -d \
	-p 1433:1433 \
	-v /var/opt/mssql:/var/opt/mssql \
	-e  ACCEPT_EULA=Y \
	-e  SA_PASSWORD=Olymel20192019 \
	-e  MSSQL_PID=Express \
	mcr.microsoft.com/mssql/server:2017-CU12-ubuntu
```

### Connect to SQL
* use sqlcmd inside running container from bash
* Use sqlcmd on running container
```
docker exec -it dev_sql /bin/bash
docker exec -it dev_sql /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P Olymel20192019 
```
Commands:
```
:HELP
SELECT NAME FROM sys.sysdatabases;
GO
:EXIT
```

For a list of commands:
* https://docs.microsoft.com/en-us/sql/tools/sqlcmd-utility?view=sql-server-2017

#### Notes
* See the sql/Makefile for shortcuts to usefull commands
* With the -p mapped connect to the running instance using Azure Data Studio or SQL Server Management Studio

With the port mapped you should

	

	* connect using sqlcmd in running database container:
		* 

	* sql server management studio 2014 (2014 for windows 7)
	* Azure Data Studio (formerly SQL Server Ops Studio) installed portable on windows


	* sql tools can we use dacpacs and bacpacs but not schema modification at this point.


### MS SQL Tools Image
is this somehow different than the tools from the running container...
probably much smaller if you want to script a process by kicking up a temporary container
```
docker pull mcr.microsoft.com/mssql-tools
```
May want to script a backup of the data


### MS SQL Package
* https://docs.microsoft.com/en-us/sql/tools/sqlpackage?view=sql-server-2017
* https://docs.microsoft.com/en-us/sql/tools/sqlpackage-download?view=sql-server-2017


Forked from Githubdave-hillier/docker-sqlpackage-ubuntu but there is a docker hub image tied to it
* https://github.com/guestlinelabs/docker-sqlpackage-ubuntu
* https://github.com/dave-hillier/docker-sqlpackage-ubuntu/blob/master/Dockerfile
* https://hub.docker.com/r/guestline/sqlpackage

```
	docker pull guestline/sqlpackage
```

Run the sqlpackage command directly
```
	docker run --name sqlpackage --rm -it guestline/sqlpackage
```


Alternatively we could build our image and push it to production as well as the Dockerfile is NOT that complicated...


### 3rd part cli to try:
It's in python???
No current docker image for it
https://github.com/dbcli/mssql-cli

Microsoft has a preview:
* https://github.com/Microsoft/mssql-docker/blob/master/linux/preview/examples/mssql-cli/Dockerfile


### Azure Data Studio SSDT Experience
This is still being worked on where you can create a bacpac or dacpac from the sql files on disk instead of a database!
Or create script files on disk for commit to git
* https://github.com/Microsoft/azuredatastudio/issues/389