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

On Mac, we can use GUI installer which is much easier.

## Problem 1

- Open up your terminal
- Create a new directory and `cd` into it.
- Create a file called `Dockerfile` and open it in your favorite editor.

*These questions should be viewed as guiding questions, you don't need to submit answers for these*

Inherit from the standard `ubuntu` image on dockerhub. (Why do we do this?)

You can build and test your Dockerfile by running these commands:

```shell
$ docker build -t <container name> .
```

Now, you have a container named whatever you specified. Try running.

```shell
$ docker run -i -t <container name>
```

If you see something like:

```shell
$ root@8a788562c667:/# 
```

You've been launched into bash, and you have made your first container!

## Problem 2

考察点是`docker label`属性，可以参考[文档](https://docs.docker.com/engine/reference/builder/#label)

只需要在`Dockerfile`里面添加：

```dockerfile
LABEL "NETID"="123456"
LABEL "CLASS"="CS199"
```

然后运行：

```shell
$ docker build -t problem2 .
$ docker run -i -t problem2:latest /bin/bash
```

新开一个终端，运行：

```shell
$ docker inspect problem2
[
    {
        "Id": "sha256:e41affe881bcf7fc1015a0503a314a6de6235c8e68842d6d50f3ef478100d952",
        "RepoTags": [
            "problem2:latest"
        ],
        "RepoDigests": [],
        "Parent": "",
        "Comment": "buildkit.dockerfile.v0",
        "Created": "2021-03-25T22:33:02.159055896Z",
        "Container": "",
        "ContainerConfig": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": null,
            "Cmd": null,
            "Image": "",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "DockerVersion": "",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "CLASS": "CS199",
                "NETID": "123456"
            }
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 63257487,
        "VirtualSize": 63257487,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/907fb0499ccf78de58051f56e7a8390b040d730eafd5b87063a47286a520e3a5/diff:/var/lib/docker/overlay2/7b18afb2652dfa47162548728f157f8521d9cc1452da18983923eb3a2c2c7ed8/diff",
                "MergedDir": "/var/lib/docker/overlay2/7a2e0892cc0b0a8b4ea396fdf003c79e90bab562e60b0e708ec30d3a98959655/merged",
                "UpperDir": "/var/lib/docker/overlay2/7a2e0892cc0b0a8b4ea396fdf003c79e90bab562e60b0e708ec30d3a98959655/diff",
                "WorkDir": "/var/lib/docker/overlay2/7a2e0892cc0b0a8b4ea396fdf003c79e90bab562e60b0e708ec30d3a98959655/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:030309cad0ba82b098939419dcb5e0a95c77d2427d99c44a690ecab59f80a487",
                "sha256:1e77dd81f9fa12f3034fa1ed55bc2d1f7a316b450ae53f36beb52af2dd83b78f",
                "sha256:6f15325cc380f8fc8fa0cdffc5cc7e38c5beb155e09ab3e0edbb1e5a842c46cc"
            ]
        },
        "Metadata": {
            "LastTagTime": "2021-04-20T06:41:13.396743607Z"
        }
    }
]
```

会发现在`Labels`属性下多出了出现了想要的结果。

## Problem 3

打开docker容器并安装python3，指定版本为3.6.1。

```shell
$ sudo docker run -t -i problem3:latest /bin/bash
$ apt-get update # 更新后才可以使用，否则会显示无法找到对应的安装包
$ apt-get install make
$ apt-get install wget
$ apt-get install xz-utils # 执行tar命令时报错
$ apt-get install gcc # 否则会在执行./configure报错
$ apt-get install zlib1g-dev # 否则会在执行make install时报错：zipimport.ZipImportError: can't decompress data; zlib not available

$ wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tar.xz # 在/home/problem3下
$ tar xvf Python-3.6.1.tar.xz
$ cd Python-3.6.1
$ ./configure --prefix=/usr/local/python3
$ make && make install
$ ln -s /usr/local/python3/bin/python3.6 /usr/local/bin/python3 # 软链接
$ python3 --version # 检验安装版本是否符合要求
```

## Probelm 4

```dockerfile
FROM ubuntu:18.04
LABEL "NETID"="123456"
LABEL "CLASS"="CS199"
RUN mkdir data
RUN cd data/ && touch info.txt && echo 'Karl the fog is the best cloud computing platform' > info.txt
```

```shell
$ docker build -t problem4 .
$ sudo docker run -t -i problem4:latest /bin/bash
$ cd data/
$ cat info.txt # 检查是否符合要求
```

## Problem 5

增加一个新的用户，名为`cs199`，可以自行设置密码，并增加一个名为`NAME`的环境变量。只需要增加，参考：[How to Create Users in Linux (useradd Command)](https://linuxize.com/post/how-to-create-users-in-linux-using-the-useradd-command/)

```dockerfile
RUN useradd --create-home --no-log-init --shell /bin/bash cs199
RUN adduser cs199 sudo
RUN echo 'cs199:password' | chpasswd

USER cs199
ENV NAME yuqi
```

启动容器后通过以下命令检验：

```shell
$ whoami
cs199
$ echo $NAME
yuqi
```

-----

完整地Dockerfile：

```dockerfile
FROM ubuntu:18.04

LABEL "NETID"="123456"
LABEL "CLASS"="CS199"

RUN mkdir data
RUN cd data/ && touch info.txt && echo 'Karl the fog is the best cloud computing platform' > info.txt

RUN useradd --create-home --no-log-init --shell /bin/bash cs199
RUN adduser cs199 sudo
RUN echo 'cs199:password' | chpasswd

USER cs199
ENV NAME yuqi
```

注意problem3是在`container`内完成地，并不需要要写入Dockerfile