changing settings for environments... it's one thing to do that when we compiple but what about runtime settings?

Donet core is NOT used to server index.html. That is done by angular/node.
Which means in development that's fine... but NOT for server side includes 
if we want to serve runtime configurations we need to build with a new set of environment variables... seems dumb.


Change settings for different identity server! Build Time

Add a new environment/ same name as Environment Variable

ClientApp/src/environments/environment.dockerdev.ts

When you startup you can see it if there is an error. well 

arguments are set "--port" "44219" from 

asdf
`ng serve -c dockerdev "--port" "44219"`

But the only thing we can change is the command name I think


What is the process for building and deploying?
npm run-script build puts everything in 
ls OBS.Spa/ClientApp/dist/
	including index.html

dotnet publish should also work actually!

https://docs.microsoft.com/en-ca/aspnet/core/client-side/spa/angular?view=aspnetcore-2.2&tabs=visual-studio


Can actually use the dotnet core for proxying in development with ng serve used 
spa.UseProxyToSpaDevelopmentServer("http://localhost:4200"); and run the spa development server WITHOUT dotnet core directly.
Faster start ups.

So to pass arguments we could simply start a different named script!


What is the USE of dotnet core to host the spa instead of node directly during development? It's just a proxy anyway.

WHat facility does Angular have for run time environment variables? 

The Limitations of Angular CLI Application Environments
Limitation 1: Every environment requires a separate build
Limitation 2: The application config is part of the application code
https://www.jvandemo.com/how-to-use-environment-variables-to-configure-your-angular-application-without-a-rebuild/
https://juristr.com/blog/2018/01/ng-app-runtime-config/