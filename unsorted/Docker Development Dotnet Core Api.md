README: Developing DotNet Core Api with Docker
* interactive

# execute the docker image manually for watching and recompiling c# webservice

# Run from the command line with vueyarn image interactively
# docker run
#  -p map port
#  -d detached
#  --rm remove container when stopped
#  --name assign a name to container
#  -v shared volume
#  -w working directory
#

docker run --name profile_api --rm -it -v /working/profile/profile.api:/app -w /app -p 8081:8081 microsoft/dotnet:2.1-sdk-alpine /bin/sh

# try the commands manually
#
> dotnet restore
> dotnet build
> dotnet watch run

# Okay default startup puts us on ports 5000 and 5001
# but once again it isn't picking up the request!
# I have no use for running the SSL layer between my proxy and the api so let's disable that and change the port
# dotnet core 2.1 removed default command line parsing... that's where I like to set it... because then it is easy to set in the stack?
# anyway had to read the command args:
# Create a configuration by reading the CommandLineArgs in the Program.cs main
# Pass them into the Build and use them there


            var configuration = new ConfigurationBuilder()
                .AddCommandLine(args)
                .Build();

            BuildWebHost(args, configuration).Run();
			
			
			public static IWebHost BuildWebHost(string[] args, IConfiguration configuration) =>
				WebHost.CreateDefaultBuilder(args)
					.UseConfiguration(configuration)
					...


# now the browser can access the running api or through the running watcher
#
docker run --name profile_api --rm -it -v /working/profile/profile.api:/app -w /app -p 8081:8081 microsoft/dotnet:2.1-sdk-alpine /bin/sh
> dotnet run --urls http://0.0.0.0:8081
> dotnet watch run --urls http://0.0.0.0:8081

# As per https://natemcmaster.com/blog/2018/05/12/dotnet-watch-2.1/ 
# adding a Directory.Build.props file for the watcher hosted in Docker
	<Project>

	  <PropertyGroup>
		<DefaultItemExcludes>$(DefaultItemExcludes);$(MSBuildProjectDirectory)/obj/**/*</DefaultItemExcludes>
		<DefaultItemExcludes>$(DefaultItemExcludes);$(MSBuildProjectDirectory)/bin/**/*</DefaultItemExcludes>
	  </PropertyGroup>

	  <PropertyGroup Condition="'$(DOTNET_RUNNING_IN_CONTAINER)' == 'true'">
		<BaseIntermediateOutputPath>$(MSBuildProjectDirectory)/obj/container/</BaseIntermediateOutputPath>
		<BaseOutputPath>$(MSBuildProjectDirectory)/bin/container/</BaseOutputPath>
	  </PropertyGroup>

	  <PropertyGroup Condition="'$(DOTNET_RUNNING_IN_CONTAINER)' != 'true'">
		<BaseIntermediateOutputPath>$(MSBuildProjectDirectory)/obj/local/</BaseIntermediateOutputPath>
		<BaseOutputPath>$(MSBuildProjectDirectory)/bin/local/</BaseOutputPath>
	  </PropertyGroup>

	</Project>

!!! dotnet watch Polling File Watcher works OVER SMB !!!
Should be able to edit through VS Code on the Windows Host and the service will auto rebuild

# Polling File Watcher came on by default... does it work without Polling in the current setup?
> export DOTNET_USE_POLLING_FILE_WATCHER 0
> dotnet watch run --urls http://0.0.0.0:8081

!!! dotnet watch INOTIFY works OVER SMB !!!

# Run daemonized & connect to output through logs when necessary
#
docker run --name profile_api --rm -d -v /working/profile/profile.api:/app -w /app -p 8081:8081 microsoft/dotnet:2.1-sdk-alpine dotnet watch run --urls http://0.0.0.0:8081
docker logs -f profile_api

# Run in a docker stack
# devstack.yml:

	services:
	  # docker run --name profile_api --rm -d -v /working/profile/profile.api:/app -w /app -p 8081:8081 microsoft/dotnet:2.1-sdk-alpine dotnet watch run --urls http://0.0.0.0:8081
	  profile_api:
		image: microsoft/dotnet:2.1-sdk-alpine
		ports:
		  - "8081:8081"
		networks:
		  - front-net
		volumes:
		  - /working/profile/profile.api:/app
		working_dir: /app
		environment:
		  - DOTNET_USE_POLLING_FILE_WATCHER 0
		command: dotnet watch run --urls http://0.0.0.0:8081


TODO:

# DEBUG FROM VSCODE... need to get here!
#
# and https://www.reddit.com/r/csharp/comments/9cyl6a/debugging_dotnet_watch_processes_inside_of_a/


# Should look into starting unit tests that automagically run and issue on failures!
# test could probably be on another or part of dev stack?
> dotnet watch test

# Is there a way to look at configuration for the service from docker instead of a file? or command line args and update those when the options change???


	