# Vscode Container based development

https://github.com/microsoft/vscode-dev-containers
https://github.com/microsoft/vscode-dev-containers/tree/master/containers

## .devcontainer

## Sidecar development Container.

### .devcontainer .devcontainer.json

Containers can be defined withint a .devcontainer folder. I prefer defining an image and building it seperately for use and using .devcontainer.json.

Notes:
* define your image
*  appPort must be unique if you want to run two vscode windows at once.
*  define and bind the host name in devarms
*  map a nuget_packages environment variable
*  

```json
{
	"image": "rms/dev:1.0",
	"appPort": 8093,
	"extensions": [
		"ms-vscode.csharp",
		"ms-dotnettools.csharp",
		"ms-azuretools.vscode-docker",
		"docsmsft.docs-yaml",
		"octref.vetur",
		"tobermory.es6-string-html"
	],
	"runArgs": [ "-u", "vscode", "--name", "rmsdev", "--add-host=devorms:192.168.36.7" ],
	"mounts": [ "source=/var/run/docker.sock,target=/var/run/docker.sock,type=bind" ],
	"settings": { 
		"terminal.integrated.shell.linux": "/bin/bash",
		"remote.extensionKind": {
			"ms-azuretools.vscode-docker": "workspace",
			"ms-vscode.csharp": "workspace"			
		},
		"files.watcherExclude": {
			"**/.git/objects/**": true,
			"**/.git/subtree-cache/**": true,
			"**/node_modules/*/**": true
		}
	},
	"containerEnv": {
		"NUGET_PACKAGES": "/workspaces/olywestrms/data/nuget",
		"TESTPATH": "${localWorkspaceFolder}",
		"DOTNET_USE_POLLING_FILE_WATCHER": "false",
		"DOTNET_CLI_TELEMETRY_OPTOUT": "true"
	}
}

```


## Runners

Dotnet
Node Vue
Node Angular