# README: Developing VueJs with Docker
* interactive

## Create the docker image container vuecli to be used as the base
* Install via npm and install via yarn
* Both options require strict-ssl off when working behind the corporate firewall
* The base image will be used for building, serving, and linting!
* See the dockerfiles as necessary

``` 
	docker build --rm -t vueyarn:latest Dockerfile.vueyarn .
    docker build --rm -t vuenpm:latest -f Dockerfile.vuenpm .
```

_At first the vuecli from npm didn't work but I did get yarn to work in the end I don't think it matters which you choose once the strict-ssl is set_

__The exact commands used are in the Makefile in the root of this solution.__

__Common Dependencies__
If all of your vue projects use the same dependencies, for example bootstrap, then you can build those dependencies into base docker image that is used to develop the app.

## Run from the command line 

### Interactive shell

```
	docker run
  		-p map port
		-d detached
		--rm remove container when stopped
		--name assign a name to container
		-v shared volume
		-w working directory
```

Run the image interactively and issue commands on the shell
```
docker run --name profile_vue --rm -it -v /working/profile/profile.vue:/app -w /app -p 8080:8080 olymel/vueyarn:latest /bin/sh
````



Initialize a new project build and serve
```
> vue init pwa <projectname>
> npm install
> npm run build
> npm run serve		
````



### Interactive
* Run the image interactively to build, lint, and serve
* Theoretically you can let it run tests as well
```
docker run --name profile_vue --rm -it -v /working/profile/profile.vue:/app -w /app -p 8080:8080 olymel/vueyarn:latest npm run serve
docker run --name profile_vue --rm -it -v /working/profile/profile.vue:/app -w /app -p 8080:8080 olymel/vueyarn:latest npm run lint
docker run --name profile_vue --rm -it -v /working/profile/profile.vue:/app -w /app -p 8080:8080 olymel/vueyarn:latest npm run build
```

## NPM RUN SERVE Configuration 

Vue-Cli will only serve to localhost and using the command arguments to --host 0.0.0.0 doesn't work!
As states by the startup if running in the container you need to map the port (this can be done in virtualbox)
alternatively we can assign it a public name but this can't be done from the command line and must be done in vue.config.js	

Reference: https://github.com/vuejs/vue-cli/issues/1616 created 

_vue.config.js_ in the root of the project:
```
	module.exports = {
		devServer: {
			host: '0.0.0.0',
			port: '8080',
			public: 'dockerdev:8080',
		},
	}
```

Once "npm run serve" is up and running... 

Should be able to browse to dockerdev:8080 and edit through VS Code on the Windows Host

_It is important to note that for HOT MODULE RELOAD to function the exterior PORT mapped from the docker service to the internal service MUST match... so 8080:8080 or 8010:8010 will work while 8010:8080 does not!_


___!!! FYI:  Changes to vue.config.js ARE NOT auto reloaded!___

___!!! HOT MODULE RELOADING WORKS OVER SMB WITHOUT POLLING WHEN FILES !!!___


## Run daemonized 

### Manual Management

```
docker run --name profile_vue --rm -d -v /working/profile/profile.vue:/app -w /app -p 8080:8080 olymel/vueyarn:latest npm run serve
```

Connect to logs from a container:
````
docker logs -f profile_vue
````

### Run daemonized on Docker Stack

Configure stack.yml:

	services:	  
	  profile_vue:
		image: olymel/vueyarn:latest
		ports:
		  - "8080:8080"
		networks:
		  - front-net
		volumes:
		  - /working/profile/profile.vue:/app
		working_dir: /app
		command: npm run serve

	networks:
	  front-net:

* Deploy the stack 
* Follow service logs
* Remove the stack
```
docker stack deploy -c devstack.yml dev
docker service logs -f dev_profile_vue
docker stack rm dev
```	

#TODO
 
## Installing additional NPM packages?
This needs to be done through the interactive shell of a running container.

Should create a makefile command that connects to a running instance in the swarm without having to list it manually.


