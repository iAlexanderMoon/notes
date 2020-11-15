vscode:

As of November you can download vscode extensions locally for installing remotely to speed it up!

Vscode setting:

remote.downloadExtensionsLocally

Is hot module reloading working in docker?

index.html:

	- assumes we are being hosted at "/" right now.
	- Why is jquery required in an angular application?

how to change the settings for the identity server locally
	- they are configured in environment I think.
	- could we serve them isntead
	

ClientApp/package.json contains the defined scripts that are used for running the development server or building the product.


In Development Mode... dotnet core runs:

spa.UseAngularCliServer(npmScript: "start");


start:	run-p theme \"serve -- {@}\" -- 
 
 * run-p	Run concurrently (in parrallel)
	* theme rule
	* serve {@}	: {@} means all arguments
	
so npm run start arg1 arg2 means: npm run sere arg1 arg2
	

 
What is Eben Monney and why did you use it?
https://www.ebenmonney.com/forum/topic/moving-from-concurrently-to-npm-run-all-in-package-json/

Hot Module Reload isn't working! Kevin said it works in his development environment... but I don't think so.
https://github.com/angular/angular-cli/blob/master/docs/documentation/stories/configure-hmr.md

The following dependency is missing from packages.json:

	npm install --save-dev @angularclass/hmr
	
The examples show adding an hmr environment/environment.hmr.ts


Update angular.json to include hmr.
file replacements 


configure the serve task in angular.json with

  "host": "localhost",
            "port": 9038,
            "publicHost": "dev.local/common-demo/sockjs-node",
            "baseHref": "/common-demo/",
            "servePath": "/"
