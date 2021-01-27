---
title: Nodejs
tags:
  - node
  - javascript
date: 2019-07-25 19:55:53
categories: frontend
---



<div align="center">
https://github.com/ruanyf/jstraining/blob/master/docs/node.md
http://i5ting.github.io/nodejs-fullstack/
https://i5ting.github.io/How-to-learn-node-correctly/ 入门教程

https://github.com/nswbmw/node-in-debugging
https://github.com/wangfupeng1988/js-async-tutorial
https://github.com/nswbmw/N-blog
https://github.com/lowerfish/js-stack-from-scratch
https://github.com/wangdoc/javascript-tutorial/tree/master/docs
</div>

<!--more-->

<!-- TOC -->

- [常用的命令](#常用的命令)
- [what is nodejs](#what-is-nodejs)
- [why we need nodejs](#why-we-need-nodejs)
- [安装](#安装)
- [how to use](#how-to-use)
- [代码生成](#代码生成)
- [deno](#deno)
- [npm](#npm)
  - [常用命令](#常用命令)
  - [开发自定义库发布到npm](#开发自定义库发布到npm)
  - [使用场景](#使用场景)
    - [使用第三方包](#使用第三方包)
    - [使用别人的命令行程序](#使用别人的命令行程序)
    - [发布自己的包or命令行程序](#发布自己的包or命令行程序)
  - [全局安装or本地安装](#全局安装or本地安装)
  - [设置淘宝镜像](#设置淘宝镜像)
    - [临时使用](#临时使用)
    - [持久使用](#持久使用)
  - [package.json](#packagejson)
  - [npm-scripts](#npm-scripts)
  - [npx 命令](#npx-命令)
  - [常用库](#常用库)
    - [react 相关](#react-相关)
- [yarn](#yarn)
  - [基本使用](#基本使用)
  - [yarn比npm好在哪里](#yarn比npm好在哪里)
- [web 后台技术栈](#web-后台技术栈)
- [配合 rust](#配合-rust)

<!-- /TOC -->

# 常用的命令

```shell
# 升级 (https://blog.csdn.net/hl449006540/article/details/79698905)
# 先清除 npm cache 
npm cache clean -f
# 通过 n 来管理版本
npm install -g n
npm view node versions # node所有版本
# 升级到最新稳定版
sudo n stable
# 最新版
n latest
# 指定版本
n 15.0.0

```

# what is nodejs

- Node.js 是 JavaScript 运行时环境, 基于 chrome v8, 因此接受请求时是单线程, 没有切换上下文的成本, 但是执行具体任务时是多线程的, 也就是异步执行, 保证响应速度

- 事件驱动（event-driven），非阻塞 I/O 模型（non-blocking I/O model）, 非常适合高并场景

  简单点讲就是每个函数都是异步的，最后由 Libuv 这个 C/C++ 编写的事件循环处理库来处理这些 I/O 操作，隐藏了非阻塞 I/O 的具体细节，简化并发编程模型，让你可以轻松的编写高性能的Web应用

  单线程就意味着，所有任务需要排队, CPU 很闲, i/o很忙, 那么cpu完全可以不管 I/O 设备，挂起处于等待中的任务，先运行排在后面的任务, 将将等待中的 I/O 任务放到 Event Loop 里, 由 Event Loop 将 I/O 任务放到线程池里

- 使用 npm 作为包管理器

# why we need nodejs

高并发, 比如作为微服务的gateway, 比如在后台api和前端之间搭建中间层

事件驱动的异步编程, 适合用于频繁 i/o 操作的应用

# 安装

https://www.cnblogs.com/jcwalker/p/10871493.html 二进制文件安装


# how to use


```sh
npm init -y # init with default

# node 支持 es6
npm install --save-dev babel-cli
npm install --save-dev babel-preset-es2015
# 编辑 .babelrc
# {
#   "presets": [
#     "es2015"
#   ],
#   "plugins": []
# }

```

app.js

```js
import fs from 'fs';

// 相对路径, 相对于命令行所在的路径
// npm scripts 中的路径是相对于 package.json 来说的
fs.readFile('./src/app.js', (err, data) => {
  if (err) {
    console.error(err);
  } else {
    console.log(data.toString());
  }
});
```

package.json

```json
{
  "name": "nodejs-tutorial",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "babel-node src/app.js"
  },
  "author": "xiaoyu",
  "license": "ISC",
  "devDependencies": {
    "babel-cli": "^6.26.0",
    "babel-preset-es2015": "^6.24.1"
  }
}

```

# 代码生成

```js
import fs from 'fs';
import { Pool } from 'pg';
import ejs from "ejs";

// fs.readFile('./src/app.js', function (err, data) {
//   if (err) {
//     console.log(err);
//   } else {
//     console.log(data.toString());
//   }
// });

const pool = new Pool({
  host: '10.59.97.10',
  port: '5432',
  database: 'sat_sme',
  user: 'sat_data',
  password: 'paic1234',

});

// const sql = 'select * from sme_application_file limit 0';
const sql = `SELECT col_description(a.attrelid, a.attnum) as comment,
format_type(a.atttypid, a.atttypmod)  as type,
a.attname                             as name,
a.attnotnull                          as notnull
FROM pg_class as c,
pg_attribute as a
where c.relname = 'sme_application_file'
and a.attrelid = c.oid
and a.attnum > 0`;

pool.query(sql, (err, res) => {
  if (err) {
    console.log(err);
  } else {
    // hanle(res);
    ejsDemo();
  }
});

function hanle(res) {
  var result = res.rows;
  console.log(result);
}

function ejsDemo() {
  fs.readFile('./resources/tpl/demo.ejs', (err, data) => {
    if (err) {
      console.error(err);
    } else {
      var result = ejs.render(data.toString(), { hah: ['a', 'b', 'c'] });
      console.log(result);
    }
  });
}
```

# deno

作为 nodejs 的替代品, https://deno.land/

[一片吐槽 deno 的文章](https://zhuanlan.zhihu.com/p/139386360)

可以直接运行 JavaScript、TypeScript 和 WebAssembly 程序。

>它内置了 V8 引擎，用来解释 JavaScript。同时，也内置了 tsc 引擎，解释 TypeScript。它使用 Rust 语言开发，由于 Rust 原生支持 WebAssembly，所以它也能直接运行 WebAssembly。它的异步操作不使用 libuv 这个库，而是使用 Rust 语言的 Tokio 库，来实现事件循环（event loop）

node 问题:

- nodejs 的异步写法有两种, Promise, 回调函数, 正常来说, 回调函数 的写法应该被淘汰的

  为了兼容历史类库, 不得不维持这种 callback 写法

- nodejs 自己的模块格式 CommonJS 与 ES 模块不兼容

- Node.js 的模块管理工具 npm，逻辑越来越复杂；模块安装目录 npm_modules 极其庞杂，难以管理

- Node.js 也几乎没有安全措施，用户只要下载了外部模块，就只好听任别人的代码在本地运行，进行各种读写操作。


```sh
# 查看
brew info deno
# 安装
brew install deno

deno --version

#### quickstart

deno run --allow-net \
https://deno.land/std/examples/curl.ts \
https://example.com

```

和浏览器 api 兼容 (它提供 window 这个全局对象，同时支持 fetch、webCrypto、worker 等 Web 标准，也支持 onload、onunload、addEventListener 等事件操作函数), 所有的异步操作，一律返回 Promise。


更现代的模块管理 (Deno 只支持 ES 模块，跟浏览器的模块加载规则一致。没有 npm，没有 npm_modules 目录，没有require()命令（即不支持 CommonJS 模块），也不需要package.json文件。支持 `import { bar } from "https://foo.com/bar.ts"`, or `import { bar } from './foo/bar.ts'`, 不需要一个中心化的模块储存系统，可以从任何地方加载模块。)

Deno 的所有模块都要通过入口脚本加载，不能通过模块名加载，所以必须带有脚本后缀名。

原生支持 ts, (它的内部会根据文件后缀名判断，如果是.ts后缀名，就先调用 TS 编译器，将其编译成 JavaScript；如果是.js后缀名，就直接传入 V8 引擎运行。)

完整的工具链

```sh
# deno -h或deno help 查看

deno bundle：将脚本和依赖打包
deno eval：执行代码
deno fetch：将依赖抓取到本地
deno fmt：代码的格式美化
deno help：等同于-h参数
deno info：显示本地的依赖缓存
deno install：将脚本安装为可执行文件
deno repl：进入 REPL 环境
deno run：运行脚本
deno test：运行测试

```


更加安全

```
--allow-read：打开读权限，可以指定可读的目录，比如--allow-read=/temp。
--allow-write：打开写权限。
--allow-net=google.com：允许网络通信，可以指定可请求的域，比如--allow-net=google.com。
--allow-env：允许读取环境变量。
```

# npm


## 常用命令

```shell
npm bin                     # 返回当前项目模块路径, 使用: `npm bin`/eslint --init

npm -v                      # node版本


npm search express          # 搜索


# "install" can be replaced with "i"
npm install npm -g                              # 升级 npm

npm install <Module Name>                       # 安装模块 ,包就放在了工程目录下的 node_modules 目录中
npm install -S <module name>                    # -S表示--save, 安装同时, 写入package.json中的"dependencies" 中
npm install -D <module name>                    # -D: --save-dev, 会存在package.json的devDependencies这个里面

npm install gulp --save-exact                   # 精确安装(指定)版本, package.json中版本号前没有 "^" 了
# or
npm install gulp -E

npm i -g webpack webpack-dev-server             # i 表示install, -g 全局
npm install                                     # 根据package.json安装


npm uninstall express       # 卸载
npm uninstall webpack -g    # 全局卸载
npm un xxx # 等价


npm update express          # 升级模块
npm update <package>        # 升级包下所有
pm update <package> -g      # 把全局安装的对应命令行程序更新至最新版


npm list -g                 # 查看所有全局安装的模块(查看安装信息)
npm list <module name>      # 查看某个模块的版本号


npm init                    # 创建模块(创建package.json)


npm adduser                 # 注册用户


npm publish                 # 发布模块
npm install . -g            # 先在本地安装当前命令行程序，可用于发布前的本地测试
npm unpublish <package>@<version>   # 可以撤销发布自己发布过的某个版本代码

npm prune                   # 移除清理未使用的模块

npm list -g --depth=0       # 查看全局安装的包
npm uninstall -g vue-cli    # 卸载全局


```

## 开发自定义库发布到npm

https://www.bilibili.com/video/BV187411n7Hh, 

使用 rollup.js, 用于打包类库的 webpack, (webpack 是用于应用打包的)

## 使用场景

### 使用第三方包

```sh
$ npm install argv # 下载最新版
$ npm install argv@0.0.1 # 下载指定版本
```

下载好之后，argv包就放在了工程目录下的node_modules目录中，因此在代码中只需要通过require('argv')的方式就好，无需指定三方包路径

如果需要下载大量包, 可使用package.json

```json
{
    "name": "node-echo",
    "main": "./lib/echo.js", // 入口文件
    "dependencies": {
        "argv": "0.0.2" // 依赖包
    }
}
```

这样处理后，在工程目录下就可以使用npm install命令批量安装三方包了, 且无需解决包的嵌套依赖关系


### 使用别人的命令行程序

```sh
npm install node-echo -g
```

参数中的-g表示全局安装，因此node-echo会默认安装到以下位置，并且NPM会自动创建好Linux系统下需要的软链文件或Windows系统下需要的.cmd文件

```
- /usr/local/               # Linux系统下
    - lib/node_modules/
        + node-echo/
        ...
    - bin/
        node-echo
        ...
    ...

- %APPDATA%\npm\            # Windows系统下
    - node_modules\
        + node-echo\
        ...
    node-echo.cmd
    ..

```

### 发布自己的包or命令行程序

第一次使用NPM发布代码前需要注册一个账号。终端下运行`npm adduser`，之后按照提示做即可。账号搞定后，接着我们需要编辑package.json文件，加入NPM必需的字段。接着上边node-echo的例子，package.json里必要的字段如下。

```json
{
    "name": "node-echo",                      # 包名，在NPM服务器上须要保持唯一
    "version": "1.0.0",                       # 当前版本号
    "dependencies": {                         # 三方包依赖，需要指定包名和版本号
        "argv": "0.0.2"
      },
    "main": "./lib/echo.js",                  # 入口模块位置
    "bin" : {
        "node-echo": "./bin/node-echo"        # 命令行程序名和主模块位置
    }
}
```

之后，我们就可以在package.json所在目录下运行`npm publish`发布代码了

## 全局安装or本地安装

```sh
npm install express           # 本地安装
npm install express -g        # 全局安装
```

* 本地安装

    1. 将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。

    2. 可以通过 require() 来引入本地安装的包。

* 全局安装

    1. 将安装包放在 /usr/local 下或者你 node 的安装目录。
    
    2. 可以直接在命令行里使用。

* 如果你希望具备两者功能，则需要在两个地方安装它或使用 `npm link`

## 设置淘宝镜像

https://blog.csdn.net/quuqu/article/details/64121812

### 临时使用

安装想要的包时候, 命令行临时指定

```sh
npm install koa --registry=http://r.cnpmjs.org
```

### 持久使用

通过 config 配置, 更换为淘宝镜像

```sh
npm config set registry="https://registry.npm.taobao.org"
# or
npm config set registry="http://r.cnpmjs.org"

# 看看是否配置成功
npm config get registry

# 回复原样
npm config set registry https://registry.npmjs.org
```

持久使用也可以这样
编辑 ~/.npmrc

```
registry = http://r.cnpmjs.org
```

or 使用cnpm

```sh
# 安装
npm install -g cnpm --registry=https://registry.npm.taobao.org
npm install -g cnpm --registry=http://r.cnpmjs.org

$ cnpm install [name] # 安装
$ cnpm sync connect # 同步
$ open http://cnpmjs.org/sync/connect # 通过web浏览器查看

$ cnpm publish [name]
$ cnpm unpublish [name]

$ cnpm info cnpm
```

```sh
cnpm install [name]

cnpm sync connect # 同步模块, 独有
```

## package.json

相当于Maven的pom.xml

https://javascript.ruanyifeng.com/nodejs/packagejson.html#toc5

```json
{
  "name": "express",                                                              // 包名 📌
  "version": "4.13.3",                                                            // 版本号 📌
  "description": "Fast, unopinionated, minimalist web framework",                 // 包的描述
  "author": {
    "name": "TJ Holowaychuk",
    "email": "tj@vision-media.ca"
  },
  "contributors": [
    {
      "name": "Aaron Heckmann",
      "email": "aaron.heckmann+github@gmail.com"
    },
    {
      "name": "Ciaran Jessup",
      "email": "ciaranj@gmail.com"
    }
  ],
  "license": "MIT",
  
  "repository": {                                     // 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上
    "type": "git",
    "url": "git+https://github.com/strongloop/express.git"
  },
  "homepage": "http://expressjs.com/",
  "main": "index.js", // 指定了加载的入口文件，require('moduleName')就会加载这个文件。这个字段的默认值是模块根目录下面的index.js
  
  "keywords": [                                         // 方便别人搜索到本模块
    "express",
    "framework",
    "sinatra"
  ],
  
  // 上线时候依赖的包 npm install --save 会将包写到这里
  // dependencies属性是一个对象，配置模块依赖的模块列表，key是模块名称，value是版本范围，版本范围是一个字符，可以被一个或多个空格分割。
  // dependencies也可以被指定为一个git地址或者一个压缩包地址
  // ^ ~ * 区别: https://blog.csdn.net/u014291497/article/details/70148468
  //    ~会匹配最新的小版本依赖包，比如~1.2.3会匹配所有1.2.x版本，但是不包括1.3.0(推荐)
  //    ^会匹配最新的大版本依赖包，比如^1.2.3会匹配所有1.x.x的包，包括1.3.0，但是不包括2.0.0
  //    *，这意味着安装最新版本的依赖包
  "dependencies": {
    "accepts": "~1.2.12",
    "array-flatten": "1.1.1",
    "vary": "~1.0.1"
  },
  // 开发时候依赖的包 npm install --save-dev会将包写到这里
  // 存放测试代码依赖的包或构建工具的包
  "devDependencies": {
    "after": "0.8.1",
    "ejs": "2.3.3",
    "vhost": "~3.0.1"
  },
  // node的参考版本, 除非你设置了engine-strict属性，engines属性是仅供参考的
  "engines": {
    "node": ">= 0.10.0"
  },
  // 你也可以在模块根目录下创建一个".npmignore"文件，写在这个文件里边的文件即便被写在files属性里边也会被排除在外，这个文件的写法".gitignore"类似。
  "files": [
    "LICENSE",
    "History.md",
    "Readme.md",
    "index.js",
    "lib/"
  ],
  // scripts属性是一个对象，里边指定了项目的生命周期个各个环节需要执行的命令。key是生命周期中的事件，value是要执行的命令
  "scripts": {
    "test": "mocha --require test/support/env --reporter spec --bail --check-leaks test/ test/acceptance/",
    "test-ci": "istanbul cover node_modules/mocha/bin/_mocha --report lcovonly -- --require test/support/env --reporter spec --check-leaks test/ test/acceptance/",
    "test-cov": "istanbul cover node_modules/mocha/bin/_mocha -- --require test/support/env --reporter dot --check-leaks test/ test/acceptance/",
    "test-tap": "mocha --require test/support/env --reporter tap --check-leaks test/ test/acceptance/"
  },
  "gitHead": "ef7ad681b245fba023843ce94f6bcb8e275bbb8e",
  "bugs": {
    "url": "https://github.com/strongloop/express/issues"
  },
  "_id": "express@4.13.3",
  "_shasum": "ddb2f1fb4502bf33598d2b032b037960ca6c80a3",
  "_from": "express@*",
  "_npmVersion": "1.4.28",
  "_npmUser": {
    "name": "dougwilson",
    "email": "doug@somethingdoug.com"
  },
  "maintainers": [
    {
      "name": "tjholowaychuk",
      "email": "tj@vision-media.ca"
    },
    {
      "name": "defunctzombie",
      "email": "shtylman@gmail.com"
    }
  ],
  "dist": {
    "shasum": "ddb2f1fb4502bf33598d2b032b037960ca6c80a3",
    "tarball": "http://registry.npmjs.org/express/-/express-4.13.3.tgz"
  },
  "directories": {},
  "_resolved": "https://registry.npmjs.org/express/-/express-4.13.3.tgz",
  "readme": "ERROR: No README data found!"
}
```

## npm-scripts

http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html

```sh
npm run xxx
#// 等价
npm xxx

```

npm scripts 可以淘汰掉 grunt 和 gulp (gulp 比 grunt更简单)

## npx 命令

https://www.jianshu.com/p/cee806439865

改善第三方包内的命令的使用体验, 如果有些包我们只会使用一次，或者只想尝试以下，不想安装到全局，也不想作为当前项目的依赖
可以使用 npx 的方式来执行

1、临时安装可执行依赖包，不用全局安装，不用担心长期的污染。
2、可以执行依赖包中的命令，安装完成自动运行。
3、自动加载node_modules中依赖包，不用指定$PATH。
4、可以指定node版本、命令的版本，解决了不同项目使用不同版本的命令的问题

```sh
# old way
npm install -g create-react-app
create-react-app my-app

# new way
npx create-react-app my-app # 命令完成后create-react-app 会删掉

###################

# old
npm i -D webpack
./node_modules/.bin/webpack -v
# or
`npm bin`/webpack -v

# new
npm i -D webpack
npx webpack -v


```

## 常用库

### react 相关

- `npm i -D babel-loader @babel/core @babel/preset-env @babel/preset-react @babel/plugin-transform-runtime` 用于转译 es6 代码

    - `babel-loader` 是一个 npm 包，它使得 webpack 可以通过 babel 转译 JavaScript 代码

    - `@babel/core` 用于 解析源代码

    - `@babel/preset-*` 实际上就是各种插件的打包组合，也就是说各种转译规则的统一设定，用于生成目标代码

    - @babel/plugin-transform-runtime 

    在 babel 7 中 `babel-core` 和 `babel-preset` 被建议使用 `@babel` 开头声明作用域，因此应该分别下载 `@babel/core` 和· `@babel/presets`. '@xxx' 的语法 类似 maven 中 group id.

    babel-loader与babel-core的版本对应关系 (版本冲突解决) - https://www.cnblogs.com/gavinpan/p/11437773.html

- `npm install --save-dev babel-polyfill` Babel 只转换语法(如箭头函数)， 你可以使用 babel-polyfill 支持新的全局变量，例如 Promise 、新的原生方法如 String.padStart (left-pad) 等。

    在 入口文件引入即可 `import 'babel-polyfill'`


# yarn

## 基本使用

```sh
npm install -g yarn

升级: `curl -o- -L https://yarnpkg.com/install.sh | bash`




npm install === yarn 
npm install taco --save === yarn add taco
npm uninstall taco --save === yarn remove taco
npm install taco --save-dev === yarn add taco --dev
npm update --save === yarn upgrade

npm run <xxx> === yarn <xxx>

# 缓存清理
npm cache clean --force === yarn cache clean
# 查看全局安装的模块
npm list -g --depth=0 === yarn global list --depth=0
# 卸载全局安装的模块
npm uninstall -g vue-cli === yarn global remove vue-cli


```

## yarn比npm好在哪里

更新: npm5已经追上来了

https://zhuanlan.zhihu.com/p/27449990



# web 后台技术栈

- 后台应用: node + MongoDB,mongoose + multer 文件上传 + blueimp-md5 
- axios + jsonp + Promise/async/await 前后台交互
- commonjs(后台模块化, 前台模块化用 es6) 
- draftjs(富文本编辑器) +echarts图表

# 配合 rust

https://blog.logrocket.com/rust-and-node-js-a-match-made-in-heaven/