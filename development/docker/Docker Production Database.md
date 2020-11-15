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
* persist files to volume on host
* the password must be 8 chars and include upper, lower and a digit or symbol
* run daemonized... -it will only show logs at startup anyway
* connect into running container
* use SQLCMD directly in running container
* should be able to connect from SQL Server Management Studio etc.
  
```
docker run --name dev_sql --rm -d \
	-p 1433:1433 \
	-v /var/opt/mssql:/var/opt/mssql \
	-e  ACCEPT_EULA=Y \
	-e  SA_PASSWORD=Olymel20192019 \
	-e  MSSQL_PID=Express \
	mcr.microsoft.com/mssql/server:2017-CU12-ubuntu

	docker exec -it dev_sql /bin/bash
	docker exec -it dev_sql /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P Olymel20192019 
```

_Convenience Makefile is created and _

### Restore
* need to get the backup to the host
* can simply do this over scp instead of setting up some kind of sharing?

### Backup
* initiate the backup and get it to a share that is backed up
* do it regularily


# TODO:
Azure Data Studio and Management Studio Both provide a file chooser from the machine running the application and NOT what is on the server... 
Are we supposed to remote or do something different?