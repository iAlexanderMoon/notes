# README: Developing with Docker

Create a single swarm has already been covered in the host setup.

## Stack
### Compose File
Use a stack compose file to layout the services in the stack.

### Makefile
Using a makefile to simplify the process of working with docker stack commands and builds.

Add rules to Makefile to start and stop the stack.

```
start-dev:
	docker stack deploy --prune -c deploy/development/local.yml -c deploy/development/tools.yml dev

stop-dev:
	docker stack rm dev
```

From the command line to start and stop the stack:
```
make start-dev
make stop-dev
```

To start and stop a single service on the dev stack.. for example the price_ui service:
```
docker service update --force dev_price_ui
```

Changes to stack compose file ARE NOT read or used to update a single service. 

Whenever changes or restarts are required you can issue an update on the entire stack instead of taking the stack all down:
```
make start-dev
```

### Notes
For simplicity it is BEST for development if the external docker port is mapped to the same internal port. For dotnet you can specify the port as part of the command in the compose file. Unfortunately, for npm serve the command argument is ignored or does NOT function (hot module reloading will not function) and so editing of the source file for serving is required. See additional documentation for developing with that tool.

#### MOVEABLE WORKING DIRECTORY
* in Makefile before docker commands set a working folder path
```
WORKING=$(shell pwd)

test:
	WORKING=${WORKING} docker stack ...
```

Now inside your docker stack files you can use ${WORKING} to pass the working directory for volume mounts!
