---
title: shell-bash-note
tags: [shell, bash]
---


https://github.com/wangdoc/bash-tutorial
https://github.com/jaywcjlove/shell-tutorial

<!-- more -->


<!-- TOC -->

- [shell script](#shell-script)
    - [片段收集](#片段收集)
        - [判断文件是否存在](#判断文件是否存在)
        - [调用 Python](#调用-python)
        - [判断是否安装](#判断是否安装)
        - [当前路径](#当前路径)
        - [判断服务是否存活](#判断服务是否存活)
        - [判断当前系统](#判断当前系统)
        - [mvnw wrapper](#mvnw-wrapper)
    - [执行方式](#执行方式)
    - [各种括号 简写](#各种括号-简写)
    - [特殊的变量名](#特殊的变量名)
        - [$?](#)
        - [$0 $123 $* $@ $](#0-123---)
        - [$$ $!](#-)
    - [空格语法的坑](#空格语法的坑)
    - [各种执行方式和差异](#各种执行方式和差异)
    - [语法](#语法)
        - [数据类型](#数据类型)
            - [字符串 打印](#字符串-打印)
            - [数组](#数组)
        - [读取标准输入](#读取标准输入)
        - [数值运算](#数值运算)
        - [条件判断](#条件判断)
        - [条件判断-case 语法](#条件判断-case-语法)
        - [条件判断-多路分支语法](#条件判断-多路分支语法)
        - [默认值](#默认值)
        - [函数](#函数)
    - [debug](#debug)
    - [学习几个demo](#学习几个demo)
- [正则&文本格式化](#正则文本格式化)
    - [正则表达式和语言系统有关系](#正则表达式和语言系统有关系)
    - [正则中的特殊符号](#正则中的特殊符号)
    - [grep进阶用法](#grep进阶用法)
    - [延伸正则表示法](#延伸正则表示法)
    - [sed](#sed)
    - [awk行内格式化](#awk行内格式化)
    - [printf格式化打印](#printf格式化打印)
    - [文件对比工具](#文件对比工具)
- [bash](#bash)
    - [快捷键](#快捷键)
    - [几种 bash 环境 startup 配置文件对比](#几种-bash-环境-startup-配置文件对比)
    - [shell and python interaction](#shell-and-python-interaction)
    - [示例](#示例)
    - [所有可用的shell](#所有可用的shell)
    - [环境变量](#环境变量)
    - [键盘读取read](#键盘读取read)
    - [declare,typeset声明变量类型](#declaretypeset声明变量类型)
    - [变量的删除取代](#变量的删除取代)
    - [变量的测试和内容替换](#变量的测试和内容替换)
    - [命令别名和设定](#命令别名和设定)
    - [命令历史](#命令历史)
    - [指令搜寻顺序](#指令搜寻顺序)
    - [bash登录欢迎信息](#bash登录欢迎信息)
    - [bash的环境配置文件](#bash的环境配置文件)
    - [终端机的环境设定stty,set](#终端机的环境设定sttyset)
    - [通配符和特殊符号](#通配符和特殊符号)
    - [数据流重导向](#数据流重导向)
    - [&&和||](#和)
    - [管线命令](#管线命令)
    - [截取命令](#截取命令)
    - [排序sort,wc,uniq](#排序sortwcuniq)
    - [字符转换命令](#字符转换命令)
    - [字符串](#字符串)

<!-- /TOC -->


# shell script

https://zhuanlan.zhihu.com/p/264346586

## 片段收集

### 判断文件是否存在

```sh
#shell 判断文件夹存在
if [ -d "${your_dir_name}" ]; then
fi
# 判断文件夹不存在
if [ ! -d "${your_dir_name}" ]; then
fi

#shell 判断文件存在
if [ -f "${your_file_path}" ]; then
    echo "${your_file_path} exists."
fi
# shell 判断文件不存在
if [ ! -f "${your_file_path}" ]; then
    echo "${your_file_path} not exists."
fi
```

### 调用 Python

```sh
# >>> shell call Python

#有时候某些操作用shell实现比较麻烦，这时可以考虑用python实现。
#再利用shell调用python的实现
#以下代码保存在 utils.py
from datetime import datetime, timedelta
def get_yesterday():
    yesterday_date = datetime.now() - timedelta(days=1)
    yesterday_date_str = yesterday_date.strftime("%Y-%m-%d")
    return yesterday_date_str
#以下代码保存在 run.sh中
#shell调用python函数
yesterday_date=`python -c "import utils; print(utils.get_yesterday())"`
echo ${yesterday_date}

```


### 判断是否安装

```sh
if [ -x "$(command -v git)" ]
then
    echo "SUCCESS: Git is installed"
else
    echo "ERROR: Git does not seem to be installed."
    echo "Please download Git using your package manager or over https://git-scm.com/!"
    exit 1
fi

```

###  当前路径

```sh

# 在命令行状态下单纯执行 $ cd `dirname $0` 是毫无意义的。因为他返回当前路径的"."
# 这个命令写在脚本文件里才有作用，他返回这个脚本文件放置的目录，并可以根据这个目录来定位所要运行程序的相对位置
echo $(cd `dirname $0`;pwd) # 直接使用 pwd 会报错: 无法找到命令

```

### 判断服务是否存活

```sh

#!/bin/bash

# detect runing service

echo -e "I will detect runing services."
testing=$(netstat -tuln | grep ":80")
if [ "${testing}" != "" ]; then
        echo "WWW is runing."
fi

testing=$(netstat -tuln | grep ":22")
if [ "${testing}" != "" ]; then
        echo "SSH is runing."
fi

testing=$(netstat -tuln | grep ":21")
if [ "${testing}" != "" ]; then
        echo "FTP is runing."
fi

testing=$(netstat -tuln | grep ":25")
if [ "${testing}" != "" ]; then
        echo "MAIL is runing"
fi
```

### 判断当前系统

```sh

# OS specific support.  $var _must_ be set to either true or false.
cygwin=false;
darwin=false;
mingw=false
case "`uname`" in
  CYGWIN*) cygwin=true ;;
  MINGW*) mingw=true;;
  Darwin*) darwin=true
    # Use /usr/libexec/java_home if available, otherwise fall back to /Library/Java/Home
    # See https://developer.apple.com/library/mac/qa/qa1170/_index.html
    if [ -z "$JAVA_HOME" ]; then
      if [ -x "/usr/libexec/java_home" ]; then
        export JAVA_HOME="`/usr/libexec/java_home`"
      else
        export JAVA_HOME="/Library/Java/Home"
      fi
    fi
    ;;
esac

```

### mvnw wrapper

```sh
#!/bin/sh
# ----------------------------------------------------------------------------
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#    https://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.
# ----------------------------------------------------------------------------

# ----------------------------------------------------------------------------
# Maven Start Up Batch script
#
# Required ENV vars:
# ------------------
#   JAVA_HOME - location of a JDK home dir
#
# Optional ENV vars
# -----------------
#   M2_HOME - location of maven2's installed home dir
#   MAVEN_OPTS - parameters passed to the Java VM when running Maven
#     e.g. to debug Maven itself, use
#       set MAVEN_OPTS=-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=8000
#   MAVEN_SKIP_RC - flag to disable loading of mavenrc files
# ----------------------------------------------------------------------------

if [ -z "$MAVEN_SKIP_RC" ] ; then

  if [ -f /etc/mavenrc ] ; then
    . /etc/mavenrc
  fi

  if [ -f "$HOME/.mavenrc" ] ; then
    . "$HOME/.mavenrc"
  fi

fi

# OS specific support.  $var _must_ be set to either true or false.
cygwin=false;
darwin=false;
mingw=false
case "`uname`" in
  CYGWIN*) cygwin=true ;;
  MINGW*) mingw=true;;
  Darwin*) darwin=true
    # Use /usr/libexec/java_home if available, otherwise fall back to /Library/Java/Home
    # See https://developer.apple.com/library/mac/qa/qa1170/_index.html
    if [ -z "$JAVA_HOME" ]; then
      if [ -x "/usr/libexec/java_home" ]; then
        export JAVA_HOME="`/usr/libexec/java_home`"
      else
        export JAVA_HOME="/Library/Java/Home"
      fi
    fi
    ;;
esac

if [ -z "$JAVA_HOME" ] ; then
  if [ -r /etc/gentoo-release ] ; then
    JAVA_HOME=`java-config --jre-home`
  fi
fi

if [ -z "$M2_HOME" ] ; then
  ## resolve links - $0 may be a link to maven's home
  PRG="$0"

  # need this for relative symlinks
  while [ -h "$PRG" ] ; do
    ls=`ls -ld "$PRG"`
    link=`expr "$ls" : '.*-> \(.*\)$'`
    if expr "$link" : '/.*' > /dev/null; then
      PRG="$link"
    else
      PRG="`dirname "$PRG"`/$link"
    fi
  done

  saveddir=`pwd`

  M2_HOME=`dirname "$PRG"`/..

  # make it fully qualified
  M2_HOME=`cd "$M2_HOME" && pwd`

  cd "$saveddir"
  # echo Using m2 at $M2_HOME
fi

# For Cygwin, ensure paths are in UNIX format before anything is touched
if $cygwin ; then
  [ -n "$M2_HOME" ] &&
    M2_HOME=`cygpath --unix "$M2_HOME"`
  [ -n "$JAVA_HOME" ] &&
    JAVA_HOME=`cygpath --unix "$JAVA_HOME"`
  [ -n "$CLASSPATH" ] &&
    CLASSPATH=`cygpath --path --unix "$CLASSPATH"`
fi

# For Mingw, ensure paths are in UNIX format before anything is touched
if $mingw ; then
  [ -n "$M2_HOME" ] &&
    M2_HOME="`(cd "$M2_HOME"; pwd)`"
  [ -n "$JAVA_HOME" ] &&
    JAVA_HOME="`(cd "$JAVA_HOME"; pwd)`"
fi

if [ -z "$JAVA_HOME" ]; then
  javaExecutable="`which javac`"
  if [ -n "$javaExecutable" ] && ! [ "`expr \"$javaExecutable\" : '\([^ ]*\)'`" = "no" ]; then
    # readlink(1) is not available as standard on Solaris 10.
    readLink=`which readlink`
    if [ ! `expr "$readLink" : '\([^ ]*\)'` = "no" ]; then
      if $darwin ; then
        javaHome="`dirname \"$javaExecutable\"`"
        javaExecutable="`cd \"$javaHome\" && pwd -P`/javac"
      else
        javaExecutable="`readlink -f \"$javaExecutable\"`"
      fi
      javaHome="`dirname \"$javaExecutable\"`"
      javaHome=`expr "$javaHome" : '\(.*\)/bin'`
      JAVA_HOME="$javaHome"
      export JAVA_HOME
    fi
  fi
fi

if [ -z "$JAVACMD" ] ; then
  if [ -n "$JAVA_HOME"  ] ; then
    if [ -x "$JAVA_HOME/jre/sh/java" ] ; then
      # IBM's JDK on AIX uses strange locations for the executables
      JAVACMD="$JAVA_HOME/jre/sh/java"
    else
      JAVACMD="$JAVA_HOME/bin/java"
    fi
  else
    JAVACMD="`which java`"
  fi
fi

if [ ! -x "$JAVACMD" ] ; then
  echo "Error: JAVA_HOME is not defined correctly." >&2
  echo "  We cannot execute $JAVACMD" >&2
  exit 1
fi

if [ -z "$JAVA_HOME" ] ; then
  echo "Warning: JAVA_HOME environment variable is not set."
fi

CLASSWORLDS_LAUNCHER=org.codehaus.plexus.classworlds.launcher.Launcher

# traverses directory structure from process work directory to filesystem root
# first directory with .mvn subdirectory is considered project base directory
find_maven_basedir() {

  if [ -z "$1" ]
  then
    echo "Path not specified to find_maven_basedir"
    return 1
  fi

  basedir="$1"
  wdir="$1"
  while [ "$wdir" != '/' ] ; do
    if [ -d "$wdir"/.mvn ] ; then
      basedir=$wdir
      break
    fi
    # workaround for JBEAP-8937 (on Solaris 10/Sparc)
    if [ -d "${wdir}" ]; then
      wdir=`cd "$wdir/.."; pwd`
    fi
    # end of workaround
  done
  echo "${basedir}"
}

# concatenates all lines of a file
concat_lines() {
  if [ -f "$1" ]; then
    echo "$(tr -s '\n' ' ' < "$1")"
  fi
}

BASE_DIR=`find_maven_basedir "$(pwd)"`
if [ -z "$BASE_DIR" ]; then
  exit 1;
fi

##########################################################################################
# Extension to allow automatically downloading the maven-wrapper.jar from Maven-central
# This allows using the maven wrapper in projects that prohibit checking in binary data.
##########################################################################################
if [ -r "$BASE_DIR/.mvn/wrapper/maven-wrapper.jar" ]; then
    if [ "$MVNW_VERBOSE" = true ]; then
      echo "Found .mvn/wrapper/maven-wrapper.jar"
    fi
else
    if [ "$MVNW_VERBOSE" = true ]; then
      echo "Couldn't find .mvn/wrapper/maven-wrapper.jar, downloading it ..."
    fi
    if [ -n "$MVNW_REPOURL" ]; then
      jarUrl="$MVNW_REPOURL/io/takari/maven-wrapper/0.5.6/maven-wrapper-0.5.6.jar"
    else
      jarUrl="https://repo.maven.apache.org/maven2/io/takari/maven-wrapper/0.5.6/maven-wrapper-0.5.6.jar"
    fi
    while IFS="=" read key value; do
      case "$key" in (wrapperUrl) jarUrl="$value"; break ;;
      esac
    done < "$BASE_DIR/.mvn/wrapper/maven-wrapper.properties"
    if [ "$MVNW_VERBOSE" = true ]; then
      echo "Downloading from: $jarUrl"
    fi
    wrapperJarPath="$BASE_DIR/.mvn/wrapper/maven-wrapper.jar"
    if $cygwin; then
      wrapperJarPath=`cygpath --path --windows "$wrapperJarPath"`
    fi

    if command -v wget > /dev/null; then
        if [ "$MVNW_VERBOSE" = true ]; then
          echo "Found wget ... using wget"
        fi
        if [ -z "$MVNW_USERNAME" ] || [ -z "$MVNW_PASSWORD" ]; then
            wget "$jarUrl" -O "$wrapperJarPath"
        else
            wget --http-user=$MVNW_USERNAME --http-password=$MVNW_PASSWORD "$jarUrl" -O "$wrapperJarPath"
        fi
    elif command -v curl > /dev/null; then
        if [ "$MVNW_VERBOSE" = true ]; then
          echo "Found curl ... using curl"
        fi
        if [ -z "$MVNW_USERNAME" ] || [ -z "$MVNW_PASSWORD" ]; then
            curl -o "$wrapperJarPath" "$jarUrl" -f
        else
            curl --user $MVNW_USERNAME:$MVNW_PASSWORD -o "$wrapperJarPath" "$jarUrl" -f
        fi

    else
        if [ "$MVNW_VERBOSE" = true ]; then
          echo "Falling back to using Java to download"
        fi
        javaClass="$BASE_DIR/.mvn/wrapper/MavenWrapperDownloader.java"
        # For Cygwin, switch paths to Windows format before running javac
        if $cygwin; then
          javaClass=`cygpath --path --windows "$javaClass"`
        fi
        if [ -e "$javaClass" ]; then
            if [ ! -e "$BASE_DIR/.mvn/wrapper/MavenWrapperDownloader.class" ]; then
                if [ "$MVNW_VERBOSE" = true ]; then
                  echo " - Compiling MavenWrapperDownloader.java ..."
                fi
                # Compiling the Java class
                ("$JAVA_HOME/bin/javac" "$javaClass")
            fi
            if [ -e "$BASE_DIR/.mvn/wrapper/MavenWrapperDownloader.class" ]; then
                # Running the downloader
                if [ "$MVNW_VERBOSE" = true ]; then
                  echo " - Running MavenWrapperDownloader.java ..."
                fi
                ("$JAVA_HOME/bin/java" -cp .mvn/wrapper MavenWrapperDownloader "$MAVEN_PROJECTBASEDIR")
            fi
        fi
    fi
fi
##########################################################################################
# End of extension
##########################################################################################

export MAVEN_PROJECTBASEDIR=${MAVEN_BASEDIR:-"$BASE_DIR"}
if [ "$MVNW_VERBOSE" = true ]; then
  echo $MAVEN_PROJECTBASEDIR
fi
MAVEN_OPTS="$(concat_lines "$MAVEN_PROJECTBASEDIR/.mvn/jvm.config") $MAVEN_OPTS"

# For Cygwin, switch paths to Windows format before running java
if $cygwin; then
  [ -n "$M2_HOME" ] &&
    M2_HOME=`cygpath --path --windows "$M2_HOME"`
  [ -n "$JAVA_HOME" ] &&
    JAVA_HOME=`cygpath --path --windows "$JAVA_HOME"`
  [ -n "$CLASSPATH" ] &&
    CLASSPATH=`cygpath --path --windows "$CLASSPATH"`
  [ -n "$MAVEN_PROJECTBASEDIR" ] &&
    MAVEN_PROJECTBASEDIR=`cygpath --path --windows "$MAVEN_PROJECTBASEDIR"`
fi

# Provide a "standardized" way to retrieve the CLI args that will
# work with both Windows and non-Windows executions.
MAVEN_CMD_LINE_ARGS="$MAVEN_CONFIG $@"
export MAVEN_CMD_LINE_ARGS

WRAPPER_LAUNCHER=org.apache.maven.wrapper.MavenWrapperMain

exec "$JAVACMD" \
  $MAVEN_OPTS \
  -classpath "$MAVEN_PROJECTBASEDIR/.mvn/wrapper/maven-wrapper.jar" \
  "-Dmaven.home=${M2_HOME}" "-Dmaven.multiModuleProjectDirectory=${MAVEN_PROJECTBASEDIR}" \
  ${WRAPPER_LAUNCHER} $MAVEN_CONFIG "$@"

```

## 执行方式

指定 解释器执行 , 同时在 sub shell 中执行, 脚本 内容不影响当前 shell

- bash xxx.sh
- sh xxx.sh sh 是 到 bash 的链接

在 脚本中指定 解释器, 在 sub shell 中执行

- ./xxx.sh - 需要 #!/usr/bin/bash
- /root/xxx/yy/zzz.sh - need #!/usr/bin/bash

在当前 shell 中执行: 常用来刷新配置文件

- . xxx.sh
- source xxx.sh


## 各种括号 简写 

https://mp.weixin.qq.com/s/B2fALisq_D8bazpD1hb0Fg
TODO



## 特殊的变量名

### $?

```sh
# 前一个命令的返回值, 执行成功为 0, 
# 如 echo "" &>/dev/null && echo $? 将打印 0
$? 

host=baidu.com
ping -c 1 $host &>/dev/null
if [ $? -eq 0 ]; then
  echo "$host is up"
else
  echo "$host is down"
fi


# 写脚本时有用
# 特殊变量$?记录上一次命令的返回值
[ $? -ne 0 ] && { ... }

```

### $0 $123 $* $@ $#

表示命令本身

```sh

# 在命令行状态下单纯执行 这个命令是没有意义的, 因为他返回当前路径 "."
# 只有在脚本中执行才有意义, 返回脚本路径
cd `dirname $0` 
```

$123 表示参数 第一个 第二个, 第三个

$* $@ 都表示 参数组成的数组, 可以看到不加引号时,二者都是返回传入的参数,但加了引号后,此时$*把参数作为一个字符串整体(单字符串)返回,$@把每个参数作为一个字符串返回, 可在循环迭代中使用

```sh
for N in "$@"
do
    echo $N
done
```

$# 参数个数

```sh

# "shift" 删除前n（默认前1个）个变量
# $0 $1 $2 $3 ... 0 -> 命令； 1 -> 第一个参数
# $# 参数个数；#@ 所有参数


echo -e "total parameter number is: ${#}"
echo -e "whhole parameter is: ${@}"
shift
echo -e "after shift"
echo -e "-> total param num is: ${#}"
echo -e "-> whole param is: ${@}"
shift 2
echo -e "after shift 2"
echo -e "--> total param num is: ${#}"
echo -e "--> whole param is: ${@}"
```


### $$ $!

```sh
# $$记录当前shell本身的 PID
可以使用$$变量扩展出当前进程的 PID 作为临时文件名，PID 在计算机中都是唯一的，所以绝不会重复

# $! Shell最后运行的后台Process的PID



###################################################################################################



# 字符串拓展
 echo {one,two,three}file
onefile twofile threefile

echo {one,two,three}{1,2,3}
one1 one2 one3 two1 two2 two3 three1 three2 three3

cp /very/long/path/file{,.bak}
# 给 file 复制一个叫做 file.bak 的副本

rm file{1,3,5}.txt
# 删除 file1.txt file3.txt file5.txt

mv *.{c,cpp} src/
# 将所有 .c 和 .cpp 为后缀的文件移入 src 文件夹

# 特殊命令!$会替换成上一次命令最后的路径
# 没有加可执行权限
$ /usr/bin/script.sh
zsh: permission denied: /usr/bin/script.sh

$ chmod +x !$
chmod +x /usr/bin/script.sh

# 特殊命令!*会替换成上一次命令输入的所有文件路径
# 创建了三个脚本文件
$ file script1.sh script2.sh script3.sh

# 特殊命令!!，可以自动替换成上一次使用的命令
$ apt install net-tools
E: Could not open lock file - open (13: Permission denied)

$ sudo !!
sudo apt install net-tools

# 给它们全部加上可执行权限
$ chmod +x !*
chmod +x script1.sh script2.sh script3.sh

# 可以在环境变量CDPATH中加入你常用的工作目录，当cd命令在当前目录中找不到你指定的文件/目录时，会自动到CDPATH中的目录中寻找
# 这样就免了经常写完整的路径名称，节约不少时间
$ export CDPATH='~:/var/log'
# cd 命令将会在 ~ 目录和 /var/log 目录扩展搜索

$ pwd
/home/labuladong/musics
$ cd mysql
cd /var/log/mysql
$ pwd
/var/log/mysql
$ cd my_pictures
cd /home/labuladong/my_pictures


# 搜索历史命令
# 过滤出所有包含 config 字段的历史命令
$ history | grep 'config'
找到后, 只要使用!+ 你想重用的命令编号即可运行该命令。

# yes命令自动输入字符y进行确认
$ yes | your_cmd

```

## 空格语法的坑

```sh
var=2 #正确
var = 2 #错误
#bash: var: command not found

# 条件语句中[],中括号内外、运算符两边一定要加空格
var=2
if [ var = 2 ]; then
	echo "对了"
else
	echo "错误"
fi

# expr命令 expr后面、运算符两边一定要加空格
expr 1 + 2

# 运算操作
$((2+4)) # 代码可读差
expr 2 + 4 # 两边一定要加空格
$[2=4] #可读好, 对空格也没严格要求
let a=2+4 # 赋值
let a++ # 自增
```

## 各种执行方式和差异

- 直接命令下达 - 会在一个新的 子 bash 环境下执行脚本, 子环境中的变量不会传到 parent bash 中来

    - 绝对路径

    - 相对路径 "./xxx.sh"

    - 变量 PATH 功能: 置入 PATH 包含的路径

    - 通过 bash 执行 "bash xxx.sh" or "sh xxx.sh"

- 使用 `source` 执行 - 直接在 parent bash(就是当前 bash) 中执行


## 语法

### 数据类型

#### 字符串 打印

```sh
#!/bin/bash

PATH=${PATH}:/bin:/sbin:~/bin
export PATH

# -e 激活转义字符
echo -e "hello world"
exit 0

```

#### 数组

```sh
myarray=()  #定义一个空数组
myarray=(1 2 3 4 5)   #全部元素为数字
myarray=(one two three four five)  #全部元素为字符串
myarray=(1 two 3 three 4 five)  #元素由数值和字符串构成
myarray=(1 two 3 “a phrase” 5 now)

# 赋值
myarray[0]=1
myarray[2]=test

# 获取
arr_element2=${arr_name[2]}

# 长度
arr_len=${#arr_name[*]}
        ${#arr_name[@]}
# 元素长度
arr_elem_len=${#arr_name[index]} 

# 打印
echo ${arr[@]} # 将元素作为一个整体打印
for ele in ${arr[*]} # 迭代每个元素打印
do echo $ele
done

# 数组分片访问
arr_name=(1 2 3 test go now)
echo ${arr_name[@]:1:2}，将输出2 3

# 删除
删除数组某个元素：unset arr_name[index]
删除整个数组：unset arr_number

# 合并
arr=()
arr+=(1)
echo ${arr[@]}# 1
arr+=(2 tes 3 xx)
echo ... # 1 2 tex 3 xx


ip_array=("192.168.1.1" "192.168.1.2" "192.168.1.3") 
for ip in ${ip_array[*]} 
do 
    if [ $ip = "192.168.1.1" ]; then 
        port="7777" 
    else 
        port="22" 
    fi 
    ssh -t -p $port $user@$ip "remote_cmd" 
done 
```

### 读取标准输入

```sh

#!/bin/bash
# "read" usage

PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH

read -p "input first name: " firstname
read -p "input last name: " lastname
echo -e ${firstname} ${lastname}

#############################

#!/bin/bash

# create 3 files, which named by user's input and date command;
echo -e "I will create 3 files."
read -p "input file name please: " file_name
file_name=${file_name:-"filename"}

date1=$(date --date="2 days ago" +%Y%m%d) # 前两天
date2=$(date --date="1 days ago" +%Y%m%d) # one day ago
date3=$(date +%Y%m%d) # today

file1=${file_name}${date1}
file2=${file_name}${date2}
file3=${file_name}${date3}

# touch

touch ${file1}
touch ${file2}
touch ${file3}

```

### 数值运算

```sh


#!/bin/bash

# desc: + - * / % 数值运算 $((xxx*xxx))
# declare -i total=${first_num}*${sec_num}
# <==> total=$((a*b))

PATH=${PATH}:/bin:/sbin:~/bin
export PATH

echo -e "input 2 numbers and I will cross them.\n"

read -p "first number: " first_num
read -p "second number: " sec_num

total=$((${first_num}*${sec_num}))
echo -e "result: ${total}"

```

### 条件判断


```sh
#!/bin/bash

# 判断
# test xx==xx <==> [ "xx" == "xx" ] (tips: [] 内部的变量都用双引号抱起来以防止变量中有空格; 每个部分都要隔开)

# -e (exist) 
# -f (file)  是否是文件(file exist)
# -d (directory) ; 是否是文件夹

# -rwx (readable? writeable? exectable?) ;

# -eq (equal) 
# -ne (not equal) 

# test -z "xxx" (zero) 判断string是否为空 0/null;  
# test -n "xxx" (not zero, 可省略) 不为0; 
# test "xxx"=="yyy" ("="也可)是否相等;
#  test "xxx"!="yyy" 是否不等;

# -a (and)
#  -o (or)
#  ! (not)

PATH=${PATH}:/bin:/sbin:~/bin
export PATH

echo -e "I'll test wether the file name you input exist. "
read -p "input a file name: " file_name

test -z ${file_name} && echo -e "you must input a valid file name" && exit 0
test ! -e ${file_name} && echo "the file ${file_name} does not exist" && exit 0

test -f ${file_name} && type="regular file"
test -d ${file_name} && type="directory"
test -r ${file_name} && perm="readable"
test -w ${file_name} && perm="${perm} writable"
test -x ${file_name} && perm="${perm} excutable"

echo -e "it's a ${type}"
echo -e "and the permissions are: ${perm}"

##########################################

#!/bin/bash

# desc: make choice 判断的另一种写法

read -p "input Y/N: " yn
[ "${yn}"=="Y" -o "${yn}"=="y" ] && echo -e "ok, continue." && exit 0
[ "${yn}"=="N" -o "${yn}"=="n" ] && echo -e "oh, break." && exit 0
echo -e "I don't know what you mean." && exit 0

#######################################


#!/bin/bash

# "if then" usage
# [ "${param}" == "xxx" -o "${param}" == "yyy" ] <==> [ "${p}" == "xxx" ] || [ "${p}" == "yyy" ]

read -p "input y/n: " p
if [ "${p}" == "y" ] || [ "${p}" == "Y" ]; then
        echo "ok, continue."
        exit 0
elif [ "${p}" == "n" ] || [ "${p}" == "N" ]; then
        echo "oh, interrupt"
else
        echo "I don't know what you mean." && exit 0
fi

##################################################

```

### 条件判断-case 语法

```sh

#!/bin/bash

# case xx in 用法

case ${1} in
        "hello")
                echo "hello, h a y"
                ;;
        "")
                echo "must input parameter, eg> ${0} someword"
                ;;
        *)
                echo "usage: ${0} hello"
                ;;
esac

```

### 条件判断-多路分支语法

```sh
action="a"
if [ "${action}" = "a" ]; then
    #操作
elif [ "${action}" = "b" ]; then
    #操作
elif [ "${action}" = "c" ]; then
    #操作
else
    #操作
fi

```

### 默认值

```sh
file_name=$1
file_name=${file_name:-"no_name.sh"}

```

### 函数

```sh

#!/bin/bash

# 定义 函数

function printit(){ 
  echo "Your choice is $1"   # 这个 $1 是 函数后面跟着的参数
} 
case $1 in # 这里的 $1 是脚本后面的参数
  "one") 
    printit 1  # 请注意， printit 指令后面还有接参数！ 
    ;; 
  "two") 
    printit 2 
    ;;
  "three") 
    printit 3 
    ;; 
  *) 
    echo "Usage $0 {one|two|three}" 
    ;; 
esac

###############################

# 循环 while do done, until do done 条件满足则种植循环, for var in value1 value2 value3... do done

#!/bin/bash

# "cut", function, for var in ... do done

PATH=${PATH}:/bin:/sbin:~/bin
export PATH

names=$(cut -d ":" -f1 /etc/passwd)
for name in ${names}
do
        id ${name}
        finger ${name}
done

########################################################

network="192.168.1"              # 先定丿一个网域的前面部分！ 
for sitenu in $(seq 1 100)       # seq 为 sequence(连续) 的缩写 
do 
    # 底下的程序在 ping 的回传值是正确的还是失败的！ 
    ping -c 1 -w 1 ${network}.${sitenu} &> /dev/null && result=0 || result=1 
    # 开始显示结果是正确的 (UP) 还是错误的没有连通 (DOWN) 
    if [ "$result" == 0 ]; then 
        echo "Server ${network}.${sitenu} is UP." 
    else 
        echo "Server ${network}.${sitenu} is DOWN." 
    fi 
done 

###################################

# 循环加判断

# 1. 先看看这个目录是否存在啊？ 
read -p "Please input a directory: " dir 
if [ "$dir" == "" -o ! -d "$dir" ]; then 
  echo "The $dir is NOT exist in your system." 
  exit 1 
fi 
 
# 2. 开始测试档案啰～ 
filelist=$(ls $dir)        # 列出所有在该目录下的文件名 
for filename in $filelist 
do 
  perm="" 
  test -r "$dir/$filename" && perm="$perm readable" 
  test -w "$dir/$filename" && perm="$perm writable" 
  test -x "$dir/$filename" && perm="$perm executable" 
  echo "The file $dir/$filename's permission is $perm " 
done 

##########################################

# for ((xx; xx; xx)) do done

for (( i=1; i<=$nu; i=i+1 )) 
do 
  s=$(($s+$i)) 
done 
echo "The result of '1+2+3+...+$nu' is ==> $s"

```

## debug

```shell
sh [-nvx] scripts.sh

-n  ：不要执行 script，仅查询语法的问题； 
-v  ：再执行 sccript 前，先将 scripts 的内容输出屏幕上； 
-x  ：输出一行脚本, 执行这一行，debug 常用

sh -x xx.sh
```

## 学习几个demo

这是一个 Java 程序的启动脚本

```sh
#!/bin/sh
## java env
export JAVA_HOME=/usr/local/jdk1.7.0_55
export JRE_HOME=$JAVA_HOME/jre

## exec shell name
EXEC_SHELL_NAME=sys-service\.sh
## service name
SERVICE_NAME=zhousw-sys-service
SERVICE_DIR=/usr/local/workspace/sys-service
JAR_NAME=$SERVICE_NAME\.jar
PID=$SERVICE_NAME\.pid

cd $SERVICE_DIR

case "$1" in

    start)
        nohup $JRE_HOME/bin/java -Xms256m -Xmx512m -jar $JAR_NAME >/dev/null 2>&1 &
        echo $! > $SERVICE_DIR/$PID
        echo "#### start $SERVICE_NAME"
        ;;

    stop)
        kill `cat $SERVICE_DIR/$PID`
        rm -rf $SERVICE_DIR/$PID
        echo "#### stop $SERVICE_NAME"

        sleep 5

        TEMP_PID=`ps -ef | grep -w "$SERVICE_NAME" | grep -v "grep" | awk '{print $2}'`
        if [ "$TEMP_PID" == "" ]; then
            echo "#### $SERVICE_NAME process not exists or stop success"
        else
            echo "#### $SERVICE_NAME process pid is:$TEMP_PID"
            kill -9 $TEMP_PID
        fi
        ;;

    restart)
        $0 stop
        sleep 2
        $0 start
        echo "#### restart $SERVICE_NAME"
        ;;

    *)
        ## restart
        $0 stop
	sleep 2
        $0 start
        ;;

esac
exit 0



```


# 正则&文本格式化

## 正则表达式和语言系统有关系

<img src="Screenshot_203.png" width=50% />

## 正则中的特殊符号

<img src="Screenshot_204.png" width=50% />

## grep进阶用法

[grep基础用法](#截取命令)

<img src="Screenshot_205.png" width=50% />

正则表示法中的特殊符号

<img src="Screenshot_206.png" width=50% /><img src="Screenshot_207.png" width=50% /><img src="Screenshot_208.png" width=50% />

* `grep -n 'key' <file>` 搜寻key
* `grep -vn 'key' <file>` 反向选择
* `dmesg | grep 'eth'`找到核心信息的eth那行
* ` dmesg | grep -n --color=auto 'eth'` 关键词显色, 显示行号
* `dmesg | grep -n -A3 -B2 --color=auto 'eth'` 前2行和后3行一起显示
* `grep -n 't[ea]st' <file>`搜寻即可字符(test 或 tast)
* ` grep -n '[^g]oo' regular_express.txt ` 不为指定字符
* `grep -n '[^a-z]oo' regular_express.txt ` 不为某个字符范围
* ` grep -n '[0-9]' regular_express.txt` 取得有数字的行
* ` grep -n '^the' regular_express.txt `以指定字符开头
* `grep -n '^[a-z]' regular_express.txt` 开头为小写字符
* ` grep -n '\.$' regular_express.txt ` 以指定字符结束
* ` grep -n 'o\{2\}' regular_express.txt ` 指定字符出现次数


如果希望设定grep自动关键词显色, 可修改`~/.bashrc` , 加上一行:`alias grep='grep --color=auto'`, 然后`source ~/.bashrc`生效

## 延伸正则表示法

egrep

<img src="Screenshot_211.png" width=50% /><img src="Screenshot_212.png" width=50% />


## sed

还可以将数据进行取代、删除、新增、抓取特定行

<img src="Screenshot_209.png" width=50% /><img src="Screenshot_210.png" width=50% />

* `nl /etc/passwd | sed [-e] '2,5d'` 带行号列出, 并删除2-5行(sed后的动作需要引号)
* ` nl /etc/passwd | sed '2a drink tea' ` 第2行后添加文字
* ` nl /etc/passwd | sed '2i drink tea' `行前添加文字
* sed增加多行, 需要\+enter跳脱
* `nl /etc/passwd | sed '2,5c No 2-5 number' ` 取代2-5行
* `nl /etc/passwd | sed -n '5,7p'` 抓取5-7行, 同`head -n 7 | tail -n 2`

sed的行内格式化: `sed 's/要被取代的字符串/新字符串/g'`

* `ifconfig eth0 | grep 'inet addr' | sed 's/^.*addr://g' | sed 's/Bcast.*$//g'`获取ip（替换功能）
* `sed -i 's/匹配串/替换成的内容/g' /xxx `替换，直接修改原始档案


## awk行内格式化

格式: `awk '条件类型1{动作 1} 条件类型 2{动作 2} ...' filename`; 默认的行内分隔符为空格或者tab; 变量可以直接用, 无需加$符号

* `last -n 5 | awk '{print $1 "\t" $3}' ` 没有条件,只有动作; $1代表第一栏, $2表示第二栏...; $0表示一整栏

awk的内建变量:

<img src="Screenshot_214.png" width=50% />

* `last -n 5| awk '{print $1 "\t lines: " NR "\t columes: " NF}' `和printf联用, printf的内容用双引号
* ` awk 'BEGIN{FS=":"} $3 < 10 {print $1 "\t " $3}` 不用默认的分隔符号


## printf格式化打印

<img src="Screenshot_213.png" width=50% />

* `printf '%s\t %s\t %s\t \r\n' $(cat /xxx) `格式化打印

## 文件对比工具

diff: 以行为对比单位

<img src="Screenshot_215.png" width=50% />

cmp: 字节为单位对比

patch: 对比升级

<img src="Screenshot_216.png" width=50% />

* ` diff -Naur passwd.old passwd.new > passwd.patch` 制作差异补丁文件
* ` patch -p0 < passwd.patch ` 更新
* `  patch -R -p0 < passwd.patch ` 还原

`-p0`的解释见[原始码&tarball](#原始码&tarball)




# bash 


## 快捷键

Ctrl+u 清除当前命令行，命令输入了一半，不想继续，用delete太慢，Ctrl+c太笨，Ctrl+u 刚合适
Ctrl+k 清除光标后内容
Ctrl+w 清除光标前的一个单词
Ctrl+a 光标移至行首，很常用
Ctrl+e 光标移至行尾，很常用
Ctrl+l 清屏，等同于clear
Ctrl+z 当前命令行执行程序挂起，想退出命令，又不想终止程序，可以使用这个命令

## 几种 bash 环境 startup 配置文件对比

- 交互式 shell: 在终端上执行，shell等待你的输入，并且立即执行你提交的命令, 如 登录、执行一些命令.

- 非交互shell: 以shell script(非交互)方式执行, 当它读到文件的结尾EOF，shell也就终止了. 

    “$-”变量的值（代表着当前shell的选项标志）, 查看字符串中是否包含“i”选项（表示interactive shell）来区分交互式与非交互式shell

- 登录shell:需要用户名、密码登录后才能进入的shell（或者在脚本中通过”–login”选项生成的shell）, 称为一个会话

- 非登录 shell: 不需要输入用户名和密码即可打开的Shell，例如：直接命令“bash”就是打开一个新的非登录shell, 通过 terminal 打开的 shell

对于bash, 登录shell（包括交互式登录shell和使用“–login”选项的非交互shell），它会首先读取和执行 `/etc/profile`全局配置文件, 接着执行` /etc/profile.d/`目录下的脚本文件，然后依次查找 `~/.bash_profile、~/.bash_login 和 ~/.profile`这三个配置文件, 然后 ~/.bashrc, 登出, 执行 `~/.bash_logout`; su - root 就是切到 root 的登录 shell

在非登录shell里，只读取` ~/.bashrc （和 /etc/bash.bashrc、/etc/bashrc ）`文件，不同的发行版里面可能有所不同, 如果装了 zsh, 则先读取 .zshrc, 一般会在 .zshrc 中 `source .bash_profile`; su root 就是 切到 root  nologin shell


## shell and python interaction

```sh

#!/bin/bash

ping -c 1 baidu.com &>/dev/null && echo "network ok" || echo "network error"

# eof 只是标识符, 可以随意定义
# <<- 为必须,
/usr/bin/python3 <<-eof
print("hello py")
eof

echo "hello bash"
```


## 示例

https://github.com/jaywcjlove/shell-tutorial
https://github.com/wangdoc/shell-tutorial/blob/master/docs/basics/variable.md

```sh


#!/bin/bash

echo  "helloworld!"

# 运行
./demo.sh


# #################shell 变量

# - 环境变量,
# 区分大小写

# - shell变量
# PATH 决定了shell将到哪些目录中寻找命令或程序 
# HOME 当前用户主目录 
# HISTSIZE　历史记录数 
# LOGNAME 当前用户的登录名 
# HOSTNAME　指主机的名称 
# SHELL 当前用户Shell类型
# LANGUGE 　语言相关的环境变量，多语言可以修改此环境变量
# MAIL　当前用户的邮件存放目录 
# PS1　基本提示符，对于root用户是#，对于普通用户是$
echo $HOME
echo ${HOME} 

###############自定义变量 字符串#################
#!/bin/bash
#自定义变量hello
# 变量声明默认都是string类型
# 等号两边无空格, 若值中有空格, 用引号(单双均可)括起来
#   [""]中的 [$] 可以保有原本特性, 如 var="lang is $LANG"
#   ['']中的 [$] 仅仅是一般字符

#* 设定变量后需要`export <变量名>` 导出以在其他程序中使用
#* 系统变量一般为大写, 自定义变量为小写方便判断
#* `unset <变量名>` 取消变量设定
hello="hello world"
# or
hello='hello world'
echo $hello
echo  "helloworld!"
echo 'hello - ${PATH}' # hello - ${PATH}

#获取字符串长度
name="SnailClimb"
# 第一种方式
echo ${#name} #输出 10
# 第二种方式
expr length "$name";

# 计算表达式

# 使用 expr 命令时，表达式中的运算符左右必须包含空格，如果不包含空格，将会输出表达式本身:
expr 5 + 6       // 输出 11
expr 5+6    // 错误, 直接输出 5+6

# 对于某些运算符，还需要我们使用符号\进行转义
expr 5 * 6       // 输出错误
expr 5 \* 6      // 输出30

################## string 截取###################3


# 截取子串
#从字符串第 1 个字符开始往后截取 10 个字符
# 从 0 开始, 左闭右开 
str="SnailClimb is a great man"
echo ${str:0:10} #输出:SnailClimb

# 查找子串
# 查找字符 i 或 o 的位置(哪个字母先出现就计算哪个)：
string="runoob is a great site"
echo `expr index "$string" io`  # 输出 4, index 从 1 开始

# 根据表达式截取
#    #:      表示左→右开始匹配，匹配最短的, 然后删除, 返回剩余的串
#    ##： 表示左→右开始匹配，匹配最长的, 然后删除
#    %：   表示右→左开始匹配，匹配最短的, 然后删除
#    %%：表示右→左开始匹配，匹配最长的, 然后删除

#!bin/bash
#author:xiaoyu

var="http://www.runoob.com/linux/linux-shell-variable.html"

s1=${var%%t*}#h
s2=${var%t*}#http://www.runoob.com/linux/linux-shell-variable.h
s3=${var%%.*}#http://www
s4=${var#*/}#/www.runoob.com/linux/linux-shell-variable.html
s5=${var##*/}#linux-shell-variable.html

####################shell数组############################

#!/bin/bash
array=(1 2 3 4 5);
# 获取数组长度
length=${#array[@]}
# 或者
length2=${#array[*]}
#输出数组长度
echo $length #输出：5
echo $length2 #输出：5
# 输出数组第三个元素
echo ${array[2]} #输出：3
unset array[1]# 删除下表为1的元素也就是删除第二个元素
for i in ${array[@]};do echo $i ;done # 遍历数组，输出： 1 3 4 5 
unset arr_number; # 删除数组中的所有元素
for i in ${array[@]};do echo $i ;done # 遍历数组，数组元素为空，没有任何输出内容

############### 引入其他指令 #####################

# 引入其他指令, 借助反单引号 " \` ", 如 
system=\`uname\`----\`uname -r\`
echo system # 显示Linux-----3.10.0-693.2.2.el7.x86_64 ; 

# 另外还有一种方法: 
system=$(uname)----$(uname -r)


```

## 所有可用的shell

bash是linux可以使用的shell之一(zsh, dash...), 查看系统可以使用的全部shell可以`vim /etc/shells`, 见下图:

某个用户登录可以使用的shell, 记录在 /etc/passwd

bash优点:

* 历史命令记录, 在 ${user.home}/.bash_history, 如root的在 ~/.bash_history

* 命令/档案补全

* alias, 如 `alias lm='ls -al' `

* (job control, foreground, background. 后台运行

* shell script

* 支持通配符 `*`

`type <cmd>` 列出指令使用情况 可以当作[which](#搜寻文件和指令)用

## 环境变量

* `env` 查看所有环境变量(也可以用`export`)

    <img src="Screenshot_164.png" width=50% /><img src="Screenshot_165.png" width=50% />
    
    * ` declare -i number=$RANDOM*10/32768` 定义随机数number, 范围是0---9

* `set` 查看环境变量和自定义变量(也可以用`declare`)

    <img src="Screenshot_166.png" width=50%/><img src="Screenshot_167.png" width=50%/>

* 设定命令提示符PS1 `echo $PS1`

    <img src="Screenshot_168.png" width=50%/><img src="Screenshot_169.png" width=50%/>

* `echo $$` 本shell 的 线程代号(PID)

* `echo $?` 上个命令的返回值(一般上个指令正常结束返回0, 异常则返回错误代码)

* `export <自定义变量名>` 自定义变量转为环境变量
    * 环境变量能被子程序识别(子程序继承了环境变量), 自定义变量无法被子程序识别(无法继承)

* `locale -a`查看所有支持的语系
* `locale` 查看本机的语系设定

    <img src="Screenshot_170.png" width=50%/ >


## 键盘读取read

<img src="Screenshot_171.png" width=50%/>

* `read <自定义变量名>` 读取键盘输入, 存入指定变量
* `read -p "Please keyin your name: " -t 30 named` 带提示信息, 限制等待时间30s

## declare,typeset声明变量类型

<img src="Screenshot_172.png" width=50% />

* ` declare -i sum=100+300+50 `, 此时sum为450
* `declare -x <变量名>` 转为环境变量 ,同 `export xxxx`
* ` declare +x <环境变量名>` 移除环境变量
* `declare -r <变量名>` 声明为只读
* `declare -p sum` 单独列出变量类型

## 变量的删除取代

```sh
#:      表示左→右开始匹配，匹配最短的, 然后删除
##： 表示左→右开始匹配，匹配最长的, 然后删除
%：   表示右→左开始匹配，匹配最短的, 然后删除
%%：表示右→左开始匹配，匹配最长的, 然后删除

匹配方向有别，但是匹配表达式还是顺序的

取代看后面的图

var="http://www.runoob.com/linux/linux-shell-variable.html"

s1=${var%%t*}#h
s2=${var%t*}#http://www.runoob.com/linux/linux-shell-variable.h
s3=${var%%.*}#http://www
s4=${var#*/}#/www.runoob.com/linux/linux-shell-variable.html
s5=${var##*/}#linux-shell-variable.html
```

<img src="Screenshot_175.png" width=50% /><img src="Screenshot_176.png" width=50% /><img src="Screenshot_177.png" width=50% /><img src="Screenshot_178.png" width=50% /><img src="Screenshot_179.png" width=50% /><img src="Screenshot_180.png" width=50% />

<img src="Screenshot_181.png" width=50% />

## 变量的测试和内容替换

判断变量是否存在

- xxx=${xxx-yyy} 如果xxx未设定，则设为yyy
- xxx=${xxx:-root} 如果未设定or为空串，设为yyy

更多见:

<img src="Screenshot_182.png" width=50% />

## 命令别名和设定

* 设定别名`alias lm='ls -al | more' `
* 查看所有`alias`
* 取消设定` unalias lm `

## 命令历史

<img src="Screenshot_183.png" width=50% /><img src="Screenshot_184.png" width=50% />

* `history` 列出所有的命令历史
* `history 3` 最近3个命令

## 指令搜寻顺序

1.   以相对/绝对路径执行命令，例如『 /bin/ls 』和『 ./ls 』； 
2.   由 alias 找到命令执行； 
3.   由 bash 内建的 (builtin) 命令来执行； 
4.   透过 $PATH 这个发量的顺序搜寻到第一个命令执行。

## bash登录欢迎信息

* ` cat /etc/issue`查看进站信息

    <img src="Screenshot_185.png" width=50% />

* `cat /etc/issue.net` 查看使用telnet登录的欢迎信息

* `cat /etc/motd` 大家登录都会看到的提示信息

## bash的环境配置文件

 login shell(输入帐号密码) 和 non-login shell(无需输入帐号密码就能登录)两种登录shell类型登录时读取的配置文件不同

 login shell只读取两个配置文件: 
 
 1. /etc/profile(系统的整体设定, 不要修改)

    * 每个使用者登录都会读取, 设定这些变量

        <img src="Screenshot_186.png" width=50% />

    * 此外, 还会读取外部的配置文件

        * /etc/inputrc 设定一些热键
        * /etc/profile.d/*.sh (多个), 规定了bash接口的颜色, 语系一些命令别名; 如果需要为所有的user设定命令别名则修改
        * /etc/sysconfig/i18n 这个档案是由 /etc/profile.d/lang.sh 调用！这也是我们决定 bash 使用什么预习,LANG的设定

 2. ~/.bash_profile 或 ~/.bash_login 或 ~/.profile---------都是个人配置文件, 在1和1的下属配置文件都调用的才调用它

    * 3个文件之后有一个被读取, 以上是读取顺序, 一旦前者被读取, 后者就不会读取了

    * 以~/.bash_profile为例, 实际会读取~/.bashrc, 可以将自定义配置写入该文件, 通过`source ~/.bashrc` 读入配置; 或者`. ~/.bashrc`

login shell只读取~/.bashrc

此外, 还有其他的配置文件会影响bash

/etc/man.config 

<img src="Screenshot_187.png" width=50% />

~/.bash_history 

<img src="Screenshot_188.png" width=50% />

~/.bash_logout

<img src="Screenshot_189.png" width=50% />

## 终端机的环境设定stty,set

`stty -a`列出所有按键和按键内容

<img src="Screenshot_190.png" width=50%/>

比如设定` stty erase ^h` 使用ctrl+h进行删除

## 通配符和特殊符号

<img src="Screenshot_191.png" width=50% /><img src="Screenshot_192.png" width=50% /><img src="Screenshot_193.png" width=50% />

## 数据流重导向

<img src="Screenshot_194.png" width=50%/>
<img src="Screenshot_195.png" width=50%/>

* ` ll / > <file>` 将数据输出到指定文件(数据会被覆盖)
* ` ll / >> <file>` 数据会被累加
* 查看/home下是否有.bashrc档案存在` find /home -name .bashrc `, 有出错信息会直接输出到屏幕, 现在将正常结果和错误信息输出到不同文件`find /home -name .bashrc > list_right 2> list_error`
* `find /home -name .bashrc 2> /dev/null` 将错误输出扔掉( /dev/null 可以吃掉任何导向这个装置的信息)
* ` find /home -name .bashrc > list 2>&1` 正确输出和错误输出写入同一个文件
* `cat > <file>` 将键盘输入存入指定文件(ctrl+d结束输入)
* ` cat > catfile < ~/.bashrc ` 新建立的catfile就是后者的复制
* `cat > catfile << "eof"` 输入"eof"就结束输入(<<表示后面接的是结束字符)
* 多重重导向 tee 

    <img src="Screenshot_202.png" width=50% />

    * `last | tee last.list | cut -d " " -f1 ` 同时输出一份数据到两处地方
    * ` ls -l /home | tee ~/homefile | more`同上
    * ` ls -l / | tee -a ~/homefile | more ` 累加输出

## &&和||

<img src="Screenshot_196.png" width=50% />

## 管线命令

<img src="Screenshot_197.png" width=50% />

## 截取命令

cut: 主要用于将每一行里的数据分解, 去除想要的分段

<img src="Screenshot_198.png" width=50% />
 
* `echo $PATH | cut -d ':' -f 5`获取 PATH 内容，找出第五个路径
* `echo $PATH | cut -d ':' -f 3,5` 找到第三个和第五个路径
* `export | cut -c 12-` 每一行从第12个字符开始取

grep: 从多行中去除需要的行, 见[grep进阶用法](#grep进阶用法)

<img src="Screenshot_199.png" width=50% />

* `last | grep 'root'` 含有root的行
* ` grep -v 'root'` 没有root的行
* ` grep --color=auto '<key>' <file> ` 在指定文件中寻找

## 排序sort,wc,uniq

sort: sort将文件/文本的每一行作为一个单位，相互比较，比较原则是从首字符向后，依次按ASCII码值进行比较，最后将他们按升序输出

<img src="Screenshot_200.png" width=50% /><img src="Screenshot_201.png" width=50% />

对于`-k`: 格式：-k  fstart.cstart, fend.cend，都是数字，从1开始，若有多重排序，后面再接 -k ...eg:http://wangchujiang.com/linux-command/c/sort.html

* `cat /etc/passwd | sort` 默认排序
* `cat /etc/passwd | sort -t ':' -k 3` 冒号分开,用第三栏排序
* `cat /etc/passwd | sort -t ':' -k 3 -n ` 以数字进行排序(默认为文字排序)

uniq: 将重复的数据只显示一行(必须在使用sort之后使用)

wc: 显示总字符, 总行数

## 字符转换命令

tr: 删除 文字替换

* `last | tr '[a-z]' '[A-Z]'` 所有的小写变为大写
* `cat /etc/passwd |tr -d ":"` 将passwd中所有的[:]删除
* `cp /xxx /root/xxx && unix2dos /root/xxx `将/xxx复制到/root下并转成dos断行
* ` cat /xxx | tr -d '\r' > /root/xxx `删除/xxx中的换行符号

col: tab以空格显示

* ` cat -A /xxx `显示出所有按键符号
* `cat /etc/man.config | col -x | cat -A | more` 显示tab为空格

expand: 替换tab为空格

split: 档案分割

xargs: 主要用于将前面的输出流格式化成输入参数

join, paste


## 字符串

删库悲剧:

```sh
# https://mp.weixin.qq.com/s/arUO8dhdUa-L1evglIQiog

cur_path = `pwd`
new_cur_path = `${cur_path}/bbb` # 问题: `` 包围的代码会先执行, 但是 这里 反引号中的内容不是有效的命令, 返回空
rm -rf ${new_cur_path}/* 2>>/dev/null # 最终变成 rm -rf /*

# 改正: 将 `` 去掉, 或者再加上 ""
# 最好的办法是:
#如果目录不为空，才执行删除操作
if [ ${work_path} != "" ];then
    rm -fr ${work_path}/*
fi

```