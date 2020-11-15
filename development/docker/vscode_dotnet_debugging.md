dotnet-outdated

		Can be used for upgrading an application.
		Should be run periodically to see what is out of date.
		

ls



Debugging Container Dev with Visual Studio Code for .net core.

.vscode/launch.json MUST include an type coreclr launch configuration. Typically named ".NET Core Attach",
```
        {
            "name": ".NET Core Attach",
            "type": "coreclr",
            "request": "attach",
            "processId: "${command:pickProcess}"
        }
```
		
The debugger needs to be inserted into the container being used for debugging,


New Window.
Attach to remote container (find the doccker container running the .net core application)

(Why does VSCODE ALWAYS have to install Omnishap for Linux... very annoying)

Installing Razor/tmp

F5 or start debugging. 
 
Chose dotnet watch run process. 
	Unfortunately it says... no symbols are loaded!
	

Chose the dotnet run 
	Unfortunately it says... no symbols are loaded!
	
TROUBLESHOOTING:

1) IS there a pdb file and where is it?

Well I provided a Directory.Build.Props which included:

  <PropertyGroup Condition="'$(DOTNET_RUNNING_IN_CONTAINER)' == 'true'">
    <BaseIntermediateOutputPath>/tmp/$(MSBuildProjectDirectory)/obj/container/</BaseIntermediateOutputPath>
    <BaseOutputPath>/tmp/$(MSBuildProjectDirectory)/bin/container/</BaseOutputPath>
  </PropertyGroup>

 So in vscode already attached to the container go to the bash terminal and see if we can find them there.
 %> find /tmp -name *.pdb
 
 et voila: they are there.
 
 /tmp/working/apps/ap/ap.api/bin/container/Debug/netcoreapp3.0/Olymel.AP.Api.pdb
/tmp/working/apps/ap/ap.api/obj/container/Debug/netcoreapp3.0/Olymel.AP.Api.pdb

and right beside it are the dlls.

So what all of that REALLY TOLD ME! YOU NEED TO CHOOSE THE PROCESS RUNNING THE dll!!!

AND IT WORKS!




How about some command line debugging

Docker Exec bin/bash in the running container.



https://codeblog.dotsandbrackets.com/command-line-debugging-core-linux/

My attempt on Xubuntu 19.10
Turns out you need to install the tool now it is no longer shipped with dotnet you have to install it seperately.
https://github.com/dotnet/diagnostics/issues/550
https://github.com/dotnet/diagnostics/blob/master/documentation/installing-sos-instructions.md

I don't like installing the global tools to /root/.dotnet/ seems like a strange place for them so I provide a tool path.
```
	dotnet tool install --tool-path /usr/local/bin dotnet-sos
	dotnet-sos install
```

There are no options to NOT put the files into /root!

So instead of finding it in shared 

```
	$ find / -name *sos*
	/root/.dotnet/sos/libsosplugin.so
```


Also need lldb install so this is assuming your devcontainer or container being used to run has lldb installed since I'm starting with 3.1.100 Stretch (debian):
Probably just want to install these things by default in the devcontainer build
```
	apt install lldb
```

Confirm everything is working: Run lldb and at the lldb shell try soshelp?

```
	$ lldb
	(lldb) soshelp
```

Okay try and attach to process... TBH this is the first time I've ever used lldb so... learning:

```
	(lldb) help
	# Lots to read!
```

Also for further reading on what to do with lldb I suggest reading the 



