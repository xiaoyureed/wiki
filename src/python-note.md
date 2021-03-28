---
title: Python
date: 2015-02-10 22:19:59
tags: [python]
categories: computer science
---

https://github.com/jackzhenguo/python-small-examples 小例子

https://github.com/jackfrued/Python-100-Days 教程

https://github.com/521xueweihan Python

https://github.com/jackfrued

python 学习笔记;
references: [1](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000), [2](https://github.com/yidao620c/python3-cookbook)
算法实现: https://github.com/yidao620c/core-algorithm， https://github.com/billryan/algorithm-exercise
scrapy 爬虫: https://github.com/yidao620c/core-scrapy

"流畅的 Python" 推荐

https://github.com/HT524/500LineorLess_CN 实现简单系统

https://github.com/iswbm/pycharm-guide
https://github.com/iswbm

https://zhuanlan.zhihu.com/p/24132781 经验简历

https://github.com/lalor 运维大牛

https://github.com/jobbole/awesome-python-cn

https://python-web-guide.readthedocs.io/zh/latest/index.html 工程实践 经验分享

https://learnku.com/docs/python-guide/2018 技术栈

https://bramblexu.com/posts/b1c0cc4f/#toc-heading-5 vscode 配置 


https://www.zhihu.com/question/19827960 指的关注的社区

<!--more-->

<!-- TOC -->

- [实用脚本](#实用脚本)
- [介绍](#介绍)
  - [python 特点](#python-特点)
  - [python 安装](#python-安装)
  - [vscode 环境配置](#vscode-环境配置)
  - [设置国内pip源](#设置国内pip源)
  - [解释器](#解释器)
  - [kite 插件使用](#kite-插件使用)
- [语法](#语法)
  - [输入输出 打印 main魔法变量](#输入输出-打印-main魔法变量)
  - [基本数据类型](#基本数据类型)
    - [字符串](#字符串)
      - [多行 不可变 比较](#多行-不可变-比较)
      - [编码 字符处理](#编码-字符处理)
      - [格式化](#格式化)
      - [字符串方法](#字符串方法)
    - [数字](#数字)
    - [布尔值 空值](#布尔值-空值)
    - [集合](#集合)
      - [有序可变 list](#有序可变-list)
      - [切片 slices](#切片-slices)
      - [有序不可变列表 tuple](#有序不可变列表-tuple)
      - [无序不可重复 set](#无序不可重复-set)
    - [键值存储 字典 dict](#键值存储-字典-dict)
    - [生成器 generator](#生成器-generator)
  - [条件循环](#条件循环)
  - [比较判断](#比较判断)
  - [函数](#函数)
    - [参数检查](#参数检查)
    - [多种参数](#多种参数)
    - [递归](#递归)
    - [高阶函数](#高阶函数)
  - [文件处理](#文件处理)
    - [创建文件](#创建文件)
    - [读写文件数据](#读写文件数据)
  - [装饰器](#装饰器)
  - [模块 作用域](#模块-作用域)
  - [面向对象](#面向对象)
    - [类](#类)
    - [继承 鸭子类型](#继承-鸭子类型)
    - [判断类型信息](#判断类型信息)
    - [动态操作](#动态操作)
    - [特殊方法](#特殊方法)
    - [枚举](#枚举)
  - [错误异常处理](#错误异常处理)
- [日志](#日志)
- [单元测试](#单元测试)
- [多进程](#多进程)
- [异步](#异步)
- [内建模块](#内建模块)
  - [日期处理](#日期处理)
  - [集合](#集合-1)
- [编写命令行程序](#编写命令行程序)
  - [命令行自动补全](#命令行自动补全)
- [类型系统](#类型系统)
- [调试](#调试)
  - [断言](#断言)
  - [设置断点 pdb](#设置断点-pdb)
- [jinja2 模板](#jinja2-模板)
- [路径问题](#路径问题)
  - [文件路径 and 终端路径](#文件路径-and-终端路径)
  - [获取脚本位置](#获取脚本位置)
- [shutil 使用](#shutil-使用)
- [venv 虚拟环境](#venv-虚拟环境)
- [web 开发](#web-开发)
  - [Django](#django)
  - [flask](#flask)
- [db 驱动 问题](#db-驱动-问题)
- [代替 shell 运维](#代替-shell-运维)
  - [传递参数](#传递参数)
  - [shell 调用 Python](#shell-调用-python)
    - [统计出现次数](#统计出现次数)
    - [读取 csv 文件](#读取-csv-文件)
  - [Python 调用 shell](#python-调用-shell)
    - [subprocess](#subprocess)
    - [os.system](#ossystem)
    - [system](#system)
    - [commands](#commands)
    - [paramiko](#paramiko)
  - [部署 springboot](#部署-springboot)
- [开源库](#开源库)
  - [图像](#图像)
  - [数据模型定义](#数据模型定义)
  - [序列化反序列化](#序列化反序列化)
- [jython](#jython)
- [调用 rust](#调用-rust)
- [python 应用部署](#python-应用部署)
  - [生成依赖清单](#生成依赖清单)
  - [gunicorn](#gunicorn)

<!-- /TOC -->


# 实用脚本

https://zhuanlan.zhihu.com/p/85728888
TODO

# 介绍

## python 特点

python 作为脚本语言, 方法定义调用有前后顺序

库多

胶水语言, cpp 交互方便

面向对象, 类似 ruby, 但是没有 ruby 面向对象纯粹


## python 安装

wls 默认装的是 python2

```sh
# 更新源
apt-get update
# 安装 (我的wls 是 Ubuntu)
apt-get install python3
# 验证
python3 --version

```

## vscode 环境配置


[wsl 中使用 vscode](https://code.visualstudio.com/docs/remote/wsl#_getting-started)

[wsl install python3.7, 修改快捷方式](https://blog.csdn.net/u014775723/article/details/85213793)

```sh
# install python
sudo apt-get install python3.7
# check py version
python3 --version
# install pip3
apt-get install python3-pip
# check pip version
pip3 --version

```

vscode 安装完 python 插件后, 会有 pylint (同类 flake8) 插件安装提示, 也可能没有 pylint 安装提示, 那么需要手动设置 `.vscode/settings.json`:

(pylint 比 flake8 更严格，问题描述也更精准，速度较后者慢一些。pylint 的严格不如说有些严苛了，经常啰里啰嗦报出一大串问题，根据场景不同，可能需要配置许多忽略项)

```json
"python.pythonPath": "/usr/bin/python",
  "python.linting.pylintEnabled": true,
  "python.linting.enabled": true
```

autopep8 格式化代码库, vscode 会自动提示 (同类是 black, yapf)

Coverage 有多种向你报告测试覆盖率的方式，包括将结果输出到控制台或 HTML 页面，并提示哪些行号没有覆盖到。你可以设置配置文件以自定义 Coverage 检查的内容并使其更便于运行

## 设置国内pip源

```
sudo vim ~/.pip/pip.conf

[global] 
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host = https://pypi.tuna.tsinghua.edu.cn  # trusted-host 此参数是为了避免麻烦，否则使用的时候可能会提示不受信任

```

or

https://mirrors.tuna.tsinghua.edu.cn/help/pypi/

## 解释器

CPython (默认)

PyPy, 采用 JIT 技术，对 Python 代码进行动态编译, PyPy 和 CPython 有一些是不同的，这就导致相同的 Python 代码在两种解释器下执行可能会有不同的结果

Jython, 直接把 Python 代码编译成 Java 字节码执行

IronPython, 把 Python 代码编译成.Net 的字节码。

如果要和 Java 或.Net 平台交互，最好的办法不是用 Jython 或 IronPython，而是通过网络调用来交互，确保各程序之间的独立性。

## kite 插件使用

```py
# -*- coding: utf-8 -*-

# Welcome to...
#
#         `hmy+.               ://:
#        .mMMMMMNho:`          NMMm
#       :NMMMMMMMMMMMds/.`     NMMm            :ss:
#      +NMMMMMMMMMMMMMMMMmy+   NMMm           -MMMM-   ---
#    `oMMMMMMMMMMMMMMMMMMMMo   NMMm            /ss/   :MMM+
#   `yMMMMMMMMNshmNMMMMMMMN`   NMMm                   /MMM+
#  .dMMMMMMMMm/hmhssydmMMM+    NMMm    `/yhhy. shhy ohmMMMmhhhh.  ./ydmmmdho-
#  omMMMMMMMd/mMMMMMmhsosy`    NMMm  .omMMmo.  mMMN odmMMMmdddd. omMNdsoshNMNy`
#   .+dMMMMy/mMMMMMMMMMMm-     NMMm-yNMMh/`    mMMN   /MMM+     sMMN:`   `:NMMy
#     `-ymo/NMMMMMMMMMMMd      NMMMNMMN/       mMMN   :MMM+     MMMNdddddddNMMN
#        ``hMMMMMMMMMMMM:      NMMm+mMMNs.     mMMN   :MMM+     MMMh//////////:
#          `:yNMMMMMMMMh       NMMm `/dMMNy-   mMMN   :MMM+  `. sMMNo`    `-:
#             .+mMMMMMM-       NMMm   `/dMMNy- mMMN   .MMMNddNN/ +NMMNdhydNNMs
#               `:yMMMy        yhhs     `/hhhh shhs    :ymmmdho:  `/sdmmmmhs/`
#                  `om.


""" Kite is your programming copilot. Kite will try to show you the
    right information at the right time as you code to prevent you from context
    switching out of your current line of thought.

    This tutorial will teach you how to use all of Kite's core features. You
    should be able to learn everything in 5 minutes.

    If you get stuck at any point, please visit https://help.kite.com/ or file
    an issue at https://github.com/kiteco/issue-tracker.
"""


""" PYTHON TUTORIAL ============================================================

    Not writing Python? Open tutorials for other languages by running the
    following actions:

    * For Javascript, run   "Kite: Javascript Tutorial"
    * For Go, run           "Kite: Go Tutorial"
"""


""" PART 0: BEFORE WE START ====================================================

    Kite's PyCharm plugin will by default try to start the Kite backend when
    the editor first starts. You can change this behavior by opening the Kite
    plugin's settings, and changing "Start Kite at startup".

    Look for the Kite icon in the bottom right corner of PyCharm's status bar —
    It will tell you if Kite is ready and working. If the indicator is red,
    then you'll have to start the Kite Engine manually before proceeding with
    the rest of this tutorial.
"""


""" PART 1: CODE COMPLETIONS ===================================================

    Kite analyzes your code and uses machine learning to show you completions
    for what you're going to type next.

    If you have your editor configured to show autocompletions, then Kite will
    show you completions automatically as you type.

    If you don't have autocompletions on, you can press ctrl+space to request
    completions at any time.

    You can toggle autocompletions in the editor preferences under "Editor" →
    "General" → "Code Completion" and changing "Autopopup code completion".

    Look for Kite's icon on the left-hand side to see which completions are
    coming from Kite.
"""


# 1a. Name completions
#
# Kite can suggest names of symbols to use, such as names of packages or names
# of variables in scope.

# TRY IT
# ------
# • Put your cursor at the end of the line marked with "<--".
# • Type "s" and select the completion for "json". (The rest of this tutorial
#   depends on you doing so!)
# • Remember to press ctrl+space if autocompletions aren't on.

import j  # <--


# 1b. Attribute completions
#
# Type a "." after a name and Kite will show you the attributes available.

# TRY IT
# ------
# • Put your cursor at the end line of the line marked with "<--".
# • Type ".", and select the completion for "dumps".
# • Remember to press ctrl+space if autocompletions aren't on.

json  # <--


# 1c. Code completions on demand
#
# Remember that you can use a keyboard shortcut at any time to request code
# completions.

# TRY IT
# ------
# • Put your cursor at the end of the line marked with "<--".
# • Press ctrl+space to request code completions to see the attributes in the
#   json module.

json.  # <--


""" PART 2: FUNCTION ASSIST ====================================================

    Kite can also show you how to use a function as you're calling it in your
    code.

    If you have your editor configured to show parameter info automatically,
    then Kite will show you this information automatically as you're coding
    when it detects that your cursor is inside a function call.

    You can prevent this UI from being shown automatically in the editor
    preferences by navigating to "Editor" → "General" → "Code Completion", and
    then changing "Autopopup" under "Parameter Info".

    You can manually request function assist at any time by pressing
    command+p. However, your cursor must be inside a function call for the UI
    to appear.

    You can hide the function assist UI by pressing escape.
"""


# 2a. Function signatures and more
#
# When you're calling a function, Kite will show you the function's signature
# to help you remember what arguments you need to pass in. It may also show you
# examples of how other developers use the function and the keyword arguments
# you can use.

# TRY IT
# ------
# • Put your cursor at the end of line marked with "<--".
# • Type "(" to start the function call, and Kite will show you how to call
#   json.dumps.
# • Remember to press command+p after typing "(" if you've disabled function
#   assist from happening automatically.
#
# • Within the UI, click on the "Examples" link to see how other developers use
#   the function.
# • You can hide this information by clicking on the "Hide" link.

json.dumps()  # <--


# 2b. Learning from your own code
#
# Kite will also show you signatures, example usages, and keyword arguments for
# functions that you have defined in your own code.

# TRY IT
# ------
# • Put your cursor at the end of the line marked with "<--".
# • Type "(" to get assistance for your locally defined pretty_print function.
# • Remember to press command+p after typing "(" if you've disabled function
#   assist from happening automatically.

def pretty_print(obj, indent=2):
    print(json.dumps(obj, indent=indent))

pretty_print(obj, indent=4)

pretty_print()  # <--


# 2c. Function assist on demand
#
# Remember that you can use a keyboard shortcut at any time to view information
# about a function.

# TRY IT
# ------
# • Put your cursor between the "(" and ")" on the line marked with "<--".
# • Press command+p to access function assist.

pretty_print()  # <--


""" PART 3: INSTANT DOCUMENTATION ==============================================

    Kite can also show you documentation for the symbols in your code. You can
    use the action "Kite: Docs at cursor" to view documentation for the symbol
    under your cursor.
"""


# TRY IT
# ------
# • Move your cursor over "dumps".
# • Press command+shift+a.
# • Run the action "Kite: Docs at cursor".

json.dumps


""" That's it!

    Now you know how to use Kite's completions to boost your productivity. You
    can access this tutorial at any time by running the action
    'Kite: Python Tutorial'.

    Learn more about Kite at our help page:

    https://help.kite.com/


    ____________________________________________________________________________

    Kite is under active development. You can expect its features to become
    smarter and more featured over time.

    We love hearing from you! Vist https://github.com/kiteco/issue-tracker at
    any time to report issues or submit feature requests.
"""


```

# 语法

## 输入输出 打印 main魔法变量

```py
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# 以上两行表示python 解释器版本；以utf-8编码

# 模块的文档注释
' a test module '

__author__ = 'xiaoyu'


def hello_print():
    '''方法注释
    可以通过 hello_print.__doc__ 获取, 或者 help(hello_print) 获取文档字符串
    '''
    print("Hello world!")
    print("aa", "bb", "ccc")  # 类似

    # 间隔
    print(1, 2, 3, sep="   ")  # 1   2   3
    # 不换行
    print('hello', end='')
    print('没换行')  # hello没换行

    # 命令行输入
    # name = input('please enter your name: ')
    # print(name)


# 
# 命令行import方式调 本文件中的方法, 将不进入if判断，此时通过 main.xxx() 调用
# 如果直接 python main.py, 则进入 if
if __name__ == "__main__":
    hello_print()

```


## 基本数据类型

### 字符串 

#### 多行 不可变 比较

```py
# 字符串
    a='a'
    b="b"
    c="I'm xy."
    d='I\'m \"OK\"!'
    # r'' 形式的默认不转义
    e=r'\\\t\\'
    f='''多行内容'''


# str 是不可变对象
    # 不可变对象不可修改，返回一个新的对象
    # >>> a = 'abc'
    # >>> a.replace('a', 'A')
    # 'Abc'
    # >>> a
    # 'abc'


# 比较字符串是否相同 使用 ==, 这点和 java 不同
# 不要使用 is, is 比较的是 引用地址是否相同, == 比较的才是是内容
# is, is not 对比的是两个变量的内存地址
# ==,!= 对比的是两个变量的值
if 'ab' == 'ab':
    print('ab == ab')
```

#### 编码 字符处理

```py
 ### 编码处理 字符处理
    #
    # 字符
    # 字符编码
    # ord:把字符串转换为ASCII    bin:把字符串转换成二进制
    # oct:把字符串转换成八进制    hex:把字符串转换成16进制
    #>>> ord('A')
    #65
    #>>> ord('中')
    #20013
    #>>> chr(66)
    #'B'
    #>>> chr(25991)
    #'文'
    #>>> 'u4e2du6587'
    #'中文'

    # s1=u’哈’
    # type(u’哈’)，则会得到<type ‘unicode’>，也就是在字符前面加u就表明这是一个unicode对象，这个字会以unicode格式存在于内存中
    # 如果不加u，表明这仅仅是一个使用某种编码的字符串，编码格式取决于python对源码文件编码的识别，如 # -*- coding: utf-8 -*-
    # https://www.cnblogs.com/yyxayz/p/4044528.html


    # 字节
    # 如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes。
    # Python对bytes类型的数据用带b前缀的单引号或双引号表示：
    # x = b'ABC'
    # # print("hello...")
    # >>> 'ABC'.encode('ascii')
    # b'ABC'
    # >>> '中文'.encode('utf-8')
    # b'xe4xb8xadxe6x96x87'
    # >>> b'ABC'.decode('ascii')
    # 'ABC'
    # >>>b'xe4xb8xadxe6x96x87'.decode('utf-8')
    # '中文'
    # >>> len('ABC')
    # 3
    # >>> len('中文')
    # 2
```

#### 格式化

```py
# 格式化 占位符
    #
    # %s表示用字符串替换，%d表示用整数替换，有几个%?占位符，后面就跟几个变量或者值，顺序要对应好。如果只有一个%?，括号可以省略
    # 如果你不太确定应该用什么，%s永远起作用，它会把任何数据类型转换为字符串
    print('Hi, %s, you have $%d.' % ('Michael', 1000000))
    # 'Hi, Michael, you have $1000000.'
    #
    print("{} {}".format("hello", "world"))   # 不设置指定位置，按默认顺序
    print("{0} {1}".format("hello", "world"))  # 设置指定位置
    print('hello {name}'.format(name='xiaoyu'))
    # 通过字典设置参数
    site = {"name": "xi", "url": "xiaoyureed.github.io"}
    print("网站名：{name}, 地址 {url}".format(**site))
    # 通过列表索引设置参数
    my_list = ['hello', 'world']
    print("网站名：{0[0]}, 地址 {0[1]}".format(my_list))  # "0" 是必须的
    # 传入一个对象 
    # my_value = {"value": "hello"}
    # print('value 为: {0.value}'.format(my_value))  # "0" 是可选的, my_value 对象需要有 value 属性

    # 格式化数字
    print("{:.2f}".format(3.1415926))  # 3.14
```

#### 字符串方法

```py

 #
    # 字符串处理方法
    #
    #
    len(b'ABC')  # 3
    len(b'xe4xb8xadxe6x96x87')  # 6
    len('中文'.encode('utf-8'))
    # 6

    # count(xxx) 某个元素出现次数
    # 计算行数
    myString = '''line1
sf
sdfsf
sdfs
    '''
    print(myString)
    c = myString.count("\n")+(not myString.endswith("\n"))
    print(c)  # 5
    # or
    c1 = myString.count("\n")+[1, 0][myString.endswith("\n")]
    print(c1)  # 5

    # 查找
    find = myString.find("sf")
    print("find = ", find)  # 6, 从 0 开始, 第 6 个字符
    find1 = myString.find("220")
    print("find1 =  ", find1)  # -1
    
    if 'world' in string:
        xxx

    # 切割为 list
    string = "hello\nworld xxx yyy zzz"
    print(string.split("\n"))  # ['hello', 'world xxx yyy zzz']
    # 以 sep 为分隔符切片 string，如果 maxsplit 有指定值，则最多切割多少次 (必须带上 maxsplit)
    print(string.split(sep='o', maxsplit=1))  # ['hell', '\nworld xxx yyy zzz']
    #按照行('\r', '\r\n', \n')分隔，返回一个包含各行作为元素的列表，如
    # 果参数 keepends 为 False，不包含换行符，如果为 True，则保留换行符。默认 false
    print(string.splitlines(True)) # ['hello\n', 'world xxx yyy zzz']
    print(string.splitlines(False)) # ['hello', 'world xxx yyy zzz']
    print(string.splitlines()) # ['hello', 'world xxx yyy zzz']


    # 切割成 tuple
    #
    #有点像 find()和 split()的结合体,从 str 出现的第一个位置起,把 字 符 串 string 分
    #成 一 个 3 元 素 的 元 组 (string_pre_str,str,string_post_str),如
    # 果 string 中不包含str 则 string_pre_str == string.
    print(string.partition('w') ) # ('hello\n', 'w', 'orld xxx yyy zzz')

    # string.rpartition(str) #类似于 partition()函数,不过是从右边开始查找


    # # 拼接
    print('\n'.join(['aaa', 'bbb', 'ccc']))

    # # 将字符串转为 字符列表
    print(list("12345")) # ['1', '2', '3', '4', '5']

    # # 
    #
    #  常用内建函数
    #
    #
    # # strip() 
    # 用于移除字符串头尾指定的字符（默认为空格或换行符）或字符序列
    str = "00000003210Runoob01230000000"
    # 移除指定字符
    print(str.strip( '0' ))  # 3210Runoob0123
    str2 = "   Runoob      "   # Runoob
    # 默认是移除空格
    print(str2.strip())
    str = "123abcrunoob321"
    # 指定的字符没有顺序之分
    print (str.strip( '12' ))  # 3abcrunoob3

    string = "   hello    "
    string = string.lstrip()#截掉 string 左边的空格
    string = string.rstrip()#删除 string 字符串末尾的空格.
    print("string = " + string)


    print(string.capitalize()) # 把字符串的第一个字符大写
    # string.lower()#转换 string 中所有大写字符为小写.
    # string.upper()#转换 string 中的小写字母为大写
    # string.swapcase()#翻转 string 中的大小写
    # string.title()#返回"标题化"的 string,就是说所有单词都是以大写开始，其余字母均为小写(见 istitle())

    # string.center(width) #返回一个原字符串居中,并使用空格填充至长度 width 的新字符串
    # string.ljust(width)#返回一个原字符串左对齐,并使用空格填充至长度 width 的新字符串
    # string.rjust(width)#返回一个原字符串右对齐,并使用空格填充至长度 width 的新字符串
    # string.zfill(width)#返回长度为 width 的字符串，原字符串 string 右对齐，前面填充0

    # 返回 str 在 string 里面出现的次数，如果 beg 或者 end 指定则返回指定范围内 str 出现的次数
    string = "xsdf aa sfefaafsef"
    print(string.count("aa")) # 2
    print(string.count("aa", 7, len(string))) # 1

    #以 encoding 指定的编码格式解码 string，如果出错默认报一个 ValueError 的 异 常 ， 
    # 除非 errors 指 定 的 是 'ignore' 或 者'replace'
    # string.decode(encoding='UTF-8', errors='strict') 
    # string.encode(encoding='UTF-8', errors='strict')


    # string.endswith(obj, beg=0, end=len(string))#检查字符串是否以 obj 结束，如果beg 或者 end 指定则检查指定的范围内是否以 obj 结束，如果是，返回 True,否则返回 False.
    # string.startswith(obj, beg=0,end=len(string))#检查字符串是否是以 obj 开头，是则返回 True，否则返回 False。如果beg 和 end 指定值，则在指定范围内检查.

    # string.expandtabs(tabsize=8) #把字符串 string 中的 tab 符号转为空格，tab 符号默认的空格数是 8。

    # string.find(str, beg=0, end=len(string)) #检测 str 是否包含在 string 中，如果 beg 和 end 指定范围，则检查是否包含在指定范围内，如果是返回开始的索引值，否则返回-1
    # string.rfind(str, beg=0,end=len(string) )#类似于 find()函数，不过是从右边开始查找.

    # string.index(str, beg=0, end=len(string)) #跟find()方法一样，只不过如果str不在 string中会报一个异常.
    # string.rindex( str, beg=0,end=len(string))#类似于 index()，不过是从右边开始.

    # string.isalnum()#如果 string 至少有一个字符并且所有字符都是字母或数字则返回 True,否则返回 False
    # string.isalpha() #如果 string 至少有一个字符并且所有字符都是字母则返回 True,否则返回 False

    # string.isdecimal()#如果 string 只包含十进制数字则返回 True 否则返回 False.
    # string.isdigit()# 如果 string 只包含数字则返回 True 否则返回 False.
    # string.isnumeric()#如果 string 中只包含数字字符，则返回 True，否则返回 False

    # string.islower()#如果 string 中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是小写，则返回 True，否则返回 False
    # string.isupper()#如果 string 中包含至少一个区分大小写的字符，并且所有这些(区分大小写的)字符都是大写，则返回 True，否则返回 False

    # string.isspace()#如果 string 中只包含空格，则返回 True，否则返回 False.

    # string.istitle() #如果 string 是标题化的(见 title())则返回 True，否则返回 False


    # string.join(seq)#以 string 作为分隔符，将 seq 中所有的元素(的字符串表示)合并为一个新的字符串


    # string.maketrans(intab, outtab]) #maketrans() 方法用于创建字符映射的转换表，对于接受两个参数的最简单的调用方式，第一个参数是字符串，表示需要转换的字符，第二个参数也是字符串表示转换的目标。

    # max(str)#返回字符串 str 中最大的字母。
    # min(str)#返回字符串 str 中最小的字母。

    # string.replace(str1, str2,  num=string.count(str1)) #把 string 中的 str1 替换成 str2,如果 num 指定，则替换不超过 num 次.


    # string.translate(str, del="")#根据 str 给出的表(包含 256 个字符)转换 string 的字符, 要过滤掉的字符放到 del 参数中
```

### 数字

```py
# 整数
    c=2
    a=0xff00  # 16进制

    # 浮点数
    #
    # 除法计算结果是浮点数，即使是两个整数恰好整除，结果也是浮点数
    a=1.24
    b=3.33
    c=1.23e9  # 1.23x10的9次方
    d=1.2e-5  # 0.000012

    
    # 特有的运算符
    # 2**3 == 8 次幂
    # 9 // 2 == 4 除法取整, 称为地板除，两个整数的除法仍然是整数, 只取结果的整数部分

    # 转换
    age_str="11"
    age_num=int(age_str)
    print(age_num)

    # 判断是否可以转换为整型
    a="1,2,3"
    print(a.isdecimal())  # False
    print(a.isdigit())  # False
    print("九".isnumeric())  # True

```

### 布尔值 空值


```py
# 布尔值
    # 只有True、False两种值
    # 可以用and or not运算

    # 空值
    # 用None表示。None不能理解为0

```

### 集合


#### 有序可变 list

```py
 #
    #
    ##### List 可变有序列表
    #
    #
    #
    classmates = ['Michael', 'Bob', 'Tracy']
    print("classmates = ", classmates) # ['Michael', 'Bob', 'Tracy']
    print (len(classmates))

    # 获取
    # # 获取第一个
    # >>> classmates[0]
    # 'Michael'
    # # 最后一个元素
    # >>> classmates[-1] 
    # or classmates[len(classmates) - 1]
    # 'Tracy'
    # # 以此类推，可以获取倒数第2个、倒数第3个：
    # >>> classmates[-2]
    # 'Bob'

    # 查找
    # index() 函数用于从列表中找出某个值第一个匹配项的索引位置
        
    # # 添加到末尾
    # >>> classmates.append('Adam')
    # >>> classmates ['Michael', 'Bob', 'Tracy', 'Adam']
    # # 添加到指定位置
    # >>> classmates.insert(1, 'Jack')
    # >>> classmates
    # ['Michael', 'Jack', 'Bob', 'Tracy', 'Adam']
    # # 删除末尾元素
    # >>> classmates.pop()
    # 'Adam'
    # # 删除指定位置的元素
    # >>> classmates.pop(1)
    # 'Jack'


    #
    # 带下标遍历
    # enumerate()可以将list变为索引-元素对
    # for i, value in enumerate([1,2,'3', '4']):
    #     print(i, value)
    #
    # 判断对象是否能迭代
    # from collections import Iterable
    # if (isinstance("acb", Iterable)):
    #     print('ok!')


    

    #
    # 列表生成式
    #
    l = list(range(1,11))
    print(l)
    l1 = [x*x for x in l]
    print(l1)
    l2 = [x*x for x in l if x%2 ==0]
    print(l2)

    m = [x+y for x in 'abc' for y in 'ABC']
    print(m)
    import os
    d = [d for d in os.listdir('.')]
    print(d)
```

#### 切片 slices

```py
# slices
    #
    #
    l = list(range(10))
    print(l)
    print(l[0:3])
    print(l[:3])
    print(l[-1])
    print(l[-1:])
    print(l[-2:-1])
    print(l[-2:])
```


#### 有序不可变列表 tuple

```py
 # Tuple 不可变有序列表
    #
    # 因为tuple不可变，所以代码更安全。如果可能，能用tuple代替list就尽量用tuple
    # 
    # 创建
    # >>> classmates = ('Michael', 'Bob', 'Tracy')
    # # 定义空
    # >>> t = ()
    # # 定义一个元素的tuple
    # 为什么这样规定?
    # 这是因为括号()既可以表示tuple，又可以表示数学公式中的小括号，这就产生了歧义.所以，只有1个元素的tuple定义时必须加一个逗号,，来消除歧义
    # >>> t = (1,)
    # >>> t
    # (1,)
    # # 可变的tuple
    # >>> t = ('a', 'b', ['A', 'B'])
    # >>> t[2][0] = 'X'
    # >>> t[2][1] = 'Y'
    # >>> t
    # ('a', 'b', ['X', 'Y'])
```

#### 无序不可重复 set

```py
  # Set 无序不可重复
    #
    #一组 key 的集合，但不存储 value，没有重复的 key，无序
    #set 和 dict 的唯一区别仅在于没有存储对应的 value
    #
    # 要创建一个set，需要提供一个list作为输入集合：
    # >>> s = set([1, 2, 3])
    # >>> s
    # {1, 2, 3}
    # # 或者直接 s = {1,2,3}创建
    # # 重复元素在set中自动被过滤
    # >>> s = set([1, 1, 2, 2, 3, 3])
    # >>> s
    # {1, 2, 3}
    # # 添加元素，重复添加无效
    # >>> s.add(4)
    # >>> s
    # {1, 2, 3, 4}
    # >>> s.add(4)
    # >>> s
    # {1, 2, 3, 4}
    # # 删除
    # >>> s.remove(4)
    # >>> s
    # {1, 2, 3}
    # # 并集交集
    # >>> s1 = set([1, 2, 3])
    # >>> s2 = set([2, 3, 4])
    # >>> s1 & s2
    # {2, 3}
    # >>> s1 | s2
    # {1, 2, 3, 4}

```

###  键值存储 字典 dict

```py
# Dict 键值存储
    #
    # 和 list 比较，dict 有以下几个特点：
    #
    # 1.  查找和插入的速度极快，不会随着 key 的增加而变慢；
    # 2.  需要占用大量的内存，内存浪费多。
    # 3.  Key 是不可变对象 
    #     需要我们自己在代码中保证 , 因为dict根据key来计算value的存储位置，如果每次计算相同的key得出的结果不同，那dict内部就完全混乱了
    #     在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。而list是可变的，就不能作为key
    #
    # 而 list 相反：
    # 1.  查找和插入的时间随着元素的增加而增加；
    # 2.  占用空间小，浪费内存很少。
    # 所以，dict 是用空间来换取时间的一种方法。
    #
    # 创建
    # >>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
    # # 获取值，如果不存在会报错
    # >>> d['Michael']
    # 95
    # # 添加键值对
    # >>> d['Adam'] = 67
    # >>> d['Adam']
    # 67
    # # 添加相同的键，会冲掉已存在的
    # >>> d['Jack'] = 90
    # >>> d['Jack']
    # 90
    # >>> d['Jack'] = 88
    # >>> d['Jack']
    # 88 # 为了防止报错，先判断是否有该键
    # >>> 'Thomas' in d
    # False
    # # 或者通过get方法，如果不存在，没有返回
    # >>> d.get('Thomas')
    # >>> d.get('Thomas', -1)
    # -1
    # # 删除
    # >>> d.pop('Bob')
    # 75
    # >>> d
    # {'Michael': 95, 'Tracy': 85}
    #
    # 迭代 遍历 dict
    #
    d = {'a': 1, 'b': 2, 'c': 3}
    for key in d:
        print(key)
    for value in d.values():
        print(value)
    # 同时迭代key和value，这种太麻烦，不推荐
    for key in d:
        print(key + ': ' + str(d.get(key)))

    for key,value in d.items():
        print(key, str(value))
    
```

### 生成器 generator

```py
# 生成器
    # 这种一边循环一边计算的机制，称为生成器：generator。
    #
    # 把一个列表生成式的[]改成()，就创建了一个generator
    #
    #或者 -> 如果一个函数定义中包含yield关键字，那么这个函数就不再是一个普通函数，而是一个generator
    #
    # 函数区别:generator和函数的执行流程不一样。函数是顺序执行，
    # 遇到return语句或者最后一行函数语句就返回。而变成
    # generator的函数，在每次调用next()的时候执行(如 next(g))，遇
    # 到yield语句返回，再次执行时从上次返回的yield语句处继续执行
    #
    l = list(range(1,11))
    g = (x*x for x in l)
    for n in g:
        print(n)

    # 斐波拉契数列
    def fib(max):
        n, a, b = 0, 0, 1
        while n < max:
            yield b
            a, b = b, a + b
            n = n + 1
        return 'done' # 用for循环调用generator时，发现拿不到generator的return语句的返回值。如果想要拿到返回值，必须捕获StopIteration错误，返回值包含在StopIteration的value中
    for n in fib(6):
        print(n)

```

## 条件循环

```py
def condition_loop():
    """
    条件 循环
    """
    names = ['Michael', 'Bob', 'Tracy']
    for name in names:
        print(name)

    fruits = ['banana', 'apple',  'mango']
    for index in range(len(fruits)):
        print('当前水果 :', fruits[index])

    count = 0
    while (count < 9):
        print ('The count is:', count)
        count = count + 1
```

## 比较判断

```py
# 在python中 None, False, 空字符串"", 0, 空列表[], 空字典{}, 空元组()都相当于False
# (ob1 is ob2) 等价于 (id(ob1) == id(ob2))

# 判空
if x is None
if x is not None


```

## 函数

### 参数检查

```py
def function_demo():
    # 参数检查类型
    def my_abs(x):
        if not isinstance(x, (int, float)):
            raise TypeError('bad operand type')
        if x >= 0:
            return x
        else:
            return -x

    # * 检查参数有效性
    def person(name, age, **kw):
        if 'city' in kw:
            # 有city参数
            pass
        if 'job' in kw:
            # 有job参数
            pass
        print('name:', name, 'age:', age, 'other:', kw)

```

### 多种参数

```py

    # 参数顺序
    #
    #必选参数、默认参数、可变参数、命名关键字参数、关键字参数
    #
    # 定义
    # c 为默认参数
    # *args 为 可变参数, 个数可变, 函数内部自动组装为 tuple
    # **kw  关键字参数, 内部自动组装为一个 dict
    #
    # 所以，对于任意函数，都可以通过类似func(*args, **kw)的形式调用它，无论它的参数是如何定义的。
    #
    def f1(a, b, c=0, *args, **kw):
        print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

    def f2(a, b, c=0, *, d, **kw):
        print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)
    # 调用
    # >>> f1(1, 2)
    # a = 1 b = 2 c = 0 args = () kw = {}
    # >>> f1(1, 2, c=3)
    # a = 1 b = 2 c = 3 args = () kw = {}
    # >>> f1(1, 2, 3, 'a', 'b')
    # a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}
    # >>> f1(1, 2, 3, 'a', 'b', x=99)
    # a = 1 b = 2 c = 3 args = ('a', 'b') kw = {'x': 99}
    # >>> f2(1, 2, d=99, ext=None)
    # a = 1 b = 2 c = 0 d = 99 kw = {'ext': None}

    # 通过一个tuple和dict，你也可以调用上述函数：
    # >>> args = (1, 2, 3, 4)
    # >>> kw = {'d': 99, 'x': '#'}
    # >>> f1(*args, **kw)
    # a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}
    # >>> args = (1, 2, 3)
    # >>> kw = {'d': 88, 'x': '#'}
    # >>> f2(*args, **kw)
    # a = 1 b = 2 c = 3 d = 88 kw = {'x': '#'}

    #
    # 命名关键字参数
    #可以限制传入的参数的键
    #例如，只接收 city 和 job 作为关键字参数。这种方式定义的函数如下：
    #
    def person1(name, age, *, city, job):# * 作为分隔符
        print(name, age, city, job)

    # # 调用方式如下：
    # # 必需要有参数名，如果没有，会视为有四个位置参数，而这个函数只有两个位置参数，报错
    # >>> person1('Jack', 24, city='Beijing', job='Engineer')
    # Jack 24 Beijing Engineer
    #
    # 如果函数定义中已经有了一个可变参数，后面跟着的命名关键字参数就不再需要一个特殊分隔符*了：
    #
    def person2(name, age, *args, city, job):
        print(name, age, args, city, job)

```

### 递归

```py

    ###
    ##
    ## 递归
    # 尾递归优化: 使得 返回值不含有表达式
    #
    # 阶乘
    def recu(n):
        if n == 1:
            return 1
        return n*recu(n-1) # 返回值带有表达式, 不好
    # 优化后
    def fact(n):
        return fact_iter(n, 1)

    def fact_iter(num, product):
        if num == 1:
            return product
        return fact_iter(num - 1, num * product)

```


### 高阶函数


```py
def map_reduce_closure_lambda():
    l = list(range(1,11))
    def f(x):
        return x*x
    r = map(f, l) # <map object at 0x10298b2d0>
    print("r = ", list(r))# r =  [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
    r1 = map(str, l)
    print(list(r1)) # ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10']

    from functools import reduce
    l = [1, 2, 3]
    def add(x, y):
        return x+y
    r2 = reduce(add, l)
    print("r2 = ", r2) # 6


    n = sorted([36, 5, -12, 9, -21])
    print(n) # [-21, -12, 5, 9, 36]
    n1 = sorted(['bob', 'about', 'Zoo', 'Credit'],reverse=True, key=str.lower)
    print(n1) # ['Zoo', 'Credit', 'bob', 'about']

    def lazy_sum(*args):
        def sum():
            ax = 0
            for n in args:
                ax = ax + n
            return ax
        return sum
    f = lazy_sum(1, 3, 5, 7, 9)
    print("f() = ", f()) # 25

    lambda x: x*x


```

## 文件处理

### 创建文件

```py
def file_dir_demo():
    import os
    import sys
    print(os.name) # 操作系统类型, 如果是posix，说明系统是Linux、Unix或Mac OS X，如果是nt，就是Windows系统
    print(os.environ.get('PATH', ['default Value'])) # 在操作系统中定义的环境变量
    # 查看当前目录的绝对路径:
    print(os.path.abspath('.')) # /Users/xiaoyu/repo/code/back_end/python-demos

    # 路径拼接
    #
    # 把两个路径合成一个时，不要直接拼字符串，而要通过os.path.join()函数，这样可以正确处理不同操作系统的路径分隔符
    #os.path.join('/Users/michael', 'testdir')
    #'/Users/michael/testdir'
    #
    # 拆分
    #
    # 要拆分路径时，也不要直接去拆字符串，而要通过os.path.split()函数，这样可以把一个路径拆分为两部分，后一部分总是最后级别的目录或文件名
    #>>> os.path.split('/Users/michael/testdir/file.txt')
    #('/Users/michael/testdir', 'file.txt')

    # 创建一个目录:
    #>>> os.mkdir('/Users/michael/testdir')
    # 删掉一个目录:
    #>>> os.rmdir('/Users/michael/testdir')

    # 文件拓展名
    #
    # os.path.splitext()可以直接让你得到文件扩展名
    # >>> os.path.splitext('/path/to/file.txt')
    # ('/path/to/file', '.txt')

    #  对文件重命名:
    # >>> os.rename('test.txt', 'test.py')
    # 删掉文件:
    # >>> os.remove('test.py')

    # 复制文件
    #
    # shutil模块提供了copyfile()的函数，你还可以在shutil模块中找到很多实用函数，它们可以看做是os模块的补充

    # 列出当前目录下的所有目录
    # os.listdir('.')
    #
    # >>> [x for x in os.listdir('.') if os.path.isdir(x)]
    # ['.lein', '.local', '.m2', '.npm', '.ssh', '.Trash', '.vim', 'Applications', 'Desktop', ...]

    # 列出所有的.py文件
    # >>> [x for x in os.listdir('.') if os.path.isfile(x) and os.path.splitext(x)[1]=='.py']
    # ['apis.py', 'config.py', 'models.py', 'pymonitor.py', 'test_db.py', 'urls.py', 'wsgiapp.py']

    # path 路径 对比    
    #
    print(__file__)  # D:/repo/repo_github/shared_already/python-demos/module_test.py
    print(os.path.dirname(__file__))  # D:/repo/repo_github/shared_already/python-demos
    curPath = os.path.abspath(os.path.dirname(__file__))
    print(curPath)  # D:\repo\repo_github\shared_already\python-demos
    rootPath = os.path.split(curPath)[0]
    print(rootPath)  # D:\repo\repo_github\shared_already
    # sys.path.append(curPath)
    sys.path.append(rootPath)
    print(sys.path)


```

### 读写文件数据

```py
def read_write_file():
    """
    open(file, mode='r', buffering=-1, encoding=None, errors=None, newline=None, closefd=True, opener=None)
        file: 必需，文件路径（相对或者绝对路径）。
        mode: 可选，文件打开模式
            r 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。
            x 写模式，新建一个文件，如果该文件已存在则会报错。
            b 二进制模式。
            w 打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。
            a 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件进行写入。
            t 文本模式(默认)
            + 打开一个文件进行更新(可读可写)。
        buffering: 设置缓冲
            通过0以关闭缓冲（仅在二进制模式下允许）
            1 选择行缓冲（仅在文本模式下可用）
            当未给出buffers参数时，默认缓冲策略工作如下:
                
        encoding: 一般使用utf8
            如果未指定encoding，则使用的编码取决于平台: locale.getpreferredencoding（False）
        errors: 报错级别, 常用 'ignore'
        newline: 区分换行符
            可以是None、''、'\n'、'\r'、'\r\n'
            读取数据时, 
                如果newline参数为None, 文件中的'\n'，'\r\n'，'\r'，在返还给调用者的时候都会被转化为'\n'
                如果newline = '', 但是行的结尾符号并不会被转化
                如果它具有任何其它合法值，则输入行仅由给定字符串终止，
            写入时
                如果不设置newline参数, 即为 None 时, 任何'\n'都会被翻译成当前系统的line separator（也就是os.linesep)
                如果参数为''或者'\n'，不会有任何翻译。
        closefd: 传入的file参数类型
        opener:
    """
    f=None
    try:
        # read()会一次性读取文件的全部内容，如果文件有10G，内存就爆了，
        #所以，要保险起见，可以反复调用read(size)方法，每次最多读取size个字节的内容。
        # r 表示 模式为 只读, 还有 w 写, rw 读写, rb/wb 二进制
        f = open('./readme.md', 'r')
        print(f.read())
    finally:
        if f:
            f.close()

    # 如果文件很小，read()一次性读取最方便；如果不能确定文件大小，反复调用read(size)比较保险；如果是配置文件，调用readlines()最方便：
    # 读取配置文件
    for line in f.readlines():
        print(line.strip()) # 把末尾的'\n'删掉

    # 更好的方法 ---- 自动关闭 f
    #
    # 但是每次都这么写实在太繁琐，所以，Python引入了with语句来自动帮我们调用close()
    with open('./readme.md', 'r') as f:
        print(f.read())


    #要读取非UTF-8编码的文本文件，需要给open()函数传入encoding参数
    # >>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk')
    # >>> f.read()
    # '测试'

    # 错误处理
    #直接忽略
    f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')

    # 读二进制文件
    f = open('/Users/michael/test.jpg', 'rb')
    f.read()
    # b'\xff\xd8\xff\xe1\x00\x18Exif\x00\x00...' # 十六进制表示的字节

    # 写二进制
    # 如果文件已存在，会直接覆盖, 可以传入'a'以追加（append）模式写入
    f = open('/Users/michael/test.txt', 'w')
    # 可以反复调用write()来写入文件
    f.write('Hello, world!')
    # 写文件时，操作系统往往不会立刻把数据写入磁盘，而是放到内存缓存起来，空闲的时候再慢慢写入。
    #只有调用close()方法时，操作系统才保证把没有写入的数据全部写入磁盘
    f.close()
    # 所以，还是用with语句方便

    #
    # 在内存读写 stringIO
    # 要把str写入StringIO，我们需要先创建一个StringIO，然后，像文件一样写入
    # >>> from io import StringIO
    # >>> f = StringIO() # 可以在这里直接初始化, 类比 StringBuffer
    # >>> f.write('hello')
    # 5
    # >>> f.write(' ')
    # 1
    # >>> f.write('world!')
    # 6
    # >>> print(f.getvalue())
    # hello world!


    # # 要操作二进制数据，就需要使用BytesIO
    # >>> from io import BytesIO
    # >>> f = BytesIO()
    # >>> f.write('中文'.encode('utf-8'))
    # 6
    # >>> print(f.getvalue())
    # b'\xe4\xb8\xad\xe6\x96\x87'

    # 序列化
    #
    # dumps()方法返回一个str，内容就是标准的JSON。类似的，dump()方法可以直接把JSON写入一个file-like Object。
    # >>> import json
    # >>> d = dict(name='Bob', age=20, score=88)
    # >>> json.dumps(d)
    # '{"age": 20, "score": 88, "name": "Bob"}'


```


## 装饰器


```py

def wrapper_decorator():
    """
    装饰器
    """
    def now():
        print('2015-1-1')
    now()
    print(now.__name__)
    f = now
    f()
    print(f.__name__)

    print('---------------')

    import functools
    # 本质上, decorator就是一个返回函数的高阶函数, 接受一个函数, 返回包装后的函数
    def log(func):
        @functools.wraps(func) # 作用是修改装饰后的函数 __name__ 属性值, 如果不加, 装饰器还是工作的, 但是 __name__ 会变为 "wrapper", 显然不行
        def wrapper(*args, **kw):
            print('call %s(): ' % func.__name__)
            return func(*args, **kw)
        return wrapper
    # 把@log放到new()函数的定义处，相当于执行了 new = log(new)
    @log
    def new():
        print('111-111-111')
    new() # 执行的是包装后的函数

    # 如果decorator本身需要传入参数，那就需要编写一个返回decorator的高阶函数
    def log1(text):
        def decorator(func):
            @functools.wraps(func)
            def wrapper(*args, **kw):
                print('%s %s():' % (text, func.__name__))
                return func(*args, **kw)
            return wrapper
        return decorator
    @log1('自定义execute') # 相当于 yyy =  log1('execute')(yyy)
    def yyy():
        print('zzz')
    yyy()
```


## 模块 作用域

```py

hehe=6

def var_demo():
    # 作用域名
    # 函数定义了本地作用域，而模块定义的是全局作用域, 如果想要在函数内定义/修改全局作用域，需要加上global修饰符
    global hehe
    print(hehe) # 6
    hehe=3 # 修改
    pass




def module_demo():
    """
    -   库 就是一个项目, 分为标准库, 第三方库(https://pypi.org/)
    -   包 就是包含 `__init__.py` 的目录, 可能有多个子包, 也必须包含 init 文件
    -   每个 `*py` 就是一个 模块
    python 有自己的模块搜索路径, 但是怎么引入自定义的模块搜索路径呢?
    # 查看默认路径
    # 查找顺序为:
    # 1. 程序所在目录
    # 2. python 安装标准库目录(lib)
    # 3. 第三方库目录 (site-package)
    import sys
    print(sys.path)
    # 一次性自定义
    sys.path.append('/Users/michael/my_py_scripts')
    # 永久
    设置环境变量PYTHONPATH，Python自身搜索路径不受影响
    """
    # 方式 1:
    # from xx.yy import hello
    # hello()

    # 方式 2:
    import xx.yy as yy
    yy.hello()

```

## 面向对象

### 类

```py
def oop():
    class Student(object):

        name__xx = "Student" # 类属性, 实例属性 类属性千万不要同名, 因为查找实例属性若没找到, 会返回类属性

        __slots__ = ('name', '__score', '_age') # 用tuple定义允许绑定的属性名称
        # `__slots__`定义的属性仅对当前类实例起作用，对继承的子类是无限制的 
        #除非在子类中也定义**slots**，这样，子类实例允许定义的属性就是自身的**slots**加上父类的**slots**

        # 构造方法
        # > **init**有两个下划线
        # > 定义对象方法第一个参数必须是 self，调用时不用传入 self
        #
        # `__len__()`:len(obj)实际调用 obj 的`__len__()` 
        #
        # `__iter__() & __next__()`:如果一个类想被用于 for ... in 循环,则必须实现`__iter__()`，返回一个迭代对象,`__next__()`方法拿到循环的下一个值
        #
        # `__getitem__ & __setitem__() & __delitem__()`:实现 list 按下标取元素的功能，则必须实现  
        #
        # `__getattr__（）`:动态返回属性
        #
        def __init__(self, name, score):
            self.name = name
            self.__score = score

        def display(self):
            print('%s: %s' % (self.name, self.__score))

        # 增加getter和setter
        def get_score(self):
            return self.__score

        def set_score(self, score):
            self.__score = score

        @property # 负责把一个方法变成属性调用
        def age(self):
            return self._age
        @age.setter # @property本身又创建了另一个装饰器@score.setter,把一个setter方法变成属性赋值
        def age(self, v):
            if not isinstance(v, int):
                raise ValueError('score must be an integer!')
            if v < 0 or v > 100:
                raise ValueError('score must between 0 ~ 100!')
            self._age = v

        def __str__(self): # `__str__()`:print(obj)实际调用 obj 的`__str__()`  
            return 'Student object (name=%s)' % self.name
        __repr__ = __str__# `__repr__()`:直接 obj 调用 obj 的`__repr__()`,通常和 `__str__()`一样

    s1 = Student('s1', 10)
    s1.display()
    print(s1)
    
    # 加了 `__slot__`, 不允许在外部绑定任何变量
    # s1.sex = 'male'
    # print(s1.sex)

     # 无法访问私有的
    # 不能直接访问__name是因为Python解释器对外把__name变量改成了_Student__name，所以，仍然可以通过_Student__name来访问__name变量
    #print(s1.__score)
    print(s1.get_score())
    s1.set_score(11) # ok
    # s1.__score =11 # error

    s1.age = 10
    print("age = ", s1.age)

```



### 继承 鸭子类型


```py

 # 
    # 继承
    #
    class People(object):
        def say(self):
            print("hello people")
        def run(self):
            print("run people")

    class Student(People):  # 继承
        def run(self): # 重写
            print("run students")

    stu = Student()
    stu.say() # hello people
    stu.run() # run students

    print(isinstance(stu, People)) # true
    print(isinstance(stu, Student))# true

    #
    # 鸭子类型
    #
    class Animal(object):
        def eat(self):
            print("animal eat")
        def run(self):
            print("run animal")
    class Bird(object):
        def eat(self):
            print("eat bird")
    print(isinstance(Bird(), Animal))# false
    def demo(p):
        p.run()
    demo(stu)
    demo(People())
    demo(Animal()) # animal 没有继承 People, 也可作为参数传入 demo(), 鸭子类型


```

### 判断类型信息


```py
#
    # 类型信息
    #
    #>>> type(123)==type(456)
    # True
    # >>> type(123)==int
    # True
    # >>> type('abc')==type('123')
    # True
    # >>> type('abc')==str
    # True
    # >>> type('abc')==type(123)
    # False

    # # 其他类型
    # >>> import types
    # >>> def fn():
    # ...     pass
    # ...
    # >>> type(fn)==types.FunctionType
    # True
    # >>> type(abs)==types.BuiltinFunctionType
    # True
    # >>> type(lambda x: x)==types.LambdaType
    # True
    # >>> type((x for x in range(10)))==types.GeneratorType
    # True

    # # isinstance()
    # # 基本类型
    # >>> isinstance('a', str)
    # True
    # >>> isinstance(123, int)
    # True
    # >>> isinstance(b'a', bytes)
    # True
    # # 继承体系
    # >>> isinstance(h, Dog)
    # True
    # >>> isinstance(h, Animal)
    # True
    # >>> isinstance(d, Dog) and isinstance(d, Animal)
    # True
    # # 判断是几种中之一种
    # >>> isinstance([1, 2, 3], (list, tuple))
    # True
    # >>> isinstance((1, 2, 3), (list, tuple))
    # True

```

### 动态操作


```py
# 
    # 动态操作 object 属性
    #
    # dir(obj)---------获取 obj 的所有对象、属性，返回一个字符串 list  
    # 配合 getattr()、setattr()以及 hasattr()，我们可以直接操作一个对象的状态
    #
    # >>> class MyObject(object):
    # ...     def __init__(self):
    # ...         self.x = 9
    # ...     def power(self):
    # ...         return self.x * self.x
    # ...
    # >>> obj = MyObject()
    # >>> hasattr(obj, 'x') # 有属性'x'吗？
    # True
    # >>> obj.x
    # 9
    # >>> hasattr(obj, 'y') # 有属性'y'吗？
    # False
    # >>> setattr(obj, 'y', 19) # 设置一个属性'y'
    # >>> hasattr(obj, 'y') # 有属性'y'吗？
    # True
    # >>> getattr(obj, 'y', 404) # 获取属性'y',如果不存在，返回默认值404
    # 19
    # >>> obj.y # 获取属性'y'
    # 19
    # # 获取对象
    # >>> hasattr(obj, 'power') # 有属性'power'吗？
    # True
    # >>> getattr(obj, 'power') # 获取属性'power'
    # <bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
    # >>> fn = getattr(obj, 'power') # 获取属性'power'并赋值到变量fn
    # >>> fn # fn指向obj.power
    # <bound method MyObject.power of <__main__.MyObject object at 0x10077a6a0>>
    # >>> fn() # 调用fn()与调用obj.power()是一样的
    # 81



 #
    # 动态创建类, 元类, metaClass
    #
    # 创建class
    class Hello(object):
        def hello(self, name='world'):
            print('Hello, %s.' % name)

    h = Hello()
    print(type(h))# <class '__main__.Hello'> h的类型是Hello
    print(type(Hello))# <class 'type'> Hello的类型是type

    # 动态创建class
    def fn(self, name='world'):
        print('Hello, %s.' % name)
    Hello1 = type('Hello1', (object,), dict(hello=fn))
    h1 = Hello1()
    print(h1) # <__main__.Hello1 object at 0x7faef9cd5c90>
    h1.hello() # Hello, world.

```



### 特殊方法 

魔术方法

```py

#
    # 特殊方法
    #
    #
    # 应用：REST API的链式调用
    class Chain(object):

        def __init__(self, path=''):
            self._path = path

        def __getattr__(self, path):
            return Chain('%s/%s' % (self._path, path))

        def __str__(self):
            return self._path

        __repr__ = __str__

    # >>> Chain().status.user.timeline.list
    # '/status/user/timeline/list'

    # `__call__（）`:通过实例本身调用的方法-------------把对象看成函数  
    # 带有`__call__（）`的对象是`Callable`对象，如何判断？  
    # 通过 callable(obj)，我们就可以判断一个 obj 是否是“可调用”对象
    #
    class Student(object):
        def __init__(self, name):
            self.name = name

        def __call__(self):
            print('My name is %s.' % self.name)
    # >>> s = Student('Michael')
    # >>> s() # self参数不要传入
    # My name is Michael.



```


### 枚举


```py


 #
    # 枚举类
    #
    #    
    # 枚举成员可进行同一性比较  
    # Color.red is Color.red  
    # 　　输出结果是：True  
    # Color.red is not Color.blue  
    # 　　输出结果是：True  
    # 枚举成员可进等值比较  
    # Color.blue == Color.red  
    # 　　输出结果是：False  
    # Color.blue != Color.red  
    # 　　输出结果是：True
    from enum import Enum, unique
    # simple way to define, 默认赋值1，2，3，4...
    Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
    print(Month.Jan.value) # 1
    print(Month.Jan.name) #Jan
    print(Month.Jan)#Month.Jan
    print(Month['Jan'])# 通过名称获取成员,  # Month.Jan
    print(Month(1))# 通过值获取成员 # Month.Jan
    print('----------------')
    for month in Month:
        print(month)
    print('----------------')
    for month in Month.__members__.items():
        print(month[0], month[1])
    print('----------------')
    for name, value in Month.__members__.items():
        print(name, value)

print("------------------------------")
    # customize class,可自己赋值
    @unique
    class Weekday(Enum):
        Sun = 0
        Mon = 1
        Tue = 2
        Wed = 3
        Thu = 4
        Fri = 5
        Sat = 6
    for name, value in Weekday.__members__.items():
        print(name, value)

```

## 错误异常处理


```py
def error_handling():
    """
    BaseException
    +-- SystemExit
    +-- KeyboardInterrupt
    +-- GeneratorExit
    +-- Exception
        +-- StopIteration
        +-- StopAsyncIteration
        +-- ArithmeticError
        |    +-- FloatingPointError
        |    +-- OverflowError
        |    +-- ZeroDivisionError
        +-- AssertionError
        +-- AttributeError
        +-- BufferError
        +-- EOFError
        +-- ImportError
            +-- ModuleNotFoundError
        +-- LookupError
        |    +-- IndexError
        |    +-- KeyError
        +-- MemoryError
        +-- NameError
        |    +-- UnboundLocalError
        +-- OSError
        |    +-- BlockingIOError
        |    +-- ChildProcessError
        |    +-- ConnectionError
        |    |    +-- BrokenPipeError
        |    |    +-- ConnectionAbortedError
        |    |    +-- ConnectionRefusedError
        |    |    +-- ConnectionResetError
        |    +-- FileExistsError
        |    +-- FileNotFoundError
        |    +-- InterruptedError
        |    +-- IsADirectoryError
        |    +-- NotADirectoryError
        |    +-- PermissionError
        |    +-- ProcessLookupError
        |    +-- TimeoutError
        +-- ReferenceError
        +-- RuntimeError
        |    +-- NotImplementedError
        |    +-- RecursionError
        +-- SyntaxError
        |    +-- IndentationError
        |         +-- TabError
        +-- SystemError
        +-- TypeError
        +-- ValueError # int() 可能发生
        |    +-- UnicodeError
        |         +-- UnicodeDecodeError
        |         +-- UnicodeEncodeError
        |         +-- UnicodeTranslateError
        +-- Warning
            +-- DeprecationWarning
            +-- PendingDeprecationWarning
            +-- RuntimeWarning
            +-- SyntaxWarning
            +-- UserWarning
            +-- FutureWarning
            +-- ImportWarning
            +-- UnicodeWarning
            +-- BytesWarning
            +-- ResourceWarning
    """
    # 通过配置，logging还可以把错误记录到日志文件里，方便事后排查
    import logging

    try:
        print('try...')
        r = 10 / int('2')
        print('result:', r)
    except ValueError as e:
        print('ValueError:', e)
        logging.exception(e)
    except ZeroDivisionError as e:
        print('ZeroDivisionError:', e)
    # 没有错误发生就执行
    else:
        print('no error!')
    finally:
        print('finally...')
    print('END')


    # 捕获然后原样抛出
    # raise语句如果不带参数，就会把当前错误原样抛出
    def foo(s):
        n = int(s)
        if n==0:
            print("can not be 0")
            raise ValueError('invalid value: %s' % s)
        return 10 / n

    def bar():
        try:
            foo('0')
        except ValueError as e:
            print('ValueError!--------------------')
            raise

    try:
        bar()
    except Exception as e:
        print("handling<<<<<<<<<<<<<")

    # 自定义错误
    #
    # 
    class FooError(ValueError):
        pass

    def foo(s):
        n = int(s)
        if n==0:
            raise FooError('invalid value: %s' % s)
        return 10 / n

    # foo('0')

```

# 日志

```py
def log_handling():
    import logging
    # 有debug，info，warning，error等几个级别，等级由低到高
    # 设定为某个级别, 低于这个级别的不会输出
    logging.basicConfig(level=logging.INFO)
    s = '0'
    n = int(s)
    logging.info('n = %d' % n)
    # print(10 / n)

```

# 单元测试

```py
def unit_test():
    # 编写一个Dict类，这个类的行为和dict一致，但是可以通过属性来访问
    class Dict(dict):

        def __init__(self, **kw):
            super().__init__(**kw)

        def __getattr__(self, key):
            try:
                return self[key]
            except KeyError:
                raise AttributeError(r"'Dict' object has no attribute '%s'" % key)

        def __setattr__(self, key, value):
            self[key] = value
            
    import unittest

    class TestDict(unittest.TestCase): # 从unittest.TestCase继承

        # 两个特殊的setUp()和tearDown()方法。这两个方法会分别在每调用一个测试方法的前后分别被执行
        def setUp(self):
            print('setUp...')

        def tearDown(self):
            print('tearDown...')

        # 以test开头的方法就是测试方法，不以test开头的方法不被认为是测试方法，测试的时候不会被执行。
        def test_init(self):
            d = Dict(a=1, b='test')
            self.assertEqual(d.a, 1)
            self.assertEqual(d.b, 'test')
            self.assertTrue(isinstance(d, dict))

        def test_key(self):
            d = Dict()
            d['key'] = 'value'
            self.assertEqual(d.key, 'value')

        def test_attr(self):
            d = Dict()
            d.key = 'value'
            self.assertTrue('key' in d)
            self.assertEqual(d['key'], 'value')

        def test_keyerror(self):
            d = Dict()
            # 期待抛出指定类型的Error，
            with self.assertRaises(KeyError):
                value = d['empty']

        def test_attrerror(self):
            d = Dict()
            with self.assertRaises(AttributeError):
                value = d.empty
    # 运行测试, 或者 命令行 python -m unittest mydict_test
    unittest.main()


def doc_test():
    """
    文档测试 doctest
    """
    class Dict(dict):
        '''
        Simple dict but also support access as x.y style.
        >>> d1 = Dict()
        >>> d1['x'] = 100
        >>> d1.x
        100
        >>> d1.y = 200
        >>> d1['y']
        200
        >>> d2 = Dict(a=1, b=2, c='3')
        >>> d2.c
        '3'
        >>> d2['empty']
        Traceback (most recent call last):
            ...
        KeyError: 'empty'
        >>> d2.empty
        Traceback (most recent call last):
            ...
        AttributeError: 'Dict' object has no attribute 'empty'
        '''
        def __init__(self, **kw):
            super(Dict, self).__init__(**kw)

        def __getattr__(self, key):
            try:
                return self[key]
            except KeyError:
                raise AttributeError(r"'Dict' object has no attribute '%s'" % key)

        def __setattr__(self, key, value):
            self[key] = value

    import doctest
    doctest.testmod()



```


# 多进程

gevent

```py
def multi_processing():
    """
    在Unix/Linux下，可以使用fork()调用实现多进程。
    要实现跨平台的多进程，可以使用multiprocessing模块。
    进程间通信是通过Queue、Pipes等实现的。
    subprocess模块可以让我们非常方便地启动一个子进程，然后控制其输入和输出。通过communicate()方法输入
    
    """

    import os
    print("主进程 pid = ", os.getpid())
    # fork()调用一次，返回两次，因为操作系统自动把当前
    # 进程（称为父进程）复制了一份（称为子进程），然后，分别在父进程和子进程内返回 process id
    # 子进程永远返回0，而父进程返回子进程的ID
    pid = os.fork()
    if pid ==0:
        print("子进程, id = {}, ppid = {}".format(os.getpid(), os.getppid()))
    else:
        print("父进程, id = {}, son pid = {}".format(os.getpid(), pid))


def multi_processing_windows():
    """跨平台的多进程"""
    from multiprocessing import Process
    import os

    # 子进程要执行的代码
    def run_proc(name):
        print('Run child process %s (%s)...' % (name, os.getpid()))

    print('Parent process %s.' % os.getpid())
    p = Process(target=run_proc, args=('test',))
    p.start()

    p.join() # main process 
    print('Child process end.')

    # 进程池
    #
    from multiprocessing import Pool
    import os, time, random

    def long_time_task(name):
        print('Run task %s (%s)...' % (name, os.getpid()))
        start = time.time()
        time.sleep(random.random() * 3)
        end = time.time()
        print('Task %s runs %0.2f seconds.' % (name, (end - start)))

    if __name__=='__main__':
        print('Parent process %s.' % os.getpid())
        p = Pool(4) # Pool的默认大小是CPU的核数
        for i in range(5):
            p.apply_async(long_time_task, args=(i,))
        print('Waiting for all subprocesses done...')
        p.close()
        p.join()
        print('All subprocesses done.')


def process_comunicate():
    """进程通信"""
    from multiprocessing import Process, Queue
    import os, time, random

    # 写数据进程执行的代码:
    def write(q):
        print('Process to write: %s' % os.getpid())
        for value in ['A', 'B', 'C']:
            print('Put %s to queue...' % value)
            q.put(value)
            time.sleep(random.random())

    # 读数据进程执行的代码:
    def read(q):
        print('Process to read: %s' % os.getpid())
        while True:
            value = q.get(True)
            print('Get %s from queue.' % value)

    # 父进程创建Queue，并传给各个子进程：
    q = Queue()
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    # 启动子进程pw，写入:
    pw.start()
    # 启动子进程pr，读取:
    pr.start()
    # 等待pw结束:
    pw.join()
    # pr进程里是死循环，无法等待其结束，只能强行终止:
    pr.terminate()


def muti_thread():
    """
    lock = threading.Lock() 创建锁
    lock.acquire() 获取锁
    lock.release() 释放锁, 一般在 finally中
    多核任务只能用进程, 
    因为:Python的线程虽然是真正的线程，但解释器执行代码时，有一个GIL锁：Global Interpreter Lock，
    任何Python线程执行前，必须先获得GIL锁，然后，每执行100条字节码，解释器就自动释放GIL锁，让
    别的线程有机会执行。这个GIL全局锁实际上把所有线程的执行代码都给上了锁，所以，多线程在Python
    中只能交替执行，即使100个线程跑在100核CPU上，也只能用到1个核
    """

    import time, threading

    def loop():
        print('thread %s is running...' % threading.current_thread().name)
        n = 0
        while n < 5:
            n = n + 1
            print('thread %s >>> %s' % (threading.current_thread().name, n))
            time.sleep(1)
        print('thread %s ended.' % threading.current_thread().name)

    print('thread %s is running...' % threading.current_thread().name)
    # 主线程实例的名字叫MainThread，子线程的名字在创建时指定，我们用LoopThread命名子线程
    t = threading.Thread(target=loop, name='LoopThread')
    t.start()
    t.join()
    print('thread %s ended.' % threading.current_thread().name)



```


# 异步

```py

def async_io():
    """
    协程原理:
    在一个死循环中, 主线程不断轮询处理队列中的事件, 
    当遇到IO操作时，代码只负责发出IO请求，不等待IO结果，然后直接结束本轮消息处理，进入下一轮消息处理过程
    当io 完成, 会产生通知进入队列, 别线程处理
    最大的优势就是协程极高的执行效率。因为子程序切换不是线程切换
    第二大优势就是不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了
    因为协程是一个线程执行，那怎么利用多核CPU呢？最简单的方法是多进程+协程，既充分利用多核，又充分发挥协程的高效率
    """
    def consumer():
        r = ''
        while True:
            n = yield r # consumer通过yield拿到消息，处理，又通过yield把结果传回
            if not n:
                return
            print('[CONSUMER] Consuming %s...' % n)
            r = '200 OK'

    def produce(c):
        c.send(None) # 启动生成器
        n = 0
        while n < 5:
            n = n + 1
            print('[PRODUCER] Producing %s...' % n)
            # 一旦生产了东西，通过c.send(n)切换到consumer执行
            r = c.send(n)
            print('[PRODUCER] Consumer return: %s' % r)
        # 通过c.close()关闭consumer，整个过程结束
        c.close()

    c = consumer()
    produce(c)


    # 
    #
    # 异步操作需要在coroutine中通过yield from完成；
    # 
    # 多个coroutine可以封装成一组Task然后并发执行。
    #
    import asyncio # asyncio是Python 3.4版本引入的标准库，直接内置了对异步IO的支持。
    # https://github.com/dabeaz/curio 替代者

    @asyncio.coroutine # 把一个generator标记为coroutine类型
                        # 然后在coroutine内部用yield from调用另一个coroutine实现异步操作
    def hello():
        print("Hello world!")
        # 异步调用asyncio.sleep(1), 虽然睡眠1s, 但是主线程并未等待，而是去执行EventLoop中其他可以执行的coroutine了
        # asyncio.sleep()也是一个coroutine
        r = yield from asyncio.sleep(1)
        print("Hello again!")

    # 获取EventLoop:
    loop = asyncio.get_event_loop()
    # 执行coroutine
    loop.run_until_complete(hello())

    # 封装两个coroutine
    # 两个coroutine是由同一个线程并发执行的
    tasks = [hello(), hello()]
    loop.run_until_complete(asyncio.wait(tasks))

    loop.close()

    #async和await是针对coroutine的新语法
    #
    #把@asyncio.coroutine替换为async；
    #把yield from替换为await。
    #
    async def hello1():
        print("Hello world!")
        r = await asyncio.sleep(1)
        print("Hello again!")

    # asyncio可以实现单线程并发IO操作。如果仅用在客户端，发挥的威力不大。
    # 如果把asyncio用在服务器端，例如Web服务器，由于HTTP连接就是IO操作，因此可以用单线程+coroutine实现多用户的高并发支持
    # aiohttp则是基于asyncio实现的HTTP框架
    #
    # 编写一个HTTP服务器:
    #
    # import asyncio

    # from aiohttp import web

    # async def index(request):
    #     await asyncio.sleep(0.5)
    #     return web.Response(body=b'<h1>Index</h1>')

    # async def hello(request):
    #     await asyncio.sleep(0.5)
    #     text = '<h1>hello, %s!</h1>' % request.match_info['name']
    #     return web.Response(body=text.encode('utf-8'))

    # async def init(loop):
    #     app = web.Application(loop=loop)
    #     app.router.add_route('GET', '/', index)
    #     app.router.add_route('GET', '/hello/{name}', hello)
    #     srv = await loop.create_server(app.make_handler(), '127.0.0.1', 8000)
    #     print('Server started at http://127.0.0.1:8000...')
    #     return srv

    # loop = asyncio.get_event_loop()
    # loop.run_until_complete(init(loop))
    # loop.run_forever()

```


# 内建模块

## 日期处理

```py
def builtin_module():
    from datetime import datetime, timedelta

    now = datetime.now()
    print(now) # 2019-06-05 23:06:46.171270

    time = datetime(2019, 4, 19, 1, 30)
    print(time) # 2019-04-19 01:30:00

    timestamp = time.timestamp()
    print(timestamp)  # 1555608600.0
    print(datetime.fromtimestamp(timestamp))  # 2019-04-19 01:30:00

    cday = datetime.strptime('2015-6-1 18:19:59', '%Y-%m-%d %H:%M:%S')
    print(cday)  # 2015-06-01 18:19:59

    strftime = now.strftime('%Y-%m-%d %H:%M:%S')
    print(strftime)  # 2019-06-05 23:06:46

    result = now + timedelta(days=1, hours=1)
    print(result)  # 2019-06-07 00:11:36.912231

```

## 集合


```py
# 命名的 tuple 
from collections import namedtuple
    # namedtuple是一个函数，它用来创建一个自定义的tuple对象，并且规定了tuple元素的个数，并可以用属性而不是索引来引用tuple的某个元素
    Point = namedtuple('Point', ['x', 'y'])  # 相当于type(), 构造了一个类
    p = Point(1, 2)
    print(p.x) # 1
    print(p.y) # 2




from collections import  deque

    # deque是为了高效实现插入和删除操作的双向列表
    #  是为了弥补 list  插入和删除效率很低 的缺点
    q = deque(['a', 'b', 'c'])
    q.append('d')
    q.appendleft('e')
    print(q)  # deque(['e', 'a', 'b', 'c', 'd'])





from collections import defaultdict
    # 使用dict时，如果引用的Key不存在，就会抛出KeyError。如果希望key不存在时，返回一个默认值，就可以用defaultdict
    # 如果要保持Key的顺序，可以用OrderedDict
    # ChainMap可以把一组dict串起来并组成一个逻辑上的dict

    ################

    from  collections import  Counter

    # Counter是一个简单的计数器
    c = Counter()
    for ch in 'programming':
        c[ch] = c[ch] + 1

    print(c)  # Counter({'r': 2, 'g': 2, 'm': 2, 'p': 1, 'o': 1, 'a': 1, 'i': 1, 'n': 1})

```



# 编写命令行程序

https://www.jianshu.com/p/005ecf9cf8aa TODO

## 命令行自动补全

https://www.cnblogs.com/lgh344902118/p/8521437.html

 TODO



# 类型系统

https://www.zhihu.com/question/265003581/answer/461562594
https://zhuanlan.zhihu.com/p/56863684

https://github.com/python/mypy

https://github.com/google/pytype

https://docs.python.org/zh-cn/3/library/typing.html

微软于7月1日发布一款新的VS Code插件，名为 Pylance


```py
# 自python3.5开始，PEP484为python引入了类型注解(type hints)
    # typing模块
    #
    # https://www.cnblogs.com/linkenpark/p/11676297.html
    # int,long,float: 整型,长整形,浮点型
    # bool,str: 布尔型，字符串类型
    # List, Tuple, Dict, Set:列表，元组，字典, 集合
    # Iterable,Iterator:可迭代类型，迭代器类型
    # Generator：生成器类型
    #
    # ":" 后面是参数类型
    # "->" 是返回值的注释，-> str 意思即是提醒函数使用者返回值会是一个str型
    def f(ham: "传一个字符串", eggs: str = 'eggs') -> str :
        print("Annotations:", f.__annotations__)
        print("Arguments:", ham, eggs)
        return ham + ' and ' + eggs

    print(f("est", 123))
```


# 调试

## 断言

类似 print, 

程序中如果到处充斥着assert，和print()相比也好不到哪去。不过，启动Python解释器时可以用-O参数来关闭assert: `python -O err.py`, 关闭后，可以把所有的assert语句当成pass来看

```python
def foo(s):
    n = int(s)
    # 只能断言 true or false
    # 如果断言失败，assert语句本身就会抛出AssertionError
    assert n != 0, 'n is zero!'
    return 10 / n

def main():
    foo('0')

# 运行时，关闭assert
$ python3 -O err.py
Traceback (most recent call last):
  ...
ZeroDivisionError: division by zero
```

## 设置断点 pdb

启动Python的调试器pdb `python -m pdb err.py`, 输入命令l来查看代码, 输入命令n可以单步执行代码, 何时候都可以输入命令p 变量名来查看变量, 输入命令q结束调试，退出程序

pdb.set_trace() 设置断点

```python
# err.py
import pdb

s = '0'
n = int(s)
pdb.set_trace() # 运行到这里会自动暂停
print(10 / n)

$ python3 err.py
> /Users/michael/Github/learn-python3/samples/debug/err.py(7)<module>()
-> print(10 / n)# 此时进入调试模式
(Pdb) p n # p查看变量
0
(Pdb) c # c 继续运行
Traceback (most recent call last):
  File "err.py", line 7, in <module>
    print(10 / n)
ZeroDivisionError: division by zero

```



# jinja2 模板

渲染使用：

```py
# FileSystemLoader：文件系统加载器
# 不需要模板文件存在某个Python包下，可以直接访问系统中的文件

# PackageLoader：包加载器
from jinja2 import PackageLoader,Environment
# 两个参数为：python包的名称，以及模板目录名称
env = Environment(loader=PackageLoader('python_project','templates'))    # 创建一个包加载器对象
template = env.get_template('bast.html')    # 获取一个模板文件
template.render(name='daxin',age=18)   # 渲染


```

模板语法：

```py
# 三种语法
# 控制结构 {% %}
# 变量取值{{}}
# 注释 {# #}

# 循环
# 迭代列表
<ul>
{% for user in users %}
<li>{{ user.username|title }}</li>
{% endfor %}
</ul>
# 迭代字典
<dl>
{% for key, value in my_dict.iteritems() %}
<dt>{{ key }}</dt>
<dd>{{ value}}</dd>
{% endfor %}
</dl>

# 过滤器 就是内置函数
# safe: 渲染时值不转义
# capitialize: 把值的首字母转换成大写，其他子母转换为小写
# lower: 把值转换成小写形式
# upper: 把值转换成大写形式
# title: 把值中每个单词的首字母都转换成大写
# trim: 把值的首尾空格去掉
# striptags: 渲染之前把值中所有的HTML标签都删掉
# join: 拼接多个值为字符串
# replace: 替换字符串的值
# round: 默认对数字进行四舍五入，也可以用参数进行控制
# int: 把值转换成整型

{{ 'abc' | captialize  }}
# Abc

{{ 'abc' | upper  }}
# ABC

{{ 'hello world' | title  }}
# Hello World

{{ "hello world" | replace('world','daxin') | upper }}
# HELLO DAXIN

{{ 18.18 | round | int }}
# 18


# 宏类似于Python中的函数，我们在宏中定义行为，还可以进行传递参数，就像Python中的函数
# 定义
{% macro input(name,age=18) %}   # 参数age的默认值为18
 <input type='text' name="{{ name }}" value="{{ age }}" >
{% endmacro %}
# 调用
<p>{{ input('daxin') }} </p>
<p>{{ input('daxin',age=20) }} </p>

# 继承和super函数
# 模板继承。模板继承允许我们创建一个基本(骨架)文件，其他文件从该骨架文件继承，然后针对自己需要的地方进行修改

# 骨架
# 定义了四处 block，即：head，title，content，footer
<!DOCTYPE html>
<html lang="en">
<head>
    {% block head %}
    <link rel="stylesheet" href="style.css"/>
    <title>{% block title %}{% endblock %} - My Webpage</title>
    {% endblock %}
</head>
<body>
<div id="content">{% block content %}{% endblock %}</div>
<div id="footer">
    {% block  footer %}
    <script>This is javascript code </script>
    {% endblock %}
</div>
</body>
</html>

# 新建文件继承
{% extend "base.html" %}       # 继承base.html文件

{% block title %} Dachenzi {% endblock %}   # 定制title部分的内容

{% block head %}
    {{  super()  }}        # 用于获取原有的信息
    <style type='text/css'>
    .important { color: #FFFFFF }
    </style>
{% endblock %}

# 其他不修改的原封不动的继承

```

# 路径问题

## 文件路径 and 终端路径

需要用命令行操作的时候，也就有参数 argv的时候，我们要求终端路径和Python文件路径一致。命令行运行程序的格式是：python+Python文件名+参数

## 获取脚本位置

```py
if __name__ == '__main__':
    import sys
    import os

    # 不适用于 命令行程序,
    print(sys.argv)    # ['D:/repo/repo_github/shared_already/rest-api-generator/test_jinja.py']
    # 获得的是当前执行脚本的位置
    # 若在命令行, 打印的是 'py xxx'命令中的 xxx
    print(sys.argv[0])    # D:/repo/repo_github/shared_already/rest-api-generator/test_jinja.py

    # python 查找库的路径, 第一个是当前项目路径
    print(sys.path)
    print(sys.path[0])  # D:\repo\repo_github\shared_already\rest-api-generator

    # 判断为脚本文件还是py2exe编译后的文件，如果是脚本文件，
    # 则返回的是脚本的目录，如果是py2exe编译后的文件，则返回的是编译后的文件路径
    cur_path = sys.path[0]
    if os.path.isdir(cur_path):
        print('current project dir: [{}]'.format(cur_path))
    elif os.path.isfile(cur_path):
        dirname = os.path.dirname(cur_path)
        print('current project dir: [{}]'.format(dirname))

    # 当前工作目录
    # 若要改变当前工作路径，可以用：os.chdir(path)
    print(os.getcwd())
    print(os.path.abspath(os.curdir))
    print(os.curdir)  # 相对路径
    print(os.path.abspath('.'))
    # 父目录
    print(os.path.abspath('..'))


```

# shutil 使用

TODO

```py
# 递归删除目录

import shutil  
shutil.rmtree('要清空的文件夹名')  # 先删除
os.mkdir('要清空的文件夹名')  # 再重建

import os
for root, dirs, files in os.walk(top, topdown=False):
    for name in files:
        os.remove(os.path.join(root, name))
    for name in dirs:
        os.rmdir(os.path.join(root, name))

# 移动文件夹
shutil.move('原文件夹/原文件名','目标文件夹/目标文件名') 

```

# venv 虚拟环境

使用 pycharm: https://blog.csdn.net/pfm685757/article/details/108590680

inherit global site-packages:勾选上的话代表创建的新项目需要copy一份全局包到虚拟环境。

Make available to all projects:勾选上，表示，当在虚拟环境下安装包的时候，copy一份到全局。


```sh
 pip3 install virtualenv
# 虚拟环境
# --no-site-packages，已经安装到系统Python环境中的所有第三方包都不会复制过来，这样，我们就得到了一个不带任何第三方包的“干净”的Python运行环境。
#               更新 , 版本 >=20 后, 默认就加上了这个选项, 不必手动指定
# -p 指定解释器
virtualenv -p python3.6 venv  
# 使用默认解释器
virtualenv  venv
# 有了venv这个Python环境，可以用source进入该环境
source ./venv/bin/activate
# 安装库依赖 到 venv
pip install -r requirements.txt
# 生成依赖文件
pip freeze > requirements.txt 
# 当前安装版本
pip freeze
# 退出 venv
deactivate


# virtualenvwrapper,是对 virtualenv 的一个封装，目的是使后者更好用。


# venv , Python 从3.3 版本开始，自带了一个虚拟环境 venv
# 仅仅支持 python 3.3 和以后版本。所以，要在 python2 上使用虚拟环境，依然要利用 virtualenv 
python3 -m venv <venv_name> # 一般命名为 .venv

.venv\Scripts\activate.bat
# or
source .venv/bin/activate



```

使用 pycharm 的 venv

设置 interpretor, 推荐 venv 虚拟环境

"inherit global site packages" 勾选表示 project 内找不到需要的库, 就扩大范围, 到全局 base interpretor 的库去找, 但是安装的库会使用 全局 pip 安装, 会安装到 base interpretor 中, 污染全局. 建议不要勾选

此时会在 project root 下生成 venv



# web 开发

https://www.zhihu.com/question/20706333/answer/24927602 有哪些框架
https://www.zhihu.com/question/41564604/answer/660256963 对比

- Flask: 小而美, 适合小站点, 开发web服务的API, 和 nosql配合良好

- Django: 大而全, 自带 template, orm, 适合开发企业级网站, 适合 sql, 无法配合nosql数据库

- Torando: 原生异步非阻塞，在IO密集型应用和多任务处理上占据绝对性的优势，属于专注型框架

    websockets 长连接;

    内嵌了HTTP服务器


## Django

```sh
# 升级 pip 到 v21
python -m pip install --upgrade pip

pip install Django

# 创建模板代码
# 末尾 . 表示当前目录, 可选
# django_demo 为项目名, 一般和当前文件夹名保持一致
django-admin startproject django_demo .

# 启动开发服务器
python manage.py runserver [[0:]8080]

# 创建 demo app: polls 的模板代码
python manage.py startapp polls

# 检查 INSTALLED_APPS 设置，为其中的每个应用创建需要的数据表/更新表结构
python manage.py migrate

# 为指定 app 创建 迁移文件(模型数据, 用于生成 SQL), polls 是自定义 app name
python manage.py makemigrations polls

#  查看 SQL
python manage.py sqlmigrate polls 0001


# 通过模型 api 操作数据库
python manage.py shell
```


## flask



```sh
export FLASK_APP=hello.py

# optional, enable debug mode
export FLASK_ENV=development

# default port -> 5000
flask run [--host=0.0.0.0]


```


# db 驱动 问题

mac 无法安装 postgres 驱动

驱动在这: https://pypi.org/project/psycopg2/, 需要首先安装 postgres  `brew install postgres`, 设置 path, 然后安装 ([参考](https://www.psycopg.org/docs/install.html#install-from-source))

如果还是不行 `pip install psycopg2-binary==2.8.3` ([参考](https://stackoverflow.com/questions/34304833/failed-building-wheel-for-psycopg2-macosx-using-virtualenv-and-pip))


# 代替 shell 运维

https://www.oschina.net/translate/python-scripts-replacement-bash-utility-scripts
https://blog.51cto.com/dashui/2334754
https://www.cnblogs.com/jacktian-it/articles/10222930.html

## 传递参数

通过 sys.stdin, sys.stdout

参数处理库: optparse.OptionParser

```py
if __name__ == "__main__":
    # 初始化一个names的字典，内容为空
    # 字典中为name和出现数量的键值对
    names = {}
    # sys.stdin是一个文件对象。 所有引用于file对象的方法，
    # 都可以应用于sys.stdin.
    for name in sys.stdin.readlines():
            # 每一行都有一个newline字符做结尾
            # 我们需要删除它
            name = name.strip()
            if name in names:
                    names[name] += 1
            else:
                    names[name] = 1

    # 迭代字典,
    # 输出名字，空格，接着是该名字出现的数量
    for name, count in names.iteritems():
            sys.stdout.write("%d\t%s\n" % (count, name))

```

`cat names.log | python namescount.py | sort -rn`

## shell 调用 Python 


### 统计出现次数

```py
#!/usr/bin/env python
import sys

if __name__ == "__main__":
    # 初始化一个names的字典，内容为空
    # 字典中为name和出现数量的键值对
    names = {}
    for name in sys.stdin.readlines():
            # 每一行都有一个newline字符做结尾
            # 我们需要删除它
            name = name.strip()
            if name in names:
                    names[name] += 1
            else:
                    names[name] = 1

    # 迭代字典,
    # 输出名字，空格，接着是该名字出现的数量
    for name, count in names.iteritems():
            sys.stdout.write("%d\t%s\n" % (count, name))
```

`cat names.log | ./xxx.py`


### 读取 csv 文件

获取指定列

`cat xxx.csv | ./xxx.py 2`

```py
#!/usr/bin/env python
# CSV module that comes with the Python standard library
import csv
import sys


if __name__ == "__main__":
    # CSV模块使用一个reader对象作为输入
    # 在这个例子中，就是 sys.stdin.
    csvfile = csv.reader(sys.stdin)

    # 这个脚本必须接收一个参数，指定列的序号
    # 使用sys.argv获取参数.
    column_number = 0
    if len(sys.argv) > 1:
            column_number = int(sys.argv[1])

    # CSV文件的每一行都是用逗号作为字段的分隔符
    for row in csvfile:
            print row[column_number]

```


## Python 调用 shell


### subprocess 

python 3.0 之后使用 subprocess代替 commands

python 3.x 使用 subprocess 创建一个新进程, 允许创建很多子进程，创建的时候能指定子进程和子进程的输入、输出、错误输出管道，执行后能获取输出结果和执行状态。

```py
#!/usr/bin/python3


subprocess.run()	Python 3.5中新增的函数。执行指定的命令，等待命令执行完成后返回一个包含执行结果的CompletedProcess类的实例。
subprocess.call()	执行指定的命令，返回命令执行状态，其功能类似于os.system(cmd)。
subprocess.check_call()	Python 2.5中新增的函数。 执行指定的命令，如果执行成功则返回状态码，否则抛出异常。其功能等价于subprocess.run(..., check=True)。
subprocess.check_output()	Python 2.7中新增的的函数。执行指定的命令，如果执行状态码为0则返回命令执行结果，否则抛出异常。
subprocess.getoutput(cmd)	接收字符串格式的命令，执行命令并返回执行结果，其功能类似于os.popen(cmd).read()和commands.getoutput(cmd)。
subprocess.getstatusoutput(cmd)	执行cmd命令，返回一个元组(命令执行状态, 命令执行结果输出)，其功能类似于commands.getstatusoutput()。




import subprocess as sp


# subprocess.run()：python3.5中新增的函数， 执行指定的命令， 等待命令执行完成后返回一个包含执行结果的CompletedProcess类的实例
# Popen 的简化版本
#执行ls -l /dev/null 命令
>>> subprocess.run(["ls", "-l", "/dev/null"])
# crw-rw-rw-  1 root  wheel    3,   2  5  4 13:34 /dev/null
# CompletedProcess(args=['ls', '-l', '/dev/null'], returncode=0)


# 执行 shell 命令, 打印输出到 stdout, 类似 os.system
# shell=True意思是shell下执行command, 如果command不是一个可执行文件，shell=True是不可省略的。
code = sp.call("ls -al", shell=True)
# <=> sp.call(["ls", "-al"])
print(code) # 0

# 异步执行, 打印到 stdout
# 0，1，2。其中0代表正确执行，1和2都是错误执行，2通常是没有读取到文件
result = sp.Popen("ls -al", shell=True) # 异步执行
print(result.returncode) # None 这步不会阻塞
print(result.wait()) # 0 阻塞

# 获取输出
# 设置输出到管道
proc = sp.Popen("ls -al", stdout=sp.PIPE, shell=True)
print(proc.stdout.readline())

proc1 = sp.Popen("ls -al", stdout=sp.PIPE, shell=True)
(stdout, stderr) = proc1.communicate("..")
print(stdout)


# ##############




```

### os.system

```py
#!/usr/bin/python3

import os

# output to stdout
# 0表示shell指令成功执行，256表示未找到，该方法适用于shell命令不需要输出内容的场景。
status = os.system("ls -al")
print(status) # 0 

# os.system() 这个方法是阻塞当前主进程执行的，只有该命令执行完毕，主进程才会继续执行
os.system('ping -c 2 www.baidu.com &>/dev/null')
print("block until ping finished")

ret = os.system('cat hello.sh | grep "ooo"')
print(ret) # 256, 没有找到时，sh 返回的状态码是1，而python调用，返回的是：256
ret1 = os.system('cat hello.sh | grep "c 1"')
print(ret1) # 0

# 异步执行命令, 
# 拿到命令执行输出
#  os.popen() 返回的是 file read 的对象，对其进行读取 read() 的操作可以看到执行的输出。这个方法是后台执行，不影响后续脚本运行
output = os.popen("ping -c1 www.baidu.com &>/dev/null")
print("popen -- nonblock")
# readlines() 也可
print(output.read())
print("popen -- block until output.read() end")




# 执行 shell 脚本
os.system("./hello.sh")

# 改变当前工作目录到指定的路径
# <=> cd ..
access = os.chdir("..") # 如果允许访问返回 True , 否则返回False

# 查看当前工作目录
retval = os.getcwd()

# 权限
# 不如直接 os.system("chmod ...")
# import stat
# os.chmod("./hello.sh", stat.S_IRWXU)

# 更改所属用户, 需要 root 权限执行
# os.chown()

# 更改当前进程的根目录为指定的目录，使用该函数需要管理员权限
# os.chroot()

```

### system

```py
def cd(dir): system.command('cd {}'.format(dir))

```


### commands

仅仅适用于 Linux

python 3.0 之后移除此命令，使用 subprocess 代替

```py



import commands
a,b = commands.getstatusoutput('ls')
a是退出状态
b是输出的结果
commands.getoutput(cmd)只返回输出结果

```

### paramiko

https://github.com/paramiko/paramiko
https://www.cnblogs.com/xiao-apple36/p/9144092.html

https://www.yisu.com/zixun/12641.html
https://www.cnblogs.com/liangyy09/p/13921693.html

pip3 install paramiko


```py
# paramiko包含两个核心组件：SSHClient和SFTPClient。

常用参数
hostname 连接的目标主机
port=SSH_PORT 指定端口
username=None 验证的用户名
password=None 验证的用户密码
pkey=None 私钥方式用于身份验证
key_filename=None 一个文件名或文件列表，指定私钥文件
timeout=None 可选的tcp连接超时时间
allow_agent=True, 是否允许连接到ssh代理，默认为True 允许
look_for_keys=True 是否在~/.ssh中搜索私钥文件，默认为True 允许
compress=False, 是否打开压缩


set_missing_host_key_policy()：设置远程服务器没有在know_hosts文件中记录时 (连接的远程主机没有本地主机密钥或HostKeys对象) 的应对策略。目前支持三种策略：

    AutoAddPolicy 自动添加主机名及主机密钥到本地HostKeys对象，不依赖load_system_host_key的配置。即新建立ssh连接时不需要再输入yes或no进行确认
    WarningPolicy 用于记录一个未知的主机密钥的python警告。并接受，功能上和AutoAddPolicy类似，但是会提示是新连接
    RejectPolicy 自动拒绝未知的主机名和密钥，依赖load_system_host_key的配置。此为默认选项

exec_command()：在远程服务器执行Linux命令的方法。

# 
# 
# 
# 
# 


# 利用SSHClient对象的open_sftp()方法，可以直接返回一个基于当前连接的sftp对象，可以进行文件的上传等操作
open_sftp()：在当前ssh会话的基础上创建一个sftp会话。该方法会返回一个SFTPClient对象。
sftp = client.open_sftp()
sftp.put('test.txt','text.txt')

from_transport(cls,t) 创建一个已连通的SFTP客户端通道
put(localpath, remotepath, callback=None, confirm=True) 将本地文件上传到服务器 参数confirm：是否调用stat()方法检查文件状态，返回ls -l的结果
get(remotepath, localpath, callback=None) 从服务器下载文件到本地
mkdir() 在服务器上创建目录
remove() 在服务器上删除目录
rename() 在服务器上重命名目录
stat() 查看服务器文件状态
listdir() 列出服务器目录下的文件

# 
# 
# 
# 

import paramiko
 
   # 实例化SSHClient
   client = paramiko.SSHClient()
 
   # 自动添加策略，保存服务器的主机名和密钥信息，如果不添加，那么不再本地know_hosts文件中记录的主机将无法连接
   client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
 
   # 连接SSH服务端，以用户名和密码进行认证
   client.connect(hostname='192.168.1.105', port=22, username='root', password='123456')
 
   # 打开一个Channel并执行命令
   stdin, stdout, stderr = client.exec_command('df -h ')  # stdout 为正确输出，stderr为错误输出，同时是有1个变量有值
 
   # 打印执行结果
   print(stdout.read().decode('utf-8'))
 
   # 关闭SSHClient
   client.close()


# ###########################


# 配置私人密钥文件位置
private = paramiko.RSAKey.from_private_key_file('/Users/ch/.ssh/id_rsa')
 
#实例化SSHClient
client = paramiko.SSHClient()
 
#自动添加策略，保存服务器的主机名和密钥信息，如果不添加，那么不再本地know_hosts文件中记录的主机将无法连接
client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
 
#连接SSH服务端，以用户名和密码进行认证
client.connect(hostname='10.0.0.1',port=22,username='root',pkey=private)


#############################

# SSHClient 封装 Transport
# 创建一个通道
   transport = paramiko.Transport(('hostname', 22))
   transport.connect(username='root', password='123')
 
   ssh = paramiko.SSHClient()
   ssh._transport = transport
 
   stdin, stdout, stderr = ssh.exec_command('df -h')
   print(stdout.read().decode('utf-8'))
 
   transport.close()

################################################################

# 获取Transport实例
tran = paramiko.Transport(('10.0.0.3', 22))
 
# 连接SSH服务端，使用password
tran.connect(username="root", password='123456')
# 或使用
# 配置私人密钥文件位置
private = paramiko.RSAKey.from_private_key_file('/Users/root/.ssh/id_rsa')
# 连接SSH服务端，使用pkey指定私钥
tran.connect(username="root", pkey=private)
 
# 获取SFTP实例
sftp = paramiko.SFTPClient.from_transport(tran)
 
# 设置上传的本地/远程文件路径
localpath = "/Users/root/Downloads/1.txt"
remotepath = "/tmp/1.txt"
 
# 执行上传动作
sftp.put(localpath, remotepath)
# 执行下载动作
sftp.get(remotepath, localpath)
 
tran.close()
```

## 部署 springboot

```py
#!/usr/bin/python
# -*- coding:utf-8 -*-
import os,sys,time,commands
from sys import argv
 
times = time.strftime('%Y年%m月%d日%H:%M:%S')
_jdkpath_ = '填写你的JDK路径'
_processpath_ = 'jar包名字'
_fileDir_ = 'jar包所在路径'
_jarpath_ = _fileDir_ + _processpath_
_logpath_ = _fileDir_ + '日志路径'
_gclog_   = _fileDir_ + 'gc日志路径'
_jarvar_ = '-Xms256m -Xmx640m -Xss256K -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=256m -XX:+UseAdaptiveSizePolicy'
 
def getpid():
    pid = commands.getoutput("ps aux | grep %s | grep -v grep | awk '{print $2}'" % _processpath_)
    return pid
 
def _status_():
    j_pid = getpid()
    if j_pid == '':
        print("%s没有运行" % _processpath_)
    else :
        print("%s进程pid值:" % _processpath_ + getpid())
 
def _start_():
    j_pid = getpid()
    if j_pid != '':
        print("%s已经在运行..." % _processpath_)
    else :
        print("启动%s......" % _processpath_)
        time.sleep(4)
        if os.system('%s -jar -XX:+PrintGCDetails -Xloggc:%s %s %s > %s 2>&1 &' % (_jdkpath_,_gclog_,_jarvar_,_jarpath_,_logpath_)) == 0:
            print("启动%s成功,进程pid值:" % _processpath_ + getpid())
 
def _stop_():
    s_pid = getpid()
    if s_pid == '':
        print("%s没有运行" % _processpath_)
    else :
        print("关闭%s......" % _processpath_)
        os.system('kill -9 ' + s_pid)
	os.system('echo "">%s && echo "">%s' % (_gclog_,_logpath_))
        time.sleep(2)
        if s_pid != '0':
            print("关闭%s成功" % _processpath_)
        else :
            print("关闭%s失败" % _processpath_)
 
def _restart_():
    _stop_()
    time.sleep(2)
    _start_()
 
if argv == "-h" or argv == "--help" or len(argv)==1:
    print "☻☻☻请别害怕,这只是个简单的脚本,对应用和系统都是无损的☻☻☻\n%s,温馨提示用法:%s {start|stop|restart|status}" % (times,argv[0])
 
elif argv[1] == 'start':
    _start_()
elif argv[1] == 'stop':
    _stop_()
elif argv[1] == 'restart':
    _restart_()
elif argv[1] == 'status':
    _status_()
else: 
    print "☻☻☻请别害怕,这只是个简单的脚本,对应用和系统都是无损的☻☻☻\n%s,温馨提示用法:%s {start|stop|restart|status}" % (times,argv[0])
```



# 开源库

https://www.zhihu.com/question/24590883
TODO

https://pypi.org/

## 图像

- Matplotlib 可视化库，可以用来绘制高质量的 2D 折线图、散点图、柱状图，或者用来显示图像
- pil 图像处理库, 适用于 Python2, py3 则安装 pillow
- opencv-python `import cv2`
- Removebg 抠图

## 数据模型定义

https://github.com/samuelcolvin/pydantic/ 是的实例化数据 model 对象更方便

https://blog.csdn.net/codename_cys/article/details/107675748


## 序列化反序列化

pickle


# jython

相互调用

https://www.py4j.org/getting_started.html run java lib in py


run py script in java

```xml
<dependency>
            <groupId>org.python</groupId>
            <artifactId>jython</artifactId>
            <version>2.7.2</version>
        </dependency>

```

```java
public class App {

    /**
     * 初始化
     */
    static void initInterpreter() {
        Properties props = new Properties();
        props.put("python.home", pyPath() + "py_lib");
        props.put("python.console.encoding", "UTF-8");
        props.put("python.security.respectJavaAccessibility", "false");
        props.put("python.import.site", "false");

        Properties preprops = System.getProperties();
        PythonInterpreter.initialize(preprops, props, new String[0]);

    }

    static String pyPath() {
        return Thread.currentThread().getContextClassLoader().getResource(".").getPath();
    }



    public static void main(String[] args) throws FileNotFoundException {
        initInterpreter();
        final PythonInterpreter interpreter = new PythonInterpreter();

        interpreter.execfile(new FileInputStream(pyPath() + "test.py"));
        final PyFunction addFunc = interpreter.get("add", PyFunction.class);
        int a = 1;
        int b = 2;
        final PyObject result = addFunc.__call__(new PyInteger(a), new PyInteger(b));
        System.out.println(result);


//        interpreter.execfile(new FileInputStream(pyPath() + "face_recog.py"));
//        final PyFunction face_verification = interpreter.get("face_verification", PyFunction.class);
//        final PyObject pyObject = face_verification.__call__();
//        System.out.println(pyObject);
    }
}

```


# 调用 rust 

https://www.v2ex.com/t/757216

https://blog.csdn.net/m0_37696990/article/details/96876692

https://blog.csdn.net/muzico425/article/details/103331676


# python 应用部署

https://www.zhihu.com/question/21888077

## 生成依赖清单

```sh
# 生成依赖文件
# https://blog.csdn.net/guolindonggld/article/details/87786032
pip3 freeze > requirements.txt

pip install -r requirements.txt

```

## gunicorn

gunicorn是一个wsgi http server

不过在production环境，起停和状态的监控最好用supervisior之类的监控工具，然后在gunicorn的前端放置一个http proxy server, 譬如nginx

https://www.jianshu.com/p/69e75fc3e08e

flask_app.py

```py
from flask import Flask

app = Flask(__name__)


@app.route('/demo', methods=['GET'])
def demo():
    return "gunicorn and flask demo."
```

gunicorn.conf.py 

```py
# coding:utf-8
import multiprocessing

bind = "0.0.0.0:5000"   #绑定的ip与端口
workers = multiprocessing.cpu_count() * 2 + 1    #进程数
errorlog = './log/gunicorn.error.log' #发生错误时log的路径
accesslog = './log/gunicorn.access.log' #正常时的log路径
backlog = 512                #监听队列
proc_name = 'gunicorn_pre_project'   #进程名
timeout = 30      # 设置超时时间120s，默认为30s。按自己的需求进行设置timeout = 120
worker_class = 'gevent' #使用gevent模式，还可以使用sync 模式，默认的是sync模式

threads = 3  #指定每个进程开启的线程数
loglevel = 'info' #日志级别，这个日志级别指的是错误日志的级别，而访问日志的级别无法设置

#设置gunicorn访问日志格式，错误日志无法设置
access_log_format = '%(t)s %(p)s %(h)s "%(r)s" %(s)s %(L)s %(b)s %(f)s" "%(a)s"'

```


```sh
# 启动命令
# 
# flask_app 来自flask_app.py
# app 来自 app = Flask(__name__)
# -c 指定 配置文件
gunicorn flask_app:app -c gunicorn.conf.py 

# 不使用配置文件
gunicorn -w 4 -b 0.0.0.0:5000 -D  flask_app:app


# 找到 父进程的 pid (masterpid)
pstree -ap | grep gunicorn 

# 或者 找到所有进程 pid, 然后 kill -9 所有相关进程
pgrep gunicorn

# 停止
kill -9 <父进程pid>
# 重启
kill -HUP <parent pid>
```




