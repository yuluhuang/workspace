# base· flow(mac)
[参考](https://yeasy.gitbooks.io/docker_practice/content/container/run.html)

1. 下载[toolbox](https://www.docker.com/toolbox)
2. `docker login`
3. `docker run -it ubuntu /bin/bash`
4. 更新包
    > `apt-get update`

    > `apt-get install idle`
    
    > `apt-get install python-setuptools ptyhon-pip`
    
5. 创建镜像

    `docker commit -m "Added python" -a "Docker name" 0b2616b0e5a8 username/upy[:tag]`
    >其中，-m 来指定提交的说明信息，跟我们使用的版本控制工具一样；-a 可以指定更新的用户信息；之后是用来创建镜像的容器的 ID；最后指定目标镜像的仓库名和 tag 信息。创建成功后会返回这个镜像的 ID 信息。
    
6.运行新创建的镜像

`docker run -it username/upy /bin/bash`


##Docker 包括三個基本概念

>镜像(Image)like ubuntu

>容器(Container)like to run image [运行后的镜像就是一个容器]

>倉庫(Repository) push or pull image



##基本命令
1. `docker images`
2. `docker run`
3. 


##利用 Dockerfile 来创建镜像
        # This is a comment
        FROM ubuntu:latest
        MAINTAINER Docker username <email>
        RUN apt-get  update
        RUN apt-get idle
        RUN apt-get python-setuptools ptyhon-pip
        
        
###Dockerfile 基本的语法是

>使用#来注释

>FROM 指令告诉 Docker 使用哪个镜像作为基础

>接着是维护者的信息

>RUN开头的指令会在创建中运行，比如安装一个软件包，
在这里使用 apt-get 来安装了一些软件

>编写完成 Dockerfile 后可以使用 docker build 来生成镜像。

运行 `docker build -t="username/upy[:tag]" .`

>其中 -t 标记来添加 tag，指定新的镜像的用户信息。

>“.” 是 Dockerfile 所在的路径（当前目录），
也可以替换为一个具体的 Dockerfile 的路径。


`*注意一个镜像不能超过 127 层`


##存出和载入镜像

存出镜像

如果要导出镜像到本地文件，可以使用 docker save 命令。

`docker images`

REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
ubuntu              14.04               c4ff7513909d        5 weeks ago         225.4 MB
...

`docker save -o ubuntu_14.04.tar ubuntu:14.04`

载入镜像

可以使用 docker load 从导出的本地文件中再导入到本地镜像库，例如

` docker load --input ubuntu_14.04.tar`

或

`docker load < ubuntu_14.04.tar`

这将导入镜像以及其相关的元数据信息（包括标签等）。



##持续集成 work flow