First we will need to know the PID (processid we want to attach too. 

```
	$ ps -ax | grep dotnet
	 1 ?        SLsl   0:02 dotnet watch run --urls http://0.0.0.0:8017
	 22 ?        SLl    0:00 /usr/share/dotnet/dotnet /usr/share/dotnet/sdk/3.1.100/DotnetTools/dotnet-watch/3.1.0-rtm.19566.1/tools/netcoreapp3.1/any/dotnet-watch.dll run --urls http://0.0.0.0:8017
	 82 ?        SLl    0:01 /usr/share/dotnet/dotnet run --urls http://0.0.0.0:8017
	192 ?        Z      0:03 [dotnet] <defunct>
```

Typically in a docker container it'll be process 1. Using dotnet watch to auto rebuild on source change so that makes sens to me.

Startup lldb 
load the libsosplugin plugin 
attach to the pid

```
	lldb
	(lldb) plugin load /root/.dotnet/sos/libsosplugin.so
	(lldb) process attach -p 1
	error: attach failed: Operation not permitted
```

Great! Well what next. 

https://github.com/microsoft/MIEngine/wiki/Troubleshoot-attaching-to-processes-using-GDB


Could be something.

```
	$ cat /proc/sys/kernel/yama/ptrace_scope
	1
```

Can't set ptrace_scope on the container MUST be changed on the HOST because the container is has the readonly system mounted.

ON THE DOCKER HOST:
```
	$ echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope
```

So what is YAMA? https://www.kernel.org/doc/Documentation/security/Yama.txt
general solution is to only allow ptrace directly from a parent to a child process

* Yama is a Linux Security Module that collects system-wide DAC security protections that are not handled by the core kernel itself.
* general solution is to only allow ptrace directly from a parent to a child process
* Mode 0: 0 - classic ptrace permissions: a process can PTRACE_ATTACH to any other process running under the same uid
* Mode 1: 1 - restricted ptrace: a process must have a predefined relationship with the inferior it wants to call PTRACE_ATTACH on.

That's enough for this laymen... set it back to mode 1 when you are done!
```
	$ echo 1 | sudo tee /proc/sys/kernel/yama/ptrace_scope
```

So yes after setting to 0 on the host I can infact now debug the application using llldb!
From reading about yama though I SHOULD have been able to do so using sudo lldb ... but that didnt't work. Probably related to Docker?
If you are someone with more knowledge please feel free to fill me in.


Alright let see if we can debug or what!



```
	$ lldb
	(lldb) plugin load /root/.dotnet/sos/libsosplugin.so
	(lldb) process attach -p 1
	# lots of output
	(lldb) clrthreads
	ThreadCount:      25
	UnstartedThread:  0
	BackgroundThread: 10
	PendingThread:    0
	DeadThread:       14
	Hosted Runtime:   no
	# and a bunch more detail

```

As previously checked the dll is Olymel.AP.Api.dll apparently use bpmd with the dll name AND the method we want a breakpoint on.
I assume the FULL namespace name:
Olymel.AP.Api.Controllers.ImportController.Synchronize

```
	(lldb) bpmd Olymel.AP.Api.dll Olymel.AP.Api.Controllers.ImportController.Synchronize
	Adding pending breakpoints... 
```
	FAILED... certainly NOT stopping at the breakpoint!
	
	Wait ... go back to the the process list:
	
	# THIS STEERED ME WRONG! We need to attach the Olymel.AP.API.dll!
```
	$ ps ax | grep dotnet 
```
	
```
	$ ps ax | grep Olymel.AP.Api
	224 ?        SLl    0:05 /tmp/working/apps/ap/ap.api/bin/container/Debug/netcoreapp3.0/Olymel.AP.Api --urls http://0.0.0.0:8017
```
	
```
	lldb
	(lldb) plugin load /root/.dotnet/sos/libsosplugin.so
	(lldb) process attach -p 224
	(lldb) bpmd Olymel.AP.Api.dll Olymel.AP.Api.Controllers.ImportController.Synchronize
	MethodDesc = 00007F53C8436BE0
	Setting breakpoint: breakpoint set --address 0x00007F53C9836E27 [Olymel.AP.Api.Controllers.ImportController.Synchronize()]
	Setting breakpoint: breakpoint set --address 0x00007F53C9836E3E [Olymel.AP.Api.Controllers.ImportController.Synchronize()]
	Adding pending breakpoints...
```	

It hit the breakpoint. EUREKA!!!

Some commands that would seem usefull:

https://www.nesono.com/sites/default/files/lldb%20cheat%20sheet.pdf

So what all of that REALLY TOLD ME! YOU NEED TO CHOOSE THE PROCESS RUNNING THE dll!!!


 


