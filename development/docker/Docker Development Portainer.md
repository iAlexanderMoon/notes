# Docker Development Docker GUI

## Portainer

Running Portainer makes it easier to see logs and status of deployed services. Portainer should be included in your compose file to launch with the rest of the services:

```
services:

  # VERY UNSECURE TO DEPLOY WITH AUTHENTICATION: DEVELOPMENT ONLY
  # https://portainer.readthedocs.io/en/stable/configuration.html
  # use portainer connected to local
  portainer:
    image: portainer/portainer:latest
    command: --no-auth --host unix:///var/run/docker.sock
    ports:
      - "9000:9000" 
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
```