---
title: 移动端技术选型
tags:
---


https://github.com/didi/DoraemonKit 移动端 app 测试工具包

<!-- more -->

<!-- TOC -->

- [移动端选型](#移动端选型)
  - [原生 native](#原生-native)
  - [react native](#react-native)
  - [flutter](#flutter)
  - [h5 hybird 混合开发](#h5-hybird-混合开发)
  - [web app](#web-app)

<!-- /TOC -->

# 移动端选型

## 原生 native

如android、ios、wp

优点: 体验最好, 功能最强

缺点:

- 开发成本高, 无法跨平台

- 上线时间不稳定，需要审核，特别是苹果审核机制，审核时间长短不一，对内容还有控制


## react native

使用react-native可以维护多种平台（Web，Android和IOS）, 原理是将 js 转为 平台对应的组件

react-native的优点：不用webview,摆脱了webview的交互和性能问题；有较强的扩展性，java端提供了基础控件，js可以自由组合使用也可以自定义组合控件；

react-native的缺点：组件不全，第三方组件也不全，遇到某些特殊功能，需要花大力气写；性能方面也无法媲美原生，还是有一些损耗，特别是交换大数据时；IOS版本略好，androi发展较慢；ios和android代码并非通用，有可能需要维护两套代码或者在代码中做一些判断；开发人员还是需要会原生开发，不然自定义组件无法编码；

目前得到经验是IOS版本比较稳定，android版本还不太成熟

## flutter


开发一套代码同时运行在iOS和Android平台, 原理是通过绘图引擎直接将 dart 绘制为图形, 省去了 rn 的转换步骤

生态不完善, 需要自己造轮子

https://www.zhihu.com/question/359468918 为什么要创造 dart 不直接用 java?


## h5 hybird 混合开发

即混合开发,由Native通过JSBridge等方法提供统一的API,然后用Html5+JS来写实际的逻辑,调用API

优点: 

- 调试适配简单, 直接在网页上调试和修改，几乎不用考虑用户机型和适配的问题

- 版本升级优势，网页的升级与用户无关，用户无需下载更新安装, 更新版本无需审核

- 部分性能要求的页面可用原生实现这应该是Hybrid模式的最多一个好处了,因为这种模式是原生混合web,所以我们完全可以将交互强,性能要求高的页面用原生写,然后一些其它页面用JS写,嵌入webview中,达到最佳体验

缺点: (主要是性能问题)

- H5加载大图片的时候性能会下降

- 大量用户访问同一个H5应用时性能会下降

- 不适用于交互性较强的app这种模式的主要应用是:一些新闻阅读类,信息展示类的app;但是不适用于一些交互较强或者性能要求较高的app(比如动画较多就不适合)

## web app

即移动端的网站,将页面部署在服务器上,然后用户使用各大浏览器访问,不是独立APP, 泛指 SPA(Single Page Application)模式开发出的网站

pros (advantage/superiority):

- 开发成本低,可以跨平台,调试方便

cons (disadvantage/weakness):

- 性能低,用户体验差, 功能受限,大量功能无法实现
- 依赖于网络,页面访问速度慢,耗费流量
- 临时性入口,用户留存率低这既是它的优点,也是缺点,优点是无需安装,确定是用完后有时候很难再找到,或者说很难专门为某个web app留存一个入口