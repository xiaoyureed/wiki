---
title: Tomcat Jetty Nginx
tags:
  - tomcat
  - jetty
  - nginx
categories: java web
date: 2016-04-28 15:31:40
---


<div align="center">
比较Tomcat, jetty, Nginx; 笔记
references: [1](https://www.zhihu.com/question/32212996), [2](https://blog.csdn.net/zhongyanpingzzz/article/details/50353111),

</div>

<!--more-->

<!-- TOC -->

- [比较](#比较)
  - [Tomcat和jetty](#tomcat和jetty)
  - [Apache http server和Nginx](#apache-http-server和nginx)
- [nginx](#nginx)
  - [使用场景](#使用场景)
  - [常用命令](#常用命令)
  - [nginx.conf 配置文件](#nginxconf-配置文件)
    - [最简配置](#最简配置)
    - [反向代理配置](#反向代理配置)
    - [负载均衡配置](#负载均衡配置)
    - [gzip 压缩](#gzip-压缩)
    - [websocket 配置](#websocket-配置)
    - [搭建谷歌镜像站](#搭建谷歌镜像站)
    - [上 https](#上-https)
    - [组成元素](#组成元素)
  - [搭配 docker 使用](#搭配-docker-使用)
  - [负载均衡配置demo](#负载均衡配置demo)
  - [nginx和lua脚本](#nginx和lua脚本)
  - [tengine](#tengine)
  - [nginx 模块开发](#nginx-模块开发)
  - [nginx面试题](#nginx面试题)
    - [nginx是如何实现高并发的](#nginx是如何实现高并发的)
    - [nginx 和 apache 区别](#nginx-和-apache-区别)
    - [fastcgi 与 cgi 的区别](#fastcgi-与-cgi-的区别)
- [jetty](#jetty)
  - [部署 app 到 jetty 内部](#部署-app-到-jetty-内部)
  - [jetty支持servlet3.0注解](#jetty支持servlet30注解)
  - [jetty的整个结构](#jetty的整个结构)
  - [Jetty的工作过程](#jetty的工作过程)
    - [启动](#启动)
    - [接受请求](#接受请求)
    - [处理请求](#处理请求)
- [Tomcat](#tomcat)
  - [Tomcat 总体结构](#tomcat-总体结构)
    - [Connector](#connector)
    - [Container](#container)
  - [用到了哪些设计模式](#用到了哪些设计模式)
    - [Facade](#facade)
    - [Observer](#observer)
    - [Command](#command)
    - [Chain of responsibility](#chain-of-responsibility)
  - [Tomcat 优化](#tomcat-优化)

<!-- /TOC -->

# 比较

## Tomcat和jetty

* jetty和Tomcat类似, 均为servlet容器 - 他们都支持标准的servlet规范和JavaEE的规范

* jetty比Tomcat轻量级

* Jetty比tomcat更容易扩展

    * jetty的架构是基于Handler来实现的，主要的扩展功能都可以用Handler来实现，扩展简单. Tomcat的架构是基于容器设计的，进行扩展是需要了解Tomcat的整体设计结构，不易扩展。

    * jetty是使用java编写的，他的api是一组以jar包的形式发布，开发人员可以将jetty容器实例化成一个对象，可以迅速为一些独立（stand-alone）的java应用提供网络和web链接

* 主攻方向不同:

    * jetty可以利用 Continuation 机制来处理大量的用户请求以及时间比较长的连接, 适合于web聊天应用等等,像淘宝的 web 旺旺就是用 Jetty 作为 Servlet 引擎; 
    
    * Tomcat适合处理少数非常繁忙的链接，也就是说链接生命周期短的话，Tomcat的总体性能更高

* Tomcat默认采用BIO处理I/O请求，在处理静态资源时，性能较差。Jetty默认采用NIO在处理I/O请求上更占优势，在处理静态资源时，性能较高

* Jetty更满足公有云的分布式环境的需求 ,比如 Google app engine ，而Tomcat更符合企业级环境

## Apache http server和Nginx

* Nginx类似于Apache http server, 不像Tomcat可以支持生成动态页面，但它们可以通过其他模块来支持（例如通过Shell、PHP、Python脚本程序来动态生成内容）

* Tomcat通常和Nginx配合使用

    * 动静态资源分离——运用Nginx的反向代理功能分发请求：所有动态资源的请求交给Tomcat，而静态资源的请求（例如图片、视频、CSS、JavaScript文件等）则直接由Nginx返回到浏览器，这样能大大减轻Tomcat的压力。

    * 负载均衡，当业务压力增大时，可能一个Tomcat的实例不足以处理，那么这时可以启动多个Tomcat实例进行水平扩展，而Nginx的负载均衡功能可以把请求通过算法分发到各个不同的实例进行处理

* Nginx相对于Apache的优点: 轻量级; 抗并发(nginx 处理请求是异步非阻塞的，而apache 则是阻塞型的); 高度模块化的设计，编写模块相对简单; 提供负载均衡;

* Apache优点: apache的 rewrite 比nginx 的强大; 性能稳定，而nginx相对bug较多; Apache 对 PHP 支持比较简单，Nginx 需要配合其他后端用

    * apache rewrite -> http://httpd.apache.org/docs/current/rewrite/, 主要的功能就是实现URL的跳转


# nginx

https://github.com/jaywcjlove/nginx-tutorial

https://docshome.gitbook.io/nginx-docs/ 中文文档

https://github.com/dunwu/nginx-tutorial 入门

## 使用场景

- 静态web服务器, 

- 负载均衡, 

- 反向代理

    (以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。)图示:

- http 缓存

## 常用命令

```sh

# -s 即signal, 向 nginx 发送 start|reload|stop 命令的意思 

nginx -s start;     # 启动
# or
./sbin/nginx

nginx -s stop       # 快速关闭Nginx，可能不保存相关信息，并迅速终止web服务。

nginx -s quit       # 平稳关闭Nginx，保存相关信息，有安排的结束web服务。

nginx -s reload     # 因改变了Nginx相关配置，需要重新加载配置而重载。

nginx -s reopen     # 重新打开日志文件。

nginx -c filename   # 为 Nginx 指定一个配置文件，来代替缺省的。

nginx -t            # 检查配置文件的语法的正确性。
                    # 不运行，

nginx -v            # 显示 nginx 的版本。

```


## nginx.conf 配置文件

### 最简配置

```conf
server {
  listen       80;
  server_name  localhost;

  location / {
    root   /usr/share/nginx/html;
    index  index.html index.htm;
    try_files $uri /index.html;
  }
}

```

### 反向代理配置

nginx的反向代理是依赖于ngx_http_proxy_module这个module来实现的

```conf
# 场景: 我要把这个https协议的图片请求反向代理到http协议的真实图片上。https协议 的这张图片是不存在，而它有一个地址实际指向的内容是http协议中的图片

# https
server {
  server_name www.example.com;
  listen       443;
  location /newchart/hollow/small/nsh000001.gif {
    proxy_pass http://image.sinajs.cn/newchart/hollow/small/nsh000001.gif;
  }
  location /newchart/hollow/small/nsz399001.gif {
    proxy_pass http://image.sinajs.cn/newchart/hollow/small/nsz399001.gif;
  }





# 再看一个例子
upstream rails365 {
    #指定了一个服务器，这个服务器和nginx 是同一台机器的，用的是unix socket来连接，连接的是一个unicorn进程 (进程内跑的是 ruby on rails 代码)
    server unix:///home/yinsigan/rails365/shared/tmp/sockets/unicorn.sock fail_timeout=0;
    ; 若希望负载均衡, 部署多一个unicorn进程，监听在另外的unix socket上，就等于多了一台服务器
    #server unix:///home/yinsigan/rails365_cap/shared/tmp/sockets/unicorn.sock;
}
server {
        listen 80 default_server;
        listen [::]:80 default_server ipv6only=on;
        server_name www.rails365.net;
        root         /home/yinsigan/rails365/current/public;
        #这里表示若访问 www.rails365.net, 先找 root目录 下的 index.html
        #若访问的是 www.rails365.net/about.html或者其他的非 index 页面 , 找 root 下的 about.html或者其他对应的非 index 页面, 若没找到, 会执行 @rails365 即 location @rails365 {...}
        try_files $uri/index.html $uri @rails365; 
        location @rails365 {
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $http_host;
            proxy_redirect off;
            #反向代理(转发)到 upstream rails365 指定的内容
            proxy_pass http://rails365;
        } }

```

### 负载均衡配置

在版本1.9之前，它只能作为http 的负载均衡，也就是在网络模型的第七层发挥作用，1.9之后，它可以对tcp进行负 载均衡，比如redis，mysql等

```conf
# load balance config 负载均衡
# 位于 server 下
; 就是在 反向代理基础上, 在 upstream 下多加几个 server节点

# upstream的负载均衡配置
upstream aaa {
    #默认情况下，如果不指定方式，就是随机轮循(round-robin)
    #还有中方式 least_conn;, (优先发送给那些接受请求少的节点)
    least_conn;

    #ip_hash 可以记录请求来源的ip，如果是同一个ip，下次访问的时候还是会到相同 的主机，这个可以略微解决那种带cookie，session的请求的一致性问题
    ip_hash;

    #hash方式, 控制粒度更小, 可根据任意变量来控制
    hash $request_uri consistent;   # 通过请求地址($request_uri)来控制, 相同的请求地址路由到相同的节点

    #weight是权重，可以根据机器配置定义权重。weigth参数表示权值，权值越高被分配到的几率越大。default to 1
    server 192.168.80.121:80 weight=3;
    server 192.168.80.122:80 weight=2;
    server 192.168.80.123:80 weight=3;

    #down 暂时移出某个节点
    server xxx.xxx.xxx.xx down;

    #backup 备用节点, 相对于备份的机器来说，其他的机器就相当于主要服务器，只要当主要服务器不可用的时候，才会用到备用服务器
    server xxx.xxx.xxx.xx backup;

    #max_fails和fail_timeout
    #默认情况下，max_fails的值为1，表示的是请求失败的次数，请求1次失败就换到下台主机
    #fail_timeout，表示的是请求失败的超时时间，在设定的时间内 没有成功，那作为失败处理
    server xxxxx max_fails=2;


}
location / {
    proxy_pass http://aaa;
    proxy_set_header Host $host; # nginx 转发时会丢失 Host header, 这里加上
}
```

### gzip 压缩

需要nginx已经有编译过 ngx_http_gzip_module 这个模块, 能对需要的静态文件压缩大小，比如图片，css，javascript，html等, 压缩是 需要消耗CPU，但能提高传缩的速度，因为传缩量少了许多，从而节省带宽;

浏览器也要支 持解压功能，只要浏览器的请求头带上 Accept-Encoding: gzip 即可

别对二进制文件，比如图片做gz压缩，因为没有任何意义

https://blog.csdn.net/qq_36431213/article/details/78221189

```conf
; http 全局开启 gzip 压缩
http {
    ; 必须
    gzip on;
    gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;

; 可选
    gzip_disable "msie6";
    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_buffers 16 8k;
    gzip_http_version 1.1;
}
server {
    ; 在需要压缩的资源这里配置三行
    #assets 目录下有很多静态资源，比如js，css等
    location ~ ^/assets/ {
        gzip_static on;
        expires max;
        add_header Cache-Control public;
    } 
}


```

### websocket 配置

```conf
; 可以在 浏览器 console 中测试 new WebSocket('ws://www.example.com/wx');

upstream ws {
  server unix:///home/eason/tt_deploy/shared/tmp/sockets/puma.sock fail_timeout=0;
}
server {
  location /ws/ {
    proxy_pass http://ws;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
} }

```

### 搭建谷歌镜像站

ngx_http_google_filter_module 是一个nginx的插件，用 的原理是nginx的反向代理, (依赖于 ngx_http_substitutions_filter_module 这个库)

要有一台能访问google.com的vps或云主机

```conf
server {
  # ... part of server configuration
  resolver 8.8.8.8;
  location / {
    google on; 
# ... 
  }
}

```


### 上 https

https://ruby-china.org/topics/31983



### 组成元素

分号结尾

分为几个部分:

- 全局块: configuring cmds that affect the global, eg: user group, worker number, log file path, pid path...
- events: config the network connections, such as worker max connection num, even driven model...
- http module: 日志 , 代理 ,缓存...超时..., 
    - http global module, 如 upstream, 超时, error page
    - include /etc/nginx/conf.d/*.conf 包含其他配置文件 (即 http 块儿下的配置可以放置到其他配置文件)
        所以 nginx.conf 基本不用动, 添加新的路由配置, 只需要到 /etc/nginx/conf.d/*.conf 新建配置文件重启 nginx即可
    - server module 1 配置虚拟主机
        - location 1 配置 route
        - location 2
    - server module 2;
- mail

```conf

# 定义Nginx运行的用户和用户组
user www www;

#工作进程个数 建议设置为等于CPU总核心数
worker_processes  1;

#全局错误日志定义类型，[ debug | info | notice | warn | error | crit ]
#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  /usr/local/nginx/logs/error.log info;

#进程pid文件
#pid        logs/nginx.pid;


# 若为空, 需要保留这一个段落
events {
    # 单个工作进程最大并发连接数, 默认 1024, 也不能设置太大，不能让你的CPU跑满100%
    worker_connections  1024;

    # 使用什么事件模型， use [ kqueue | rtsig | epoll | /dev/poll | select | poll ]
    #epoll模型是Linux 2.6以上版本内核中的高性能网络I/O模型，如果跑在FreeBSD上面，就用kqueue模型。
    #use epoll;

    #连接超时时间。
    #keepalive_timeout 60;

    #
    #client_header_buffer_size 4k;


    # 事件驱动模型
}


http {
    #文件扩展名与文件类型映射表 (支持的 mine 类型)
    include       /etc/nginx/mime.types;
    #默认文件类型
    default_type  application/octet-stream;
    #默认编码
    #charset utf-8; 

    # server_names_hash_bucket_size 128; #服务器名字的hash表大小
    # client_header_buffer_size 32k; #上传文件大小限制

    # 日志格式
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #日志路径
    #access_log  logs/access.log  main;

    #开启高效文件传输模式，
    #sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用设为 on，如果用来进行下载等 磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。
    #注意：如果图片显示不正常把这个改成off
    sendfile        on;

    #autoindex on; #开启目录列表访问，合适下载服务器，默认关闭。

    ##防止网络阻塞
    #tcp_nopush     on;
    tcp_nodelay on; #防止网络阻塞

    keepalive_timeout 120; #长连接超时时间，单位是秒

    #FastCGI相关参数是为了改善网站的性能：减少资源占用，提高访问速度。
    fastcgi_connect_timeout 300;
    fastcgi_send_timeout 300;
    fastcgi_read_timeout 300;
    fastcgi_buffer_size 64k;
    fastcgi_buffers 4 64k;
    fastcgi_busy_buffers_size 128k;
    fastcgi_temp_file_write_size 128k;

    #gzip模块设置
    gzip on; #开启gzip压缩输出

    # 不是必须
    gzip_min_length 1k; #最小压缩文件大小
    gzip_buffers 4 16k; #压缩缓冲区
    gzip_http_version 1.0; #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
    gzip_comp_level 2; #压缩等级
    #压缩类型，默认就已经包含text/html，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
    gzip_types text/plain application/x-javascript text/css application/xml;
    gzip_vary on;

    #limit_zone crawler $binary_remote_addr 10m; #开启限制IP连接数的时候需要使用

    

    # 虚拟主机的配置
    # 可以放到其他配置文件, 通过 http.include 引入, 如 (常用) -> include /etc/nginx/conf.d/*.conf , 每个 .conf 是一个 server
    server {
        # 监听端口
        listen       8080;

        # #域名, 当前 server 为这个域名服务
        # nginx会 比较 请求头里的 Host 请头, 和这里比较
        #可以有多个，用空格隔开
        server_name  localhost aa.cn;

        #index index.html index.htm index.php;  # 设置访问主页 (也可以在 location.index 设置)
        #root /home/wwwroot/aa.cn/web$subdomain;# 访问域名跟目录  

     # 绑定目录为二级域名 bbb.aa.com  根目录 /bbb  文件夹
        set $subdomain ''; # 定义变量
        if ( $host ~* "(?:(\w+\.){0,})(\b(?!www\b)\w+)\.\b(?!(com|org|gov|net|cn)\b)\w+\.[a-zA-Z]+" ) { 
            set $subdomain "/$2"; 
        }
        

        include rewrite/dedecms.conf; #rewrite end   #载入其他配置文件

        #charset koi8-r;

        #access_log  logs/host.access.log  main;



        location / { # 为 / (根路径) 配置 路径映射, 若为 /articles, 则是为 /articles 配置路径映射
            root   html; # 项目根目录为 html (即 /usr/share/nginx/html)
            index  index.html index.htm; # 欢迎页
            proxy_pass http://baidu.com ;# 代理给百度
            proxy_set_header Host $host; # nginx 转发给 gateway, 会丢失 Host , 这里给补上
        }

        

        #图片缓存时间设置
        location ~ .*.(gif|jpg|jpeg|png|bmp|swf)$ {
        　　expires 10d;
        }

        #JS和CSS缓存时间设置
        location ~ .*.(js|css)?$ {
        　　expires 1h;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }




        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }




    #动态路由配置
    #当用户访问 http://example.org 时就会去读取 /data/www/index.html, 若没有, 则 index.php, 就会就会转发到 "location ~ \.php$ {...}" 里面的逻辑
    server {

        listen 80;
        server_name xxx.org www.xxx.org;
        root   /data/www;
        
        location / {
            index index.html index.php;
        }

        location ~* \.(gif|jpg|png)$ {
            expires 30d;
        }

        location ~ \.php$ {
            fastcgi_pass  localhost:9000;
            fastcgi_param SCRIPT_FILENAME
                $document_root$fastcgi_script_name;
            include       fastcgi_params;
        }




    #对 "/" 启用反向代理
    location / {
        proxy_pass http://127.0.0.1:88;
        proxy_redirect off;
        proxy_set_header X-Real-IP $remote_addr;
        #后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        #以下是一些反向代理的配置，可选。
        proxy_set_header Host $host;
        client_max_body_size 10m; #允许客户端请求的最大单文件字节数
        client_body_buffer_size 128k; #缓冲区代理缓冲用户端请求的最大字节数，
        proxy_connect_timeout 90; #nginx跟后端服务器连接超时时间(代理连接超时)
        proxy_send_timeout 90; #后端服务器数据回传时间(代理发送超时)
        proxy_read_timeout 90; #连接成功后，后端服务器响应时间(代理接收超时)
        proxy_buffer_size 4k; #设置代理服务器（nginx）保存用户头信息的缓冲区大小
        proxy_buffers 4 32k; #proxy_buffers缓冲区，网页平均在32k以下的设置
        proxy_busy_buffers_size 64k; #高负荷下缓冲大小（proxy_buffers*2）
        proxy_temp_file_write_size 64k;
        #设定缓存文件夹大小，大于这个值，将从upstream服务器传
    }
    
    #设定查看Nginx状态的地址
    location /NginxStatus {
        stub_status on;
        access_log on;
        auth_basic "NginxStatus";
        auth_basic_user_file conf/htpasswd;
        #htpasswd文件的内容可以用apache提供的htpasswd工具来产生。
    }

    #本地动静分离反向代理配置
    #所有jsp的页面均交由tomcat或resin处理
    location ~ .(jsp|jspx|do)?$ {

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://127.0.0.1:8080;

    }
    #所有静态文件由nginx直接读取不经过tomcat或resin
    location ~ .*.(htm|html|gif|jpg|jpeg|png|bmp|swf|ioc|rar|zip|txt|flv|mid|doc|ppt|pdf|xls|mp3|wma)$ {　　 
        expires 15d;　　 
    }
    location ~ .*.(js|css)?$ { 　　
        expires 1h;　　
    }





    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}

mail {
    
}
```

## 搭配 docker 使用

http://www.ruanyifeng.com/blog/2018/02/nginx-docker.html

```sh
$ docker container run \
  -p 127.0.0.2:8080:80 \    # 容器的80端口映射到127.0.0.2:8080
  --rm \                    # 容器停止运行后，自动删除容器文件(不是image), 如果是后台运行(-d), 则无效
  --name mynginx \
  nginx                     # 本地没有这个image, 会自动从远程 pull 最新版

# $PWD表示当前目录, 接string需要"", 
$ docker container run \
  -p 127.0.0.2:8080:80 \
  --rm \
  --name mynginx \
  --volume $PWD/html:/usr/share/nginx/html \
  nginx

$ ducker run -p 8080:80 --rm --name mynginx --privileged=true -v "$PWD/html":/usr/share/nginx/html -v "$PWD/conf":/etc/nginx nginx
```

## 负载均衡配置demo

```sh
http {
    #设定mime类型,类型由mime.type文件定义
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    #设定日志格式
    access_log    /var/log/nginx/access.log;

    #设定负载均衡的服务器列表, 然后在location中对应。
    upstream load_balance_server {
        #weigth参数表示权值，权值越高被分配到的几率越大
        server 127.0.0.1        weight=2;
        server 127.0.0.1:8081   weight=1;
    }

   #HTTP服务器
   server {
        #监听80端口
        listen       8090;
        
        #以什么IP进行访问
        server_name  localhost;

        #对所有请求进行负载均衡请求
        location /blog {
            #请求转向load_balance_server 定义的服务器列表
            proxy_pass  http://load_balance_server ;
        }
    }
}

```

即上面配置中的upstream可选的四种负载均衡算法([ref](https://zhuanlan.zhihu.com/p/25102281)))

- 服务器轮询（默认方式）
- ip_hash
- fair
- url_hash

## nginx和lua脚本

http://wiki.jikexueyuan.com/project/nginx-lua/introduction.html
 https://blog.csdn.net/cgj296645438/article/details/88560461 灰度发布
 https://www.cnblogs.com/wangxusummer/p/4309007.html ngx_lua 模块
 https://www.cnblogs.com/xd502djj/p/6097773.html, https://www.cnblogs.com/digdeep/p/4859575.html Openrestry入门

## tengine

1、tengine是在nginx上面开发的，包含了nginx的性能。

2、tengine更适合大访问量网站的需求，相比nginx更加的稳定，性能更加的强劲。

## nginx 模块开发

http://blog.codinglabs.org/articles/intro-of-nginx-module-development.html


## nginx面试题

https://www.cnblogs.com/mmdln/p/8952261.html
https://blog.csdn.net/xal0610/article/details/79531692

TODO

### nginx是如何实现高并发的

一个主进程，多个工作进程，每个工作进程可以处理多个请求

事件驱动：每进来一个request，会有一个worker进程去处理。但不是全程的处理，处理到可能发生阻塞的地方，比如向上游（后端）服务器转发request，并等待请求返回。那么，这个处理的worker继续处理其他请求，而一旦上游服务器返回了，就会触发这个事件，worker才会来接手，这个request才会接着往下走。

由于web server的工作性质决定了每个request的大部份生命都是在网络传输中，实际上花费在server机器上的时间片不多。这是几个进程就解决高并发的秘密所在

### nginx 和 apache 区别

nginx 比 apache 占用更少的资源

nginx 处理请求是异步非阻塞的，一个进程可以同时处理多个连接；apache 是阻塞的， 一个进程只能同时处理一个连接

### fastcgi 与 cgi 的区别


# jetty

references: [embed jetty into your app](https://www.cnblogs.com/windlaughing/archive/2013/06/07/3125358.html), [source code reading](https://my.oschina.net/tryUcatchUfinallyU?tab=newest&catalogId=300813), [official specifications](https://www.eclipse.org/jetty/documentation/9.4.12.v20180830/)

embed jetty into your app -> https://github.com/xiaoyureed/samples/

## 部署 app 到 jetty 内部

开发好的应用程序打成 WAR 包放到 Jetty 的 Webapps 目录下面。然后用如下的命令来启动 Jetty 服务器：Java –jar start.jar， 在启动服务器后。我们就可以访问我们的应用程序了，Jetty 的默认端口是 8080，WAR 的名字也就是我们的应用程序的 Root Context。例如一个典型的 URL 就是：`http://127.0.0.1:8080/sample/index.html` 。

## jetty支持servlet3.0注解

Jetty Distribution - 默认支持

Jetty Maven Plugin - pre-enabled for the Maven plugin

embedded jetty 需要手动设置:  include the `jetty-annotations` jar, include the `org.eclipse.jetty.annotations.AnnotationConfiguration`

## jetty的整个结构

![alt](Snipaste_2018-10-30_13-04-46.png)


- 整个 Jetty 的所有组件的生命周期管理是基于观察者模式设计

- 整个 Jetty 的核心组件由 Server 和 Connector 两个组件构成

    - 整个 Server 组件是基于 Handler 容器工作的，它类似与 Tomcat 的 Container 容器; server组件由`多个 server Handler` 组成
    
    - Connector，它负责接受客户端的连接请求，并将请求分配给一个处理队列去执行

- Jetty 中还有一些非必须的组件，我们可以在它上做扩展。如 JMX，我们可以定义一些 Mbean 把它加到 Server 中，当 Server 启动的时候，这些 Bean 就会一起工作。

- Handler 的体系结构 - 提供了两种 Handler 类型

    - HandlerWrapper - 可以将一个 Handler 委托给另外一个类去执行

    - HandlerCollection - 将多个 Handler 组装在一起，构成一个 Handler 链, ScopeHandler 可以帮助你控制这个链的访问顺序


## Jetty的工作过程

### 启动

Server 的 start 方法调用就会调用所有已经注册到 Server 的组件，Server 启动其它组件的顺序是：首先启动设置到 Server 的 Handler，通常这个 Handler 会有很多子 Handler，这些 Handler 将组成一个 Handler 链。Server 会依次启动这个链上的所有 Handler。接着会启动注册在 Server 上 JMX 的 Mbean，让 Mbean 也一起工作起来，最后会启动 Connector，打开端口，接受客户端请求

### 接受请求

- 使用两种协议: 一个是 HTTP，一个是 AJP 协议

- 如果前端没有其它 web 服务器，那么 Jetty 应该是基于 HTTP 协议工作。也就是当 Jetty 接收到一个请求时，必须要按照 HTTP 协议解析请求和封装返回的数据 - 设置 Jetty 的 Connector 实现类为 `org.eclipse.jetty.server.bi.SocketConnector `让 Jetty 以 BIO 的方式工作

- 在应用服务器，如 Jboss 的前面在加一个 web 服务器，如 Apache 或者 nginx，这样做的目的是: 做日志分析、负载均衡、权限控制、防止恶意请求以及静态资源预加载等等。此时使用 ajp协议  

    - 这种架构下 servlet 引擎就不需要解析和封装返回的 HTTP 协议，因为 HTTP 协议的解析工作已经在 Apache 或 Nginx 服务器上完成了，Jboss 只要基于更加简单的 AJP 协议工作就行了，这样能加快请求的响应速度。

    - 让 Jetty 工作在 AJP 协议下，需要配置 connector 的实现类为 `Ajp13SocketConnector`

### 处理请求


Jetty 要做的就是调用你注册的第一个 Handler 的 `handle(String target, Request baseRequest, HttpServletRequest request, HttpServletResponse response) `方法

```java
Server server = new Server(8080); 
ContextHandler context = new ContextHandler(); 
context.setContextPath("/"); 
context.setResourceBase("."); 
context.setClassLoader(Thread.currentThread().getContextClassLoader()); 
server.setHandler(context); 
context.setHandler(new HelloHandler()); 
server.start(); 
server.join();

```

# Tomcat

references: [1](https://www.ibm.com/developerworks/cn/java/j-lo-tomcat1/), [2](https://www.ibm.com/developerworks/cn/java/j-lo-tomcat2/)

## Tomcat 总体结构

![alt](Snipaste_2018-10-30_13-56-29.png)

- 2个核心组件 - Connector 和 Container: 

    - connector 接收浏览器的发过来的 tcp 连接请求: 创建 Request, response 对象, 传递给 container的线程

    - container 主要处理 Connector 接受的请求: 产生一个线程来处理connector产生的 Request 和 Response 对象

- 多个 Connector 和一个 Container 就形成了一个 Service对外提供服务 (service 就是一个接口)

- server 可以包含多个 service

### Connector

主要任务是负责接收浏览器的发过来的 tcp 连接请求，创建一个 Request 和 Response 对象分别用于和请求端交换数据，然后会产生一个线程来处理这个请求并把产生的 Request 和 Response 对象传给处理这个请求的线程，处理这个请求的线程就是 Container 组件要做的事了

### Container

![alt](Snipaste_2018-10-30_14-19-38.png)

Container 是容器的父接口，所有子容器都必须实现这个接口，Container 容器的设计用的是典型的责任链的设计模式

它有四个子容器组件构成, 有父子关系，分别是：Engine、Host、Context、Wrapper (遵从 "parent -> child" 顺序:)

一个 Servlet class 对应一个 Wrapper，如果有多个 Servlet 就可以定义多个 Wrapper

真正管理 Servlet 的容器是 Context 容器，一个 Context 对应一个 Web 工程

```xml
<Context
    path="/library"
    docBase="D:\projects\library\deploy\target\library.war"
    reloadable="true" 
/>
<!-- 
- reloadable="true" - war 被修改后 Tomcat 会自动的重新加载这个应用
 -->

```

Context 还可以定义在父容器 Host 中，Host 不是必须的，但是要运行 war 程序，就必须要 Host，因为 war 中必有 web.xml 文件，这个文件的解析就需要 Host 了，如果要有多个 Host 就要定义一个 top 容器 Engine 了。而 Engine 没有父容器了，一个 Engine 代表一个完整的 Servlet 引擎。

四个组件都会有自己的一套 Valve 集合, 在 server.xml 文件中可以添加Valve; StandardEngineValve 和 StandardHostValve 是 Engine 和 Host 的默认的 Valve，它们是最后一个 Valve 负责将请求传给它们的子容器，以继续往下执行

Wrapper 的实现类是 StandardWrapper，StandardWrapper 还实现了拥有一个 Servlet 初始化信息的 ServletConfig，由此看出 StandardWrapper 将直接和 Servlet 的各种信息打交道

## 用到了哪些设计模式

### Facade

门面模式

HttpRequestFacade 类封装了 HttpRequest 接口能够提供数据，通过 HttpRequestFacade 访问到的数据都被代理到 HttpRequest 中

### Observer

观察者模式

控制组件生命周期的 Lifecycle 就是这种模式的体现

### Command

命令模式

Connector 也是通过命令模式调用 Container

### Chain of responsibility

责任链模式

 Tomcat 中 Container , 整个容器的就是通过一个链连接在一起，这个链一直将请求正确的传递给最终处理请求的那个 Servlet

## Tomcat 优化

https://mp.weixin.qq.com/s/3WQB3NGkPrSqmLaYIJ7koQ