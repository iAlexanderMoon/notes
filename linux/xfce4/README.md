# Xfce4 - Development Environment

Xfce4 is a lightweight desktop environment.

It's gui applications tend to be simpler or not as sophisticated when compared with other desktop environments... but for Xfce users, this is a good thing.

Instead of adding gui applications to the DE, prefer using terminal based applications:
* htop - process monitoring
* lazydocker - docker monitoring

This script will start these tools. 
Geometry is set to put them on the secondary monitor.

```sh
#!/usr/bin/bash

xfce4-terminal -T lazydocker --geometry 1080 --fullscreen -e lazydocker
xfce4-terminal -T htop --geometry 1080 --fullscreen -e htop
```
