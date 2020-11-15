SPA Development with dotnet

dotnet watch spa

The configured user limit (8192) on the number of inotify watches has been reached.

nodejs in the spa:
 ENOSPC: System limit for number of file watchers reached, watch '/home/moon/working/olywestbs/OBS.Spa/ClientApp/src/app/assets/themes/'


So the total number of watches in the container environment of 8192 is exceeded.

YOU CANNOT INCREASE THE KERNEL IN A CONTAINER... the kernel configuration is mounted read-only from the host... kernel is SHARED!!!! 
You can modify the kernel host settings! Don't forget to restart containers aftward (is this necessary?)
 
Get settings of the inotify system:
```
sysctl fs.inotify
```
fs.inotify.max_queued_events = 16384
fs.inotify.max_user_instances = 128
fs.inotify.max_user_watches = 8192


Edited /etc/sysctl.d/local.conf
fs.inotify.max_user_watches = 524288

Restart to updated
```
sysctl fs.inotify
```
fs.inotify.max_queued_events = 16384
fs.inotify.max_user_instances = 128
fs.inotify.max_user_watches = 524288




### EVALUATING WHAT IS USING ALL THE WATCHES!

Examine inotify watches to see what is being watched. Don't forget your context... you may need exec into a running container!

```
losf | grep inotify
```

```
for foo in /proc/\*/fd/*; do readlink -f $foo; done |grep inotify |cut -d/ -f3 |xargs -I '{}' -- ps --no-headers -o '%p %U %a' -p '{}' |uniq -c |sort -n
```

In our sideload container it is unsurprising vscode. How do we get the path the inotify endoint?
https://github.com/cdr/code-server/issues/628
























Are we excluding what should be excluded by each process?
is the node_modules being ignored?

    <!--Dotnet Watch should NOT rebuild on changes to node_models-->
  <ItemGroup>
    <Watch Exclude="ClientApp\node_modules\**\*.js;$(DefaultExcludes)" />
  </ItemGroup>
  
VS CODE File Watching

files.watcherExclude settings to the .devcontainer

https://code.visualstudio.com/docs/setup/linux#_visual-studio-code-is-unable-to-watch-for-file-changes-in-this-large-workspace-error-enospc

.devcontainer.json
```
{
	settings: {
		"files.watcherExclude": {
			"**/.git/objects/**": true,
			"**/.git/subtree-cache/**": true,
			"**/node_modules/*/**": true
		}
	}
}
```

Bash Check inodes:



