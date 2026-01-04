+++
date = "2020-11-10"
title = "Docker"
slug = "docker"
tags = []
categories = []
+++

![docker art](/images/docker_art.png)

{{< rawhtml >}}

<p style="font-size:18%; color: #8f8f8f; margin: 0;">Photo credit to Medium.com</p>
{{< /rawhtml >}}

Docker makes a process with unique environment requirments run anywhere.

It's built on the Linux kernal. When the host kernal is Windows or Unix, Docker can't interact with it directly. To run on Windows or Mac (Unix kernal), Docker creates a Linux VM and runs containers in that VM.

To run Docker on Mac specifically, the newest and best option is to get Docker Desktop for Mac. Docker Desktop for Mac uses HyperKit (virtualization software) to create a Linux OS. Then Docker runs inside of that Linux OS.

Docker acheives its sandbox effect using namespaces. Operating systems have process numbers starting with 1. Each process that opens gets a subsequent number. No two processes can have the same number (hence namespace). So the Docker process actually has a process number of say 6 on the host OS. But Docker creates a new namespace for itself and assigns its own process number 1. Therefore achieving a kind of sandbox. Everything that dockers needs to do, like create containers, networks, mounts, and etc, use separate namespaces to isolate themselves from the host OS.

## Container

![docker container](/images/docker_container.jpeg)

A process on your machine that is isolated from all other processes.

Containers are not like VMs in that they don't just run on standby, they are not meant to run full operating systems. Containers are environments for running processes. Therefore a container only lives as long as the process inside it is alive. When the process finishes, the container exits.

## Container Filesystem

It uses various layers from its image for its filesystem. However, each container gets a scratch space to add/update/remove files. This scratch space is isolated in each container, even containers built from the same image. When the container exits, all files and changes made in the scratch space are gone.

Docker Volumes allow you to connect the filesystem of the host OS filesystem with the filesystem of a container. **named volumes** are like buckets of data that are saved in a docker controlled directory close to root of the Linux OS that Docker is running on. If the host OS is Linux, **named volumes** can be accessed by the host OS CLI. But if the host OS is Windows or Mac, **named volumes** cannot be accessed by the host OS CLI, instead you would need to enter the Linux virtual machine to access them outside a container. **bind mounts** are another type of volume and way to persist data in docker, and are preferable to **named volumes** in most cases. With **bind mounts** you can control where the data sits on the host. For example, if your project is in `/home/github/my_project`, you can simply mount that directory into a container using the `-v` flag to be able to access and edit `my_project` code in a container.

## Dockerfile and Image

{{< rawhtml >}}

<p style="font-size: 130%; font-weight:600; color: #3489eb; line-height: 1.2em; padding-bottom:1.5%;">
A Dockerfile is a set of instructions for building an image. When an image is built it can then be run. Running an image creates a container. The Dockerfile is composed of commands that create image layers like this:
</p>
{{< /rawhtml >}}

- OS
- source repos
- depedencies
- python dependencies
- copy source to /opt
- run web server command

A Dockerfile must be built off a previous image. That is why there is a default base image for docker: its Alpine (a lightweight Linux distro). Interestingly, the Dockerfile is not included in the image that made it. The best way to get more information about an image is docker inspect <image_name>.

Here is what a minimal Dockerfile looks like.

```
FROM Ubuntu

RUN apt-get update
RUN apt-get install python

COPY . .

ENTRYPOINT ["sleep"]

WORKDIR /home/work

CMD ["5"]

```

FROM

- Specify the image to build off of.

ENTRYPOINT

- When you **docker run** the container, you can pass in a parameter, which will be added the command you set as the entrypoint. For example if the entrypoint is: **ENTRYPOINT ["sleep"]** and you run **docker run ubuntu-sleeper 10**, the ubuntu-sleeper container will run the sleep command for 10 seconds, and then exit.

CMD

- The process that is run in the container. When this process is finished, the container exits. If your Dockerfile has **CMD ["sleep", "5"]** but you run **docker run ubunutu-sleeper sleep 10** the contianer will run sleep for 10 seconds, then exit, becuase the command you pass in overides the CMD in the Dockerfile.

## docker-compose

docker-compose is a command line tool that reads a file called **docker-compose.yml** and has only two commands `docker-compose up` and `docker-compose down`. docker-compose makes the process of building, running, and linking multiple containers easier. This might be done to create different containers for parts of a single application (one container for the frontend, and one for the sql database). However, docker-compose can also be used to start a single container. docker-compose runs all the containers with their specificed parameters in docker-compose.yml and connects them so they can access each other.

**docker-compose.yml**

```sh
version: 3
services:
redis:
image: my_redis_image
db:
image: postgres:9.4
vote:
image: voting-app
ports:
-5000:80
```

Run containers in **docker-compose.yml**

