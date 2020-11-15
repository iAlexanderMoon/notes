README: Developing VueJs with Docker
* interactive

# execute the docker image manually for watching vue
# Use our previously created image with the vue sdk installed
# we have created the sdk image 2 ways:
# node with vuecli installed through npm: vuenpm
	* without a package.json this didn't work!
	* vueyarn image does work!

# Run from the command line with vueyarn image interactively
# docker run
#  -p map port
#  -d detached
#  --rm remove container when stopped
#  --name assign a name to container
#  -v shared volume
#  -w working directory
#
docker run --name profile_vue --rm -it -v /working/profile/profile.vue:/app -w /app -p 8080:8080 olymel/vueyarn:latest /bin/sh

# try the commands manually
#
> npm run build
> npm run serve		

	Vue-Cli will only serve to localhost and using the command arguments to --host 0.0.0.0 doesn't work!
	As states by the startup if running in the container you need to map the port (this can be done in virtualbox)
	alternatively we can assign it a public name but this can't be done from the command line and must be done in vue.config.js	
	Reference: https://github.com/vuejs/vue-cli/issues/1616 created vue.config.js in the root of the project
	
	module.exports = {
		devServer: {
			host: '0.0.0.0',
			port: '8080',
			public: 'dockerdev:8080',
		},
	}

Once "npm run serve" is up and running... 

!!! HOT MODULE RELOADING WORKS OVER SMB !!!

Should be able to browse to dockerdev:8080 and edit through VS Code on the Windows Host

# Run daemonized & connect to output through logs when necessary
#
docker run --name profile_vue --rm -d -v /working/profile/profile.vue:/app -w /app -p 8080:8080 olymel/vueyarn:latest npm run serve
docker logs -f profile_vue

# Run in a docker stack
# devstack.yml:

	services:
	  # docker run --name profile_vue --rm -d -v /working/profile/profile.vue:/app -w /app -p 8080:8080 olymel/vueyarn:latest npm run serve
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

# deploy the stack & connect to output through logs when necessary
docker stack deploy -c devstack.yml dev
docker service logs -f dev_profile_vue
	
	
	
# Probably want to look into this! DEBUG REMOTE... wait I really don't need this... 
# unless I am writing the server side api in node because all the debugging I need should be in the browser (firefox or chrome) 
# https://blog.risingstack.com/how-to-debug-a-node-js-app-in-a-docker-container/
# https://codefresh.io/docker-tutorial/debug_node_in_docker/
	
	
		
TOP MOST DIRECTORY MAKE FILE...
make dependencies
make environment
make init-stack
make deploy-stack
make attach-<servicename>

