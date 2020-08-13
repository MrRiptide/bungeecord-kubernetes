---
title: "Configuring the Server"
linkTitle: "Configuring"
weight: 1
date: 2020-08-12
description: >
  This page describes how to configure your Minecraft server.
---

## Environment Variables
There are several different environment variables that can be adjusted to configure your deployment properly. Any additions should be made in the section of the deployment yaml labeled ```<changes>```:

```
containers:
- name: paper
image: itzg/minecraft-server
tty: true
stdin: true
env:
<changes>
volumeMounts:
- name: paper
mountPath: /data
```

### Accepting the EULA

Mojang requires accepting the [Minecraft EULA](https://www.minecraft.net/en-us/eula/) in order to run the server. In order to accept it, set the environment variable ```EULA``` in the container to ```"true"```:
```
- name: EULA
  value: "true"
```

### Choosing a Minecraft version

In order to select the version of Minecraft you want your server to be running, set the ```VERSION``` environment variable in the container to the version you would like to run.
For example, if you would like to run 1.8.9:
```
- name: VERSION
  value: "1.8.9"
```

### Choosing a server type

To select a server type, set the ```type``` environment variable in the container to the type you would like to run. Valid server types are ```SPIGOT```, ```BUKKIT```, ```PAPER```, ```TUINITY```, ```MAGMA```, ```MOHIST```, ```CATSERVER```, ```FTBA```, ```CURSEFORGE```, ```SPONGEVANILLA```, and ```FABRIC```. Several of these version have special instructions, which can be found on [itzg's documentation](https://github.com/itzg/docker-minecraft-server/blob/master/README.md#running-a-bukkitspigot-server)

For example, if you would like to use PaperSpigot(the default in deployments.yaml):
```
- name: TYPE
  value: "PAPER"
```
or Bukkit:
```
- name: TYPE
  value: "BUKKIT"
```

### Disable Online Mode

In order to use Bungeecord, the minecraft server must be in offline mode. In order to set this, set the environment variable ```ONLINE_MODE``` to ```"false"```:
```
- name: ONLINE_MODE
  value: "false"
```

## Config files

After the container is running, there may still be a few changes you want to make to the server files.

In order change configuration of the server, you must first know the name of the pod that the server is running in. You can find this by running:
```
kubectl get pods
```
Then find the pod name that follows the pattern ```pod/paper-XXXXXX```.

Now that you have the pod's name, get inside the pod running the server:
```
kubectl --stdin --tty pod/<pod-name> -- /bin/bash
```

Once you are inside the pod, navigate to the server directory. If you used the provided setup, it should be accessible through:
```
cd /data
```

Most, if not all, of the configuration for your minecraft server will be available within that directory.

#### Useful Commands for Configuration
|Command               |Usage                                               |
|----------------------|----------------------------------------------------|
|```ls```              |List all files and subdirectories within a directory|
|```ls -R```           |List all files and subdirectories recursively       |
|```cat <file-name>``` |Print the contents of a file                        |
|```nano <file-name>```|Edit a file                                         |
