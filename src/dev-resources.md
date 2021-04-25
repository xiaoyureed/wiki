---
title: Dev Resources
date: 2018-09-10 20:38:10
tags: [dev resources]
categories: tool
---

开发相关 资源 & 链接

https://github.com/jhaddix/pentest-bookmarks 高质量书签集合
TODO

f12 浏览器 base64 编码解码
btoa('Hello World!'); // SGVsbG8gV29ybGQh
atob('SGVsbG8gV29ybGQh'); // Hello World!

<!--more-->

<!-- TOC -->

- [技术站点](#技术站点)
  - [技术blog](#技术blog)
  - [速查表](#速查表)
  - [前端docs](#前端docs)
  - [后端docs](#后端docs)
- [类库](#类库)
  - [前端类库](#前端类库)
  - [后端类库](#后端类库)
- [开发工具](#开发工具)
  - [在线开发环境](#在线开发环境)
- [有用的插件](#有用的插件)
  - [vs code](#vs-code)
  - [idea](#idea)
  - [eclipse](#eclipse)
  - [chrome](#chrome)
- [效率相关的软件](#效率相关的软件)
- [文件临时共享](#文件临时共享)
- [文件同步](#文件同步)
- [有用的搜索源](#有用的搜索源)
- [产品](#产品)
- [books](#books)
  - [技术](#技术)
  - [创业](#创业)
  - [处世](#处世)
- [story](#story)
- [freelancer 兼职](#freelancer-兼职)

<!-- /TOC -->

--------------

# 技术站点

## 技术blog

https://coolshell.cn/ 酷壳
http://www.ruanyifeng.com/blog/ 阮一峰

## 速查表

- https://devhints.io/ - 速查表

## 前端docs

- [技术栈](https://www.awesomes.cn/)
- [Bootstrap](http://www.bootcss.com/)
- [CSSreference](https://cssreference.io/)

## 后端docs

- [SDK.cn](https://sdk.cn/) - 技术栈
- [Spring全家桶](https://spring.io/) - [spring boot initializer](http://start.spring.io)
- [Apache.org](https://www.apache.org/index.html#projects-list)
- [mvn repo](https://mvnrepository.com/) - 相关：https://repo.spring.io/webapp/#/home
- [算法-数据结构-动画演示](https://visualgo.net)
- [ibm developer](https://developer.ibm.com/zh/)

# 类库

## 前端类库

js插件

- [typed](https://github.com/mattboldt/typed.js/) - 打字效果
- [dropzone](https://www.dropzonejs.com/)  - 拖拽上传
- [clipboardjs](https://clipboardjs.com/) - 复制, 剪贴


- [react-slides](https://github.com/react-component/slider)
- [markdown-slides](https://github.com/hiroppy/fusuma)

https://github.com/AEPKILL/devtools-detector 前端开发中如何在 JS 文件中检测用户浏览器是否打开了调试面板


## 后端类库

- [AssertJ](https://github.com/joel-costigliola/assertj-core) - fluently assert
- [Lombok](https://projectlombok.org/)

# 开发工具

前端开发工具

- [JSBin](http://jsbin.com) , [Codepen](https://codepen.io/) , [jsfiddle](http://jsfiddle.net/) (无需登录) , [pastebin](https://pastebin.com/) (粘贴分享代码, 无需注册), [codesandbox](https://codesandbox.io/dashboard/recent)
- [gif效果](https://photomosh.com/)
- [图片无损放大](http://bigjpg.com/)
- deploy your web app: [serge.sh](https://surge.sh/), [netlify](https://www.netlify.com/)
- [oktools](https://oktools.net/)
- [性能分析](https://github.com/GoogleChrome/lighthouse)

后端开发工具

- [工具合集(json, xml 编辑/格式化, ip lookup, port scan, base64编解码)](https://www.tutorialspoint.com/online_dev_tools.htm)
- [hutool util 工具包](https://hutool.cn/)
- [Java 临时在线测试(带提示)](https://www.dooccn.com/java/#contact)
- [finalshell](http://www.hostbuf.com/c/131.html)
- [spring-loaded](https://github.com/spring-projects/spring-loaded) -  alternatives：Jrebel, Spring Boot Developer Tools
- [fiddler](https://www.telerik.com/fiddler)
- [idea](https://zhile.io/), [idea](https://www.jiweichengzhu.com/article/eb340e382d1d456c84a1d190db12755c), [idea](http://idea.medeming.com/)


mac:

- [sequel pro 数据库连接管理工具](http://www.sequelpro.com/)

 Markdown文档生成网站

 - https://github.com/docsifyjs/docsify
 - https://github.com/squidfunk/mkdocs-material

## 在线开发环境

https://repl.it/

https://www.gitpod.io/

codepen, codesandbox 只用于前端


# 有用的插件

## vs code

[官方 marketplace](https://marketplace.visualstudio.com/)


其他通用

- shan.code-settings-sync: 借助 GitHub 同步 vscode 插件. [我的插件备份](https://gist.github.com/xiaoyureed/b578b86d402cda9e2fc4db397a7ef1b0)
- streetsidesoftware.code-spell-checker: 拼写检查
- christian-kohler.path-intellisense: 文件路径提示
- humao.rest-client: 测试 rest api
- ms-vscode-remote.remote-ssh: 使用 ssh 远程打开文件
- alefragnani.bookmarks: 代码书签
- markdown all in one
- gruntfuggly.todo-tree: 添加 todo 树到面板, 显示 todo, (TODO 必须大写, 在行的开头才有效)
- instant markdown 双屏展示 Markdown


前端通用

- prettier 格式化, 用来替代 beautify插件
- eslint 用于 es 代码或者 ts 代码的语法检查
- msjsdiag.debugger-for-chrome: debugger for chrome
- ms-vscode-remote.remote-wsl: 在 wsl 中打开文件夹
- cipchk.cssrem: css 长度单位转换, px -> rem
- nucllear.vscode-extension-auto-import: es6, typescript 自动导包 (貌似不生效)
- wix.vscode-import-cost: 显示导包的大小
- live server 启动服务器浏览静态页面
- prettier
- eslint
- abusaidm.html-snippets html提示
- HTML CSS Support


react

- dsznajder.es7-react-js-snippets: react 开发代码段
- clinyong.vscode-css-modules: css module 提示和 css 类跳转
- wallabyjs.quokka-vscode 实时变量显示


后端通用


java

- k--kato.intellij-idea-keybindings: idea shortcut 映射 idea 快捷键
- vscjava.vscode-java-pack: 
    - redhat java language support, 
    - debugger for java, 
    - java test runner, 
    - maven for java, 
    - java  dependencies viewer , 
    - visual studio intelliCode
- gabrielbb.vscode-lombok: 简化 pojo


python

- ms-python.python: python 开发插件

golang

- ms-vscode.go: golang 开发插件



另: [vscode java 环境](https://javahello.github.io/2018/02/26/ubuntu/vscode-java-environment/)

[react 开发环境](https://www.cnblogs.com/xbzhu/p/10823300.html)

## idea

- go language - golang support

## eclipse

## chrome

开发

- Octotree - GitHub 目录
- Restlet Client - rest api test client. alternatives: Postman
- talent api tester - rest api 测试
- LiveReload - 页面开发实时更新
- Fatkun - 图片批量下载 - 图片下载助手 - 右键提取所有图片, 带分辨率, 类似的还有 ImageAssitant 图片嗅探
- iMacros - 网页自动化, 重放. https://imacros.net/browser/cr/welcome/?utm_source=browser&utm_medium=product, https://chrome.google.com/webstore/detail/imacros-for-chrome/cplklnmnlbnpmjogncfgfijoopmnlemp/related //todo
- Proxy SwitchyOmega - 配合 goagent/XX-ne 科学上网, https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif


效率

- Grammarly - grammar checking 英语语法检查
- AdBlock - 广告屏蔽, Adblock Plus 更强大
- ublock plus adblocker
- Tampermonkey - 油猴, firefox: GreaseMonkey - https://greasyfork.org/zh-CN
    - 网页限制解除 - 破解粘贴限制, 类似的有 Enable Right Click and Copy 插件, Enable copy插件
    - saveFrom.net helper - 下载视频, YouTube有效
    - ac-baidu - 百度去广告, 去重定向
    - 百度网盘直接下载助手
    - 全网音乐一键免费下载
    - 豆瓣资源下载大师
    - 购物党比价工具
    - 右键新标签中打开图片时显示最优图像质量
- [octotree](https://github.com/ovity/octotree) - make github easy to use - 为 GitHub 添加目录
- OneTab - 关闭标签, 并将 url 加入到单独的暂存页, 类似的还有 oneTab plus, Toby
- The Great Suspender - 超时冻结标签页
- [baiduexporter](https://chrome.google.com/webstore/detail/baiduexporter/jgebcefbdjhkhapijgbhkidaegoocbjj) - 到处百度云链接, export baiduyun download links to aria2/aria2-rpc - [aria2](https://aria2.github.io/), [web控制台](http://aria2c.com/)
- google translator -  谷歌翻译插件
- [有道词典 chrome 划词插件](https://chrome.google.com/webstore/detail/%E6%9C%89%E9%81%93%E8%AF%8D%E5%85%B8chrome%E5%88%92%E8%AF%8D%E6%8F%92%E4%BB%B6/eopjamdnofihpioajgfdikhhbobonhbb)
- Momentum - Replace new tab page
- QuicKey – The quick tab switcher 标签切换, 搜索
- dark reader - 网页变暗 https://darkreader.org/, https://github.com/darkreader/darkreader
- project Naptha - 图片取词, https://projectnaptha.com/, https://chrome.google.com/webstore/detail/project-naptha/molncoemjfmpgdkbdlbjmhlcgniigdnf
- Awesome screenshot - 浏览器截图, 录屏; 类似的还有 FireShot (https://getfireshot.com/)
- 书签侧边栏, 类似的有 Tidy Bookmarks Tree
- Nooboss - 管理插件的插件, 何时打开何时关闭插件, 类似的 Extensions Manager
- Raindrop.io - 书签全平台同步
- Markdown Here - 在基于浏览器的编辑器中使用 markdown
- AHA Music - 识别正在播放歌曲
- 搜我听歌 - 类似 listen1
- Video Speed Controller - 视频加速, 仅仅支持 html5 播放器
- -Free Download Manager 每次下载能直接启动FDM来进行下载 , 如果你使用的是迅雷，我建议使用迅雷下载支持



# 效率相关的软件

Windows

- [FadeTop](http://www.fadetop.com/)
- [autohotkey](https://www.autohotkey.com/docs/AutoHotkey.htm) - keymap
- bandzip, 7-zip 压缩

- [腾讯兔小巢 - 免费的用户反馈社区](https://txc.qq.com/)

web

- [作图(uml图, 时序图...)](https://www.draw.io/)
- [表关系图 数据库关系图](https://dbdiagram.io/d)
- [web sequence diagrams 专门画时序图](https://www.websequencediagrams.com/)
- [ascii 画流程图](http://asciiflow.com/), [更精细画图 (非web)](http://www.jave.de/)
- [生成文字字符画](http://patorjk.com/software/taag)
- [根据图片生成字符画](https://www.ascii-art-generator.org/)

# 文件临时共享

- https://box.zjuqsc.com/ - 潮云优盘 100m
- https://tempfile.cloud/
- https://cowtransfer.com/ - 2g
- https://send.firefox.com/ - 1g
- https://bitsend.jp/?setLang=zh-tw - 2g 国内慢
- https://catbox.moe/ - 200m
- https://drop.me/ - 无限制，国内慢
- http://tmp.link/ - 5g 快
- https://coka.la/
- https://transfer.sh/ - 命令行， 慢
- https://wetransfer.com/
- https://send.firefox.com/ 大文件

# 文件同步

Syncthing

Seafile

Resilio Sync
分享一个key - BCWHZRSLANR64CGPTXRE54ENNSIUE5SMO

NextCloud

# 有用的搜索源

[Google](https://google.com), [GitHub](https://github.com), [GitHub中文](https://www.githubs.cn/), [stackoverflow](https://stackoverflow.com), [知乎](https://zhihu.com), [quora](https://quora.com), [秘迹it搜索](https://mijisou.com/), [magi](https://magi.com)

 [slideshare](https://slideshare.net), [speakdeck](https://speakdeck.com), [YouTube](https://youtube.com), [哔哩哔哩](https://bilibili.com), [微信读书](https://weread.qq.com/), [微信搜索](https://weixin.sogou.com/), [豆瓣](https://www.douban.com/), [维基百科](https://zh.wikipedia.org/zh-cn/), [Wikipedia](https://www.wikipedia.org/), 


# 产品

https://matomo.org - 用户行为数据统计 (实时统计用户在你网站上的行为轨迹)

https://web.dev - 谷歌出品 web站点性能优化

https://www.17ce.com - 网站全国测速

https://www.intercom.com/ 客户关系即时通信
https://www.hotjar.com/
https://crisp.chat/en/ 客服

https://www.typeform.com/ 在线表格


# books

https://github.com/0voice/expert_readed_books
https://github.com/0voice/from_coder_to_expert

## 技术

《计算机网络自顶向下方法与Internet特色》
《反应式设计模式》

《Python 3反爬虫原理与绕过实战》作者：韦世东

《自然语言处理入门》作者：何晗

《虚拟机设计与实现：以JVM为例》作者：中 李晓峰 译者：单业 

《黑客大揭秘：近源渗透测试》作者：王永涛，柴坤哲，杨芸菲，杨卿

《Python深度学习》作者：François Chollet 译者：张亮（hysic）
《深度学习入门》作者：斋藤康毅 译者：陆宇杰
《深度学习的数学》作者：涌井良幸，涌井贞美 译者：杨瑞龙
https://mp.weixin.qq.com/s/O-2L-i4SdaV0btvVnoK17A

《松本行弘：编程语言的设计与实现》

《数据密集型应用系统设计》

## 创业

《上瘾》“Indistractable: How to Control Your Attention and Choose Your Life” （《永不分心：如何控制你的注意力和人生》 ----- Nir Eyal


德鲁克的《创新和企业家精神》创新机遇的7个来源

《理解媒介》麦克卢汉

戴维迈尔斯《社会心理学》

《产品方法论》俞军

易到的创始人周航的《重新理解创业》

DuckDuckGo 的创始人写的《Traction》 市场

https://kalasearch.cn/blog

## 处世

《煤老板自述三十年》
《世间的盐》

影响力 
态度改变与社会影响

《乡下人的悲歌》，这本书真实记录了近20年美国经济欣欣向荣的背景下底层老百姓令人绝望的生活状态。


# story

https://us.v2ex.com/t/701124#reply101


# freelancer 兼职

- 卖课
  小鹅通, 荔枝微课, 千聊
- 写稿
  公众号-投稿约稿指南, 豆瓣小组-豆瓣稿费银行
- 视频
- 线上家教
  掌门 1v1, 优思教育, 儒林教育,悟空中文, 哈兔中文

