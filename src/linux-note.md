---
title: Linux Note
tags:
  - linux
categories: java web
date: 2014-03-18 15:04:50
---

Linux Note | references: [1](http://linux.vbird.org/); 各种命令使用实例: https://linuxtools-rst.readthedocs.io/zh_CN/latest/ ; 50个常用命令: https://gywbd.github.io/posts/2014/8/50-linux-commands.html ; 

https://github.com/ckjbug/kali-Linux-learning 破解

https://linuxtools-rst.readthedocs.io/zh_CN/latest/base/01_use_man.html 学会使用 man 帮助命令

<!--more-->

<!-- TOC -->

- [常用命令](#常用命令)
  - [sed ack](#sed-ack)
  - [ssh config](#ssh-config)
  - [软件包相关](#软件包相关)
    - [启动 and 任务管理](#启动-and-任务管理)
    - [压缩解压](#压缩解压)
    - [ubuntu 软件相关](#ubuntu-软件相关)
    - [centos 软件相关](#centos-软件相关)
  - [系统](#系统)
    - [系统信息](#系统信息)
    - [搜索相关](#搜索相关)
    - [进程id pid](#进程id-pid)
  - [网络相关](#网络相关)
    - [系统网络](#系统网络)
    - [文件下载](#文件下载)
  - [文本相关](#文本相关)
    - [命令行文本命令](#命令行文本命令)
    - [vim 快捷键](#vim-快捷键)
- [介绍](#介绍)
- [主机规划-磁盘分区](#主机规划-磁盘分区)
  - [系统开机过程](#系统开机过程)
  - [硬件设备在Linux中的文件名称](#硬件设备在linux中的文件名称)
  - [磁盘组成](#磁盘组成)
  - [磁盘分割](#磁盘分割)
  - [挂载(mount):文件系统(磁盘)和目录树结合](#挂载mount文件系统磁盘和目录树结合)
- [linux系统目录规范/Filesystem Hierarchy Standard (FHS)](#linux系统目录规范filesystem-hierarchy-standard-fhs)
  - [详细目录用途说明](#详细目录用途说明)
  - [不能和[/]分开到不同分区的目录](#不能和分开到不同分区的目录)
  - [FHS推荐的目录规范](#fhs推荐的目录规范)
    - [[/]的内容和意义](#的内容和意义)
    - [[/usr]的内容和意义](#usr的内容和意义)
    - [[/var]的内容和意义](#var的内容和意义)
- [杂七杂八的问题](#杂七杂八的问题)
  - [命令帮助](#命令帮助)
    - [whatis](#whatis)
    - [info](#info)
    - [man](#man)
    - [which](#which)
  - [环境变量](#环境变量)
  - [日期时间日历计算器](#日期时间日历计算器)
- [常见问题](#常见问题)
  - [关机 重启](#关机-重启)
  - [root密码丢失了](#root密码丢失了)
  - [文件系统错误的问题](#文件系统错误的问题)
- [nano使用](#nano使用)
- [vi和vim使用](#vi和vim使用)
  - [快捷键](#快捷键)
  - [常用指令示意图](#常用指令示意图)
  - [几个常见的例子](#几个常见的例子)
  - [暂存档和数据恢复](#暂存档和数据恢复)
  - [区块选择](#区块选择)
  - [多档案编辑和多窗口功能](#多档案编辑和多窗口功能)
  - [vim环境参数设定](#vim环境参数设定)
  - [dos和linux下的换行符](#dos和linux下的换行符)
- [文件和目录的权限](#文件和目录的权限)
  - [3个身份3种权限](#3个身份3种权限)
  - [文件的属性,ls](#文件的属性ls)
  - [改变文件属性&权限](#改变文件属性权限)
    - [改变群组chgrp](#改变群组chgrp)
    - [改变拥有者chown](#改变拥有者chown)
    - [改变权限chmod](#改变权限chmod)
  - [文件和目录的权限区别](#文件和目录的权限区别)
  - [文件/目录的默认权限umask](#文件目录的默认权限umask)
  - [文件/目录的隐藏属性chattr](#文件目录的隐藏属性chattr)
  - [文件/目录的特殊权限SUID,SGID,SBIT](#文件目录的特殊权限suidsgidsbit)
    - [Set Uid](#set-uid)
    - [Set Gid](#set-gid)
    - [Sticky Bit](#sticky-bit)
  - [几个场景对应的权限分配](#几个场景对应的权限分配)
  - [几个场景实例](#几个场景实例)
- [文件和目录的管理](#文件和目录的管理)
  - [路径问题](#路径问题)
  - [处理目录的指令](#处理目录的指令)
  - [文件查看/复制/删除/移动](#文件查看复制删除移动)
    - [文件检视](#文件检视)
    - [文件复制/建立连接档(快捷方式)](#文件复制建立连接档快捷方式)
    - [删除文件/目录](#删除文件目录)
    - [移动or重命名](#移动or重命名)
    - [获取路径的文件名/目录名](#获取路径的文件名目录名)
  - [文件内容查看](#文件内容查看)
    - [cat和tac](#cat和tac)
    - [nl设计行号](#nl设计行号)
    - [翻页检视more/less](#翻页检视moreless)
    - ["行级别"的数据过滤head/tail](#行级别的数据过滤headtail)
    - [读取非文本数据od](#读取非文本数据od)
  - [创建文件/修改文件时间](#创建文件修改文件时间)
  - [搜寻文件和指令](#搜寻文件和指令)
    - [which,whereis,locate](#whichwhereislocate)
    - [find](#find)
  - [光盘写入工具和iso文件](#光盘写入工具和iso文件)
- [硬盘和文件系统管理](#硬盘和文件系统管理)
  - [ext2文件系统](#ext2文件系统)
    - [inode,block,superblock](#inodeblocksuperblock)
    - [fs格式化后的结构图](#fs格式化后的结构图)
    - [目录/文件和inode,block关系](#目录文件和inodeblock关系)
  - [ext2/ext3文件存取和日志式文件系统](#ext2ext3文件存取和日志式文件系统)
  - [磁盘/目录容量查看](#磁盘目录容量查看)
  - [实体链接和符号链接](#实体链接和符号链接)
  - [磁盘管理fdisk](#磁盘管理fdisk)
    - [分区/删除](#分区删除)
    - [格式化mkfs](#格式化mkfs)
    - [检验fsch,badblocks](#检验fschbadblocks)
    - [挂载mount/unmount](#挂载mountunmount)
    - [设定开机挂载](#设定开机挂载)
    - [设定磁盘参数](#设定磁盘参数)
  - [内存置换空间(swap)](#内存置换空间swap)
- [压缩和打包](#压缩和打包)
  - [常见的压缩工具](#常见的压缩工具)
  - [gzip和zcat](#gzip和zcat)
  - [bzip2和bzcat](#bzip2和bzcat)
  - [打包tar](#打包tar)
  - [dd](#dd)
  - [cpio](#cpio)
  - [dump完整备份工具](#dump完整备份工具)
  - [restore复原](#restore复原)
- [帐号管理&ACL权限设定](#帐号管理acl权限设定)
  - [user management](#user-management)
  - [ulimit限制用户的资源](#ulimit限制用户的资源)
- [周期任务](#周期任务)
- [磁盘阵列和LVM文件系统](#磁盘阵列和lvm文件系统)
- [周期任务](#周期任务-1)
- [程序管理](#程序管理)
  - [Job control背景前景](#job-control背景前景)
  - [程序](#程序)
  - [安装卸载](#安装卸载)
    - [原始码-tarball](#原始码-tarball)
      - [一般的安装指令](#一般的安装指令)
      - [c文件编译运行](#c文件编译运行)
      - [使用 make 简化编译](#使用-make-简化编译)
    - [包管理软件-rpm和srmp和 yum](#包管理软件-rpm和srmp和-yum)
      - [rpm](#rpm)
        - [rpm安装软件](#rpm安装软件)
        - [rpm更新软件](#rpm更新软件)
        - [rpm查询软件](#rpm查询软件)
        - [rpm验证和数字签名](#rpm验证和数字签名)
        - [rpm软件卸载和重建数据库](#rpm软件卸载和重建数据库)
      - [srpm](#srpm)
    - [apt和apt-get](#apt和apt-get)
    - [yum 使用](#yum-使用)
- [SELinux](#selinux)
- [系统服务 daemons](#系统服务-daemons)
  - [daemons是什么](#daemons是什么)
  - [daemons相关的文件脚本](#daemons相关的文件脚本)
  - [怎么启动](#怎么启动)
  - [设定开机启动](#设定开机启动)
  - [service 和 systemctl 和 chkconfig 区别](#service-和-systemctl-和-chkconfig-区别)
  - [supervisor 进程管理](#supervisor-进程管理)
- [操作记录](#操作记录)
  - [安装c编译环境gcc](#安装c编译环境gcc)
  - [安装nodejs](#安装nodejs)
  - [jdk环境](#jdk环境)
  - [maven环境](#maven环境)
  - [mysql环境](#mysql环境)
  - [安装 zsh](#安装-zsh)

<!-- /TOC -->

# 常用命令

## sed ack

```sh
# 替换 文件中的 xxx 为 yyy, 支持正则
sed -i 's/xxx/yyy/' <file_path>

```


## ssh config

```sh
Host kn1 
    HostName 192.168.56.102
    Port 22
    User root
    # optional, private key
    IdentityFile ~/.ssh/id_rsa
    

Host kn2 
    HostName 192.168.56.103
    Port 22
    User root

Host kn3 
    HostName 192.168.56.104
    Port 22
    User root

```

ssh kn1 即可登录


## 软件包相关

### 启动 and 任务管理

```sh
# 查服务是否启动
pidof mysqld                    # 返回进程id

# 启动程序

# https://wangchujiang.com/linux-command/c/nohup.html
# nohub是保证后台启动, 默认将日志打印到 ./nohup.out, 这里是指定打印日志到 xxx.file
# 若当前目录的 nohup.out 文件不可写，输出重定向到$HOME/nohup.out文件中
# & 产生作业编号就是那个 [1] 19649,nohup要和&一起用才能保证关闭窗口程序任然在运行
nohup <cmd> xxx.file 2>&1 &

# 用jobs命令查看后台运行，fg + 编号调到前台来
jobs
fg 1


```



### 压缩解压

```sh

# 压缩/解压
zip <zip name> <file1 file2...>
# zip name 可以和 folder name 不同
zip -r <zip_file_name> <folder1> [foder2...] 
unzip [-d <dest_dir>]  xxx.zip
# 使用 gzip (-z) 或 biz2 (-j) 解压缩 (-x) 压缩(-c)
tar [-C output_dir] -zxvf xxx


```



### ubuntu 软件相关

```sh
# ubuntu

dpkg -l | grep <httpd>          # 查看程序是否安装
dpkg -L | grep <httpd>          # 查看程序安装路径
aptitude show <httpd>           # 查看软件版本

# 搜索

apt-cache search package        # 搜索包
apt-cache show package          # 获取包的相关信息，如说明、大小、版本等

# 安装

apt-get install package [-y]        # 安装包[ 默认yes ]

# 卸载

apt-get remove package [--purge]          # 删除包[同时删除配置文件]
apt-get autoremove              # 自动删除不需要的包


```

### centos 软件相关



## 系统 

### 系统信息 

```sh
# 配置环境变量可能 吧 path 配置错误, 这时 vim 命令需要使用全路径, sudo 路径类似
# /usr/bin/vim , /usr/share/vim , /usr/share/man/man1/vim.1.gz


# system version info 系统版本
cat /proc/version
uname -a # kernel 版本
uname -r

# 发行版本 distribution version
lsb_release -a

# centos release version 发行版本
cat /etc/redhat-release

# 中文乱码
echo $LANG 显示当前语系. `LANG=en_US` 设置语系(暂时的, 再次登录会失效)


# >>> ps 

# 正在运行的程序
ps -ef | grep docker
ps -aux | grep docker


# >>> 磁盘占用

du -sh #查看当前目录所占空间大小
du -sh * #查看当前目录下文件、子目录所占空间大小
1.查寻磁盘剩余空间：df -h
2.查询某个目录下占用最大的子目录：
    cd /your_path
    du -sh * | sort -n 排序
    找到占用上G的目录：du -h --max-depth=1 |grep 'G' |sort



# 系统 cpu 负载
# 按数字 1，可以查看CPU的核数和每个CPU的使用情况。
# 按下 f 键盘可以调出更多pid进程显示选项。按esc键返回top显示页
# P 按CPU使用排序
# M 按内存使用排序
# H 显示线程
# Z 以多色彩显示top。
top
# 输出如下: (https://www.cnblogs.com/54chensongxia/p/12518705.html)
# top - 15:34:12 up 127 days, 10:23,  2 users,        load average: 0.04, 0.03, 0.00
#      系统时间    启动多久了          几个用户登录       过去 1， 5， 15 mins 内负载情况, 即运行队列中(在CPU上运行或者等待运行的有多少进
#                                                       程)的平均进程数 (单核cpu 0~1正常, >=1就需要优化了)
#                                                        (多核cpu, 负载数值/CPU核数 在0.00-1.00之间表示正常)
# Tasks: 291 total,   1 running, 290 sleeping,   0 stopped,   0 zombie
#       总任务                                                 僵尸状态
# Cpu(s):  0.0%us,                   0.3%sy,                0.0%ni,      98.3%id,       1.3%wa,             0.0%hi,  0.0%si,  0.0%st
#  用户模式下所花费 CPU 时间占比      内核进程占用cpu时间比              cpu空闲时间比   io等待占用cpu时间比
#    us + sy的参考值为80%, 超过说明cpu不足                                              (>30%则io严重)
# Mem:   1792312k total,   288300k used,  1504012k free,    10384k buffers
# Swap:  6291452k total,     5380k used,  6286072k free,    14128k cached
# 效果等同 free -ah -s 2 -c 2 显示所有的内存信息，每隔两秒显示一次，一共显示两次

# PID    USER  PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND           
# 372007 root  20   0 15160 1336  888 R  0.3  0.1   0:00.33 top                                   
#      1 root  20   0 19356  236   88 S  0.0  0.0   0:16.06 init 

# ... 下面省略...

# cpu 负载: 在一段时间内, cpu正在处理和等待处理的进程个数
# cpu 使用率: 进程真正使用cpu运算的时间/进程占用cpu时间 (进程占用cpu不代表正在使用cpu运算, 比如进程可能在进行  io操作)

# 高CPU负载 低CPU使用率：可能系统中较多的文件IO和网络IO操作。
# 高CPU负载 高CPU使用率：CPU资源不足
# 低CPU负载 低CPU使用率：系统CPU资源良好，道路非常顺畅；
# 低CPU负载 高CPU使用率：这种情况一般都是程序的问题，比如程序中代码进入死循环，有很多自旋操作等。CPU使用率一直过高对CPU伤害比较大。


# 系统内存负载 
# 显示所有的内存信息，每隔两秒显示一次，一共显示两次
free -h -s 2 -c 2
#              total       used       free     shared    buffers     cached  available
# Mem:          6.6G       5.5G       1.0G       1.1M       247M       3.5G         0B
# -/+ buffers/cache:       1.8G       4.8G 
# Swap:         5.0G       106M       4.9G 

# Men表示具体的物理内存
# Swap: 交换空间(swap space),是磁盘上的一块区域，可以是一个分区，也可以是一个文件.当系统物理内存吃紧时, Linux 会将内存中不常访问的数据保存到 swap 上. 但是它需要读写磁盘数据，所以性能不是很高。
# total：总计物理内存的大小。
# used：已使用的物理内存的大小。包括实际被程序吃掉的内存 + 缓存
# free：可用物理内存有多少。
# shared：多个进程共享的内存总额
# buffers：写盘内存缓冲区的大小; cached: 读盘内存缓冲大小
# -buffers/cache: 表示被程序实实在在吃掉的内存
# +buffers/cache：表示应用程序还可以可以申请的内存总数。
# free参数和available参数的区别: free 是真正尚未被使用的物理内存数量, 而 available 是从应用程序的角度看到的可用内存数量


# >>> 查找命令路径

# 查找命令当前使用的 path
which python3 # /usr/local/bin/python3

# 查找命令所有路径
where python3 
# /usr/local/bin/python3 这个是链接
# /usr/bin/python3

# >>> 查配置文件位置

rpm -qc bash
```

### 搜索相关


```sh

# 查找命令

find . -name 'docker*' # 当前目录下, docker 开头的文件
find . -name 'my*' -ls # 同上, 并且显示详细信息
find . -type f -mmin -10 # 过去10分钟中更新过的普通文件, (不加-type f参数，则搜索普通文件+特殊文件+目录)

locate -i /etc/sh # 类似 "find -name", 但是更快 (原因在于它不搜索具体目录，而是搜索一个数据库, 一般先 updatedb 一下)
    -i 忽略大小写

whereis grep # whereis命令只能用于程序名的搜索，而且只搜索二进制文件（参数-b）、man说明文件（参数-m）和源代码文件（参数-s）。如果省略参数，则返回所有信息

which grep # 在PATH路径中，搜索系统命令的位置，并且返回第一个结果 (可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令)

type cd # 系统会提示，cd是shell的自带命令（build-in）  (用来区分到底是shell自带, 还是shell外部的独立二进制文件提供的)
type -p grep # 系统会提示，grep是一个外部命令，并显示该命令的路径


```

### 进程id pid

```sh
pgrep <name> # eg: pgrep gunicorn, 显示所有 进程 id

pstree -ap|grep gunicorn # 查看进程树 ; 安装: apt-get install psmisc


```

## 网络相关

### 系统网络

```sh
# 查看 ip 地址

ifconfig # 查看所有网卡信息
ip addr show docker0 # 查看网卡 docker0 的信息

# 网络状态, 端口号

ps -aux | grep <httpd>          # 查看各个进程的资源占用, 顺便可以看看是否正在运行
lsof -i:<port>                  #查看端口占用情况
netstat -tunlp | grep <xxx>     # 查看端口号
netstat -ano | findstr "8761"   # 查看端口占用 (第二列看端口，最后一列是pid) Windows
    tasklist | findstr 9268    # 查看进程号
    tskill pid # 杀死进程(win8)
    taskkill -PID 进程号 -F # 杀死进程(win10)


ifconfig eth0 down //关闭网卡
ifconfig eth0 up //启动网卡
ifconfig eth0 hw ether 00:AA:BB:CC:DD:EE //修改MAC地址
# 设置 ip
ifconfig eth0 172.16.20.56 netmask 255.255.255.0 broadcast 172.16.20.255
ifconfig eth0 arp //开启arp协议
ifconfig eth0 -arp //关闭arp协议


# 防火墙
# http://blog.chinaunix.net/uid-26495963-id-3279216.html
# close firewall
systemctl stop firewalld
systemctl disable firewalld
```

### 文件下载

```sh
# 下载文件

# 推荐替代品 https://github.com/jakubroztocil/httpie/
# -L location
# --output == -o 指定文件 path
curl -L <source_url> > --output <target_file_name> 
curl -O -# <url> # 不指定文件名, -# 进度条, -s 静默下载
# 分块下载
curl -r 0-100 -o dodo1_part1.JPG http://www.linux.com/dodo1.JPG
curl -r 100-200 -o dodo1_part2.JPG http://www.linux.com/dodo1.JPG
curl -r 200- -o dodo1_part3.JPG http://www.linux.com/dodo1.JPG
cat dodo1_part* > dodo1.JPG
# 断点续传
curl -C -O http://www.linux.com/dodo1.JPG

wget [-O wordpress.zip] http://www.jsdig.com/download.aspx?id=1080

# ftp 下载
curl -O -u 用户名:密码 ftp://www.linux.com/dodo1.JPG
curl -O ftp://用户名:密码@www.linux.com/dodo1.JPG
# 上传
curl -T dodo1.JPG -u 用户名:密码 ftp://www.linux.com/img/

# 执行远程 shell脚本
curl -L <script url> | sh
curl -sSLf https://spacevim.org/cn/install.sh | bash


# web 访问

# 发送 http 请求: get/post/delete/put 
curl -X/--request [GET|POST|PUT|DELETE] {"https://xxx/"} # 加引号防止路径异常
    -v/--verbose 输出详细信息
    -X/--request [GET|POST|PUT|DELETE|…]  默认 发送 get 请求
    -H/--header        設定request裡的header, 如 -H "Content-Type: application/json"
    -i/--include                          顯示response的header 和网页,  -I 只显示 头信息
    -d/--data        設定 http parameters , 如 -d "param1=value1&param2=value2" 或者  -d "param1=value1" -d "param2=value2"; 发送json则将url参数让在url中, -d 只用于 json, 用引号包裹, 需要请求头 Content-Type:application/json
    -u/--user                             使用者帳號、密碼, 如 --user username:password
    -b/--cookie           添加 cookie  , 如: --header 'sessionid:1234567890987654321'; 或 从存档读取 cookie:  -b ~/cookie.txt
    -c    将 返回的 cookie 存档 如 -c  ~/cookie.txt
# 如
# 测试网页 resp code
curl -o /dev/null -s -w %{http_code} www.linux.com
# 设置代理
curl -x 192.168.100.100:1080 http://www.linux.com
# 保存 resp cookie
curl -c cookiec.txt http://www.linux.com
# 保存 header
curl -D cookied.txt http://www.linux.com
# 使用 cookie
curl -b cookiec.txt http://www.linux.com

# curl 配合 jq 解析 json 
# 如 : {"name": "han","age": "20"}
curl -s -d 'user=aa&pass=cc' http://www.example.com | jq '.name' >> file.txt
cat file.txt # han

# 模仿浏览器IE8.0
curl -A "Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.0)" http://www.linux.com

# 伪造referer请求头（盗链）
curl -e "www.linux.com" http://mail.linux.com
```

## 文本相关

### 命令行文本命令

```sh
# 查看文本文件
less -N <file name> # [b] 上一页，[d] 下一页

# read from stdin and send to file/stdout
#  -a append
# -i 忽略中断信号。
tee file1 file2  # 从键盘输入
# <<- 可以省掉 -
# eof 可以小写, 去掉引号
tee /etc/docker/daemon.json <<- 'EOF'
{
    "register-mirrors": [
        "https://registry.docker-cn.com"
    ]
}
EOF

# grep 按照指定文件类型搜索
grep -inr --include \*.h --include \*.cpp hello_world /your/search/path
-i :ignore 不区分大小写。
-n : line number 行号
-r : recursively 递归
--include：指定搜索的文件类型。

# 从指定文件夹中搜索指定文件。
find /search/path -name file_name
#例如,从/home目录下搜索名为a.txt的文件
find /home -name a.txt

```

### vim 快捷键

https://vimjc.com/ blog
https://github.com/wsdjeg/vim-galore-zh_cn 中文教程

```

比如想删除一行再进行编辑, 之前习惯 dd 再 insert, 其实有更简便的 S; 

再比如 a = "xyz" 想更换双引号为单引号, 使用 r 来一个一个替代, 其实有更简便的: 光标在 xyz 中, cs"'(change surrounding " to ').

 ZZ 替代 :wq
 :x 保存退出

( 向上跳转到空行和空行附近的一行
) 向下跳转到空行和空行附近的一行
{ } 和 ( ) 类似但不跳转空行的附近行
]] 跳转到下一个 {
[[ 跳转到上一个 {
][ 跳转下一个 }
[] 跳转到上一个 }
% 跳转到另一个花括号或圆括号



>> 浏览:

ctrl+d    "向下"移动 【半页】
ctrl+u    “向上”移动 【半页】
ctrl+f    向下一页
ctrl+b    向上一页
Ctrl+o      返回上一个位置
G         移动到【最后一行】
18G         移动到 18 行
:18         到第 18 行
gg        移动到【第一行】
enter     下一行有效字符行首         n+enter   向下移动n行行首
-         上一行有效行首

0         移动到行首
^           移动到有字符的行首
$           移动到行尾


w           下一个单词头部
W           下一个大单词头部
e           下一个单词尾部




>> 搜寻:

/keyword    回车 向下搜寻keyword， 按下【n】后， 继续向下搜寻下一个， 【N】表示向上搜寻; 找到目标后回车光标到达目标位置
?keyword    回车 向上搜寻
\<keyword\>  首尾精确匹配           “\>” 是一个特殊的记号，表示只匹配单词末尾。类似地，”\<” 只匹配单词的开头

:set ic(ignorecase缩写)   忽略大小写, 默认是区分大小写的
:set noic(noignorecase的缩写)   恢复大小写敏感

/\Cword 区分大小写的查找 
/word   (不加 \C 默认就是区分大小写的)

/\cword 不区分大小写的查找 



>> 行内查找

fx      查找本行第一个 x 字符, 使用分号查找下一个
Fx      反向查找





>> 选择/复制/删除: (复制/剪切的内容可以跨文件)

x        删除当前光标下的字符
d        删除
dd       删除一行               2dd         向下剪切 2 行
dw       删除光标之后的单词剩余部分(包括单词后的空格)。
daw      delete around word 删除光标所在的 整个单词, 包括单词后的空格
diw      delete inner Word 删除光标所在单词, 不包括空格
d$       删除光标之后的该行剩余部分。
dit      删除 标签内部内容
di"      删除 quotes 内部
dip      删除段落
das      删除整句


存储到缓冲区
1-9个数字缓冲区，用来保存最近删除的数据
a-z缓冲区用来保存保存用户需要临时保存的数据
如果用大写字母表示缓冲区，则数据被追加到后面


"ayy 把数据复制到a缓冲区里面 (前面要加上“)
"2p 把2号缓冲区的内容复制出来
"ap 把a缓冲区的内容复制出来


yy         即 yank. 复制整行， 
p       在光标下一行粘贴
v     进入 选择模式选中多行, 再按下 y 复制, 或者 d 剪切
V       选中当前行
ctrl + v   垂直选择 列选择
:1,.d       删除第一行到当前行 ("."当前行 ，"1,."表示从第一行到当前行 ，"d"删除)




>> 修改 (删除后进入插入模式)

c      即 change.  功能和d相同，区别在于完成删除操作后进入INSERT MODE
cc       删除当前行，然后进入INSERT MODE, 等同 S
ciw    change inner word 删除当前单词进入插入模式
ci"    change inner " 删除双引号内的字符, 等待插入
ct)     change to ) 删除到右括号(不包括右括号), 等待插入




>> 插入

i:在光标所在字符前开始插入
a:在光标所在字符后开始插入
o:在光标所在行的下面另起一新行插入
s:删除光标所在的字符并开始插入

I:在光标所在行的行首开始插入 如果行首有空格则在空格之后插入
A:在光标所在你行的行尾开始插入
O:在光标所在行的上面另起一行开始插入
S:删除光标所在行并开始插入





>> 撤销/恢复

u 撤销上一步的操作 
Ctrl+r 恢复上一步被撤销的操作




>> 重做

重复操作: https://blog.csdn.net/ii1245712564/article/details/46496347
https://learnku.com/articles/21986
https://www.jianshu.com/p/05ffb5823d96

重复类型	重复操作符	回退操作符
文本改变重复	.	u
行内查找重复	;	,
全文查找重复	n	N
文本替换重复	&	u
宏重复	@[寄存器]	u

>> 宏

qx 开始录制宏, 名为 x
@x 执行宏 x


>> 行号

:set nu     显示行号 nu === number
:set nonu   取消显示行号 nonu === nonumber

vim xxx +13 # 打开 xxx 光标定位到 13 行


>> 执行命令 并读取结果

:read !ls *pdf


>> 缓冲区

vim file1 file2 file3...     一次打开多个文件, 每个文件对应一个缓冲区, 第一个文件会被现实到窗口

:badd [+99] filename   新增一个缓冲区加载指定文件 (可指定光标行数)

:buffers, :files, :ls     列出所有缓冲区 (第一个数字是缓冲区的标号；第二个标记是缓冲区当前的状态；紧接着是与缓冲区所关联的文件名)
            状态包括: 
            1. - 或 = 只读
            1. a 激活, 当前正显示在窗口
            1. h 隐藏, 已经载入, 但是没有显示在窗口
            1. + 已经更改过的缓冲区
            1. x 读取时报错
            1. % 当前缓冲区(current buffer)
            1. # alternate buffer 交换缓冲区
            1. u unlisted buffer 只有 :ls! 才显示的 buffer

:buffer number/filename 显示缓冲区

:sbuffer number/filename    分割当前窗口开始编辑一个缓冲区

切换缓冲区 (根据默认设置，在切换到另一缓冲区之前，Vim将提示你保存当前缓冲区。你也可以使用:set hidden命令，允许在未保存的情况下切换缓冲区)

:bnext到下一个缓冲区；
:bprevious, :bNext到前一个缓冲区；
:blast到最后一个缓冲区；
:bfirst到第一个缓冲区


:bdelete filename，:bdelete 3或:3 bdelete命令来删除一个缓冲区      有改动, 则无法删除, 
:bdelete! filename   强制删除
:1,3 bdelete命令来删除指定范围的缓冲区


:bunload filename 卸载 buffer, 类似 :bdelete, 但是 文件名仍然留存在 buffer list 中
```

# 介绍

* Linux kernel 版本: 主次版本为奇数(如2.5.xx)表示发展中版本(development), 主次版本为偶数(如2.6.xx)表示稳定版(stable)

* Linux 发行版本(distribution):如centos, 是我们常用的版本, 表示主次版本架构不变的情况下, 新增的功能积累到一定的程度新释出的核心版本; 

    所以碰到问题正确的提问方式是: 我的Linux是centos这个distribution, 版本为5.x, 请问.....

    著名的distribution: red hat的rhel(服务器), suse(服务器), ubuntu(桌面), fedora(桌面), debian(桌面), openSuse(桌面)


# 主机规划-磁盘分区

## 系统开机过程

1. BIOS：开机主动执行, 找到可开机的设备(如硬盘)

1. MBR：主要启动记录区(Master Boot Record, MBR)
   
   可以安装开机管理程序(boot loader)的地方

2. 开机管理程序(boot loader)：一支可读操作系统启动程序来执行的软件, 可以安装在MBR 和 每个分区的 boot sector(启动扇区), 这是一个重要的设计, 这样就可以制作多重引导的环境(多系统), 因为我们可以将不同的boot loader安装到不同的分区的最前端(boot sector)而不用覆盖整个磁盘唯一的MBR; boot loader可以有这些作用:

    - 多系统启动：用户可以选择不同的开机项目，这也是多重引导的重要功能

    - 载入操作系统启动程序

    - 转交其他 loader：将开机管理功能转交给其他 loader 负责

3. 操作系统启动程序：开始操作系统的功能

BIOS 和 MBR 都是硬件本身会支持的功能,  Boot loader 则是操作系统安装在 MBR 上面的一套软件

## 硬件设备在Linux中的文件名称

![](Screenshot_1.png)

![](Screenshot_2.png)

![](Screenshot_8.png)

## 磁盘组成

![](Screenshot_3.png)

![](Screenshot_4.png)

磁柱是磁盘分区的最小单位
整个磁盘的第一个扇区特别重要, 记录了整个磁盘的元信息, 记录2点信息:

1. 主要启动记录区(Master Boot Record, MBR)：可以安装开机管理程序的地方，有 446 bytes
2. 分割表(partition table)：记录整颗硬盘分割癿状态，有 64 bytes

下面着重谈谈磁盘分区表(partition table)

![](Screenshot_5.png)

* 其实所谓癿『分割』叧是针对那个 64 bytes 癿分割表迚行设定而已
* 硬盘默讣分割表仅能写入四组分割信息
* 这四组分割信息我们称为主要(Primary) or 延伸(Extended)分割槽
* 当系统要写入磁盘时，一定会参考磁盘分区表，才能针对某个分割槽迚行数据处理
* 延伸分割最多只能有1个, 这1个继续分割出多个逻辑分区

## 磁盘分割

为什么要分割?

* 数据安全
* 提高系统性能(如: 当有数据需要读取自分割后的分区时, 磁盘只会搜寻1-100磁柱而不会搜寻全部磁盘)

![](Screenshot_6.png)

硬盘四个分割记录区仅使用到两个，P1 为主要分割，而 P2 则为延伸分割, 延伸分割可以继续分割多个逻辑分区
上述分区在Linux中文件名为 /dev/hda[1-2], /dev/hda[5-9]

怎举装置文件名没有/dev/hda3 , /dev/hda4 呢？因为前面四个号码都是保留给 Primary 和 Extended 用的, 所以逻辑分割槽装置名称号码就由 5 号开始

* 简单自定义分割, 适合新手:分割[/(根目录)]和[swap(内存置换空间)]即可, [/]对应的磁盘要容量最大; 此外如果可以, 预留一个备用的剩余磁盘容量供练习分区or 备份之用
    * 如: 20G硬盘, 15G的分区给[/], 512M的分区给[swap], 剩下的作为预留备份磁盘
* 最好的分割: 根据用途不同, 为 /, /boot, /usr, /home, /var, /Swap 分配合适的磁盘空间; 如: 希望邮件主机大点, 则 /var 分配大点. 开放samba服务, 给其他人存放资料, /home 分配大点. 

## 挂载(mount):文件系统(磁盘)和目录树结合

![](Screenshot_7.png)

# linux系统目录规范/Filesystem Hierarchy Standard (FHS)

![](Screenshot_26.png)

![](Screenshot_27.png)

## 详细目录用途说明

简单版本:

/ 根目录下

```
- opt/ 安装的自定义软件包, tomcat, node, redis...

- etc/ 程序的全局配置文件 (系统密码)
- etc/passwd 用户数据库
- etc/shadow 影子口令文件。就是密码文件, 只对root可读。这使破译口令更困难
- etc/group 用户组
- etc/init.d 启动脚本
- etc/rc 启动脚本
- etc/rc.d

- usr/ (unix system resource) 安装发行版官方软件包的位置
- usr/bin 大部分软件的命令
- usr/sbin root用户才能执行的命令
- usr/share 程序资源文件(MySQL配置文件模板)
- usr/local 用户自己安装的程序的快捷方式 (下载到 /opt/ 下的程序会在这里配置软链接)

- var/ 动态数据。这个目录的内容是经常变动的
- var/www apache服务器网站内容
- var/log 日志文件, 需要定期删除
- var/lib 会动态改变的数据文件 (MySQL的数据文件)

- bin/ 开机, 进入单人维护模式 相关的命令, 系统程序相关命令(ls , mkdir)
- sbin/ 开机, 进入单人维护模式 相关命令, root的命令

```

详细版本:

/ 根目录    (/必须和如下目录在一个分区: /bin, /sbin, /etc, /dev, /lib; 而/home, /usr, /var, /tmp最好和/在不同分区)  - 下面的目录都在根目录下


- boot/    启动文件[开机配置文件]。Linux的内核及引导系统程序所需要的文件目录，比如 vmlinuz initrd.img 文件都位于这个目录中。在一般情况下，GRUB或LILO系统引导管理器也位于这个目录；

    - grub/             Grub引导器相关的文件

- dev/      设备文件。设备文件在安装是产生，以后可以用 /dev/MAKEDEV 描述。 /dev/MAKEDEV.local 是系统管理员为本地设备文件(或连接)写的描述文稿 (即如一些非标准设备驱动不是标准MAKEDEV 的一部分)。

- opt/      表示的是可选择的， 额外的意思，比如自定义软件包，额外安装的可选应用程序包所放置的位置。一般情况下，我们可以把tomcat等都安装到这里；。有些我们自己编译的软件包，就可以安装在这个目录中；通过源码包安装的软件，可以通过 ./configure --prefix=/opt/目录 。

- proc/    是一个假的文件系统。它不存在在磁盘某个磁盘上。而是由核心在内存中产生。用于提供关于系统的信息(originally about processes, hence the name)。 虚拟文件系统目录，是系统内存的映射。可直接访问这个目录来获取系统信息。 /proc 文件系统在proc man页中有更详细的说明。

    - 1/                   关于进程1的信息目录。每个进程在/proc 下有一个名为其进程号的目录。

    - cpuinfo          处理器信息，如类型、制造商、型号和性能。

    - devices         当前运行的核心配置的设备驱动的列表。

    - dma               显示当前使用的DMA通道。

    - filesystems核心配置的文件系统。

    - interrupts    显示使用的中断，and how many of each there have been.

    - ioports 当前使用的I/O端口。

    - kcore             系统物理内存映象。与物理内存大小完全一样，但不实际占用这么多内存；it is generated on the fly as programs access it. (记住：除非你把它拷贝到什么地方，/proc 下没有任何东西占用任何磁盘空间。)

    - kmsg             核心输出的消息。也被送到syslog 。

    - ksyms            核心符号表。

    - loadavg         系统"平均负载"；3个没有意义的指示器指出系统当前的工作量。

    - meminfo       存储器使用信息，包括物理内存和swap。

    - modules       当前加载了哪些核心模块。

    - net                 网络协议状态信息。

    - self                 到查看/proc 的程序的进程目录的符号连接。当2个进程查看/proc 时，是不同的连接。这主要便于程序得到它自己的进程目录。

    - stat                系统的不同状态，such as the number of page faults since the system was booted.

    - uptime          系统启动的时间长度。

    - version          核心版本。

- mnt/     临时挂载另外的文件系统。这个目录一般是用于存放挂载储存设备的挂载目录的，比如有cdrom 等目录。可以参看/etc/fstab的定义。有时我们可以把让系统开机自动挂载文件系统，把挂载点放在这里也是可以的。

    - cdrom           光驱。

- media/ 挂载媒体设备

- root/    root用户的$HOME目录

- home/  普通用户的$HOME目录

    - user/             用户

- bin/       系统程序。存放二进制可执行文件(ls,cat,mkdir等)，常用命令一般都在这里；

    功能和/usr/bin类似，区别是/bin主要放置在开机时用到的以及进入单人维护模式还能够被使用的命令;而/usr/bin放置的是大部分软件提供的指令。

- sbin/     管理员系统程序, /bin和/sbin都和开机/单人维护模式有关。

    大多是涉及系统管理的命令的存放，是超级权限用户root的可执行命令存放地，普通用户无权限执行这个目录下的命令，
    
    这个目录和/usr/sbin; /usr/X11R6/sbin或/usr/local/sbin目录是相似的；
    
    凡是目录sbin中包含的都是root权限才能执行的。

- lib/        程序所需的共享库 （库文件）。可能还有 lib64/

    - modules       核心可加载模块，特别是那些恢复损坏系统时引导所需的(例如网络和文件系统驱动)。
│
- etc/      系统程序和大部分应用程序的`全局配置文件`,尤其passwd, shadow

    - init.d/   SystemV风格的启动脚本[系统开机时候加载服务的scripts的摆放地点]

    - rcX.d/    启动脚本的链接，定义运行级别

    - network/      网络配置文件

    - X11/               图形界面配置文件

    - rc or rc.d or rc?.d         启动、或改变运行级时运行的scripts或scripts的目录。

    - passwd                  用户数据库，其中的域给出了用户名、真实姓名、家目录、加密的口令和用户的其他信息。格式见passwd 的man页。

    - fdprm            软盘参数表。说明不同的软盘格式。用setfdprm 设置。更多的信息见setfdprm 的man页。

    - fstab              启动时mount -a命令(在/etc/rc 或等效的启动文件中)自动mount的文件系统列表。 Linux下，也包括用swapon -a启用的swap区的信息。见4.8.5节和mount 的man页。

    - group            类似/etc/passwd ，但说明的不是用户而是组。见group 的man页。

    - inittab  init 的配置文件。

    - issuegetty 在登录提示符前的输出信息。通常包括系统的一段短说明或欢迎信息。内容由系统管理员确定。

    - magicfile      的配置文件。包含不同文件格式的说明，file 基于它猜测文件类型。见magic 和file 的man页。

    - motd             Message Of The Day，成功登录后自动输出。内容由系统管理员确定。经常用于通告信息，如计划关机时间的警告。

    - mtab             当前安装的文件系统列表。由scripts初始化，并由mount 命令自动更新。需要一个当前安装的文件系统的列表时使用，例如df 命令。

    - shadow                  在安装了影子口令软件的系统上的影子口令文件。影子口令文件将/etc/passwd 文件中的加密口令移动到/etc/shadow 中，而后者只对root可读。这使破译口令更困难。

    - login              defslogin 命令的配置文件。

    - printcap        类似/etc/termcap ，但针对打印机。语法不同。

    - profile,csh.login,csh.cshrc  登录或启动时Bourne或C shells执行的文件。这允许系统管理员为所有用户建立全局缺省环境。各shell见man页。

    - securetty     确认安全终端，即哪个终端允许root登录。一般只列出虚拟控制台，这样就不可能(至少很困难)通过modem或网络闯入系统并得到超级用户特权。

    - shells             列出可信任的shell。chsh 命令允许用户在本文件指定范围内改变登录shell。提供一台机器FTP服务的服务进程ftpd 检查用户shell是否列在 /etc/shells 文件中，如果不是将不允许该用户登录。

    - termcap       终端性能数据库。说明不同的终端用什么"转义序列"控制。写程序时不直接输出转义序列(这样只能工作于特定品牌的终端)，而是从 /etc/termcap 中查找要做的工作的正确序列。这样，多数的程序可以在多数终端上运行。见termcap 、 curs_termcap 和terminfo 的man页。

- usr/      这个是系统存放程序的目录 （Unix system resource），比如命令、帮助文件等。这个目录下有很多的文件和目录。当我们`安装一个Linux发行版官方提供的软件包时，大多安装在这里`。如果有涉及服务器配置文件的，会把配置文件安装在/etc目录中。

    - bin/                几乎所有用户命令。有些命令在/bin 或/usr/local/bin 中。[一般用户指令]

    - sbin/              根文件系统不必要的系统管理命令，例如多数服务程序。[系统管理员指令]

    - lib/                 应用程序库文件，程序或子系统的不变的数据文件

    - share/           应用程序资源文件

        - fonts              字体目录

        - man 或 doc        帮助目录

    - src/                应用程序源代码

    - local/             本地安装的软件和其他文件放在这里。

        - soft/              用户程序      ， 下面还有单独文件夹， 通常使用单独文件夹

    - X11R6/                   图形界面系统

    - include/        C编程语言的头文件。为了一致性这实际上应该在/usr/lib 下，但传统上支持这个名字。

- var/      动态数据。这个目录的内容是经常变动的；

    - www/            目录是定义Apache服务器站点存放目录；

    - catman/       当要求格式化时的man页的cache。man页的源文件一般存在/usr/man/man* 中；有些man页可能有预格式化的版本，存在/usr/man/cat* 中。而其他的man页在第一次看时需要格式化，格式化完的版本存在/var/man 中，这样其他人再看相同的页时就无须等待格式化了。 (/var/catman 经常被清除，就象清除临时目录一样。)

    - lib/                 `系统正常运行时要改变的一些库文件，比如MySQL`的，以及MySQL数据库的的存放地

    - local/             安装的程序的可变数据(即系统管理员安装的程序)。注意，如果必要，即使本地安装的程序也会使用其他/var 目录，例如/var/lock 。

    - lock/              锁定文件。许多程序遵循在/var/lock 中产生一个锁定文件的约定，以支持他们正在使用某个特定的设备或文件。其他程序注意到这个锁定文件，将不试图使用这个设备或文件。

    - log/                各种程序的Log文件,[摆放系统注册表档案的地方]，特别是login (/var/log/wtmp log所有到系统的登录和注销) 和syslog (/var/log/messages 里存储所有核心和系统程序信息。 /var/log 里的文件经常不确定地增长，应该定期清除。

    - run/               保存到下次引导前有效的关于系统的信息文件。例如， /var/run/utmp 包含当前登录的用户的信息。

    - spool/            mail, news, 打印队列和其他队列工作的目录。每个不同的spool在/var/spool 下有自己的子目录，例如，用户的邮箱在/var/spool/mail 中。

    - tmp/              比/tmp 允许的大或需要存在较长时间的临时文件。 (虽然系统管理员可能不允许/var/tmp 有很旧的文件。)

- temp/                     临时文件目录，有时用户运行程序的时候，会产生临时文件。/tmp就用来存放临时文件的。/var/tmp目录和这个目录相似。

- lost+found/  在ext2或ext3文件系统中，当系统意外崩溃或机器意外关机，而产生一些文件碎片放在这里。当系统启动的过程中fsck工具会检查这里，并修复已经损坏的文件系统。 有时系统发生问题，有很多的文件被移到这个目录中，可能会用手工的方式来修复，或移到文件到原来的位置上。



## 不能和[/]分开到不同分区的目录

另外要注意的是，因为[/]和开机有关，开机过程中仅有[/]会被挂载, 其他分割槽则是在开机完成后才会持续的挂载。就是因为如此，因此[/]下和开机过程有关的目录就不能够和[/]放到不同的分割槽去, 哪些目彔不可与[/]分开呢？有底下这些：

*  /etc：配置文件 
*  /bin：重要执行档 
*  /dev：所需要的装置档案 
*  /lib：执行档所需的函式库/核心所需的模块 
*  /sbin：重要的系统执行文件 

## FHS推荐的目录规范

![](Screenshot_33.png)

### [/]的内容和意义

![](Screenshot_34.png)![](Screenshot_28.png)![](Screenshot_29.png)![](Screenshot_30.png)![](Screenshot_31.png)![](Screenshot_32.png)

### [/usr]的内容和意义

其实 usr 是 Unix Software Resource 的缩写， 也就是『Unix操作系统软件资源』所放置的目录，而不是用户的数据

依据 FHS 的基本定义，/usr 里面放置的数据属于可分享的不可变动的(shareable, static)

类似 Windows 系统的『C:\Windows\ + C:\Program files\』这两个目录的综合体，系统刚安装完毕时，这个目录会占用最多的硬盘容量。 一般来说，/usr 的次目录建议有底下这些:

![](Screenshot_35.png)![](Screenshot_36.png)

### [/var]的内容和意义

![](Screenshot_37.png)![](Screenshot_38.png)![](Screenshot_39.png)

# 杂七杂八的问题

## 命令帮助

### whatis

`whatis command` 命令简述 , 显示命令所在的具体的文档类别

`whatis -w "loca*"` 正则匹配

### info

`info command` 更加详细的说明文档:类似于man, 只支持info格式的说明文件

### man

帮助文档分为了如下9个类别( 一般我们查询bash命令，归类在1类中 )

```
(1)、用户可以操作的命令或者是可执行文件
(2)、系统核心可调用的函数与工具等
(3)、一些常用的函数与数据库
(4)、设备文件的说明
(5)、设置文件或者某些文件的格式
(6)、游戏
(7)、惯例与协议等。例如Linux标准文件系统、网络协议、ASCⅡ，码等说明内容
(8)、系统管理员可用的管理条令
(9)、与内核有关的文件
```

* `man 3 printf` 查看第三类帮助文档

* `man -k keyword` 关键字查询命令

* `man command` 查看指令, 例如: man date, 空格翻页, q退出, `/keyword`回车键搜索, n跳到下一个搜索目标, N跳到上一个目标

    ![](Screenshot_14.png)

    其中 DATE(1)中的"1"表示什么呢?

    ![](Screenshot_15.png)

* `man -f command`(等同于`whatis command`, 前提是建立命令数据库:`makewhatis`) 查看更多命令相关信息, 严格匹配下图左边的命令名称, 如果需要更具关键字匹配 `man -k command`(等同于`apropos command`)

    ![](Screenshot_16.png)

    有两个相关命令: man(1), man(7), 查看指定命令 `man 1 man`or `man 7 man`;
    如果仅仅使用 `man man` 到底使用哪个描述文件呢? 在/etc/man.conf里配置

### which

`which command` 查看程序的binary文件(可直接执行的文件)所在路径

`whereis command` 查看程序的搜索路径(一般是压缩文件)




## 环境变量

* 环境变量

    * `echo $PATH` 查看环境变量(每个目录使用冒号分割, 有顺序之分)
    * ` PATH="$PATH":/root ` 修改环境变量
    * 为了安全起见，不建议将『.』加入 PATH 的搜寻目录中;
    * 不同身份使用者预设置的 PATH 不同，默认能够随意执行的命令也不同(如 root 和 vbird)
    * PATH 可以修改，所以一般使用者还是可以透过修改 PATH 来执行某些位亍/sbin 或/usr/sbin 下的指令；

## 日期时间日历计算器

* 日期时间(date),日历(cal),计算器(bc)

    <img src="Screenshot_10.png" width=70%><img src="Screenshot_11.png" width=70%><img src="Screenshot_12.png" width=70%><img src="Screenshot_13.png" width=70%>

# 常见问题

## 关机 重启

1. 首先查看系统使用状态, `who`看看有谁在线, `netstat -a`看看网络的联机状态, `ps -aux`看看后台执行的程序

2. 通知在线的使用者关机时间

3. 关机, shutdown/reboot

![](Screenshot_18.png)

模拟一个关机过程:

首先 `sync`内存数据同步到磁盘;

知道shutdown但是不知道具体用法, `man shutdown`查看; 用法为: shutdown [-t 秒] [-arkhncfF] 时间 [警告讯息] 

![](Screenshot_19.png)

看几个例子:

`shutdown -h 10 'I will shutdown after 10 min'` 10 min后关机, 并发送警告信息出去
`shutdown -h +10`10 min后关机
`shutdown -h now`立马关机
`shutdown -h 10:15` 今天的10:15分会关机, 如果命令键入时间晚于这个时间, 会第二天的这个时间点执行关机
`shutdown -r now` 马上重启
`shutdown -r +30 'The system will reboot after 30 min'`30 min后reboot, 并发送信息
`shutdown -k now 'some message'` 给所有在线者发送信息, 不会真的关机

`sync;sync;sync;reboot`同步数据重启

关机还有另一种方式: `init 0`, 解释如下

![](Screenshot_20.png)

重启: `reboot`

## root密码丢失了

进入单人维护模式:
先将系统重新启劢，在读秒的时候按下任意键, 摁下『e』就能够进入 grub 的编辑模式了, 此时，请将光标移到 kernel 那一行，再按一次『 e 』进入 kernel 所在的这行的编辑画面中,  然后在出现的画面当中的最后方输入 single , 再按下『 Enter 』确定后，按下 b 就可以开机进入单人维护模式了. 在这个模式底下，你会在 tty1 下无需要输入密码即可取得终端机的控制权(而且是使用 root 的身份)。 之后就能够修改 root
的密码了(使用`passwd`)

## 文件系统错误的问题

在开机的过程中最容易遇到的问题就是硬盘可能有坏轨or文件系统发生错误(数据损毁)的情况; 
* 如果你的根目录『/』没有损毁，那就径容易解决: `fsck 某个分区文件名`进行文件系统检查,  这时屏幕会显示开始修理硬盘的讯息，如果有发现任何的错时，屏幕会显示： clear [Y/N]？ 的询问讯息，就直接输入 Y 吧, 最后reboot即可
* 如果根目录毁坏了, 这时你可以将硬盘拔掉，接到另一台 Linux 系统的计算机上， 且不要挂载(mount)该硬盘，然后以 root 的身份执行『fsck /dev/sdb1 』(/dev/sdb1 是你的硬盘装置文件名，你要依你的实际状况来定)

# nano使用

![](Screenshot_17.png)

# vi和vim使用

https://vim-adventures.com/ vim游戏
https://mp.weixin.qq.com/s/_XncbAGmfeWDSG04Ce51vA tips

<img src="Screenshot_145.png" width=50%>

* 一般模式: 默认模式, 可移动光标, 删除, 复制粘贴; 无法编辑内容
* 编辑模式: 一般模式中, 按下[i, o, a, r] 进入, 按下[Esc]退出
* 命令模式: 一般模式中, 按下[:, /, ?]三个中的任何一个, 即可进入, 可 搜索, 保存, 离开vim/vi, 大量取代字符


## 快捷键

<img src="Screenshot_146.png" width=70%><img src="Screenshot_147.png" width=70%><img src="Screenshot_148.png" width=70%><img src="Screenshot_149.png" width=70%><img src="Screenshot_150.png" width=70%><img src="Screenshot_151.png" width=70%><img src="Screenshot_152.png" width=70%><img src="Screenshot_153.png" width=70%><img src="Screenshot_154.png" width=70%><img src="Screenshot_155.png"width=70%>

## 常用指令示意图

## 几个常见的例子

## 暂存档和数据恢复

## 区块选择

## 多档案编辑和多窗口功能

## vim环境参数设定

## dos和linux下的换行符

<img src="Screenshot_156.png" width=70%>

# 文件和目录的权限

## 3个身份3种权限

linux中有3种身份, owner/group/others, 且各有read/write/execute权限

帐号信息--/etc/passwd 
密码信息--/etc/shadow
群组信息--/etc/group

## 文件的属性,ls

![](Screenshot_21.png)

1. 第一栏的文件权限属性的详细解释:

    * 第一个字符表示这个数据是 `目录(d)`or `文件(-)`or `连接档link file(l)` or `可存取设备(b)` or `鼠标/键盘串行端口设备(c)`
    * 接着的3个字符: owner的权限
    * 接着的3个字符: 同group的人的权限
    * 接着的3个字符: other身份的权限

2. 第二栏的连接数表示有多少文件名连接到这个节点(i-node); 这个属性记录的，就是有多少不同的档名连结到相同的一个 i-node 号码去了.

3. 第三栏表示拥有者是谁

4. 第四栏表示属于哪一个分组

5. 第五栏表示file size ,单位默认bytes, 如果希望显示完整的时间`ls -l --full-time`

6. 第六栏表示更新日期/创建日期

7. 第七栏为这个档案的档名, 如果以[.]开头, 表示为隐藏文件, 也就是`ls`和`ls -a`的区分

## 改变文件属性&权限

![](Screenshot_22.png)

### 改变群组chgrp

被改变的组名必项要在/etc/group 档案内存在才行，否则就会显示错误

![](Screenshot_23.png)

### 改变拥有者chown

在/etc/passwd 这个档案中有记录的用户名称才能改变

chown 用处很多, 比如同时修改owner和group,仅修改owner, 仅修改group 

`chown <owner>:<group> <file>` 同时修改
`chown <owner> <file>` 仅修改owner
`chown .<group> <file>` 仅修改group

在哪里使用呢? 最常见的例子就是在复制档案(`cp <source file> <target file>`)给你之外的其他人, 复制后需要修改文件属性/权限

### 改变权限chmod

![](Screenshot_24.png)

* 使用数字表示权限

    r: 4, w: 2, x: 1

    例如: -rwxrwx---, 分数为
    owner: rwx=4+2+1=7
    group: rwx=7
    other: ---=0+0+0=0
    所以命令为 `chmod 770 <file>`

    再比如: 编写一个shell文件, 希望给别人执行, 不能修改, 需要 -rwxr-xr-x, 键入命令 `chmod 755 <file>`

* 使用符号表示权限

    基本上九个权限分别是(1)user (2)group (3)others 三种身份, 那我们就可以由 u, g, o 来代表三种身份的权限！此外, a 则代表 all 亦即全部的身份.

    ![](Screenshot_25.png)

    例如: 设定 -rwxr-xr-x, 
    user(u): rwx
    group(g): rx
    other(o): rx
    所以命令为 ` chmod u=rwx,go=rx <file> `

    再比如: 不知道原先文件权限, 只是希望给其增加每个人的[w]权限, `chmod a+w <file>`
    再比如: 不知道原先文件权限, 只是希望拿掉每个人对文件的[x]权限, `chmod a-x <file>`

## 文件和目录的权限区别

文件是实际存放数据的地方，包括一般文本文件、数据库内容文件、二进制可执行文件(binary program)等等, rwx对文件的意义是这样的:

* r (read)：可读取此一档案的实际内容，如读取文本文件的文字内容等； 
* w (write)：可以编辑、新增or修改该档案的内容(但不能删除该档案)； 
* x (execute)：该档案具有可以被系统执行的权限

目彔是记录文件名列表，文件名与目录有强烈的联系, rwx对于目录的意义:

* r (read contents in directory)： 
 
    表示具有读取目录结构列表的权限，所以当你具有读取(r)一个目录的权限时，表示你可以查询该目录下的文件名数据。 所以你就可以利用 `ls` 这个指令将该目录的文件名列表显示出来

* w (modify contents of directory)： 
 
    这个可写入的权限对目录来说，是很大的权限. 因为他表示你具有改动该目录文件名列表的权限，也就是底下这些权限： 
    * 建立新的文件/目录； 
    * 删除已经存在的文件/目录(不管该文件的权限为何) 
    * 将已存在的文件/目录更名； 
    * 改变该目录内的文件、目录位置。 
    
    总之, 目录的 w 权限就和该目录底下的文件名异动有关.

* x (access directory)

    目录的 x 权限代表的是用户能否进入该目录成为工作目录, 也就是`cd <dir>`能否顺利进入. 所谓的工作目录(work directory)就是你目前所在的目录.

## 文件/目录的默认权限umask

文件新增时, 如果不指定权限, 会有默认权限, umask设定

如何查看默认权限呢?

![](Screenshot_54.png)

可以看出umask的结果表示owner, group, other `被拿掉的权限`

比如图中 0`022`: 第一个"0"表示特殊权限暂时不看, 第二个"0"表示owner没有被拿掉任何权限(rwx), 第一"2"表示group被拿掉了w权限, 第二个"2"表示other被拿掉了w权限

新建的文件和目录的默认权限设置是不同的:

* 新建文件, 默认任何人都没有"x"权限,都有"rw", 也就是文件没有被"umask设定的排除权限"处理之前, 权限属性为 -rw-rw-rw-
* 新建目录, 默认所有权限对任何人都开放, 也就是目录没有被"umask设定的排除权限"处理之前, 权限属性为 drwxrwxrwx

所以上图中如果新建文件, 最终权限为 (-rw-rw-rw-) 减去 (-----w--w-) ==> -rw-r--r-- . 如果新建目录, 最终权限为 (drwxrwxrwx) 减去(d----w--w-) ==> drwxr-xr-x 

修改文件/目录的默认权限: `umask <xxx>` 如: `umask 002`新建的文件自己和同group的人都可以r和w

root用户的umask比较严格, 会拿掉比较多的权限, 一般为'022', 这时为了安全考虑;
普通用户的umask就比较宽松, 拿掉的权限比较少, '002', 同组的人可以读写


## 文件/目录的隐藏属性chattr

隐藏属性在系统文件安全方面作用很大, chattr只能在 Ext2/Ext3 的文件系统上面生效， 其它文件系统可能就无法支持这个指令了

![](Screenshot_55.png)![](Screenshot_56.png)![](Screenshot_57.png)

* `chattr +i <file>` 使得文件无法增删改, 更名, root才能设定该权限
* `chattr -i <file>` i属性取消
* `chattr -a <file>` 文件只能增加数据, 其他均不能, root才能设定该权限

如何显示隐藏属性?

![](Screenshot_58.png)
![](Screenshot_59.png)

## 文件/目录的特殊权限SUID,SGID,SBIT

![](Screenshot_60.png)

上图中s, t 就是特殊权限

如何设定呢?借助chmod命令的两种使用方式

第一种方式

4 为 SUID , 二进制文件的owner上的x权限位为s
2 为 SGID , 二进制文件or目录的group上的x权限位为s
1 为 SBIT , 目录的other上的x权限位为t

* ` chmod 4755 filename` 设定file权限为 -rwsr-xr-x, 具有suid
* `chmod 6755 <file>` 具有suid和sgid
* `chmod 1755 <file>` 具有sbit

第二种方式

其中 SUID 为 u+s ，而 SGID 为 g+s ，SBIT 则是 o+t 

* ` chmod u=rwxs,go=x <file>` 设定权限为  -rws--x--x
* ` chmod g+s,o+t <file>` 加上 SGID 和 SBIT  

### Set Uid

当 s 这个标志出现在档案拥有者的 x 权限上时(如: -rwsr-xr-x), 就称为Set UID, 简称为具有SUID特殊权限(suid重点在于"u"表示暂时性的获得user的身份, 不是group/other身份)

![](Screenshot_61.png)

以上就是说: 如果一个二进制程序有SUID,任何用户在执行该程序时, 身份将暂时转变为该二进制文件的owner身份

此外, suid对于shell script, 目录 均无效

这里有个例子:
<img  src="Screenshot_62.png"/>


### Set Gid

当s符号在owner的x权限位上则表示该二进制文件具有suid, 那么, 当s符号位于group的x权限位上则表示该二进制文件具有sgid; 和suid不同, sgid可以针对二进制文件和目录设定:

![](Screenshot_64.png)
![](Screenshot_65.png)

如果一个二进制程序有SGID, 则任何一个用户执行时,将暂时获得该程序所属群组的权限;
如果一个目录有SGID, 则任何可以进入该目录(具备rx权限)的用户在进入后, 其有效群组暂时变为该目录的所属群组

### Sticky Bit

相应的, 如果t符号位于other的x位上则表示目录具有sbit, 目前只针对目录有效, 对于文件已经失效了, sbit对于目录的作用是:

![](Screenshot_63.png)

如果目录具有SBIT, 则如果一个用户可以在该目录中写入(具有wx权限),该用户在写入后,只有自己和root能够删除写入的目录or档案

## 几个场景对应的权限分配


* 用户能够进入该目录, 成为工作目录(即能够使用cd xxx)

    * 用户对该目录至少需要有`x`权限
    * 如果进一步希望能够 ls 查阅目录下的文件, 还需要`r`权限

* 进一步用户能够读取该目录下一个文件(也就是能够使用cat, more, less)

    * 用户对当前目录至少需要`x`权限
    * 用户对文件至少需要`r`权限

* 能够执行一个文件

    * 目录具有`x`
    文件具有`x`

* 进一步, 能够修改一个文件内容

    * 对当前目录具有`x`
    * 对文件具有`rw`

* 能够创建一个文件

    * 对当前目录具有`wx`

## 几个场景实例

情景是这样的

![](Screenshot_80.png)

数据准备

![](Screenshot_81.png)

1. `mkdir /srv/ahome`建立工作目录
2. `ll -d /srv/ahome` 检视刚才创建的目录, 这里的`-d`表示后面接的是要检视的目录
3. 发现需要修改目录所属群组, 权限也需要相应变化`chown .project /srv/ahome`or`chgrp project /srv/ahome`, `chmod 770 /srv/ahome`
4. 现在看貌似没问题了, 切换身份, 创建一个文件看看能否实现共同编辑.

    * 但是问题来了, 新创建的文件, 群组为创建者, 而不是希望的project, 同组人无法编辑

5. 考虑为工作目录加上 `sgid`特殊权限--`如果一个目录有SGID, 则任何可以进入该目录(具备rx权限)的用户在进入后, 其有效群组暂时变为该目录的所属群组`

    * `chmod 2770 /srv/ahome` 给工作目录加上sgid权限
  

# 文件和目录的管理

## 路径问题

* 绝对路径: 以"/"开头, 如 /root/test

* 相对路径: 不是以"/"开头, 如 ../test; 相对于当前工作目录的路径

* 特殊符号代表的目录

    * `.       `  代表此层目录(在所有的目录中都存在)
    * `..      `  代表上一层目录 (在所有的目录中都存在)
    * `-       `  代表前一个工作目录 
    * `~       `  代表『目前用户身份』所在的家目录 
    * `~account`  代表 account 这个用户的家目录(account 是个账号名称) 

## 处理目录的指令

![](Screenshot_40.png)

* `pwd`显示当前工作目录; `pwd -P`显示确切路径而非连接档路径
* `mkdir -p <dir1>/<dir2>/<dir3>` 递归创建多层目录
* `mkdir -m 771 test1` 建立dir的同时设置权限(这时不用管预设权限umask)
* `rmdir <dir>` 只能删除空目录; 
* `rmdir -p <dir1>/<dir2>/<dir3>`递归删除空目录
* `rmdir -r <dir>` 强制删除目录及其下所有数据


## 文件查看/复制/删除/移动

### 文件检视

* `ls -al <dir>` 详细列出所有文件(包括隐藏文件)
* `ls --color=never` 不带颜色显示
* `ls --full-time` 显示详细时间

### 文件复制/建立连接档(快捷方式)

* `cp <source1> <source2> <dir>` 复制多处数据到一个目录
* `cp -i <source> <target>` 如果有数据覆盖, 会需要确认; 带确认的复制
* `cp -a <source> <target>` 完全复制(属性完全一样, 包括创建时间, 是`-a`中的`-p`的作用)
    * -a 包括 -pdr
    * `cp -r <source> <target>` 递归复制目录

* `cp -u <source> <target>` 是在目标档案和源档案有差异时，才会复制; 常用于数据备份

* `scp xxx_file root@192.xxx.xxx.xx:~/yyy_file` 上传文件, 远程复制 (https://www.cnblogs.com/qiangqiangqiang/p/7677027.html)

* 快捷方式: -l 建立实体链接(hard link); -s 建立符号链接(symbolic link); 属性第二栏会变

    * `cp -l <source> <target>`建立实体链接(hard link)
    * `cp -s <source> <target>`建立符号链接(symbolic link)


### 删除文件/目录

![](Screenshot_41.png)

* `rm -i test*` 以互动模式删除所有以'test'开头的文件
* `\rm -r <xxx>` 直接删除, 不要询问(`\`表示忽略alias的指定选项)

### 移动or重命名

![](Screenshot_42.png)

* `mv <old name> <new name>` 更名 ; 和`rename` 类似
* `mv <source1> <source2> <target dir>` 移动多个数据到一个目录

### 获取路径的文件名/目录名

![](Screenshot_43.png)

## 文件内容查看

![](Screenshot_44.png)

### cat和tac

![](Screenshot_45.png)

* `cat -n <file>` 带行号显示
* `cat -A <file>` 完整显示内容(包括换行符,空格, tab)
* tac和cat 类似, 只是会从最后一行开始反向显示

### nl设计行号

![](Screenshot_46.png)
![](Screenshot_47.png)

* ` nl -b a <file>` 带行号打印(空行也有行号)
* ` nl -b a -n rz <file>` 行号前自动补0, 默认行号是6位, 如果希望改为3位 ` nl -b a -n rz -w 3 <file>`

### 翻页检视more/less

对于more: `more <file>`会进入more视图

![](Screenshot_48.png)

* 进入more视图后, `/<keyword>`搜寻, 按`n`跳到下一处

less 是 more的加强版; 进入 less视图后有更多的选项

![](Screenshot_49.png)

### "行级别"的数据过滤head/tail

* `head <file>` 默认显示前10行
* `head -n 20 <file>` 显示前20行
* `head -n -100 <file>` 不要显示后100行(只显示剩下的前xx行, 如总共110行, 则只显示前10行, 后100行都不显示)

* `tail <file>` 默认显示后10行
* `tail -n 20 <file>` 显示后20行
* `tail -n +100 <file>` 不知道file有多少行, 只想显示100行(包括第100行)以后的数据;
* `tail -f <file>`持续侦测, 直到ctrl+c终止;
    * 这个排查bug的时候很好用, 实际操作时, 怎么找到正确的日志文件? `find . -mmin -1`查看当前文件夹下1min之内变动的日志文件
    * 可能有中文乱码, 在xshell中设置即可
    <img src="Screenshot_157.png" width=40%>

### 读取非文本数据od

![](Screenshot_50.png)

## 创建文件/修改文件时间

每个文件都有3个时间属性; 
ls 默认显示的是mtime

![](Screenshot_51.png)

![](Screenshot_52.png)

* `touch <file>` 将mtime和atime时间修改为目前时间(如果不存在则创建文件)
* ` touch -d "2 days ago" <file>` 修改mtime和atime为2天前, ctime不变(ctime记录文件权限属性变化, 无法自定义)

    ![](Screenshot_53.png)

* `touch -t 0709150202 <file>`日期改为 2007/09/15 2:02 (同样是修改mtime和atime, ctime无法修改)



## 搜寻文件和指令

### which,whereis,locate

搜寻指令

![](Screenshot_66.png)

* `which ifconfig` 搜寻指令

    ![](Screenshot_67.png)


搜寻文件, 通常会先使用whereis和locate利用数据库来搜寻, 如果没有结果再使用find

![](Screenshot_69.png)
![](Screenshot_71.png)

whereis和locate区别:
![](Screenshot_70.png)

### find

最后考虑使用find:

语法比较复杂, 先看跟时间有关的

![](Screenshot_72.png)

* ` find / -mtime 0 ` 搜寻0天之前的24h(从现在开始到24h之前)的改动过的文件
* `find / -mtime 3` 是三天前的 24 小时内
* `find / -mtime -4` 4天内被更动过的文件

关于正负号的用法, 

![](Screenshot_73.png)
![](Screenshot_74.png)

再看跟使用者和group有关的语法

![](Screenshot_75.png)

* `find /home -user <someone> ` 找出任何一个用户在系统中的所有档案
* ` find / -nouser ` 搜寻系统中不属于任何人的档案

和文件权限/名称有关的语法:

![](Screenshot_76.png)![](Screenshot_77.png)

* ` find / -name passwd ` 找出档名为 passwd 这个档案
* `find /var -type s ` 找出 /var 下，文件类型为 Socket 的文件
* ` find / -perm +7000 ` ：搜寻档案中有 SGID 或 SUID 或 SBIT 等属性

    * `find / -perm +4000 -print ` 找出系统中所有具有suid的文件

* `find / -perm -7000` 表示要有 ---s--s--t 所有三个权限
* `find / -size +1000k` 找出大于1M的文件

    * `find /etc -size +50k -a -size -60k -exec ls -l {} \;` 档案大小介于50-60k之间的文件, 并列出来(`-a`表示and, 符合两个条件才行)
    * `find /etc -size +50k -a ! -user root -exec ls -ld {} \; ` 找出 /etc 底下，档案容量大亍 50K 且档案所属人员是 root 的档名，将权限完整列出 (ls -l)
    * `find /etc -size +1500k -o -size 0` 找出容量大于 1500K 以及容量等于 0 的档案(`-o`表示或)

最后有一个很有用的选项: `-exec`, 语法为:  -exec command ：command 为其他指令，-exec 后面可再接额外的指令处理搜寻到的结果

* ` find / -perm +7000 -exec ls -l {} \`将结果使用ls -l 列出(不支持别名) 

    * 解释如下:
    ![](Screenshot_78.png)
    ![](Screenshot_79.png)

## 光盘写入工具和iso文件

mkisofs：建立映像档

<img src="Screenshot_140.png" width=70%><img src="Screenshot_141.png" width=70%>

cdrecord：光盘刻录工具


  
# 硬盘和文件系统管理

fs的基本组成:inode, block, superblock

## ext2文件系统

### inode,block,superblock

* inode: 存放文件的权限, 属性(owner/group/time等); 一个文件占用一个inode, 同时记录该文件的数据所在的block号码; 带编号
* data block: 存放实际数据; 一个文件可能有多个block; 带编号
* superblock: 记录整个fs的元信息(如inode/block的总量, 使用量, 剩余以及fs支持的格式等);大小1024bytes, superblock前面还需要保留1024bytes以供开机管理程序的安装

ext2属于 索引式文件系统(indexed allocation), 数据读取方法类似于下图:

![](Screenshot_82.png)

os先找到代表文件的inode, 然后读取出存放实际数据的一个或多个block;

与之相对的还有另外一种类型的fs--闪存, 一般是FAT格式的; 没有 inode 存在，所以 FAT 没有办法将这个档案的所有 block 在一开始就读出来。每个 block 号码都记在前一个 block 当中, 读取过程类似这样:

![](Screenshot_83.png)

如果同一个档案数据写入的 block 分散的太过厉害，则磁盘头将无法在磁盘转一圈就读到所有的数据， 因此磁盘就会多转好几圈才能完整的读到这个档案的内容, fat格式的文件系统需要 磁盘碎片整理 就是由此而来

ext2文件系统的格式化会将分区区分为多个block group, 每个block group 都会有自己的独立的 inode/block/superblock 系统

### fs格式化后的结构图

ext2格式化后, 类似下图:

![](Screenshot_90.png)

* boot sector: 文件系统最前面有一个启动扇区(boot sector)，可以安装开机管理程序(boot loader)， 这是个重要的设计，因为如此一来我们就能够将不同的开机管理程序安装到不同的文件系统的最前端，而不用覆盖整颗硬盘唯一的 MBR, 这样才能够制作出多重引寻环境(多系统).

* data block (资料区块): 用来放置档案内容数据地方. 在 Ext2 文件系统中所支持的 block 大小有 1K, 2K 及 4K 三种, 而这会导致磁盘支持的最大容量和最大单文件容量的差异, 看下图:

    ![](Screenshot_84.png)

    此外block还有如下限制:

    ![](Screenshot_85.png)

    因此如果档案都非帯小，但是你的block 在格式化时却选用最大的 4K 时，可能会产生一些容量的浪费. 那么block是否选定最小的1k就行了呢? 显然不是, 如果block统一定为1k, 那么大文件会占用更多的block, 对应的inode会需要记录非常多的data block 编号, 导致读写性能下降.所以格式化时选定block大小需要考虑使用场景.

* inode table (inode 表格) : 记录档案的属性以及该档案实际数据是放置在哪几号 block 内. 具体记录哪些内容看下图

    ![](Screenshot_86.png)

    此外inode还有如下限制:

    ![](Screenshot_87.png)

    因为inode大小只有128bytes, 系统将 inode 记录 block 号码的区域定为 12 个直接，一个间接, 一个双间接和一个三间接记录区, 见下图:

    ![](Screenshot_88.png)

    这样, 1k类型的block就支持最大16G的单文件容量

* Superblock (赸级区块): 记录整个 filesystem 相关信息的地方， 没有 Superblock ，就没有这个 filesystem 了. 具体记录哪些见下图

    ![](Screenshot_89.png)

    一般整个磁盘仅有第一个block group有一个superblock, 但是有时候我们为了安全会备份这个superblock到多个group block中方便救援

* Filesystem Description (文件系统描述说明) :这个区段可以描述每个 block group 开始和结束的 block 号码，以及说明每个区段的superblock, bitmap, inodemap, data block) 分别介于哪一个 block 号码间。这部分能够用 dumpe2fs 来观察.

* block bitmap (区块对照表): 从block bitmap 当中可以知道哪些 block 是空的，因此系统就能够快速找到可使用的空间来新增档案, 同样的, 如果删除某些档案时, 需要block bitmap 需要更新哪些block恢复为空了.

* inode bitmap (inode 对照表) : 和 block bitmap 类似, 只是记录的是inode使用或者是空的情况.

### 目录/文件和inode,block关系

创建一个目录, 系统会分配一个inode(记录目录的`权限`和`属性` 和 `block号码`)和至少一个block(记录目录下`文件的名称`和`文件inode号码`); `ls -li` 查看所有文件的inode号码

建立一个文件时, 系统会分配一个inode和若干个block; 注意inode仅有12个直接指向, 可能需要专门的block记录其他block编号;

文件读取过程和inode,block, 以读取  /etc/passwd  为例:

1. `"/" 的 inode`： 透过挂载点信息找到 /dev/hdc2 的 inode 号码为 2 的根目录 inode，且 inode 记录的权限让root可以读该 block的内容(有 r 和 x) ；
2. `"/" 的 block`:根据inode获取block, 在block中找"etc/"目录的inode
3. `etc/ 的 inode`: 找到"etc/"目录的inode, 进而找到ect/ 的block
4. `passwd的inode`
5. `passwd的block`

## ext2/ext3文件存取和日志式文件系统

一般来说，我们将 inode table 和 data block 称为数据存放区域，其他例如 superblock、 block bitmap 和 inode bitmap 等区段就被称为 metadata (元信息) ，因为 superblock, inode bitmap 及 block bitmap 的数据经常变化, 比如每次新增、移除、编辑时都可能会影响到这三个部分的数据.

新增文件时, 这些数据区域是怎么运作的?

1. 确定工作目录对于当前用户的权限
2. 根据 inode bitmap 找到空闲的inode, 写入新文件的权限, 属性
3. 根据 block bitmap 找到空闲的block, 写入实际数据, 并更新步骤2中的inode中的block编号
4. 将以上步骤的inode, block信息同步到 inode bitmap, block bitmap, 并更新superblock内容; (更新metadata)

如果进行到步骤3, 这时突然关机了, metadata无法同步更新------这就是`数据的不一致状态(Inconsistent)`

再次重启会需要进行 数据一致性检查, 相当耗时, 由此 日志式文件系统(代表: ext3) 出现

![[](Screenshot_93.png)]

日志式文件系统会在磁盘中划出专门的一个区域记录准备编辑/新增的文件信息, 这样, 如果写入的过程过程中发生宕机, 再次开机可以简化一致性检查的步骤: 

![](Screenshot_91.png)

linux fs 的运作:

![](Screenshot_92.png)

## 磁盘/目录容量查看

默认单位都是 kb

![](Screenshot_94.png)![](Screenshot_95.png)![](Screenshot_96.png)

* `df -h` 打印系统所有的fs, 结果以易读的形式(容量不是默认的kb, 而是G/M)展示出来; 包括分区文件名, 挂载点, 用量等
* `df -h <dir>` 将指定目录下的磁盘容量显示出来
* `du` 列出当前目录下的所有目录(默认只列出目录, 不会列出文件)的容量, "."表示当前目录占用的总容量
* `du -a` 列出档案和文件的容量
* `du -sm /*` 检视根目录下的一级目录容量; -s表示列出匹配的一级目录(次目录),下级目录不要; -m表示容量以Mb表示

df 展示出来的表头的意义:

![](Screenshot_97.png)

此外, 如果发现某个目录占用的硬盘资源为0, 不必要奇怪, 可能这个目录挂载的不是硬盘, 而是内存, 当然不占硬盘资源了

## 实体链接和符号链接

![](Screenshot_104.png)

* `ln -s <s> <t>`符号链接: 类似于windows的快捷方式， 常用
* `ln <source> <target>` 建立实体链接: 通过文件系统的inode产生新的文件名(不是产生新文件)

先看**实体链接**

![](Screenshot_98.png)![](Screenshot_99.png)

文件系统的读取过程看下图:

![](Screenshot_100.png)

hard link优点:

1. 安全(删除一个还有一个), 
2. 不占空间(某个目录的block中多写入一个关联inode而已)

hard link缺点:

1. 无法跨filesystem
2. 不能link目录(因为会造成复杂对的大提升, 原因如下)

![](Screenshot_101.png)

**符号链接**

Symbolic link 就是在建立一个独立的文件，而这个文件会将数据的读取指向它 link 的那个文件的文件名

读取过程如图:

![](Screenshot_102.png)![](Screenshot_103.png)

这种方式最大的缺点就是, 如果删掉源文件, 那么整个环节就断了


## 磁盘管理fdisk

### 分区/删除

一个典型的分区场景:

* `df /` 主要目的是找到指定目录对应的磁盘的文件名
* ` fdisk -l ` 查看系统内所有磁盘的分区信息, 也可以看到磁盘文件名, 以及每个磁盘的分区信息. 效果类似fdisk <设备名>中的p命令, 不同的是这个命令可以查看所有磁盘的分区信息.
* `fdisk <设备文件名>` 进入分区视图( 使用『设备文件名』不要加上数字，因为partition 是针对『整个硬盘』而不是某个 partition )

![](Screenshot_106.png)![](Screenshot_105.png)

此时如果按p, 出现该磁盘的分区表, 如下图

![](Screenshot_108.png)

另外, 大于2T的硬盘分区就要借助 `parted `命令了

### 格式化mkfs

![](Screenshot_109.png)

* `mkfs -t ext3 <device name>` 格式化指定分区为ext3格式
* `mkfs 连按2下tab` 出现供选择的文件系统

如果希望指定更详细的信息, 使用 `mke2fs`

### 检验fsch,badblocks

fsch主要检视文件系统是否出错;

![](Screenshot_110.png)

* ` fsck -C -f -t ext3 /dev/hdc6` 指定格式, 指定分区进行检视(主要检视文件系统是否出错); 如果没有`-f`, 由于分区没有问题, 检视结果会非常快, 加上后, 会强制详细检查.
* `fsch 连按2下tab` 列出所有支持的文件系统

需要注意的是, 被检视的磁盘不能被挂载; 

fsch背后调用  e2fsck软件, 同样后者可以设定详细信息

badblocks主要查看磁盘是否有坏轨

![](Screenshot_111.png)

* `badblocks -sv /dev/hdc6` 检视是否有坏轨

### 挂载mount/unmount

挂载有这些问题需要注意

![](Screenshot_112.png)

语法规范是这样的

![](Screenshot_113.png)![](Screenshot_114.png)![](Screenshot_115.png)

* `mount <device> <dir>` 将指定设备挂载到指定目录
* `mount -t <文件系统类型,如ext3/iso9660等> <device> <dir> ` 指定fs type 挂载(可以指定, 也可以不指定让系统自动测试进行挂载)
* `df` 查看是否挂载上了; 也可以用 `mount -l`查看挂载情况
* ` mount -o remount,rw,auto / ` 重新挂载根目录(如果根目录出现只读情况很实用)
* ` mount --bind <source dir> <target dir>` 将source dir 暂时挂载到target dir下( 和 symbolic link功能类似)
* ` mount -o loop /root/centos5.2_x86_64.iso /mnt/centos_dvd ` loop挂载, 可以挂载ios镜像文件; 使用完后记得卸载掉

    * loop挂载有个重要的用途: 建立大文件以制作loop设备文件(可以代替分区)
    ` dd if=/dev/zero of=/home/loopdev bs=1M count=512`建立大文件
    ` mkfs -t ext3 /home/loopdev ` 格式化目标文件
    ` mount -o loop /home/loopdev /media/cdrom/ `挂载到指定目录

    ![](Screenshot_131.png)![](Screenshot_132.png)![](Screenshot_133.png)

相应的卸载语法:

![](Screenshot_116.png)

* `unmount <device/dir>` 卸载

此外, 来由一种使用`文件系统标头(label)`来挂载的方法:

![](Screenshot_117.png)

详细可查阅`e2label`, 见 [设定磁盘参数](#设定磁盘参数)

### 设定开机挂载

需要修改文件 ` /etc/fstab ` (filesystem table), 看下图:

![](Screenshot_124.png)![](Screenshot_125.png)

这个文件会在`mount`时被更新, 此外还记录了备份相关(dump), 开机自检(fsck)相关信息

具体有哪些信息呢?

1. 设备文件名 or label name
2. 挂载点
3. 磁盘分区的文件系统类型
4. 文件系统参数

    ![](Screenshot_126.png)![](Screenshot_127.png)

5. 能否被备份指令`dump`作用

    ![](Screenshot_128.png)

6. 是否以`fsch`检验扇区

    ![](Screenshot_129.png)


看一个情景: 要将 /dev/hdc6 每次开机都自动挂载到 /mnt/hdc6 ，该如何进行?

* 编辑 `/etc/fstab` 新增一行 `/dev/hdc6  /mnt/hdc6    ext3    defaults   1 2 `
* 接下来需要检测文件编写的语法是否错误: `mount -a`(依照fstab内容挂载), `df`(查看是否成功挂载), 如果看到成功挂载, 那么证明以后每次开机都会自动挂载

![](Screenshot_130.png)

### 设定磁盘参数

比如设定Label name, journal参数

`mknod`可以用来设定主/次设备编号

![](Screenshot_118.png)

什么是主/次设备编号?

![](Screenshot_119.png)![](Screenshot_120.png)

`e2label` 设定文件系统标头, 类似windows系统的自定义磁盘名称

磁盘挂载就可以使用它, 这种挂载方式相比于传统有什么优缺点?

![](Screenshot_121.png)

使用比较简单:

![](Screenshot_122.png)

* ` dumpe2fs -h <device>` 查看设备原先的label name
* `e2label <device> "label name"` 修改label name; 修改完成后可以`dumpe2fs -h <device>`查看是否修改成功

与之相关的还有一个命令: `tune2fs`---查看设备superblock内容

![](Screenshot_123.png)

* ` tune2fs -l /dev/hdc6 ` 列出设备的superblock内容

## 内存置换空间(swap)

![](Screenshot_135.png)

可以通过2种方式:

* 直接增加分区
* 通过`dd`建立一个大文件, 通过loop挂载

先看第一种

![](Screenshot_134.png)

第二种

类似第一种方式, 只是将分区操作替换为使用`dd`创建一个大文件

# 压缩和打包

先简单介绍一下压缩的原理. 这里介绍两种:

* 第一种, 我们一般都是使用bytes单位来计量文件, 实际上计算机计算的最小单位是bits. 1byte = 8bits, 现在考虑如何表示数字1---在计算机中会存储成 00000001, 可以看到有很多0, 这实际上造成了浪费. 这种压缩方式压缩的就是这里的空间
* 第二种, 会将重复的数据进行统计, 比如数据为"111...111"共100个1, 这种压缩技术会记录"100个1"而不是真的存储100个1的位

## 常见的压缩工具

*.tar, *.tar.gz, *.tgz, *.gz, *.Z, *.bz2

![](Screenshot_136.png)

* compress已经快退休了, gzip也可以解开compress的压缩档
* 常见的是gzip和bzip2, bzip2压缩比更好
* tar用来将多个文件打包为一个文件, 并不压缩文件, 一般和gzip/bzip2结合使用

## gzip和zcat

<img src="Screenshot_137.png" width=70%>

* `gzip -v <file1> <file2> <file3>` 压缩(多个)指定文件到当前文件夹下(不能指定压缩文件名), 同时显示压缩信息(注意:不是备份, 压缩后源文件就不见了)
* `gzip -d <待解文件1> <待解文件2>` 解压缩(多个)指定文件到当前目录(类似的, 解压后, 压缩文件就不见了)
* `zcat <压缩文件>` 读取文本压缩文件
* `gzip -c <file> > <指定压缩文件名>` ">"数据流重导向, 将屏幕上的数据流导入指定文件, 变相的备份, 可以指定压缩文件名

## bzip2和bzcat

<img src="Screenshot_138.png" width=70%>

* `bzip2 -z <file>` 压缩
* `bzcat <file>` 读取文本型压缩文件
* `bzip2 -d <待解文件>` 解压缩
* `bzip2 -c <file> > <指定压缩文件名>` 带备份的压缩

## 打包tar

tarfile: 单纯使用tar打包后的文件(打包时, 不带-j, -z等参数)
tarball: 打包后压缩的文件

<img src="Screenshot_139.png" width=70%>

`-j`: bzip2 ; `-z`: gzip;

`-c`:压缩　; `-x`: 解压缩; `-t`: 查看压缩; `-v` : 显示过程

`-C <dir_path>` 指定目的地, 如解压到指定目录

* `tar -jcv -f <自定义压缩文件名.tar.bz2> <需要压缩的文件/目录(可以有多个)>` 打包后, bzip2压缩; 推荐自定义压缩名: xxx.tar.bz2 (或者 xxx.tar.xz)

* `tar -jcv -f <自定义压缩文件名.tar.bz2> --exclude=<排除文件> <需要压缩的文件/目录(可以有多个)>` 排除指定目录下某几个文件, 然后压缩

* `tar -jtv -f <待查看压缩档.tar.bz2>` 查看压缩档包括哪些文件名(类似 ls)

* `tar -jxv -f <待解文件.tar.bz2>` 解压到当前目录, 使用bzip2

* `tar -jxv -f <待解文件.tar.bz2> -C <指定解压到这个目录>` 解包后, 使用bzip2解压到指定目录

* `tar -jxv -f <待解文件.tar.bz2> <指定tar包中的一个文件>` 解压压缩包中指定的一个文件, 使用bzip2; (注意: 指定的单一文件不要带"/")

    这个命令一般需要先使用`tar -jtv -f <待查看压缩档.tar.bz2> | grep '关键词'`找到希望解压的那个文件

* `tar -zpcv -f <custom_file.tar.gz>` 带权限,属性打包备份, 使用gzip备份(`-z`换为`-j`就是使用bzip2压缩)

* `-P` 表示压缩的时候为每个文件保留"/"根目录, 默认是移除根目录的, 防止解压缩时机器上的数据被覆盖; 建议不要加

* `--newer-mtime="2018/09/20"` 仅备份比某个时刻还要新的文件

* `tar -cv -f - <待打包文件> | tar -xv -f -` 复制(一般打包, 一边解开); (- 代表打包缓存文件), 其实就是简化`cp -r`命令



## dd

<img src="Screenshot_142.png" width=70%>

* `dd if=<input file> of=<output file>` 复制文件, 指定输入输出
* `dd if=/dev/hdc1 of=xxxx bs=512 count=1`备份指定磁盘的第一个扇区(一个扇区512bytes, 特别是:第一个扇区包含有MBR和partition table, 值得备份)
* `dd if=/dev/hdc1 of=xxxx` 备份整个磁盘, 连同metadata一起复制, 两个磁盘完全一模一样(这个特别方便)

## cpio

cpio 不会主动去找档案来备份, 不能指定某个特定的待备份文件, 需要配合find等命令使用, 使用"<"">"进行数据重导向

<img src="Screenshot_143.png" width=70%><img src="Screenshot_144.png" width=70%>

* ` find <dir> | cpio -ocvB > <压缩文件.cpio>` 将dir下所有文件备份到指定文件
* `cpio -idvc < <压缩文件.cpio>` 将压缩文件解开



## dump完整备份工具

* 支持差异备份(0~9的等级, 0为完整备份, 1为对0的差异备份, 2为对1的差异备份...)
* 但是对某一个指定文件夹的备份支持不足(不支持差异备份, 所有的备份文件只能在该目录下)

<img src="Screenshot_158.png" width=50%/><img src="Screenshot_159.png" width=50% >

* 备份某个磁盘; 先`df -h` 列出所有的磁盘使用量, 然后测试备份需要多少容量`dump -S <磁盘文件名>`(单位为bytes), 备份`dump -0u -f <指定生成备份文件名> <文件夹/设备名>` 完全备份, 同时更新 /etc/dumpdates

* `dump -W` 查看所有磁盘的dump记录
* `dump -0j -f /root/etc.dump.bz2 /etc` 备份某个指定目录(带压缩功能)



## restore复原

<img src="Screenshot_160.png" width=50% /><img src="Screenshot_161.png" width=50% />

* ` restore -t -f /root/boot.dump` 查看dump的内容
* ` restore -C -f /root/boot.dump ` 比对dump文件和当前实际文件系统差异
* 还原

    * 建立新的文件系统(分区)`fdisk /dev/hdc`, 然后`partprobe`, 建立好后格式化分区` mkfs -t ext3 /dev/hdc8`, 挂载` mount /dev/hdc8 /mnt`
    * 进入某个目录正式还原` restore -r -f /root/boot.dump `

* `restore -i -f /root/etc.dump ` 进入互动模式




# 帐号管理&ACL权限设定

## user management

- `whoami` current user
- `groups` current user's partners that in the same group
- `groups <user_name>` view someone's partners and their group.
- `cat /etc/passwd` view user list
- `cat /etc/group` view all group
- `useradd <option> <user_name>`
    - options
        - -d : specify the user home dir
        - -m : 如果 user home dir 不存在, 会自动创建
        - -g: specify the user group
        - -G: additional user group
        - -s: specify the shell while user login
    - demos:
        - useradd -d /usr/sam -m sam 
        - useradd -s /bin/sh -g group -G adm,root gem
- `userdel -r <user_name>` 删除用户, 同时删除家目录
- `usermod <options> <user_name>`修改账号; options 类似useradd
- `passwd <options> [username]`  
    - options:
        - -l: lock the password <=> forbidden account
        - -u: 口令解锁
        - -d: 删除口令, 使得账号无需口令即可使用
        - -f: 强制用户下次登陆修改口令
    - 如果不指定user, 就是修改当前用户
    - `passwd <username>` 修改任何用户密码

## ulimit限制用户的资源

<img src="Screenshot_173.png" width=50%/>

*   ` ulimit -a ` 列出当前身份的限制数值

    <img src="Screenshot_174.png" width=50% />

* ` ulimit -f 10240 ` 限制只能建立10M以下的文件

# 周期任务

# 磁盘阵列和LVM文件系统

# 周期任务

# 程序管理

## Job control背景前景

登入Linux, 会得到一个 bash (父程序)的 shell, 然后在bash 下执行另外的指令如 ls, passwd(子程序)

- ` tar -zpcvf /tmp/etc.tar.gz /etc > /tmp/log.txt 2>&1 & ` 在背景中备份 /etc (最后的 "&" 表示丢到背景中执行, "2>&1" 表示 错误输出写入和正确输出相同的一个文件)

- ctrl + z 暂停目前工作(并丢到背景中), 比如要暂时将 vi 给他丢到背景当中等待

- `jobs -l` 列出所有背景工作, 带 pid 号 ( + 代表最近被放到背景癿工作号码， - 代表最近最后第二个被放置到背景中癿工作号码。 而超过最后第三个以后癿工作，就丌会有 +/- 符号存在了)

- `fg [%job_number]` 从背景中恢复工作; 默认取出 "+" job; `job -` 取出 "-" job

- `bg [%job_number]` 在背景中 从 "暂停" 到 "运行", 相当于给命令 加了个 "&"

- `kill [-signal] <%job_number>` 杀掉 背景程序 (一定要加"%", 否则会被认为时 pid, 而不是 job number)

    - `kill -l` 列出所有可用的 kill 讯号码(使用的时候, 用1...15的数字也可以直接使用名称, `man 7 signal`可查阅);

    - `kill -9 %2` 强制删除 2 号 job

    - `kill -15 %2` 正常结束 2 号 job (-15 默认值)

    - `kill -1 %2` 重新读取参数的配置文件 类似 reload

- `nohup [command] [&]` 脱机(即注销)后仍可以 前景/背景 工作 (场景: 如执行的脚本需要在登出系统后仍然可以工作)

## 程序

- `ps`, `top` 观察程序

    - `ps -l` 列出自己bash下相关的程序(带 pid); `ps aux` 列出全部

    - `top -d 2` 动态观察, 2秒更新一次top; `top -d 2 -p <pid>` 查看指定程序

- `netstat -tlnp` 端口占用; ` netstat -tnlp | grep 873 ` 查看指定端口

- `fuser` 通过文件找到正在使用该文件的程序

    - `fuser -uv <file/dir>` 找到正在使用该文件/文件夹的程序

    - `fuser -ki <file>` 查看使用该文件的程序并尝试删除该程序 

- `lsof` 列出目前系统上面所有已经被开启的文件, 和 fuser 正好相反

    - `lsof -u root | grep bash` 列出用户 root 的 bash 程序所打开的文件

- `pidof <program_name> [program_name2] ...` 查找 pid  

## 安装卸载

### 原始码-tarball

tarball 是将原始码打包压缩后的文件, 使得软件便于传输. 后缀为 `*.tar.gz` or `*.tgz` or `*tar.bz2`

#### 一般的安装指令

```sh
# 首先将 tarball 解压到 /usr/local/src 下

# 创建 makefile
./configure

# clean
make clean

# 编译
make [main]

# 安装, 一般是安装在 /usr/local 下
make install

```

#### c文件编译运行

原始码: (c实现)

```c
#include <stdio.h>// 从文件 stdio.h 中读入数据, 比如 printf的定义
            // 默认位置在 /usr/include
            // 若不是在默认位置 , 编译时要制定 include 目录: gcc <target_file> [-I<include_path>]
int main(void) {
        printf("Hello world.\n");
}

```

编译

```sh
gcc hello.c # 默认输出 a.out (目标文件 Object file) - 一般不使用

gcc -c hello.c # 输出 hello.o (目标文件)  - 可能有多个
gcc -O -c <target_file> # 优化编译速度
gcc -Wall -c <target_file> #显示编译详细信息

gcc -o hello hell.o # 输出 hello (可执行文件) - 仅仅一个

# 编译时加入外部函数库, 比如在源码中使用了外部函数
gcc <target_file> -lm [-L<lib_path> [-L<lib_path_2>]]
    # -l 表示加入某个函数库 ; m 表示 libm.so 这个函数库 (lib 和 后缀 .so 省略了)
    # -L 表示 函数库路径, 可以有多个, 省略了就使用默认路径(/lib 或 /usr/lib)

```

为什么要先 制作 Object file , 进而制作成 可执行文件? - Object file 可能有多个 , 而我们的最终目标是只生成一个执行文件

#### 使用 make 简化编译

- 简化编译命令
- 增量编译 - 仅仅会重新编译修改过的源文件

makefile:

```makefile
LIBS = -lm
OBJS = hello.o sin.o
BASE_LIB_PATH = -L/lib -L/usr/lib
CFLAGS = -Wall # 环境变量; gcc 在编译时始终会使用这个参数
        # 优先级: make后跟的 > makefile文件内部指定的(也即是这里的) > shell指定的
# $@ 代表当前的 target , 即 main 
main: ${OBJS}
        gcc -o main ${OBJS} ${LIBS} ${BASE_LIB_PATH}

clean:
        rm -f main ${OBJS}
```

使用 - `make main`, `make clean main`

### 包管理软件-rpm和srmp和 yum

两大主流: 

- rmp(centos, fedora, suse) - rmp则分别有: yum(red hat系), you(suse)等等

    rmp和srpm(source rpm)区别: rmp输出的软件包不够自由, 在不同版本的系统上可能无法安装; srpm输出的软件包为源码, 但和tarball有区别, 它包含一份配置文件,包含相关软件依赖, 可修改, 之后再编译为rpm包, 比之tarball又先进, 比之rpm灵活

- dpkg(debian, ubuntu) - apt-get/apt 是在dpkg上开发而来



<img src="Snipaste_2018-04-16_12-13-58.png" width=50% />
<img src="Snipaste_2018-04-16_12-22-24.png" width=50% />

rpm和srpm两者包格式:

<img src="Snipaste_2018-04-16_12-25-31.png" width=50% /><img src="Snipaste_2018-04-16_12-27-08.png" width=50% /> 

rpm都是编译好的包, 可以直接安装, 但是如果软件安装有软件依赖问题时就无法安装了, yum很好的解决了这个问题(最终还是需要调用rpm)

#### rpm

##### rpm安装软件

安装信息写在  /var/lib/rpm/  下的数据库文件中

那么软件具体被安装在哪儿呢?

<img src="Snipaste_2018-04-16_13-24-51.png" width=50% />
<img src="Snipaste_2018-04-16_13-26-17.png" width=50% />

` rpm -ivh package_name` rpm安装(可以接多个软件包, 可以接网址)

##### rpm更新软件

<img src="Snipaste_2018-04-16_13-32-08.png" width=50% />

##### rpm查询软件

查询数据来源于 /var/lib/rpm/下的数据库文档

<img src="Snipaste_2018-04-16_13-35-02.png" width=50% />

* `rpm -q logrotate` 查看是否安装指定软件(无需版本号)
* ` rpm -ql logrotate ` 列出指定软件相关的文件和目录
* ` rpm -qi logrotate` 查看指定软件详细信息
* `rpm -qc logrotate`仅仅列出知道指定软件的配置文件
* ` rpm -qd logrotate `仅仅列出知道指定软件的说明文档
* `rpm -qR logrotate ` 查看安装指定软件需要的依赖软件
* `rpm -qf /bin/sh ` 找到指定文件的所属软件

##### rpm验证和数字签名

##### rpm软件卸载和重建数据库

<img src="Snipaste_2018-04-16_13-49-18.png" width=50% /><img src="Snipaste_2018-04-16_13-49-47.png" width=50% />

卸载后需要重建一下软件数据库  `rpm --rebuilddb`   <==重建数据库 

#### srpm

`rpmbuild`

采用默认值

<img src="Snipaste_2018-04-16_13-52-08.png" width=50% />

相关目录: 在 `usr/src`下的目录

<img src="Snipaste_2018-04-16_13-55-23.png" width=50% /><img src="Snipaste_2018-04-16_13-55-47.png" width=50% />

如果编译正常, 以上几个目录中的文件会在安装好后被删掉, 如果在编译中有错误, 会在/tmp下生成错误文档

配置文件: *.spec

<img src="Snipaste_2018-04-16_14-27-07.png" width=50% />

` rpm -i rp-pppoe-3.5-32.1.src.rpm` 仅仅解开软件包,放入某个目录/usr/src/redhat

### apt和apt-get

apt 和 apt-get 是从 dpkg 开发而来的 包管理工具, 使用它的Linux发行版有 Ubuntu, debian; apt 命令更少更易用, 彩色, 进度条, 推荐使用 apt

yum 是 rpm 上开发而来的, centos 上用;

```sh
apt install	apt-get install	安装软件包
apt remove	apt-get remove	移除软件包, 保留配置文件
apt purge	apt-get purge	移除软件包及配置文件
apt update	apt-get update	刷新存储库索引, 更新源
apt upgrade	apt-get upgrade	升级所有可升级的软件包
apt autoremove	apt-get autoremove	自动删除不需要的包
apt full-upgrade	apt-get dist-upgrade	在升级软件包时自动处理依赖关系
apt search	apt-cache search	搜索已经安装的应用程序
apt show	apt-cache show	显示软件包细节

apt list --installed：列出已经安装的包
apt list --upgradeable：列出可以升级的包
apt list --all -versions：列出系统中所有能找到的包及版本

apt edit-sources，新的apt命令，编辑源列表

apt-get clean # 删除无用的安装文件

```

### yum 使用

Yellow dog Updater, Modified

https://www.cnblogs.com/zhichaoma/p/7533247.html

```sh
更新所有的rpm包
#yum update
更新指定的rpm包,如更新kernel和kernel source
#yum update kernel kernel-source
大规模的版本升级,与yum update不同的是,连旧的淘汰的包也升级
#yum upgrade


# yum install xxx            　　安装xxx软件
# yum info xxx                　 查看xxx软件的信息
# yum remove xxx        　　　　　删除软件包
# yum list                      列出软件包
# yum clean                     清除缓冲和就的包
# yum provides xxx              以xxx为关键字搜索包（提供的信息为关键字）
# yum search xxx           　　 搜索软件包（以名字为关键字）

yum groupinstall "Development Tools"
# yum groupupdate xxx　　　　   更新xxx软件分组
# yum grouplist xxx
# yum groupremove xxx
# yum groupinfoinfo xxx
这三个都是一组为单位进行升级 列表和删除的操作。。比如 "Mysql Database"就是一个组会同时操作相关的所有软件包；

# yum update                  系统升级
# yum list available          列出所有升级源上的包；
# yum list updates            列出所有升级源上的可以更新包；
# yum list installed          列出已经安装的包；
# yun update kernel           升级内核；


```

# SELinux


# 系统服务 daemons

## daemons是什么

通常在服务的名称后会加上一个 d

分为两类:

一类可以独立启动, 一旦启动就一直占用内存, 不会自动关闭: 如WWW 的 daemon (httpd)、FTP 的 daemon (vsftpd).

一类需要一个 super daemon ( xinetd) 来统一管理启动, 有 client 请求, 则唤起, 请求结束后则关闭释放资源: 如 telnet


## daemons相关的文件脚本

- `cat /etc/services` - 查看daemon和端口号映射;  ` grep 'rsync' /etc/services`  查看指定程序的端口映射

- `/etc/init.d/*` - 启动脚本放在这里

- `/etc/sysconfig/*` - 各个服务的初始化环境配置文件

- `/etc/xinetd.conf`, `/etc/xinetd.d/*` ：super daemon 配置文件和下属 daemon配置文件

- `/var/lib/*` ：各服务产生的数据库 

- `/var/run/*` ：各服务的程序 PID 记录处 

## 怎么启动

stand alone(独立启动) 类型的 daemon启动

- 方式1: 进入 `/etc/init.d/*`, 通过这里的脚本启动

- 方式2: `service <name> <params>`

    - `service crond restart ` or `/etc/init.d/crond restart ` 重新启动 crond 这支 daemon

    - ` service --status-all ` 目前系统上面所有服务的运作状态



super daemon 类型 的启动: 修改 `/etc/xinetd.d/*` 下的配置文件



- ` grep -i 'disable' /etc/xinetd.d/* ` 查看 super daemon 管理 的 服务是否启动

- 如果需要启动 某个服务, 先修改 /etc/xinetd.d/ 底下的配置文件，然后再重新启动 xinetd 这个 super daemon 即可

```sh
#  先修改配置文件成为启动的模样 ( disable = no )
vim /etc/xinetd.d/rsync 
#  重新启动 xinetd 这个服务
/etc/init.d/xinetd restart 
```

使用 chkconfig 更方便, 不用去编辑配置文件

```sh
# 范例五：查阅 rsync 是否启动，若要将其关闭该如何处理？ 
[root@www ~] /etc/init.d/rsync status 
-bash: /etc/init.d/rsync: No such file or directory 
# rsync 是 super daemon 管理的，所以当然可以使用 stand alone 的启动方式来观察 

[root@www ~] netstat -tlup | grep rsync 
tcp  0 0 192.168.201.110:rsync  *:*     LISTEN     4618/xinetd 
tcp  0 0 www.vbird.tsai:rsync   *:*     LISTEN     4618/xinetd 

[root@www ~] chkconfig --list rsync 
rsync           on   <==预讴启动, 现在将它处理成预设不启动吧 

[root@www ~] chkconfig rsync off; chkconfig --list rsync 
rsync           off  <==看吧！关闭了喔！现在来处理一下 super daemon 的东
东！ 

[root@www ~] /etc/init.d/xinetd restart; netstat -tlup | grep rsync 
```

## 设定开机启动

登陆系统, 可以有不同的模式,  成为 "run level" , 具备 x 窗口接口的时 run level 5, 纯文本接口时 run level 3

使用 `chkconfig` 管理服务是否默认开机启动

- ` chkconfig --list |more ` - 列出目前系统上面所有被 chkconfig 管理的服务

- ` chkconfig --list | grep '3:on' ` 显示出目前在 run level 3 为启动的服务

- `chkconfig --level 345 atd on ` ：让 atd 这个服务在 run level 为 3, 4, 5 时启动; `chkconfig rsync off/on` 一般用这种简单的设置方法

也可以直接使用 systemctl: `sudo systemctl enable docker`

## service 和 systemctl 和 chkconfig 区别

service命令：可以启动、停止、重新启动系统服务，还可以显示所有系统服务的当前状态。 会去/etc/init.d目录下寻找相应的服务，进行开启和关闭等操作.   底层也是调用的 systemctl

chkconfig命令：是管理系统服务 (就是开机自启动的程序)的命令行工具, 用于维护`/etc/rc[0-6].d`目录的命令行工具。底层也是调用 systemctl

systemctl命令：是一个systemd工具。相当于 service + chkconfig

```sh

systemctl status httpd.service # 运行状态
systemctl # 列出正在运行的服务状态

systemctl list-unit-files --type=service # 列出所有 service
systemctl list-unit-files|grep enabled # 列出所有已经启动的服务

systemctl restart httpd.service
systemctl start httpd.service
systemctl stop httpd.service
# 重新加载配置
systemctl reload httpd.service

# 设置是否开机启动
systemctl enable/disable httpd
# 查看是否开机启动
systemctl is-enabled httpd.service 


# 杀死服务
systemctl kill httpd

# 系统默认启动级别
systemctl get-default  # 获取
systemctl set-default multi-user.target # 设置


# 重启、停止，挂起、休眠系统
# systemctl reboot
# systemctl halt
# systemctl suspend
# systemctl hibernate

# 启动/停止 防火墙
# 查看防火墙状态: firewall-cmd --state
systemctl start/stop firewalld.service





############### service 使用



# 启动/停止服务
service httpd start/stop
# 服务状态
service --status-all

# 开机自动启动, 允许/不允许/重置
# 默认情况下，on和off开关只对运行级2，3，4，5有效，reset可以对所有运行级有效。
chkconfig name on/off/reset

# 设置 service运行级别
# 等级0表示：表示关机
# 等级1表示：单用户模式
# 等级2表示：无网络连接的多用户命令行模式
# 等级3表示：有网络连接的多用户命令行模式
# 等级4表示：不可用
# 等级5表示：带图形界面的多用户模式
# 等级6表示：重新启动
chkconfig --level 5 httpd on

# 列出service启动信息：
chkconfig --list [name]



```

## supervisor 进程管理

http://www.ttlsa.com/linux/using-supervisor-control-program/
https://www.jianshu.com/p/0b9054b33db3

# 操作记录

## 安装c编译环境gcc

https://blog.csdn.net/AnneQiQi/article/details/51725658

如果需要提示编辑 sources.list 需要切换为 root 再修改, 必须把 deb-src 取消注释

## 安装nodejs

https://nodejs.org/zh-cn/download/

https://blog.csdn.net/ziyetian666/article/details/79737541
https://blog.csdn.net/cckavin/article/details/85088670

```sh
su root
mkdir /opt/node.js
cd /opt/node.js
wget <url>
tar -zxv xxx
cd xxx
./configure
make clean
make
make install
```


## jdk环境

https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-get-on-ubuntu-16-04

```sh

# 首先添加 Oracle ppa(源), 然后update

sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update

# jdk8
sudo apt-get install oracle-java8-installer

# 如果有多个jdk版本, 如何管理
sudo update-alternatives --config java
```

## maven环境

ref: https://www.vultr.com/docs/how-to-install-apache-maven-on-ubuntu-16-04

在之前安装了jdk的基础上

## mysql环境

[ubuntu下安装mysql及卸载mysql方法](https://www.cnblogs.com/565261641-fzh/p/6128377.html)
碰到一个问题: [MySQL 5.7 No directory, logging in with HOME=/](https://blog.csdn.net/josenhuang/article/details/53585280), 解决之后, `sudo mysql -uroot`, 空密码登陆MySQL即可

[为mysql root 设置密码, &忘记密码怎么登陆](https://www.cnblogs.com/snoopys/p/6129068.html)

```sh
service mysql stop # 停止MySQL， 也可通过kill
mysqld_safe --skip-grant-tables & # 忽略密码验证启动MySQL
mysql -u root # login to mysql
show databases; # 查看数据库
use mysql; # 使用 mysql 数据库
update user set password=password("root") where user='root'; # update root's pwd, 实际就是更新 user表


```

[一份mysql user 设置指南](http://wiki.ubuntu.org.cn/MySQL%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97)

```sh
sudo apt-get update # 更新源
sudo apt-get install mysql-server mysql-client

sudo mysql -uroot #login to mysql
GRANT ALL PRIVILEGES ON *.* TO root@localhost IDENTIFIED BY "123456";# setting pwd for root

```

## 安装 zsh

zsh 插件管理器: https://github.com/zsh-users/antigen

推荐 oh-my-zsh, 简化 zsh 配置
