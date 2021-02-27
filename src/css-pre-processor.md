---
title: 几种CSS预处理器比较选型
tags:
  - css
  - processor
date: 2018-05-02 10:54:38
categories: frontend
---
<div align="center">
css的预处理器主流的大概有: SASS, LESS, Stylus;

https://tympanus.net/codrops/ 灵感

css 模块化

</div>

<!--more-->

<!-- TOC -->

- [css 预处理器介绍](#css-预处理器介绍)
  - [why css preprocessor](#why-css-preprocessor)
  - [会带来的新问题](#会带来的新问题)
- [选型和对比](#选型和对比)
- [PostCSS](#postcss)
  - [简单介绍 postcss](#简单介绍-postcss)
  - [怎么使用](#怎么使用)
  - [自定义postcss插件](#自定义postcss插件)
- [SASS](#sass)
- [LESS](#less)
  - [环境配置](#环境配置)
  - [浏览器端用法](#浏览器端用法)
  - [语法](#语法)
    - [变量](#变量)
    - [混合](#混合)
      - [不带参数](#不带参数)
      - [带参数](#带参数)
      - [参数有默认值](#参数有默认值)
    - [暂时隐藏css属性](#暂时隐藏css属性)
    - [@arguments变量](#arguments变量)
    - [模式匹配(混合中的参数匹配)](#模式匹配混合中的参数匹配)
    - [导引表达式(类似@media query)](#导引表达式类似media-query)
      - [单表达式](#单表达式)
      - [多表达式](#多表达式)
    - [参数值类型判断(isXXX内置函数)](#参数值类型判断isxxx内置函数)
    - [嵌套](#嵌套)
    - [运算](#运算)
    - [color函数](#color函数)
    - [Math函数](#math函数)
    - [命名空间](#命名空间)
    - [作用域](#作用域)
    - [importing引入](#importing引入)
    - [字符串插值](#字符串插值)
    - [避免编译](#避免编译)
    - [JavaScript 表达式](#javascript-表达式)
- [css 模块化](#css-模块化)
- [css 框架](#css-框架)
- [响应式页面](#响应式页面)

<!-- /TOC -->

# css 预处理器介绍

## why css preprocessor

CSS仅仅是一个标记语言，不是编程语言，因此不可以自定义变量，不可以引用

## 会带来的新问题

CSS的好处在于简便、随时随地被使用和调试。

预编译CSS步骤的加入，让我们开发工作流中多了一个环节，调试也变得更麻烦了。

更大的问题在于，预编译很容易造成后代选择器的滥用。

# 选型和对比

语法区别: https://zhuanlan.zhihu.com/p/38715068, https://efe.baidu.com/blog/revisiting-css-preprocessors/

============ sass 优点

sass 比 less 更像完整的编程语言, 有 变量 (全局, 局部区分), 函数, 作用域, 流程控制, 数据结构...,

有成熟的框架, 如 Compass, Foundation

社区热度比 less 更发达

还有Scss对sass语法进行了改良, 完全兼容 css 语法 (和 less 打平了)

bootstrap最新版就抛弃 less, 转向 sass了

丰富的 ruby api 支持, 适合开发 css 框架

=============== sass 缺点

但是 sass 安装需要 ruby 环境支持, less 只需要 引入 'less.js' 即可

开发工具也没有相应的sass编码提示

=================Stylus

来自node.js社区

人气不及sass, less

语法更简洁, 比如用缩进代替大括号

# PostCSS

## 简单介绍 postcss

https://github.com/postcss/postcss git 仓库

不是 css preprocessor, 是一个使用JS插件来转换样式的工具 (检查（lint）你的 CSS，支持 CSS Variables 和 Mixins， 编译尚未被浏览器广泛支持的先进的 CSS 语法，内联图片)

作为一个平台, 可以编写各种插件实现对 css 的处理, Autoprefixer 是最火的插件 (底层原理实际是 PostCSS 提供了一个解析器，它能够将 CSS 解析成抽象语法树（AST）。)

TailwindCSS 可以作为 postcss 的一个插件来使用

可以搜索插件 在这里 https://www.postcss.parts/ , 所有插件同时列在这里了 https://github.com/postcss/postcss/blob/main/docs/plugins.md

## 怎么使用

## 自定义postcss插件

https://github.com/postcss/postcss/blob/main/docs/writing-a-plugin.md

# SASS

https://www.bilibili.com/video/BV1Wb411Y7Ms?p=1 开发简历页面

```
//过时
// 解决 node-sass 无法下载问题
npm config set sass_binary_site=https://npm.taobao.org/mirrors/node-sass

//推荐
//使用 dart-sass 替代 node-sass, 后者过时了
yarn add sass -D


```

# LESS

Less 是一门 CSS 预处理语言，它扩展了 CSS 语言，增加了变量、Mixin、函数等特性，使 CSS 更易维护和扩展。
Less 可以运行在 Node 或浏览器端。
LESS 做为 CSS 的一种形式的扩展，它并没有阉割 CSS 的功能，而是在现有的 CSS 语法上，添加了很多额外的功能

## 环境配置

```sh
npm install -g less
```

命令行使用:

* 不压缩 `lessc myStyle.less myStyle.css`
* 压缩后输出 `lessc --clean-css styles.less styles.min.css`

代码用法:

```js
var less = require('less');

less.render('.class { width: (1 + 1) }', function (e, output) {// e: 异常, output: 输出
    console.log(output.css);
    // console.log(output.toCSS());
});
```

输出

```css
.class {
    width: 2;
}
```

render方法有更多配置项:

```js
less.render('.class { width: (1 + 1) }',
    {
        paths: ['.', './lib'],  // // Specify search paths for @import directives
        filename: 'style.less', // // Specify a filename, for better error messages
        compress: true          // Minify CSS output
    },
    function (e, output) {
        console.log(output.css);
    });
```

## 浏览器端用法

需要在server下打开, 直接打开无效

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
    <title></title>
    <!-- do not change the order -->
    <link rel="stylesheet/less" type="text/css" href="css/index.less" />
    <!-- download less.js, and import -->
    <script src="lib/less.js" type="text/javascript"></script>
</head>
<body>
	<div id="header">div-header</div>
    <h2>h2 h2 h2</h2>
</body>
</html>
```

编写index.less即可

监视模式: 自动刷新

监视模式是客户端的一个功能，这个功能允许你当你改变样式的时候，客户端将自动刷新。

要使用它，只要在URL后面加上'`#!watch`'，然后刷新页面就可以了。另外，你也可以通过在终端运行`less.watch()`来启动监视模式。

## 语法

### 变量

```less
@nice-blue: #5B83AD;
@light-blue: @nice-blue + #111;

#header { color: @light-blue; }
```

输出

```css
#header { color: #6c94be; }
```

### 混合

#### 不带参数

```less
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}

#menu a {
  color: #111;
  .bordered;
}
.post a {
  color: red;
  .bordered;
}
```

输出

```css
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}

#menu a {
  color: #111;
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
.post a {
  color: red;
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
```

#### 带参数

```less
.border-radius (@radius) {
  border-radius: @radius;
  -moz-border-radius: @radius;
  -webkit-border-radius: @radius;
}

#header {
  .border-radius(4px);
}
.button {
  .border-radius(6px);  
}
```

#### 参数有默认值

```less
.border-radius (@radius: 5px) {
  border-radius: @radius;
  -moz-border-radius: @radius;
  -webkit-border-radius: @radius;
}

#header {
  .border-radius;  // radius值为默认: 5px
}
```

### 暂时隐藏css属性

```less
.wrap () { // 里面的属性不会暴露到css, 只能被内部调用
  text-wrap: wrap;
  white-space: pre-wrap;
  white-space: -moz-pre-wrap;
  word-wrap: break-word;
}

pre { .wrap }
```

### @arguments变量

@arguments包含了所有传递进来的参数. 如果你不想单独处理每一个参数的话就可以像这样写

```less
.box-shadow (@x: 0, @y: 0, @blur: 1px, @color: #000) {
  box-shadow: @arguments;
  -moz-box-shadow: @arguments;
  -webkit-box-shadow: @arguments;
}
.box-shadow(2px, 5px);
```

输出

```css
.box-shadow {
     box-shadow: 2px 5px 1px #000;
  -moz-box-shadow: 2px 5px 1px #000;
  -webkit-box-shadow: 2px 5px 1px #000;
}
```

### 模式匹配(混合中的参数匹配)

只有被匹配的混合才会被使用。变量可以匹配任意的传入值，而变量以外的固定值就仅仅匹配与其相等的传入值。

```less
.mixin (dark, @color) { // 第一个混合定义并未被匹配，因为它只接受dark做为首参
  color: darken(@color, 10%);
}
.mixin (light, @color) { // 第二个混合定义被成功匹配，因为它只接受light
  color: lighten(@color, 10%);
}
.mixin (@_, @color) {// 第三个混合定义被成功匹配，因为它接受任意值
  display: block;
}
.mixin (@param) {...} // 不会匹配, 因为参数个数不对

@switch: light;

.class {
  .mixin(@switch, #888);
}
```

输出

```css
.class {
  color: #a2a2a2;
  display: block;
}
```

### 导引表达式(类似@media query)

#### 单表达式

当我们想根据表达式进行匹配，而非根据值和参数匹配时，导引就显得非常有用

```less
// 导引中表达式符号:  >, >=, =, =<, <, true; 
// 除了true之外的的值都被视为假
// 大于等于, 小于等于, 符号可以位置交换
.mixin (@a) when (lightness(@a) >= 50%) {// lightness是less内置函数
  background-color: black;
}
.mixin (@a) when (lightness(@a) < 50%) {
  background-color: white;
}
.mixin (@a) {
  color: @a;
}

.class1 { .mixin(#ddd) }
.class2 { .mixin(#555) }
```

输出

```css
.class1 {
  background-color: black;
  color: #ddd;
}
.class2 {
  background-color: white;
  color: #555;
}
```

#### 多表达式

[,] 表示 "或"
[and] 表示"与"条件
[not] 表示 "非"

```less
.mixin (@a) when (@a > 10), (@a < -10) { ... }

.mixin (@a) when not (@a < 50%) and not (@a < 5px){
    font-size: 20px;
}
```

### 参数值类型判断(isXXX内置函数)

常见的检测函数:

iscolor：是否为颜色值。
isnumber：是否为数值。
isstring：是否为字符串。
iskeyword：是否为关键字。
isurl：是否为URL字符串。

以下是常见的单位检查函数：

ispixel：是否为像素单位。
ispercentage：是否为百分比。
isem：是否为em单位。
isunit：是否为单位。

```less
.mixin (@a, @b: 0) when (isnumber(@b)) { ... }
.mixin (@a, @b: black) when (iscolor(@b)) { ... }
```

### 嵌套

```less
#header {
  h1 {
    font-size: 26px;
    font-weight: bold;
  }
  p { font-size: 12px;
    a { text-decoration: none;
      &:hover { border-width: 1px }// & 表示串联, 即[a]和[:hover]间没有空格
    }
  }
}
```

输出

```css
#header h1 {
  font-size: 26px;
  font-weight: bold;
}
#header p {
  font-size: 12px;
}
#header p a {
  text-decoration: none;
}
#header p a:hover {
  border-width: 1px;
}

```

### 运算

任何数字、颜色或者变量都可以参与运算.

```less
@base: 5%;
@filler: @base * 2;
width: (@var + 5) * 2;
@other: @base + @filler;
@var: 1px + 5;// 输出6px

color: #888 / 4;
background-color: @base-color + #111;
height: 100% / 2 + @filler;
border: (@width * 2) solid black;
```

### color函数

```less
lighten(@color, 10%);     // return a color which is 10% *lighter* than @color
darken(@color, 10%);      // return a color which is 10% *darker* than @color

saturate(@color, 10%);    // return a color 10% *more* saturated than @color
desaturate(@color, 10%);  // return a color 10% *less* saturated than @color

fadein(@color, 10%);      // return a color 10% *less* transparent than @color
fadeout(@color, 10%);     // return a color 10% *more* transparent than @color
fade(@color, 50%);        // return @color with 50% transparency

spin(@color, 10);         // return a color with a 10 degree larger in hue than @color
spin(@color, -10);        // return a color with a 10 degree smaller hue than @color

mix(@color1, @color2);    // return a mix of @color1 and @color2

hue(@color);        // returns the `hue` channel of @color
saturation(@color); // returns the `saturation` channel of @color
lightness(@color);  // returns the 'lightness' channel of @color

@new: hsl(hue(@old), 45%, 90%);// @new 将会保持 @old的 色调, 但是具有不同的饱和度和亮度.

```

### Math函数

```less
round(1.67); // returns `2`
ceil(2.4);   // returns `3`
floor(2.6);  // returns `2`
percentage(0.5); // returns `50%`
```

### 命名空间

```less
// 先定义好
#bundle {
  .button () {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover { background-color: white }
  }
  .tab { ... }
  .citation { ... }
}

// 使用
#header a {
  color: orange;
  #bundle > .button;
}
```

### 作用域

```less
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}

#footer {
  color: @var; // red  
}
```

### importing引入

```less
// 你可以在main文件中通过下面的形势引入 .less 文件, .less 后缀可带可不带:
@import "lib.less";
@import "lib";
// 如果你想导入一个CSS文件而且不想LESS对它进行处理，只需要使用.css后缀就可以:
@import "lib.css";
```

### 字符串插值

```less
// 变量可以用类似ruby和php的方式嵌入到字符串中，像@{name}这样的结构:
@base-url: "http://assets.fnord.com";
background-image: url("@{base-url}/images/bg.png");
```

### 避免编译

```less
.class {
  filter: ~"ms:alwaysHasItsOwnSyntax.For.Stuff()";
}

// 输出
.class {
  filter: ms:alwaysHasItsOwnSyntax.For.Stuff();
}
```

### JavaScript 表达式

```less
// JavaScript 表达式也可以在.less 文件中使用. 可以通过反引号的方式使用
@var: `"hello".toUpperCase() + '!'`;
@height: `document.body.clientHeight`;// 访问JavaScript环境

// 输出
@var: "HELLO!";
```

# css 模块化

# css 框架

https://github.com/nostalgic-css/NES.css 像素风

https://github.com/papercss/papercss 手写风格

bootstrap 响应式布局框架 (bootstrap-react 库)

https://github.com/jgthms/bulma 响应式, 基于 Flexbox, 类似 Bootstrap

https://github.com/tailwindlabs/tailwindcss 高度自定义

https://github.com/palantir/blueprint react 组件库, 类似 antd

# 响应式页面

- 通过 @media 媒体查询 + flex
- 通过 bootstrap 框架
- 和 react 使用, 可以根据 `window.innerWidth` 渲染不同的组件 (https://juejin.cn/post/6844904089164185607)
