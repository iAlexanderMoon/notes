``# README: Developing DotNet Core WebApi with Docker

## Custom .net core SDK Image
To run .net core commands in a docker development environment without requiring it on the host... then we need to run 


## Dotnet Watch
The custom .net core SDK Image can be used to run the dotnet web api in watch mode such that changes to the source files are recompiled. In development mode we can set this up to run in the compose file.

The following example uses the default SDK image, watch with polling turned OFF, and proxied. The application is volume mapped from to the app folder in the container:

```
  profile_api:
    image: microsoft/dotnet:2.1-sdk-alpine
    networks:
      - traefik
    ports:
      - "8011:8011"
    volumes:
      - /working/apps/profile/profile.api:/app
    working_dir: /app
    environment:
      - DOTNET_USE_POLLING_FILE_WATCHER 0
    command: dotnet watch run --urls http://0.0.0.0:8011
    deploy:
      labels:
        - "traefik.enable=true"
        - "traefik.docker.network=dev_traefik"
        - "traefik.port=8011"
        - "traefik.frontend.priority=20"
        - "traefik.frontend.passHostHeader=true"
        - "traefik.frontend.rule=Host:dockerhost;PathPrefix:/profile/api,/profile/swagger"
        - "traefik.backend.loadbalancer.method=wrr"
```

## Dotnet Watch Test
Using dotnet watch test for continuous testing with .NET Core and XUnit.net of libraries etc. 

You can setup additional services in your local development compose file for automatically running tests. It works the same way but the entry point command is:

```
		command: dotnet watch test
```

The results of tests are output to standard out and so are visible to docker in portainer.

__It would be very cool to have a tool simpler from portainer just for development where it could then watch the results for errors__

## Dotnet Build Release
A Dockerfile with Dual Stage build should be included within the core application. This file specifies HOW to build the Docker Image artifact for deployment.

Dockerfile.release
````
FROM microsoft/dotnet:2.1-sdk-alpine3.7 AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

FROM microsoft/dotnet:2.1.6-aspnetcore-runtime-alpine3.7
WORKDIR /app
COPY --from=build-env /app/out .

#ENV ASPNETCORE_URLS=http://*:80
ENTRYPOINT ["dotnet", "Olymel.Profile.Api.dll"]
````

## Development Considerations

### Get inside a running services
The SDK image comes with /bin/sh inside the container and so you can exec into the running container to issue commands and look about if necessary.

```
docker exec -i -t <container_name> /bin/bash
```

FYI: Portainer will let you do this as well.

### Using an SDK to create a new api:

Run bash into a container and issue commands for creating the application
```
docker run --rm -it -v /working/app:/app -w /app microsoft/dotnet:2.1-sdk-alpine /bin/sh
```

Create whatever types of apps are available in your container... if a custom container was created earlier with more templates they will be available. Just make sure you run that one instead of the microsoft/dotnet:2.1-sdk-alpine as above.
```
%> dotnet new console|classlib|web|webapi|xunit 
```




## Configuring Web Host
The current default web and webapi templates runs on ports 5000 and 5001 and wants to enforce SSL and doesn't read command line args anymore which simplifies the docker deploy file so lets go ahead and fix that.

Create a configuration by reading the CommandLineArgs in the Program.cs main
and pass them into the Build and use them there

```
  public static void Main(string[] args)
  {  
      var configuration = new ConfigurationBuilder()
          .AddCommandLine(args)
          .Build();

      BuildWebHost(args, configuration).Run();
  }
```

```			
	public static IWebHost BuildWebHost(string[] args, IConfiguration configuration) =>
			WebHost.CreateDefaultBuilder(args)
			.UseConfiguration(configuration)
			...
```

### Project Properties for watcher hosted in Docker

* https://natemcmaster.com/blog/2018/05/12/dotnet-watch-2.1/ 

adding a Directory.Build.props file for the watcher hosted in Docker
```
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
```

## Application Development

### Configuration 
* can we use docker secrets in our swarm
* when using the watcher will they automagically take effect without restart?

### WebApi Claims Transformer


### WebAPI Identity and Authorization

### WebAPI Swagger

### WebApi Content Negotiation

### WebApi Exception Handling


# TODO:

# DEBUG FROM VSCODE... need to get here!
*https://www.reddit.com/r/csharp/comments/9cyl6a/debugging_dotnet_watch_processes_inside_of_a/




Probably should add pages about:

IdentityServer

serisink -> Application Logger and Log Watcher and Admin Interface
* UI and API ONLY for system administrators
* Websocket to watch errors in realtime
* serisinkpulse -> generate messages for testing
* we don't need 