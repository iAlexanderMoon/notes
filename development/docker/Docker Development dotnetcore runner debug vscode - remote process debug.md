# Docker Development - Dotnet core runner debug vscode

## Abstract

In the Dockerfile for the dotnet runner the following lines were included:

```dockerfile
## Install VSCODE .net core debugger
RUN curl -sSL https://aka.ms/getvsdbgsh | bash /dev/stdin -v latest -l /vsdbg-sos install
```

## VSCode: Attach to remote debugger remoteProcess

### launch.json

Uses the attach to local process definition in .vscode/launch.json

```json
{
    "name": ".NET Core Attach dotnet_watch_docker_example",
    "type": "coreclr",
    "request": "attach",
    "processId": "${command:pickRemoteProcess}",
    "sourceFileMap": {
        "/app": "${workspaceRoot}/"
    },
    "pipeTransport": {
        "pipeCwd": "${workspaceRoot}",
        "pipeProgram": "docker",
        "pipeArgs": [
            "exec",
            "-i",
            "dotnet_watch_docker_example" <<-- SUBSTITUTE YOUR CONTAINER NAME HERE
        ],
        "quoteArgs": false,
        "debuggerPath": "/vsdbg/vsdbg"
    }
}
```

### DevContainer Dockerfile Definition

* Install Docker
* Create container user and add to the docker group
* Optionally install Docker Compose if you want to use it inside the container


#### Permission Denied in dev container to docker.sock 
Even though we added the user to the docker group, it still received permission denied. 

chmod 666 /var/run/docker.sock resolved the issue... but why?

Because the docker group id on the host system did NOT match the docker group id on dev container build. so mapping is useless...

On host

```sh
cat /etc/group | grep docker
docker:x:133:vscode
```

MUST map the docker group gid to 133.

__ARGS can be sent to the make devimage target when building the devcontainer in the makefile!__

* id -u
* id -g
* getent group docker | awk -F: '{printf "%d", $3}'

__Lots of times the remote isn't found... restart the watcher and try the picker again.__

__Break points won't take__
If intellisense isn't working... debugger will be disabled. Restart omnisharp:

* You may need to dotnet restore
* command pallet: Omnisharp Restart Omnisharp

__Can't find src code__
Remote code can't find source... IT MUST BE ON THE SAME PATH for both the devcontainer and the runner container?
Set this in the .devcontainer.json by mapping it instead of using the default:

Example (default is /workspace):

```json
{
    ...
	"workspaceMount": "source=${localWorkspaceFolder},target=/working,type=bind,consistency=delegated",
	"workspaceFolder": "/working",
    ...
}
```

Or adapt docker-compose to match the vscode "/workspace/{project}" convention

__Service not found...__
Are the services composed up? make up

### Attach to process from using remote

__This doesn't work from a vscode folder view (unless you installed the dotnet sdk on your workstation... but since this is about developing without installing local SDK and ONLY using docker...__

* Docker cli must be installed in the development container to use this connection directly.
* In your project launch.json you can create multiple launch rules if you have multiple dotnet containers to attach or other project types.
