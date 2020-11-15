 16425 ms  RestoreTask                                1 calls
 
 How to make dotnet restore faster in a docker container...
* We want a clean bin and obj... but we can provide a faster method than downloading the nuget packages

https://docs.microsoft.com/en-us/nuget/consume-packages/managing-the-global-packages-and-cache-folders

%> dotnet new nugetconfig


Using docker compose set the environment variable for the global reference package:

    environment:
      - NUGET_PACKAGES=${WORKING}/data/nuget
	  
Before:

    15613 ms  MsBuild                                   17 calls
    24865 ms  RestoreTask                                1 calls
	
After:

     4397 ms  RestoreTask                                1 calls
    18813 ms  MsBuild                                   17 calls


Access to the path '/workspaces/olywestbs/data/nuget/system.threading.tasks.extensions/4.3.0' is denied. [/workspaces/olywestids/src/ids/Olymel.IdentityServer.csproj]

Make sure:
	NUGET_PACKAGE environment variable PATH CORRECTLY in 
		.devcontainer (containerEnv)
		deploy/developmnet/compose
		Makefile (cli rule)