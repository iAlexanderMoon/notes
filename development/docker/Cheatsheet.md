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

Clean up dangling images only
Clean up all images not attached to a container
Clean up all images not attached to a container older than 24 hr

```sh
docker image prune 
docker image prune -a
docker image prune -a --filter "until=24h"
```
