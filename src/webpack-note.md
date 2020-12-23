---
title: Webpack
tags:
  - webpack
date: 2018-06-28 20:26:45
categories: frontend
---

<div align="center">
[官方文档](https://webpack.js.org/concepts/)
http://zhaoda.net/webpack-handbook/module-system.html
{% post_link js-proj-history 📚 前端模块化 %}
</div>

<!--more-->

<!-- TOC -->

- [介绍](#介绍)
- [为什么需要 webpack](#为什么需要-webpack)
- [安装](#安装)
- [webpack.config.js](#webpackconfigjs)
- [Loader](#loader)
- [命令行](#命令行)
- [插件](#插件)

<!-- /TOC -->

# 介绍

webpack: 模块分析打包工具（module bundler）, 它将一堆文件中的每个文件都作为一个模块(module)，找出他们的依赖关系，将它们打包为可部署的静态资源(bundle)

主要作用:

- 将 CSS、图片与其他资源打包

- 打包之前预处理（Less、CoffeeScript、JSX、ES6 等）

- 依 entry 文件不同，把 .js 分拆为多个 .js 

- 整合丰富的 Loader 可以使用（Webpack 本身仅能处理 JavaScript ，其余如：CSS、Image 需要载入不同 Loader 进行处理）

    - Webpack 本身只能处理原生的 JavaScript 模块，但是 loader 转换器可以将各种类型的资源转换成 JavaScript 模块。这样，任何资源都可以成为 Webpack 可以处理的模块。

    - 由于loader的存在, 可同通过require方式加载各种打包成模块的资源:

      ```js
      require("./style.css");
      require("./style.less");
      require("./template.jade");
      require("./image.png");
      ```

# 为什么需要 webpack

首先考虑一个问题：前端模块怎么加载呢(资源怎么从服务器传送到浏览器呢)

什么是前端中的模块 ? 不仅包括js模块文件, 样式、图片、字体、HTML 模板等等众多的资源。这些资源还会以各种方言的形式存在，比如 coffeescript、 less、 sass、众多的模板库、多语言系统（i18n）等等都是模块， 那么如何加载这些模块?

🐎一种是每个模块文件都单独请求，

🐎另一种是把所有模块打包成一个文件然后只请求一次。显而易见，`每个模块都发起单独的请求造成了请求次数过多，导致应用启动速度慢；一次请求加载所有模块导致流量浪费、初始化过程慢。这两种方式都不是好的解决方案`，它们过于简单粗暴。

实际的加载方式：`分块传输，按需进行懒加载`，在实际用到某些模块的时候再增量更新，才是较为合理的模块加载方案。要实现模块的按需加载，就需要一个对整个代码库中的模块进行静态分析、编译打包的过程。webpack由此引入.



# 安装

通常我们会将 Webpack 安装到项目的依赖中而不是全局安装，这样就可以使用项目本地版本的 Webpack。

> 全局安装有一个版本管理问题， 如果我们的项目中，有的用webpack 1.0，有的用webpack2.0 ，而全局webpack 却只有一个命令，那就不好办了

```sh
npm init                    # 生成package.json
npm install webpack webpack-cli -D      # 安装
npm info webpack            # 查看安装情况
# Webpack 开发工具: webpack-dev-server 可以启动开发用 server，方便我们测试
npm install webpack-dev-server --save-dev
```

打开package.json 文件，找到scripts 字段: 

```json
"scripts": {
    "dev": "webpack-dev-server --mode development",
    "build": "webpack --mode production"// 输出压缩过的代码
},
// webapck4开始, 可以直接配置 "build": "webpack", 默认输入为 ./src/index.js 默认输出为./dist/main.js
```

`npm run build`or `npm run dev` 

这时候没有 webpack.config.js(为空文件), `webpack`命令默认读取`src/index.js`生成 `dist/main.js`

#  webpack.config.js

分环境配置: https://segmentfault.com/a/1190000019711348

webpack也提供了另外一种方式来完成打包工作，就是使用webpack的配置文件 webpack.config.js

Webpack 在执行的时候，除了在命令行传入参数，还可以通过指定的配置文件来执行 (通过 --config 选项来指定配置文件, 默认为 `./webpack.config.js`)，这个文件是一个 node.js 模块，导出一个 json 格式的配置信息对象


```js
// 处理路径
const path = require('path');
// - 创建html入口文件，比如单页面可以生成一个html文件入口，配置N个html-webpack-plugin可以生成N个页面入口
// - 为引入的静态资源添加 hash. 每次 compile 后, 为 html 页面引入的外部资源如script、link动态添加 hash，防止引用缓存的外部文件问题
const HtmlWebpackPlugin = require('html-webpack-plugin');
// 每次 buid 前先清除一下 dist 目录 , 防止 这个目录积累的编译文件过多
const CleanWebpackPlugin = require('clean-webpack-plugin');
// 使用webpack 内建的插件
// 如: 实现 HMR功能(hmr: 无刷新更新页面), 如果
// 使用的是webpack-dev-middleware instead of webpack-dev-server, 
// 则无法使用内建插件实现 hmr, 需要 webpack-hot-middleware插件
const webpack = require('webpack');

module.exports = {
    // entry 入口文件：webpack 打包的时候的从哪个文件开始。 相当于webpack 命令中 src/index.js 
    // 单页面
    // entry: './src/index.js', // 等同于  main: './path/to/my/entry/file.js'

    // 多页面
    // entry: {
    //     pageOne: './src/pageOne/index.js',
    //     pageTwo: './src/pageTwo/index.js',
    //     pageThree: './src/pageThree/index.js'
    // },
    entry: {
        // app 在 output 中会被引用, 通过 `[name]`, 
        app: './src/index.js'
    },

    output: {
        // 不使用 entry 中的 key
        // filename: 'main.js',

        // [name]表示entry中的key, app.bundle.js
        filename: '[name].bundle.js',
        // 可省略, 默认输出到 'dist' 文件夹
        path: path.resolve(__dirname, 'dist'), // 等价 `${__dirname}/dist`
        publicPath: '/' // 表示 express将会启动在 http://localhost:3000, 在 server.js中指定
    },

    // 三个选项: none, development or production(default). 
    // 或者在 npm scripts 中 'webpack --mode=production'
    // mode: 'production', 

    module: {
        rules: [{
            // 配置 css loader
            // Webpack 本身只能处理 JavaScript 模块，如果要处理其他类型的
            // 文件，就需要使用 loader 进行转换
            // 更多 loader: https://webpack.js.org/guides/hot-module-replacement/#other-code-and-frameworks
            test: /\.css$/,
            use: [
                // 一般来说需要引入css-loader和style-loader，其中css-loader用
                // 于解析，而style-loader则将解析后的样式嵌入js代码。
                // 顺序不可变: 从左到右 越来越具体
                'style-loader', 'css-loader'
            ]
        }]
    },

    // 插件用于拓展 webpack 功能
    // 是一个数组, 每个元素都是一个 插件对象
    // 自定义参数通过插件的构造函数传入
    plugins: [
        // 默认 清除这个目录: output.path
        new CleanWebpackPlugin(),

        new HtmlWebpackPlugin({
            title: 'Hello World app',// 指定index.html的title

            // template: './src/index.html', // 指定一个index.html模板, 会原样复制到dist

            // filename: 'index.html',

            minify: { // 压缩HTML文件
                removeComments: true, // 移除HTML中的注释
                collapseWhitespace: true, // 删除空白符与换行符
                minifyCSS: true// 压缩内联css
              },
        }),

        // 方便的查看module的依赖关系, HMR用; 
        // development 会将 process.env.NODE_ENV 的值设为 development 。启用 NamedChunksPlugin 和 NamedModulesPlugin。
        new webpack.NamedModulesPlugin(),
        // 开启hmr支持
        new webpack.HotModuleReplacementPlugin() 
    ],
    // mode: "development", // 设定为开发模式(代码不会压缩), 还有 production模式(压缩代码), 命令行指定: --mode xxx
    devtool: 'inline-source-map',// 追溯错误源文件, 仅需要这一行配置
    devServer: {
        contentBase: './dist',// 命令行指定 --content-base dist
        port: 18080, // 默认: localhost:8080
        hot: true, // Enabling HMR, 对应的命令行参数 webpack-dev-server --hot
        inline: true // 如果 hot(hmr)无效, 则inline(页面整个刷新)
    }
};


```

# Loader

Webpack 本身只能处理 JavaScript 模块，如果要处理其他类型的文件，就需要使用 loader 进行转换

Loader 可以在 require() 引用模块的时候添加，也可以在 webpack 全局配置中进行绑定，还可以通过命令行的方式使用。

```js
// main.js, commonjs写法
require("!style-loader!css-loader!./index.css");//也可以将 index.js 中的 require("!style!css!./style.css") 修改为 require("./style.css") ，然后执行：webpack entry.js bundle.js --module-bind "css=style-loader!css-loader"

// 当然简化也可通过webpack.config.js配置

```

具体引入 less 的方法， 参见：http://www.css88.com/doc/webpack2/loaders/less-loader/

# 命令行

- webpack：开发模式
- webpack -p：production 模式
- webpack --watch：会监听程式码的修改，当储存时有异动时会更新档案
- webpack -d：加入 source maps 档案
- webpack --progress --colors：加上处理进度与颜色

简化: 使用 package.json 中的 scripts 设定, `npm run dev`

# 插件

- extract-text-webpack-plugin该插件的主要是为了抽离css样式,防止将样式打包在js中引起页面样式加载错乱的现象











