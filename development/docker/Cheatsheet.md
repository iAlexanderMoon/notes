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

* Clean up volumes not attached to a container
* Clean up volumes not attached to a container ignoring those with a keep label assigned

```sh
docker volume prune
docker volume prune --filter "label!=keep"
```

## Want a nearly complete restart?

* Prune everything
* Prune everyting and volumes

```sh
docker system prune
docker system prune --volumes
```