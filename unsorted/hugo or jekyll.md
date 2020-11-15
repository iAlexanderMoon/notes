Jekyll using Docker Tooling:

	docker pull jekyll/jekyll

make dependencies

make all

make clean

make deploy

# For development we can just use a 

Run Jekyll as the current user PID and GID.

# Run the new directory in the container

```
	make cli
```

```
cli> cd src
cli> jekyll new blog
cli> jekyll new doc
```

jekyll build
jekyll new <name> --blank


docker run --rm -it --name=jekyllcli -e JEKYLL_UID=1000 -e JEKYLL_GID=1000 jekyll/jekyll:4.0 /bin/bash 


What is discourse 

https://gohugo.io/



