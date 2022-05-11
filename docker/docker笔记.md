## 1 概述

> 官网：https://docs.docker.com/

### 1.1 简释

> 容器就是将软件打包成标准化单元，以用于开发、交付和部署。

*   `容器镜像是轻量的、可执行的独立软件包` ，包含软件运行所需的所有内容：代码、运行时环境、系统工具、系统库和设置。
*   `容器化软件适用于基于 Linux 和 Windows 的应用，在任何环境中都能够始终如一地运行。`
*   `容器赋予了软件独立性`，使其免受外在环境差异（例如，开发和预演环境的差异）的影响，从而有助于减少团队间在相同基础设施上运行不同软件时的冲突。

![image-20220410222252425](http://cdn.lsbysl.com//image-20220410222252425.png)

### 1.2 Docker的优点

> Docker 是一个用于开发，交付和运行应用程序的开放平台。
>
> Docker 使您能够将应用程序与基础架构分开，从而可以快速交付软件。
>
> 借助 Docker，您可以与管理应用程序相同的方式来管理基础架构。
>
> 通过利用 Docker 的方法来快速交付，测试和部署代码，您可以大大减少编写代码和在生产环境中运行代码之间的延迟。

#### 1.2.1 快速，一致地交付您的应用程序

Docker 允许开发人员使用您提供的应用程序或服务的本地容器在标准化环境中工作，从而简化了开发的生命周期。

容器非常适合持续集成和持续交付（CI / CD）工作流程，请考虑以下示例方案：

- 您的开发人员在本地编写代码，并使用 Docker 容器与同事共享他们的工作。
- 他们使用 Docker 将其应用程序推送到测试环境中，并执行自动或手动测试。
- 当开发人员发现错误时，他们可以在开发环境中对其进行修复，然后将其重新部署到测试环境中，以进行测试和验证。
- 测试完成后，将修补程序推送给生产环境，就像将更新的镜像推送到生产环境一样简单。

#### 1.2.2 响应式部署和扩展

Docker 是基于容器的平台，允许高度可移植的工作负载。

Docker 容器可以在开发人员的本机上，数据中心的物理或虚拟机上，云服务上或混合环境中运行。

Docker 的可移植性和轻量级的特性，还可以使您轻松地完成动态管理的工作负担，并根据业务需求指示，实时扩展或拆除应用程序和服务。

#### 1.2.3 在同一硬件上运行更多工作负载

Docker 轻巧快速。它为基于虚拟机管理程序的虚拟机提供了可行、经济、高效的替代方案，因此您可以利用更多的计算能力来实现业务目标。

Docker 非常适合于高密度环境以及中小型部署，而您可以用更少的资源做更多的事情。

## 2 虚拟化

### 2.1 虚拟化技术和容器化技术

> ​			虚拟化技术是一种资源管理技术，是将计算机的各种实体资源（CPU、内存、磁盘空间、网络适配器等）,予以抽象、转换后呈现出来并可供分割、组合为一个或多个电脑配置环境。由此，打破实体结构间的不可切割的障碍，使用户可以比原本的配置更好的方式来应用这些电脑硬件资源。这些资源的新虚拟部分是不受现有资源的架设方式，地域或物理配置所限制。一般所指的虚拟化资源包括计算能力和数据存储。

### 2.2 Docker 基于 LXC 虚拟容器技术

> Docker 技术是基于 LXC（Linux container- Linux 容器）虚拟容器技术的。

> LXC，其名称来自 Linux 软件容器（Linux Containers）的缩写，一种操作系统层虚拟化（Operating system–level virtualization）技术，为 Linux 内核容器功能的一个用户空间接口。它将应用软件系统打包成一个软件容器（Container），内含应用软件本身的代码，以及所需要的操作系统核心和库。通过统一的名字空间和共用 API 来分配不同软件容器的可用硬件资源，创造出应用程序的独立沙箱运行环境，使得 Linux 用户可以容易的创建和管理系统或应用容器。

> LXC 技术主要是借助 Linux 内核中提供的 CGroup 功能和 name space 来实现的，通过 LXC 可以为软件提供一个独立的操作系统运行环境。

### 2.3 cgroup 和 namespace 介绍

> `namespace 是 Linux 内核用来隔离内核资源的方式。` 通过 namespace 可以让一些进程只能看到与自己相关的一部分资源，而另外一些进程也只能看到与它们自己相关的资源，这两拨进程根本就感觉不到对方的存在。具体的实现方式是把一个或多个进程的相关资源指定在同一个 namespace 中。Linux namespaces 是对全局系统资源的一种封装隔离，使得处于不同 namespace 的进程拥有独立的全局系统资源，改变一个 namespace 中的系统资源只会影响当前 namespace 里的进程，对其他 namespace 中的进程没有影响。

> `CGroup 是 Control Groups 的缩写，是 Linux 内核提供的一种可以限制、记录、隔离进程组 (process groups) 所使用的物力资源 (如 cpu memory i/o 等等) 的机制。`

> 两者都是将进程进行分组，但是两者的作用还是有本质区别。namespace 是为了隔离进程组之间的资源，而 cgroup 是为了对一组进程进行统一的资源监控和限制。

## 3 Docker基本组成

### 3.1 生命周期

> Docker 中有非常重要的三个基本概念，理解了这三个概念，就理解了 Docker 的整个生命周期。

* `镜像（Image）`

  ~~~
  Docker 镜像（Image），就相当于是一个 root 文件系统。
  比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。
  ~~~
* `容器（Container）`

  ~~~
  镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。
  容器可以被创建、启动、停止、删除、暂停等。
  ~~~

*   `仓库（Repository）`

  ~~~
  仓库可看成一个代码控制中心，用来保存镜像。
  ~~~

> Docker 使用客户端-服务器 (C/S) 架构模式，使用远程API来管理和创建Docker容器。
>
> Docker 容器通过 Docker 镜像来创建。
>
> 容器与镜像的关系类似于面向对象编程中的对象与类。

| Docker | 面向对象 |
| :----: | :------: |
|  容器  |   对象   |
|  镜像  |    类    |

> 理解了这三个概念，就理解了 Docker 的整个生命周期

![image-20220410222328641](http://cdn.lsbysl.com//image-20220410222328641.png)

### 3.2 底层原理

~~~
Docker 是一个Client-Server结构的系统，Server的守护进程运行在主机上。通过socket从客户端访问！ 
~~~

## 4 Docker 安装

> 环境查看命令

~~~shell
uname -r     #查看系统内核版本 内核3.10以上
cat /etc/os-release  #查看系统版本
~~~

### 4.1 卸载旧版本

~~~shell
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
~~~

### 4.2 需要安装的包

~~~shell
yum -y install gcc # gcc相关环境
yum -y install gcc-c++ # gcc相关环境
yum install -y yum-utils
~~~

### 4.3 配置镜像仓库

~~~shell
#国外的地址
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo  

# 设置阿里云的Docker镜像仓库
yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
~~~

### 4.4 更新yum软件包索引

~~~shell
yum makecache fast
~~~

### 4.5 安装docker

~~~shell
yum install -y docker-ce docker-ce-cli containerd.io   # 安装社区版
yum install -y docker-ee docker-ee-cli containerd.io   # 安装企业版
~~~

### 4.6 启动docker

~~~shell
systemctl start docker   # 启动Docker
docker version           # 查看当前版本号，是否启动成功
systemctl enable docker  # 设置开机自启动
~~~

### 4.7 测试是否成功

~~~shell
docker run hello-world
~~~

~~~shell
[root@lsbysl ~]# docker run hello-world
Unable to find image 'hello-world:latest' locally # 本地没有w
latest: Pulling from library/hello-world # 去远程拉取
2db29710123e: Pull complete # pull 全部
Digest: sha256:bfea6278a0a267fad2634554f4f0c6f31981eea41c553fdf5a83e95a41d40c38
Status: Downloaded newer image for hello-world:latest # 下载成功

Hello from Docker! # 运行结果
~~~

### 4.8 查看镜像

~~~
[root@lsbysl ~]# docker  images
REPOSITORY                                        TAG        IMAGE ID       CREATED        SIZE   
hello-world                                       latest    feb5d9fea6a5   6 months ago   13.3kB
~~~

### 4.9 卸载docker

~~~shell
yum remove docker-ce docker-ce-cli containerd.io  # 卸载依赖
rm -rf /var/lib/docker    # 删除资源  . /var/lib/docker是docker的默认工作路径
~~~

### 4.10 配置阿里云加速

> - 容器镜像服务 -> 镜像加速器 -> 操作文档

~~~
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://4gumn2i1.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
~~~

### 4.11 容器自动启动和取消自动启动

> `--restart`参数

~~~shell
# --restart参数=
	no
		# 默认策略，在容器退出时不重启容器
	on-failure
		# 在容器非正常退出时（退出状态非0），才会重启容器
	on-failure:3
		# 在容器非正常退出时重启容器，最多重启3次
	always
		 # 在容器退出时总是重启容器
#开机自启
	unless-stopped
		# 在容器退出时总是重启容器，但是不考虑在Docker守护进程启动时就已经停止了的容器
# 一般推荐使用always参数
	--restart=always
~~~

> 自启

~~~shell
# docker update --restart=always 容器名或容器ID
docker update --restart=always <CONTAINER ID>
# 例如将tomcat设为自启动
docker update --restart=always tomcat
~~~

> 取消自启

~~~shell
# docker update --restart=no 容器名或容器ID
docker update --restart=no <CONTAINER ID>
# 例如取消tomcat的自启动
docker update --restart=no tomcat
~~~

## 5 Docker运行流程和原理

### 5.1 启动流程

![image-20220410222350519](http://cdn.lsbysl.com//image-20220410222350519.png)

### 5.2 运行原理

![image-20220410222407564](http://cdn.lsbysl.com//image-20220410222407564.png)

### 5.3 Docker整体架构

![image-20220410222433075](http://cdn.lsbysl.com//image-20220410222433075.png)

### 5.4 Docker镜像详解

![image-20220410222449719](http://cdn.lsbysl.com//image-20220410222449719.png)

#### 5.4.1 镜像是什么

~~~
镜像是一种轻量级，可执行的独立软件包，用打包软件运行环境和基于运行环境开发的软件，它包括运行某个软件所需要的所有内容，包括代码，运行时库，环境变量和配置文件。
所有的应用直接打包docker镜像，就可以直接跑起来
~~~

#### 5.4.2 Docker镜像加载原理

> UnionFS（联合文件系统）

我们下载的时候看到一层一层的就是这个

*   联合文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下。联合文件系统是 Docker 镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。
*   特性：一次同时加载多个文件系统，但从外面看起来只能看到一个文件系统。联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

> 镜像加载原理

*   bootfs（boot file system）主要包含bootloader和kernel。bootloader主要是引导加载kernel，完成后整个内核就都在内存中了。此时内存的使用权已由bootfs转交给内核，系统卸载bootfs。可以被不同的Linux发行版公用。
*   rootfs（root file system），包含典型Linux系统中的/dev，/proc，/bin，/etc等标准目录和文件。rootfs就是各种不同操作系统发行版（Ubuntu，Centos等）。因为底层直接用Host的kernel，rootfs只包含最基本的命令，工具和程序就可以了。

#### 5.4.3 分层理解  

> 分层的镜像

    所有的Docker镜像都起始于一个基础镜像层，当进行修改或增加新的内容时，就会在当前镜像层之上，创建新的容器层。  
    容器在启动时会在镜像最外层上建立一层可读写的容器层（R/W），而镜像层是只读的（R/O）。

![image-20220410222514796](http://cdn.lsbysl.com//image-20220410222514796.png)



## 6 Docker常用命令

### 6.1 基本命令

> 命令的帮助文档地址:https://docs.docker.com/engine/reference/commandline/docker/

~~~shell
docker version          #查看docker的版本信息
docker info             #查看docker的系统信息,包括镜像和容器的数量
docker stats            # 显示容器使用的系统资源
docker 命令 --help       #帮助命令(可查看可选的参数)
docker COMMAND --help
docker tag 镜像id 新的名字:[TAG]
docker port myredis     # 查看容器占用的端口
~~~

> 示例：

~~~shell
[root@lsbysl ~]# docker rm --help

Usage:  docker rm [OPTIONS] CONTAINER [CONTAINER...]

Remove one or more containers

Options:
  -f, --force     Force the removal of a running container (uses SIGKILL)
  -l, --link      Remove the specified link
  -v, --volumes   Remove anonymous volumes associated with the container
~~~

### 6.2 镜像命令

#### 6.2.1 查看本机镜像

> 命令:docker images

~~~shell
[root@lsbysl ~]# docker images
# 镜像的仓库源                                    镜像的标签     镜像的id     镜像的创建时间      镜像的大小
REPOSITORY                                        TAG       IMAGE ID       CREATED        SIZE
redis                                             latest    87c26977fd90   13 days ago    113MB
halohub/halo                                      latest    b7b9923025c2   3 months ago   325MB
mongo                                             latest    dfda7a2cf273   3 months ago   693MB
nextcloud                                         latest    e848004dfd99   3 months ago   969MB
mysql                                             latest    bbf6571db497   3 months ago   516MB
hello-world                                       latest    feb5d9fea6a5   6 months ago   13.3kB
centos                                            latest    5d0da3dc9764   6 months ago   231MB
~~~

> 可选参数

~~~
-a/--all 列出所有镜像
-q/--quiet 只显示镜像的id
~~~

#### 6.2.2   搜索镜像

> 命令:docker search

> 示例：

~~~shell
[root@lsbysl ~]# docker search mysql
# 名字                             描述                                            收藏      官方的       自动化
NAME                             DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                            MySQL is a widely used, open-source relation…   12338     [OK]       
mariadb                          MariaDB Server is a high performing open sou…   4746      [OK]       
mysql/mysql-server               Optimized MySQL Server Docker images. Create…   916                  [OK]
percona                          Percona Server is a fork of the MySQL relati…   572       [OK]       
phpmyadmin                       phpMyAdmin - A web interface for MySQL and M…   488       [OK]       
mysql/mysql-cluster              Experimental MySQL Cluster Docker images. Cr…   93                   
centos/mysql-57-centos7          MySQL 5.7 SQL database server                   92                   
~~~

> 可选参数

~~~shell
Search the Docker Hub for images

Options:
  -f, --filter filter   Filter output based on conditions provided # 根据提供的条件过滤输出
      --format string   Pretty-print search using a Go template # 使用 Go 模板进行漂亮打印搜索
      --limit int       Max number of search results (default 25)#  最大搜索结果数（默认 25）
      --no-trunc        Don't truncate output # 不截断输出
~~~

> 示例：搜索收藏数大于3000的镜像

~~~shell
[root@lsbysl ~]# docker search mysql --filter=STARS=3000
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   12339     [OK]       
mariadb   MariaDB Server is a high performing open sou…   4746      [OK]  
~~~

#### 6.2.3 下载镜像

> 命令： `docker pull 镜像名[:tag]` 

> 示例：

~~~shell
[root@lsbysl ~]# docker pull mysql
Using default tag: latest
latest: Pulling from library/mysql
72a69066d2fe: Pull complete 
93619dbc5b36: Pull complete 
99da31dd6142: Pull complete 
626033c43d70: Pull complete 
37d5d7efb64e: Pull complete 
ac563158d721: Pull complete 
d2ba16033dad: Pull complete 
688ba7d5c01a: Pull complete 
00e060b6d11d: Pull complete 
1c04857f594f: Pull complete 
4d7cfa90e6ea: Pull complete 
e0431212d27d: Pull complete 
Digest: sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest
~~~

#### 6.2.4 删除镜像

> docker rmi

~~~shell
#删除指定的镜像id
[root@lsbysl ~]# docker rmi -f  镜像id
#删除多个镜像id
[root@lsbysl ~]# docker rmi -f  镜像id 镜像id 镜像id
#删除全部的镜像id
[root@lsbysl ~]# docker rmi -f  $(docker images -aq)
~~~

#### 6.2.5 镜像变更历史

> docker history 镜像id

~~~shell
[root@lsbysl dockerfile]# docker history 74ad2dd7c325
IMAGE          CREATED          CREATED BY                                      SIZE      COMMENT
74ad2dd7c325   24 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "/bin…   0B
456ca93cc296   24 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "echo…   0B
ab70a2286518   24 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "echo…   0B
1304f6f4bead   24 minutes ago   /bin/sh -c #(nop)  EXPOSE 80                    0B
12e320b0fabe   24 minutes ago   /bin/sh -c yum -y install net-tools             161MB
d167c8a682ef   24 minutes ago   /bin/sh -c yum -y install vim                   216MB
39b6c6f1f857   27 minutes ago   /bin/sh -c #(nop) WORKDIR /usr/local            0B
664739aa1feb   27 minutes ago   /bin/sh -c #(nop)  ENV MYPATH=/usr/local        0B
343709520887   27 minutes ago   /bin/sh -c #(nop)  MAINTAINER lsbysl.com<New…   0B
eeb6ee3f44bd   6 months ago     /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B
<missing>      6 months ago     /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B
<missing>      6 months ago     /bin/sh -c #(nop) ADD file:b3ebbe8bd304723d4…   204MB
~~~

### 6.3 容器命令

#### 6.3.1 运行容器

> 命令:docker run [可选参数] image

~~~shell
#参数说明
--name="名字"           指定容器名字
-d                     后台方式运行
-it                    使用交互方式运行,进入容器查看内容
-p                     指定容器的端口
( -p ip:主机端口:容器端口  配置主机端口映射到容器端口
  -p 主机端口:容器端口
  -p 容器端口)
-P                     随机指定端口(大写的P)
# 补充
--rm                   加上表示用完就删除(用于测试)
~~~

> 示例：

~~~shell
[root@lsbysl ~]# docker run --name="test_run_docker" mysql
2022-04-01 06:36:51+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.27-1debian10 started.
2022-04-01 06:36:51+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2022-04-01 06:36:51+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.27-1debian10 started.
2022-04-01 06:36:51+00:00 [ERROR] [Entrypoint]: Database is uninitialized and password option is not specified
    You need to specify one of the following:
    - MYSQL_ROOT_PASSWORD
    - MYSQL_ALLOW_EMPTY_PASSWORD
    - MYSQL_RANDOM_ROOT_PASSWORD
~~~

> docker run -it --rm image
>
> 表示用完就删除用于测试

~~~shell
docker run -it --rm -p 9998:80 nginx
~~~

#### 6.3.2 进入容器

> 命令:docker run -it [容器ID] /bin/bash

~~~shell
# 容器外
[root@lsbysl ~]# hostname
lsbysl
[root@lsbysl ~]# docker run -it centos /bin/bash
# 容器内
[root@eb108318ded2 /]# hostname
eb108318ded2
~~~

#### 6.3.3 退出容器

> 命令:exit 停止并退出容器（后台方式运行则仅退出）

~~~shell
# 容器内
[root@eb108318ded2 /]# exit
exit
# 容器外
[root@lsbysl ~]# docker ps # 退出容器且容器不是后台运行查询不到
CONTAINER ID   IMAGE                 COMMAND                  CREATED        STATUS       PORTS                               NAMES
66d9ffd58530   redis                 "docker-entrypoint.s…"   8 days ago     Up 5 hours   0.0.0.0:6379->6379/tcp              redis-test
6e6913643a1b   halohub/halo:latest   "/bin/sh -c 'java -X…"   3 months ago   Up 5 hours   0.0.0.0:8090->8090/tcp              halo
9ccc3b619981   bbf6571db497          "docker-entrypoint.s…"   3 months ago   Up 5 hours   0.0.0.0:3306->3306/tcp, 33060/tcp   mysql-test
69adfff8cfd5   nextcloud             "/entrypoint.sh apac…"   3 months ago   Up 5 hours   0.0.0.0:9999->80/tcp                nextcloud
77e63d41a5a6   mongo                 "docker-entrypoint.s…"   3 months ago   Up 5 hours   0.0.0.0:27017->27017/tcp            mongo
~~~

> 命令:Ctrl+P+Q  不停止容器退出

~~~shell
[root@lsbysl ~]# docker run -it centos /bin/bash
[root@53d45eeecf3c /]#
# Ctrl+P+Q  不停止容器退出
[root@lsbysl ~]# 
[root@lsbysl ~]# docker ps # 可以看到centos还在后台运行
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                               NAMES
53d45eeecf3c   centos                "/bin/bash"              30 seconds ago   Up 30 seconds                                       interesting_carver
~~~

#### 6.3.4 查看容器

> 命令:docker ps 

> 参数说明:

~~~shell
docker ps       # 列出当前正在运行的容器
docker ps -a    # 列出所有容器的运行记录
docker ps -n=1  # 显示最近创建的n个容器
docker ps -q    # 只显示容器的编号
~~~

> 示例:

~~~shell
[root@lsbysl ~]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED        STATUS       PORTS                               NAMES
66d9ffd58530   redis                 "docker-entrypoint.s…"   8 days ago     Up 6 hours   0.0.0.0:6379->6379/tcp              redis-test
6e6913643a1b   halohub/halo:latest   "/bin/sh -c 'java -X…"   3 months ago   Up 6 hours   0.0.0.0:8090->8090/tcp              halo
9ccc3b619981   bbf6571db497          "docker-entrypoint.s…"   3 months ago   Up 6 hours   0.0.0.0:3306->3306/tcp, 33060/tcp   mysql-test
69adfff8cfd5   nextcloud             "/entrypoint.sh apac…"   3 months ago   Up 6 hours   0.0.0.0:9999->80/tcp                nextcloud
77e63d41a5a6   mongo                 "docker-entrypoint.s…"   3 months ago   Up 6 hours   0.0.0.0:27017->27017/tcp            mongo
[root@lsbysl ~]# docker ps -a
CONTAINER ID   IMAGE                 COMMAND                  CREATED        STATUS       PORTS                               NAMES
66d9ffd58530   redis                 "docker-entrypoint.s…"   8 days ago     Up 6 hours   0.0.0.0:6379->6379/tcp              redis-test
6e6913643a1b   halohub/halo:latest   "/bin/sh -c 'java -X…"   3 months ago   Up 6 hours   0.0.0.0:8090->8090/tcp              halo
9ccc3b619981   bbf6571db497          "docker-entrypoint.s…"   3 months ago   Up 6 hours   0.0.0.0:3306->3306/tcp, 33060/tcp   mysql-test
69adfff8cfd5   nextcloud             "/entrypoint.sh apac…"   3 months ago   Up 6 hours   0.0.0.0:9999->80/tcp                nextcloud
77e63d41a5a6   mongo                 "docker-entrypoint.s…"   3 months ago   Up 6 hours   0.0.0.0:27017->27017/tcp            mongo
[root@lsbysl ~]# docker ps -n=1
CONTAINER ID   IMAGE     COMMAND                  CREATED      STATUS       PORTS                    NAMES
66d9ffd58530   redis     "docker-entrypoint.s…"   8 days ago   Up 6 hours   0.0.0.0:6379->6379/tcp   redis-test
[root@lsbysl ~]# docker ps -q
66d9ffd58530
6e6913643a1b
9ccc3b619981
69adfff8cfd5
77e63d41a5a6
~~~

#### 6.3.5 删除容器

~~~shell
docker rm 容器id                 #删除指定的容器,不能删除正在运行的容器,强制删除使用 rm -f
docker rm -f $(docker ps -aq)   #删除所有的容器
docker ps -a -q|xargs docker rm #删除所有的容器
~~~

#### 6.3.6 启动和重启容器

~~~shell
docker start 容器id          #启动容器
docker restart 容器id        #重启容器
docker stop 容器id           #停止当前运行的容器
docker kill 容器id           #强制停止当前容器
~~~

#### 6.3.7 查看日志

> 命令: docker logs

> 参数:

~~~shell
[root@lsbysl ~]# docker logs --help

Usage:  docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

Options:
					    # 显示提供给日志的额外详细信息
      --details        Show extra details provided to logs 
                       # 跟随日志输出
  -f, --follow         Follow log output 
  						# 显示自时间戳（例如 2013-01-02T13:23:37Z）或相对时间（例如 42m 持续 42 分钟）以来的日志
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
                       # 从日志末尾开始显示的行数（默认为“all”）
  -n, --tail string    Number of lines to show from the end of the logs (default "all")
                       # 显示时间戳
  -t, --timestamps     Show timestamps
  					   # 在时间戳（例如 2013-01-02T13:23:37Z）或相对时间（例如 42m 42 分钟）之前显示日志
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
~~~

> 常用:

~~~shell
docker logs -tf 容器id
docker logs --tail number 容器id #number为要显示的日志条数
~~~

#### 6.3.8 查看容器中的进程信息

> 命令:docker top 容器id

>  示例：

~~~shell
[root@lsbysl ~]# docker top 9ccc3b619981
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
polkitd             27029               26997               0                   Mar31               pts/0               00:01:41            mysqld
~~~

#### 6.3.9 查看容器的元数据

> 命令：docker inspect 容器id

> 作用:查看镜像详细信息，包括制作者、适应架构、各层的数字摘要等。

#### 6.3.10 进入正在运行的容器

> 命令一:docker exec -it 容器id /bin/bash
>
> 进入容器后开启一个新的终端，可以在里面操作

~~~shell
[root@lsbysl ~]# docker exec -it 6e6913643a1b /bin/bash
root@6e6913643a1b:/application# ls
BOOT-INF  META-INF  org
~~~

> 命令二:docker attach 
>
> 进入容器正在执行的终端，不会启动新的进程 退出可能会中断当前运行的终端

~~~shell
[root@lsbysl ~]# docker attach db32
[root@db3264736be8 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
~~~

####  6.3.11 拷贝容器文件到主机

> 命令:docker cp 容器id:容器内路径 目的主机路径

~~~shell
[root@lsbysl ~]# docker cp 2b31:/home/lsbysl.py /root
[root@lsbysl ~]# ll
-rw-r--r-- 1 root root    0 4月   1 10:27 lsbysl.py
~~~



#### 6.3.12 常用部署

##### Nginx

~~~shell
[root@lsbysl ~]# docker search nginx  # 查找
[root@lsbysl ~]# docker pull nginx    # 下载
[root@lsbysl ~]# docker run -d --name nginx -p 9998:80 nginx   # 启动

# 备注
-d 后台运行
--name 给容器命名
-p 9998:80 将宿主机的端口3334映射到该容器的80端口
~~~

##### Dockerui

> 图形化界面

~~~shell
docker search dockerui
docker pull abh1nav/dockerui
docker run -d --privileged --name dockerui -p 9998:9000 -v /var/run/docker.sock:/var/run/docker.sock abh1nav/dockerui  
#放开物理机的9000端口对应Docker容器的9000端口
~~~

##### Elasticsearch

> Elasticsearch 是一个功能强大的开源搜索和分析引擎，它使数据易于探索。

> 命令:

~~~shell
# es十分消耗内存
# es的数据一般需要放置到安全目录！挂载
# --net somenetwork 网络配置

# 启动Elasticsearch - 这个命令未添加内存现在巨卡
docker run -d --name elasticsearch -p 9998:9200 -p 9997:9300 -e "discovery.type=single-node" elasticsearch:7.6.2
# -e 环境配置修改 添加内存限制参数 最小64m 最大512m
docker run -d --name elasticsearch -p 9998:9200 -p 9997:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
# 查看stats
docker status
# 查看是否启动成功
curl localhost:20000
~~~

> 示例：

~~~shell
# 下载运行Elasticsearch
[root@lsbysl ~]# docker run -d --name elasticsearch -p 9998:9200 -p 9997:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx512m" elasticsearch:7.6.2
Unable to find image 'elasticsearch:7.6.2' locally
7.6.2: Pulling from library/elasticsearch
ab5ef0e58194: Pull complete
c4d1ca5c8a25: Pull complete
941a3cc8e7b8: Pull complete
43ec483d9618: Pull complete
c486fd200684: Pull complete
1b960df074b2: Pull complete
1719d48d6823: Pull complete
Digest: sha256:1b09dbd93085a1e7bca34830e77d2981521a7210e11f11eda997add1c12711fa
Status: Downloaded newer image for elasticsearch:7.6.2
372804845f4fe5943a43cfbe31176370d3c60c603d5fbc86d3c84a46cfa32a36
# 查看是否运行
[root@lsbysl ~]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS         PORTS                                            NAMES
372804845f4f   elasticsearch:7.6.2   "/usr/local/bin/dock…"   2 minutes ago   Up 2 minutes   0.0.0.0:9998->9200/tcp, 0.0.0.0:9997->9300/tcp   elasticsearch
# 查看运行状态及内存占用
[root@lsbysl ~]# docker stats
CONTAINER ID   NAME            CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O         PIDS
372804845f4f   elasticsearch   0.00%     393.7MiB / 3.692GiB   10.41%    2.33kB / 3.61kB   16.8MB / 1.47MB   47
# 查看是否启动成功
[root@lsbysl ~]# curl localhost:9998
{
  "name" : "372804845f4f",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "xj9cHltAQ2m4hfOKUSyGAg",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
~~~

##### Mysql(目录挂载)

> 命令:docker run -d -p 9998:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=passwd --name mysql_test01 mysql

> 示例：

~~~shell
# 获取镜像（可忽略）
[root@lsbysl ceshi]# docker pull mysql
# 运行mysql容器 挂载conf data目录且设置root密码
-d 后台运行
-p 映射端口
-v 挂载目录
-e 环境配置
--name 设置容器名字
[root@lsbysl ceshi]# docker run -d -p 9998:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=passwd --name mysql_test01 mysql
46a509c3804acfb6a4a1b3ed1b17ad65214829a38282f824f767bb390363c534
# 在数据库中创建一个数据库看映射的目录中是否增加
[root@lsbysl data]# ll
总用量 198064
-rw-r----- 1 polkitd input       56 4月   5 04:14 auto.cnf
-rw-r----- 1 polkitd input  3116921 4月   5 04:14 binlog.000001
-rw-r----- 1 polkitd input      376 4月   5 04:22 binlog.000002
-rw-r----- 1 polkitd input       32 4月   5 04:14 binlog.index
-rw------- 1 polkitd input     1680 4月   5 04:14 ca-key.pem
-rw-r--r-- 1 polkitd input     1112 4月   5 04:14 ca.pem
-rw-r--r-- 1 polkitd input     1112 4月   5 04:14 client-cert.pem
-rw------- 1 polkitd input     1680 4月   5 04:14 client-key.pem
-rw-r----- 1 polkitd input   196608 4月   5 04:22 #ib_16384_0.dblwr
-rw-r----- 1 polkitd input  8585216 4月   5 04:14 #ib_16384_1.dblwr
-rw-r----- 1 polkitd input     5706 4月   5 04:14 ib_buffer_pool
-rw-r----- 1 polkitd input 12582912 4月   5 04:22 ibdata1
-rw-r----- 1 polkitd input 50331648 4月   5 04:22 ib_logfile0
-rw-r----- 1 polkitd input 50331648 4月   5 04:14 ib_logfile1
-rw-r----- 1 polkitd input 12582912 4月   5 04:14 ibtmp1
drwxr-x--- 2 polkitd input     4096 4月   5 04:14 #innodb_temp
drwxr-x--- 2 polkitd input     4096 4月   5 04:14 mysql
-rw-r----- 1 polkitd input 31457280 4月   5 04:22 mysql.ibd
drwxr-x--- 2 polkitd input     4096 4月   5 04:14 performance_schema
-rw------- 1 polkitd input     1676 4月   5 04:14 private_key.pem
-rw-r--r-- 1 polkitd input      452 4月   5 04:14 public_key.pem
-rw-r--r-- 1 polkitd input     1112 4月   5 04:14 server-cert.pem
-rw------- 1 polkitd input     1676 4月   5 04:14 server-key.pem
drwxr-x--- 2 polkitd input     4096 4月   5 04:14 sys
drwxr-x--- 2 polkitd input     4096 4月   5 04:22 test_add # 新创建的数据库
# 删除容器mysql看数据是否会丢失
[root@lsbysl data]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS         PORTS                               NAMES
46a509c3804a   mysql                 "docker-entrypoint.s…"   9 minutes ago   Up 9 minutes   33060/tcp, 0.0.0.0:9998->3306/tcp   mysql_test01
66d9ffd58530   redis                 "docker-entrypoint.s…"   12 days ago     Up 4 days      0.0.0.0:6379->6379/tcp              redis-test
6e6913643a1b   halohub/halo:latest   "/bin/sh -c 'java -X…"   3 months ago    Up 4 days      0.0.0.0:8090->8090/tcp              halo
9ccc3b619981   bbf6571db497          "docker-entrypoint.s…"   3 months ago    Up 4 days      0.0.0.0:3306->3306/tcp, 33060/tcp   mysql-test
69adfff8cfd5   nextcloud             "/entrypoint.sh apac…"   3 months ago    Up 4 days      0.0.0.0:9999->80/tcp                nextcloud
77e63d41a5a6   mongo                 "docker-entrypoint.s…"   3 months ago    Up 4 days      0.0.0.0:27017->27017/tcp            mongo
# 删除运行中的容器
[root@lsbysl data]# docker rm -f 46a509c3804a
46a509c3804a
# 数据依然存在
[root@lsbysl data]# ll
总用量 198064
-rw-r----- 1 polkitd input       56 4月   5 04:14 auto.cnf
-rw-r----- 1 polkitd input  3116921 4月   5 04:14 binlog.000001
-rw-r----- 1 polkitd input      376 4月   5 04:22 binlog.000002
-rw-r----- 1 polkitd input       32 4月   5 04:14 binlog.index
-rw------- 1 polkitd input     1680 4月   5 04:14 ca-key.pem
-rw-r--r-- 1 polkitd input     1112 4月   5 04:14 ca.pem
-rw-r--r-- 1 polkitd input     1112 4月   5 04:14 client-cert.pem
-rw------- 1 polkitd input     1680 4月   5 04:14 client-key.pem
-rw-r----- 1 polkitd input   196608 4月   5 04:22 #ib_16384_0.dblwr
-rw-r----- 1 polkitd input  8585216 4月   5 04:14 #ib_16384_1.dblwr
-rw-r----- 1 polkitd input     5706 4月   5 04:14 ib_buffer_pool
-rw-r----- 1 polkitd input 12582912 4月   5 04:22 ibdata1
-rw-r----- 1 polkitd input 50331648 4月   5 04:22 ib_logfile0
-rw-r----- 1 polkitd input 50331648 4月   5 04:14 ib_logfile1
-rw-r----- 1 polkitd input 12582912 4月   5 04:14 ibtmp1
drwxr-x--- 2 polkitd input     4096 4月   5 04:14 #innodb_temp
drwxr-x--- 2 polkitd input     4096 4月   5 04:14 mysql
-rw-r----- 1 polkitd input 31457280 4月   5 04:22 mysql.ibd
drwxr-x--- 2 polkitd input     4096 4月   5 04:14 performance_schema
-rw------- 1 polkitd input     1676 4月   5 04:14 private_key.pem
-rw-r--r-- 1 polkitd input      452 4月   5 04:14 public_key.pem
-rw-r--r-- 1 polkitd input     1112 4月   5 04:14 server-cert.pem
-rw------- 1 polkitd input     1676 4月   5 04:14 server-key.pem
drwxr-x--- 2 polkitd input     4096 4月   5 04:14 sys
drwxr-x--- 2 polkitd input     4096 4月   5 04:22 test_add
-rw-r----- 1 polkitd input 16777216 4月   5 04:22 undo_001
-rw-r----- 1 polkitd input 16777216 4月   5 04:16 undo_002
~~~



#### 6.3.13 端口暴露的原理

![image-20220410222606874](http://cdn.lsbysl.com//image-20220410222606874.png)

#### 6.3.14 commit镜像

> 命令:docker commit 

> 想要保存容器当前状态，就可以通过commit来提交，获得一个镜像，相当于vm虚拟机的快照      

~~~shell
docker commit # 提交容器成为一个新的副本
# 命令和git原理类似
docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名:[TAG]
~~~

>  示例：

~~~shell
# 步骤一运行镜像tomcat 进入容器且暴露端口9998:8080
[root@lsbysl ~]# docker run -it -p 9998:8080 tomcat
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr/local/openjdk-11
Using CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
Using CATALINA_OPTS:
# 步骤二 查看容器 进入刚创建好的容器中
[root@lsbysl ~]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                               NAMES
dd3df6014d42   tomcat                "catalina.sh run"        37 seconds ago   Up 35 seconds   0.0.0.0:9998->8080/tcp              magical_leakey
# 官方默认的webapps 目录是空的
[root@lsbysl ~]# docker exec -it dd3df6014d42 /bin/bash
root@dd3df6014d42:/usr/local/tomcat# cd webapps
root@dd3df6014d42:/usr/local/tomcat/webapps# ls
root@dd3df6014d42:/usr/local/tomcat/webapps#
# 拷贝文件 拷贝目录下的所有文件用-r 递归
root@dd3df6014d42:/usr/local/tomcat/webapps# cd ..
root@dd3df6014d42:/usr/local/tomcat# cp -r webapps.dist/* webapps
root@dd3df6014d42:/usr/local/tomcat# cd webapps
root@dd3df6014d42:/usr/local/tomcat/webapps# ls
ROOT  docs  examples  host-manager  manager
# 退出容器
root@dd3df6014d42:/usr/local/tomcat/webapps# exit
exit
# 查看容器
[root@lsbysl ~]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                               NAMES
dd3df6014d42   tomcat                "catalina.sh run"        35 minutes ago   Up 35 minutes   0.0.0.0:9998->8080/tcp              magical_leakey
66d9ffd58530   redis                 "docker-entrypoint.s…"   12 days ago      Up 4 days       0.0.0.0:6379->6379/tcp              redis-test
6e6913643a1b   halohub/halo:latest   "/bin/sh -c 'java -X…"   3 months ago     Up 4 days       0.0.0.0:8090->8090/tcp              halo
9ccc3b619981   bbf6571db497          "docker-entrypoint.s…"   3 months ago     Up 4 days       0.0.0.0:3306->3306/tcp, 33060/tcp   mysql-test
69adfff8cfd5   nextcloud             "/entrypoint.sh apac…"   3 months ago     Up 4 days       0.0.0.0:9999->80/tcp                nextcloud
77e63d41a5a6   mongo                 "docker-entrypoint.s…"   3 months ago     Up 4 days       0.0.0.0:27017->27017/tcp            mongo
# 提交容器
[root@lsbysl ~]# docker commit -a="lsbysl.com" -m="add webapps app" dd3df6014d42 tomcat01:1.0
sha256:c657c796768227d431f9b29bb961e40c048178f644a48e55dde9ece3eaec33c1
# 查看提交后的镜像也就是我们修改过后的镜像
[root@lsbysl ~]# docker images
REPOSITORY                                        TAG       IMAGE ID       CREATED              SIZE
tomcat01                                          1.0       c657c7967682   About a minute ago   684MB
~~~

#### 6.3.15 扩展-导出&导入容器

> 导出容器:docker export

~~~shell
docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                    PORTS               NAMES
7691a814370e        ubuntu:18.04        "/bin/bash"         36 hours ago        Exited (0) 21 hours ago                       test
# 导出容器
docker export 7691a814370e > ubuntu.tar
~~~

这样将导出容器快照到本地文件。

> 导入容器:docker import

~~~shell
# 导入容器
cat ubuntu.tar | docker import - test/ubuntu:v1.0

docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
test/ubuntu         v1.0                9d37a6082e97        About a minute ago   171.3 MB
~~~

此外，也可以通过指定 URL 或者某个目录来导入，例如

~~~shell
docker import http://example.com/exampleimage.tgz example/imagerepo
~~~

注：用户既可以使用 `docker load` 来导入镜像存储文件到本地镜像库，也可以使用 `docker import` *来导入一个容器快照到本地镜像库。这两者的区别在于容器快照文件将丢弃所有的历史记录和元数据信息（即仅保存容器当时的快照状态），而镜像存储文件将保存完整记录，体积也要大。此外，从容器快照文件导入时可以重新指定标签等元数据信息。

### 7 Docker容器数据卷

#### 7.1 什么是容器数据卷

> 总结一句话:容器的持久化和同步操作！

~~~
我们正常启动的容器如果不挂载数据都是存放在容器内，删除容器所有数据都会丢失，相当于删库跑路风险很大
卷的本质是文件或者目录，存在一个或者多个容器中，由docker挂载到容器，但不属于联合文件系统。 
卷的概念不仅解决了数据持久化的问题，还解决了容器间共享数据的问题。
~~~

> 下面信息总结:容器数据卷实现容器直接配置等信息传递，数据卷容器的生命周期一直维持到没有人使用为止
>
> 持久化到了本地，这个时候本地数据不会被删除的

#### 7.2 使用数据卷

> 双向绑定

> 命令一: docker run -it -v 主机目录:容器内目录

>  示例：

~~~shell
# 容器的/home目录挂载到/home/ceshi 目录下
docker run -it -v /home/ceshi:/home centos /bin/bash
# 启动前本机 /home目录
[root@lsbysl home]# pwd
/home
[root@lsbysl home]# ll
总用量 0
# 启动后本机 /home目录
[root@lsbysl home]# pwd
/home
[root@lsbysl home]# ls
ceshi
# 查看容器元数据
[root@lsbysl home]# docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED         STATUS         PORTS                               NAMES
8f7fd0e142f9   centos                "/bin/bash"              3 minutes ago   Up 3 minutes                                       stupefied_cerf
[root@lsbysl home]# docker inspect 8f7fd0e142f9
        "Mounts": [
            {
                "Type": "bind",
                "Source": "/home/ceshi", # 资源-主机地址
                "Destination": "/home", #目的地-docker 容器内地址
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],
# 测试文件是否同步(双向绑定)
# 步骤1 容器/home目录创建一个文件
[root@8f7fd0e142f9 home]# pwd
/home
[root@8f7fd0e142f9 home]# touch lsbysl.py
[root@8f7fd0e142f9 home]#
# 步骤2 查看 主机/home/ceshi目录下是否有
[root@lsbysl home]# ll
总用量 4
drwxr-xr-x 2 root root 4096 4月   5 02:00 ceshi
~~~

> 示例2：

> 测试关闭容器修改主机挂载目录文件内容后启动容器，容器中内容是否会随之改变

~~~shell
# 步骤1 退出在运行的容器
[root@8f7fd0e142f9 home]# exit
exit
# 步骤2 主机挂载目录修改文件
[root@lsbysl home]# cd ceshi/
[root@lsbysl ceshi]# ll
总用量 0
-rw-r--r-- 1 root root 0 4月   5 02:00 lsbysl.py
[root@lsbysl ceshi]# vim lsbysl.py
[root@lsbysl ceshi]# ll
总用量 4
-rw-r--r-- 1 root root 20 4月   5 02:06 lsbysl.py
# 步骤3 启动停止的容器且查看是否同步成功
[root@lsbysl home]# docker ps -a
CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS                     PORTS                               NAMES
8f7fd0e142f9   centos                "/bin/bash"              14 minutes ago   Exited (0) 2 minutes ago                                       stupefied_cerf
[root@lsbysl home]# docker start 8f7fd0e142f9
8f7fd0e142f9
[root@lsbysl home]# docker attach 8f7fd0e142f9
[root@8f7fd0e142f9 /]# cd /home/
[root@8f7fd0e142f9 home]# cat lsbysl.py
print("lsbysl.com")
[root@8f7fd0e142f9 home]#
~~~

> 示例3：安装mysql且目录挂载 详情看6.3.12-mysql

#### 7.3 匿名挂载和具名挂载

> 所有的docker容器内的卷，没有指定目录的情况下都是在/var/lib/docker/volumes/

~~~shell
# 查看所有volume的情况
[root@lsbysl data]# docker volume ls
DRIVER    VOLUME NAME
local     24cc22e817282ee1873013e810376f76906cc04336a7cb13cdc513ceea23dc71
# 这种没有名字的就是匿名挂载 -v只写了容器内的 没有写容器外的 

# 匿名挂载
-v 容器内路径
docker run -d -P --name nginx01 -v /etc/nginx nginx

# 具名挂载
docker run -d -P --name nginx01 -v juming-nginx:/etc/nginx nginx
DRIVER    VOLUME NAME
local     juming-nginx

# 查看挂载信息
[root@lsbysl data]# docker volume inspect juming-nginx
[
    {
        "CreatedAt": "2022-04-05T04:39:47-04:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/juming-nginx/_data",
        "Name": "juming-nginx",
        "Options": null,
        "Scope": "local"
    }
]
~~~

#### 7.3.1 如果确定是匿名挂载还是具名挂载

~~~shell
-v 容器内目录 # 匿名挂载
-v 卷名:容器内目录 # 具名挂载
-v 宿主机目录:容器内目录 # 指定目录挂载
~~~

#### 7.3.2 挂载可读可写

~~~shell
ro readonly # 只读
rw readwrite # 可读可写
docker run -d -P --name nginx01 -v juming-nginx:/etc/nginx:ro nginx # 可读不可写
docker run -d -P --name nginx01 -v juming-nginx:/etc/nginx:rw nginx # 可读可写
# 只要看到了ro就说明这个路径只能通过宿主机来改变，容器内是无法操作的
~~~

#### 7.3.3 数据卷容器

> 命令:--volumes-from

> 多个容器使用同一份数据

~~~shell
# 容器1
docker run -it --name docker01 lsbysl/centos:1.0
# 容器2 此时的docker01就是数据卷容器
docker run -it --name docker02 --volumes-from docker01 lsbysl/centos:1.0
~~~

> 示例1：多个mysql实现数据共享

~~~shell
# mysql数据卷容器
docker run -d -p 9998:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=passwd --name mysql_test01 mysql
# mysql容器2
docker run -d -p 9998:3306 -e MYSQL_ROOT_PASSWORD=passwd --name mysql_test02 --volumes-from mysql_test01 mysql
~~~

### 8 DockerFile介绍

#### 8.1 DockerFile介绍

dockerfile是用来构建docker镜像的文件！命令参数脚本

#### 8.2 构建步骤

~~~shell
1.编写一个dockerfile文件
2.docker build构建成为一个镜像
3.docker run 运行镜像
4.docker push 发布镜像(Docker,阿里云镜像仓库)
~~~

#### 8.3 DockerFile构建过程

##### 8.3.1 基础知识

~~~
1.每个保留关键字(指令)都必须是大写字母
2.执行顺序从上到下
3.# 表示注释
4.每一个指令都会创建一个新的镜像层，并提交！
5.dockerfile文件命名为 Dockerfile 不需要再加-f，build会自动寻找这个文件编译
~~~

##### 8.3.2 构建指令

|    命令     |                             释意                             |                          备注                           |
| :---------: | :----------------------------------------------------------: | :-----------------------------------------------------: |
|    FROM     |                         镜像从那里来                         |                    基础镜像 如centos                    |
| MAINTAINER  |                        镜像维护者信息                        |                        姓名+邮箱                        |
|     RUN     |          构建镜像执行的命令，每一次RUN都会构建一层           |                     需要运行的命令                      |
|     CMD     | 容器启动的命令，如果有多个则以最后一个为准，也可以为ENTRYPOINT提供参数 | 启动的时候需要运行的命令，只有最后一个会生效,可以被替代 |
|   VOLUME    |              定义数据卷，如果没有定义则使用默认              |                       挂载的目录                        |
|    USER     |                  指定后续执行的用户组和用户                  |                                                         |
|   WORKDIR   |                    切换当前执行的工作目录                    |                     镜像的工作目录                      |
| HEALTHCHECH |                         健康检测指令                         |                                                         |
|     ARG     |               变量属性值，但不在容器内部起作用               |                                                         |
|   EXPOSE    |                           暴露端口                           |                                                         |
|     ENV     |                变量属性值，容器内部也会起作用                |                 构建的时候设置环境变量                  |
|     ADD     |                添加文件，如果是压缩文件也解压                |  某个文件，或者某个文件的压缩包，可以通过 && 拼接命令   |
|    COPY     |                    添加文件，以复制的形式                    |                    文件拷贝到镜像中                     |
| ENTRYPOINT  |                     容器进入时执行的命令                     |         启动的时候需要运行的命令，可以追加命令          |
|   ONBUILD   |    当构建一个被继承DockerFile这个时候就会运行ONBUILD指令     |                        触发指令                         |

##### 8.3.3 CMD和ENTRYPOINT的区别

~~~shell
# CMD 启动的时候需要运行的命令，只有最后一个会生效,可以被替代
# ENTRYPOINT 启动的时候需要运行的命令，可以追加命令
~~~

> 测试cmd

~~~shell
# 步骤1 编写dockerfile文件
[root@lsbysl dockerfile]# vim test-cmd-centos
[root@lsbysl dockerfile]# cat test-cmd-centos
FROM centos:7
CMD ["ls","-a"]
[root@lsbysl dockerfile]# docker build -f test-cmd-centos -t cmdtest .
Sending build context to Docker daemon  9.728kB
Step 1/2 : FROM centos:7
 ---> eeb6ee3f44bd
Step 2/2 : CMD ["ls","-a"]
 ---> Running in 88846dd26d57
Removing intermediate container 88846dd26d57
 ---> 84aff1950126
Successfully built 84aff1950126
Successfully tagged cmdtest:latest
# 运行
[root@lsbysl dockerfile]# docker run 84af
.
..
.dockerenv
anaconda-post.log
bin
dev
etc
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
# 追加参数运行失败了 原因替换了cmd内容 原本ls -a 变成了 -l 所以失败
[root@lsbysl dockerfile]#  docker run 84af -l
docker: Error response from daemon: OCI runtime create failed: container_linux.go:380: starting container process caused: exec: "-l": executable file not found in $PATH: unknown.
ERRO[0000] error waiting for container: context canceled
# 完整命令可以使用
[root@lsbysl dockerfile]#  docker run 84af ls -al
total 64
drwxr-xr-x   1 root root  4096 Apr  5 13:45 .
drwxr-xr-x   1 root root  4096 Apr  5 13:45 ..
-rwxr-xr-x   1 root root     0 Apr  5 13:45 .dockerenv
-rw-r--r--   1 root root 12114 Nov 13  2020 anaconda-post.log
lrwxrwxrwx   1 root root     7 Nov 13  2020 bin -> usr/bin
drwxr-xr-x   5 root root   340 Apr  5 13:45 dev
drwxr-xr-x   1 root root  4096 Apr  5 13:45 etc
drwxr-xr-x   2 root root  4096 Apr 11  2018 home
lrwxrwxrwx   1 root root     7 Nov 13  2020 lib -> usr/lib
lrwxrwxrwx   1 root root     9 Nov 13  2020 lib64 -> usr/lib64
drwxr-xr-x   2 root root  4096 Apr 11  2018 media
drwxr-xr-x   2 root root  4096 Apr 11  2018 mnt
drwxr-xr-x   2 root root  4096 Apr 11  2018 opt
dr-xr-xr-x 177 root root     0 Apr  5 13:45 proc
dr-xr-x---   2 root root  4096 Nov 13  2020 root
drwxr-xr-x  11 root root  4096 Nov 13  2020 run
lrwxrwxrwx   1 root root     8 Nov 13  2020 sbin -> usr/sbin
drwxr-xr-x   2 root root  4096 Apr 11  2018 srv
dr-xr-xr-x  13 root root     0 Apr  1 14:27 sys
drwxrwxrwt   7 root root  4096 Nov 13  2020 tmp
drwxr-xr-x  13 root root  4096 Nov 13  2020 usr
drwxr-xr-x  18 root root  4096 Nov 13  2020 var
~~~

> 测试ENTRYPOINT

> 可以看出命令被追加了

~~~shell
[root@lsbysl dockerfile]# cat test-entrypoint
FROM centos:7
ENTRYPOINT ["ls","-a"]
[root@lsbysl dockerfile]# docker build -f test-entrypoint -t test-entrypoint:0.1 .
Sending build context to Docker daemon  10.75kB
Step 1/2 : FROM centos:7
 ---> eeb6ee3f44bd
Step 2/2 : ENTRYPOINT ["ls","-a"]
 ---> Running in 1a3e83532c45
Removing intermediate container 1a3e83532c45
 ---> 2ec2e658fd75
Successfully built 2ec2e658fd75
Successfully tagged test-entrypoint:0.1
# 命令 ls -a
[root@lsbysl dockerfile]# docker run 2ec2e658fd75
.
..
.dockerenv
anaconda-post.log
bin
dev
etc
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
# 命令等于 ls -al
[root@lsbysl dockerfile]# docker run 2ec2e658fd75 -l
total 64
drwxr-xr-x   1 root root  4096 Apr  5 13:51 .
drwxr-xr-x   1 root root  4096 Apr  5 13:51 ..
-rwxr-xr-x   1 root root     0 Apr  5 13:51 .dockerenv
-rw-r--r--   1 root root 12114 Nov 13  2020 anaconda-post.log
lrwxrwxrwx   1 root root     7 Nov 13  2020 bin -> usr/bin
drwxr-xr-x   5 root root   340 Apr  5 13:51 dev
drwxr-xr-x   1 root root  4096 Apr  5 13:51 etc
drwxr-xr-x   2 root root  4096 Apr 11  2018 home
lrwxrwxrwx   1 root root     7 Nov 13  2020 lib -> usr/lib
lrwxrwxrwx   1 root root     9 Nov 13  2020 lib64 -> usr/lib64
drwxr-xr-x   2 root root  4096 Apr 11  2018 media
drwxr-xr-x   2 root root  4096 Apr 11  2018 mnt
drwxr-xr-x   2 root root  4096 Apr 11  2018 opt
dr-xr-xr-x 176 root root     0 Apr  5 13:51 proc
dr-xr-x---   2 root root  4096 Nov 13  2020 root
drwxr-xr-x  11 root root  4096 Nov 13  2020 run
lrwxrwxrwx   1 root root     8 Nov 13  2020 sbin -> usr/sbin
drwxr-xr-x   2 root root  4096 Apr 11  2018 srv
dr-xr-xr-x  13 root root     0 Apr  1 14:27 sys
drwxrwxrwt   7 root root  4096 Nov 13  2020 tmp
drwxr-xr-x  13 root root  4096 Nov 13  2020 usr
drwxr-xr-x  18 root root  4096 Nov 13  2020 var
~~~



#### 8.4 构建测试

> 命令:docker build -f dockerfile文件 -t 镜像名:[TAG] .

> 示例1:centos

~~~shell
# 步骤一 编写Dockerfile的文件
[root@lsbysl dockerfile]# cat test-dockerfile-centos
FROM centos:7
MAINTAINER lsbysl.com<NewLifeMd@163.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash
# 步骤二 通过文件构建镜像
# 命令:docker build -f test-dockerfile-centos -t mycentos:0.1 .
Successfully built 74ad2dd7c325
Successfully tagged mycentos:0.1
# 步骤三 测试运行
[root@lsbysl dockerfile]# docker images
REPOSITORY                                        TAG       IMAGE ID       CREATED          SIZE
mycentos                                          0.1       74ad2dd7c325   9 minutes ago    580MB
[root@lsbysl dockerfile]# docker run -it --rm mycentos:0.1
# 可以看到进入到了设置的工作目录
[root@ded07a22f4ff local]# pwd
/usr/local
[root@ded07a22f4ff local]# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.7  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:ac:11:00:07  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
~~~

#### 8.5 发布镜像

> DockerHub

~~~shell
# 1. 地址:https://hub.docker.com/ 注册自己的账号
# 2. 确定账号可以
# 3.提交镜像
~~~

~~~shell
[root@lsbysl dockerfile]# docker login --help

Usage:  docker login [OPTIONS] [SERVER]

Log in to a Docker registry.
If no server is specified, the default is defined by the daemon.

Options:
  -p, --password string   Password
      --password-stdin    Take the password from stdin
  -u, --username string   Username
~~~

> 命令:docker login -u lsbysl

~~~shell
[root@lsbysl dockerfile]# docker login -u lsbysl
Password:
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

# 推送镜像
[root@lsbysl dockerfile]# docker push mycentos:0.1
~~~

### 9 Docker网络

####  9.1 理解Docker0

> 测试

~~~shell
[root@lsbysl ~]# ip addr
# 本地回环地址
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
# 服务器地址
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:15:5d:f8:37:71 brd ff:ff:ff:ff:ff:ff
    inet 222.211.72.186/24 brd 222.211.72.255 scope global eth0
       valid_lft forever preferred_lft forever
# 服务器内网地址
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 00:15:5d:f8:37:72 brd ff:ff:ff:ff:ff:ff
    inet 10.211.72.186/24 brd 10.211.72.255 scope global eth1
       valid_lft forever preferred_lft forever
# docker0 地址
4: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:0b:1c:c2:1d brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
~~~

> 查看容器内部网络地址 ip addr ifconfig

~~~shell
# 查看容器内部网络地址，会发现eth0的地址 就是docker分配的
[root@lsbysl ~]# docker run -it mycentos:0.1 /bin/bash
# centos 7 使用 ip addr 需要安装
# yum -y install iproute iproute-doc
[root@8a9dd107b664 local]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
176: eth0@if177: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:08 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.8/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
        
# linux可以ping同容器内部
[root@8a9dd107b664 local]# ping 172.17.0.8
PING 172.17.0.8 (172.17.0.8) 56(84) bytes of data.
64 bytes from 172.17.0.8: icmp_seq=1 ttl=64 time=0.051 ms
64 bytes from 172.17.0.8: icmp_seq=2 ttl=64 time=0.052 ms
64 bytes from 172.17.0.8: icmp_seq=3 ttl=64 time=0.050 ms
~~~

#### 9.2 原理

>我们每启动一个docker容器，docker就会给docker容器分配一个ip，我们只要安装了docker，就会有一个docker0桥接模式，使用的技术是evth-pair技术！

> 再次测试宿主机 ip addr

~~~shell
177: vetha0f629e@if176: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether 46:39:15:f5:73:9a brd ff:ff:ff:ff:ff:ff link-netnsid 6
~~~

~~~
我们发现这个容器带来网卡都是一对一对的 如上面 176 177

evth-pair 就是一对的虚拟设备接口，他的都是成对出现的，一段连着协议，一段彼此相连

正因为有这个特性，enth-pair 充当一个桥梁

OpenStac，Docker容器之间的连接，OVS的连接， 都是使用evth-pair技术
~~~

> 测试新的容器是否可以ping通 172.17.0.8

~~~shell
[root@lsbysl ~]# docker run -it mycentos:0.1 /bin/bash
[root@f83a6d564435 local]# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.9  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:ac:11:00:09  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions  0

[root@f83a6d564435 local]# ping 172.17.0.8
PING 172.17.0.8 (172.17.0.8) 56(84) bytes of data.
64 bytes from 172.17.0.8: icmp_seq=1 ttl=64 time=0.132 ms
64 bytes from 172.17.0.8: icmp_seq=2 ttl=64 time=0.083 ms
# 结论 容器之间是可以互相通讯的 
# 所有的容器不指定网络的情况下都是docker0路由的，docker会给我们的容器分配一个默认的可用ip
~~~

#### 9.3 小结

~~~shell
# Docker使用的是Linux的桥接，宿主机中是一个Docker容器的网桥docker0
# 容器和docker0 之间使用的是evth-pair 技术
# Docker中所有的网络接口都是虚拟的，虚拟的转发效率高！（内网传递文件）
# 只要容器删除，对应网桥一对就没了
~~~

![](http://cdn.lsbysl.com//docker%E9%80%9A%E8%AE%AFdcoekr0-enth-pair-b8cb34d8ee8a4d38a096733305b18d23.png)

#### 9.4 --link(不推荐使用)

> 命令:--link 容器id/容器名

> 思考一个场景，编写了一个微服务，database url=ip:，项目不重启，数据库ip换掉了，我们希望可以处理这个问题，可以名字来进行访问容器？

~~~shell
[root@lsbysl ~]# docker exec -it centos02 ping centos01
ping: centos01: Name or service not known

# 如何可以解决呢？
# 通过--link处理
[root@lsbysl ~]# docker run -it --name centos03 --link centos02 mycentos:0.1
[root@lsbysl ~]# docker exec -it centos03 ping centos02
PING centos02 (172.17.0.7) 56(84) bytes of data.
64 bytes from centos02 (172.17.0.7): icmp_seq=1 ttl=64 time=0.353 ms
64 bytes from centos02 (172.17.0.7): icmp_seq=2 ttl=64 time=0.076 ms
~~~

> 本质
>
> 查看 hosts配置

~~~shell
[root@lsbysl ~]# docker exec -it centos03 cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.17.0.7	centos02 48d42b9105de
172.17.0.9	d9757a175a16  
~~~

> 总结:--link本质就是在hosts文件中配置了 centos02的映射

#### 9.5 自定义网络 

> 查看所有的docker网络:docker network ls

~~~shell
[root@lsbysl ~]# docker network ls
NETWORK ID     NAME          DRIVER    SCOPE
173d2b6b95d1   bridge        bridge    local 
98b6a6f3333e   host          host      local
1284c5e04e4b   none          null      local
0cf0820ad2b8   somenetwork   bridge    local
~~~

##### 9.5.1 网络模式

|   模式    |       释意       |      注释      |
| :-------: | :--------------: | :------------: |
|  bridge   |     桥接模式     |      搭桥      |
|    no     |    不配置网络    |                |
|   host    | 和宿主机共享网络 |                |
| container |  容器内网络连通  | 用的少，局限大 |
|           |                  |                |

> 测试

~~~shell
# 之前启动默认的参数 --net bridge 而这个就是docker0
docker run -it --name centos01 mycentos:0.1
docker run -it --name centos01 --net bridge mycentos:0.1
# docker0特点：默认的，域名不能访问，--link可以打通连接！

# 自定义网络
--driver bridge # 网络模式
--subnet 192.168.0.0/16 # 子网地址
--gateway 192.168.0.1 # 网关地址
# 起止范围 192.168.0.2--192.168.255.255
[root@lsbysl ~]# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
88dba1550e0c3d92af097464a9a8e6e707fb490748033580870304c72bf4ed9c
[root@lsbysl ~]# docker network ls
NETWORK ID     NAME          DRIVER    SCOPE
173d2b6b95d1   bridge        bridge    local
98b6a6f3333e   host          host      local
88dba1550e0c   mynet         bridge    local
1284c5e04e4b   none          null      local
0cf0820ad2b8   somenetwork   bridge    local
~~~

##### 9.5.2 查看网络元数据

> 命令:docker network inspect 网络名

~~~shell
[root@lsbysl ~]# docker network inspect mynet
[
    {
        "Name": "mynet",
        "Id": "88dba1550e0c3d92af097464a9a8e6e707fb490748033580870304c72bf4ed9c",
        "Created": "2022-04-06T04:57:32.03529891-04:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
~~~

##### 9.5.3 自定义网络连通测试

> 示例

~~~shell
[root@lsbysl ~]# docker run -it --name centos01 --net mynet mycentos:0.1
[root@51a42d3a0030 local]# [root@lsbysl ~]# 
[root@lsbysl ~]# docker run -it --name centos02 --net mynet mycentos:0.1
[root@d43c4f93281c local]# [root@lsbysl ~]# 
[root@lsbysl ~]# docker network inspect mynet
[
    {
        "Name": "mynet",
        "Id": "88dba1550e0c3d92af097464a9a8e6e707fb490748033580870304c72bf4ed9c",
        "Created": "2022-04-06T04:57:32.03529891-04:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        # 可以看到我们创建的容器网络已经配置在里面了
        "Containers": {
            "51a42d3a00303d8f5253521e9cf359f2150266b90802e31e61725d50cdb42833": {
                "Name": "centos01",
                "EndpointID": "d947485b8642ccc82300fb1c75f7999a59741c3e6a4b1405024469ea112644cf",
                "MacAddress": "02:42:c0:a8:00:02",
                "IPv4Address": "192.168.0.2/16",
                "IPv6Address": ""
            },
            "d43c4f93281c411589109f724fc23e27825eca1470713f63843a9b5f33f5165c": {
                "Name": "centos02",
                "EndpointID": "dd0e482ca2b97bca8ad828e6514d372da7f88f823a969be36b87a70ed3d3679e",
                "MacAddress": "02:42:c0:a8:00:03",
                "IPv4Address": "192.168.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
~~~

> 使用测试
>
> 可以看到容器内可以直接ping通

~~~shell
[root@lsbysl ~]# docker exec -it centos02 ping centos01
PING centos01 (192.168.0.2) 56(84) bytes of data.
64 bytes from centos01.mynet (192.168.0.2): icmp_seq=1 ttl=64 time=0.085 ms
64 bytes from centos01.mynet (192.168.0.2): icmp_seq=2 ttl=64 time=0.083 ms
64 bytes from centos01.mynet (192.168.0.2): icmp_seq=3 ttl=64 time=0.111 ms
64 bytes from centos01.mynet (192.168.0.2): icmp_seq=4 ttl=64 time=0.083 ms
^C
--- centos01 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 2999ms
rtt min/avg/max/mdev = 0.083/0.090/0.111/0.015 ms
[root@lsbysl ~]# docker exec -it centos02 ping 192.168.0.2
PING 192.168.0.2 (192.168.0.2) 56(84) bytes of data.
64 bytes from 192.168.0.2: icmp_seq=1 ttl=64 time=0.083 ms
64 bytes from 192.168.0.2: icmp_seq=2 ttl=64 time=0.084 ms
64 bytes from 192.168.0.2: icmp_seq=3 ttl=64 time=0.087 ms
^C
--- 192.168.0.2 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 1999ms
rtt min/avg/max/mdev = 0.083/0.084/0.087/0.010 ms
~~~

> 好处:不同的集群使用不同的网络

#### 9.6 网络连通

> 命令:docker network connect 网络名 容器名/容器id

> 作用:假设要跨网络操作别人，就需要使用docker network connect连通

> 一个容器两个或多个ip地址

~~~shell
[root@lsbysl ~]# docker network --help

Usage:  docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network # 将容器连接到网络
  create      Create a network # 创建网络
  disconnect  Disconnect a container from a network # 从网络断开容器的连接
  inspect     Display detailed information on one or more networks # 有关一个或者多个点容器详情
  ls          List networks # 网络列表
  prune       Remove all unused networks # 删除所有未使用的网络
  rm          Remove one or more networks # 删除一个或者多个网络

Run 'docker network COMMAND --help' for more information on a command.
~~~

~~~shell
[root@lsbysl ~]# docker network connect --help

Usage:  docker network connect [OPTIONS] NETWORK CONTAINER

Connect a container to a network

Options:
      --alias strings           Add network-scoped alias for the container # 为容器添加网络范围的别名
      --driver-opt strings      driver options for the network # 网络驱动程序选项
      --ip string               IPv4 address (e.g., 172.30.100.104)
      --ip6 string              IPv6 address (e.g., 2001:db8::33)
      --link list               Add link to another container # 添加链接到另一个容器
      --link-local-ip strings   Add a link-local address for the container # 为容器添加链接本地地址
~~~

> 测试打通 centos03(docker0)-->mynet

~~~shell
# 连通命令
[root@lsbysl ~]# docker network connect mynet centos03
# 可以看到centos03已经加入到了mynet中
[root@lsbysl ~]# docker network inspect mynet
[
    {
        "Name": "mynet",
        "Id": "88dba1550e0c3d92af097464a9a8e6e707fb490748033580870304c72bf4ed9c",
        "Created": "2022-04-06T04:57:32.03529891-04:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "51a42d3a00303d8f5253521e9cf359f2150266b90802e31e61725d50cdb42833": {
                "Name": "centos01",
                "EndpointID": "d947485b8642ccc82300fb1c75f7999a59741c3e6a4b1405024469ea112644cf",
                "MacAddress": "02:42:c0:a8:00:02",
                "IPv4Address": "192.168.0.2/16",
                "IPv6Address": ""
            },
            "c58a8578a5729e94628ad27b7e280e9785dcc2c6e59f50e0260a1c24305939b5": {
                "Name": "centos03",
                "EndpointID": "73bcd2270fac3ff971242b4783237f7bad3f1db1ea995d1aaba5b1ac947a483b",
                "MacAddress": "02:42:c0:a8:00:04",
                "IPv4Address": "192.168.0.4/16",
                "IPv6Address": ""
            },
            "d43c4f93281c411589109f724fc23e27825eca1470713f63843a9b5f33f5165c": {
                "Name": "centos02",
                "EndpointID": "dd0e482ca2b97bca8ad828e6514d372da7f88f823a969be36b87a70ed3d3679e",
                "MacAddress": "02:42:c0:a8:00:03",
                "IPv4Address": "192.168.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
[root@lsbysl ~]#
~~~

> 查看centos03的网络信息

~~~shell
[root@lsbysl ~]# docker exec -it centos03 /bin/bash
# 可以看到有两个ip地址
[root@c58a8578a572 local]# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.7  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:ac:11:00:07  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.4  netmask 255.255.0.0  broadcast 192.168.255.255
        ether 02:42:c0:a8:00:04  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
~~~

> 测试centos03容器内pingcentos02

~~~shell
[root@c58a8578a572 local]# ping centos02
PING centos02 (192.168.0.3) 56(84) bytes of data.
64 bytes from centos02.mynet (192.168.0.3): icmp_seq=1 ttl=64 time=0.088 ms
64 bytes from centos02.mynet (192.168.0.3): icmp_seq=2 ttl=64 time=0.083 ms
64 bytes from centos02.mynet (192.168.0.3): icmp_seq=3 ttl=64 time=0.087 ms
64 bytes from centos02.mynet (192.168.0.3): icmp_seq=4 ttl=64 time=0.086 ms
^Z
[1]+  Stopped                 ping centos02
~~~

> centos02是否可以打通centos03呢,因为都在mynet中可以

~~~shell
[root@lsbysl ~]# docker exec -it centos02 ping centos03
PING centos03 (192.168.0.4) 56(84) bytes of data.
64 bytes from centos03.mynet (192.168.0.4): icmp_seq=1 ttl=64 time=0.063 ms
64 bytes from centos03.mynet (192.168.0.4): icmp_seq=2 ttl=64 time=0.083 ms
64 bytes from centos03.mynet (192.168.0.4): icmp_seq=3 ttl=64 time=0.088 ms
^Z64 bytes from centos03.mynet (192.168.0.4): icmp_seq=4 ttl=64 time=0.099 ms
64 bytes from centos03.mynet (192.168.0.4): icmp_seq=5 ttl=64 time=0.089 ms
64 bytes from centos03.mynet (192.168.0.4): icmp_seq=6 ttl=64 time=0.086 ms
^Z64 bytes from centos03.mynet (192.168.0.4): icmp_seq=7 ttl=64 time=0.079 ms
64 bytes from centos03.mynet (192.168.0.4): icmp_seq=8 ttl=64 time=0.082 ms
^C
--- centos03 ping statistics ---
8 packets transmitted, 8 received, 0% packet loss, time 6998ms
rtt min/avg/max/mdev = 0.063/0.083/0.099/0.014 ms
~~~

##### 9.6.1 部署redis集群

shell脚本

~~~shell
# 创建网卡
docker network create redis --subnet 172.38.0.0/16
# 通过脚本创建六个redis配置
for port in $(seq 1 6); \
do \
mkdir -p /mydata/redis/node-${port}/conf
touch /mydata/redis/node-${port}/conf/redis.conf
cat << EOF >/mydata/redis/node-${port}/conf/redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done
# 启动容器
for port in $(seq 1 6); \
do \
docker run -p 637${port}:6379 -p 1637${port}:16379 --name redis-${port} \
-v /mydata/redis/node-${port}/data:/data \
-v /mydata/redis/node-${port}/conf/redis.conf:/etc/redis/redis.conf \
-d --net redis --ip 172.38.0.1${port} redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
done


# 创建集群
redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1
~~~

> 示例

~~~shell
# 步骤一:创建配置目录及文件
[root@lsbysl ~]# for port in $(seq 1 6); \
> do \
> mkdir -p /mydata/redis/node-${port}/conf
> touch /mydata/redis/node-${port}/conf/redis.conf
> cat << EOF >/mydata/redis/node-${port}/conf/redis.conf
> port 6379
> bind 0.0.0.0
> cluster-enabled yes
> cluster-config-file nodes.conf
> cluster-node-timeout 5000
> cluster-announce-ip 172.38.0.1${port}
> cluster-announce-port 6379
> cluster-announce-bus-port 16379
> appendonly yes
> EOF
> done
# 查看创建的信息
[root@lsbysl ~]# cd /mydata/
[root@lsbysl mydata]# ll
总用量 4
drwxr-xr-x 8 root root 4096 4月   6 12:17 redis
[root@lsbysl mydata]# cd redis/
[root@lsbysl redis]# ls
node-1  node-2  node-3  node-4  node-5  node-6
[root@lsbysl redis]# cd node-1/
[root@lsbysl node-1]# ll
总用量 4
drwxr-xr-x 2 root root 4096 4月   6 12:17 conf
[root@lsbysl node-1]# cd conf/
[root@lsbysl conf]# ll
总用量 4
-rw-r--r-- 1 root root 206 4月   6 12:17 redis.conf
[root@lsbysl conf]# cat redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.11
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
# 步骤2:运行容器
[root@lsbysl conf]# for port in $(seq 1 6); \
> do \
> docker run -p 637${port}:6379 -p 1637${port}:16379 --name redis-${port} \
> -v /mydata/redis/node-${port}/data:/data \
> -v /mydata/redis/node-${port}/conf/redis.conf:/etc/redis/redis.conf \
> -d --net redis --ip 172.38.0.1${port} redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
> done
4454d75963defe569e35322dd447cbd43d48e80acafb8cafb985f276098def5d
68986438d68370dee1bb98e03e17e39235e905339d79bb301a470ea00c782652
c58224e40f2f0c8ed1da38e0d43e29758683479879708b2a819d2080d622cfed
90de1263f4b627c6e71afe15e04625b2476cb376aca27f40f755a9381468d3ff
1a0c0ce1f7baacf20e626cd9e5bc44c89ca48c08abd9d223013bc1fa6a0d1672
# 步骤3:进入redis01容器
[root@lsbysl ~]# docker exec -it redis-1 /bin/sh
/data # ll
/bin/sh: ll: not found
/data # ls
appendonly.aof  nodes.conf
# 步骤4:开启集群
/data # redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1
>>> Performing hash slots allocation on 6 nodes...
Master[0] -> Slots 0 - 5460
Master[1] -> Slots 5461 - 10922
Master[2] -> Slots 10923 - 16383
Adding replica 172.38.0.15:6379 to 172.38.0.11:6379
Adding replica 172.38.0.16:6379 to 172.38.0.12:6379
Adding replica 172.38.0.14:6379 to 172.38.0.13:6379
M: 8896741cfbb9467eb13c48dc279d46a69d8f41b8 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
M: ca519ae6e0a6783e6da2e0dd102d07cdb2723dfe 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
M: fdad7a3167c89064ddc2d853099bf83376225ac5 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
S: 519272c346c8b609afbc61eff5d016c11d98fe21 172.38.0.14:6379
   replicates fdad7a3167c89064ddc2d853099bf83376225ac5
S: 440d2c9ce9a57bdc1c447b67e19b373aca139cb7 172.38.0.15:6379
   replicates 8896741cfbb9467eb13c48dc279d46a69d8f41b8
S: 31eb9bfb55ccf54e22a0172165d77209e179554d 172.38.0.16:6379
   replicates ca519ae6e0a6783e6da2e0dd102d07cdb2723dfe
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join
...
>>> Performing Cluster Check (using node 172.38.0.11:6379)
M: 8896741cfbb9467eb13c48dc279d46a69d8f41b8 172.38.0.11:6379
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
M: ca519ae6e0a6783e6da2e0dd102d07cdb2723dfe 172.38.0.12:6379
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
M: fdad7a3167c89064ddc2d853099bf83376225ac5 172.38.0.13:6379
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
S: 31eb9bfb55ccf54e22a0172165d77209e179554d 172.38.0.16:6379
   slots: (0 slots) slave
   replicates ca519ae6e0a6783e6da2e0dd102d07cdb2723dfe
S: 440d2c9ce9a57bdc1c447b67e19b373aca139cb7 172.38.0.15:6379
   slots: (0 slots) slave
   replicates 8896741cfbb9467eb13c48dc279d46a69d8f41b8
S: 519272c346c8b609afbc61eff5d016c11d98fe21 172.38.0.14:6379
   slots: (0 slots) slave
   replicates fdad7a3167c89064ddc2d853099bf83376225ac5
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
~~~

> 验证高可用

~~~shell
# 进入redis
/data # redis-cli -c
# 查看集群信息
127.0.0.1:6379> cluster info
cluster_state:ok
cluster_slots_assigned:16384
cluster_slots_ok:16384
cluster_slots_pfail:0
cluster_slots_fail:0
cluster_known_nodes:6
cluster_size:3
cluster_current_epoch:6
cluster_my_epoch:1
cluster_stats_messages_ping_sent:499
cluster_stats_messages_pong_sent:500
cluster_stats_messages_sent:999
cluster_stats_messages_ping_received:495
cluster_stats_messages_pong_received:499
cluster_stats_messages_meet_received:5
cluster_stats_messages_received:999
# 查看节点
127.0.0.1:6379> cluster nodes
ca519ae6e0a6783e6da2e0dd102d07cdb2723dfe 172.38.0.12:6379@16379 master - 0 1649263023000 2 connected 5461-10922
fdad7a3167c89064ddc2d853099bf83376225ac5 172.38.0.13:6379@16379 master - 0 1649263022764 3 connected 10923-16383
31eb9bfb55ccf54e22a0172165d77209e179554d 172.38.0.16:6379@16379 slave ca519ae6e0a6783e6da2e0dd102d07cdb2723dfe 0 1649263023765 6 connected
8896741cfbb9467eb13c48dc279d46a69d8f41b8 172.38.0.11:6379@16379 myself,master - 0 1649263023000 1 connected 0-5460
440d2c9ce9a57bdc1c447b67e19b373aca139cb7 172.38.0.15:6379@16379 slave 8896741cfbb9467eb13c48dc279d46a69d8f41b8 0 1649263022000 5 connected
519272c346c8b609afbc61eff5d016c11d98fe21 172.38.0.14:6379@16379 slave fdad7a3167c89064ddc2d853099bf83376225ac5 0 1649263023000 4 connected
# 设置值
127.0.0.1:6379> set a b
# 主机172.38.0.13 处理
-> Redirected to slot [15495] located at 172.38.0.13:6379
OK
# 此时停掉172.38.0.13看看是否还能get到值
[root@lsbysl conf]# docker stop redis-3
redis-3
# 获取值发现0.14处理了
127.0.0.1:6379> get a
-> Redirected to slot [15495] located at 172.38.0.14:6379
"b"
# 可以看到172.38.0.13:6379@16379 master,fail故障转移了
127.0.0.1:6379> cluster nodes
ca519ae6e0a6783e6da2e0dd102d07cdb2723dfe 172.38.0.12:6379@16379 master - 0 1649263322011 2 connected 5461-10922
fdad7a3167c89064ddc2d853099bf83376225ac5 172.38.0.13:6379@16379 master,fail - 1649263267664 1649263267363 3 connected
31eb9bfb55ccf54e22a0172165d77209e179554d 172.38.0.16:6379@16379 slave ca519ae6e0a6783e6da2e0dd102d07cdb2723dfe 0 1649263322513 6 connected
8896741cfbb9467eb13c48dc279d46a69d8f41b8 172.38.0.11:6379@16379 myself,master - 0 1649263321000 1 connected 0-5460
440d2c9ce9a57bdc1c447b67e19b373aca139cb7 172.38.0.15:6379@16379 slave 8896741cfbb9467eb13c48dc279d46a69d8f41b8 0 1649263321000 5 connected
519272c346c8b609afbc61eff5d016c11d98fe21 172.38.0.14:6379@16379 master - 0 1649263322000 7 connected 10923-16383
~~~

### 10 Docker Compose

####  10.1 简介

~~~shell
# Docker compose 来轻松管理容器i，定义运行多个容器，作用批量容器编排 
# Compose 是docker官方开源的项目，需要安装
# Dockerfile 让程序在任何地方运行，web服务，redis，mysql，nginx多个容器
~~~

>  Compose示例

~~~shell
version:'2.o'
services:
	web:
		build: .
		ports:
		- "5000:5000"
		volumes:
		- .:/code
		- logvolume01:/var/log
		links:
		- redis
	redis:
		image:redis
volumes:
	logvolume01:{}
~~~

Docker-compose up 100个服务

##### 10.1.1 Compose:重要概念

- 服务services，容器，应用(web,redis.mysql)
- 项目project 一组关联的容器。博客，web，mysql，up

#### 10.2 安装

##### 10.2.1 下载

~~~shell
# linux 国外下载
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# linux国内下载 网站 https://get.daocloud.io/
curl -L https://get.daocloud.io/docker/compose/releases/download/v2.4.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
~~~

##### 10.2.2 使用测试

> 示例

~~~shell
# 步骤一 下载
[root@lsbysl ~]# curl -L https://get.daocloud.io/docker/compose/releases/download/v2.4.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   423  100   423    0     0    171      0  0:00:02  0:00:02 --:--:--   171
100 25.2M  100 25.2M    0     0  7832k      0  0:00:03  0:00:03 --:--:-- 32.5M
# 步骤二授权
[root@lsbysl ~]# chmod +x /usr/local/bin/docker-compose
# 测试是否安装成功
[root@lsbysl bin]# docker-compose version
Docker Compose version v2.4.1

~~~

> 使用示例参考官方文档 https://docs.docker.com/compose/gettingstarted/

- 准备工作

~~~shell
# 为项目创建一个目录：
mkdir composetest
cd composetest
~~~

- 步骤一 创建文件 在项目目录中创建一个名为的文件app.py

~~~python
# apps.py
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)
~~~

在此示例中，redis是应用程序网络上的 redis 容器的主机名。我们使用 Redis 的默认端口，6379.

- 步骤二 在项目目录中创建另一个名为的文件requirements.txt

~~~shell
# requirements.txt
flask
redis
~~~

- 步骤三 在您的项目目录中，创建一个名为Dockerfile

~~~dockerfile
# syntax=docker/dockerfile:1
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]
~~~

> 从 Python 3.7 映像开始构建映像。
> 将工作目录设置为/code.
> 设置命令使用的环境变量flask。
> 安装 gcc 和其他依赖项
> 复制requirements.txt并安装 Python 依赖项。
> 向镜像添加元数据以描述容器正在侦听端口 5000
> 将项目中的当前目录复制.到镜像中的workdir .。
> 将容器的默认命令设置为flask run.

- 步骤四  在项目目录中创建一个名为的文件docker-compose.yml

~~~yaml
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8000:5000"
  redis:
    image: "redis:alpine"
~~~

> 该服务使用从当前目录中web构建的图像。Dockerfile然后它将容器和主机绑定到暴露的端口，8000. 此示例服务使用 Flask Web 服务器的默认端口，5000.
> 该redis服务使用 从 Docker Hub 注册表中提取的公共Redis映像。

- 步骤五 使用 Compose 构建并运行您的应用程序 在项目目录中，通过运行docker-compose up

~~~shell
docker-compose up
~~~

> Compose 拉取 Redis 映像，为您的代码构建映像，并启动您定义的服务。在这种情况下，代码在构建时被静态复制到映像中。
> 在浏览器中输入 http://localhost:8000/ 以查看正在运行的应用程序。
>
> 如果您在 Linux、Mac 的 Docker Desktop 或 Windows 的 Docker Desktop 上本地使用 Docker，那么 Web 应用程序现在应该在 Docker 守护程序主机上的端口 8000 上进行侦听。将您的 Web 浏览器指向 http://localhost:8000 以查找Hello World消息。如果这不能解决，您也可以尝试 http://127.0.0.1:8000。

- 步骤六 停止容器 docker-compose down 或者在启动应用程序的原始终端中按 CTRL+C。
- 步骤七 编辑 Compose 文件以添加绑定挂载 在您的项目目录中编辑docker-compose.yml以添加服务的 绑定挂载web：

~~~yaml
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8000:5000"
    volumes:
      - .:/code
    environment:
      FLASK_ENV: development
  redis:
    image: "redis:alpine"
~~~

> 新volumes密钥将主机上的项目目录（当前目录）挂载到/code容器内，允许您即时修改代码，而无需重建映像。environment键设置 FLASK_ENV环境变量，它告诉flask run在开发模式下运行并在更改时重新加载代码。这种模式应该只在开发中使用。

- 步骤八  使用 Compose 重新构建并运行应用程序 在您的项目目录中，键入docker-compose up以使用更新的 Compose 文件构建应用程序，然后运行它。 docker-compose up
- 步骤九 尝试其他一些命令

~~~shell
docker-compose up -d
docker-compose ps
docker-compose run # 命令允许您为您的服务运行一次性命令。例如，要查看 web服务可以使用哪些环境变量：如 docker-compose run web env
docker-compose stop # 停止服务
docker-compose down --volumes # 所有内容都关闭，完全删除容器down 。传递--volumes也删除 Redis 容器使用的数据卷：
docker-compose up  --build # 重新构建
~~~

#### 10.3 yaml规则

> docker-compose.yaml 核心

~~~yaml
# 3 层
version:"" # 版本
services: # 服务
	服务1: web
	   # 服务配置
	   images
	   build
	   network
	   ...
	服务2: redis
	服务3: redis
	   ....
	   
# 其他配置 网络/卷 全局规则
volumes:
networks:
configs:
~~~

#### 10.4 开源项目 wordpress

> 参考 https://hub.docker.com/_/wordpress

### 11 Docker Swarm

> 官网文档:https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/

#### 11.1 工作模式

Docker Engine 1.12 引入了 swarm 模式，使您能够创建一个由一个或多个 Docker 引擎组成的集群，称为 swarm。一个 swarm 由一个或多个节点组成：在 swarm 模式下运行 Docker Engine 1.12 或更高版本的物理机或虚拟机。

有两种类型的节点：[**管理器**](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/#manager-nodes)和 [**工作**](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/#worker-nodes)器。 节点的概念

![Swarm 模式集群](http://cdn.lsbysl.com//swarm-diagram.png)

所有操作都在manager(管理节点上) 工作节点是没法操作的，管理节点之间是互通的

#### 11.2 raft一致性算法

> Raft 是一个非拜占庭的一致性算法，即所有通信是正确的而非伪造的。 N 个结点的情况下（N为奇数）可以最多容忍\((N-1)/2\) 个结点故障。 Raft 正常工作时的流程如上图，也就是正常情况下日志复制的流程。 Raft 中使用日志来记录所有操作，所有结点都有自己的日志列表来记录所有请求

#### 11.3 搭建集群示例

> 示例

> 集群搭建-两主两从

- 步骤一购买服务

![image-20220410091628906](http://cdn.lsbysl.com//image-20220410091628906.png)

- 步骤二 安装docker

> iTerm2 支持向多窗口（当前打开的所有窗口）同时输入相同的指令。只需要输入快捷键 ⌘(command) + ⇧(shift) + i

![image-20220410093547508](http://cdn.lsbysl.com//image-20220410093547508.png)

- 步骤三配置加速器

> 参考4.10

![image-20220410093715884](http://cdn.lsbysl.com//image-20220410093715884.png)

![image-20220410093745436](http://cdn.lsbysl.com//image-20220410093745436.png)

- 步骤四 查看docker网络信息

![image-20220410094844871](http://cdn.lsbysl.com//image-20220410094844871.png)

- 步骤五

> Swarm 命令

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker swarm --help

Usage:  docker swarm COMMAND

Manage Swarm

Commands:
  ca          Display and rotate the root CA
  init        Initialize a swarm # 初始化集群
  join        Join a swarm as a node and/or manager # 加入集群
  join-token  Manage join tokens
  leave       Leave the swarm # 离开集群
  unlock      Unlock swarm
  unlock-key  Manage the unlock key
  update      Update the swarm # 更新集群

Run 'docker swarm COMMAND --help' for more information on a command.
~~~

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker swarm init --help

Usage:  docker swarm init [OPTIONS]

Initialize a swarm

Options:
			# 地址
      --advertise-addr string                  Advertised address (format: <ip|interface>[:port])
      --autolock                               Enable manager autolocking (requiring an unlock
                                               key to start a stopped manager)
      --availability string                    Availability of the node
                                               ("active"|"pause"|"drain") (default "active")
      --cert-expiry duration                   Validity period for node certificates
                                               (ns|us|ms|s|m|h) (default 2160h0m0s)
      --data-path-addr string                  Address or interface to use for data path traffic
                                               (format: <ip|interface>)
      --data-path-port uint32                  Port number to use for data path traffic (1024 -
                                               49151). If no value is set or is set to 0, the
                                               default port (4789) is used.
      --default-addr-pool ipNetSlice           default address pool in CIDR format (default [])
      --default-addr-pool-mask-length uint32   default address pool subnet mask length (default 24)
      --dispatcher-heartbeat duration          Dispatcher heartbeat period (ns|us|ms|s|m|h)
                                               (default 5s)
      --external-ca external-ca                Specifications of one or more certificate signing
                                               endpoints
      --force-new-cluster                      Force create a new cluster from current state
      --listen-addr node-addr                  Listen address (format: <ip|interface>[:port])
                                               (default 0.0.0.0:2377)
      --max-snapshots uint                     Number of additional Raft snapshots to retain
      --snapshot-interval uint                 Number of log entries between Raft snapshots
                                               (default 10000)
      --task-history-limit int                 Task history retention limit (default 5)
~~~

- 步骤六绑定主节点

> docker swarm init --advertise-addr 172.24.228.99

~~~shell
# 初始化集群
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker swarm init --advertise-addr 172.24.228.99
#  Swarm 已初始化：当前节点 (0fc1j0ptly0hx7cjf1q7ogpi6) 现在是管理器。
Swarm initialized: current node (0fc1j0ptly0hx7cjf1q7ogpi6) is now a manager.
# 要将工作人员添加到此集群，请运行以下命令：
To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-3bcbmh6ca7byyr7427uk77rdicxwjz4s5i4wee627z3tx4c0u9-ebls9tk7k3psb91lok25akps5 172.24.228.99:2377
# 要将管理器添加到此 swarm，请运行“docker swarm join-token manager”并按照说明进行操作。
To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
~~~

> 命令总结

~~~shell
# 初始化节点
docker swarm init
# 加入一个节点
docker swarm join
# 获取令牌
docker swarm join-token manager # 管理员
docker swarm join-token worker # 工作
~~~

- 步骤七 从机加入到主节点

~~~shell
[root@iZbp1chrrv37a1icoiq5v0Z ~]# docker swarm join --token SWMTKN-1-3bcbmh6ca7byyr7427uk77rdicxwjz4s5i4wee627z3tx4c0u9-ebls9tk7k3psb91lok25akps5 172.24.228.99:2377
# 这个节点作为一个工人加入了一个群体。
This node joined a swarm as a worker.
~~~

- 步骤八 主节点查看node

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker node ls
ID                            HOSTNAME                  STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
ygj022pjtdirijxokren23vfg     iZbp1chrrv37a1icoiq5v0Z   Ready     Active                          20.10.14
0fc1j0ptly0hx7cjf1q7ogpi6 *   iZbp1chrrv37a1icoiq5v2Z   Ready     Active         Leader           20.10.14
~~~

- 步骤九 生成工作者令牌加入到集群中

~~~shell
# 主节点生成令牌
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker swarm join-token worker
To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-3bcbmh6ca7byyr7427uk77rdicxwjz4s5i4wee627z3tx4c0u9-ebls9tk7k3psb91lok25akps5 172.24.228.99:2377
# 3号机器加入到主节点
[root@iZbp1chrrv37a1icoiq5uzZ ~]# docker swarm join --token SWMTKN-1-3bcbmh6ca7byyr7427uk77rdicxwjz4s5i4wee627z3tx4c0u9-ebls9tk7k3psb91lok25akps5 172.24.228.99:2377
This node joined a swarm as a worker.
# 主节点查看node
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker node ls
ID                            HOSTNAME                  STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
n28tienf3npo6meb9r6wsb1jq     iZbp1chrrv37a1icoiq5uzZ   Ready     Active                          20.10.14
ygj022pjtdirijxokren23vfg     iZbp1chrrv37a1icoiq5v0Z   Ready     Active                          20.10.14
0fc1j0ptly0hx7cjf1q7ogpi6 *   iZbp1chrrv37a1icoiq5v2Z   Ready     Active         Leader           20.10.14
~~~

- 步骤十 生成主节点令牌加入到集群中

~~~shell
# 1号机主节点生成令牌
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker swarm join-token manager
To add a manager to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-3bcbmh6ca7byyr7427uk77rdicxwjz4s5i4wee627z3tx4c0u9-6uj375zuhc8n5071f275l01xy 172.24.228.99:2377
# 4号机作为主节点加入到集群中
[root@iZbp1chrrv37a1icoiq5v1Z ~]# docker swarm join --token SWMTKN-1-3bcbmh6ca7byyr7427uk77rdicxwjz4s5i4wee627z3tx4c0u9-6uj375zuhc8n5071f275l01xy 172.24.228.99:2377
This node joined a swarm as a manager.
# 1号机主节点查看node
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker node ls
ID                            HOSTNAME                  STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
n28tienf3npo6meb9r6wsb1jq     iZbp1chrrv37a1icoiq5uzZ   Ready     Active                          20.10.14
ygj022pjtdirijxokren23vfg     iZbp1chrrv37a1icoiq5v0Z   Ready     Active                          20.10.14
062ymometyo2xkvs7a67o2liy     iZbp1chrrv37a1icoiq5v1Z   Ready     Active         Reachable        20.10.14
0fc1j0ptly0hx7cjf1q7ogpi6 *   iZbp1chrrv37a1icoiq5v2Z   Ready     Active         Leader           20.10.14
~~~

#### 11.4 弹性,扩缩容！集群使用

- 步骤一 查看节点

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker node ls
ID                            HOSTNAME                  STATUS    AVAILABILITY   MANAGER STATUS   ENGINE VERSION
n28tienf3npo6meb9r6wsb1jq     iZbp1chrrv37a1icoiq5uzZ   Ready     Active                          20.10.14
ygj022pjtdirijxokren23vfg     iZbp1chrrv37a1icoiq5v0Z   Ready     Active                          20.10.14
062ymometyo2xkvs7a67o2liy     iZbp1chrrv37a1icoiq5v1Z   Ready     Active         Reachable        20.10.14
0fc1j0ptly0hx7cjf1q7ogpi6 *   iZbp1chrrv37a1icoiq5v2Z   Ready     Active         Leader           20.10.14
~~~

- 步骤二 查看service 命令

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker service --help

Usage:  docker service COMMAND

Manage services

Commands: 
              # 创建新服务
  create      Create a new service
              # 显示一项或多项服务的详细信息
  inspect     Display detailed information on one or more services
              # 获取服务或任务的日志
  logs        Fetch the logs of a service or task
              # 列出服务
  ls          List services
              # 列出一项或多项服务的任务
  ps          List the tasks of one or more services
              # 移除一个或多个服务
  rm          Remove one or more services
              # 恢复对服务配置的更改
  rollback    Revert changes to a service's configuration
              # 扩展一个或多个复制服务
  scale       Scale one or multiple replicated services
              # 更新一个服务
  update      Update a service

Run 'docker service COMMAND --help' for more information on a command.
~~~

- 步骤三 创建一个服务

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker service create -p 8888:80 --name my-nginx nginx
gtvl3tlt2dakofwr8aevw0vmz
overall progress: 1 out of 1 tasks
1/1: running
verify: Service converged
~~~

~~~shell
docker run # 容器启动！不具有扩缩容功能
docker service # 服务！具有扩缩容，滚动更新
~~~

- 步骤四 查看启动的服务REPLICAS

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker service ps my-nginx
ID             NAME         IMAGE          NODE                      DESIRED STATE   CURRENT STATE           ERROR     PORTS
d4ia5fe7qpd0   my-nginx.1   nginx:latest   iZbp1chrrv37a1icoiq5v2Z   Running         Running 2 minutes ago
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker service ls
ID             NAME       MODE         REPLICAS   IMAGE          PORTS
gtvl3tlt2dak   my-nginx   replicated   1/1        nginx:latest   *:8888->80/tcp
~~~

- 步骤五 查看服务的元数据

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker service inspect my-nginx
[
    {
        "ID": "gtvl3tlt2dakofwr8aevw0vmz",
        "Version": {
            "Index": 28
        },
        "CreatedAt": "2022-04-10T02:29:49.28654808Z",
        "UpdatedAt": "2022-04-10T02:29:49.291024023Z",
        "Spec": {
            "Name": "my-nginx",
            "Labels": {},
            "TaskTemplate": {
                "ContainerSpec": {
                    "Image": "nginx:latest@sha256:0d17b565c37bcbd895e9d92315a05c1c3c9a29f762b011a10c54a66cd53c9b31",
                    "Init": false,
                    "StopGracePeriod": 10000000000,
                    "DNSConfig": {},
                    "Isolation": "default"
                },
                "Resources": {
                    "Limits": {},
                    "Reservations": {}
                },
                "RestartPolicy": {
                    "Condition": "any",
                    "Delay": 5000000000,
                    "MaxAttempts": 0
                },
                "Placement": {
                    "Platforms": [
                        {
                            "Architecture": "amd64",
                            "OS": "linux"
                        },
                        {
                            "OS": "linux"
                        },
                        {
                            "OS": "linux"
                        },
                        {
                            "Architecture": "arm64",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "386",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "mips64le",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "ppc64le",
                            "OS": "linux"
                        },
                        {
                            "Architecture": "s390x",
                            "OS": "linux"
                        }
                    ]
                },
                "ForceUpdate": 0,
                "Runtime": "container"
            },
            "Mode": {
                "Replicated": {
                    "Replicas": 1
                }
            },
            "UpdateConfig": {
                "Parallelism": 1,
                "FailureAction": "pause",
                "Monitor": 5000000000,
                "MaxFailureRatio": 0,
                "Order": "stop-first"
            },
            "RollbackConfig": {
                "Parallelism": 1,
                "FailureAction": "pause",
                "Monitor": 5000000000,
                "MaxFailureRatio": 0,
                "Order": "stop-first"
            },
            "EndpointSpec": {
                "Mode": "vip",
                "Ports": [
                    {
                        "Protocol": "tcp",
                        "TargetPort": 80,
                        "PublishedPort": 8888,
                        "PublishMode": "ingress"
                    }
                ]
            }
        },
        "Endpoint": {
            "Spec": {
                "Mode": "vip",
                "Ports": [
                    {
                        "Protocol": "tcp",
                        "TargetPort": 80,
                        "PublishedPort": 8888,
                        "PublishMode": "ingress"
                    }
                ]
            },
            "Ports": [
                {
                    "Protocol": "tcp",
                    "TargetPort": 80,
                    "PublishedPort": 8888,
                    "PublishMode": "ingress"
                }
            ],
            "VirtualIPs": [
                {
                    "NetworkID": "6qrehrd6t6kl845s4wojygz6q",
                    "Addr": "10.0.0.6/24"
                }
            ]
        }
    }
]
~~~

- 步骤六 查看容器运行在哪台机器上

![image-20220410103655877](http://cdn.lsbysl.com//image-20220410103655877.png)

分配的目前运行在了一号机器上，测试多副本情况下

- 步骤七 创建多副本

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker service update --replicas 3 my-nginx
my-nginx
overall progress: 3 out of 3 tasks
1/3: running
2/3: running
3/3: running
verify: Service converged
~~~

![image-20220410104000931](http://cdn.lsbysl.com//image-20220410104000931.png)

可以看到1 2 3号机器都有容器在运行

- 步骤八 测试外部访问连通

> 集群内的所有机器都可以访问即使当前机器没有副本在运行

![image-20220410104651213](http://cdn.lsbysl.com//image-20220410104651213.png)

- 步骤九 动态扩缩容

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker service update --replicas 10 my-nginx
my-nginx
overall progress: 10 out of 10 tasks
1/10: running
2/10: running
3/10: running
4/10: running
5/10: running
6/10: running
7/10: running
8/10: running
9/10: running
10/10: running
verify: Service converged
~~~

![image-20220410104933775](http://cdn.lsbysl.com//image-20220410104933775.png)

- 步骤十 缩容

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker service update --replicas 1 my-nginx
my-nginx
overall progress: 1 out of 1 tasks
1/1: running
verify: Service converged
~~~

![image-20220410105210096](http://cdn.lsbysl.com//image-20220410105210096.png)

- 步骤十一 扩缩缩命令2

~~~shell
# 扩缩容同理
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker service scale my-nginx=5
my-nginx scaled to 5
overall progress: 5 out of 5 tasks
1/5: running
2/5: running
3/5: running
4/5: running
5/5: running
verify: Service converged
~~~

![image-20220410105618075](http://cdn.lsbysl.com//image-20220410105618075.png)

- 步骤十二 移除服务

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker service rm my-nginx
my-nginx
~~~

![image-20220410105843266](http://cdn.lsbysl.com//image-20220410105843266.png)

可以看到没有容器在运行

![image-20220410105945412](http://cdn.lsbysl.com//image-20220410105945412.png)

可以看到没有服务且工作节点不可以执行

#### 11.5 概念总结

> swarm

~~~tex
集群的管理和编号 docker可以初始化一个swarm集群，其他节可以加入(管理者，工作者)
~~~

> Node

~~~shell
就是一个docker节点，多个节点就组成了一个网络集群(工作者管理者)
~~~

> Service

~~~
任务，可以在管理节点或者工作节点来运行。核心！用户可以访问
~~~

> Task

~~~
容器内命令，细节任务
~~~

![image-20220410110748610](http://cdn.lsbysl.com//image-20220410110748610.png)





### 12 Docker Stack

> Docker-compose 单机部署项目
>
> Docker Stack部署，集群部署

~~~shell
# 单机
docker-compose up -d wordpress.yaml
# 集群
docker stack deploy wordpress.yaml
~~~

> 命令

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker stack --help

Usage:  docker stack [OPTIONS] COMMAND

Manage Docker stacks

Options:
      --orchestrator string   Orchestrator to use (swarm|kubernetes|all)

Commands:
              # 部署新堆栈或更新现有堆栈
  deploy      Deploy a new stack or update an existing stack
  ls          List stacks
  ps          List the tasks in the stack
  rm          Remove one or more stacks
  					  # 列出堆栈中的服务
  services    List the services in the stack

Run 'docker stack COMMAND --help' for more information on a command.
~~~

> 示例参考:https://zhuanlan.zhihu.com/p/182198031

### 13 Docker Secret

#### 13.1 Docker Secret作用

在动态的、大规模的分布式集群上，管理和分发 `密码`、`证书` 等敏感信息是极其重要的工作。传统的密钥分发方式（如密钥放入镜像中，设置环境变量，volume 动态挂载等）都存在着潜在的巨大的安全风险。

Docker 目前已经提供了 `secrets` 管理功能，用户可以在 Swarm 集群中安全地管理密码、密钥证书等敏感数据，并允许在多个 Docker 容器实例之间共享访问指定的敏感数据。

> 注意： `secret` 也可以在 `Docker Compose` 中使用。

我们可以用 `docker secret` 命令来管理敏感信息。接下来我们在上面章节中创建好的 Swarm 集群中介绍该命令的使用。

这里我们以在 Swarm 集群中部署 `mysql` 和 `wordpress` 服务为例。

> 命令

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker secret

Usage:  docker secret COMMAND

Manage Docker secrets

Commands:
							# 从文件或 STDIN 创建一个秘密作为内容
  create      Create a secret from a file or STDIN as content
              # 显示有关一个或多个秘密的详细信息
  inspect     Display detailed information on one or more secrets
  ls          List secrets
  rm          Remove one or more secrets

Run 'docker secret COMMAND --help' for more information on a command.
~~~

#### 13.2 使用案例

- 步骤一 创建secret

~~~shell
# 使用 docker secret create 命令以管道符的形式创建 secret
openssl rand -base64 20 | docker secret create mysql_password -
openssl rand -base64 20 | docker secret create mysql_root_password -
~~~

- 步骤二 查看secret

~~~shell
[root@lsbysl ~]# docker secret ls
ID                          NAME                  DRIVER    CREATED              UPDATED
a0xncucg5wrlf5sk3oj9ggfb5   mysql_password                  2 minutes ago        2 minutes ago
cf7l3h3vppbygaed3bullrypm   mysql_root_password             About a minute ago   About a minute ago
~~~

- 步骤三 创建mysql服务

~~~shell
# 创建mysql网络
[root@lsbysl ~]# docker network create -d overlay mysql_private
t3jecp68alwt6dp7sb9a322kz
# 创建mysql服务
[root@lsbysl ~]# docker service create \
>      --name mysql \
>      --replicas 1 \
>      --network mysql_private \
>      --mount type=volume,source=mydata,destination=/var/lib/mysql \
>      --secret source=mysql_root_password,target=mysql_root_password \
>      --secret source=mysql_password,target=mysql_password \
>      -e MYSQL_ROOT_PASSWORD_FILE="/run/secrets/mysql_root_password" \
>      -e MYSQL_PASSWORD_FILE="/run/secrets/mysql_password" \
>      -e MYSQL_USER="wordpress" \
>      -e MYSQL_DATABASE="wordpress" \
>      mysql:latest
dl1h4y3zxhl8x2r26pug6lej6
overall progress: 1 out of 1 tasks
1/1: running
verify: Service converged
~~~

如果你没有在 `target` 中显式的指定路径时，`secret` 默认通过 `tmpfs` 文件系统挂载到容器的 `/run/secrets` 目录中

- 步骤三 创建wordpress

~~~shell
[root@lsbysl ~]# docker service create \
>      --name wordpress \
>      --replicas 1 \
>      --network mysql_private \
>      --publish 9998:80 \
>      --mount type=volume,source=wpdata,destination=/var/www/html \
>      --secret source=mysql_password,target=wp_db_password,mode=0400 \
>      -e WORDPRESS_DB_USER="wordpress" \
>      -e WORDPRESS_DB_PASSWORD_FILE="/run/secrets/wp_db_password" \
>      -e WORDPRESS_DB_HOST="mysql:3306" \
>      -e WORDPRESS_DB_NAME="wordpress" \
>      wordpress:latest
21js3jt87z94coiz40kno96hi
overall progress: 1 out of 1 tasks
1/1: running
verify: Service converged
~~~

- 步骤四 查看服务

~~~shell
[root@lsbysl ~]# docker service ls
ID             NAME        MODE         REPLICAS   IMAGE              PORTS
dl1h4y3zxhl8   mysql       replicated   1/1        mysql:latest
21js3jt87z94   wordpress   replicated   1/1        wordpress:latest   *:9998->80/tcp
~~~

现在浏览器访问 `IP:9998`，即可开始 `WordPress` 的安装与使用。

通过以上方法，我们没有像以前通过设置环境变量来设置 MySQL 密码， 而是采用 `docker secret` 来设置密码，防范了密码泄露的风险。

### 14 Docker Config

#### 14.1 Docker config 作用

在动态的、大规模的分布式集群上，管理和分发配置文件也是很重要的工作。传统的配置文件分发方式（如配置文件放入镜像中，设置环境变量，volume 动态挂载等）都降低了镜像的通用性。

在 Docker 17.06 以上版本中，Docker 新增了 `docker config` 子命令来管理集群中的配置信息，以后你无需将配置文件放入镜像或挂载到容器中就可实现对服务的配置。

> 注意：`config` 仅能在 Swarm 集群中使用。

以在 Swarm 集群中部署 `redis` 服务为例。

> 命令

~~~shell
[root@iZbp1chrrv37a1icoiq5v2Z ~]# docker config

Usage:  docker config COMMAND

Manage Docker configs

Commands:
  create      Create a config from a file or STDIN
  inspect     Display detailed information on one or more configs
  ls          List configs
  rm          Remove one or more configs

Run 'docker config COMMAND --help' for more information on a command.
~~~

#### 14.2 使用案例

- 步骤一 创建config

~~~shell
# 创建文件写入内容
vim redis.conf
port 6380
# 此项配置 Redis 监听 6380 端口
# 我们使用 docker config create 命令创建 config
docker config create redis.conf redis.conf
~~~

- 步骤二 查看config

> 使用 `docker config ls` 命令来查看 `config`

~~~shell
docker config ls

ID                          NAME                CREATED             UPDATED
yod8fx8iiqtoo84jgwadp86yk   redis.conf          4 seconds ago       4 seconds ago
~~~

- 步骤三 创建redis服务

~~~shell
docker service create \
     --name redis \
     # --config source=redis.conf,target=/etc/redis.conf \
     --config redis.conf \
     -p 6379:6380 \
     redis:latest \
     redis-server /redis.conf
~~~

如果你没有在 `target` 中显式的指定路径时，默认的 `redis.conf` 以 `tmpfs` 文件系统挂载到容器的 `/config.conf`。

经过测试，redis 可以正常使用。

以前我们通过监听主机目录来配置 Redis，就需要在集群的每个节点放置该文件，如果采用 `docker config` 来管理服务的配置信息，我们只需在集群中的管理节点创建 `config`，当部署服务时，集群会自动的将配置文件分发到运行服务的各个节点中，大大降低了配置信息的管理和分发难度。



