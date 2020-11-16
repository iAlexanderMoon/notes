# Docker Development - Dotnet core runner debug cli

## Abstract

In the Dockerfile for the dotnet runner the following lines were included:

```dockerfile
## Command line debugging
RUN dotnet tool install --tool-path /usr/local/bin dotnet-sos
RUN dotnet-sos install
```

## Command line Debugging

These instructions are for debugging on the container..

### On the Docker Host

Configure YAMA PTrace:
```sh
$ echo 0 | sudo tee /proc/sys/kernel/yama/ptrace_scope
```

_Yama Yama is a Linux Security Module and setting it to mode 0: classic ptrace permissions: a process can PTRACE_ATTACH to any other process running under the same uid_


Docker Exec bin/bash in the running container.
```sh
$ docker exec -it {containername} /bin/bash
```

### On the Docker Runner Container

#### Confirm paths of sos installation

Expected results is a list of files for the version of dotnet being used.

```sh
$ find / -name *sos*
```

_Note: /root/.dotnet/sos/libsosplugin.so_

#### Confirm lldb installed

_lldb must also be installed. This could come from your base image or installed in the Dockerfile definition._

```sh
$ whereis lldb    
```
Expected result path to lldb.

#### Run lldb

```sh
$ lldb
(lldb) help
(lldb) soshelp
(lldb) quit
```

#### Confirm YAMA Mode 0

Expected Result: 0

```sh
$ cat /proc/sys/kernel/yama/ptrace_scope
0
```

## Getting started with lldb

### Confirm PID to attach

If the container is using dotnet run then PID 1 SHOULD be the dll. Intended for debugging. However, in development dotnet watch is likely PID 1, in which case the PID. In the case below assuming Project.Api is the dll name, PID 2682 needed.

```sh
$ ps ax
PID TTY      STAT   TIME COMMAND
      1 ?        SLsl   0:07 dotnet watch run --urls http://0.0.0.0:8010
     23 ?        SLl    0:00 /usr/share/dotnet/dotnet /usr/share/dotnet/sdk/3.1.100/DotnetTools/dotnet-watch/3.1.0-rtm.19566.1/tools/netcoreapp3.1/any/dotnet-watch.dll run --urls http://0.0.0.0:801
     77 ?        Z      0:03 [dotnet] <defunct>
   2342 ?        Z      0:04 [dotnet] <defunct>
   2430 ?        Z      0:08 [dotnet] <defunct>
   2600 ?        SLl    0:04 /usr/share/dotnet/dotnet run --urls http://0.0.0.0:8010
   2682 ?        SLl    0:17 /tmp/working/apps/rms/rms.api/bin/container/Debug/netcoreapp3.1/Project.Api --urls http://0.0.0.0:8010
  13256 pts/0    Ss     0:00 /bin/bash
  14161 pts/0    R+     0:00 ps ax
```

Alternatively

```sh
$ ps ax | grep Project.Api
PID TTY      STAT   TIME COMMAND
   2682 ?        SLl    0:17 /tmp/working/apps/rms/rms.api/bin/container/Debug/netcoreapp3.1/Project.Api --urls http://0.0.0.0:8010  14456 pts/0    S+     0:00 grep Project.Api
```

### Load sos and attach to PID

Using 2682

```sh
$ lldb
(lldb) plugin load /root/.dotnet/sos/libsosplugin.so
(lldb) process attach -p 2682
```
_Note: error: attach failed: Operation not permitted, revisit security settings above for yama mode 0._

### Add Breakpoint

```sh
(lldb) bpmd Project.Api.dll Project.Api.Controllers.Values.Get
Adding pending breakpoints
(lldb)
```

On Stop

```sh
(lldb) continue
```

### List breakpoints and clear breakpoints

```sh
(lldb) bpmd -list
(lldb) bpmd -clear 1
(lldb) bpmd -clearall
```

### More

That's the extent of what I know... search for other articles on what do do next from here. Otherwise, do it from your IDE.

## References

* https://codeblog.dotsandbrackets.com/command-line-debugging-core-linux/
* https://github.com/dotnet/diagnostics/issues/550
* https://github.com/dotnet/diagnostics/blob/master/documentation/installing-sos-instructions.md
* https://github.com/dotnet/diagnostics/blob/master/documentation/sos-debugging-extension.md
* https://github.com/microsoft/MIEngine/wiki/Troubleshoot-attaching-to-processes-using-GDB
* https://www.kernel.org/doc/Documentation/security/Yama.txt
* https://www.nesono.com/sites/default/files/lldb%20cheat%20sheet.pdf
