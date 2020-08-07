---
title: "Quickstart"
linkTitle: "Quickstart"
weight: 2
date: 2020-08-07
description: >
  This page walks you through the basic installation of bungeecord within kubernetes.
---
## Prerequisites

### Kubernetes

You will need an recent version of [Kubernetes](https://kubernetes.io/) to use this project. This quickstart is written specifically for [Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine), however it should work on most other hosting providers with minor changes.

The rest of this quickstart is under the assumption that you have an open command line connected to your kubernetes cluster.

## Setting it up for a single survival server

Download [deployments.yml](https://github.com/MrRiptide/bungeecord-kubernetes/blob/master/deployments.yaml) from the GitHub repo into the directory your command line is currently in.
```
wget https://raw.githubusercontent.com/MrRiptide/bungeecord-kubernetes/master/deployments.yaml
```
Apply the yaml definitions to your kubernetes cluster
```
kubectl apply -f deployments.yaml
```
Congratulations, you should now have a bungeecord Minecraft server running in your kubernetes cluster.

### Connecting to the server

Use the External IP address of the bungeecord load balancer as the server's IP address inside your Minecraft client. You can obtain the External IP address with the following command under "External IP"
```
kubectl get service/bungee-lb
```
