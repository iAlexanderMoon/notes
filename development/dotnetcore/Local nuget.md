
# List all nuget packages
dotnet nuget locals all -l

info : http-cache: /home/vscode/.local/share/NuGet/v3-cache
info : global-packages: /home/vscode/.nuget/packages/
info : temp: /tmp/NuGetScratch
info : plugins-cache: /home/vscode/.local/share/NuGet/plugins-cache


# Make a nugetconfiguration in the root folder of the solution... hopefully it is used in all the childern project below it
dotnet new nugetconfig


To persist the nuget cache we need to map a folder in... or just use in package it think.


    <config>
        <add key="repositoryPath" value="data/nuget" />
    </config>
	
	
For the development message... I think we should try a dev image that puts a global-packages instead of by user.
Can this be done by specifying in the global

Global Nuget configuration: NuGetDefaults.Config

Solution, User, Computer: /usr/local/share

Docker Compose is included in the DevContainer... so we could have the Makefile run it or we can simply control the solution through the cli
VSCODE can likely ALSO use the docker compose 

Set the environment variable NUGET

NUGET_PACKAGES 	Path to use for the global-packages folder as described on Managing the global packages and cache folders.
https://docs.microsoft.com/en-us/nuget/reference/cli-reference/cli-ref-environment-variables