```sh
docker-compose up
```

Exit and remove containers in **docker-compose.yml**

```sh
docker-compose down
```

## .dockerignore

While building an image from a Dockerfile, the Docker client will send everything the entire directory the Dockerfile is in to the Docker daemon. So if your project has a lot of data, use **.dockerignore** to tell the Docker client what not to send to the daemon. This will speed up image building significantly for large projects.

{{< rawhtml >}}
<br>
<br>
{{< /rawhtml >}}

# Commands

{{< rawhtml >}}
<br>
{{< /rawhtml >}}

### Basics

Run the container in the background **-d** and map port **-p** 80 of the host to 80 in the container. Without **-d** you won't get your prompt back and STDOUT of the container will be printed to the terminal.

```sh
docker run -dp 80:80 <image_name>
```

Run a container based off the ubuntu image. If no **ubuntu** image exists locally, docker will pull it from Docker Hub. Specifically, Docker will look for **uhbuntu/ubuntu:latest** when **ubuntu** is passed in. No tag is specified, so docker will use the version of ubuntu tagged with **latest** on Docker Hub.

```sh
docker run -d ubuntu
```

Run a container based off the verison of ubuntu tagged with **11.2.0**.

```sh
docker run -d ubuntu:11.2.0
```

Run a container in interactive mode **-i**. By default, containers dont listen to STDIN. In interactive mode, they will. **-t** attaches to the container's terminal. With both of these flags, STDIN and STDOUT will flow through the container.

```sh
docker run -it ubuntu ls /
```

List containers that are running and because **-a** exited.

```sh
docker ps -a
```

Stop a running container.

```sh
docker stop <container_id>
```

Remove a container.

```sh
docker rm <container_id>
```

Remove all stopped containers and cached image layers, and dangling images (all things you dont need).

```sh
docker system prune
```

Execute a command in a running container.

```sh
docker exec file_in_container < container_id > cat
```

### Image

List images.

```sh
docker image list
```

List dangling images. Dangling images are layers that have no relationship to any tagged iamges. They consume disk space, so its usually better to remove them. **-f** is for filter.

```sh
docker images -f dangling=true
```

Remove an image.

```sh
docker rmi <image_name>
```

Tag an image. Tagging an image doesnt rename an image, it creates a copy of the image and give it the new name.

```sh
docker tag <source_image_name>:<tag> <target_image_name>:<tag>
```

Build an iamge off of the Dockerfile in the current directory `.` and tag it `-t` with `image_name`.

```sh
docker build -t < image_name > .
```

See image layer information.

```sh
docker history <image_name>
```

### Registry

The predominant registry for docker images is [Docker Hub](https://hub.docker.com).

![docker registry](/images/docker_registry.png)

{{< rawhtml >}}

<p style="font-size:18%; color: #8f8f8f; margin: 0;"> Image credit to IT'zGeek</p>
{{< /rawhtml >}}

Login to Docker Hub.

```sh
docker login -u <user_name>
```

Pull image. It will pull from Docker Hub unless a different registry is specified. If you dont specifiy a username, docker will assume the username is the same as the image name. So these two commands will do the same thing:

```sh
docker pull ubuntu

docker pull ubuntu/ubuntu:latest
```

Pull image from a different registry.

```sh
docker pull myregistry.local:5000/testing/test-image
```

Push image. It will push to Docker Hub unless a different registry is specified.

```sh
docker push katharineme/myimage
```

### Volume

Map host data to container to create a **bind-mount**. When the container is running, the data will be accessible by the container at the mapped location. The container will listen to changes made the data and changes made to the data in the container will be saved on the host.

```sh
docker run -v </host/data/dir>:</container/dir> <image_name>
```

To move a file or directory on the host into a running container (in a more one-off fashion) you can use the `cp` command.

```sh
docker cp /Downloads/file.txt <container_name>:<container_directory>

docker cp /Downloads/file.txt sad_burnell:/home/
```

Create a **named volume**. named volumes are stored in a directory in the virtual machine docker is running. So they arent accessible in the host CLI.

```sh
docker volume create <volume_name>
```

List named volumes.

```sh
docker volume ls
```

### Container Status

See container details.

```sh
docker inspect <container_name>
```

See container logs (STDOUT).

```sh
docker logs <container_name>
```

### Container Power

Give a container 50% of host CPU

```sh
docker run --cpus=.5 ubuntu
```

Give a container 100Mb of host Memory

```sh
docker run --memory=100m ubuntu
```

### Environment Variables

Run container with environment variable. See a containers current environment variables by running `docker inspect`.

```sh
docker run -e APP_COLOR=blue <image_name>
```

## More

[Best Video Docker tutorial](https://www.youtube.com/watch?v=zJ6WbK9zFpI&t=1s)
