# PROXY
* using Traefik

## Runing Applications behind a reverse proxy
To run behind the proxy we have a couple of options that need to be considered when it comes to paths.
Some reverse proxies will process the content before serving it back down to the client... this is slower, resource intensive and error prone.
Definitely prefer NOT to do that

As per Traefik:
    After speaking with other members of the team, we think that the process of rewriting the response body in a reverse proxy is too heavy/prone to errors.
    We encourage you to either:
    * use host based rules only
    * use specific Traefik path rewrite headers in you app
    * let your app listen on a specific path

_use host based rules only_
* dns configuration required
* more cumbersome for SSL
* can move and link to services not behind the same proxy

_use specific Traefik path rewrite headers in you app_
* headers don't work for static js paths

_let your app listen on a specific path_ 
* not very portable
* easy by convention
* should work with static vue js files

___!!! IMPORTANT !!!___

When routing paths it will be likely necessary to add priorities to your front end.

As per Traefik:
    
    Priorities
    By default, routes will be sorted (in descending order) using rules length (to avoid path overlap): PathPrefix:/foo;Host:foo.com (length == 28) will be matched before PathPrefixStrip:/foobar (length == 23) will be matched before PathPrefix:/foo,/bar (length == 20).

    You can customize priority by frontend. The priority value override the rule length during sorting:

Higher priority rules are processed first, (50>20) 50 is processed first.

### use host based rules only ###
Modify dns entries on development host linux or windows:
* /etc/hosts entries
* C:\Windows\System32\Drivers\etc\hosts
```
ip-address dockerdev
ip-address profile.dockerdev
ip-address profile-api.dockerdev
```

#### .net core - web api ####
* No base changes and can run on any port

Add the rule to the service configuration in the service stack
```
- "traefik.frontend.rule=Host:profile-api.dockerdev"
```
The following should work:
* http://profile-api.dockerdev/swagger/
* http://profile-api.dockerdev/swagger/v1/swagger.json
* http://profile-api.dockerdev/api/Values

#### vuejs server (Development from node) ####
* Development Server Settings are only pertinent during development
* You MUST configure the inbound host, port AND public in vue.config.js:
```
module.exports = {
    devServer: {
        host: '0.0.0.0',
        port: '8080',
        public: 'profile.dockerdev:8080'
    },
}
```

Add the rule to the service configuration in the service stack
```
- "traefik.frontend.rule=Host:profile.dockerdev"
```

The following should work:
* http://profile.dockerdev/

### let your app listen on a specific path ###
*Root DNS was likely added when setting up the host.:*
* /etc/hosts entries
* C:\Windows\System32\Drivers\etc\hosts
```
ip-address dockerdev
```

#### .net core - web api ####
* Change the root of the middleware using app.Map in Startup.Configure
* By convention Constant.AppName is defined in static Constants (a configuratino override could be provided to specify the root path)
* For swagger, notice the swagger UI pointing to the relative root "./"

```
app.Map($"/{Constant.AppName}", api => 
{    
    api.UseSwagger();
    api.UseSwaggerUI(c =>
    {
        c.SwaggerEndpoint($"./{Constant.Version}/swagger.json", {Constant.Title} - {Constant.Version}");
    });
    api.UseMvc();
});
```

Add the rule to the service configuration in the service stack:
* api and swagger priority higher than the vuejs
* passHostHeader is required or _Bad Gateway_ is returned
* appname should be substitued as defined in the Constants file of the .net core api project (this is by convention ONLY for now... could provide a configuration override to change the default root of the api)
```
- "traefik.frontend.priority=20"
- "traefik.frontend.passHostHeader=true"
- "traefik.frontend.rule=Host:dockerdev;PathPrefix:/appname/api,/appname/swagger"
```

The following should work:
* http://dockerdev/swagger/*
* http://dockerdev/api/*


__Note: Leave the exposed port in the docker service docker port for direct access to the api during development__

#### vuejs server (Development from node) ####
* Development Server Settings are only pertinent during development
* You MUST configure the inbound host, port AND public in vue.config.js:
Add the rule to the service configuration in the service stack, remembering that priority needs to be higher for api and swagger than the vuejs app. appname should be substitued as defined in the Constants file of the .net core api project (this is by convention ONLY for now... could provide a configuration override to change the default root of the api)
```
- "traefik.frontend.priority=10"
- "traefik.frontend.passHostHeader=true"
- "traefik.frontend.rule=Host:dockerdev;PathPrefix:/profile"
```

* Invalid Host Header: public address set incorrect!
* baseUrl must be configured, by convention it should be set to appname
```
module.exports = {
    devServer: {
        host: '0.0.0.0',
        port: '8080',
        public: 'dockerdev:8080'
    },
    baseUrl: '/profile/',
}
```

# Notes
* If the proxy is failing double check the syntax of the rules... it is easy to mix up : or ; or , or = signs.
