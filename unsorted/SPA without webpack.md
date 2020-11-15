dotnet new mvs 

default tools 

dotnet tool install -g --version 2.0.0 RendleLabs.UnpkgCli

https://github.com/RendleLabs/dotnet-unpkg


# Create a new unpkg based mvc application

We use a docker container image for running the cli. 
It mounts the working directory into the container.

```
make dotnetcli
cd /working
```

From inside the container we now run dotnet commands.
To create a project. Here is an example to create rms which is a dotnet core mvc application with NO authentication.

```
dotnet new mvc -au None -o rms
exit
````

Unfortunately this will create all files with the root user so... better get access to them back on the development machine.

```
sudo chown -R moon:moon rms
```


# Simple SPA
# 
https://medium.com/@steveolensky/create-a-spa-vuejs-application-without-node-webpack-or-any-other-build-process-using-vue-router-b9bf8e36e958


# Easiest to start with mvc project.
# Clean OUT EVERYTHING.
# Home/Index mvc should serve all requests accept those under the api or swagger!


Use unpkg to install javascript dependencies:

unpkg add vue.js
unpkg add axios.js




https://medium.com/@steveolensky/create-a-spa-vuejs-application-without-node-webpack-or-any-other-build-process-using-vue-router-b9bf8e36e958
