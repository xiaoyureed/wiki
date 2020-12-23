---
title: InteliJ idea tips
tags:
  - tool
date: 2018-09-12 10:48:35
categories: tool
---

<div align="center">
https://github.com/judasn/IntelliJ-IDEA-Tutorial/blob/master/keymap-introduce.md

https://www.cnblogs.com/yuze2020/p/13194962.html 激活
</div>

<!--more-->

<!-- TOC -->

- [快捷键](#快捷键)
    - [Windows](#windows)
    - [mac](#mac)
- [snippets](#snippets)
- [quickstart设置](#quickstart设置)
- [tips](#tips)
    - [debug](#debug)
    - [设置默认java编译版本](#设置默认java编译版本)

<!-- /TOC -->


# 快捷键

## Windows

```
ctrl alt s          全局设置
ctrl alt shift s    项目设置

CTRL + f4           关闭tab-----------------------------------------------ahk: CapsLock + W

编码常用:

f2                  定位到下一个错误
Ctrl + F1           在光标所在的错误代码处显示错误信息
Alt + Enter         根据位置不同 快速修复

ctrl + w            递进式选择
Ctrl + Y            删除光标所在行---------------------------------- ahk: CapsLock + D
Ctrl + D            复制到新行
shift + Enter       下方添加新行
CTRL + alt +Enter   上方添加新行 ------------------------------------------- CTRL + Enter
Ctrl+Shift+↑/↓      移动行 -------------------------- alt + ↑/↓ ---------- AHK shortcut: alt + i/k
Alt+Shift+↑/↓

CTRL + shift + v    从粘贴板选择复制

Ctrl + Alt + v      补全变量名称(抽取变量),类eclipse中的 CTRL+2;l ----------------------alt + M
CTRL + alt + M      抽取 方法
CTRL + alt + F      抽取 域变量/field ----------------------------------- alt + shift + f
CTRL + O            选择父类中需要 覆盖/override 的方法
Ctrl + Space        基础代码补全------------------------------------------------ alt + /
Ctrl + J            插入自定义动态代码模板 
alt + insert        1. 在代码中: 自动生成 getter, setter, constructor...; 
                    2. 在目录区: 创建 -----------------------------------------------  alt + N
Ctrl + Alt + T      对选中的代码弹出环绕选项弹出层, (比如 try/catch, if/else ...)
alt + 1...9         各个功能选项卡, 如:alt+1为 project选项卡
shift + esc         退出功能选项卡直接定位到 editor
alt + f1; 1         link到project目录 --------------------------------------- Alt + s 进入 Select in 菜单enter
Ctrl + Shift + U    对选中的代码进行大 / 小写轮流转换 

alt + 左/右         左右切换tab ----------------------------------------------ahk：CapsLock + O/P
CTRL + alt + 左右   跳到上一个编辑处 ------------------------------------------ahk: Cpaslock + H/B

CTRL + Z            撤销
Ctrl + Shift + Z    取消撤销

CTRL + /            行注释, 注释后光标另提一行
Ctrl + Shift + /    代码块注释 

shift + f6          rename ----------------------------AHK shortcut: CapsLock + R

CTRL + Q            查看方法说明;
                    double: 展开放大详细查看
Ctrl + P            方法参数提示显示 (简单的提示, 详细的意义说明还是用 CTRL + Q)

Ctrl + U            前往接口方法 [定义] (记忆: upper, 去往高层)
CTRL + alt + B      前往接口方法 [实现] 而不是接口定义处 (等同 CTRL + alt + 左键)
CTRL + B            1. 前往 方法/类/变量 使用处 (等同 CTRL + 左键)
                    2. 有多处使用则 find usage 弹窗

Ctrl + shift + f9   recompile current file ----------------------------- ahk: CapsLock + S
ctrl + f9           recompile project ---------------------------------- ahk: CapsLock + a

Ctrl + Alt + L      格式化代码，可以对当前文件和整个包目录使用
Ctrl + Alt + O      优化导入的类，可以对当前文件和整个包目录使用 


版本控制:


alt + `             版本控制菜单


查找:


Ctrl + N            查找 类
CTRL + shift + N    查找 所有类型 文件
shift + shift       全局查找文件
ctrl + f            查找(需要选择)
CTRL + r            替换 or 查找
Ctrl + Shift + F7   高亮显示光标所在文本(无需选择)，按Esc高亮消失 ----------------------------------- ahk: CapsLock + Q
CTRL + L, Ctrl + shift + l       (在没有搜索框的情况下直接) 寻找下/上一个指定字符, 可以和 CTRL+shift+f7 或 CTRL+f 联用


浏览整个项目结构:


Ctrl + "[" 或者 "]"  
                    移动光标到当前所在代码的花括号开始/结束位置
Ctrl + 左键单击      1. 在打开的文件标题上，"show in explorer"选择菜单, 即打开文件位置; 
                    2. 查看使用情况/ find usage ( <==> 右键 -> find usage)
alt + F7            find usage 查看 方法or类 使用情况 ;
ctrl + h            调用层级 call hierarchy

CTRL + f12          查看 方法outline -------- 添加新的shortcut: Ctrl + K

CTRL + alt + U      查看 类图/ diagram (任何对象 类型 上均可使用)


```

## mac

```
classical:

alt + enter 自动修复/show context actions   -> cmd + enter
shift + f6 重命名 rename   -> cmd + shift + r
cmd + shift + up/down 上下移动行  -> alt + up/down
// opt + up 渐进选择 selection  -> opt + cmd + i
ctrl + j 查看方法说明   -> shift + cmd + p
cmd + p 参数说明
cmd + n 生成
opt + f1, 1   select in 视图, 匹配  -> opt + q, 1
opt + cmd + v 抽取变量/自动补全变量   cmd + m
shift + cmd + f4 关闭 tab   -> cmd + e  
cmd + shift + e 重新打开reopen tab

cmd + shift + f7 高亮显示光标所在文本
cmd + ,   设置


macOS:

cmd + shift + u   转大写
```

# snippets

```
xxx_var.sout 打印

xxx_var.par 括号

xxx_bool.if     if判断

new String[] {"ab"}.length.for 循环, 结果如下:
for (int i = 0; i < new String[]{"ab"}.length; i++) {
                    
```

# quickstart设置

- 多台机器的配置同步 - https://github.com/judasn/IntelliJ-IDEA-Tutorial/blob/master/installation-directory-introduce.md

- 智能提示 首字母区分大小写 - https://github.com/judasn/IntelliJ-IDEA-Tutorial/blob/master/settings-recommend-introduce.md

- 设置代码检查等级 , 对于打开大文件尤其有用

- 类层级太多, 生成的 diagram 无法显示全貌, 可以取消 diagram | Java class diagram 下的 fields, constructor, method

# tips

- 部署时war和war exploded区别 - https://blog.csdn.net/xlgen157387/article/details/56498938
- 查看类的引用 - https://blog.csdn.net/wc250025/article/details/51352658
- 查看方法调用链 - https://blog.csdn.net/yinbucheng/article/details/77466613
- Lombok插件无法使用 - https://blog.csdn.net/xzp_12345/article/details/80268834
- 有其它类编译错误idea如何单独运行main方法 - https://jingyan.baidu.com/article/915fc414a557b351394b20cf.html
- 警告,错误下划线呈现样式(比如改的显眼一点) - https://blog.csdn.net/xiaoye142034/article/details/80869372
- debug with idea - https://www.cnblogs.com/chiangchou/p/idea-debug.html#_label0
- idea 中的 .gitignore: https://stackoverflow.com/questions/11968531/what-to-gitignore-from-the-idea-folder
- 设置 language level 总是又自动变回来 - https://blog.csdn.net/isea533/article/details/48575983  (maven 的 properties 不会继承, 所以对于 springboot 项目, 需要在每个项目指定 java.version) 对于 普通maven项目, 指定 maven-source-plugin 的 config 的 target , source 的版本即可

## debug

执行 Drop Frame 即可返回上个错过的断点

右击断点设置 Suspend 挂起条件为 Thread, 那么断点挂起时，可以切换线程进行调试, 可以实现多线程调试

## 设置默认java编译版本

```xml
<properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <maven.compiler.encoding>UTF-8</maven.compiler.encoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
    </properties>

或者

<plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>

```
