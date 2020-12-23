---
title: front-end-development-history
tags: [history, frontend]
categories: java web
---


https://blog.csdn.net/classicbear/article/details/7069223
https://github.com/ruanyf/jstraining/blob/master/docs/history.md

[JavaScript模块化](https://zhuanlan.zhihu.com/p/32324311)的进化
模块化七日谈: https://github.com/Huxpro/js-module-7day

<!--more-->

<!-- TOC -->

- [前端模块管理器和工具](#前端模块管理器和工具)
  - [npm和bower和browserify](#npm和bower和browserify)
  - [Browserify](#browserify)
  - [babel](#babel)
  - [webpack](#webpack)
  - [gulp和grunt](#gulp和grunt)
  - [ESLint](#eslint)
- [阶段一:无模块化(CDN-Based)](#阶段一无模块化cdn-based)
- [阶段二:CommonJS规范](#阶段二commonjs规范)
- [阶段三:AMD规范](#阶段三amd规范)
- [阶段四:CMD规范](#阶段四cmd规范)
- [AMD和CMD区别](#amd和cmd区别)
- [阶段五:ES6规范中的模块化](#阶段五es6规范中的模块化)
- [前端框架分级](#前端框架分级)

<!-- /TOC -->



# 前端模块管理器和工具

Bower，Browserify, npm 
babel

https://www.zhihu.com/question/37694275/answer/113609266
http://www.ruanyifeng.com/blog/2014/09/package-management.html
https://github.com/carlleton/reactjs101/blob/zh-CN/Ch02/browserify-gulp-dev-enviroment.md

## npm和bower和browserify

{% post_link npm-note 📚 npm笔记 %}

* npm是Node.js 下的主流套件管理工具

* NPM 主要是基于 CommonJS 的规范, 通常必须搭配 Browserify (见 [Browserify](#Browserify))这样的工具才能在前端使用 NPM 的模组

* NPM 是基于 Nested Dependency Tree，不同的套件有可能会在引入依赖时会引入相同但不同版本的套件，造成档案大小过大的情形

* 另一个套件管理工具 Bower 和npm区别

    * Bower的安装和升级全都依赖于NPM，使用如下命令就可以全局安装Bower : `npm install -g bower`

    * bower专注在前端套件比如bootstrap，jquery等前端框架, 下载到当前目录的bower_components子目录中,当然依赖的下载目录结构可以自定义; nmp适用于后端比如express(Node.js项目的内部依赖包管理), 安装的模块位于项目根目录下的node_modules文件夹内

    * bower使用 Flat Dependency Tree（只能支持扁平的依赖（嵌套的依赖，由程序员自己解决）; npm基于 Nested Dependency Tree支持嵌套依赖

    * 比如 `bower install jquery`之后, 可以在html中这么使用: (其他方面和npm非常类似)

      ```js
      <script type="text/javascript" src="bower_components/jquery/jquery.min.js"></script>
      ```

## Browserify

https://www.ibm.com/developerworks/cn/web/1501_chengfu_browserify/index.html

* Browserify: Browserify本身不是模块管理器，只是让服务器端的CommonJS格式(使用require(...))的模块可以运行在浏览器端(将require语法编译为普通js, 接着在html中引入)。这意味着通过它，我们可以使用Node.js的npm模块管理器。所以，实际上，它等于间接为浏览器提供了npm的功能

* 前后端可以使用一致的模块管理方式

> 如同官网上说明的：Browserify lets you require('modules') in the browser by bundling up all of your dependencies.，Browserify 是一个可以让你在浏览器端也能使用像 Node 用的 CommonJS 规范一样，用输出（export）和引用（require）来管理模组。此外，也能让前端使用许多在 NPM 中的模组。

## babel

ref: http://www.ruanyifeng.com/blog/2016/01/babel.html

* Babel: 将ES6+ 、JSX 等换成浏览器可以看得懂的语法。通常会在 root 位置加入 .babelrc 进行转译规则 preset 和引用插件（plugin）的设定。

> babellify 这个是 babel 为 browserify 提供的, babellify 类似 babel-loader 是一个使用 Browserify 进行 Babel 转换的插件

## webpack

{% post_link webpack-note 🚪 webpack笔记 %}

## gulp和grunt

👉 更新: 发现更好的替代品------npm srcipts(http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html)

Gulp 是一个前端任务工具自动化管理工具（Task Runner）, 同类: Grunt( Gulp 是通过 pipeline (编码🐎)方式来处理档案，在使用上比起 Grunt 的方式直观许多)

类似 java 中的 编译(.java --> .class)

> 开发前端应用程式时有许多工作是必须重复进行，例如：打包文件、uglify、将 LESS 转译成一般的 CSS 的档案，转译 ES6 语法等工作。若是使用一般手动的方式，往往会造成效率的低下，所以透过像是 Grunt、Gulp 这类的 Task Runner 不但可以提升效率，也可以更方便管理这些任务。

## ESLint

提供 JavaScript 和 JSX 的程式码检查, 根据需求在 .eslintrc 设定检查规则(常用 [ Airbnb React/JSX Style Guide](https://github.com/airbnb/javascript/tree/master/react))

# 阶段一:无模块化(CDN-Based)

也就是最传统的 `<script>` 引入方式, 最开始的js都处在一个script标签中, 进一步发展为提出单独js文件, 使用时候引入, 类似:

```html
<!-- 
缺点:
    1. 污染全局作用域. 因为每一个模块都是暴露在全局的，简单的使用，会导致全局变量命名冲突，当然，我们也可以使用命名空间的方式来解决, 典型的例子如 YUI 库
    2. 依赖关系不明显，不利于维护(即引入顺序不能变). 比如main.js需要使用jquery，但是，从上面的文件中，我们是看不出来的，如果jquery忘记了，那么就会报错
 -->
<script src="jquery.js"></script>
<script src="jquery_scroller.js"></script>
<script src="main.js"></script>
<script src="other1.js"></script>
<script src="other2.js"></script>
<script src="other3.js"></script>
```

# 阶段二:CommonJS规范

* CommonJS就是一个JavaScript模块化的规范，该规范最初是用在服务器端的node的(npm基于commonjs)，前端的webpack也是对CommonJS原生支持的

* CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作

* CommonJS的核心思想就是通过 require 方法来同步加载所要依赖的其他模块，然后通过 exports 或者 module.exports 来导出需要暴露的接口

* 缺点: 只适合服务器端,不适用于浏览器端(由于 CommonJS 是同步加载模块的, 在服务器端，文件都是保存在硬盘上，所以同步加载没有问题，但是对于浏览器端，需要将文件从服务器端请求过来，那么同步加载就不适用了，所以，CommonJS是不适用于浏览器端的。)

```js
// a.js
var x = 5;
var addX = function (value) {
  return value + x;
};
module.exports.x = x;// module就代表了这个模块
module.exports.addX = addX;

/**
使用
*/
var a = require('./a.js');
console.log(a.x); // 5
console.log(a.addX(1)); // 6
```

# 阶段三:AMD规范

* AMD规范则是非同步加载模块，允许指定回调函数. 因此浏览器端一般采用AMD规范。而AMD规范的实现，就是大名鼎鼎的`require.js`了。

* 优点: 适合在浏览器环境中异步加载模块。可以并行加载多个模块。

* 缺点:　提高了开发成本，并且不能按需加载，而是必须提前加载所有的依赖

```js
/**
amd规范定义了两个api

  1.require([module], callback)

   2. define(id, [depends], callback)
*/
define('alert', function () {
    var alertName = function (str) {
      alert("I am " + str);
    }
    var alertAge = function (num) {
      alert("I am " + num + " years old");
    }
    return {
      alertName: alertName,
      alertAge: alertAge
    };
});


require(['alert'], function (alert) {
  alert.alertName('JohnZhu');
  alert.alertAge(21);
});
```

# 阶段四:CMD规范

* 实现js库为`sea.js`

* 它和requirejs(AMD)非常类似，即一个js文件就是一个模块, 但是CMD的加载方式更加优秀，是通过`按需加载`的方式，而不是必须在模块开始就加载所有的依赖

* 优点: 可以按需加载，依赖就近。比amd简洁，却又保持和 CommonJS 的兼容性

* 缺点: 依赖SPM打包，模块的加载逻辑偏重

```js
define(function(require, exports, module) {
  var $ = require('jquery');
  var Spinning = require('./spinning');
  exports.doSomething = ...
  module.exports = ...
})
```

# AMD和CMD区别

AMD: 对于依赖的模块提前执行; 推崇依赖前置

CMD: 对于依赖的模块延迟执行; 推崇依赖就近(在需要用到某个模块的时候再require)

```js
// AMD
define(['./a', './b'], function(a, b) {  // 依赖必须一开始就写好  
   a.doSomething()    
   // 此处略去 100 行    
   b.doSomething()    
   ...
});
// CMD
define(function(require, exports, module) {
   var a = require('./a')   
   a.doSomething()   
   // 此处略去 100 行   
   var b = require('./b') 
   // 依赖可以就近书写   
   b.doSomething()
   // ... 
});
```

> UMD: 为了要兼容 CommonJS 和 AMD 所设计的规范，希望让模组能跨平台执行

# 阶段五:ES6规范中的模块化

* 之前的几种模块化方案都是前端社区自己实现的, 而ES6的模块化方案是真正的规范;

* 在ES6中，我们可以使用 import 关键字引入模块，通过 export 关键字导出模块

* 但是由于ES6目前无法在浏览器中执行，所以，我们只能通过babel将不被支持的import编译为当前受到广泛支持的 require。

* ES6 模块的设计思想，是尽量的静态化，使得编译时就能确定模块的依赖关系，以及输入和输出的变量。CommonJS 和 AMD 模块，都只能在运行时确定这些东西。所以容易进行静态分析.

```js
import store from '../store/index'
import {mapState, mapMutations, mapActions} from 'vuex'
import axios from '../assets/js/request'
import util from '../utils/js/util.js'

export default {
    created () {
        this.getClassify(); 

        this.RESET_VALUE();
        console.log('created' ,new Date().getTime());

    }
}
```


# 前端框架分级

各大前端框架可以按照“封装度”的标准来区分

1. 纯html+css, 不涉及到js，就是纯页面皮肤

1. bootstrap系列, 对响应式的支持以及良好的体验, 是一套ui皮肤+少量js组成的框架

1.  jQuery-ui, 分界点。jQuery以下级别的框架，代码以css为主，自身的js代码少，框架量级更轻，更灵活，更适合互联网web产品。jQuery以上级别的框架，属于前端的重度封装，通过框架暴露的接口进行开发，开发人员甚至不需要太多前端知识

1. easy-ui/DWZ, easy-ui基于jQuery-ui，不过具有更丰富的组件库。貌似商业版收费很高。听说某大型国企花了大价钱购买下来使用。DWZ是国产框架中我认为综合表现还不错的，完全免费

1.  extjs系列, extjs属于前端框架领域中的庞然大物，封装程度很高，具有自成体系的元素选择引擎和浏览器兼容方案，js写法上也有自己的方式。组件很多很全

1. vaadin/GWT, 使用后台语言写前端