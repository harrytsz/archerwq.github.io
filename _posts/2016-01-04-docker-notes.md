---
layout: post
title:  Docker学习笔记
date:   2016-01-04
banner_image: docker.png
tags: [Docker]
---

我理解的Docker是一个容器，把应用和其所依赖的运行环境打包成一个镜像，可以方便快速地把你的应用在各种环境上deploy起来，并保证一致性，省去配置环境的痛苦。  

<!--more-->

Docker基于操作系统之上，是进程级别的隔离，比虚机更轻量级。  
![docker vs. vm](/images/posts/docker_vs_vm.png)

Docker包括两部分:  

* Docker - the open source container virtualization platform.
* Docker Hub - SaaS platform for sharing and managing Docker containers.


#### Docker架构
![docker architecture](/images/posts/docker_architecture.svg)  

> Docker uses a client-server architecture. The Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing your Docker containers. Both the Docker client and the daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon. The Docker client and daemon communicate via sockets or through a RESTful API.  

##### Docker镜像
> A Docker image is a read-only template. For example, an image could contain an Ubuntu operating system with Apache and your web application installed. Images are used to create Docker containers. Docker provides a simple way to build new images or update existing images, or you can download Docker images that other people have already created. Docker images are the build component of Docker.

##### Docker容器
> A container consists of an operating system, user-added files, and meta-data. As we’ve seen, each container is built from an image. That image tells Docker what the container holds, what process to run when the container is launched, and a variety of other configuration data. The Docker image is read-only. When Docker runs a container from an image, it adds a read-write layer on top of the image (using a union file system as we saw earlier) in which your application can then run.

可以把镜像理解为代码，容器理解为一个进程，一个是静态表示，一个动态实例，这样一个镜像可以生成多个容器就不难理解了。Docker image采用一种叫Union File System存储，每个镜像像洋葱一样包括很多层，一般最里层是基础镜像，比如ubuntu, dedora, centos, debain之类的，然后我们用一些指令对基础镜像进行修改，比如运行一个命令、增加一个文件或目录、加个环境变量、指定以当前镜像起一个容器时要第一个运行哪个进程，等等，每个指令都在基础镜像上新增一层，每层实际上是文件集合，包含指向上一层的指针(层特别像Git的commit), 这些指令一般保存在一个叫做Dockerfile的文件里，Docker基于这个文件创建镜像。

![docker image layers](/images/posts/docker_image_layers.png)

#### Docker常用基础命令

##### 基本信息
**`docker version`**

// Display system-wide information  
**`docker info`**  

// List all the avaiable commands  
**`docker --help`**  

// List all the information related to docker run command  
**`docker run -help`**  

##### 容器相关
// 创建一个处于停止状态的容器  
**`docker create ubuntu:14.04`**  

// List all the running container  
**`docker ps`**  
* **`-a`** will list all the containers including the stopped 
 
// 创建一个交互型容器  
**`docker run -i -t --name=inspect_shell ubuntu /bin/bash`**  

* **--name** 给容器起个名字

// 创建一个后台容器  
**`docker run -d ubuntu:14.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"`**  
* **`-d`** tells Docker to run the container and put it in the background, to daemonize it

// 起一个web应用，容器端口5000映射到宿主机的80端口    
**`docker run -d -p 80:5000 training/webapp python app.py`**  

// 把宿主机目录(/src/webapp) mount到容器指定目录(/opt/webapp)  
**`docker run -d -P --name web -v /src/webapp:/opt/webapp training/webapp python app.py`**  
* **`-P`** Docker会在宿主机上随机为容器应用分配端口(49000~49900)并将其映射到溶剂开放的端口  

// 查看容器日志  
**`docker logs -f insane_babbage`**  
* **`-f`** 只看最后几行，类似`tail -f`   

// 查看容器进行信息  
**`docker top nostalgic_morse`**  

// 查看容器详细配置信息    
**`docker inspect nostalgic_morse`**  

// 停止容器  
**`docker stop insane_babbage`**  

// 删除容器  
**`docker rm nostalgic_morse`**  

// 删除所有容器
**`docker rm $(docker ps -aq)`**

##### 镜像相关
// List all the images you have locally on our host  
**`docker images`**  

// Pull image from Docker Hub  
**`docker pull centos`**  

// Create a new image from a container's changes  
**`docker commit -m="MySQL" -author="archerwq" 0ddf83b837fe archerwq/mysql:v1`**
* `0ddf83b837fe`: container ID
* `archerwq/mysql:v1`: repository:tag 


// Build an image from a Dockerfile  
**`docker build -t archerwq/test:v1 ./`**  
* **`-t archerwq/test:v1`**: image Repository:tag
* **`./`**: Dockerfile location

// Add tag to an existing image  
**`docker tag 5db5f8471261 ouruser/sinatra:devel`**  

// Push an image to Docker Hub  
**`docker push ouruser/sinatra`**  

// Remove an image from local host  
**`docker rmi training/sinatra`**

// Remove all the images  
**`docker rim $(docker images -q) `**  


##### Docker资料

* [Tutorial by Example](https://docs.docker.com/engine/userguide/dockerizing/)
* [Best Practices for Writing Dockerfile](https://docs.docker.com/engine/articles/dockerfile_best-practices/)
* [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)


