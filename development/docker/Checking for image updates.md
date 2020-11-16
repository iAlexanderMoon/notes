docker images --format "{{.ID}}: {{.Repository}}"

docker images --digests --format "{ {{json .Repository}}, {{json .Tag}}, {{json .Digest}} }"

Images without digests are created locally.

Tags can be done in any way

How can we tell what the root server is... 
dockerhub or mcr.microsoft etc?

