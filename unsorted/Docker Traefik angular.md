
ng serve with traefik reverse proxy and docker

## Angular Version
			  
Angular CLI: 7.3.9
Node: 10.18.0
OS: linux x64
Angular: 7.2.15

## TLDR:

I couldn't figure out how to proxy the socket requests... so I stopped trying. Environment variables. Set them how you like. ${ }

* Expose the websocket port through docker.
* Pass a configuration to ng serve from npom run script on container startup. 
* Use Traefik to Proxy to the subpath with passthrough /spa
* Set ng serve to listen on ANY address "host"
* Let ng serve serve ANY address:	"disableHostCheck"\
* Set ng serve base path for application response modifying index.html served:	"baseHref" pay attention to slashes! Probably need the trailing slash.
* Set ng serve address for websocket requests: "publicHost"

Done here as excerpt from docker-compose.yml file:
```
  servicename:
    ports:
      - "${HOST_IP}:8011:4200"
    command: npm run-script start -- --configuration=dockerdev
	labels:
      - "traefik.enable=true"
      - "traefik.docker.network=${STACK}_traefik"
      - "traefik.port=4200"
      - "traefik.frontend.priority=20"
      - "traefik.frontend.passHostHeader=true"
      - "traefik.frontend.rule=Host:${HOST_NAME};PathPrefix:/spa"
      - "traefik.backend.loadbalancer.method=wrr"
```

