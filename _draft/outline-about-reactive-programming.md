---
title: 响应式编程模式
tags: [reactive]
categories: java web
---

RxJava: https://github.com/ReactiveX/RxJava, 是一个通过观察者模式实现的, Java平台的响应式编程库
rxjs: https://github.com/ReactiveX/rxjs
RxJava最早就是Netflix为了后端而开发的, 但是在Android上火起来了;
reference: [1](http://wiki.jikexueyuan.com/project/android-weekly/issue-145/introduction-to-RP.html), [2](https://zhuanlan.zhihu.com/p/27678951), [3](https://zhuanlan.zhihu.com/p/23584382), [4](https://zhuanlan.zhihu.com/p/20380883)
Project Reactor: http://springcloud.cn/view/366

https://github.com/hs-web/hsweb-framework 基于spring-boot 2.x开发 ,首个使用全响应式编程的企业级后台管理系统基础项目
<!--more-->

<!-- TOC -->

- [由来](#由来)
- [Rxjava](#rxjava)
- [Rxjs](#rxjs)
- [webflux](#webflux)
  - [what is webflux](#what-is-webflux)
  - [为什么使用](#为什么使用)
  - [how to use](#how-to-use)

<!-- /TOC -->

# 由来

非响应式是这样的:

```java
int a=1;
int b=a+1; // 简单的赋值
System.out.print(“b=”+b)    //  b=2
a=10;
System.out.print(“b=”+b)    //  b=2

```

如果我们想表达的并不是一个赋值动作，而是b和a之间的关系,即无论a如何变化，b永远比a大1, 就需要花额外的精力去构建和维护一个b和a的关系

```java
int a=1;
int b <= a+1;   // <= 符号只是表示a和b之间关系的操作符
System.out.print(“b=”+b)    //  b=2
a=10;
System.out.print(“b=”+b)    //  b=11
```

这就是是响应式的思想，它希望有某种方式能够构建关系，而不是执行某种赋值命令

响应式编程是一种通过异步和数据流来构建事物关系的编程模型, 可以类比 react 中的 数据单向绑定

# Rxjava

 底层原理就是: 观察者模式和异步

RxJava中存在 被观察者(Observable), 观察者(Observer)和 操作符 ; 构建被观察者（Observable），观察者（Observer/Subscriber），然后建立二者的订阅关系

Observer是观察者的接口， Subscriber是实现这个接口的抽象类,因此两个类都可以被当做观察者，由于Subscriber在Observe的基础上做了一些拓展，加入了新的方法，一般会更加倾向于使用Subscriber。

(rxjava在android开发更流行, 是因为 UI 开发对异步流数据处理有需求, 大部分的java后台开发不涉及异步或者异步用message queue所替代)

当android处理复杂的网络请求，（比如接口之间要串行，接口返回结果要对应不同的UI显示），异步IO，等种种情况时，写出来的代码往往只能用一坨来形容。Rxjava则可以很好的有效把让这些代码逻辑整理清晰，解决回调地狱的问题。

```xml
<dependency>
    <groupId>io.reactivex.rxjava2</groupId>
    <artifactId>rxjava</artifactId>
    <version>2.0.4</version>
</dependency>
```

# Rxjs


# webflux

## what is webflux

https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/web-reactive.html#webflux

webflux在web开发中的位置等同springmvc, 可以对比理解:

- springmvc 基于 servlet, 使用同步阻塞 i/o 模型, 每个请求过来都对应于一个thread, 在请求处理完成前, 这个thread是被完全占用的, 而 webflux 使用的是异步非阻塞 i/o, 适合 io密集的应用 (如微服务网关, 网络io密集)
- 以 Reactor 库为基础, 基于异步和事件驱动, 提升系统吞吐量和伸缩性, 并不提升接口的请求响应时间
  - 吞吐量: 一次请求处理过程, 传输的数据总量

## 为什么使用

或者说使用webflux 会带来什么好处呢

- 提升系统吞吐量, 伸缩性


## how to use

https://blog.csdn.net/suchahaerkang/article/details/90645760
https://blog.csdn.net/qq_15144655/article/details/80708915
https://www.ibm.com/developerworks/cn/java/spring5-webflux-reactive/index.html

