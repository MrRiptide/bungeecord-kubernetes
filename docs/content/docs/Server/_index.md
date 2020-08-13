---
title: "The Minecraft Server"
linkTitle: "The Server"
weight: 2
date: 2020-08-07
description: >
  This page walks you through the configuration of the actual Minecraft server.
---
## The Deployment

The deployment is designed to create one pod holding a minecraft server inside. It utilizes [itzg's minecraft server docker image](https://hub.docker.com/r/itzg/minecraft-server/). Full documentation for the image can be found [on the GitHub page](https://github.com/itzg/docker-minecraft-server/blob/master/README.md).

### The YAML

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: paper
  labels:
    app: paper
spec:
  selector:
    matchLabels:
      app: paper
  template:
    metadata:
      labels:
        app: paper
    spec:
      volumes:
        - name: paper # specifies the internal name of the volume
          persistentVolumeClaim:
            claimName: minecraft-data-pvc # specifies the persistentVolumeClaim to use, more information futher down on that
      containers:
      - name: paper
        image: itzg/minecraft-server # specifies which image to use
        tty: true
        stdin: true
        env: # a collection of environment variables to pass through
          - name: EULA
            value: "TRUE"
          - name: ONLINE_MODE
            value: "FALSE"
          - name: TYPE
            value: "PAPER"
        volumeMounts:
          - name: paper # specifies what volume to use
            mountPath: /data # specifies where in the container the volume will be mounted
```

## The Service

The service built in deployments.yaml allows for the bungeecord server to interact with the minecraft server. The name of the service defines a static address for the bungeecord server to look for the minecraft server at, which you can set in the bungeecord config.

### The YAML

```
apiVersion: v1
kind: Service
metadata:
  name: paper # specifies the name of the service
spec:
  ports:
  - port: 25565 # specifies the port of the service
  selector:
    app: paper # specifies what pods should be connected to the service
```

## The Persistent Volume

In order to be able to make sure the data from the world is persistent, a Persistent Volume is required to store files in for the next time a server is spun up. You request a persistent volume with a PersistentVolumeClaim, in which you can specify various settings for the persistent volume.

### The YAML

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: minecraft-data-pvc # specifies the name of the PersistentVolumeClaim
spec:
  storageClassName: standard # the type of storage to get, may be different on different hosts
  accessModes:
    - ReadWriteOnce # means that the server will read from the storage once on boot and then write once on shutdown
  resources:
    requests:
      storage: 10Gi # specifies how much storage to claim
```