Angular needs to be setup to read different environments (unfortunately this is a BUILD TIME ONLY and CAN'T BE MODIFIED FOR DEPLOYMENT. 
But for now you can specify file replacements for the target configuration in angular.js in the build configurations. 
And specify the serve configuration to use the build configuration as well as allow 

```
      "architect": {
	    "build": {
		  "configurations": {
            "dockerdev": {
              "fileReplacements": [
                {
                  "replace": "src/environments/environment.ts",
                  "with": "src/environments/environment.dockerdev.ts"
                }
              ]
            },


      "architect": {
		"serve": {
          "builder": "@angular-devkit/build-angular:dev-server",
          "options": {
            "browserTarget": "quickapp:build"
          },
          "configurations": {
            "dockerdev": {
              "browserTarget": "quickapp:build:dockerdev",
              "verbose": false,
              "progress": false,
              "host": "0.0.0.0",
              "disableHostCheck": true,
              "baseHref": "/spa/",
              "publicHost": "http://devobs:8011/"
            },

```


-----------------------


By Default... spa will run on 4200

Start by setting up an alternate development configuration

npm run-script start -- --configuration=dockerdev


** Angular Live Development Server is listening on localhost:4200, open your browser on http://localhost:4200/ **


So we can expose the service to docker... by exposing port 4200 docker... excerpt from a compose file: 

 ports:
      - "${HOST_IP}:4200:4200"
Still not seeing any output when requesting port 4200. 
	  
Setting verbose only increases the build output. Which we don't need. 
Actually because we are running in docker it's probably better to not show progress either.

Is it an http interceptore?
https://angular.io/guide/http#intercepting-requests-and-responses

              "verbose": true,
              "progress": false


Why doesn't ng serve have any mechnism for request logging? 

Pretty sure the ng serve server is dropping the request because the hostname is incorrect.
Let's try binding the server to ANY request that hits it.

  "host": "0.0.0.0",
  
** Angular Live Development Server is listening on 0.0.0.0:4200, open your browser on http://localhost:4200/ **
  
Now atleast it serves something: 

	Invalid Host header
  
What if we add disableHostCheck:

              "disableHostCheck": true,
     
Served with websocket and liveReload running to automatically reserve the entire application after a file change.
	
	http://devobs:4200/sockjs-node/info?t=1577558915776 


Now... how about behind the traefik proxy? instead of requesting http://devobs:4200 we want to http://devobs

	THAT WORKS! with websocket and liveReload

So if we want more then 1 spa we will need more than 1 port. Does docker port forwarding work to 8011:4200?

	THAT WORKS! with websocket and liveReload

So no real reason to change the port because each app is running in a seperate docker container.

----------------------------------

So.... what if we don't want the application to be run from root? http://devobs/spa

Reverse proxying can be cumbersome... instead set the application to be run from the path and the traefik proxy can straight through requests.

--baseHref=baseHref 		Base url for the application being built.
--publicHost=publicHost 	Specify the URL that the browser client will use.
--servePath=servePath 		The pathname where the app will be served.	
--deployUrl=deployUrl 		URL where files will be deployed.

### baseHref
* Base url for the application being built

	"baseHref": "spa"
		
** Angular Live Development Server is listening on 0.0.0.0:4200, open your browser on http://localhost:4200/ **
	
http://devobs:8011/spa/
	Page Found.
	Spa assets work.
	static assets work.
	websocket works.	http://devobs:8011/sockjs-node/info?t=1577561768045
	
http://devobs/spa
	Page Found.
	Spa assets didn't work!
	static assets didn't work!
	websocket didn't work because request wasn't made!
	
	
	"baseHref": "/spa/"

http://devobs/spa
	Page Found.
	Spa assets work.
	static assets work.
	websocket didn't work!	http://devobs:8011/sockjs-node/info?t=1577561768045
	
http://devobs:8011/spa/
	Page Found.
	Spa assets work.
	static assets work.
	websocket does work!	http://devobs:8011/sockjs-node/info?t=1577568449855
	
If we leave it the way it is can we expose the websocket server for development on 8011 without proxy

	"baseHref": "/spa/"
	"publicHost": "http://devobs:8011/"

http://devobs/spa
	Page Found.				http://devops/spa/..
	Spa assets work.		http://devops/spa/..
	static assets work. 	http://devops/spa/..
	websocket works			http://devobs:8011/sockjs-node/info?t=1577569011460
	
http://devobs:8011/spa/
	Page Found.				http://devops:8011/spa/..
	Spa assets work.		http://devops:8011/spa/..
	static assets work.		http://devops:8011/spa/..
	websocket works.	http://devobs:8011/sockjs-node/info?t=1577568449855
	
	
With the Websocket working liveReload is working. This is slower than hot module reload though. It rebuilds and triggers a refresh on the browser.
	
	
	
	
	
	
### deployUrl

	"deployUrl" : "spa"
	
	** Angular Live Development Server is listening on 0.0.0.0:4200, open your browser on http://localhost:4200/spa **

	Try it without the proxy first:		http://devobs:8011/spa
		Page Not Found!
		websocket works.
		static assets don't work!

	Then with the proxy:				http://devobs/spa
		Page Not Found!
		websocket does not work!
		static assets don't work!

Try with trailing slash!

	"deployUrl" : "spa/"
	
		** Angular Live Development Server is listening on 0.0.0.0:4200, open your browser on http://localhost:4200/spa **

	Try it without the proxy first:		http://devobs:8011/spa
		Page Not Found!
		Spa assets work.
		static assets don't work!
		websocket works.	http://devobs:8011/sockjs-node/info?t=1577560678437

	Then with the proxy:				http://devobs/spa
		Page Not Found!
		Spa assets work.
		static assets don't work!
		websocket does not work!		http://devobs/sockjs-node/info?t=1577560709907
	

		**** Looks like the request for the websocket is NOT proxied because it is missing /spa ***
	

### publicHost	

--publicHost=spa 

	websocket doesn't work!	http://spa:8011/sockjs-node/info?t=1577561139485
	websocket doesn't work!	http://spa:8011/sockjs-node/info?t=1577561139485

 --publicHost=http://devobs/spa
	
	websocket does not work! http://devobs/spa/sockjs-node/info?t=1577561421005
	websocket does not work! http://devobs:8011/spa/sockjs-node/info?t=1577561421020

	
Okay time to stop playing with publicHost... you'd think you'd just use that to set the host name anyway which doesn't seem to be the problem.
	
* public-host represents the URL that a web browser will use to access the app. 
	

### servePath
	
	serve-path represents the path that the dev server itself will serve the app. 
	It defaults to a combination of the base HREF and deploy URL.
	
So let's go without servePath and put baseHref and deployUrl togther.


	"baseHref": "/spa/"
	"deployUrl" : "spa/"
	
Nothing seems to work this way.
	
	"baseHref": "/spa"
	"servePath": "spa"
	  
** Angular Live Development Server is listening on 0.0.0.0:4200, open your browser on http://localhost:4200/spa **
	
	"baseHref": "/spa"	DOESN'T WORK, missing trailing slash
	"baseHref": "/spa/" DOES WORK.

	http://devobs/sockjs-node/info?t=1577564864462	Socket not being found or forwarded in either case.
	
	

	
	
 --publicHost=http://devobs/spa
	
	http://devobs/spa/sockjs-node/info?t=1577567758705
	
	Correct path but not found.

	
	
--deployUrl=deployUrl 	URL where files will be deployed.
** Angular Live Development Server is listening on 0.0.0.0:4200, open your browser on http://localhost:4200/spa/devobs **
	
	
	
Everything is still working. Now.. we want to change the hosting path for the proxy.

Let's move it... actually let's just try another port first

http://devobs/spa


First... 


         "port": 8011,
              "host": "0.0.0.0",
              "publicHost": "devobs/obsspa",
              "disableHostCheck": true,
              "baseHref": "/obsspa/",
              "servePath": "/obsspa/",
              "browserTarget": "quickapp:build:dockerdev",
              "watch": true,
              "liveReload": true,
              "progress": false
			  


