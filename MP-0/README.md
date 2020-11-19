> # MP-0: Introduction to Docker

## Goals

1. Inherits from the Ubuntu public image.
2. Implements labels for a maintainer and a class.
3. Installs python3.
4. Creates a few environment variables.
5. Writes to a file in the container
6. Sets the current user to root.
7. Runs bash whenever the container is run.

## Environment set up

Installing `docker` under Ubuntu 20.04 and 16.04 is successful, but failed under WSL on windows.

Firstly, read  the [official document](https://docs.docker.com/engine/install/ubuntu/) of `docker`.

```bash
$ sudo apt-get remove docker docker-engine docker.io containerd runc
$ sudo apt-get update
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$  sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io

# error occurs on WSL
$ sudo docker run hello-world
docker: Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?.
See 'docker run --help'.

# run normally under ubuntu
$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete 
Digest: sha256:8c5aeeb6a5f3ba4883347d3747a7249f491766ca1caa47e5da5dfcf6b9b717c0
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```



## Problem 1

- Open up your terminal
- Create a new directory and `cd` into it.
- Create a file called `Dockerfile` and open it in your favorite editor.

*These questions should be viewed as guiding questions, you don't need to submit answers for these*

Inherit from the standard `ubuntu` image on dockerhub. (Why do we do this?)

You can build and test your Dockerfile by running these commands:

```
$ docker build -t <container name> .
```

Now, you have a container named whatever you specified. Try running.

```
$ docker run -i -t <container name>
```

If you see something like:

```
root@8a788562c667:/# 
```

You've been launched into bash, and you have made your first container!