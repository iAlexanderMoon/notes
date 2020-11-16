# Docker Development - Dotnet core runner debug vscode

## Abstract

In the Dockerfile for the dotnet runner the following lines were included:

```dockerfile
## Install VSCODE .net core debugger
RUN curl -sSL https://aka.ms/getvsdbgsh | bash /dev/stdin -v latest -l /vsdbg-sos install
```

## VSCode: Attach new window to remote container and debug

### launch.json

Uses the attach to local process definition in .vscode/launch.json

```json
        {
            "name": ".NET Core Attach",
            "type": "coreclr",
            "request": "attach",
            "processId": "${command:pickProcess}"
        },
```

### Attach to running container

Using command pallet {ctrl-shift-p}: Remote-Containers: Attach to Running Container...

Choose the desired running container to attach.

This will create a new vscode window attached to the specific container (not the devcontainer).

### Debug

From the debugger, start the ".Net Core Attach" debugger.

* Debugger interface {ctrl-shift-d}
* Command pallet {ctrl-shift-p}: Debug: Select and Start Debugging.
* {F5} will start last used debugger as long as it was ".Net Core Attach"
 
Select the process with the project dll in the name, the list can be long. Use the type ahead filter to find it easily.

___Note: don't be fooled into selecting the dotnet watch or dotnet run processes. You want the actual program process named like your project.___


