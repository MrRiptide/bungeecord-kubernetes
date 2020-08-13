---
title: "Adding and Configuring Plugins"
linkTitle: "Plugins"
weight: 3
date: 2020-08-10
description: >
  This page describes how to add and configure plugins.
---
## Adding Plugins
There are two techniques for adding plugins to the server. After adding a plugin, make sure to restart the server in order for the plugin to be loaded.

### Finding the pod name
In order to add plugins to the server, you must first know the name of the pod that the server is running in. You can find this by running:
```
kubectl get pods
```
Then find the pod name that follows the pattern ```pod/paper-XXXXXX```.

### Adding from inside the pod
*This method may not work with plugins downloaded directly from the bukkit or spigot websites*

In order to add a plugin from inside the pod, you must first get inside the pod running the server:
```
kubectl --stdin --tty pod/<pod-name> -- /bin/bash
```

Once you are inside the pod, navigate to the plugins directory. If you used the provided setup, it should be accessible through:
```
cd /data/plugins/
```

Now that you're in the plugins directory, find the link to the plugin download, and download it into the container by using:
```
wget <plugin-url>
```

If the ```wget``` fails, the site you are downloading from may have blocked automatic downloads, in which case, try the next method. However, if it worked, restart your server, and the plugin will load in.

### Adding from outside the pod
Before continuing, be sure to download the plugin from your prefered source. Navigate in your command line to the directory where the plugin was saved in:
```
cd <path>
```
Now that you're in the same directory as the plugin, move the plugin jar into the container:
```
kubectl cp <plugin-file-name> <pod-name>:/data/plugins
```
Now restart the server and the plugin should be loaded in!

## Configuring Plugins
Some plugins have varying levels of configuration that needs to be edited in order to get the gameplay you are looking for.

### Editing Configuration
To be able to edit configuration files, you will need to get inside the pod.
```
kubectl exec --stdin --tty pod/<pod-name> -- /bin/bash
```

Once you're inside the pod, you will need to navigate to the plugins directory

```
cd /data/plugins
```

Most plugins will have their configuration in their own folder within the plugins directory.

#### Useful Commands
|Command               |Usage                                               |
|----------------------|----------------------------------------------------|
|```ls```              |List all files and subdirectories within a directory|
|```ls -R```           |List all files and subdirectories recursively       |
|```nano <file-name>```|Edit a file                                         |
