# Docker Cli Cheatsheet

```sh
docker ps
docker images
docker run -it {image} /bin/{bash|sh...}
docker exec -it {container} /bin/{bash|sh...}
docker tag {imagehash} {tag:number}
docker rmi --force {imagehash}
docker rmi {tag:number}
```

## Clean up

* Clean up dangling images only
* Clean up all images not attached to a container
* Clean up all images not attached to a container older than 24 hr

```sh
docker image prune 
docker image prune -a
docker image prune -a --filter "until=24h"
```

* Remove all stopped containers
* Remove all stopped containers older than 24 hr
```sh
docker container prune
docker container prune --filter "until=24h"
```


## Want a nearly complete restart?

Stop everything and prune everything

```sh
docker container prune
docker image prune -a
```