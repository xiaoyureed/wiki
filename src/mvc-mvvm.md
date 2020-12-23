---
title: MVC to MVVM
tags:
  - history
  - mvc
  - mvvm
categories: frontend
date: 2018-04-02 10:45:01
---


<div align="center">
从前端技术演进的角度对mvc等架构模式进行梳理
</div>

<!--more-->

<!-- TOC -->

- [前端架构演变](#前端架构演变)
- [mvc](#mvc)
    - [概念](#概念)
    - [一个简单的前端mvc demo](#一个简单的前端mvc-demo)
    - [设计一个后端mvc框架](#设计一个后端mvc框架)
- [mvp](#mvp)
- [mvvm](#mvvm)

<!-- /TOC -->

# 前端架构演变

MVC（Model-View-Controller）
MVP（Model-View-Presenter）
MVVM（Model-View-ViewModel）

从阶段分为这样三个阶段

*   script, 例如命令行的一些命令
*   页面代码和页面交互逻辑分离
*   mvc, mvp, mvvm; 关注数据的更新界面的更新

<img src="bcc32c459e2f8526826f187b3197c1fb_r.jpg"/>

<img src="83d0c7d0628894e78f8688b2e5ad9bb3_r.jpg"/>

# mvc

## 概念

MVC模式的意思是，软件可以分成三个部分

* 视图（View）：用户界面。
* 控制器（Controller）：业务逻辑
* 模型（Model）：数据保存

各部分通信方式类似这样:

<img src="Screenshot_4.png"/>

所有的通信都是单向的;

前后端中mvc使用的都很多, 但是有差别:([reference](https://www.zhihu.com/question/20440717))

服务端:
客户端发送请求 -> 服务器触发Controller -> 服务器进行Model各种操作 -> 服务器根据Model数据渲染View -> 服务器回复请求，包含了整个View的html -> 客户端重新渲染整个页面，所有的css都又计算了一遍，所有的js都重新执行了一遍，所有的资源都重新请求了一遍（虽然可能已经cache了）

前端:
客户端根据用户的行为修改客户端Model -> 客户端更新和该Model相关的View -> 客户端Model发送sync请求到服务器，只包含改变了哪些数据 -> 服务器审核数据改动是否合法，只需回复是否修改成功 -> 客户端收到成功，什么都不用做；不成功，则把刚才改的View改回来，然后通知用户。（当然，也可以在客户端的Model里面也加入审核机制，这样对不合法数据的反应更快，但还是得保留服务器端的审核）

## 一个简单的前端mvc demo

```html
 <!-- 未使用mvc模式的代码; 页面代码很大的时候，修改js代码还需要查找整个页面，导致整个页面非常混乱。 -->
<HTML>

<HEAD>

    <TITLE> example </TITLE>

</HEAD>

<BODY>

    <input type="button" value="baidu" onclick="alert(this.value);" />

</BODY>

</HTML>

```

```html
<!-- 将html, js脚本进行适当分离; 但是还是需要在html标签中写tipInfo(this)的js调用代码。即html和js仍然有混合 -->
<HTML>

<HEAD>

    <TITLE> example </TITLE>

</HEAD>

<BODY>

    <input type="button" value="baidu" onclick="tipInfo(this);" />

    <SCRIPT LANGUAGE="JavaScript">

        function tipInfo(o) {

            alert(o.value);

        }

    </SCRIPT>

</BODY>

</HTML>
```

js和html彻底分离, 但是还没有使用mvc, model和controller混合在一起

```html
<!-- //  -->
<HTML>

<HEAD>

    <TITLE> example </TITLE>

</HEAD>

<BODY>

    <input type="button" value="baidu" id="baidu" />

</BODY>

<script src="example.js"></script>

</HTML>

```

```js
var o = document.getElementById("baidu");  
  
o.onclick = function(){  

        alert(this.value);  

}  
```

全面的mvc改造:

```html
<!-- V层控制界面显示，将界面与数据连接 -->
<HTML>

<HEAD>

    <TITLE> example </TITLE>

</HEAD>

<BODY>

    <input type="button" value="baidu" id="baidu" />

</BODY>

<script src="model.js"></script>

<script src="controller.js"></script>

</HTML>
```

```js
// 实现html页面节点事件的注册。使得节点的各种事件知道会触发什么函数去执行, 以及页面加载性能的实现
function G(id) {
    return document.getElementById(id);
}
var UI = new Object();
/**
 * id 元素id
 * even 触发事件名称
 * fun 事件触发函数
 * arr 传递的参数
*/
UI.register = function (id, even, fun, arr) {
    if (G(id)) {
        G(id)["on" + even] = function () {
            fun(arr);
        };
    }
}

UI.register("baidu", "click", tipInfo, [1, 2]);
```

```js
// 具体的函数操作(业务逻辑操作)
function tipInfo(arr){  
    alert(arr[1]);  
    //实现其他组件功能，例如popup、form等。都属于model模型  
} 
```

## 设计一个后端mvc框架

设计一个后端mvc框架需要考虑哪些方面呢?

<img src="Screenshot_5.png"/>

借助servlet实现, 看代码:

```java

```


# mvp

在MVC，当需求有变化的时候你需要同时维护三个对象和三个交互，这显然让事情复杂化了。mvp由此诞生; mvp切断了View和Model的联系，让View只和Presenter（原Controller）交互，减少在需求变化中需要维护的对象的数量

MVP定义了Presenter和View之间的接口，让一些可以根据已有的接口协议去各自分别独立开发，以此去解决界面需求变化频繁的问题

<img src="Screenshot_2.png"/>

# mvvm

<img src="Screenshot_3.png"/>

ViewModel大致上就是MVP的Presenter和MVC的Controller了，唯一的区别是，它采用双向绑定（data-binding）：View的变动，自动反映在 ViewModel，反之亦然。Angular 和 Ember 都采用这种模式


