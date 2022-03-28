---
layout: post
title:  "Useful kubectl commands"
date:   2021-03-08
desc: "This is useful kubectl commands"
keywords: "kubernetes, commands, kubectl, cloud"
categories: [Cloud]
tags: [cloud, kubernetes, kubectl, commands]
icon: icon-html
---

# Docker commands

### 1. kubectl get 

Use get to pull a list of resources you have currently on your cluster. The types of resources you can get include:

* Namespace
* Pod
* Node
* Deployment
* Service
* ReplicaSets

```
kubectl get ns
kubectl get pods -n kube-system
kubectl get nodes
kubectl get services
```

### 2. kubectl describe

Describe shows the details of the resource you're looking at.

Resources you can describe include:

* Nodes
* Pods
* Services
* Deployments
* Replica sets

```
kubectl describe pods my-pods -n kube-system

kubectl describe servcies my-services -n kube-system

```

### 3. kubectl logs

logs offer detailed insights into what's happening inside Kubernetes in relation to the pod.

```
kubectl logs my-pod -n kube-system
```

### 4. kubectl exec
exec into a container to troubleshoot an application directly.

```
kubectl exec -it my-pod -n kube-system /bin/bash
```

### 5. kubectl cp

This command is for copying files and directories to and from containers, much like the Linux cp command. The syntax follows a kubectl cp <filename> <namespace/podname:/path/tofile> format:

```
kubectl cp commands_copy.txt charts/cherry-chart-88d49478c-dmcfv:commands.txt
```


## kubectl cheat sheet 

<!-- Embed PDF File -->
 [PDF link ](../../resources/cheatsheet/kubernetes-cheat-sheet.pdf){target="_blank"}

<iframe src="../../resources/cheatsheet/kubernetes-cheat-sheet.pdf" style="width:1000px; height:800px;" frameborder="0" allowfullscreen></iframe>


## docker cheat sheet 

 [PDF link ](https://dockerlabs.collabnix.com/docker/cheatsheet/)
 
<!-- Embed PDF File -->
<iframe src="https://dockerlabs.collabnix.com/docker/cheatsheet/" style="width:1000px; height:800px;" frameborder="0" allowfullscreen></iframe>
