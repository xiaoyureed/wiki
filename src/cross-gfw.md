---
title: Cross The GFW
date: 2017-12-30 22:30:52
tags: [gfw]
categories: tool
---

<div align="center">
<img src="Screenshot_1.png" width=30%/>
记录 FQ 相关原理, 招数, 不断更新中...
https://github.com/hoochanlon
</div>

<!--more-->

<!-- TOC -->

- [GFW封锁手段](#gfw封锁手段)
  - [DNS相关](#dns相关)
    - [DNS劫持](#dns劫持)
    - [DNS污染（DNS欺骗）](#dns污染dns欺骗)
  - [IP封锁（IP黑名单）](#ip封锁ip黑名单)
  - [敏感字过滤](#敏感字过滤)
  - [CNNIC的CA证书造成中间人攻击](#cnnic的ca证书造成中间人攻击)
- [应对手段](#应对手段)
  - [修改DNS服务器](#修改dns服务器)
  - [修改本机hosts文件](#修改本机hosts文件)
  - [HTTPS 的加密 web 代理](#https-的加密-web-代理)
  - [vpn 代理软件](#vpn-代理软件)
  - [其他方法](#其他方法)
  - [移动端](#移动端)
- [vps](#vps)
- [内网穿透](#内网穿透)

<!-- /TOC -->

# GFW封锁手段

## DNS相关

### DNS劫持

DNS 服务器上的DNS缓存里的记录被人为修改成错误的，浏览器查询到IP自然就是错误的     

最有可能干这事的呢，就是国内的ISP  

### DNS污染（DNS欺骗）

又有如下叫法，都是一个意思：  

    DNS欺骗：DNS Spoofing  
    DNS污染：域名缓存投毒（DNS Cache Poisoning）  

如果你的DNS server使用的是国外的，如谷歌提供的“8.8.8.8”，“8.8.4.4”等，每次DNS查询你都会经过国际出口，这时GFW会通过DNS污染伪造DNS查询结果，造成网站无法访问    

“域名污染”的原理大概是这样：（直接污染）    

当你的电脑向域名服务器发送了“域名查询”的请求，然后域名服务器把回应发送给你的电脑，这之间是有一个时间差的。如果某个攻击者能够在域名服务器的“DNS应答”还没有到达你的电脑之前，先伪造一个错误的“DNS应答”发给你电脑。那么你的电脑收到的就是错误的信息，并得到一个错误的 IP地址。

最有可能干这事的，就是GFW了（当然只要位于电脑和DNS server 间的所有攻击者都能DNS污染，比如ISP,骇客）  

以上是GFW实现DNS污染的两种方式之一，属于直接污染，还有一种间接污染的方法，下面说说。  

GFW的设备部署在每个国际出口，如果你用的DNS server 在国外， DNS请求和应答必定会经过国际出口，GFW在这个过程中做手脚，是为直接污染。  

如果你使用国内的DNS server，如：ISP提供的DNS server，那么恭喜你，除了会被“域名劫持”外，还会被“间接域名污染”， 整个过程是这样的：  

1. 你访问某个网站 www.xxx.yyy.com  
2. 浏览器向“ISP提供的DNS server”发出DNS 请求，“ISP提供的DNS server”缓存中没有记录，这台“ISP提供的DNS server”会去“www.xxx.yyy.com”这个网站的上一级域名（yyy.com,或者com）对应的“权威域名服务器”去进行“域名查询”，而这个“权威域名服务器”在国外，会经过国际出口，被GFW污染  
3. 最后“ISP提供的DNS server”拿到的IP地址是已经被污染了的错误的响应，而且“ISP提供的DNS server”会将这个错误的记录保存在DNS缓存中（如果有其他的DNS请求，错误的记录将被返回，造成更大范围的DNS污染）  



## IP封锁（IP黑名单）

直接屏蔽掉某些IP地址，简单粗暴。  

## 敏感字过滤

GFW会维护一个长长的敏感词列表，并且经常更新，如果网页中包含敏感词，会被屏蔽。  

## CNNIC的CA证书造成中间人攻击

CA 是“Certificate Authority”的缩写
CA 证书，顾名思义，就是CA颁发的证书。
作用：
验证网站是否可信（针对HTTPS）
验证某文件是否可信（是否被篡改）

所以CA 证书对于 https 协议的重要性（可以防止攻击者伪造虚假网站）不言而喻；
中间人攻击：各个公司的软件的根证书中包含了cnnic的ca证书后，配合GFW进行DNS域名污染

# 应对手段

## 修改DNS服务器

应对【域名劫持】，但是对域名污染无效  

Google 不愧是牛X的公司，而且很人性化——它提供了如下两个很好记的 DNS 服务器。  

    8.8.8.8
    8.8.4.4

为了防止 ＧＦW 把 Google 的 DNS 服务器封杀掉，我们还得多再备份几个。下面这些 DNS 的 IP 可就没 Google 的 DNS 那么好记了。  

OpenDNS：  

    208.67.222.222
    208.67.220.220

台湾中华电讯的DNS：  

    168.95.192.1
    168.95.192.2

香港宽频的DNS：  

    203.80.96.9
    203.80.96.10

## 修改本机hosts文件

应对域名污染----且只能用于影响力巨大的网站  

缺点：  
(1) 如果你访问的是 HTTP 协议的网页，且内容包含敏感词，那么此招数照样没戏；
(2) 如果 GFW 不光封域名，还封 IP，那么此招数也还是没戏；
(3) 如果某个网站的 IP 已经变化了，那你就要与时俱进地修改你的 hosts 文件

 Windows 系统，可以使用如下命令打开 hosts 文件：  

    notepad %SystemRoot%\system32\drivers\etc\hosts

Linux 在这个文件里： /etc/hosts

打开之后，把那些被封锁的网站的 IP 及域名加入 hosts 文件（每行一个）即可  

    74.125.227.198 drive.google.com

修改完成后, 保存, 输入命令“ipconfig /flushdns” 刷新DNS缓存

## HTTPS 的加密 web 代理

对抗IP封锁  

基于 HTTPS 方式的加密 Web 代理，主要好处是：无需安装其它软件，光靠浏览器就可以搞定。  

但是有个明显的缺点，就是用的人一多就容易引起GFW注意，自然就挂掉了，所以需要自个找到好用的代理  

## vpn 代理软件

一类是有运营商提供服务端的代理, 如 (赛风，蓝灯... ) 

一类是可以自己搭建 server 的 代理: 

shadowsocks， windows客户端在[这里](https://github.com/shadowsocks/shadowsocks-windows/releases), 可以自己租用国外vps搭建server端， 也可以使用好心人分享的搭建好的帐号：[1](https://free-ss.site/) (host添加 “104.18.18.18    free-ss.site” 访问), [2](https://freessr.win/), [3](https://shadowsocks-share.herokuapp.com/); macos 客户端: Shadowsocks-NG


- Shadowsocks-go: 二进制编译, 轻量, 快速
- Shadowsocks-python: 无功无过，也是最原始的版本，近年来更新速度略慢
- Shadowsocks-libev: 一直处于更新之中，最大的特点是支持obfs混淆
- ShadowsocksR: 从作者到产品都极负争议性, obfs混淆模式开创者, 但是前一段时间SSR服务器普遍遭到GFW的封杀.

还有一个特别的, [vpn gate](http://www.vpngate.net/cn/), 下载即用, 本质还是走代理, 只不过没有一个中心化的 server 了，每个人都是一个 server, 我为人人, 人人为我. 不过现在好像被墙了，想要上网, 需要先下载, 想下载, 需要先上网 这变成了一个鸡生蛋，蛋生鸡的问题...Orz


```sh
# 分享 vps 搭建 shadowsocks-libev server 的脚本 (https://www.24kplus.com/linux/1371.html)
# 另外可参考 https://www.flyzy2005.com/fan-qiang/shadowsocks/install-shadowsocks-in-one-command/ (ssr, 验证 bbr 是否开启成功)
# 另外可参考 https://github.com/teddysun/shadowsocks_install/tree/master
wget https://down.24kplus.com/linux/shadowsocks/centos-shadowsocks-libev.sh
chmod +x centos-shadowsocks-libev.sh && ./centos-shadowsocks-libev.sh

```

脚本内容:

```sh
#!/usr/bin/env bash
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
echo "#"
echo "# Auto install shadowsocks-libev server"
echo "#"
echo "# Copyright (C) 2019 24K PLUS"
echo "#"
echo "# System Required:  CentOS 7/8"
echo "#"
echo "# Welcome to https://www.24kplus.com"
echo "#"

red='\033[0;31m'
green='\033[0;32m'
yellow='\033[0;33m'
plain='\033[0m'

[[ $EUID -ne 0 ]] && echo -e "[${red}Error${plain}] This script must be run as root!" && exit 1

cur_dir=$( pwd )

libtool_file="libtool-2.4.6"
libtool_url="https://down.24kplus.com/linux/libtool-2.4.6.tar.gz"

shadowsocks_libev_config="/etc/shadowsocks-libev/config.json"
shadowsocks_libev_service="/etc/systemd/system/shadowsocks-libev.service"

libsodium_file="libsodium-1.0.18-stable"
libsodium_url="https://down.24kplus.com/linux/libsodium-1.0.18-stable.tar.gz"

mbedtls_file="mbedtls-2.16.3"
mbedtls_url="https://down.24kplus.com/linux/mbedtls/mbedtls-2.16.3-gpl.tgz"

localconf_file="local"
localconf_url="https://down.24kplus.com/linux/shadowsocks/local.conf"

common_ciphers=(
xchacha20-ietf-poly1305
chacha20-ietf-poly1305
chacha20
aes-256-gcm
aes-256-cfb
)

install_main(){
	check_sys
    install_prepare
    install_dependencies
	install_libtool
	install_mbedtls
	install_libsodium
	check_bbr_status
	if [ $? -eq 0 ]; then
		check_kernel_version
		if [ $? -eq 1 ]; then
			enable_bbr
		fi
	fi
	optimization_sys_config
    install_shadowsocks_libev
	install_completed_libev
    config_firewall
    install_cleanup
}

getversion(){
	grep -oE "[0-9.]+" /etc/redhat-release
}

check_sys(){
	if [ -f /etc/redhat-release ]; then
		echo ""
	else 
		echo "Only supported CentOS 7/8."
		exit 1
	fi
	local version="$(getversion)"
    sys_main_ver=${version%%.*}
	if [ "$sys_main_ver" != '7' ] && [ "$sys_main_ver" != '8' ]; then
		echo "Only supports CentOS 7/8."
		exit 1
	fi
}

check_kernel_version(){
	kernel_version=$(uname -r | awk -F'.' '{print $1}')
	kernel_main_version=$(uname -r | awk -F'.' '{print $2}')
	if [ $kernel_version -eq 4 ] && [ $kernel_main_version -lt 9 ]; then
		echo "Google BBR only supports kernel version 4.9 or higher"
		return 0
	elif [ $kernel_version -lt 4 ]; then
		echo "Google BBR only supports kernel version 4.9 or higher"
		return 0
	fi
	return 1
}

get_libev_ver(){
    libev_ver=$(wget --no-check-certificate -qO- https://api.github.com/repos/shadowsocks/shadowsocks-libev/releases/latest | grep 'tag_name' | cut -d\" -f4)
    [ -z ${libev_ver} ] && echo -e "[${red}Error${plain}] Get shadowsocks-libev latest version failed" && exit 1
}

download_shadowsocks_libev(){
    cd ${cur_dir}

    get_libev_ver
    shadowsocks_libev_file="shadowsocks-libev-$(echo ${libev_ver} | sed -e 's/^[a-zA-Z]//g')"
    local shadowsocks_libev_url="https://github.com/shadowsocks/shadowsocks-libev/releases/download/${libev_ver}/${shadowsocks_libev_file}.tar.gz"

    download "${shadowsocks_libev_file}.tar.gz" "${shadowsocks_libev_url}"
}

download(){
    local filename=$(basename $1)
    if [ -f ${1} ]; then
        echo "${filename} [found]"
    else
        echo "${filename} not found, download now..."
        wget --no-check-certificate -c -t3 -T60 -O ${1} ${2}
        if [ $? -ne 0 ]; then
            echo -e "[${red}Error${plain}] Download ${filename} failed."
            exit 1
        fi
    fi
}
install_libtool(){
    if [ ! -f /usr/bin/libtool ] && [ ! -f /usr/local/bin/libtool ]; then
        cd ${cur_dir}
        download "${libtool_file}.tar.gz" "${libtool_url}"
        tar -zxf ${libtool_file}.tar.gz
        cd ${libtool_file}
        ./configure --prefix=/usr && make && make install && ldconfig
        if [ $? -ne 0 ]; then
            echo -e "[${red}Error${plain}] ${libtool_file} install failed."
            install_cleanup
            exit 1
        fi
    else
        echo -e "[${green}Info${plain}] libtool already installed."
    fi
}
install_libsodium(){
    if [ ! -f /usr/lib/libsodium.a ]; then
        cd ${cur_dir}
        download "${libsodium_file}.tar.gz" "${libsodium_url}"
        tar -zxf ${libsodium_file}.tar.gz
        cd libsodium-stable
        ./configure --prefix=/usr && make && make install && ldconfig
        if [ $? -ne 0 ]; then
            echo -e "[${red}Error${plain}] ${libsodium_file} install failed."
            install_cleanup
            exit 1
        fi
    else
        echo -e "[${green}Info${plain}] libsodium already installed."
    fi
}

install_mbedtls(){
    if [ ! -f /usr/lib/libmbedtls.a ] && [ ! -f /usr/bin/mbedtls_hello ] && [ ! -f /usr/local/bin/mbedtls_hello ]; then
        cd ${cur_dir}
        download "${mbedtls_file}-gpl.tgz" "${mbedtls_url}"
        tar -xf ${mbedtls_file}-gpl.tgz
        cd ${mbedtls_file}
        make && make DESTDIR=/usr install && ldconfig
        if [ $? -ne 0 ]; then
            echo -e "[${red}Error${plain}] ${mbedtls_file} install failed."
            install_cleanup
            exit 1
        fi
    else
        echo -e "[${green}Info${plain}] mbedtls already installed."
    fi
}

install_shadowsocks_libev(){
	download_shadowsocks_libev
    cd ${cur_dir}
    tar -zxf ${shadowsocks_libev_file}.tar.gz
    cd ${shadowsocks_libev_file}
    ./configure --disable-documentation && make && make install
    if [ $? -eq 0 ]; then
		config_shadowsocks_libev
        systemctl start shadowsocks-libev
		systemctl enable shadowsocks-libev
    else
        echo
        echo -e "[${red}Error${plain}] shadowsocks-libev install failed."
        install_cleanup
        exit 1
    fi
}

install_completed_libev(){
    clear
    echo
    echo -e "Congratulations, shadowsocks-libev server install completed!"
    echo -e "Your Server IP        : ${red} $(get_ip) ${plain}"
    echo -e "Your Server Port      : ${red} ${shadowsocksport} ${plain}"
    echo -e "Your Password         : ${red} ${shadowsockspwd} ${plain}"
    echo -e "Your Encryption Method: ${red} ${shadowsockscipher} ${plain}"
}

install_cleanup(){
    cd ${cur_dir}
    rm -rf libsodium-stable ${libsodium_file}.tar.gz
    rm -rf ${mbedtls_file} ${mbedtls_file}-gpl.tgz
    rm -rf ${libtool_file} ${libtool_file}.tar.gz
    rm -rf ${shadowsocks_libev_file} ${shadowsocks_libev_file}.tar.gz
}

check_kernel_headers(){
    if rpm -qa | grep -q headers-$(uname -r); then
		return 0
	else
		return 1
	fi
}


config_shadowsocks_libev(){	
	if [ ! -d "$(dirname ${shadowsocks_libev_config})" ]; then
		mkdir -p $(dirname ${shadowsocks_libev_config})
	fi

	cat > ${shadowsocks_libev_config}<<-EOF
	{
		"server":"0.0.0.0",
		"server_port":${shadowsocksport},
		"password":"${shadowsockspwd}",
		"timeout":600,
		"method":"${shadowsockscipher}",
		"fast_open":true,
		"mode":"tcp_only"
	}
	EOF
	
	cat > ${shadowsocks_libev_service}<<-EOF
	[Unit]
	Description=shadowsocks-libev server
	After=network.target

	[Service]
	Type=simple
	ExecStartPre=/bin/sh -c 'ulimit -n 51200'
	ExecStart=/usr/local/bin/ss-server -c /etc/shadowsocks-libev/config.json
	Restart=on-abort

	[Install]
	WantedBy=multi-user.target
	EOF
}


error_detect_depends(){
    local command=$1
    local depend=`echo "${command}" | awk '{print $4}'`
    echo -e "[${green}Info${plain}] Starting to install package ${depend}"
    ${command} > /dev/null 2>&1
    if [ $? -ne 0 ]; then
        echo -e "[${red}Error${plain}] Failed to install ${red}${depend}${plain}"
        exit 1
    fi
}

install_dependencies(){
	local gcc_path=$(command -v gcc)
    if [ "${gcc_path}" == "" ]; then
		error_detect_depends "yum -y install gcc"
	fi
    yum_depends=(
		gettext autoconf automake make pcre-devel asciidoc xmlto c-ares-devel libev-devel wget tar m4 perl
	)
	for depend in ${yum_depends[@]}; do
		error_detect_depends "yum -y install ${depend}"
	done
	
	if [ "$sys_main_ver" == '8' ]; then
		install_python2
	fi
}

install_python2(){
	if [ ! -f /usr/lib64/libpython2.7.so.1.0 ]; then
		error_detect_depends "yum -y install python2"
	fi
}

config_firewall(){
    systemctl status firewalld > /dev/null 2>&1
	if [ $? -eq 0 ]; then
		default_zone=$(firewall-cmd --get-default-zone)
		firewall-cmd --permanent --zone=${default_zone} --add-port=${shadowsocksport}/tcp
		firewall-cmd --permanent --zone=${default_zone} --add-port=${shadowsocksport}/udp
		firewall-cmd --reload
	else
		echo -e "[${yellow}Warning${plain}] firewalld looks like not running or not installed, please enable port ${shadowsocksport} manually if necessary."
	fi
}

install_prepare_port() {
    while true
    do
    dport=$(shuf -i 9000-19999 -n 1)
    echo -e "Please enter a port for shadowsocks-libev [1-65535]"
    read -p "(Default port: ${dport}):" shadowsocksport
    [ -z "${shadowsocksport}" ] && shadowsocksport=${dport}
    expr ${shadowsocksport} + 1 &>/dev/null
    if [ $? -eq 0 ]; then
        if [ ${shadowsocksport} -ge 1 ] && [ ${shadowsocksport} -le 65535 ] && [ ${shadowsocksport:0:1} != 0 ]; then
            echo
            echo "port = ${shadowsocksport}"
            echo
            break
        fi
    fi
    echo -e "[${red}Error${plain}] Please enter a correct number [1-65535]"
    done
}

install_prepare_password(){
	local mkpasswd_path=$(command -v mkpasswd)
    if [ "${mkpasswd_path}" == "" ]; then
		error_detect_depends "yum -y install expect"
	fi
	radompassword=$(mkpasswd -l 18 -d 2 -s 4)

    echo "Please enter password for shadowsocks-libev"
    read -p "(Default password: ${radompassword}):" shadowsockspwd
    [ -z "${shadowsockspwd}" ] && shadowsockspwd=${radompassword}
    echo
    echo "password = ${shadowsockspwd}"
    echo
}

install_prepare_cipher(){
    while true
    do
    echo -e "Please select stream cipher for shadowsocks-libev:"

	for ((i=1;i<=${#common_ciphers[@]};i++ )); do
		hint="${common_ciphers[$i-1]}"
		echo -e "${green}${i}${plain}) ${hint}"
	done
	read -p "Which cipher you'd select(Default: ${common_ciphers[0]}):" pick
	[ -z "$pick" ] && pick=1
	expr ${pick} + 1 &>/dev/null
	if [ $? -ne 0 ]; then
		echo -e "[${red}Error${plain}] Please enter a number"
		continue
	fi
	if [[ "$pick" -lt 1 || "$pick" -gt ${#common_ciphers[@]} ]]; then
		echo -e "[${red}Error${plain}] Please enter a number between 1 and ${#common_ciphers[@]}"
		continue
	fi
	shadowsockscipher=${common_ciphers[$pick-1]}
    
   

    echo
    echo "cipher = ${shadowsockscipher}"
    echo
    break
    done
}

check_bbr_status() {
    local param=$(lsmod | grep bbr | awk '{print $1}')
    if [[ "${param}" == "tcp_bbr" ]]; then
		echo "[${green}Info${plain}] Your Google BBR enabled"
        return 1
    else
        return 0
    fi
}

enable_bbr() {
	modprobe tcp_bbr
    echo "tcp_bbr" >> /etc/modules-load.d/modules.conf
    echo "net.core.default_qdisc = fq" >> /etc/sysctl.conf
    echo "net.ipv4.tcp_congestion_control = bbr" >> /etc/sysctl.conf
    sysctl -p
	echo "[${green}Info${plain}] Google BBR enabled"
}

optimization_sys_config(){
	cd ${cur_dir}
	if [ ! -f /etc/sysctl.d/local.conf ]; then
		download "${localconf_file}.conf" "${localconf_url}"
		mv ${localconf_file}.conf /etc/sysctl.d/local.conf
		sysctl --system	
	fi
	if [ "$(grep "* soft nofile" /etc/security/limits.conf)" == "" ]; then
		echo "* soft nofile 51200" >> /etc/security/limits.conf
	fi
	if [ "$(grep "* hard nofile" /etc/security/limits.conf)" == "" ]; then
		echo "* hard nofile 51200" >> /etc/security/limits.conf
	fi
}

get_ip(){
    local IP=$( ip addr | egrep -o '[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}' | egrep -v "^192\.168|^172\.1[6-9]\.|^172\.2[0-9]\.|^172\.3[0-2]\.|^10\.|^127\.|^255\.|^0\." | head -n 1 )
    [ -z ${IP} ] && IP=$( wget -qO- -t1 -T2 api.24kplus.com/myip )
    echo ${IP}
}

get_char(){
    SAVEDSTTY=$(stty -g)
    stty -echo
    stty cbreak
    dd if=/dev/tty bs=1 count=1 2> /dev/null
    stty -raw
    stty echo
    stty $SAVEDSTTY
}

install_prepare(){

    install_prepare_password
    install_prepare_port
    install_prepare_cipher

    echo
    echo "Press any key to start...or Press Ctrl+C to cancel"
    char=`get_char`

}

install_main
```


## 其他方法

内网穿透：比较典型的是ZeroTier，

V2Ray：V2Ray 是 Project V 下的一个工具。Project V 是一个包含一系列构建特定网络环境工具的项目，而 V2Ray 属于最核心的一个

## 移动端

ios: shadowrocket, Surge

Android:  shadowsocks , Postern, 


# vps 

国内: 阿里云, 腾讯云    , 华为云/京东云/七牛云/ucloud

香港: GigsGigsCloud,枫叶vps.        Raksmart/HostKVM/搬瓦工/阿里云香港

国外: Vultr, 搬瓦工         , HostDare/CloudCone/VirMach/Raksmart/BudgetVM/Linode/DigitalOcean/Godaddy

域名注册: Namesilo, Godaddy(老牌/中文)      Name.com/NameCheap 



宝塔面板
百度云加速：CDN/安全加速
ThemeForest 全球最大网站主题库


SeaFile、ownCloud、BTSync 云网盘

# 内网穿透

FRP  https://github.com/fatedier/frp

https://github.com/ehang-io/nps nps

https://blog.k8s.li/ffmepg-rtsp.html
https://www.cnblogs.com/liusingbon/p/12623774.html
https://www.zhihu.com/question/20671869 摄像头视频流

https://segmentfault.com/a/1190000022390255 常用的内网穿透工具


reference: [1](https://zhuanlan.zhihu.com/p/30351943)
https://www.youtube.com/watch?v=VKSKZ0-3oT8
https://www.youtube.com/watch?v=LUg8cEBjYYM (web css 设计)
GitHub: frp

https://imququ.com/post/self-hosted-ngrokd.html
https://ngrok.com
https://www.skysilk.com/cloud/pricing/#classId=Basic&section=1
https://www.vultr.com/


