---
title: "Interacting With the Server"
linkTitle: "Interacting"
weight: 2
date: 2020-08-10
description: >
  This page describes how to interact with the server while it is running.
---

## Accessing logs
Accessing the logs is a very important part of running a Minecraft server.

### Finding the pod name
In order to access the logs of the server, you must first know the name of the pod that the server is running in. You can find this by running:
```
kubectl get pods
```
Then find the pod name that follows the pattern ```pod/paper-XXXXXX```.

### Accessing the pod's logs
Luckily, the image we're using logs the minecraft logs into the pod's logs, so all we have to do is view the logs for the pod itself:
```
kubectl logs pod/<your-pod-name>
```

## Sending console commands
By default, no users are given any permissions within the server. If you want to give yourself OP permissions or run any command with console permissions, you'll first need to be able to access the console.

### Finding the pod name
In order to send commands to the server, you must first know the name of the pod that the server is running in. You can find this by running:
```
kubectl get pods
```
Then find the pod name that follows the pattern ```pod/paper-XXXXXX```.

### Sending single commands
The image used in this example has rcon enabled by default, so you can send commands to the server through a simple ```exec``` command:
```
kubectl exec pod/<your-pod-name> -- rcon-cli <your-command>
```

### Sending multiple commands
If you want to access the console to send multiple commands, all you have to do is access rcon interactively:
```
kubectl exec -i pod/<your-pod-name> -- rcon-cli
```
