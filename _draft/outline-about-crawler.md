---
title: crawler
tags: [crawler]
categories: outline
---

<div align="center">
java实现的 分布式爬虫:https://github.com/xpleaf/ispider, 

[zhuhu一个问题](https://www.zhihu.com/question/27850529)

[Java 分布式爬虫系统](https://www.cnblogs.com/davidwang456/articles/9143136.html) 
https://github.com/wycm/zhihu-crawler 知乎爬虫

TODO

[设计爬虫框架](https://blog.csdn.net/qq_42677001/article/details/95625920)

https://github.com/code4craft java 爬虫国产作者

https://juejin.im/post/6844903697047257101 总结

https://github.com/facert/awesome-spider

https://github.com/microsoft/playwright-python
</div>

<!--more-->

<!-- TOC -->

- [主要步骤](#主要步骤)
- [技术选型](#技术选型)
- [Puppeteer](#puppeteer)
    - [和 selenium 区别](#和-selenium-区别)
    - [使用](#使用)
- [phantomjs](#phantomjs)
- [抓包工具](#抓包工具)
    - [fiddler](#fiddler)
    - [Charles](#charles)
    - [wireshark](#wireshark)

<!-- /TOC -->

# 主要步骤

1. 分析登录请求，分析工具可以有IE自带(快捷键F12)、Fiddler、HttpWatcher；

1. 编写代码模拟登录的过程。

1. 网页下载

    最好先把网站的页面抓取到本地电脑上，后面再来慢慢解析

    对于分页的数据, 每个分页使用一个 thread

1. 网页解析和提取

    可以使用 regex

    使用 httpclient, jsoup, HtmlUnit

1. 数据清洗, 去重 （redis 布隆过滤器）

1. 存储

    mongodb, hbase, MySQL

1. 对抗反爬

    代理IP

    对单个代理IP进行发起HTTP请求的速度控制，两秒一次

    做DNS缓存 (免去DNS查询的时间)

# 技术选型

- HtmlUnit是一个无界面浏览器Java程序, 根据配置的不同模拟Chrome、Firefox或Internet Explorer等浏览器。

    对于一个静态的html页面来说，使用httpClient足够。但是对于现在越来越多的动态网页来说，数据是通过异步JS代码获取并渲染到的，最开始的html页面是不包含这部分数据的。 

    但是本质上是自动化测试工具, 不是用来爬数据的, 页面兼容性不好

- phantomjs 爬取异步加载数据, 坑太多, 推荐 Puppeteer

    需要编写 js 代码 来使用 phantomjs

    改进: Selenium webdriver + phantomjs, 对java程序员来说只操作webdriver就可以了，不需要写js代码

    https://www.jianshu.com/p/96220e239c35

- Jsoup 连接 url, 形成 Document, 解析 xml


# Puppeteer 

Headless Chrome Node.js API 无界面浏览器, 完全可以替换 selenium / phantomjs

https://github.com/puppeteer/puppeteer


## 和 selenium 区别

puppeteer 仅仅支持 js, webdriver 是一个大而全的解决方案，可以用 C#， Java， JS， Python，Ruby开发，支持 IE，FireFox， Safari，Chrome，andriod Chrome, 等等

同样是实现针对 Chrome的测试，webdriver 需要通过调用 chromedriver 提供的 Chrome Remote Debug Protocol， 再封装一层保持接口统一. puppeteer 则是直接和 chrome 建立 websocket 链接，通过 msg， 直接发送 Protocol 让 chrome 来执行对应的操作。

如果不考虑兼容性，puppeteer 可以带来更好的性能（少了一层调用的原因），更多的功能，以及 Chrome 团队直接的支持

## 使用

https://zhuanlan.zhihu.com/p/76237595 - 使用

TODO


# phantomjs

https://github.com/ariya/phantomjs

停止维护了, 


# 抓包工具

## fiddler

Win 下好用的抓包工具------ https://www.telerik.com/download/fiddler

能够抓 http, https(需要配置证书)

[https的 decrypt, 手机 app 的抓包 图文详解](https://www.cnblogs.com/yyhh/p/5140852.html)
[itchat: 利用 fiddler 抓包 网页微信](https://itchat.readthedocs.io/zh/latest/tutorial/tutorial1/)
[fiddler 抓包的原理](http://mouxuejie.com/blog/2017-03-19/capture-package-principle/)

https://www.cnblogs.com/zdz8207/p/qil-Fiddler4.html


## Charles

macos 下简单的工具, 适用于 移动端抓包


## wireshark

重量级工具, macos, win 支持

适用于 tcp, udp, 无法 https
