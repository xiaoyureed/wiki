---
title: audio-video
tags: [audio, video]
categories: outline
---

JNI技术、NDK技术、HOOK技术、视频直播技术: https://github.com/UCodeUStory/JNI_NDK

go实现的 视频直播 server: https://github.com/QPlus/GotyeLive

ffmpeg: https://github.com/feixiao/ffmpeg, https://github.com/huwan/FFmpeg-Tutorial-CN, 搜索 "ffmpeg 入门"; java api: https://github.com/bytedeco/javacv, http://einverne.github.io/post/2015/12/ffmpeg-first.html, https://github.com/tonydeng/fmj

https://github.com/EasyDarwin/Course

http://www.ruanyifeng.com/blog/2020/01/ffmpeg.html

https://github.com/daniulive/SmarterStreaming 超强全自研跨平台(windows/android/iOS)流媒体内核 , 直播


<!--more-->

# 在线视频广告

https://blog.csdn.net/v6543210/article/details/40990197
https://www.jianshu.com/p/f2e1dc62cc82
https://www.ijophy.com/2013/12/block-youku-ads-again.html

## 视频播放原理



## 视频广告怎么加上的

最开始是直接加在视频开头, 使用普通的浏览器插件即可去除 (adblock...), 后来升级, 将广告嵌入到播放器, 即使屏蔽广告, 播放器仍然会等待直到广告播完, 这段时间黑屏

## 屏蔽过滤广告原理

- 广告元素隐藏

    网页中注入一段css样式，将某些id选择器和类选择器的样式设置为{ display: none; }来隐藏

- 请求阻断类 (拦截广告 API)

    通过正则匹配拦截规则库来阻断广告网络请求, 因为视频网站的广告和视频一般是放在不同URL地址的，而URL地址中字符串的前缀或后缀有着一定的规律性

- 拒绝接收来自特定IP(广告服务器)地址信息

# 音视频编程

https://blog.csdn.net/weixin_34378767/article/details/88601005 
TODO
