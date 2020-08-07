---
title: "The Minecraft Server"
linkTitle: "The Server"
weight: 2
date: 2020-08-07
description: >
  This page walks you through the configuration of the actual Minecraft server.
---
## The Deployment



```

apiVersion: v1
kind: Service
metadata:
  name: paper
spec:
  ports:
  - port: 25565
  selector:
    app: paper
---
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
        - name: paper
          persistentVolumeClaim:
            claimName: minecraft-data-pvc
      containers:
      - name: paper
        image: itzg/minecraft-server
        tty: true
        stdin: true
        env:
          - name: EULA
            value: "TRUE"
          - name: ONLINE_MODE
            value: "FALSE"
          - name: TYPE
            value: "PAPER"
        volumeMounts:
          - name: paper
            mountPath: /data
```
