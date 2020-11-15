## Command line

## VSCode



With vsdbg now in our docker-container, we now have to modify our launch.json in our .vscode folder. Add a configuration such as below:

{
    "name": ".NET Core Attach Docker",
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
            "dotnet_watch_docker_example"
        ],
        "quoteArgs": false,
        "debuggerPath": "/vsdbg/vsdbg"
    }
}
