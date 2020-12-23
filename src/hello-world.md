---
title: Hello World
tag: hello
reward: true
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

<!--more-->

## init

```sh
hexo init
# 默认使用 landscape, 这里使用 even
git clone https://github.com/ahonn/hexo-theme-even themes/even
# even 需要使用 scss 
npm install hexo-renderer-scss --save
cp themes/even/_config.yml.example themes/even/_config.yml
# 自动部署工具
npm install hexo-deployer-git --save


```

修改如下 `_config.yml`:

```yml
title: 肖雨的网络 WiKi
author: 肖雨

url: https://xiaoyureed.gitee.io

theme: even


```

`hexo s` 预览

## Quick Start

### Create a new post

``` bash
# create post/draft
$ hexo new [draft] "My New Post"

# 发布草稿
hexo publish "xxx"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
# or 
# hexo s
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
# or
# hexo g
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
# hexo d
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

### clean 

```sh
hexo clean
```
