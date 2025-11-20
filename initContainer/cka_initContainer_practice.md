# Kubernetes Init Container Practice

## Overview
This project demonstrates a Kubernetes Deployment using an init container. Init containers run **before the main containers** and must complete successfully before the main container starts.

## Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: merodeployment
  labels:
    app: merodeployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: merodeployment
  template:
    metadata:
      labels:
        app: merodeployment
    spec:
      initContainers:
      - name: init-myservice
        image: busybox
        command: ["sh","-c","echo initializing....... && sleep 5"]
      containers:
      - name: nginx
        image: nginx



Apply Deployment



kubectl apply -f initContainerdeploy.yaml
kubectl get pods -w



How it Works

Kubernetes schedules a pod.

The init container init-myservice runs first:

Prints initializing.......

Sleeps for 5 seconds

Once the init container completes successfully, the main container nginx starts.

Repeat for all replicas (3 pods in this example).