---
title: github-starts
tags: [github, start]
categories: tool
---

<div>
https://gitstar-ranking.com/
https://github.com/explore
https://hellogithub.com/
手动维护一份 github start 项目列表在这里

到 awesome-wechat-weapp webporter

badges: github标签

https://gitter.im/ - 社区交流工具
https://github.com/Terminus2049/Awesome-Social-Impact 社会议题
</div>

<!--more-->

<!-- TOC -->

- [博客](#博客)
- [检索搜索技巧](#检索搜索技巧)
- [文档&资源](#文档资源)
  - [语言](#语言)
    - [go](#go)
    - [node](#node)
    - [java](#java)
    - [rust](#rust)
    - [objectc](#objectc)
  - [分布式](#分布式)
    - [rpc](#rpc)
    - [ELK日志分析](#elk日志分析)
  - [CS基础](#cs基础)
- [动画库](#动画库)
- [工具](#工具)
  - [爬虫](#爬虫)
  - [生成文档](#生成文档)
  - [生成简历](#生成简历)
- [demo](#demo)
- [follow](#follow)

<!-- /TOC -->

# 博客

[camsong / blog](https://github.com/camsong/blog) - 关注 Javascript, ES6, Reat全家桶, css 模块化
[ProtoTeam/blog](https://github.com/ProtoTeam/blog) - 蚂蚁数据体验技术团队的文章仓库, 主要是 ts, 前端工程实践...
[fouber/blog](https://github.com/fouber/blog) - 前端工程实践
[xufei/blog](https://github.com/xufei/blog) - 关注 angularJS
[码农周刊](https://github.com/nemoTyrant/manong) - 官网: https://weekly.manong.io/ , 关注的技术多而杂, 这个项目做了一个分类整理
[唯品会博客](https://github.com/vipshop/vjtools) - 关于Java的一些小家底。
[PopularBlogSites](https://github.com/jxqlovejava/PopularBlogSites) - 作者收集的觉得不错的技术站点或技术博客
[front-end-collect](https://github.com/foru17/front-end-collect) - 分享自己长期关注的前端开发相关的优秀网站、博客、以及活跃开发者
[My-blog](https://github.com/markyun/My-blog) - 前端面试题
[slashhuang/blog](https://github.com/slashhuang/blog) - webpack, react,...
[LearningNotes](https://github.com/francistao/LearningNotes) - 安卓, Android, 数据结构/datastructure
[java-bible](https://github.com/biezhi/java-bible) - java8, mvc, 设计模式...
[wintercn/blog](https://github.com/wintercn/blog)  - 前端, 响应式设计
[guobinhit/cg-blog](https://github.com/guobinhit/cg-blog) - 支付技术

- https://github.com/esofar/cnblogs-theme-silence 博客主题


# 检索搜索技巧

```
# 项目名称中包含 xxx
in:name xxx

# start 数必须大于 1500, fork数必须大于 100, 语言必须为 java, 最后更新时间必须大于 九月三号
in:name xxx starts:>1500 forks:>100 language:java pushed:>2019-09-03

# 在 readme 中搜索
in:readme xxx

# 在 描述中搜索 (试用于用中文搜索)
in:description xxx


```

# 文档&资源

[xiaomi pro -> 黑苹果](https://github.com/daliansky/XiaoMi-Pro/blob/master/README-CN.md)
[vim-galore-zh_cn](https://github.com/wsdjeg/vim-galore-zh_cn) - Vim galore 中文翻译，构建 Vim 知识体系
[use_vim_as_ide](https://github.com/yangyangwithgnu/use_vim_as_ide) - vim 配置
[es6tutorial](https://github.com/ruanyf/es6tutorial) - 介绍 ECMAScript 6 新增的语法特性。
[javascript-tutorial](https://github.com/wangdoc/javascript-tutorial)  - js教程
[leetcode](https://github.com/haoel/leetcode) - LeetCode Problems' Solutions
[algorithm-exercise](https://github.com/billryan/algorithm-exercise) - Data Structure and Algorithm notes. 数据结构与算法/leetcode/lintcode题解/ https://algorithm.yuanbin.me
[awesome-java-leetcode](https://github.com/Blankj/awesome-java-leetcode) - leetcode, java实现
[awesome-wechat-weapp](https://github.com/justjavac/awesome-wechat-weapp)
[Note](https://github.com/scalad/Note) - 常规Java工具，算法，加密，数据库，面试题，源代码分析，解决方案
[七月在线，面试算法，数据结构，机器学习](https://github.com/julycoding/The-Art-Of-Programming-By-July)
[awesome-programming-books](https://github.com/jobbole/awesome-programming-books) - 经典编程书籍大全，涵盖：计算机系统与网络、系统架构、算法与数据结构、前端开发、后端开发、移动开发、数据库、测试、项目与团队、程序员职业修炼、求职面试等
[restful-api-design-references](https://github.com/aisuhua/restful-api-design-references) - RESTful API 设计参考文献列表，可帮助你更加彻底的了解REST风格的接口设计
[awesome-react](https://github.com/enaqx/awesome-react) - react 相关资源
[reactjs101](https://github.com/carlleton/reactjs101) - react 教程
[react-native-open-project](https://github.com/MarnoDev/react-native-open-project) - 作者收集的 React Native 优秀开源项目大全
[You-Dont-Need-jQuery](https://github.com/nefe/You-Dont-Need-jQuery) - Examples of how to do query, style, dom, ajax, event etc like jQuery with plain javascript.
[airbnb/javascript](https://github.com/airbnb/javascript) - airbnb 的js规范
[vue-tutorials](https://github.com/keepfool/vue-tutorials) - Let you insight into the Vue.js
[500 Lines or Less](https://github.com/aosabook/500lines) - This is the source for the book 500 Lines or Less, [中文翻译](https://github.com/HT524/500LineorLess_CN)
[You-need-to-know-css](https://github.com/l-hammer/You-need-to-know-css) - 各种css  效果 [ref]](https://lhammer.cn/You-need-to-know-css/#/)
[30-seconds-of-css](https://github.com/atomiks/30-seconds-of-css) - 常用的 css 片段
[怎么编写 jq 插件](http://i5ting.github.io/How-to-write-jQuery-plugin/build/jquery.plugin.html#106)
[HelloGitHub](https://github.com/521xueweihan/HelloGitHub) - 分享 GitHub 上有趣、入门级的开源项目
[better-Java](http://link) - 关于java的周边, 技术栈
[awesome-java-cn](https://github.com/jobbole/awesome-java-cn) - Java资源大全中文版，包括开发库、开发工具、网站、博客、微信、微博等，由伯乐在线持续更新
[bootswatch](https://github.com/thomaspark/bootswatch) - 多种 bootstrap 样式
[N-blog](https://github.com/nswbmw/N-blog) - 《一起学 Node.js》
[AiLearning](https://github.com/apachecn/AiLearning) - AiLearning: 机器学习 - MachineLearning - ML、深度学习 - DeepLearning - DL、自然语言处理 NLP http://ml.apachecn.org/mlia , 这个团体 有很多 机器学习方面的工具中文文档翻译
[Awesome-Chinese-NLP](https://github.com/crownpku/Awesome-Chinese-NLP) - A curated list of resources for Chinese NLP 中文自然语言处理相关资料
[zhao](https://github.com/programthink/zhao) - 太子党关系网络
[crash-course-computer-science-chinese](https://github.com/1c7/crash-course-computer-science-chinese) - 计算机速成课 | Crash Course 字幕组 (全40集 2018-5-1 精校完成)
[awesome-scalability](https://github.com/binhnguyennus/awesome-scalability) - 高可扩展性，高可用性，高稳定性，高性能和高智能系统设计
[system-design-primer](https://github.com/donnemartin/system-design-primer)  - 怎样设计大规模系统。
[build-your-own-x](https://github.com/danistefanovic/build-your-own-x) - 构建你自己的xxx
[architecture.of.internet-product](https://github.com/davideuler/architecture.of.internet-product) - 互联网公司技术架构，微信/淘宝/微博/腾讯/阿里/美团点评/百度/Google/Facebook/Amazon/eBay的架构
[be-a-professional-programmer](https://github.com/stanzhai/be-a-professional-programmer) - 各种优秀资料、神器及框架
[tenant-point](https://github.com/soulteary/tenant-point) - 租房要点，适用于北上广深杭
[architect-awesome](https://github.com/xingshaocheng/architect-awesome)  - 后端架构师技术图谱
[HTML5](https://github.com/NalvyBoo/HTML5) - HTML5学习、总结、实践
[Web](https://github.com/smyhvae/Web) - 前端入门和进阶
[Linux-Tutorial](https://github.com/judasn/Linux-Tutorial) - 《Java 程序员眼中的 Linux》
[github](https://github.com/phodal/github) - GitHub 漫游指南
[growth-in-action](https://github.com/phodal/growth-in-action) - 全栈增长工程师实战 http://growth-in-action.phodal.com/ , python, Django 及其他。作者的其他电子书也非常好
[2017_wuhan_house_experience](https://github.com/itbirds/2017_wuhan_house_experience) - 2017年武汉买房经历
[CS-Notes](https://github.com/CyC2018/CS-Notes) - Computer Science Learning Notes
[awesome-blockchain-cn](https://github.com/chaozh/awesome-blockchain-cn) - 收集所有区块链(BlockChain)技术开发相关资料，包括Fabric和Ethereum开发资料
[typescript-tutorial](https://github.com/xcatliu/typescript-tutorial) - ts 入门
[30-seconds-of-java8](https://github.com/biezhi/30-seconds-of-java8) - java8 新特性
[30-seconds-of-code](https://github.com/Chalarangelo/30-seconds-of-code) - Curated collection of useful Javascript snippets that you can understand in 30 seconds or less. 有用的 js 片段
[java-library-examples](https://github.com/biezhi/java-library-examples)  - java各种组件的 demo
[FE-Cookbook](https://github.com/hijiangtao/FE-Cookbook) - HTML/CSS & JavaScript, Web and related. 前端手册
[interview](https://github.com/HIT-Alibaba/interview) - 笔试面试知识整理, 移动开发相关, 网络tcp
[datastructure](https://github.com/buptdavid/datastructure) - 用Java实现的数据结构暨算法，同时对其进行了测试
[Android_Data](https://github.com/Freelander/Android_Data) - 安卓学习材料, Some Android learning materials, hoping to help you learn Android development.
[free-programming-books](https://github.com/EbookFoundation/free-programming-books) - 免费编程书籍
[You-Dont-Know-JS](https://github.com/getify/You-Dont-Know-JS) - A book series on JavaScript. @YDKJS on twitter.
[what-happens-when](https://github.com/alex/what-happens-when) - 当...时候发生了什么.
[kdn251/interviews](https://github.com/kdn251/interviews) - Everything you need to know to get the job. Java 大集合。
[ResumeSample](https://github.com/geekcompany/ResumeSample) - 简历模板
[git-recipes](https://github.com/geeeeeeeeek/git-recipes) - 一份非常好的 git 教程
[linux-command](https://github.com/jaywcjlove/linux-command) - Linux命令大全搜索工具，内容包含Linux命令手册、详解、学习、搜集。https://git.io/linux
[the_new_world_linux](https://github.com/yangyangwithgnu/the_new_world_linux) - Linux各种服务配置
[translations](https://github.com/oldratlee/translations) - 一些不错英文资料的中文翻译, 并发, 分布式, 系统设计,  java
[developer](https://github.com/phodal/developer) - 工程师成长路线图.
[gitignore](https://github.com/github/gitignore) - 各种 gitignore
[JacksonTian/fks](https://github.com/JacksonTian/fks) - 前端技能汇总
[gold-miner](https://github.com/xitu/gold-miner) - 掘金翻译计划， 偏前端。
[free book](https://github.com/justjavac/free-programming-books-zh_CN) - 免费的书籍, 英文版在 这里: https://github.com/EbookFoundation/free-programming-books
[nginx指南](https://github.com/xuexb/learn-nginx)
[Qix](https://github.com/ty4z2008/Qix) - 分布式，go，机器学习

## 语言

### go

go 语言进阶教程: https://github.com/chai2010/advanced-go-programming-book ; 基础教程: https://github.com/golang-china/gopl-zh (go 语言圣经)
go 入门指南: https://github.com/Unknwon/the-way-to-go_ZH_CN ; 
go 学习笔记: https://github.com/qyuhen/book
go教程(中文翻译): https://github.com/astaxie/build-web-application-with-golang/blob/master/zh/preface.md
go 资源帖: https://zhuanlan.zhihu.com/p/30350493
go实现的文档管理系统： https://github.com/lifei6671/mindoc

### node

node调试指南: https://github.com/nswbmw/node-in-debugging
node 最佳实践: https://github.com/i0natan/nodebestpractices

### java

Mybatis 指南: https://github.com/mybatis-book/book
Java 技术栈书籍: https://github.com/woooowen/iBooks
springmvc代码分析: https://github.com/fangjian0423/springmvc-source-minibook
java面试指南：https://github.com/Snailclimb/JavaGuide
maven插件：https://github.com/hzdavid/findConflicts 查找各种冲突，版本冲突

### rust

https://github.com/rust-lang/book


### objectc 

https://github.com/dongyuwei


## 分布式

### rpc

[lu-rpc](https://github.com/stateIs0/Lu-Rpc) - rpc demo

### ELK日志分析

[搭建日志分析平台](https://github.com/ameizi/ELK)

[ELK stack](https://github.com/chenryn/ELKstack-guide-cn) - ELK技术栈入门， 日志处理

## CS基础

tcp, udp详解: https://github.com/JerryC8080/understand-tcp-udp
全栈: https://github.com/phodal/growth-ebook
Java面试: https://zhuanlan.zhihu.com/p/76215980


# 动画库

[popmotion](https://github.com/Popmotion/popmotion) - 动画库, 支持 react
[animate.css](https://github.com/daneden/animate.css) - A cross-browser library of CSS animations

# 工具

[react-webpack-babel](https://github.com/ReactJSResources/react-webpack-babel) - Simple React Webpack Babel Starter Kit
[Microsoft / TypeScript-React-Starter](https://github.com/Microsoft/TypeScript-React-Starter) -  TypeScript and React 模板
[springside4](https://github.com/springside/springside4) - SpringSide是以Spring Framework为核心的，Pragmatic风格的JavaEE应用参考示例，是JavaEE世界中的主流技术选型，最佳实践的总结与演示。
[reveal.js](https://github.com/hakimel/reveal.js)  - The HTML Presentation Framework, 可以实现在web页面中播放幻灯片
[remark](https://github.com/gnab/remark) - 同上, http://remarkjs.com
[i5ting_ztree_toc](https://github.com/i5ting/i5ting_ztree_toc) - 一个jq插件, 可以预览 markdown 内容的目录, 还有 node ruby 等版本
[remote-working](https://github.com/greatghoul/remote-working) - 收集整理远程工作相关的资料 https://remote-china.info/
[taro](https://github.com/NervJS/taro) - 使用 react 开发 小程序, h5...
[flexmark-java](https://github.com/vsch/flexmark-java) - Markdown Java parser. With HTML to MD, MD to PDF, MD to DOCX conversion modules
[awesome-adb](https://github.com/mzlogin/awesome-adb) - ADB Usage Complete / ADB 用法大全
[HanLP](https://github.com/hankcs/HanLP) - HanLP是一系列模型与算法组成的NLP工具包，由大快搜索主导并完全开源，目标是普及自然语言处理在生产环境中的应用。

[awesome-comment](https://github.com/Blankj/awesome-comment) - 神级注释
[hosts](https://github.com/racaljk/hosts) - 最新可用的google hosts文件; update: 目前废弃, 新项目: https://github.com/googlehosts/hosts
[sentry](https://github.com/getsentry/sentry)  - Sentry是跨平台应用监控程序，专注于错误报告。
[error-prone](https://github.com/google/error-prone) - Java的静态分析工具，可以在编译时捕获常见的编程错误
[NullAway](https://github.com/uber/NullAway) - 帮助消除Java代码中的NullPointerExceptions（NPE）的工具，具有较低的构建时间开销
[ShadowSocks-Share](https://github.com/zc-zh-001/ShadowSocks-Share) - ss分享
[ShadowSocksShare-OpenShift](https://github.com/the0demiurge/ShadowSocksShare-OpenShift) - ss分享
[shadowsocks-android](https://github.com/shadowsocks/shadowsocks-android) - ss的 Android端
[shadowsocks-windows](https://github.com/shadowsocks/shadowsocks-windows) - ss 的 Windows 端
[new-pac](https://github.com/Alvin9999/new-pac) - 自由上网方法
[truth/wiki](https://github.com/comeforu2012/truth/wiki) - 同上
[new-vpn](https://github.com/txthinking/brook) - vpn
[XX-Net](https://github.com/XX-net/XX-Net) - 一个web代理工具。
[frontend-maven-plugin](https://github.com/eirslett/frontend-maven-plugin) - 前台开发maven插件, 此插件为项目本地下载/安装Node和NPM，运行npm install，然后运行Bower，Grunt，Gulp，Jspm，Karma或Webpack的任意组合
[polyglot-maven](https://github.com/takari/polyglot-maven) - 换种方言来写 maven pom.xml
[zmirror](https://github.com/aploium/zmirror) - 一个Python反向HTTP代理程序, 用于快速、简单地创建别的网站的镜像, 自带本地文件缓存、CDN支持
比如国内可以访问的Google镜像/中文维基镜像
[faker.js](https://github.com/Marak/faker.js) - 在Node.js和浏览器中生成大量虚假数据
[json-server](https://github.com/typicode/json-server) - 一个极简server, 提供 restful api
[pretender](https://github.com/pretenderjs/pretender) - a mock server library
[RAP](https://github.com/thx/RAP) - Web接口管理工具，开源免费，接口自动化，MOCK数据自动生成，自动化测试，企业级管理
[listen1_chrome_extension](https://github.com/listen1/listen1_chrome_extension) - chrome拓展， 聚合多家平台音乐资源
[BaiduExporter](https://github.com/acgotaku/BaiduExporter) - Assistant for Baidu to export download links to aria2/aria2-rpc
[webui-aria2](https://github.com/ziahamza/webui-aria2) - aria2/aria2-rpc 的web 界面
[aria2](https://github.com/aria2/aria2) - 一个下载工具， 应对百度云限速特别好使
[mean](https://github.com/linnovate/mean) - The MEAN stack uses Mongo, Express, Angular(4) and Node for simple and scalable fullstack js applications http://mean.io
[flutter](https://github.com/flutter/flutter) - Flutter makes it easy and fast to build beautiful mobile apps.
[JSON-js](https://github.com/douglascrockford/JSON-js) - The files in this collection implement JSON encoders/decoders in JavaScript. 在序列化时候，可以去除js对象中的循环引用
[chinese-poetry](https://github.com/chinese-poetry/chinese-poetry) - 最全中华古诗词数据库, 唐宋两朝近一万四千古诗人, 接近5.5万首唐诗加26万宋诗. 两宋时期1564位词人，21050首词。
[craft-atom](https://github.com/mindwind/craft-atom) - Java原子组件库
[mapdb](https://github.com/jankotek/mapdb) - MapDB提供由磁盘存储或堆外内存支持的并发映射，集和队列。它是一种快速且易于使用的嵌入式Java数据库引擎。
[jetbrick-template-2x](https://github.com/subchen/jetbrick-template-2x) - 新一代 Java 模板引擎, 可替代 JSP 页面或者 Velocity 等模板。 指令和 Velocity 相似，表达式和 Java 保持一致
[java-repl](https://github.com/albertlatacz/java-repl) - repl for java, Read Eval Print Loop for Java
[flexText](https://github.com/alexdunphy/flexText) - 自动拓展高度的 textarea
[SQLAdvisor](https://github.com/Meituan-Dianping/SQLAdvisor) - 输入SQL，输出索引优化建议
[blade](https://github.com/lets-blade/blade) - Lightning fast and elegant mvc framework for Java8
[lunr.js](https://github.com/olivernn/lunr.js) - A bit like Solr, but much smaller and not as bright , 搜索 js 库
[ant-design](https://github.com/ant-design/ant-design) - antd, react ui库
[timeago.js](https://github.com/hustcc/timeago.js) - timeago.js is a tiny(~2.0kb) library used to format date with `*** time ago` statement. eg: '3 hours ago'. No dependency & localization & tiny.
[grice](https://github.com/biezhi/grice) - A simple web document engine written by java. 文档引擎.
[docsify](https://github.com/docsifyjs/docsify) - A magical documentation site generator.
[jsonschema2pojo](https://github.com/joelittlejohn/jsonschema2pojo) - Generate Java types from JSON or JSON Schema and annotates those types for data-binding with Jackson, Gson, etc
[owner](https://github.com/lviggiano/owner) - Get rid of the boilerplate code in properties based configuration. 接入配置文件的最简方法
[octotree](https://github.com/buunguyen/octotree) - chrome拓展, github好搭档, 生成目录树
[thefuck](https://github.com/nvbn/thefuck) - orrects your previous console command.
[localtunnel](https://github.com/localtunnel/localtunnel) - 暴露 localhost 给外网
[lanproxy](https://github.com/ffay/lanproxy) - lanproxy是一个将局域网个人电脑、服务器代理到公网的内网穿透工具
[weixin-java-tools](https://github.com/Wechat-Group/weixin-java-tools) - 全能微信Java开发工具包，支持包括微信支付、开放平台、小程序、企业号和公众号等的开发
[webmagic](https://github.com/code4craft/webmagic) - java爬虫, 国产, 一个应用 -> https://github.com/brianway/webporter

download


[proxyee-down](https://github.com/proxyee-down-org/proxyee-down) - http下载工具，基于http代理，支持多连接分块下载
[you-get](https://github.com/soimort/you-get) - 下载器, YouTube下载神器
[youtube-dl](https://github.com/rg3/youtube-dl) - YouTube 命令行下载
[annie](https://github.com/iawia002/annie)
[you2php](https://github.com/You2php/you2php) - 搭建Youtube视频镜像站, php实现, 而php空间贼便宜, 甚至有很多免费的php空间 (国内的肯定不行, 需要找国外的空间服务商); 实现了视频/频道搜索、视频在线播放 、频道/类别内容预览、视频下载等功能

## 爬虫

nodejs爬虫: https://github.com/bda-research/node-crawler

pornhub python: https://github.com/formateddd/pornhub

## 生成文档

[Gatsby](https://www.gatsbyjs.org/) - 通过 react 生成静态网站
[vuePress](https://vuepress.vuejs.org/) - 通过 vue 生成静态网站

## 生成简历

https://github.com/DIYgod/Resume

# demo

[paascloud-master](https://github.com/paascloud/paascloud-master) - spring cloud + vue + oAuth2.0全家桶实战，前后端分离模拟商城，完整的购物流程、后端运营平台，可以实现快速搭建企业级微服务项目。支持微信登录等三方登录。
[PiggyMetrics](https://github.com/sqshq/PiggyMetrics) -  Spring Boot, Spring Cloud and Docker 结合 实现微服务架构 一个记账app 英文
[lepus](https://github.com/Georce/lepus) - 企业级数据库监控系统,MySQL/Oracle/MongoDB/Redis一站式性能监控
[favorites-web](https://github.com/cloudfavorites/favorites-web) - 云收藏 Spring Boot 2.0 开源项目
[react-pxq](https://github.com/bailicangdu/react-pxq) - 一个 react + redux 的完整项目 和 个人总结
[30-days-of-react-native](https://github.com/fangwei716/30-days-of-react-native) - 30 days of React Native demos
[oh-my-session](https://github.com/biezhi/oh-my-session) - distributed session storage scheme, using redis to store data.
[vue2-happyfri](https://github.com/bailicangdu/vue2-happyfri) - vue2 + vue-router + vuex 入门项目
[react-redux-chat](https://github.com/meibin08/react-redux-chat) - react+redux-chat 模仿实现PC微信聊天
[Distributed_spider_pku_java](https://github.com/PkuJavaGroupCzz/Distributed_spider_pku_java) - 分布式 Java 爬虫
[SSM](https://github.com/crossoverJie/SSM) - SSM, 微服务
[极简ssm](https://github.com/liyifeng1994/ssm) - 整合最优雅SSM框架：SpringMVC + Spring + MyBatis, 进一步完善(页面分离+Nginx负载均衡+Tomcat集群 -> https://github.com/wosyingjun/beauty_ssm_cluster)
[My-Blog](https://github.com/ZHENFENG13/My-Blog) - 由Docker+SpringBoot+Mybatis+thymeleaf等技术实现的Java博客系统
[any-video](https://github.com/ChinaSilence/any-video) - 基于 Spring Boot 开发的应用，利用网络爬虫技术，实时解析互联网资源，可对所有资源分类收藏，并分享
[hxyFrame](https://github.com/huangxianyuan/hxyFrame) - 一个OA办公系统，采用流行的框架springMvc+spring+mybatis+shiro+ehcache开发,还集成了权限管理（菜单权限、数据权限），完善的代码生成器，solr全文搜索引擎，activiti工作流程引擎，cas单点登陆等功能，后期还会考虑改造成Dubbo微服务化,做到模块的相对独立，使用更加灵活
[xxl-job](https://github.com/xuxueli/xxl-job) - 分布式任务调度平台
[jfinal-api-scaffold](https://github.com/kevin09002/jfinal-api-scaffold) - 为APP开发HTTP API应用的脚手架模板，使用Java编写
[es](https://github.com/zhangkaitao/es) - JavaEE项目开发脚手架
[solo](https://github.com/b3log/solo) -  一款小而美的 Java 博客系统。
[bugCatcher](https://github.com/youzan/bugCatcher) - 方便产品、开发、测试三方协同管理、测试、监控项目进度和质量，以持续交付
[framework-free-single-page-app](https://github.com/coderLMN/framework-free-single-page-app) - 无框架单页面web应用的一种技术实现和演示
[PLMCodeTemplate](https://github.com/xwjie/PLMCodeTemplate) - 给部门制定的代码框架模板
[JDeploy](https://github.com/wucao/JDeploy) - 自动部署平台
[symphony](https://github.com/b3log/symphony) -  A modern community (forum/BBS/SNS/blog) platform written in Java. 一个用 Java 实现的现代化社区（论坛/BBS/社交网络/博客）平台。https://hacpai.com
[shopping-management-system](https://github.com/zhanglei-workspace/shopping-management-system) - 网购管理系统, 多个小项目合集.

# follow

https://github.com/wxyyxc1992 - 前端, python, web开发, 多本 ebook作者
https://github.com/hjzgg - Java, 有很多有用的小demo
https://github.com/kenberkeley - react, vue, redux
https://github.com/bailicangdu - react, vue, 多个完整demo
https://github.com/kenberkeley - ssm, leetcode, shiro
https://github.com/doyoe - css, 移动前端开发经验指南
https://github.com/yangyangwithgnu - linux, vim, hardseed(💊 )
https://github.com/i5ting - node.js
https://github.com/ScienJus - 服务端开发, spring-authorization
https://github.com/wzjgn - springcloud, 爬虫
https://github.com/crossoverJie - netty, java core tips
https://github.com/monkeyWie - proxyee-down 作者
https://github.com/xuxueli - 分布式 任务调度系统/爬虫/api管理/单点登陆
https://github.com/judasn - IDEA, Linux
https://github.com/sindresorhus - node, js test
https://github.com/addyosmani - todomvc, material-design-lite...
https://github.com/aviaryan - vscode, ahk, python
https://github.com/brianway - java, spring, springmvc
https://github.com/Trinea - Android
https://github.com/WuXiaolong - Android
https://github.com/kaedea - Android, hexo主题
https://github.com/DIYgod - 前端, 多款开源
https://github.com/typicode - json-server作者
https://github.com/ityouknow - springboot, springcloud, 微服务
https://github.com/coderLMN - 前端, 无框架单页面web应用的一种技术实现和演示
https://github.com/xiaolongzuo - java, 多个小工具
https://github.com/yeasy - Docker
https://github.com/jaywcjlove - Linux, 效率工具
https://github.com/hzlzh - app, 硬件, 周边
https://github.com/chloerei - ruby
https://github.com/huacnlee - ruby
https://github.com/phodal - 前端, 多本ebook作者
https://github.com/lepture - flask作者
https://github.com/fouber - 前端工程实践
https://github.com/ChenYilong - ios app
https://github.com/ibireme - ios
https://github.com/wintercn - 前端
https://github.com/justjavac - 前端, php
https://github.com/RubyLouvre - 前端, 移动开发
https://github.com/stormzhang - Android
https://github.com/sofish  - 前端
https://github.com/yyx990803 - vue作者
https://github.com/laruence - php
https://github.com/michaelliao - 廖雪峰, python, js
https://github.com/cloudwu - 云风, c
https://github.com/JacksonTian - 前端
https://github.com/daimajia - Android
https://github.com/biezhi - java, 规范
https://github.com/ruanyf - 阮一峰老师, js, es6tutorial, node, react
https://github.com/bannedbook - cross the gfw
https://github.com/out0fmemory - goagent
https://github.com/ouyangzhiping?tab=stars - start的小项目非常多, 侧重数据分析
https://github.com/woooowen - 阿里面试
https://github.com/astaxie - go 
https://github.com/Unknwon - go 
https://github.com/huichen - go
https://github.com/vaporz - go 小作品
https://github.com/huangz1990 - redis 
https://github.com/codefollower - java
https://github.com/BBcaptain - 涉猎广泛， 新鲜的工具
https://github.com/yidao620c - python, 算法
https://github.com/beyon
dyuefei - java ， java类小工具
https://github.com/RitterHou - 参考
https://github.com/dhh - ruby作者
https://github.com/lifesinger - 前端老鸟，阿里大神，博客
https://github.com/lingyv - 关注着很多Java大牛，作者很普通 //todo
https://github.com/cloudsthunder?tab=stars - 空的, 但是又start了很多黑科技
https://github.com/digoal/blog - PostgreSQL专家
https://github.com/crossoverJie - java 专家
https://github.com/aalansehaiyang - java
https://github.com/mqyqingfeng - 淘宝前端大神(技术博客, js系列: https://github.com/mqyqingfeng/Blog)
https://github.com/aa65535 - 下载, 路由器 OpenWrt/LEDE
https://github.com/binux - 同上, python 爬虫
https://github.com/fengjiachun - 分布式java
https://github.com/eisneim - 特里
https://github.com/RubyLouvre - 司徒正美 JavaScript , 去世