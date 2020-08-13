---
title: "Configuring Bungeecord"
linkTitle: "Configuring"
weight: 1
date: 2020-08-13
description: >
  This page describes how to configure your Bungeecord server.
---

## Environment Variables
There are several different environment variables that can be adjusted to configure your deployment properly. Any additions should be made in the section labeled ```<changes>```:

```
containers:
- name: bungeecord
  image: itzg/bungeecord
  tty: true
  stdin: true
  env:
    <changes>
  volumeMounts:
    - name: bungee-data
      mountPath: /server
  ports:
    - containerPort: 25565
```

### Choosing a Server Type
There are two types of servers you can use by default. To pick one, set the ```TYPE``` environment variable to ```BUNGEE``` or ```WATERFALL```. For example, if you want to run ```WATERFALL```:

```
- name: TYPE
  value: "WATERFALL"
```

## Config files

After the container is running, there may still be a few changes you want to make to the server files.

In order change configuration of the server, you must first know the name of the pod that the server is running in. You can find this by running:
```
kubectl get pods
```
Then find the pod name that follows the pattern ```pod/bungeecord-XXXXXX```.

Now that you have the pod's name, get inside the pod running the server:
```
kubectl exec --stdin --tty pod/<pod-name> -- /bin/bash
```

Once you are inside the pod, navigate to the server directory. If you used the provided setup, it should be accessible through:
```
cd /server
```

Most, if not all, of the configuration for your bungeecord server will be available within that directory.

#### Useful Commands for Configuration
|Command               |Usage                                               |
|----------------------|----------------------------------------------------|
|```ls```              |List all files and subdirectories within a directory|
|```ls -R```           |List all files and subdirectories recursively       |
|```cat <file-name>``` |Print the contents of a file                        |
|```nano <file-name>```|Edit a file                                         |

### Adding a Server

If you want to add a minecraft server to the bungeecord server, you will need to edit ```config.yml```.
```
nano config.yml
```

Under ```servers:```, add a new entry:
```
<server name>:
  motd: <your motd>
  address: <service name>.default.svc.cluster.local:25565
  restricted: false
```
Set ```restricted``` to ```true``` if you want to restrict the server to only players with a certain permission.

Then log out of the pod running bungeecord with ```logout``` and reboot the bungeecord server:

```
kubectl exec pod/<pod-name> -- rcon-cli end
```
