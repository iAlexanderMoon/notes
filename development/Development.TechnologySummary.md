## Technologies
* .NetCore 2
  * Identity Server 4
  * Core WebApi
* NodeJs 8
  * Webpack
  * VueJs
  * Progressive Web Technologies
* Docker

### .Net Core 2
Create a new project
````
dotnet new <type>
````

### VueJs 2
Create a new pwa (progressive web app) project`
````
vue init pwa _projectname_
````
#### Components
* vue-router
* vuex store
* vue-bulma
* axios

### .NetCore 2
Create new class library, webapi (mvc)

````
mkdir _projectname_ && cd _projectname_
dotnet new classlib
dotnet new xunit
dotnet new console
dotnet new mvc
dotnet new webapi
````

#### Webapi ####
* Use bearer token authentication (jwt)
  * user id (jwt:sub)
  * preferred_username (jwt:preferred_username)
  * roles (jwt:roles)
* Roles should map to sets of access tokens in claims transformer
* Use Policies
  * check for access tokens (not roles)
* Swagger
* Swagger UI
