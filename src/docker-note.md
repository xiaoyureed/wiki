---
title: Docker Note
tags:
  - docker
categories: distributed system
date: 2017-12-14 11:22:42
---

<div align="center">
Docker Note | reference: [1](https://docker_practice.gitee.io/image/dockerfile/healthcheck.html), [2](https://docs.docker.com/), [3](https://github.com/docker-library/docs)
https://blog.csdn.net/zmx729618/article/details/72930474

[docker online](https://labs.play-with-docker.com/)

[快速的实验环境](https://www.katacoda.com/)

https://github.com/wsargent/docker-cheat-sheet 速查表

https://github.com/docker/kitematic 可视化管理gui
</div>

<!--more-->

<!-- TOC -->

- [常用指令](#常用指令)
  - [开启远程连接](#开启远程连接)
  - [shipyard ui管理界面](#shipyard-ui管理界面)
- [介绍](#介绍)
  - [使用场景-得到什么好处](#使用场景-得到什么好处)
- [安装](#安装)
- [镜像 image](#镜像-image)
  - [镜像实现原理](#镜像实现原理)
  - [获取镜像](#获取镜像)
  - [运行镜像](#运行镜像)
  - [列出镜像](#列出镜像)
  - [删除本地镜像](#删除本地镜像)
- [制作镜像的方法](#制作镜像的方法)
  - [最佳实践 优化体积](#最佳实践-优化体积)
  - [通过 dockerfile 来 build 镜像](#通过-dockerfile-来-build-镜像)
  - [commit容器成为镜像-不推荐](#commit容器成为镜像-不推荐)
  - [从压缩包导入](#从压缩包导入)
  - [docker save 和 docker load](#docker-save-和-docker-load)
- [Dockerfile](#dockerfile)
  - [run 和 from](#run-和-from)
  - [镜像的构建上下文context](#镜像的构建上下文context)
  - [COPY 和 ADD](#copy-和-add)
  - [WORKDIR 指定工作目录](#workdir-指定工作目录)
  - [cmd 和 entrypoint 容器启动命令](#cmd-和-entrypoint-容器启动命令)
  - [ENV 和 arg 设置环境变量](#env-和-arg-设置环境变量)
  - [VOLUME 定义匿名数据卷](#volume-定义匿名数据卷)
  - [EXPOSE 声明端口](#expose-声明端口)
  - [USER 指定当前用户](#user-指定当前用户)
  - [HEALTHCHECK 健康检查](#healthcheck-健康检查)
  - [ONBUILD 给别人用](#onbuild-给别人用)
  - [Dockerfile的多阶段构建](#dockerfile的多阶段构建)
- [使用容器](#使用容器)
  - [查看](#查看)
  - [启动](#启动)
    - [-it 维持交互终端](#-it-维持交互终端)
    - [-d 后台运行](#-d-后台运行)
    - [--privileged=true 赋予特权](#--privilegedtrue-赋予特权)
  - [终止container](#终止container)
  - [删除container](#删除container)
  - [进入容器](#进入容器)
  - [容器的导入和导出](#容器的导入和导出)
  - [拷贝容器内文件到主机](#拷贝容器内文件到主机)
- [访问仓库](#访问仓库)
  - [dockerhub](#dockerhub)
  - [私有仓库](#私有仓库)
- [docker数据管理](#docker数据管理)
  - [数据卷](#数据卷)
  - [挂载主机目录](#挂载主机目录)
  - [volume 的共享](#volume-的共享)
- [网络互联](#网络互联)
  - [docker 默认初始创建](#docker-默认初始创建)
  - [网卡](#网卡)
  - [network分类](#network分类)
    - [bridge 类型](#bridge-类型)
    - [host 类型](#host-类型)
  - [创建固定ip](#创建固定ip)
  - [查看container的ip](#查看container的ip)
  - [外部访问容器](#外部访问容器)
  - [容器互联](#容器互联)
  - [配置dns 和 主机名 hostname](#配置dns-和-主机名-hostname)
- [docker-compose](#docker-compose)
  - [compose简介](#compose简介)
  - [命令使用](#命令使用)
    - [up](#up)
    - [run](#run)
    - [scale](#scale)
    - [start](#start)
    - [build](#build)
    - [config](#config)
    - [exec](#exec)
    - [images](#images)
    - [kill](#kill)
    - [down](#down)
    - [pause](#pause)
    - [unpause](#unpause)
    - [restart](#restart)
    - [stop](#stop)
    - [rm](#rm)
    - [logs](#logs)
    - [top](#top)
    - [version](#version)
    - [port](#port)
    - [ps](#ps)
    - [pull](#pull)
    - [push](#push)
  - [docker-compose.yml](#docker-composeyml)
- [docker-machine](#docker-machine)
- [docker-swarm,swarm-mode](#docker-swarmswarm-mode)
- [docker实践](#docker实践)
  - [搭建持续集成系统](#搭建持续集成系统)
  - [docker和springboot](#docker和springboot)
  - [oracle11g](#oracle11g)
  - [使用 mysql 镜像](#使用-mysql-镜像)
  - [使用postgres镜像](#使用postgres镜像)
  - [使用 rabbitmq镜像](#使用-rabbitmq镜像)
  - [redis镜像](#redis镜像)
  - [ubuntu镜像](#ubuntu镜像)
  - [zookeeper](#zookeeper)
  - [rabbitmq](#rabbitmq)
  - [zipkin](#zipkin)
  - [nginx](#nginx)
  - [mongodb](#mongodb)

<!-- /TOC -->

# 常用指令

```sh
docker -v # 版本
docker version # client, server 版本

docker stats # 监视容器 CPU 内存使用率


########## 更新 容器启动配置

# 容器随 docker 自动启动
docker update <container> --restart=always


######################### prune 命令 


# [-f] 强制
# [--filter "until=24h"] 删除24h以前的; [--filter "label!=keep"]只删除标签不为keep的
docker container prune
docker images prune
docker volume prune
docker network prune
docker system prune     # 删除以上所有空闲



####################### image 相关



# 拉取 docker pull
docker pull <image>:<tag>  

# 查看镜像
docker images 
docker image ls [image_name][:tag]       #查看所有/指定的镜像
docker image ls -a                    #显示包括中间层镜像在内的所有镜像
# 自定义结果镜像查询
docker image ls --format "{{.ID}}: {{.Repository}}"    # 列出镜像结果，并且只包含镜像ID和仓库名
docker image ls --format "table {{.ID}}\t{{.Repository}}\t{{.Tag}}" #带标题, 但是自己定义列
docker image ls -q                        # 仅仅显示image的id

# 删除镜像
docker image rm <xxx>                           删除指定镜像(可以是id, name....)
docker image rm $(docker image ls -q redis)     和其他命令配合删除
docker image rm $(docker image ls -q -f before=mongo:3.2) 配合
docker image prune                              批量清理临时镜像文件



# ###########################docker machine的命令

docker-machine ls                                  #查看运行docker server的虚拟机
docker-machine create --driver=virtualbox default  #创建一个虚拟机用于运行docker
docker-machine env [default]          #查看环境变量(机器ip在里面可以看到), 默认查看名为default的机器



############################ docker 命令



docker ps | less -S                 #查看所有运行的container, 每条记录只占用一行显示
docker ps -a                       # 查看所有(包括停止的container)
docker system df             #查看镜像、容器、数据卷所占用的空间, 运行状况(通过表格的形式)(常用) 
docker info        # docker的基本信息(多少container, 多少image, 各个container状况 ,docker root dir)
docker network ls                   #查看docker网络
docker build -t <nginx:v3> <.>      #根据Dockerfile构建镜像
    # -t 指定镜像名称
    # . 表示构建上下文 context 为当前目录
docker top <id, container_name>



######################## docker run/exec



docker run <IMAGE> env                              查看镜像支持的环境变量
docker run --name webserver -d -p <host>:<container> nginx       指定container名字运行, 端口映射
    * `-d` 后台运行(不加, 则前台运行, 日志输出到前台, ctrl+c不会终止container, 但会终止日志输出)
    * `-p` 指定端口映射
    * `--name` 指定container名字
docker run <image_name>:<tag> <cmd>                 运行指定镜像启动容器, 并执行一条指令
docker run image_name apt-get install -y app_name 运行镜像, 在容器中安装新的程序, 完成后退出
                                                    * -y : 不要进入交互模式
docker exec -itd <container_name/id> <cmd>        进入容器, 执行命令后退出
                                                        * -d : 后台运行, 去掉后, cmd执行完成后会呆在container内部
docker run -it --rm <image_name>:<tag> bash         运行并进入指定容器内部, 打开bash
                                                        * -i：交互式操作
                                                        * -t 终端
                                                        * --rm：容器退出后随之将其删除
docker exec -it webserver bash                      进入某个容器内, 并执行一条命令, 以`exit` 退出
                                                        * 不推荐 docker attach <xxx>, exit退出, container也随之终止

docker cp <container_name/id>:<container_path> <host_path>      复制文件, 从container到本机




# docker container

docker container ls 或 docker ps                    运行着的容器
                                                        * -q : 只返回容器id
docker container start <stoped_container>           启动终止状态的容器
docker container restart <xxx>                      重启
docker-compose restart <service_name>               必须在上下文目录, 是service name, 而非container_name

docker stop <xxx>                                   停止容器
docker container stop <xxx>
docker start/restart <xxx>                          启动
docker kill $(docker ps -a -q)                      停止所有正在运行的容器
docker rm <xxx>                               删除容器
docker rm -fv <容器ID>               停止、删除容器，并清除数据 
docker container rm <xxx>
docker container prune                              删除所有的停止的容器


# docker inspect 容器详细信息(json格式)

docker inspect <container>                                              容器详细信息(json格式, 大量信息 包括ip)
docker inspect -f='{{.Name}}' $(sudo docker ps -a -q)                   查看所有容器name (可以不带等号)
docker inspect --format '{{ .NetworkSettings.IPAddress }}'  <container> 获取指定容器ip
                                                                            * 用 $(sudo docker ps -a -q) 替换<container> 获取所有container的ip
docker inspect <id,container_name> | grep "IPAddress"                   查看ip



# docker commit 提交 docker history 提交历史


docker commit \                                                 # 提交修改后的容器形成新的镜像
        --author "Tao Wang <twang2218@gmail.com>" \ # 修改人
        --message "修改了默认网页" \ # 修改说明
        webserver \ # 修改的container
        nginx:v2 # 仓库名:标签名
        
docker history nginx:v2                 查看指定镜像的历史提交记录

docker rename <old容器名>  <new容器名> 修改容器名称



# docker log 查看日志

docker logs <xxx>
docker container logs <xxx>            #  查看指定container的日志(全部)
docker logs -f -t --since="2017-05-31" --tail=10 <container>   # 追踪日志文件
    #* -f : 查看实时日志
    #* -t : 带上时间戳
    #* --since : 此参数指定了输出日志开始日期
    #* --tail=10 : 后10行



# 网络相关命令

docker network create -d <network-type> --subnet=172.18.0.0/16 <my-network> # 创建网络
    * -d 网络类型, 默认 bridge
    * --subnet 指定网段
docker network rm my-net # 删除
docker network connect my-net my-nginx # 运行着的 container 加入网络
docker network disconnect my-net my-nginx # 断开网络

docker run -itd --name <test1> --network bridge --ip 172.17.0.10 <centos:latest> </bin/bash >
    * 用 --network 参数，可以指定网络类型
    * --ip 172.17.0.10指定ip

```



## 开启远程连接

用于开发环境, 不安全

原生 docker: https://blog.csdn.net/yjt520557/article/details/99715097 或者 https://www.cnblogs.com/weiyiming007/p/10168733.html

wsl 下的 docker: https://www.jianshu.com/p/c7bc8fa1ee5f 修改 `/lib/systemd/system/docker.service`

## shipyard ui管理界面

web 管理控制台, 管理容器, 默认用户名密码 admin/shipyard

同类: docker-ui

# 介绍

docker client和docker server: docker是c/s架构, client和server可以在同一台机器也可以不再同一台机器

Repository: 具体某个镜像的仓库, 只存储某一个项目的镜像(比如Tomcat下面有很多个版本(用tag表示)的镜像，它们共同组成了Tomcat的Repository。)

Registry: 所有的镜像, 官方为Docker Hub, 也可以部署私有registry(Registry上有很多的Repository，Redis、Tomcat、MySQL等等Repository组成了Registry。)

## 使用场景-得到什么好处

docker是什么? 轻量化的虚拟机

传统虚拟机在宿主操作系统上先虚拟出一套硬件, 硬件上再运行操作系统，在该系统上再运行所需应用进程；

而 docker 容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，而且也没有进行硬件虚拟。因此容器要比传统虚拟机更为轻便

<img src="Snipaste_2018-04-19_16-04-19.png" width=50% />




* 在同一台服务器上进行资源隔离, 比如限制每个应用(比如网站, Restful api等等)最大内存使用量 ` 节省系统资源`

* Docker给应用提供了一个从开发到上线  `统一的环境`

* 数据库的数据与数据库程序本身分离开：用一个container A作为数据存储，然后另一个container B运行数据库。当你想升级数据库时，用新的container C替换掉container B即可 . `方便的迁移应用`

* [把Docker镜像当作桌面系统使用](https://zasdfgbnm.github.io/2017/12/28/%E6%8A%8Adocker%E9%95%9C%E5%83%8F%E5%BD%93%E4%BD%9C%E6%A1%8C%E9%9D%A2%E7%B3%BB%E7%BB%9F%E6%9D%A5%E7%94%A8/)

* 使用docker build完成单元测试(创建隔离的环境进行测试, 如用jenkins ci这样的ci工具启动一个用于测试的container)

* 构建paas, saas

* 尤其适合微服务

    * 后端应用是微服务化的，有很多个独立的 java 应用，开发、测试和线上多套环境。

    * docker 之前的部署方案: 打 war 包，上传到 ecs，重启tomcat
    
    * docker 部署方案：编译机器上编译 jar 包，打出 docker-img（把jar包放进去）, push 到 docker registry，线上机器pull docker-img, 然后 docker stop， docker run。


# 安装

ailiyun加速https://7lwsinal.mirror.aliyuncs.com

安装docker前检查环境是否匹配:

* `uname -a` 查看内核版本
* 升级内核版本: 先通过`sudo apt-get update` 更新atp-get, 然后通过`sudo atp-get install <指定linux内核>`安装, 最后通过`sudo update-grub`加载新内核. 还需要重启`sudo reboot`
* 检查是否以device mapper作为存储驱动 `grep device-mapper /proc/devices`


安装参考: https://docs.docker.com/engine/install/centos/#install-using-the-convenience-script

最简单的 `curl -fsSL https://get.docker.com -o get-docker.sh && sudo sh get-docker.sh`

```sh
sudo systemctl enable docker
sudo systemctl start docker
docker run hello-world

```

使用国内镜像

临时使用 `docker run hello-world --registry-mirror=https://docker.mirrors.ustc.edu.cn`

持久使用, 修改 or 新增 `vim /etc/docker/daemon.json`, 

```json
{ 

"registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"] 

}


{
  "registry-mirrors": [
    "https://registry.docker-cn.com",
    "http://hub-mirror.c.163.com",
    "https://docker.mirrors.ustc.edu.cn"
  ]
}

systemctl daemon-reload
systemctl restart docker
```


https://www.cnblogs.com/nihaorz/p/12131873.html

# 镜像 image

可以通过Dockerfile构建image，也可以将image运行，使其变成container

## 镜像实现原理

docker 镜像都是层层叠加，每一层都可能被不同的镜像共享 (在Dockerfile中每一步都会产生一层layer，每一步的结果产出变成文件)

Docker 使用 Union FS 将这些不同的层结合到一个镜像中去。

通常 Union FS 有两个用途, 一方面可以实现不借助 LVM、RAID 将多个 disk 挂到同一个目录下,另一个更常用的就是将一个只读的分支和一个可写的分支联合在一起，Live CD 正是基于此方法可以允许在镜像不变的基础上允许用户在其上进行一些写操作。

## 获取镜像

通过 `docker pull --help` 查询可知格式为:

`docker pull [选项] [Docker Registry地址 [:端口号] / ] 仓库名[:标签]`

* Docker 镜像仓库地址的格式一般是 <域名/IP>[:端口号]。默认地址是 Docker Hub。

* 仓库名：如之前所说，这里的仓库名是两段式名称，即 <用户名>/<软件名>。对于 Docker Hub，如果不给出用户名，则默认为 library，也就是官方镜像。

例如: 

docker pull ubuntu:16.04

上面的命令中没有给出 Docker 镜像仓库地址，因此将会从 Docker Hub 获取镜像。而镜像名称是 ubuntu:16.04，因此将会获取官方镜像 library/ubuntu 仓库中标签为 16.04 的镜像。

从下载过程中可以看到我们之前提及的分层存储的概念，镜像是由多层存储所构成。下载也是一层层的去下载，并非单一文件。下载过程中给出了每一层的 ID 的前 12 位。并且下载结束后，给出该镜像完整的 sha256 的摘要，以确保下载一致性

## 运行镜像

```sh
# 以上面的 ubuntu:16.04 为例，如果我们打算启动里面的 bash 并且进行交互式操作的话，可以执行下面的命令
$ docker run -it --rm \
    ubuntu:16.04 \
    bash
```

## 列出镜像

* `docker image ls [xxx:xxx]` 列出所有/指定下载的顶层镜像

* `docker image ls -a` 列出所有包括中间层镜像

* `docker images` 也可查看

## 删除本地镜像

`docker image rm [选项] <镜像1> [<镜像2> ...]`

# 制作镜像的方法

## 最佳实践 优化体积

https://www.infoq.cn/article/3-simple-tricks-for-smaller-docker-images
https://segmentfault.com/a/1190000017858358
https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
https://fuckcloudnative.io/posts/docker-images-part1-reducing-image-size/
TODO

```dockerfile
# 编译
FROM node:latest AS builder
maintainer xiaoyu <775000738@qq.com>
WORKDIR /usr/src/app
# 
COPY package.json yarn.lock ./
RUN yarn
# 复制当前路径(通过 docker build -t xxx . 这个 点 指定)下所有文件到 目录 ${workdir}
COPY . ./
RUN yarn build

# 实际镜像
FROM nginx
COPY --from=builder /usr/src/app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
CMD ["nginx", "-g", "daemon off;"]



```

## 通过 dockerfile 来 build 镜像

```sh
docker build -f MyDockerfile -t xiaoyureed/myImage:v1 .

# 简写
# 加入当前就在 dockerfile 所在目录
docker build -t imgName .
```

## commit容器成为镜像-不推荐

将自定义的容器保存为镜像, 一般不用commit制作镜像, 因为会有大量的无关内容被添加进来，如果不小心地清理，将会导致镜像极为臃肿。)

`docker commit [选项] <容器ID或容器名> [<仓库名>[:<标签>]]`

镜像是多层存储，每一层是在前一层的基础上进行的修改；而容器同样也是多层存储，是在以镜像为基础层，在其基础上加一层作为容器运行时的存储层

先看一个例子: 定制一个 web server

```sh
# 用 nginx 镜像启动一个容器，命名为 webserver，并且映射了 80 端口，这样我们可以用浏览器去访问这个 nginx 服务器
$ docker run --name webserver -d -p 80:80 nginx

# 进入container内部, 打开bash
docker exec -it webserver bash
# 用 <h1>Hello, Docker!</h1> 覆盖了 /usr/share/nginx/html/index.html 的内容
echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html

```

浏览器输入虚拟机的ip地址即可访问;(`docker-machine env`可以查看虚拟机ip)

现在希望commit这个新的镜像

```sh
# 先退出container
exit

# 看下图
docker commit --author "xy<775000738@qq.com>" --message "修改 index.html" webserver nginx:v2

# 启动第二个container, 浏览器访问81端口;
# 至此, 手动操作给旧的镜像添加了新的一层，形成新的镜像
docker run --name test_1 -d -p 81:80 nginx:v2
```

<img src="Snipaste_2018-04-19_20-33-52.png" width=50% />

`docker history nginx:v2` 查看某个Repository的历史提交记录


## 从压缩包导入

格式：docker import [选项] <文件>|<URL>|- [<仓库名>[:<标签>]]

eg:

```sh
# 这条命令自动下载了 ubuntu-14.04-x86_64-minimal.tar.gz 文件，并且作为根文件系统展开导入，并保存为镜像 openvz/ubuntu:14.04
$ docker import \
    http://download.openvz.org/template/precreated/ubuntu-14.04-x86_64-minimal.tar.gz \
    openvz/ubuntu:14.04
```

## docker save 和 docker load

Docker 还提供了 docker load 和 docker save 命令，用以将镜像保存为一个 tar 文件，然后传输到另一个位置上，再加载进来。这是在没有 Docker Registry 时的做法，现在已经不推荐，镜像迁移应该直接使用 Docker Registry，无论是直接使用 Docker Hub 还是使用内网私有 Registry 

`docker save alpine | gzip > alpine-latest.tar.gz` 保存之后复制到其他机器, `docker load -i alpine-latest.tar.gz`加载镜像

`docker save <镜像名> | bzip2 | pv | ssh <用户名>@<主机名> 'cat | docker load'`从一个机器将镜像迁移到另一个机器，并且带进度条的功能



# Dockerfile

```dockerfile
FROM node:latest AS builder
WORKDIR /usr/src/app
COPY package.json yarn.lock ./
RUN yarn
COPY . ./
RUN yarn build

FROM nginx
COPY --from=builder /usr/src/app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
CMD ["nginx", "-g", "daemon off;"]

```

通过dockerfile来build定制镜像

是  一种构建image的 文件DSL

Dockerfile 是一个文本文件，其内包含了一条条的指令(Instruction)，`每一条指令构建一层`，因此每一条指令的内容，就是描述该层应当如何构建。

## run 和 from


```sh
$ mkdir mynginx
$ cd mynginx

# Dockerfile的内容如下:
# FROM nginx
# RUN echo '<h1>Hello, Docker!</h1>' > /usr/share/nginx/html/index.html
$ touch Dockerfile
```

```sh
`FROM`指令: 指定基础镜像; 一个 Dockerfile 中 FROM 是必备的指令，并且必须是第一条指令。

    Docker 还存在一个特殊的镜像，名为 scratch。这个镜像是虚拟的概念，并不实际存在，它表示一个空白的镜像. 如果你以 scratch 为基础镜像的话，意味着你不以任何镜像为基础，接下来所写的指令将作为镜像第一层开始存在。

    可以 from xxx as bbb, 后面可以引用 ${bbb}

`RUN`指令: 执行命令 (每一个 RUN 都是启动一个容器 -> 执行命令 -> 然后提交存储层文件变更)

    两种格式 , shell 格式：RUN <命令>; 就像直接在命令行中输入的命令一样

    # 所有的命令只有一个目的，就是编译、安装 redis 可执行文件。
    FROM debian:jessie

    RUN buildDeps='gcc libc6-dev make' \
    && apt-get update \
    && apt-get install -y $buildDeps \
    && wget -O redis.tar.gz "http://download.redis.io/releases/redis-3.2.5.tar.gz" \
    && mkdir -p /usr/src/redis \
    && tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 \
    && make -C /usr/src/redis \
    && make -C /usr/src/redis install \
    # 必要的清理工作, 保证每一层没有垃圾
    && rm -rf /var/lib/apt/lists/* \
    && rm redis.tar.gz \
    && rm -r /usr/src/redis \
    && apt-get purge -y --auto-remove $buildDeps

    还有一种 exec 格式：RUN ["可执行文件", "参数1", "参数2"]，这更像是函数调用中的格式


```


编写完毕, 可以键入构建命令: `docker build [选项] < 上下文路径/URL/- >`

```sh
# 在dockerfile所在的目录运行, -t表示指定最终镜像名称, 这里的[.]表示上下文路径
docker build -t nginx:v3 .
```

## 镜像的构建上下文context

docker build 命令构建镜像，其实并非在本地构建，而是在服务端，也就是 Docker 引擎中构建的。那么在这种客户端/服务端的架构中，如何才能让服务端获得本地文件呢？

`docker build -t nginx:v3 .` 这里的[.]不是命令当前所在的目录, 而是指定docker context路径, Dockerfile中的路径都是相对于这个contxt路径来说的

一般应该会将 Dockerfile 置于一个空目录下，或者项目根目录下。如果该目录下没有所需文件，那么应该把所需文件复制一份过来。如果目录下有些东西确实不希望构建时传给 Docker 引擎，那么可以用 .gitignore 一样的语法写一个 `.dockerignore`，该文件是用于剔除不需要作为上下文传递给 Docker 引擎的。

在默认情况下，如果不额外指定 Dockerfile 的话，会将上下文目录下的名为 Dockerfile 的文件作为 Dockerfile;实际上 Dockerfile 的文件名并不要求必须为 Dockerfile，而且并不要求必须位于上下文目录中，比如可以用 `-f ../Dockerfile.php` 参数指定某个文件作为 Dockerfile。

docker build还支持从git repo中拉取文件构建`docker build https://github.com/twang2218/gitlab-ce-zh.git#:8.14`这行命令指定了构建所需的 Git repo，并且指定默认的 master 分支，构建目录为 /8.14/，然后 Docker 就会自己去 git clone 这个项目、切换到指定分支、并进入到指定目录后开始构建

还支持`docker build http://server/context.tar.gz`从压缩包构建


## COPY 和 ADD 

复制文件

和 RUN 指令一样，也有两种格式，一种类似于命令行，一种类似于函数调用:

COPY <源路径>... <目标路径>

COPY ["<源路径1>",... "<目标路径>"]

<源路径> 可以是多个，甚至可以是通配符，其通配符规则要满足 Go 的 filepath.Match 规则; 

<目标路径> 可以是容器内的绝对路径，也可以是相对于工作目录的相对路径（工作目录可以用 WORKDIR 指令来指定）。目标路径不需要事先创建，如果目录不存在会在复制文件前先行创建缺失目录

eg:
`COPY package.json /usr/src/app/`
`COPY hom* /mydir/`
`COPY hom?.txt /mydir/`


ADD 更高级的复制文件, 和copy类似-------但是, 可以自动解压缩

## WORKDIR 指定工作目录

使用 WORKDIR 指令可以来指定工作目录（或者称为当前目录），就是将当前执行命令的目录定义为确定值, 以后各层的当前目录就被改为指定的目录

格式为 `WORKDIR <工作目录路径>`。(如该目录不存在，WORKDIR 会帮你建立目录)

典型错误: 如下, 是错误的, 两条run命令在内存上实际是没有联系的

```sh
# 希望在 /app 下创建文件
RUN cd /app
RUN echo "hello" > world.txt
```

修改:

```sh
WORKDIR /app
RUN echo "hello" > world.txt
```


## cmd 和 entrypoint 容器启动命令

先看cmd指令

Docker 不是虚拟机，容器就是进程。既然是进程，那么在启动容器的时候，需要指定所运行的程序及参数。CMD 指令就是用于指定默认的容器主进程的启动命令

CMD 指令的格式和 RUN 相似，：

* shell 格式：CMD <命令>

* exec 格式：CMD ["可执行文件", "参数1", "参数2"...]-------推荐这种

    * 可执行文件为容器的主进程, 它结束了, 容器也就退出了
    
    * 第一种会被docker转换为这种方式. 如 CMD echo $HOME 会转为 CMD [ "sh", "-c", "echo $HOME" ]

* 参数列表格式：CMD ["参数1", "参数2"...]。在使用 ENTRYPOINT 指定了起使命令后，用 CMD 指定具体的参数。


在运行时可以指定新的命令来替代镜像设置中的这个默认命令，比如，ubuntu 镜像默认的 CMD 是 /bin/bash，如果我们直接` docker run -it ubuntu` 的话，会直接进入 bash。我们也可以在运行时指定运行别的命令，如 `docker run -it ubuntu cat /etc/os-release`。这就是用 cat /etc/os-release 命令替换了默认的 /bin/bash 命令了，输出了系统版本信息。

eg:
`CMD ["nginx", "-g", "daemon off;"]` 直接执行 nginx 可执行文件，并且要求以前台形式运行(CMD service nginx start是错的, 此时是"sh"这只程序作为主程序, 它运行结束, 容器就退出了)




再看看ENTRYPOINT 入口点

和CMD效果类似, 只是可以将命令行中镜像名后接的cmd当作参数传给Dockerfile中的enterpoint

eg1: 让镜像变成像命令一样使用

```sh
# 获取自己的公网ip, Dockerfile如下:
FROM ubuntu:16.04
RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
CMD [ "curl", "-s", "http://ip.cn" ]

# 构建镜像 
$ docker build -t myip .

# 测试:查询ip
$ docker run myip

# 但是上面的命令无法在后面接curl的参数, 比如直接在后面加"-i", 会把默认的cmd整个替换而出错, 不完美的解决方法如下:
$ docker run myip curl -s http://ip.cn -i

## 上面的命令太啰嗦了, 正确的方法是修改Dockerfile, 用ENTERYPOINT代替CMD
FROM ubuntu:16.04
RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
ENTRYPOINT [ "curl", "-s", "http://ip.cn" ]

# 现在可以正常使用"-i"参数了
$ docker run myip -i
```

eg2: 应用运行前的准备工作(这些准备工作是和容器 CMD 无关的，无论 CMD 为什么，都需要事先进行一个预处理的工作。这种情况下，可以写一个脚本，然后放入 ENTRYPOINT 中去执行，而这个脚本会将接到的参数（也就是 `<CMD>`）作为命令，在脚本最后执行)

```sh
# 可以看到其中为了 redis 服务创建了 redis 用户，并在最后指定了 ENTRYPOINT 为 docker-entrypoint.sh 脚本。
FROM alpine:3.4
...
RUN addgroup -S redis && adduser -S -G redis redis
...
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 6379
CMD [ "redis-server" ]

# docker-entrypoint.sh ,该脚本的内容就是根据 CMD 的内容来判断，如果是 redis-server 的话，则切换到 redis 用户身份启动服务器，否则依旧使用 root 身份执行

#!/bin/sh
...
# allow the container to be started with `--user`
if [ "$1" = 'redis-server' -a "$(id -u)" = '0' ]; then
    chown -R redis .
    exec su-exec redis "$0" "$@"
fi

exec "$@"
```

## ENV 和 arg 设置环境变量

env 在container的生命周期内始终有效

在 dockerfile 声明后, 后面的指令中可以使用

格式有两种：

* `ENV <key> <value>`
* `ENV <key1>=<value1> <key2>=<value2>...`

eg:
```sh
# 简单使用
ENV VERSION=1.0 DEBUG=on \
    NAME="Happy Feet"

# 官方node镜像
ENV NODE_VERSION 7.2.0

RUN curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
  && curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
  && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
  && grep " node-v$NODE_VERSION-linux-x64.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
  && tar -xJf "node-v$NODE_VERSION-linux-x64.tar.xz" -C /usr/local --strip-components=1 \
  && rm "node-v$NODE_VERSION-linux-x64.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt \
  && ln -s /usr/local/bin/node /usr/local/bin/nodejs
```



ARG 构建参数, 定义参数名称，以及定义其默认值。该默认值可以在构建命令 docker build 中用 `--build-arg <参数名>=<值>` 来覆盖。

和 ENV 类似, 但是 ARG 所设置的构建环境的环境变量，`在将来容器运行时是不会存在这些环境变量的`. 但是不要因此就使用 ARG 保存密码之类的信息，因为 docker history 还是可以看到所有值的。


## VOLUME 定义匿名数据卷

对于数据库类需要保存动态数据的应用，其数据库文件/动态数据应该保存于卷(volume)中, 而不要直接写入容器存储层

为了防止运行时用户忘记将动态文件所保存目录挂载为卷，在 Dockerfile 中，我们可以事先指定某些目录挂载到匿名卷，这样在运行时如果用户不指定挂载,动态数据会写入匿名卷

格式为：

*   `VOLUME ["<路径1>", "<路径2>"...]`
*   `VOLUME <路径>`

`VOLUME /data` 这里的 /data 目录是容器内的目录, 会在运行时自动挂载为匿名卷，任何向 /data 中写入的信息都不会记录进容器存储层，从而保证了容器存储层的无状态化。可以进入 container, 使用 `ll` 查看, 会发现容器自动创建了目录 /data, 并挂载为匿名数据卷

当然，运行时可以覆盖这个挂载设置`docker run -d -v mydata:/data xxxx`在这行命令中，就使用了 mydata 这个命名卷挂载到了 /data 这个位置，替代了 Dockerfile 中定义的匿名卷的挂载配置

为什么在 dockerfile 中无法使用 类似 `docker -run -v /host_path:/container_path` 的效果? 这时出于可移植和分享的考虑, 因为不是每个 宿主机都一定有这样的 "host_path" 特定目录

匿名数据卷的数据在宿主机哪里呢? `docker inspect <container>` 可以在json的 volumes 值看到

## EXPOSE 声明端口

格式为` EXPOSE <端口1> [<端口2>...]`。

EXPOSE 指令是声明运行时容器提供服务的端口, 这只是一个声明，在运行时并不会因为这个声明应用就会开启这个端口的服务

要将 EXPOSE 和在运行时使用` -p <宿主端口>:<容器端口>` 区分开来。-p，是映射宿主端口和容器端口，换句话说，就是将容器的对应端口服务公开给外界访问，而 EXPOSE 仅仅是声明容器打算使用什么端口而已，并不会自动在宿主进行端口映射



那既然只是一个声明, 并没有实际暴露端口, 有什么用呢? 在 Dockerfile 中写入这样的声明有两个好处

* 一个是帮助镜像使用者理解这个镜像服务的守护端口，以方便配置映射；
    
* 另一个用处则是在运行时使用随机端口映射时，也就是 docker run -P 时，会自动随机映射 EXPOSE 的端口


## USER 指定当前用户

USER指令用于指定容器执行程序的用户身份，默认是 root用户。

在docker run 中可以通过 -u 选项来覆盖USER指令的设置。

举例：docker run -i -t -u mysql newmysqldb /bin/bash

显示的shell提示符是： mysql@57cd57edba38:/$

注意：docker容器中的root用户密码是随机分配的。




USER 指令和 WORKDIR 相似，都是改变环境状态并影响以后的层。WORKDIR 是改变工作目录，USER 则是改变之后层的执行 RUN, CMD 以及 ENTRYPOINT 这类命令的身份

格式：USER <用户名> (这个用户必须是事先建立好的，否则无法切换。)

eg1:

```sh
RUN groupadd -r redis && useradd -r -g redis redis
USER redis
RUN [ "redis-server" ]
```

eg2:希望以某个已经建立好的用户来运行某个服务进程，不要使用 su 或者 sudo，这些都需要比较麻烦的配置，而且在 TTY 缺失的环境下经常出错。建议使用 gosu

```sh
# 建立 redis 用户，并使用 gosu 换另一个用户执行命令
RUN groupadd -r redis && useradd -r -g redis redis
# 下载 gosu
RUN wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/1.7/gosu-amd64" \
    && chmod +x /usr/local/bin/gosu \
    && gosu nobody true
# 设置 CMD，并以另外的用户执行
CMD [ "exec", "gosu", "redis", "redis-server" ]
```

## HEALTHCHECK 健康检查

HEALTHCHECK 指令是告诉 Docker 应该如何进行判断容器的状态是否正常(来自 1.12 之后，Docker 提供了 HEALTHCHECK 指令，通过该指令指定一行命令，用这行命令来判断容器主进程的服务状态是否还正常，从而比较真实的反应容器实际状态。)

为什么出现呢? 

在没有 HEALTHCHECK 指令前，Docker 引擎只可以通过容器内主进程是否退出来判断容器是否状态异常。很多情况下这没问题，但是如果程序进入死锁状态，或者死循环状态，应用进程并不退出，但是该容器已经无法提供服务了。在 1.12 以前，Docker 不会检测到容器的这种状态，从而不会重新调度，导致可能会有部分容器已经无法提供服务了却还在接受用户请求

生效的原理: 当在一个镜像指定了 HEALTHCHECK 指令后，用其启动容器，初始状态会为 `starting`，在 HEALTHCHECK 指令检查成功后变为 `healthy`，如果连续一定次数失败，则会变为 `unhealthy`。

和 CMD, ENTRYPOINT 一样，HEALTHCHECK 只可以出现一次，如果写了多个，只有最后一个生效

格式：

`HEALTHCHECK [选项] CMD <命令>`：设置检查容器健康状况的命令; 命令部分的格式和 ENTRYPOINT 一样，分为 shell 格式，和 exec 格式。命令的返回值决定了该次健康检查的成功与否：0：成功；1：失败

```sh
# 设置了每 5 秒检查一次（这里为了试验所以间隔非常短，实际应该相对较长），如果健康检查命令超过 3 秒没响应就视为失败，并且使用 curl -fs http://localhost/ || exit 1 作为健康检查命令
FROM nginx
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*
HEALTHCHECK --interval=5s --timeout=3s \
CMD curl -fs http://localhost/ || exit 1

```

支持如下选项:

- `--interval=<间隔>`：两次健康检查的间隔，默认为 30 秒；
- `--timeout=<时长>`：健康检查命令运行超时时间，如果超过这个时间，本次健康检查就被视为失败，默认 30 秒；
- `--retries=<次数>`：当连续失败指定次数后，则将容器状态视为 unhealthy，默认 3 次。

查看健康检查日志:

```sh
$ docker inspect --format '{{json .State.Health}}' web | python -m json.tool
{
    "FailingStreak": 0,
    "Log": [
        {
            "End": "2016-11-25T14:35:37.940957051Z",
            "ExitCode": 0,
            "Output": "<!DOCTYPE html>\n<html>\n<head>\n<title>Welcome to nginx!</title>\n<style>\n    body {\n        width: 35em;\n        margin: 0 auto;\n        font-family: Tahoma, Verdana, Arial, sans-serif;\n    }\n</style>\n</head>\n<body>\n<h1>Welcome to nginx!</h1>\n<p>If you see this page, the nginx web server is successfully installed and\nworking. Further configuration is required.</p>\n\n<p>For online documentation and support please refer to\n<a href=\"http://nginx.org/\">nginx.org</a>.<br/>\nCommercial support is available at\n<a href=\"http://nginx.com/\">nginx.com</a>.</p>\n\n<p><em>Thank you for using nginx.</em></p>\n</body>\n</html>\n",
            "Start": "2016-11-25T14:35:37.780192565Z"
        }
    ],
    "Status": "healthy"
}
```

另外, `HEALTHCHECK NONE`：如果基础镜像有健康检查指令，使用这行可以屏蔽掉其健康检查指令




## ONBUILD 给别人用


格式：`ONBUILD <其它指令>`

ONBUILD 是一个特殊的指令，它后面跟的是其它指令，比如 RUN, COPY 等，而这些指令，在当前镜像构建时并不会被执行。只有当以当前镜像为基础镜像，去构建下一级镜像的时候才会被执行


下面以一个例子说明:

制作 Node.js 所写的应用的镜像:  Node.js 使用 npm 进行包管理，所有依赖、配置、启动信息等会放到 package.json 文件里。在拿到程序代码后，需要先进行 npm install 才可以获得所有需要的依赖。然后就可以通过 npm start 来启动应用

Dockerfile:

```sh
FROM node:slim
RUN mkdir /app # 可以省掉?
WORKDIR /app
COPY ./package.json /app
RUN [ "npm", "install" ]
COPY . /app/
CMD [ "npm", "start" ]
```

把这个 Dockerfile 放到 Node.js 项目的根目录，构建好镜像后，就可以直接拿来启动容器运行。

现在考虑这样的情景: 

我们还有第二个 Node.js 项目, 和这个类似, 如果需要制作镜像, 需要再复制一份Dockerfile到项目根目录, 构建. 这里就涉及到重复了, 如果第一个项目新发现有问题, 修复后, 第二个项目也需要手动再次修复

初级改进: 制作一份基础镜像, 后续的Dockerfile FROM这个镜像

```sh
# 基础镜像, 名字为 my-node
FROM node:slim
RUN mkdir /app
WORKDIR /app
CMD [ "npm", "start" ]

# 其他子镜像, 
# 基础镜像变化后，各个项目都用这个 Dockerfile 重新构建镜像，会继承基础镜像的更新
FROM my-node
COPY ./package.json /app
RUN [ "npm", "install" ]
COPY . /app/
```

现在考虑这个场景: 如果子 Dockerfile 里面有些东西需要调整呢？比如 npm install 需要加一些参数，那怎么办？这一行 RUN 是不可能放入基础镜像的，那么只能每个 子 dockerfile 都修改 npm install 这条命令了, 能否将子file中的命令提到公共file 中而不执行呢?

ONBUILD 可以解决这个问题

基础镜像:

```sh
FROM node:slim
RUN mkdir /app
WORKDIR /app
# 下面三行在当前 dockerfile 构建时候不会执行
ONBUILD COPY ./package.json /app
ONBUILD RUN [ "npm", "install" ]
ONBUILD COPY . /app/
CMD [ "npm", "start" ]
```

然后各个子项目的 Dockerfile 就变成了简单地

```sh
# 当在各个项目目录中，用这个只有一行的 Dockerfile 构建镜像时，之前基础镜像的那三行 ONBUILD 就会开始执行，成功的将当前项目的代码复制进镜像、并且针对本项目执行 npm install，生成应用镜像
FROM my-node
```

## Dockerfile的多阶段构建

一种方式是将所有的构建过程包含在一个 Dockerfile 中，包括项目及其依赖库的编译、测试、打包等流程，这里可能会带来的一些问题：

Dockerfile 特别长，可维护性降低

镜像层次多，镜像体积较大，部署时间变长

源代码存在泄露的风险


# 使用容器

容器是独立运行的一个或一组应用，以及它们的运行态环境

## 查看

* `docker container ls -a`or`docker ps -a` 查看所有, 包括处于终止状态的容器
* `docker container ls`or`docker ps` 仅查看运行状态的容器

## 启动

启动容器有两种方式，一种是基于镜像新建一个容器并启动(常用)，另外一个是将在终止状态（stopped）的容器重新启动。

新建容器并启动: `docker run <容器名:版本> <命令>`)

启动已有的容器: `docker start <container>` or `docker container start/restart xxx`

当利用 docker run 来创建容器时，Docker 在后台进行如下流程:

* 检查本地是否存在指定的镜像，不存在就从公有仓库下载
* 利用镜像创建并启动一个容器
* 分配一个文件系统，并在只读的镜像层外面挂载一层可读写层
* 从宿主主机配置的网桥接口中桥接一个虚拟接口到容器中去
* 从地址池配置一个 ip 地址给容器
* 执行用户指定的应用程序 (如果有的话)
* 执行完毕后容器被终止

### -it 维持交互终端

eg:

```sh
# 输出一个 “Hello World”，之后终止容器
$ docker run ubuntu:14.04 /bin/echo 'Hello world'
Hello world

# 启动一个 bash 终端，允许用户进行交互
# 其中，-t 选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上， -i 则让容器的标准输入保持打开(以便交互式操作)。 -it 一般一起使用
$ docker run -t -i ubuntu:14.04 /bin/bash
root@af8bae53bdd3:/#
```

### -d 后台运行

前面都是在前台运行的, 如何以守护状态运行? 通过 `docker run -d`


eg:

```sh
# 如果不使用 -d 参数运行容器,容器会把输出的结果 (STDOUT) 打印到宿主机上面
$ docker run ubuntu:17.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
hello world
hello world
hello world
hello world

# 如果使用了 -d 参数运行容器
# 此时容器会在后台运行, 宿主机器没有任何输出(输出结果可以用 docker logs 查看)
$ docker run -d ubuntu:17.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
77b2dc01fe0f3f1265df143181e7b9af5e05279a884f4776ee75350ea9d8017a

# 查看容器日志
docker container logs <container ID or NAMES> 
docker logs <container id or name>
```

### --privileged=true 赋予特权

是否让docker 应用容器 获取宿主机root权限

使用该参数，container内的root拥有真正的root权限。
否则，container内的root只是外部的一个普通用户权限。
privileged启动的容器，可以看到很多host上的设备，并且可以执行mount。
甚至允许你在docker容器中启动docker容器。


## 终止container

`docker container stop <xxx>`

此外，当 Docker 容器中指定的应用终结时，容器也自动终止(例如: 只启动了一个终端的容器，用户通过 exit 命令或 Ctrl+d 来退出终端时，所创建的容器立刻终止。)

`docker container ls -a` 查看所有的container, 包括停止的container


## 删除container

container终止后, 不会删除, 需要手动删除

* `docker container rm <xxx>` 删除指定container(容器处于终止状态)
* `docker container rm -f <xxx>`如果要删除一个运行中的容器，可以添加 -f 参数。Docker 会发送 SIGKILL 信号给容器
* `docker container prune` 删除所有终止的容器

## 进入容器

在使用 -d 参数时，容器启动后会进入后台, 某些时候需要进入容器进行操作，包括使用 docker attach 命令或 docker exec 命令(推荐后者, 因为后者exit时不会导致容器退出)

* `docker attach <xxx>` 进入container, 当exit时, 容器也随之终止

* `docker exec -it <xxx> bash` 进入container, 退出时容器不会终止(注意, 后面要跟一个命令bash)

## 容器的导入和导出

导出

```sh
$ docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                    PORTS               NAMES
7691a814370e        ubuntu:14.04        "/bin/bash"         36 hours ago        Exited (0) 21 hours ago                       test
$ docker export 7691a814370e > ubuntu.tar
```

这样将导出容器快照到本地文件

导入容器快照为镜像:

```sh
$ cat ubuntu.tar | docker import - test/ubuntu:v1.0
$ docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
test/ubuntu         v1.0                9d37a6082e97        About a minute ago   171.3 MB

# 或者
$ docker import http://example.com/exampleimage.tgz example/imagerepo
```

docker import和docker load比较: 容器快照文件(docker import)将丢弃所有的历史记录和元数据信息（即仅保存容器当时的快照状态），而镜像存储文件(docker load)将保存完整记录，体积也要大。此外，从容器快照文件导入时可以重新指定标签等元数据信息。

## 拷贝容器内文件到主机

`docker cp <container_name>:/path <host_path>`

# 访问仓库

一个容易混淆的概念是注册服务器（Registry）。实际上注册服务器是管理仓库的具体服务器，每个服务器上可以有多个仓库，而每个仓库下面有多个镜像。从这方面来说，仓库可以被认为是一个具体的项目或目录。例如对于仓库地址 dl.dockerpool.com/ubuntu 来说，dl.dockerpool.com 是注册服务器地址，ubuntu 是仓库名。

## dockerhub

`docker login`登录
`docker logout` 退出

`docker search <xxx>` 搜索
`docker pull <xxx>` 下载到本地

推送镜像到自己的仓库:

```sh
# 标记新的tag用于推送到远程
$ docker tag ubuntu:17.10 xiaoyureed/ubuntu:17.10

$ docker image ls

REPOSITORY                                               TAG                    IMAGE ID            CREATED             SIZE
ubuntu                                                   17.10                  275d79972a86        6 days ago          94.6MB
username/ubuntu                                          17.10                  275d79972a86        6 days ago          94.6MB

# 推送
$ docker push xiaoyureed/ubuntu:17.10

# 现在搜索可以搜到了
$ docker search username

NAME                      DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
xiaoyureed/ubuntu
```

此外, docker hub 支持 自动创建(Automated Builds), 对于需要经常升级镜像内程序来说，十分方便 


## 私有仓库


https://github.com/goharbor/harbor Harbor

```sh
# 可以通过获取官方 registry 镜像来运行
# 默认情况下，仓库会被创建在容器的 /var/lib/registry 目录
docker run -d -p 5000:5000 --restart=always --name registry registry

# 使用 -v 自定义 仓库目录
$ docker run -d \
    -p 5000:5000 \
    -v /opt/data/registry:/var/lib/registry \
    registry
```

想让本网段的其他主机也能把镜像推送到私有仓库。你就得把例如 192.168.199.100:5000 这样的内网地址作为私有仓库地址，这时你会发现无法成功推送镜像。这是因为 Docker 默认不允许非 HTTPS 方式推送镜像。我们可以通过 Docker 的配置选项来取消这个限制:

# docker数据管理

Docker container 产生的数据在 container 删除后就丢失了, 除非 commit 这个 container 生成新的 image, 那么如何保存这些数据呢:

* 数据卷（Volumes）

* 挂载主机目录 (Bind mounts)

## 数据卷

数据卷 是被设计用来持久化数据的 , 类似linux下目录的mount; 它绕过 UFS，可以提供很多有用的特性：

* 数据卷 可以在容器之间共享和重用

* 对 数据卷 的修改会立马生效

* 对 数据卷 的更新，不会影响镜像

* 数据卷 默认会一直存在，即使容器被删除

    它的生命周期独立于容器，Docker 不会在容器被删除后自动删除 数据卷



`docker volume create my-vol` 创建一个volume
`docker volume ls` 查看所有的volume
`docker volume inspect my-vol` 查看指定的volume
`docker inspect web` 在容器中查看volume信息(数据卷 信息在 "Mounts" Key 下面, 这里的web为container名)
`docker volume rm my-vol`删除volume
`docker volume prune` 清理无主的volume


创建一个container同时挂载一个/多个数据卷

```sh
# 创建一个名为 web 的容器，并加载一个 数据卷 到容器的 /webapp 目录。, 如果/webapp中有内容则暂时会被屏蔽
$ docker run -d -P \
    --name web \
    # -v my-vol:/wepapp \
    # 等同
    --mount source=my-vol,target=/webapp \
    training/webapp \
    python app.py
```

## 挂载主机目录

使用` --mount` 标记可以指定挂载一个本地主机的目录到容器中去 (也可以挂在数据卷到容器中去)

```sh
# 加载主机的 /src/webapp 目录到容器的 /opt/webapp目录
$ docker run -d -P \
    --name web \
    # -v /src/webapp:/opt/webapp \
    --mount type=bind,source=/src/webapp,target=/opt/webapp \
    training/webapp \
    python app.py
```

这个功能在进行测试的时候十分方便，比如用户可以放置一些程序到本地目录中，来查看容器是否正常工作。本地目录的路径必须是绝对路径

Docker 挂载主机目录的默认权限是 读写，用户也可以通过增加 readonly 指定为 只读:

```sh
$ docker run -d -P \
    --name web \
    # -v /src/webapp:/opt/webapp:ro \
    --mount type=bind,source=/src/webapp,target=/opt/webapp,readonly \
    training/webapp \
    python app.py
```

查看使用`docker inspect <container name>`, 挂载主机目录 的配置信息在 "Mounts" Key 下面;




也可以挂载主机单个文件

```sh
$ docker run --rm -it \
   # -v $HOME/.bash_history:/root/.bash_history \
   --mount type=bind,source=$HOME/.bash_history,target=/root/.bash_history \
   ubuntu:17.10 \
   bash

root@2affd44b4667:/# history
1  ls
2  diskutil list
```

这样就可以记录在容器输入过的命令了

## volume 的共享

`--volume-from`

有镜像 imageA, 派生容器 container1, 挂载有匿名卷 /data, `docker run --name container2 --volume-from container1 imageA` 那么 /data 目录会被两个 容器共享


# 网络互联

## docker 默认初始创建

docker 安装的时候，会在宿主机建立一块虚拟网卡 docker0 作为网桥， 宿主机 通过这个网卡 桥接到了docker内局域网。在docker容器内，通过 docker0 的 IP 地址就能访问宿主机的服务了。

Docker安装后，默认会创建下面三个 network, 除非另有指定，否则新启动的容器会链接 bridge 这个 network。

```sh
$ docker network ls
NETWORK ID     NAME        DRIVER       SCOPE
9781b1f585ae    bridge       bridge       local   ==> 桥接网络, 默认, 重启下，Docker的IP地址就变了
1252da701e55    host        host        local     ==> 主机网络, 容器的网络会附属在主机上，两者是互通的(在容器中运行一个Web服务，监听8080端口，则主机的8080端口就会自动映射到容器中)
237ea3d5cfbf    none        null        local     ==> 无指定网络, docker 容器就不会分配局域网的IP

```

## 网卡

容器内部如何访问宿主机器?

宿主执行 ifconfig, `eth0, eth1 ...` 代表真实网卡, `docker0` 是 docker 新建的虚拟网卡, 作为容器访问宿主的网桥, `veth2ddf66cc, vethxxxxx ...` 和容器内的 eth0 绑定

## network分类

### bridge 类型

桥接网络, 重启下，Docker的IP地址就变了, 系统默认初始创建一个默认桥接网络名为 "bridge", 但是生产一般自定义一个桥接网络(因为自定义桥接网络优先级高于默认桥接网络, 不要在生产环境使用默认 bridge network)

连接同一个自定义桥接网络的容器彼此暴露端口，而不会对外世界暴露端口, 但是外界若要连接, 需要使用 -p (host port):(container port) 发布接口

默认请求网络 and 自定义桥接网络 的区别:

- 自定义桥接网络优先级高于默认桥接网络, 不要在生产环境使用默认 bridge network

- 默认桥接网络上的容器只能通过IP地址相互访问，除非使用 `--link` 选项，而在用户定义的桥接网络上，容器可以通过名称或别名彼此解析

- 在容器的生命周期中，可以动态地将它与用户定义的网络连接或断开。而要从默认桥接网络中删除容器，需要停止容器并使用不同的网络选项重新创建容器。

### host 类型

host: 容器的网络栈并不是和docker宿主机隔离的

容器的网络会附属在主机上，两者是互通的(在容器中运行一个Web服务，监听8080端口，则主机的8080端口就会自动映射到容器中)


## 创建固定ip

自己创建一个新的bridge网络bridge1，在创建bridge1的时候同时创建子网，那么在创建容器的时候指定网络为bridge1并指定ip即可

```sh
# 创建网络, 指定网段, 默认是 bridge 类型, 所以可省略
# --driver 或者 -d 指定网络类型
docker network create [--driver bridge] --subnet=172.18.0.0/16 my-network 

docker run -itd --name my-container --network my-network --ip 172.18.0.10 my-image </bin/bash >
        * 用 --network 参数，可以指定网络类型
        * --ip 172.17.0.10指定ip
```

## 查看container的ip

```sh
# 查看docker0的网络(宿主机上操作)
ip a show docker0
# 到容器内部查看ip
ip a show eth0
# or
docker inspect <id,container_name> | grep "IPAddress"
```

## 外部访问容器

一个是通过 host 类型的 network (宿主连容器)

一个 是 查询 docker0 网卡的地址 `ip addr show docker0` 作为 宿主在 docker 局域网的地址 (容器连宿主),  在 wsl2 下的的 容器, 外部连接容器暂时还需要使用 wsl 的ip即 `ifconfig` 的 eth0


容器中可以运行一些网络应用，要让外部也可以访问这些应用，可以通过

` -P `(Docker 会随机映射一个 49000~49900 的端口到内部容器开放的网络端口)

` -p `(指定要映射的端口，并且，在一个指定端口上只可以绑定一个容器。支持的格式有 ip:hostPort:containerPort | ip::containerPort | hostPort:containerPort

eg: 

```sh
# 端口自动映射, 需要手动查看端口是多少
$ docker run -d -P training/webapp python app.py

$ docker container ls -l
CONTAINER ID  IMAGE                   COMMAND       CREATED        STATUS        PORTS                    NAMES
bc533791f3f5  training/webapp:latest  python app.py 5 seconds ago  Up 2 seconds  0.0.0.0:49155->5000/tcp  nostalgic_morse

# 映射所有接口地址
# 使用 hostPort:containerPort 格式本地的 5000 端口映射到容器的 5000 端口; 此时默认会绑定本地所有接口上的所有地址
$ docker run -d -p 5000:5000 training/webapp python app.py

# -p 可多次使用, 指定不同的端口映射
$ docker run -d \
    -p 5000:5000 \
    -p 3000:80 \
    training/webapp \
    python app.py

# 映射到指定地址的指定端口
# 可以使用 ip:hostPort:containerPort 格式指定映射使用一个特定地址，比如 localhost 地址 127.0.0.1
$ docker run -d -p 127.0.0.1:5000:5000 training/webapp python app.py

# 映射到指定地址的随机端口
# 使用 ip::containerPort 绑定 localhost 的任意端口到容器的 5000 端口，本地主机会自动分配一个端口。
$ docker run -d -p 127.0.0.1::5000 training/webapp python app.py

# 还可以使用 udp 标记来指定 udp 端口, 默认为tcp
$ docker run -d -p 127.0.0.1:5000:5000/udp training/webapp python app.py
```

查看当前映射的端口配置，也可以查看到绑定的地址:

```sh
$ docker port <container name> [port]
127.0.0.1:49155
```

## 容器互联

一个是通过 docker network

```sh

# 创建一个叫做 my-bridge 的网桥: 	
docker network create -d bridge my-bridge
# 显示所有 bridge	: 
docker network ls
# 创建容器mynginx2并添加到 my-bridge 网络中: 
docker run --name mynginx2 --network my-bridge -p 8080:80 -d nginx:latest
# 使用 inspect 查看: 	
docker inspect mynginx2
# 查看 my-bridge 网络里面的容器:	
docker inspect my-bridge
# 手动将某个运行着的容器加入网桥:	
docker network connect my-bridage test2s



############  实例

# BusyBox 
# 是一个集成了三百多个最常用Linux命令和工具的软件

# 创建docker网络, 启动两个容器, 加入网络
$ docker network create -d bridge my-net
$ docker run -it --rm --name busybox1 --network my-net busybox sh
# 需要打开新的terminal
$ docker run -it --rm --name busybox2 --network my-net busybox sh
# 最后在各自的terminal中ping测试
ping busybox2
ping busybox1


# MySQL

docker network create -d bridge some-network
docker run -d --name some-mysql --network some-network -e MYSQL_ROOT_PASSWORD=my-secret-pw mysql:tag
# 启动一个临时的 MySQL client
docker run -it --rm --network some-network mysql mysql -hsome-mysql -uexample-user -p
```



一个是通过 `--link`, docker run --link可以用来链接2个容器，使得源容器（被链接的容器）和接收容器（主动去链接的容器）之间可以互相通信，并且接收容器可以获取源容器的一些数据，如源容器的环境变量。

```sh
#--link <container_name>:<link_name> 
docker run -d --name node --link selenium_hub:hub selenium/node-chrome-debug
docker exec -it node /bin/bash
ping hub # 可以 ping 通

```

更方便的容器互联使用[ Docker Compose](#docker-compose)

## 配置dns 和 主机名 hostname

Docker 利用虚拟文件来挂载容器的 3 个相关配置文件。

在容器中使用 mount 命令可以看到挂载信息:

```sh
$ mount
/dev/disk/by-uuid/1fec...ebdf on /etc/hostname type ext4 ...
/dev/disk/by-uuid/1fec...ebdf on /etc/hosts type ext4 ...
tmpfs on /etc/resolv.conf type tmpfs ...
```

这种机制可以让宿主主机 DNS 信息发生更新后，所有 Docker 容器的 DNS 配置通过 `/etc/resolv.conf` 文件立刻得到更新

配置全部容器的 DNS ，也可以在` /etc/docker/daemon.json` 文件中增加以下内容来设置:

```json
{
  "dns" : [
    "114.114.114.114",
    "8.8.8.8"
  ]
}
```

这样每次启动的容器 DNS 自动配置为 114.114.114.114 和 8.8.8.8。使用以下命令来证明其已经生效:`docker run -it --rm ubuntu:17.10  cat etc/resolv.conf`

如果用户想要手动指定容器的配置，可以在使用 docker run 命令启动容器时加入如下参数：

* `-h HOSTNAME` 或者` --hostname=HOSTNAME` 设定容器的主机名，它会被写到容器内的 /etc/hostname 和 /etc/hosts。但它在容器外部看不到，既不会在 docker container ls 中显示，也不会在其他的容器的 /etc/hosts 看到。

* `--dns=IP_ADDRESS` 添加 DNS 服务器到容器的 /etc/resolv.conf 中，让容器用这个服务器来解析所有不在 /etc/hosts 中的主机名。

* `--dns-search=DOMAIN` 设定容器的搜索域，当设定搜索域为 .example.com 时，在搜索一个名为 host 的主机时，DNS 不仅搜索 host，还会搜索 host.example.com。

# docker-compose

## compose简介

Docker Compose 是 Docker 官方编排（Orchestration）项目之一，负责快速的部署分布式应用(实现对 Docker 容器集群的快速编排。从功能上看，跟 OpenStack 中的 Heat 十分类似; 其前身是开源项目 Fig;

Compose 项目由 Python 编写，实现上调用了 Docker 服务提供的 API 来对容器进行管理

通俗的理解: 可以同时定义和运行多个 Docker 容器.

用户通过一个单独的 docker-compose.yml 模板文件（YAML 格式）来定义一组相关联的应用容器为一个项目（project）

Compose 中有两个重要的概念：

* 服务 (service)：一个应用的容器，实际上可以包括若干运行相同镜像的容器实例。

* 项目 (project)：由一组关联的应用容器组成的一个完整业务单元，在 docker-compose.yml 文件中定义。

Compose 的默认管理对象是项目，通过子命令对项目中的一组容器进行便捷地生命周期管理


安装卸载

https://docs.docker.com/compose/install/

1. 通过 Python 的包管理工具 pip 进行安装，
2. 也可以直接下载编译好的二进制文件使用 
3. 能够直接在 Docker 容器中运行

前两种方式是传统方式，适合本地环境下安装使用；最后一种方式则不破坏系统环境，更适合云计算场景; 

Docker for Windows 自带 docker-compose 二进制文件，安装 Docker 之后可以直接使用。

`docker-compose --version` 查看版本

如果实在linux中:

```sh
# 直接下载对应的二进制包(推荐)
$ sudo curl -L https://github.com/docker/compose/releases/download/1.17.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
$ sudo chmod +x /usr/local/bin/docker-compose

# 还可以使用pip安装
$ sudo pip install -U docker-compose

# 命令补全功能需要下载一个文件
$ curl -L https://raw.githubusercontent.com/docker/compose/1.8.0/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose

# 如何在linux中执行? 下载的run.sh实际是下载docker/compose 镜像并运行
$ curl -L https://github.com/docker/compose/releases/download/1.8.0/run.sh > /usr/local/bin/docker-compose
$ chmod +x /usr/local/bin/docker-compose

# 测试是否安装成功, 第一次会下载compose镜像
docker-compose version
```

卸载:

```sh
# 如果是二进制包方式安装的，删除二进制文件即可。
$ sudo rm /usr/local/bin/docker-compose

# 如果是通过 pip 安装的，则执行如下命令即可删除。
$ sudo pip uninstall docker-compose
```

看一个 quick-start:

eg: 用Python 来建立一个能够记录页面访问次数的 web 网站

在文件夹project/下新建app.py

```py
from flask import Flask
from redis import Redis

app = Flask(__name__)
redis = Redis(host='redis', port=6379)

@app.route('/')
def hello():
    count = redis.incr('hits')
    return 'Hello World! 该页面已被访问 {} 次。\n'.format(count)

if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True)
```

新建 Dockerfile 

```sh
FROM python:3.6-alpine
ADD . /code
WORKDIR /code
RUN pip install redis flask
CMD ["python", "app.py"]
```

新建 docker-compose.yml

```yml
# 这里的版本和docker-compose有关, 如果报错, 则改为'2'
version: '3'
services:

  web:
    build: . # 在当前目录寻找 dockerfile构建
    ports:
     - "5000:5000"

  redis:
    image: "redis:alpine"
```

运行: `docker-compose up`



## 命令使用

默认的命令对象是项目，这意味着项目中所有的服务都会受到命令影响

格式: `docker-compose [-f=<arg>...] [options] [COMMAND] [ARGS...]`

options有这些:

```
-f, --file FILE 指定使用的 Compose 模板文件，默认为 docker-compose.yml，可以多次指定。

-p, --project-name NAME 指定项目名称，默认将使用所在目录名称作为项目名。

--x-networking 使用 Docker 的可拔插网络后端特性

--x-network-driver DRIVER 指定网络后端的驱动，默认为 bridge

--verbose 输出更多调试信息。

-v, --version 打印版本并退出
```

### up

* 格式为 docker-compose up [options] [SERVICE...]

* 该命令十分强大，它将尝试自动完成包括 构建, 启动...

* 默认情况，`docker-compose up` 启动的容器都在前台，控制台将会同时打印所有容器的输出信息，可以很方便进行调试。

    当通过 Ctrl-C 停止命令时，所有容器将会停止

    如果使用 `docker-compose up -d`，将会在后台启动并运行所有的容器。一般推荐生产环境下使用该选项

* `docker-compose up --no-recreate`只会启动处于停止状态的容器，而忽略已经运行的服务

    默认情况，如果服务容器已经存在，docker-compose up 将会尝试停止容器，然后重新创建

* `docker-compose up --no-deps -d <SERVICE_NAME>` 只想重新部署某个服务, 通过这个命令来重新创建服务并后台停止旧服务，启动新服务，并不会影响到其所依赖的服务



所有可用的选项:

```
-d 在后台运行服务容器。

--no-color 不使用颜色来区分不同的服务的控制台输出。

--no-deps 不启动服务所链接的容器。

--force-recreate 强制重新创建容器，不能与 --no-recreate 同时使用。

--no-recreate 如果容器已经存在了，则不重新创建，不能与 --force-recreate 同时使用。

--no-build 不自动构建缺失的服务镜像。

-t, --timeout TIMEOUT 停止容器时候的超时（默认为 10 秒）

```

### run

格式为 `docker-compose run [options] [-p PORT...] [-e KEY=VAL...] SERVICE [COMMAND] [ARGS...]`。

在指定服务上执行一个命令。

eg:

```sh
# 将会启动一个 ubuntu 服务容器，并执行 ping docker.com 命令
$ docker-compose run ubuntu ping docker.com
```

默认情况下，如果存在关联，则所有关联的服务将会自动被启动，除非这些服务已经在运行中

如果不希望自动启动关联的容器，可以使用 --no-deps 选项

```sh
# 不会启动 web 容器所关联的其它容器
$ docker-compose run --no-deps web python manage.py shell
```

该命令类似启动容器后运行指定的命令，相关卷、链接等等都将会按照配置自动创建, 但是有两个不同点

1. 给定命令将会覆盖原有的自动运行命令
2. 不会自动创建端口，以避免冲突



所有可选的选项:

```
-d 后台运行容器。

--name NAME 为容器指定一个名字。

--entrypoint CMD 覆盖默认的容器启动指令。

-e KEY=VAL 设置环境变量值，可多次使用选项来设置多个环境变量。

-u, --user="" 指定运行容器的用户名或者 uid。

--no-deps 不自动启动关联的服务容器。

--rm 运行命令后自动删除容器，d 模式下将忽略。

-p, --publish=[] 映射容器端口到本地主机。

--service-ports 配置服务端口并映射到本地主机。

-T 不分配伪 tty，意味着依赖 tty 的指令将无法运行。
```

### scale

* 格式为 docker-compose scale [options] [SERVICE=NUM...]。

* 设置指定服务运行的容器个数。

* 通过 service=num 的参数来设置数量。例如

    * eg

    ```sh
    # 将启动 3 个容器运行 web 服务，2 个容器运行 db 服务
    $ docker-compose scale web=3 db=2
    ```

* 当指定数目多于该服务当前实际运行容器，将新创建并启动容器；反之，将停止容器

可用选项:

-t, --timeout TIMEOUT 停止容器时候的超时（默认为 10 秒）。

### start

格式为 docker-compose start [SERVICE...]。

启动已经存在的服务容器

### build

* 格式为 docker-compose build [options] [SERVICE...]

* 构建（重新构建）项目中的服务容器

* 服务容器一旦构建后，将会带上一个标记名，例如对于 web 项目中的一个 db 容器，可能是 web_db

* 可以随时在项目目录下运行 docker-compose build 来重新构建服务

所有选项:

```
--force-rm 删除构建过程中的临时容器。

--no-cache 构建镜像过程中不使用 cache（这将加长构建过程）。

--pull 始终尝试通过 pull 来获取更新版本的镜像。

```

### config

验证 Compose 文件格式是否正确，若正确则显示配置，若格式错误显示错误原因

### exec

进入指定的容器

### images

列出 Compose 文件中包含的镜像。

### kill

格式为 docker-compose kill [options] [SERVICE...]

通过发送 SIGKILL 信号来强制停止服务容器。

支持通过 -s 参数来指定发送的信号，例如通过如下指令发送 SIGINT 信号。

```sh
$ docker-compose kill -s SIGINT
```

### down

此命令将会停止 up 命令所启动的容器，并移除网络

### pause

格式为 docker-compose pause [SERVICE...]。

暂停一个服务容器。

### unpause

格式为 docker-compose unpause [SERVICE...]。

恢复处于暂停状态中的服务。

### restart

格式为 docker-compose restart [options] [SERVICE...]。

重启项目中的服务。

选项：

-t, --timeout TIMEOUT 指定重启前停止容器的超时（默认为 10 秒）。

### stop

格式为 docker-compose stop [options] [SERVICE...]。

停止已经处于运行状态的容器，但不删除它。通过 docker-compose start 可以再次启动这些容器。

选项：

-t, --timeout TIMEOUT 停止容器时候的超时（默认为 10 秒）。

### rm

格式为 docker-compose rm [options] [SERVICE...]。

删除所有（停止状态的）服务容器。推荐先执行 docker-compose stop 命令来停止容器。

选项：

-f, --force 强制直接删除，包括非停止状态的容器。一般尽量不要使用该选项。

-v 删除容器所挂载的数据卷。

### logs

格式为 docker-compose logs [options] [SERVICE...]。

查看服务容器的输出。默认情况下，docker-compose 将对不同的服务输出使用不同的颜色来区分。可以通过 --no-color 来关闭颜色。

该命令在调试问题的时候十分有用。

### top

查看各个服务容器内运行的进程

### version

格式为 docker-compose version。

打印版本信息。

### port

格式为 docker-compose port [options] SERVICE PRIVATE_PORT。

打印某个容器端口所映射的公共端口。

选项：

```
--protocol=proto 指定端口协议，tcp（默认值）或者 udp。
--index=index 如果同一服务存在多个容器，指定命令对象容器的序号（默认为 1）。
```

### ps

格式为 docker-compose ps [options] [SERVICE...]。

列出项目中目前的所有容器。

选项：

-q 只打印容器的 ID 信息。

### pull

格式为 docker-compose pull [options] [SERVICE...]。

拉取服务依赖的镜像。

选项：

--ignore-pull-failures 忽略拉取镜像过程中的错误。

### push

推送服务依赖的镜像到 Docker 镜像仓库。

## docker-compose.yml

默认的模板文件名称为 docker-compose.yml

模版中的命令基本跟 docker run 中对应参数的功能一致。

如果变量名称或者值中用到 true|false，yes|no 等表达 布尔 含义的词汇，最好放到引号里, 这些符号包括:

```
y|Y|yes|Yes|YES|n|N|no|No|NO|true|True|TRUE|false|False|FALSE|on|On|ON|off|Off|OFF
```

```yml
version: "3"

services:
    webapp:
        # 指定为镜像名称或镜像 ID。如果镜像在本地不存在，Compose 将会尝试拉取这个镜像
        # 每个服务都必须通过 image 指令指定镜像或 build 指令（需要 Dockerfile）等来自动构建生成镜像。
        # 可以不加引号
        image: "examples/web"
        ports:
            - "80:80"
        volumes:
            - "/data"
        # cap_add, cap_drop 指定容器的内核能力（capacity）分配
        cap_add:
            - ALL # 让容器拥有所有能力
        cap_drop:
            - NET_ADMIN # 去掉 NET_ADMIN 能力
        # 覆盖容器启动后默认执行的命令
        command: echo "hello world"
        # 仅用于 Swarm mode
        configs:
        # 仅用于 Swarm mode
        deploy:
        # 指定父 cgroup 组，意味着将继承该组的资源限制; 例如，创建了一个 cgroup 组名称为 cgroups_1
        cgroup_parent: cgroups_1
        # 指定容器名称。默认将会使用 项目名称_服务名称_序号 这样的格式
        # 注意: 指定容器名称后，该服务将无法进行扩展（scale），因为 Docker 不允许多个容器具有相同的名称
        container_name: docker-web-container
        # 指定设备映射关系
        devices:
            - "/dev/ttyUSB1:/dev/ttyUSB0"
        # 解决容器的依赖、启动先后的问题。以下例子中会先启动 redis db 再启动 webapp
        # 注意：webapp 服务不会等待 redis db 「完全启动」之后才启动
        depends_on:
            - db
            - redis
        # 自定义 DNS 服务器。可以是一个值，也可以是一个列表
        # dns: 8.8.8.8
        dns:
            - 8.8.8.8
            - 114.114.114.114
        # 配置 DNS 搜索域。可以是一个值，也可以是一个列表
        # dns_search: example.com
        dns_search:
            - domain1.example.com
            - domain2.example.com
        # 挂载一个 tmpfs 文件系统到容器。可以为单个, 也可以为列表
        # tmpfs: /run
        tmpfs:
            - /run
            - /tmp
        # 设置环境变量。你可以使用数组或字典两种格式
        # 只给定名称的变量会自动获取运行 Compose 主机上对应变量的值，可以用来防止泄露不必要的数据。
        # environment:
        #     RACK_ENV: development
        #     SESSION_SECRET:
        environment:
            - RACK_ENV=development
            - SESSION_SECRET
        # 从文件中获取环境变量，可以为单独的文件路径或列表
        # 如果通过 docker-compose -f FILE 方式来指定 Compose 模板文件，则 env_file 中变量的路径会基于模板文件路径
        # 如果有变量名称与 environment 指令冲突，则按照惯例，以后者为准
        # 环境变量文件中每一行必须符合格式(PROG_ENV=development)，支持 # 开头的注释行
        # env_file: .env
        env_file:
            - ./common.env
            - ./apps/web.env
            - /opt/secrets.env
        # 暴露端口，但不映射到宿主机，只让内部的服务访问
        expose:
            - "3000"
            - "8000"
        # 暴露端口信息。
        # 使用宿主端口：容器端口 (HOST:CONTAINER) 格式，或者仅仅指定容器的端口（宿主将会随机选择端口）都可以。
        ports:
            - "3000"
            - "8000:8000"
            - "49100:22"
            - "127.0.0.1:8001:8001"
        # 链接到 docker-compose.yml 外部的容器，甚至并非 Compose 管理的外部容器。不建议使用
        external_links:
            - redis_1
            - project_db_1:mysql
            - project_db_1:postgresql
        # 类似 Docker 中的 --add-host 参数，指定额外的 host 名称映射信息
        # 会在启动后的服务容器中 /etc/hosts 文件中添加host映射条目8.8.8.8 googledns和52.1.157.61 dockerhub
        extra_hosts:
            - "googledns:8.8.8.8"
            - "dockerhub:52.1.157.61"
        # 通过命令检查容器是否健康运行。
        healthcheck:
            test: ["CMD", "curl", "-f", "http://localhost"]
            interval: 1m30s
            timeout: 10s
            retries: 3
        # 为容器添加 Docker 元数据（metadata）信息。例如可以为容器添加辅助说明信息
        labels:
            com.startupteam.description: "webapp for a startup team"
            com.startupteam.department: "devops department"
            com.startupteam.release: "rc3 for v1.0"
        # 配置日志选项。
        logging:
            driver: syslog # 支持三种日志驱动类型: "json-file", "syslog", "none"
            # 配置日志驱动的相关参数
            options:
                syslog-address: "tcp://192.168.0.42:123"
        # 设置网络模式。使用和 docker run 的 --network 参数一样的值
        network_mode: "bridge" # 支持"bridge", "host", "none", "service:[service name]", "container:[container name/id]"
        # 跟主机系统共享进程命名空间。打开该选项的容器之间，以及容器和宿主机系统之间可以通过进程 ID 来相互访问和操作。
        pid: "host"
        # 指定容器模板标签（label）机制的默认属性（用户、角色、类型、级别等）。例如配置标签的用户名和角色名
        security_opt:
            - label:user:USER
            - label:role:ROLE
        # 设置另一个信号来停止容器。在默认情况下使用的是 SIGTERM 停止容器
        stop_signal: SIGUSR1
        # 配置容器内核参数, 两种方式
        # sysctls:
        #     net.core.somaxconn: 1024
        #     net.ipv4.tcp_syncookies: 0
        sysctls:
            - net.core.somaxconn=1024
            - net.ipv4.tcp_syncookies=0
        # 指定容器的 ulimits 限制值
        # 例如，指定最大进程数为 65535，指定文件句柄数为 20000（软限制，应用可以随时修改，不能超过硬限制） 和 40000（系统硬限制，只能 root 用户提高）。
        ulimits:
            nproc: 65535
            nofile:
                soft: 20000
                hard: 40000
        # 数据卷所挂载路径设置。可以设置宿主机路径 （HOST:CONTAINER） 或加上访问模式 （HOST:CONTAINER:ro）。
        volumes:
            - /var/lib/mysql
            - cache/:/tmp/cache
            - ~/configs:/etc/configs/:ro

    db:
        # 如果使用 build 指令，在 Dockerfile 中设置的选项(例如：CMD, EXPOSE, VOLUME, ENV 等) 将会自动被获取，无需在 docker-compose.yml 中再次设置。
        # 指定 Dockerfile 所在文件夹的路径（可以是绝对路径，或者相对 docker-compose.yml 文件的路径）
        build: ./dir
        # 配置容器连接的网络。注意这里是引用和services同级的networks下的项
        networks:
            - some-network
            - other-network
        # 存储敏感数据，例如 mysql 服务密码, 注意这里是引用
        secrets:
            - db_root_password
            - my_other_secret
    # 另一种指定Dockerfile的方式, 自由度更高
    redis:
        build:
            context: ./dir # Dockerfile所在文件夹
            dockerfile: Dockerfile-alternate # Dockerfile 文件名
            # 指定构建镜像时的变量
            args:
                buildno: 1
            cache_from: # 指定构建镜像的缓存
                - alpine:latest
                - corp/web_app:3.14
networks:
  some-network:
  other-network:
secrets:
  my_secret:
    file: ./my_secret.txt
  my_other_secret:
    external: true
```

Compose 模板文件支持动态读取主机的系统环境变量和当前目录下的 .env 文件中的变量

```yml
version: "3"
services:

    db:
    # 如果执行 MONGO_VERSION=3.2 docker-compose up 则会启动一个 mongo:3.2 镜像的容器；如果执行 MONGO_VERSION=2.8 docker-compose up 则会启动一个 mongo:2.8 镜像的容器
    image: "mongo:${MONGO_VERSION}"
```

若当前目录存在 .env 文件，执行 docker-compose 命令时将从该文件中读取变量

在当前目录新建 .env 文件并写入以下内容。

```props
# 支持 # 号注释
MONGO_VERSION=3.6
```

执行 docker-compose up 则会启动一个 mongo:3.6 镜像的容器



docker-compose.yml模版文件中的其他指令:

```sh
指定服务容器启动后执行的入口文件。

entrypoint: /code/entrypoint.sh
指定容器中运行应用的用户名。

user: nginx
指定容器中工作目录。

working_dir: /code
指定容器中搜索域名、主机名、mac 地址等。

domainname: your_website.com
hostname: test
mac_address: 08-00-27-00-0C-0A
允许容器中运行一些特权命令。

# 允许container内的root拥有真正的root权限, 可以看到很多host上的设备，并且可以执行mount。甚至允许你在docker容器中启动docker容器。
privileged: true

#指定容器退出后的重启策略为始终重启。该命令对保持服务始终运行十分有效，在生产环境中推荐配置为 always 或者 unless-stopped。
# 命令行中 --restart always, 重启策略是面向生产环境的一个启动策略，在开发过程中可以忽略
# 共有如下策略:
# no，默认策略，在容器退出时不重启容器
# on-failure，在容器非正常退出时（退出状态非0），才会重启容器
# on-failure:3，在容器非正常退出时重启容器，最多重启3次, 可自己指定
# always，在容器退出时总是重启容器
# unless-stopped，在容器退出时总是重启容器，但是不考虑在Docker守护进程启动时就已经停止了的容器
# 查看容器重启次数 docker inspect -f "{{ .RestartCount }}" ba-208
# 查看容器最后一次的启动时间 docker inspect -f "{{ .State.StartedAt }}" ba-208
restart: always

# 以只读模式挂载容器的 root 文件系统，意味着不能对容器内容进行修改。
read_only: true
打开标准输入，可以接受外部输入。

stdin_open: true
模拟一个伪终端。

tty: true
```

# docker-machine

Docker Machine 是 Docker 官方编排（Orchestration）项目之一，负责在多种平台上快速安装 Docker 环境, 包括虚拟机、本地主机和云平台

linux上安装
```sh
$ sudo curl -L https://github.com/docker/machine/releases/download/v0.13.0/docker-machine-`uname -s`-`uname -m` > /usr/local/bin/docker-machine
$ sudo chmod +x /usr/local/bin/docker-machine

`docker-machine -v` # 查看版本
```

使用virtualbox驱动创建本地主机实例:

`docker-machine create -d virtualbox test`


# docker-swarm,swarm-mode

Docker Swarm 是 Docker 官方三剑客项目之一，提供 Docker 容器集群服务，是 Docker 官方对容器云生态进行支持的核心方案; 使用它，用户可以将多个 Docker 主机封装为单个大型的虚拟 Docker 主机，快速打造一套容器云平台;Docker 1.12.0+ Swarm mode 已经内嵌入 Docker 引擎，成为了 docker 子命令 docker swarm, 因此一般使用docker的swarm mode

# docker实践

## 搭建持续集成系统

```sh
# 查看端口占用
lsof -i:8080
# 查看指定端口的进程
netstat -tunlp | grep 8000


# 搭建 jenkins

# 初始密码到jenkins内部[/var/jenkins_home/secrets/initialAdminPassword]看
# -u 0 表示覆盖镜像内部的帐号, 使用外部的root, 用于解决volume权限问题
# -v xxx:yyy 表示 volume:container中的目录
docker run -d -p 8090:8080 -p 50000:50000 -v /srv/docker/jenkins:/var/jenkins_home --name jenkins -u 0 jenkins

# 或者 使用docker-compose编排
# docker-compose.yml:(http://www.voidcn.com/article/p-keekrhyk-brk.html)
version: '2'

services:
  gitlab:
      image: gitlab/gitlab-ce:latest
      ports:
          - "20443:443"
          - "20080:80"
          - "20022:22"
      networks:
          - devops
      volumes:
          - /data/gitlab/config:/etc/gitlab
          - /data/gitlab/logs:/var/log/gitlab
          - /data/gitlab/data:/var/opt/gitlab

  jenkins:
      image: jenkins:latest
      ports:
          - "28080:8080"
          - "50000:50000"
      networks:
          - devops
      volumes:
          - /data/jenkins:/var/jenkins_home
          - /var/run/docker.sock:/var/run/docker.sock

  sonarqube:
      image: sonarqube:latest
      ports:
          - "29000:9000"
          - "29092:9092"
      networks:
          - devops

networks:
  devops:
    driver: bridge
```

搭建gitlab, 上传代码时候, `git remote add origin http://43.254.2.51:20080/root/rest-api-starter.git`, `git push --set-upstream origin master`

自动部署需要jenkins的插件 Publish over SSH;


## docker和springboot

老外的一篇博文: https://www.callicoder.com/spring-boot-docker-example/, 先没有使用maven工具, 然后引入maven插件, 两种实现

使用了 maven 插件: http://www.ityouknow.com/springboot/2018/03/19/spring-boot-docker.html


## oracle11g

https://blog.csdn.net/qq_33982232/article/details/83133870
https://www.35youth.cn/685.html (https://registry.hub.docker.com/r/jaspeen/oracle-11g)

## 使用 mysql 镜像

```sh
docker pull mysql
docker run -d -p 3306:3306 --name my-mysql -e MYSQL_ROOT_PASSWORD=hello mysql
mysql -h127.0.0.1 -uroot -p # password is 'hello'

# 若 local 没有安装 MySQL client, 则使用 container client
docker network create my-net
docker network connect my-net my-mysql
docker run -it --network my-net --rm mysql mysql -hmy-mysql -uroot -p

# or
docker run -it --rm --link my-mysql:xxxhost mysql mysql -hxxxhost -uroot -p 

```

[mysql容器的数据持久化问题](https://www.jianshu.com/p/530d00f97cbf)

[导入导出](https://zhuanlan.zhihu.com/p/26129750)

在宿主机上连接 MySQL 容器, 地址是 127.0.0.1:3306 , 而不是 localhost, (查container ip：`cat /etc/hosts`)

` Authentication plugin 'caching_sha2_password' cannot be loaded` 异常 - https://www.cnblogs.com/chuancheng/p/8964385.html

`Access denied for user 'root'@'172.19.0.1'` 异常 - 进入dockers内部 `docker exec -it <xxx> bash`, `mysql -uroot -p` 登陆MySQL, `update mysql.user set host = '%' where user = 'root';` 修改root用户允许所有ip访问, 默认只能localhost访问; 如果还不行 `ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root'; flush privileges;` 修改root用户的权限 (https://blog.csdn.net/iw1210/article/details/54646093)

```yml
version: "3.2"
services: 
  mysql:
    image: mysql
    ports: 
      - 3306:3306
    environment:
        # 校正时区
      - TZ=Asia/Shanghai
    # 设置编码
    command: --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci
    # 数据卷
    volumes: 
      - ./data:/var/lib/mysql
    # root 的密码, 不过貌似没有生效 不要紧 手动设置也可
    environment: 
      - MYSQL_ROOT_PASSWORD=root

```

root, root 登陆

## 使用postgres镜像

`docker run -d --name postgres -p 5432:5432 -e POSTGRES_USER=xy -e POSTGRES_PASSWORD=xy123  postgres`

or

```yml
version: "3"
services:
  postgresdb:
    image: postgres
    volumes: 
      #- ./data:/var/lib/postgresql/data # 挂载本地目录
      # 使用数据卷
      - pgdata:/var/lib/postgresql/data # data volume
    ports:
      - 5432:5432
# data volume 需要在这里声明
# 
volumes: 
  pgdata:

```

可能错误: data directory "/var/lib/postgresql/data" has wrong ownership

ref: https://stackoverflow.com/questions/49148754/docker-container-shuts-down-giving-data-directory-has-wrong-ownership-error-wh

## 使用 rabbitmq镜像

```yml
version: "3"
services:
  rabbitmq:
    image: rabbitmq:management
    ports:
      - "5672:5672"
      - "15672:15672"
    
```

## redis镜像

```yml
redis:
  image: redis 
  ports: 
    - "6379:6379"
  

```


```sh
docker run -p 6379:6379 -d redis:latest redis-server
# 数据持久化存方式启动
docker run -p 6379:6379 -v $PWD/data:/data --name redis -d redis redis-server --appendonly yes
# 自定义配置文件, 先创建 /usr/local/docker/redis.conf
docker run -p 6379:6379 --name myredis -v /usr/local/docker/redis.conf:/etc/redis/redis.conf -v /usr/local/docker/data:/data -d redis redis-server /etc/redis/redis.conf --appendonly yes

# 带密码启动
docker run --name redis -p 6379:6379 -d --restart=always redis:latest redis-server --appendonly yes --requirepass "redis123456aB"


# redis client
docker exec -ti d0b86 redis-cli [-h localhost -p 6379]
# remote client
docker exec -it redis_image redis-cli -h 192.168.1.100 -p 6379 -a your_password //如果有密码 使用 -a参数

```

redis.conf

```conf
bind 127.0.0.1 #注释掉这部分，这是限制redis只能本地访问
# or
# 允许外网访问redis
bind 0.0.0.0 

protected-mode no #默认yes，开启保护模式，会限制为本地访问

daemonize no#默认no，改为yes意为以守护进程方式启动，可后台运行，除非kill进程，改为yes会使配置文件方式启动redis失败

databases 16 #数据库个数（可选）

dir ./ #输入本地redis数据库存放文件夹（可选）

appendonly yes #redis持久化（可选）

# 设置访问redis的密码为123(可选)
requirepass 123 


```

redis-commander: 提供 redis web 管理界面

`docker run --rm -d --name redis-cmder -p 8081:8081 --link redis:re -e REDIS_HOSTS=re:6379 rediscommander/redis-commander`

## ubuntu镜像

```sh
docker pull ubuntu
docker run -it ubuntu bash

apt update
# 安装 vim ping wget ifconfig
apt install vim iputils-ping wget net-tools zsh
# 2、把默认的Shell改成zsh
chsh -s /bin/zsh
# 安装 oh-my-zsh 需要先装git
apt install git
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## zookeeper

```sh
docker run --name some-zookeeper -d -p 2181:2181 zookeeper
# 查看日志
docker logs -f --tail=10 some-zookeeper
# 进入命令行 client (进入后 执行 zkCli.sh -server <ip>)
docker run -it --rm --link some-zookeeper:zk zookeeper zkCli.sh -server zk


###### zookeeper client cmd
ls / # 查看根节点
get /zookeeper

```

## rabbitmq

```sh
# 带 management 的版本有 web 界面
docker pull rabbitmq:3.8.3-management
# 15672 为 web ui 端口, 登录 guest/guest
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management

docker run -d --name rabbitmq3.7.7 -p 5672:5672 -p 15672:15672 -v `pwd`/data:/var/lib/rabbitmq --hostname myRabbit -e RABBITMQ_DEFAULT_VHOST=my_vhost  -e RABBITMQ_DEFAULT_USER=admin -e RABBITMQ_DEFAULT_PASS=admin df80af9ca0c9

```

## zipkin

https://zipkin.io/pages/quickstart.html

```sh
docker run --name zipkin-x -d -p 9411:9411 openzipkin/zipkin
```

http://localhost:9411/zipkin/


## nginx

```sh
# 保证 nginx 下有 conf, conf 文件夹复制自 nginx docker 内部 (docker container cp nginx:/etc/nginx .)
docker run -d --name nginx -p 80:80 -v ~/docker_data/nginx/html:/usr/share/nginx/html -v ~/docker_data/nginx/logs:/var/nginx/logs -v ~/docker_data/nginx/conf:/etc/nginx nginx

```

## mongodb

```sh

```