# Docker VueJs Production:

## Dockerfile.release
### Stage 1: Vue 
* Use a 2 stage docker build:
  * stage 1 vue build 
* npm cert fails behind corporate firewall
```
npm set strict-ssl false
```

### Stage 2: Static File 

	pierrezemb/gostatic is this actually smaller than nginx? Yes

		static webserver:
		bind to any
		write requests to stdout and stderr
		run in the foreground
		very small

## TODO:
* npm will redownload the npm packages every time... no it will cache what layers it can first
* Still it may be better to run a local registry.
* Verdaccio? See setting up Alpine Host 

``` 
npm config set registry http://registry.npmjs.org/
```
