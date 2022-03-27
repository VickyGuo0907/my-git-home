---
layout: post
title:  "Useful docker commands"
date:   2021-03-07
desc: "This is useful docker commands"
keywords: "docker, commands"
categories: [HTML]
tags: [docker, commands]
icon: icon-html
---

# Docker commands

## Essential Toolkit for docker

Details check [docker docs](https://docs.docker.com/engine/reference/run/)
### 1. version 

```
docker --version
``` 

### 2. download images

Pull an image or a repository from a registry.  docker pull [OPTIONS] NAME[:TAG|@DIGEST]

```
# pull from docker hub
docker pull ubuntu:14.04

# Pull from a different registry
docker pull myregistry.local:5000/testing/test-image
```

### 3. list images

List all the docker images pulled on the system with image details 

```
docker images
```

### 4. build image

Build an image from a Dockerfile. docker build [OPTIONS] PATH | URL | -

```
# Build with PATH 
docker build . 

# Tag an image with (-t) and Specify a Dockerfile with (-f)
docker build -f Dockerfile.debug -t vieux/apache:2.0 .
```

### 5. Run image

Run the docker image mentioned in the command.  docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]

```
# run detached 
docker run -d -p 80:80 my_image 
```

### 6. List containers

lists all the docker containers are running with container details.

```
docker ps

# List all the docker containers running/exited/stopped with container details.
docker ps -a
```

### 7. Access container

Access the docker container and run commands inside the container.

```
docker exec -it my_image bash
```

### 8. Removing container

Remove the docker container with container id mentioned in the command.

```
docker rm my-container
```

### 8. Removing image

Remove the docker image with the docker image id mentioned in the command.

```
docker rmi my-image
```

### 9. start Docker || Stop Docker

Start or Stop the docker container with container id mentioned in the command.

```
docker start my-container
docker stop my-container

docker restart my-container
```


## docker cheat sheet 

 [PDF link ](https://dockerlabs.collabnix.com/docker/cheatsheet/)
 
<!-- Embed PDF File -->
<iframe src="https://dockerlabs.collabnix.com/docker/cheatsheet/" style="width:1000px; height:800px;" frameborder="0" allowfullscreen></iframe>
