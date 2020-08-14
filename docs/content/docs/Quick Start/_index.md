---
title: "Quickstart"
linkTitle: "Quickstart"
weight: 2
date: 2020-08-14
description: >
  This page walks you through the basic installation of bungeecord within kubernetes.
---
## Prerequisites

### Kubernetes

You will need an recent version of [Kubernetes](https://kubernetes.io/) to use this project. This quickstart is written specifically for [Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine), however it should work on most other hosting providers with minor changes.

{{% alert title="🛑 STOP 🛑" color="warning" %}}
The rest of this quickstart is under the assumption that you have an open command line connected to your kubernetes cluster. Make sure you can connect before continuing.
{{% /alert %}}


## Setting it up for a single survival server

Download [deployments.yml](https://github.com/MrRiptide/bungeecord-kubernetes/blob/master/deployments.yaml) from the GitHub repo into the directory your command line is currently in.
```
wget https://raw.githubusercontent.com/MrRiptide/bungeecord-kubernetes/master/deployments.yaml
```
Apply the yaml definitions to your kubernetes cluster
```
kubectl apply -f deployments.yaml
```

### Connecting to the server

Use the External IP address of the bungeecord load balancer as the server's IP address inside your Minecraft client. You can obtain the External IP address with the following command under ```EXTERNAL-IP```:
```
kubectl get service/bungee-lb
```

### Adding the server to bungeecord

Even though the servers are running, you will need to add the minecraft server to the bungeecord config. Edit the ```config.yml``` within the bungeecord server by first finding the name of the bungee pod:
```
kubectl get pods
```
The bungee pod will follow the pattern ```pod/bungeecord-XXXXXXXXXX-XXXXX```.

Now access the pod through:

```
kubectl exec --stdin --tty pod/<pod name> -- /bin/bash
```

And open ```config.yml``` for editing with:

```
vi config.yml
```
*To edit files though vi, press i to insert text*

Then change the address for the ```lobby``` server to ```paper.default.svg.cluster.local:25565```

*To save the file, press ```ESC``` and then ```wq``` and ```Enter``` to write and quit*

Then ```logout``` of the bungee pod and reboot it with:

```
kubectl exec pod/<pod name> -- rcon-cli end
```

Congratulations, you should now have a bungeecord Minecraft server running in your kubernetes cluster. By using this server, you agree to the [Minecraft EULA](https://www.minecraft.net/en-us/eula/).
