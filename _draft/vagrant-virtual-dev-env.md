---
title: vagrant-virtual-dev-env
tags:
---

https://github.com/hashicorp/vagrant

https://github.com/utmapp/UTM ios 平台, mac 平台

 firecracker, kata 之类新轻量虚拟机

<!--more-->
<!-- TOC -->

- [vagrant](#vagrant)
  - [基本使用](#基本使用)
  - [vagrant 设置 ip](#vagrant-设置-ip)
  - [VirtualBox四种网络模式](#virtualbox四种网络模式)
- [docker](#docker)
- [kali](#kali)
- [虚拟网卡 macvlan](#虚拟网卡-macvlan)
- [Multipass](#multipass)

<!-- /TOC -->

# vagrant

## 基本使用

https://www.vagrantup.com/downloads.html 下载
https://www.virtualbox.org/wiki/Downloads virtualbox 下载
https://learn.hashicorp.com/collections/vagrant/getting-started 入门

http://www.vagrantbox.es/ 搜索 box
https://app.vagrantup.com/boxes/search 搜索 box 
https://c4ys.com/archives/1230 国内镜像收集 ([清华 centos](http://mirrors.ustc.edu.cn/centos-cloud/centos/7/vagrant/x86_64/images/))

如何手动下载 box?
例如 找到 kali 的 box 页面 https://app.vagrantup.com/kalilinux/boxes/rolling, 点进 指定版本的页面 https://app.vagrantup.com/kalilinux/boxes/rolling/versions/2020.4.0 , 那么可以 `curl -L https://app.vagrantup.com/kalilinux/boxes/rolling/versions/2020.4.0/providers/virtualbox.box -O kali-2020.4.0.box`

```sh
# verify the installation
vagrant -v  

vagrant box list # list all available vm

# 下载好 box 文件然后
# 加载到 vagrant
vagrant box add <xxx_box_name> <box_file_Url_path>
vagrant init <xxx_box_name>         #初始化, create a vagrantfile in current folder
vagrant up        # 启动环境

# 也可以不下载 box file, 直接 init a box (box name can be find from some website), 但是可能很慢
vagrant init centos/7
vagrant up # 启动的系统 

# 使用 vagrant 用户连接 虚拟机 , 不用密码, 若仅仅一个 machine, 无需指定 machine name
# 切换 root, 也可使用 root/vagrant 登录 or : sudo su
vagrant ssh [machine_name]
vagrant upload <source> [dest] [name|id] # 上传文件

vagrant reload # 重启
vagrant halt  # 关闭虚拟机
vagrant status  # 查看虚拟机运行状态
vagrant destroy  # 销毁当前虚拟机

# 打包自己的 box 进行分发, 打包完成后会在当前目录生成一个 package.box 的文件
vagrant package


# 上传, 默认传到 vagrant home
vagrant upload xxx_file [dest_path]
```

## vagrant 设置 ip

配置虚拟机为固定 ip: 修改 vagrantfile , 配置为私有网络 (需要先使用 virtualbox 的主机网络管理器配置新增 hostonly 网络)

```
config.vm.network "private_network", ip:"virtualbox_host_only_ip"
``` 

virtualbox_host_only_ip 就是 宿主机 中 virtualbox 的 hostonly ip, 如 192.168.56.1/24


其实 Vagrant 中一共有三种网络配置, 

```
端口映射(Forwarded port):

config.vm.forwarded_port 80, 8080   将访问宿主计算机8080端口的请求都转发到虚拟机的80端口上
config.vm.forwarded_port 80, 8080, protocol: "udp"    默认只转发TCP包，UDP需要额外添加以下语句

好处:

简单易理解
容易实现外网访问虚拟机

坏处:

如果一两个端口需要映射很容易，但是如果有有很多端口，比如MySQL，MongoDB，tomcat等服务，端口比较多时，就比较麻烦。
不支持在宿主机器上使用小于1024的端口来转发。比如：不能使用SSL的443端口来进行https连接。


```

```
私有网络（Private network），只有主机可以访问虚拟机，如果多个虚拟机设定在同一个网段也可以互相访问，当然虚拟机是可以访问外部网络的

config.vm.network "private_network", ip: "192.168.50.4"   这里 ip 为 宿主机 上 virtualbox 对应的 host only ip 地址

好处:

安全，只有自己能访问

坏处:

因为私有的原因，所以团队成员其他人不能和你写作

```

```
公有网络（Public network），虚拟机享受实体机器一样的待遇，一样的网络配置，vagrant1.3版本之后也可以设定静态IP

config.vm.network "public_network", ip: "192.168.1.120"

公有网络中还可以设置桥接的网卡
config.vm.network "public_network", :bridge => 'en1: Wi-Fi (AirPort)'

好处:
方便团队协作，别人可以访问你的虚拟机

坏处:
需要有网络，有路由器分配IP

```

## VirtualBox四种网络模式

https://blog.csdn.net/qq_28513801/article/details/90138491


# docker


# kali

http://uni.mirrors.163.com/kali-images/kali-2020.4/
http://mirrors.ustc.edu.cn/kali-images/
http://old.kali.org/kali-images/kali-2020.4/


https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/

默认用户名是 root，默认密码是 toor
kali/kali


# 虚拟网卡 macvlan

https://fuckcloudnative.io/posts/netwnetwork-virtualization-macvlan/


# Multipass

类似 vagrant, 快速获取 Linux 环境

https://www.chenmo.com.cn/402492