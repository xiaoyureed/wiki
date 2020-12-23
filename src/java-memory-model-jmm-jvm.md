---
title: JMM and GC
tags:
  - jvm
  - gc
date: 2015-03-07 11:02:43
categories: java core
---
<div align="center">
Java Memory Model.
references: [1](https://zhuanlan.zhihu.com/p/34461147)
Java 内存模型: https://zhuanlan.zhihu.com/p/42546444
https://openjdk.java.net/projects/panama/ - 和cpp交互, native 方法
</div>

<!--more-->
<!-- TOC -->

- [为什么要有jmm](#为什么要有jmm)
- [jvm内存区域模型](#jvm内存区域模型)
  - [Method Area](#method-area)
  - [Heap](#heap)
  - [JVM Stack](#jvm-stack)
  - [Native Method Stack](#native-method-stack)
  - [PC Register(程序计数器)](#pc-register程序计数器)
- [gc 垃圾回收](#gc-垃圾回收)
  - [垃圾收集器](#垃圾收集器)
  - [如何判断对象是否应该回收](#如何判断对象是否应该回收)
    - [引用计数法 reference counting](#引用计数法-reference-counting)
    - [可达性分析算法 reachability analysis](#可达性分析算法-reachability-analysis)
    - [reference 分类](#reference-分类)
  - [垃圾回收算法](#垃圾回收算法)
    - [标记-清除 算法](#标记-清除-算法)
    - [复制算法](#复制算法)
    - [标记-整理 算法](#标记-整理-算法)
    - [分代收集算法](#分代收集算法)
- [Java的代码执行机制](#java的代码执行机制)
  - [类编译](#类编译)
  - [类加载](#类加载)
    - [classloader类加载器](#classloader类加载器)
    - [装载(load)](#装载load)
    - [链接(Link)](#链接link)
    - [初始化(Initialize)](#初始化initialize)
  - [类执行](#类执行)
    - [字节码解释执行and编译执行并存如何分界](#字节码解释执行and编译执行并存如何分界)
    - [字节码解释执行](#字节码解释执行)
      - [方法相关的jvm指令](#方法相关的jvm指令)
      - [jvm指令怎么解释执行](#jvm指令怎么解释执行)
    - [字节码编译执行](#字节码编译执行)
      - [client compiler](#client-compiler)
      - [server compiler](#server-compiler)
    - [反射执行](#反射执行)
- [java中的关键字和内存的关系](#java中的关键字和内存的关系)
  - [final](#final)
  - [volatile](#volatile)
  - [synchronized](#synchronized)
  - [static](#static)
- [java的内存泄露](#java的内存泄露)
  - [会造成哪些溢出](#会造成哪些溢出)
  - [Java内存泄露引起原因](#java内存泄露引起原因)
- [jvm 中的 object](#jvm-中的-object)
  - [对象的创建过程(内存怎么分配)](#对象的创建过程内存怎么分配)
  - [对象的访问](#对象的访问)
    - [对象的2种访问方式](#对象的2种访问方式)
    - [thread具体怎么访问对象中的值呢以及volatile关键字](#thread具体怎么访问对象中的值呢以及volatile关键字)
  - [对象回收(内存怎么释放)](#对象回收内存怎么释放)
- [有哪些虚拟机](#有哪些虚拟机)
- [java线上问题诊断排查](#java线上问题诊断排查)
  - [日志检索](#日志检索)
  - [查看数据库连接](#查看数据库连接)
  - [查看CPU 负载](#查看cpu-负载)
    - [CPU User高 and  CPU Load 高](#cpu-user高-and--cpu-load-高)
      - [代码中存在非常消耗 CPU 的操作](#代码中存在非常消耗-cpu-的操作)
      - [频繁gc](#频繁gc)
    - [CPU System 高 and CPU Load 高](#cpu-system-高-and-cpu-load-高)
    - [CPU Wait高 and CPU Load高 and CPU利用率低](#cpu-wait高-and-cpu-load高-and-cpu利用率低)
  - [查内存](#查内存)
  - [排查垃圾回收问题](#排查垃圾回收问题)
  - [网络问题](#网络问题)
- [jdk 自带命令行工具](#jdk-自带命令行工具)
  - [java 启动 javac 编译 javap 反编译](#java-启动-javac-编译-javap-反编译)
  - [jstack](#jstack)
  - [jcmd](#jcmd)
  - [jps](#jps)
  - [jstat](#jstat)
  - [jvisualvm](#jvisualvm)
  - [jconsole](#jconsole)
  - [jhat](#jhat)
  - [jmap](#jmap)
  - [jrunscript](#jrunscript)
  - [jinfo](#jinfo)
  - [jprofile](#jprofile)
- [阿里 arthas](#阿里-arthas)
- [jvm 性能调优 and 实用参数](#jvm-性能调优-and-实用参数)
  - [有哪些参数](#有哪些参数)
  - [配置方式](#配置方式)

<!-- /TOC -->

# 为什么要有jmm


JMM可以屏蔽掉各种硬件和操作系统的内存访问差异，以实现让java程序在各种平台下都能达到一致的内存访问效果。

对于串行执行的程序, 天然就是正确的, 但是对于并行程序就需要 jmm 来保证多个 thread 协同工作

# jvm内存区域模型

<img src="Snipaste_2018-05-27_21-52-26.png" width="50%">
<img src="Snipaste_2018-05-27_22-58-34.png" width="50%">


- 线程共享内存区

    - method area (方法区)

        保存 类 信息 (如 class name, 方法定义, 字段信息)

        对于频繁创建类的系统 (如存在大量反射, 动态代理的系统), 方法区要设置大一点

    - heap (堆)

        保存对象实例, 数组

        gc 主要是在这里

        空间不足会抛出 java.lang.OutOfMemoryError

        空间远大于 jvm stack

        常量池: 

- 线程私有内存区

    - register (程序计数器)
    
        用于在 thread 上下文切换时记录当前执行到字节码的哪一步 

    - jvm stack (栈)

        保存方法调用栈, 对象引用, 局部变量

        空间不足会抛出 Stack Overflow

    - ~~native method stack~~ (sun jdk 实现中并入 jvm stack)


## Method Area

示意图:

![alt](Snipaste_2018-10-13_17-14-24.png)

这里 "对象类型数据" 即 类信息

![alt](Snipaste_2018-10-13_23-14-58.png)

虽然对象实例有多个, 但是方法定义只有一份, 存储于 method area


用于存储已被虚拟机加载的`类信息`(类名, 类修饰符), `静态变量` (所以又叫 "静态存储区"), `final常量`, `方法定义`, `field` ; 当通过 class 对象的 getName, isInstaceOf 方法获取信息时, 数据来自此区域

Sun jdk 中对应 `持久代 (Permanent Genaration)`, 默认最小值为16MB，最大值为64MB，可以通过-XX:PermSize 和 -XX:MaxPermSize 参数限制方法区的大小, 容量不够时抛出 OutOfMemory错误

相对而言，垃圾收集行为在这个区域是比较少出现的, 但并非数据进入方法区后就“永久存在”了。

运行时常量池 (runtime constant pool) - JDK1.7及之后版本的 JVM 已经将运行时常量池从方法区中移了出来，在 Java 堆（Heap）中开辟了一块区域存放运行时常量池。


## Heap


唯一目的就是存放`对象实例`及`数组`; 也就是 通过 "new" 创建的对象

是所有区域中最大的一块, 垃圾回收主要是回收这一块 (所以又叫 "GC堆")

为了便于垃圾回收，Java堆空间有更细致的划分: 见 gc 一节 (`进一步细分的目的: 加快内存回收分配的速度`)

* New Generation (新生代) - 一般新建对象都存放在这里;
    
* Old Generation 或叫作 Tenured(年老) Generation， - 存放经过多个 GC 仍然存活的 对象 (如 缓存对象), 新建的对象也可能直接存在这里 (通过 -XX:PretenureSizeThreshold=1024, 单位字节, 默认0, 来设置当前对象超过多少size, 就直接分配到 old generation)

可以通过用JVM的命令行选项 -Xms, -Xmx, -Xmn来调整Java堆空间的大小。不要忘了在大小后面加上”M”或者”G”来表示单位。

`-Xmx256M` 最大堆内存 为 256M. 默认为物理内存的1/4但小于1G. 默认当空余堆内存小于40%时，JVM会增大Heap到-Xmx指定的大小，可通过 `-XX:MinHeapFreeRation=`来指定这个比列

`-Xms256M` 最小堆为 256M (为避免在运行时频繁调整Heap的大小，通常-Xms与-Xmx的值设成一样), 如果不设置, 默认为操作系统物理内存的1/64但小于1G. 当空余堆内存大于70%时，JVM会减小heap的大小到-Xms指定的大小，可通过`XX:MaxHeapFreeRation=`来指定这个比列

`-Xmn` 指定默认大小


可以用JConsole或者 Runtime.maxMemory(), Runtime.totalMemory(), Runtime.freeMemory()来实时查看Java中堆内存的大小

当遇到java.lang.outOfMemoryError时，有时候仅仅增加堆空间就可以了，但如果经常出现的话，就要看看Java程序中是不是存在内存泄露了;

## JVM Stack

JVM stack 记录了线程的 "`方法调用`" and `局部变量`; 

java虚拟机栈是由一个个栈帧组成，而每个栈帧中都拥有：`局部变量表`、`操作数栈`、`动态链接`、`方法出口信息`。(每个方法被调用到执行完的过程，就对应着一个栈帧在jvm stack中从入栈到出栈的过程。) 局部变量表存放了编译器可知的各种基本数据类型(boolean、byte、char、short、int、float、long、double)、对象引用(引用指针，并非对象本身)

Java 虚拟机栈会出现两种异常：StackOverFlowError 和 OutOfMemoryError。

* StackOverFlowError： 若Java虚拟机栈的内存大小不允许动态扩展，那么当线程请求栈的深度超过当前Java虚拟机栈的最大深度的时候，就抛出StackOverFlowError异常 -------- "栈溢出", 可通过 -Xss 修改大小 (例如 -Xss1K, 设置大小为 1K)

* OutOfMemoryError： 若 Java 虚拟机栈的内存大小允许动态扩展，且当线程请求栈时内存用完了，无法再动态扩展了，此时抛出OutOfMemoryError异常。-------- "内存用光了"

`-Xss ` 控制栈容量

## Native Method Stack

` 在 HotSpot 虚拟机(也就是 sun jdk实现)中和 Java 虚拟机栈合二为一`。性质类似

用于支持 native 方法的执行, 存储每个 native 方法的调用状态;

## PC Register(程序计数器)

是最小的一块内存区域，它的作用是当前线程所执行的字节码的行号指示器; 

字节码解释器工作时通过改变这个计数器的值来选取下一条需要执行的字节码指令，分支、循环、跳转、异常处理、线程恢复等功能都需要依赖这个计数器来完。

每个 thread 都有一个独立的 "pc-register" ---------- 这样才能保证 线程切换后能找到正确的执行位置

# gc 垃圾回收

gc 主要发生在堆内存。所以对内存又称为 gc 堆

## 垃圾收集器

- serial 收集器

    用于新生代, 单线程, 有 stop the world

    简单高效, 用于 client 模式下的 jvm  (也就是桌面应用, 停顿时间 100 ms内, 完全可以接受) 的新生代收集器

- ParNew 收集器

    新生代, 多线程 (就是 Serial 的多线程版本 ), 有 stop the world

    只有它能够配合老年代的 CMS (concurrent mark sweep)收集器

    用于 Server 模式运行的jvm的新生代收集器

- Parallel Scavenge 收集器

    新生代, 多线程

    吞吐量可控, 可以通过jvm参数设置 (吞吐量 = cpu运行代码的时间 / (cpu运行代码时间 + cpu 运行垃圾回收时间))

    自适应调节 , 即可以指定一个 jvm 参数, 由jvm自动设置 新生代大小, Eden和Survivor比例, 晋升老年代年龄阈值

- Serial Old 收集器

    单线程, 老年代 (Serial 收集器的老年代版本), 有stop the world

    用在 client 模式 下的 jvm

- Parallel Old 收集器

    老年代 (Parallel Scavenge 的老年代版本), 多线程

    吞吐量可控

    jvm 自适应调节

- CMS (concurrent mark sweep) 收集器

    多线程, 老年代 (标记-清除算法, 有内存碎片, 不利于大Object的内存分配)

    低停顿 (尽量缩短了 stop the world 停顿时间)

    更耗cpu资源 (因为并发收集嘛)

    无法处理浮动垃圾 (即并发清理阶段, 用户线程产生的垃圾, 只能下次 GC 清除)

    用于 网站, b/s 架构系统, 注重服务的响应速度

- G1 收集器

    多线程, 统领 新生代, 老年代 (通过 Region)

    标记-整理, and 复制

    可预测的停顿

    用于 server 模式的jvm


## 如何判断对象是否应该回收

### 引用计数法 reference counting

就是给 object 加上一个计数器, 记录 object 有多少个引用, 为 0 时证明没有引用, 可以回收

无法解决对象间循环引用的问题

### 可达性分析算法 reachability analysis

就是看 gc roots 到 object 是否有 reference chain, 如果没有, 证明可以回收

哪些对象可作为 GC roots Object?

- jvm stack 中的 对象引用
- method area 中类静态属性引用的Object 比如 类变量
- method area 中常量引用的Object

不可达对象一定会被回收吗?

- object 只有经历 `二次标记` 才会被回收

    - 第一次标记  - 经过 reachability analysis 当前Object没有发现到 gc roots 的 reference chain, 不一定就被回收, 这时会第一次标记

    - 第二次标记 - 如果 当前 Object 覆盖了 finalize(), 将当前Object 和 reference chain 上任意Object建立连接(如 将this赋值给静态成员变量) 就可以逃避回收, 如果没有建立链接, 会被第二次标记, 真正回收

- obj.finalize() 对象被回收前会执行这个方法

### reference 分类

- strong reference
- soft reference
- weak reference
- phantom reference

## 垃圾回收算法

### 标记-清除 算法

首先可达性分析, 两次标记过程后, 清除被标记的内存块

会造成内存碎片 (因为回收的对象不是内存连续的), 给新Object分配内存时找不到足够的连续空间, 导致提前触发GC

### 复制算法

将堆空间一分为二, 内存分配只在某一块空间, 当空间不够时候, 将仍然会存活的对象复制到空闲的另一块空间, 然后清除满了的空间

- 解决了内存碎片化的问题
- 对于大对象, 复制是个耗时耗力的操作
- 会浪费一半的空间

### 标记-整理 算法

标记待回收的对象, 将标记的Object向内存一端移动, 然后清理掉标记的Object所在的内存

- 没有复制操作, 省时省力
- 没有空间浪费

### 分代收集算法

为了提高回收效率, 根据Object存活周期不同, 存到不同的内存块, 这些内存块采用不同的收集算法:

- new generation (新生代。) - 采用复制算法, 分为三块, eden: survivor1: survivor2 = 8:1:1, 始终保证有一个 survivor 空闲, 也就是说会浪费 10% 空间

    新创建的 object 放在 Eden, 空间不够发起 minor gc (大量对象分配在这里, 所以 minor gc 发生的很频繁, 但是很快)

    对象经历 15 次 minor gc 后, 会进入到 老年代, 这个阈值通过 `-XX:MaxTenuringThreshold` 设定

    大对象 (长字符串, 数组 这样的需要连续空间的对象) 直接分配进入老年代, 这个尺寸阈值 通过 `-XX:PretenureSizeThreshold` 设定. (为什么要设置这条策略? 一是避免提前触发 major gc, 明明Eden还有空间, 但是容不下这个 大对象了. 二是新生代 gc 采用复制算法, 而复制大对象是非常耗时的)

    内存分配担保机制: minor gc 时候, 若 survivor2 空间也不够, 则存活对象直接进入 old generation (如果 old generation 也满了, 则只能触发 full/major gc 了)

- old generation (老年代。) - 标记-整理 算法

    这里的对象生命周期长, 如果继续使用 复制算法, 需要大块的分配担保内存, 不划算

- 永久代

minor gc : 新生代发起的 gc 叫做 minor gc.  eden 和 survivor1 中存活的对象被复制进入空闲的 survivor2，清空eden 和 survivor1. 

major gc: 发生在 老年代 的 gc . 非常慢

为什么需要 survivor？加入 survivor，存活对象有了一个缓冲区域， 不会直接进入老年代，减少了 full gc
      
为什么需要2块 survivor？类似 复制收集算法， 是为了利用复制操作, 避免内存碎片化的问题

什么时候出发 minor gc: 当年轻代满时就会触发Minor GC，这里的年轻代满指的是Eden代满，Survivor满不会引发GC

什么时候出发 full gc: 

- 堆内存的老年代满了
- 方法区满了

内存分配担保机制详细过程:

1. jvm 先检查 老年代空闲连续空间是否大于 新生代所有Object总大小, 若 TRUE, Minor GC 可以确保安全, 若 FALSE, 下一步

1. jvm 查看是否配置了 `-XX:-HandlePromotionFailure` (一般会打开, 避免 Full GC 频繁), 若 FALSE, 不允许冒担保失败的风险, 老年代直接进行 Full GC, 若 TRUE, 那么允许冒担保失败的风险, 下一步

1. 继续检查老年代连续空闲空间是否大于 历史上晋升到老年代的Object的平均大小, 若TRUE, 尝试进行 Minor GC (冒着风险), 若 FALSE, 进行 Full GC

# Java的代码执行机制

## 类编译

jvm 规定了 class 规范， 但是怎么从 java 文件到 class 文件并没有规定， 厂商有很多实现， javac， ecj （eclipse compiler for Java）， jikes

`javac -g Foo.java` 编译 （-g 生成 所有调试信息）， 然后 通过 `javap -c -s -l -verbose Foo` 查看 class 文件

```java
/**
 * Foo
 */
public class Foo {

    private static final int MAX_COUNT = 100;
    private static int count = 0;

    public int bar() throws Exception {
        if (++count >= MAX_COUNT) {
            count = 0;
            throw new Exception("count overflow.");
        }
        return count;
    }
}

```

```
Classfile /C:/Users/xiaoyu/Desktop/Foo.class
  Last modified 2018-12-28; size 607 bytes
  MD5 checksum f76c99d05a7c762ddacdbd5c4c2d1650
  Compiled from "Foo.java"
public class Foo
  minor version: 0
  major version: 52 // -- jdk 版本号, 50 -> jdk 6, 51 -> jdk7, 52 -> jdk8
  flags: ACC_PUBLIC, ACC_SUPER
Constant pool: //-- 常量池, 存放 field name, method name, type name, 常量值
   #1 = Methodref          #7.#27         // java/lang/Object."<init>":()V
   #2 = Fieldref           #3.#28         // Foo.count:I
   #3 = Class              #29            // Foo
   #4 = Class              #30            // java/lang/Exception
   #5 = String             #31            // count overflow.
   #6 = Methodref          #4.#32         // java/lang/Exception."<init>":(Ljava/lang/String;)V
   #7 = Class              #33            // java/lang/Object
   #8 = Utf8               MAX_COUNT
   #9 = Utf8               I
  #10 = Utf8               ConstantValue
  #11 = Integer            100
  #12 = Utf8               count
  #13 = Utf8               <init>
  #14 = Utf8               ()V
  #15 = Utf8               Code
  #16 = Utf8               LineNumberTable
  #17 = Utf8               LocalVariableTable
  #18 = Utf8               this
  #19 = Utf8               LFoo;
  #20 = Utf8               bar
  #21 = Utf8               ()I
  #22 = Utf8               StackMapTable
  #23 = Utf8               Exceptions
  #24 = Utf8               <clinit>
  #25 = Utf8               SourceFile
  #26 = Utf8               Foo.java
  #27 = NameAndType        #13:#14        // "<init>":()V
  #28 = NameAndType        #12:#9         // count:I
  #29 = Utf8               Foo
  #30 = Utf8               java/lang/Exception
  #31 = Utf8               count overflow.
  #32 = NameAndType        #13:#34        // "<init>":(Ljava/lang/String;)V
  #33 = Utf8               java/lang/Object
  #34 = Utf8               (Ljava/lang/String;)V
{
  public Foo(); // -- 生成的默认构造器方法
    descriptor: ()V
    flags: ACC_PUBLIC
    Code:
      stack=1, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V //-- jvm提供的四个方法指令之一
         4: return
      LineNumberTable:
        line 4: 0
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       5     0  this   LFoo;

  public int bar() throws java.lang.Exception; //-- bar 方法的元数据信息
    descriptor: ()I
    flags: ACC_PUBLIC
    Code:
      stack=3, locals=1, args_size=1
         0: getstatic     #2                  // Field count:I
         3: iconst_1
         4: iadd
         5: dup
         6: putstatic     #2                  // Field count:I
         9: bipush        100
        11: if_icmplt     28
        14: iconst_0
        15: putstatic     #2                  // Field count:I
        18: new           #4                  // class java/lang/Exception
        21: dup
        22: ldc           #5                  // String count overflow.
        24: invokespecial #6                  // Method java/lang/Exception."<init>":(Ljava/lang/String;)V
        27: athrow
        28: getstatic     #2                  // Field count:I
        31: ireturn
      LineNumberTable: // -- 对应了源码的行号, 查找问题很方便
        line 10: 0
        line 11: 14
        line 12: 18
        line 14: 28
      LocalVariableTable: // -- 局部变量信息, 如果没有这行信息, 我们无法知道局部变量名称
        Start  Length  Slot  Name   Signature
            0      32     0  this   LFoo;
      StackMapTable: number_of_entries = 1 //--记录有分支的情况
        frame_type = 28 /* same */
    Exceptions: //-- 异常处理器表
      throws java.lang.Exception

  static {};
    descriptor: ()V
    flags: ACC_STATIC
    Code:
      stack=1, locals=0, args_size=0
        //-- 方法对应的字节码
         0: iconst_0
         1: putstatic     #2                  // Field count:I
         4: return
      LineNumberTable:
        line 7: 0
}
SourceFile: "Foo.java"

```


## 类加载

将 class 文件 加载到 jvm , 并形成 Class 对象 的过程;

- 装载
- 链接 (此时已经加载完毕, 形成 Class 对象)
- 初始化 (使用对象前必须初始化, 为静态变量赋值)

### classloader类加载器

check java-note#classloader类加载器

### 装载(load)

找到 class 文件 并加载到 jvm, jvm 通过  `ClassLoader 实例` 和 类的` full qualified name` 加载, 同样也通过这两个元素标识被加载了的 类

```java
@Test
public void testClassLoad() {
    // 对于 数组类型的类，类名 “[” + "基本类型or L" + 引用类型类名
    byte[] bytes = new byte[1024];
    Object[] objArr = new Object[10];
    System.out.println(">>> bytes name: " + bytes.toString());
    System.out.println(">>> objArr name: " + objArr.toString());
    // result:
    //>>> bytes name: [B@11028347
    //>>> objArr name: [Ljava.lang.Object;@14899482
}

```

### 链接(Link)

也就是常见class对象；具体包括：

- 对class文件的格式校验, 如果class格式不符合, 抛出 error, 校验中如果碰到其他类, 也加载, 若加载失败, 抛出 NoClassdeffoundError

- 校验完后, 初始化 静态变量

- 最后校验 属性, 方法. 确保属性,方法存在. 可能抛出 NosuchmethodError, NosuchfieldError

### 初始化(Initialize)

执行类中的 `静态初始化代码`, `构造器代码`, `静态属性初始化`


## 类执行

### 字节码解释执行and编译执行并存如何分界

根据方法上的2个计数器是否超过阈值决定是否由 解释执行升级为编译执行

- 调用计数器 - 方法被调用的次数
- 回边计数器 - 方法内部循环代码的循环次数

### 字节码解释执行

#### 方法相关的jvm指令

class 二进制文件中, jvm提供一套指令来执行不同的方法调用

- invokestatic - 调用 static 方法
- invokevirtual  - 调用对象实例的方法
- invokeinterface - 调用接口的方法
- invokespecial - 调用 private 的方法 和 调用 编译后生成的 <init> 方法 (即对象实例化时的初始化方法)

通过 javap -c xxx 可以以文本形式查看 class二进制文件


jvm 以 `基于 栈的结构` 来执行 字节码, 好处时体积小, 代码紧凑

一个 thread 在创建后会产生 

- `程序计数器` (pc register) - 存放下一条待执行的指令在方法内的偏移量
- `栈` - 存放 "栈帧" (stack frame), 每个方法每次调用都会产生栈帧

示意图:

![alt](Snipaste_2018-12-29_17-39-30.png)

局部变量区  : 存放局部变量和参数

操作数栈: 存放方法执行过程中产生的中间结果


#### jvm指令怎么解释执行

冯-诺伊曼 体系 中的 fdx 循环, 思路如下:

```java
// 实现 fdx 循环有多种方式, switch-threading, token-threading, inline-threading...
// switch-threading 实现循环
while(true) {
    // // 获取下一条指令, 分派, 执行
    int code = fetchNextCode(); 
    switch (code) {
        case IADD:
            // do int add
        case ...:
            // do sth
    }
}

// token-threading 实现循环, sun jdk 默认采用这种循环方式
// 较之 前一种方式, 冗余了 fetch 和 dispatch, 去除了 switch; 所以性能好些, 但是内存占用更多
while(true) {
    IADD: {
        // do int add
        fetchNextCode();
        dispatch();
    }
    ICONST_0: {
        push(0);
        fetchNextCode();
        dispatch();
    }

}
```

### 字节码编译执行

字节码解释执行的效率不够好, 新的执行方式出现: 将 字节码 编译为 机器码, 在运行时进行 ------- `JIT编译器` (just-in-time compiler 即时编译器)

sun jdk 对字节码中执行频率较高的部分代码编译为机器码, 频率不够高的部分二字节码仍旧解释执行 -------- 因此 sun jdk 又称为 Hotspot VM, 取 "热点"之意

sun jdk 提供两种编译模式 : client compiler, server compiler

sun jdk 根据 机器配置决定使用什么模式, 也可以强制指定以哪种模式启动jvm

#### client compiler

又名 `C1 编译`, 轻量级, 占用内存少, 适合桌面交互式应用

主要优化手段:

- 方法内联 - 当前方法内部调用另一个方法, JIT 编译后, 另外的方法代码直接被编入当前方法中
- 去除虚拟化 - class字节码被 load 后, 会分析其类层次结构, 如果一个接口只有一个实现类, 而这个接口在某个地方被使用了, JIT 会在 调用处进行 `方法内联`优化
- 冗余削除, 在编译时, 根据代码运行情况进行代码的削除, 如果某个条件分支的代码没有执行到, C1编译 会在编译后直接去掉这段不会被执行到的代码

```java

// 优化 - 方法内联

public void bar() {
    foo()
}
private void foo() {
    // foo method
    System.out.println("foo method");
}

// 优化后:
public void bar() {
    // foo method
    System.out.println("foo method");
}

////////////////////////////////////////////

// 优化 - 去除虚拟化

public interface IFoo {
    void bar();
}
public class Foo implements IFoo {// IFoo 只有这一个实现
 public void bar() {
     // bar method
 }
}

public class Demo {
    public void exec(IFoo foo) {
        foo.bar();
    }
}

// 优化后:
// 当 jvm 发现 只有 Foo 实现了 IFoo, 会如下优化: 直接把 bar() 的实现代码移到 exec() 中去
public void exec(IFoo foo) {
    // bar method
}

////////////////////////////////////////////////////

// 优化 - 冗余削除
private static final Log log = ....
private static final boolean isDebug = log.isDebugEnable();

public void exec() {
    if (isDebug) {
        log.debug("...");
    }

    // do sth
}

// 优化后:
// isDebug 为 false 才会优化
public void exec() {
    // do sth
}

// 这类优化也是为什么有的代码编写规范规定: 不要直接使用 log.debug(), 而要先判断 debug 是否开启; 
// 这样可以提高代码性能


```

#### server compiler

又称为 `C2编译`, 重量级, 内存占用较多, 适合服务器端应用

和 c1编译 主要的不同在于: 寄存器分配策略and优化范围不同

`逃逸分析`是C2编译优化的基础, 基于 逃逸分析, 有这些优化手段: (逃逸分析: http://www.importnew.com/23150.html) ------ 逃逸分析的基本行为就是分析对象动态作用域：当一个对象在方法中被定义后，它可能被外部方法所引用，例如作为调用参数传递到其他地方中，称为方法逃逸

- 标量替换 (聚合量替换为标量) - 将 `对象` 代码替换为 `属性` 代码的形式; 好处: 由于逃逸变量被其内部属性替换, 可节省内存; 而且代码执行时无需去找逃逸对象的引用, 会更快一点
- 栈上分配 - 如果 变量 没有逃逸, C2 会直接在 栈上创建这个变量代表的对象 好处: 访问更加快速; 随着方法执行结束, 对象也自动销毁了, 减轻了垃圾回收的压力
- 同步削除 (这里的 "同步" 是名词) - 如果发现同步的对象没有逃逸, 也就没有同步的必要了, C2编译时会直接去掉同步

```java

// 标量替换

Point point = new Point(1, 2);
Sysout.out.println("point的x坐标为: " + point.x + ", point 的y坐标为: " + point.y);

// 优化后: (point 在代码中没有被直接使用, 只是使用了内部的属性)
int x = 1;
int y = 2;
Sysout.out.println("point的x坐标为: " + x + ", point 的y坐标为: " + y);

// 同步削除
// 就是 去掉 代码同步 synchronized


```

### 反射执行

典型如 mvc 框架

反射代码中， `getMethod()` 比较消耗性能(背后需要进行方法权限校验, 所有方法的扫描, Method 对象的复制), 所以 得到的 Method 对象最好缓存



# java中的关键字和内存的关系

## final

https://blog.csdn.net/Ditto_zhou/article/details/78738197

final如果用来表示数据变量的话，通常是指该变量被赋值一次，就不能改变；如果这个final变量是复杂类型， 变量内部数据仍然可变

写final变量时的重排序特性：如果一个类中定义了final变量， 那么针对这个变量的write操作`不会被重排序到构造函数之外` ------相反的, 普通变量可能被重排序到构造函数之外

结论就是: 在一个对象的引用对一个线程可见前，能保证final变量被正确初始化，而普通域不具有这个特性，因为普通域的写入可能会重排序到构造函数外.也就是在多线程环境下，拿到一个对象的引用后，可能会出现它的普通属性的变量还没有被正确初始化的情况.

读取final变量时的重排序: 读取一个包含final变量的对象的引用和随后读取这个final变量，不能重排序

另外, 如果匿名内部类访问了外部数据:

- 如果访问的是局部变量(方法内部的变量), 数据存在于线程栈上, 变量无法进行共享, 匿名内部类无法获得这个变量, 只能通过值传递(拷贝)的方式传递到匿名内部类中 ---- 匿名内部类和外部类中的数据无法自动同步；类似js中的闭包， 但是js中的闭包可以自动同步内外变量；
- 如果这个外部数据是成员变量, 对应的在虚拟机里是在堆的位置, 变量可以共享, 因此在创建匿名内部类的对象时无需拷贝, 直接通过 this.xxx 即可获取；

因此可以得出结论：匿名内部类来自外部闭包环境的自由变量必须是final的，除非自由变量来自类的成员变量。

## volatile

一条thread每次对变量进行修改都会立即刷新到堆内存中被其他thread看到;

## synchronized

一条thread进入synchronized代码块, 会获取目标对象的锁

## static



# java的内存泄露

## 会造成哪些溢出

内存泄露会造成 "溢出", 堆溢出, 栈溢出, 方法区溢出

堆溢出: 典型如不断往一个集合中添加对象, 直到堆溢出, 但是由于 GC,现在不成问题了

方法区溢出: 运行时动态生成大量类 (反射, cglib), 方法区不够用了

栈溢出: 方法递归调用,没有退出条件; or 内存对象明明已经不需要的时候，还仍然保留着这块内存和它的访问方式（引用）, 造成无法 GC, 比如弹栈时没有将栈中的元素赋值为null;

```java
public class Heap
{
    public static void main(String[] args)
    {
        ArrayList list=new ArrayList();
        while(true)
        {
            list.add(new Heap());
        }
    }
}

////////////////////////

public class Stack
{
    public static void main(String[] args)//java.lang.StackOverflowError
    {
        new Stack().test();
    }
    public void test()
    {
        test();
    }
}

```

## Java内存泄露引起原因

静态集合类引起内存泄露 (静态变量的生命周期和应用程序一致，他们所引用的所有的对象Object也不能被释放)

```java
Static Vector v = new Vector(10);
for (int i = 1; i<100; i++)
{
Object o = new Object();
v.add(o);
o = null;
}

```

当集合里面的对象属性被修改后，再调用remove（）方法时不起作用

```java
public static void main(String[] args)
{
Set<Person> set = new HashSet<Person>();
Person p1 = new Person("唐僧","pwd1",25);
Person p2 = new Person("孙悟空","pwd2",26);
Person p3 = new Person("猪八戒","pwd3",27);
set.add(p1);
set.add(p2);
set.add(p3);
System.out.println("总共有:"+set.size()+" 个元素!"); //结果：总共有:3 个元素!
p3.setAge(2); //修改p3的年龄,此时p3元素对应的hashcode值发生改变

set.remove(p3); //此时remove不掉，造成内存泄漏

set.add(p3); //重新添加，居然添加成功
System.out.println("总共有:"+set.size()+" 个元素!"); //结果：总共有:4 个元素!
for (Person person : set)
{
System.out.println(person);
}
}

```

各种连接: 比如数据库连接（dataSourse.getConnection()），网络连接(socket)和io连接，除非其显式的调用了其close（）方法将其连接关闭，否则是不会自动被GC 回收的。对于Resultset 和Statement 对象可以不进行显式回收，但Connection 一定要显式回收，因为Connection 在任何时候都无法自动回收，而Connection一旦回收，Resultset 和Statement 对象就会立即为NULL。但是如果使用连接池，情况就不一样了，除了要显式地关闭连接，还必须显式地关闭Resultset Statement 对象

单例模式: 果单例对象持有外部对象的引用，那么这个外部对象将不能被jvm正常回收，导致内存泄露


# jvm 中的 object 

HotSpot 虚拟机在 Java 堆中对象创建、布局和访问的全过程。reference: https://juejin.im/post/5b7d69e4e51d4538ca5730cb

## 对象的创建过程(内存怎么分配)

- 类加载检查 - 检查类是否已经加载过, 如果没有需要先加载类
- 分配内存 - 为新建对象分配内存 (即将一块确定大小的内存块从 Heap 中划分出来, 对象耗费内存大小在类加载后可以得知); 分配方式: "指针碰撞", "空闲散列" . 选择那种分配方式由` Java 堆是否规整`(是否有内存碎片)决定，而Java堆是否规整又由`所采用的垃圾收集器是否带有压缩整理功能`决定。
- 初始化0值 - 给对象实例的属性赋零值
- 设置对象头 - 虚拟机要对对象进行必要的设置, 保证对象和类能够联系起来，例如这个`对象是那个类的实例`、`如何才能找到类的元数据信息`、对象的哈希吗、对象的 GC 分代年龄等信息。 这些信息存放在对象头中。 另外，根据虚拟机当前运行状态的不同，如是否启用偏向锁等，对象头会有不同的设置方式。
- 执行官init方法 - 把对象按照程序员的意愿进行初始化

给新建对象分配内存的两种方式:

- 指针碰撞 - 用过的内存和空白内存分开, 中间有分界指针, 分配时, 指针向空白内存方向移动"新建对象"大小的位置即可
- 空闲列表 - 虚拟机维护一个列表, 记录哪些空白内存, 分配时, 从中找到一块合适的内存分给对象实例, 同时更新这个列表 

![alt](Snipaste_2019-01-02_21-05-54.png)

`内存分配怎么防止并发问题`: TLAB (thread local allocation buffer) -> 为每一个线程预先在new generation 区域的 Eden区分配的一块儿内存 (可通过 jvm 参数指定)，JVM在给线程中的对象分配内存时，首先在TLAB分配，当对象大于TLAB中的剩余内存或TLAB的内存已用尽时，再在堆上分配, 采用CAS进行内存分配 ( CAS+失败重试： CAS 是乐观锁的一种实现方式。所谓乐观锁就是，每次不加锁而是假设没有冲突而去完成某项操作，如果因为冲突失败就重试，直到成功为止。虚拟机采用 CAS 配上失败重试的方式保证更新操作的原子性。)


## 对象的访问

### 对象的2种访问方式

目前主流的访问方式有①使用句柄和②直接指针两种：(Java程序通过栈上的 reference 数据来操作堆上的具体对象)

1. 句柄： 如果使用句柄的话，那么`Java堆中将会划分出一块内存来作为句柄池`，reference 中存储的就是对象的句柄地址，而句柄中包含了`对象实例数据`(Heap)与`类型数据`(Method Area)各自的具体地址信息; 好处:  reference 中存储的是稳定的句柄地址，在对象被移动时只会改变句柄中的实例数据指针，而 reference 本身不需要修改

1. 直接指针： 如果使用直接指针访问，那么 Java 堆对像的布局中就必须考虑如何防止访问类型数据的相关信息，reference 中存储的直接就是对象的地址。好处: 速度快，它节省了一次指针定位的时间开销

《深入理解java虚拟机》中的示意图:

![alt](Snipaste_2018-10-13_17-13-07.png)![alt](Snipaste_2018-10-13_17-14-24.png)

### thread具体怎么访问对象中的值呢以及volatile关键字

http://www.cnblogs.com/aigongsi/archive/2012/04/01/2429166.html

jvm 有一个内存区域是jvm虚拟机栈， 包含一个个的 线程栈（thread stack），每个 thread stack 保存了保存了线程运行时候变量值信息

当线程访问某一个对象时候值的时候，首先通过对象的引用找到对应在堆内存的变量的值，然后把堆内存变量的具体值load到线程本地内存中，建立一个变量副本，之后线程就不再和对象在堆内存变量值有任何关系，而是直接修改副本变量的值，在修改完之后的某一个时刻（线程退出之前），自动把线程变量副本的值回写到对象在堆中变量

![alt](Snipaste_2019-03-21_17-35-58.png)

- read and load 从主存复制变量到当前工作内存

- use and assign  执行代码，改变共享变量值 （可反复出现, `如果变量加上volatile， 每次线程更新变量后， 会立即将变动后的结果更新到堆内存中的对象中`）

- store and write 用工作内存数据刷新主存相关内容

这一系列操作并非原子性的， 如果一个thread1 读取某个变量后， cpu 被另一个 thread2抢占， 对该变量写入新的值， thread1由于已经加载该变量， 并不会重新加载， 读取了脏数据；

对于volatile修饰的变量，jvm虚拟机只是保证从主内存加载到线程工作内存的值是最新的， 无法保证线程安全， 比如：

例如假如线程1，线程2 在进行read,load 操作中，发现主内存中count的值都是5，那么都会加载这个`最新的值`

在线程1堆count进行修改之后，会write到主内存中，主内存中的count变量就会变为6

线程2由于已经进行read,load操作，在进行运算之后，也会更新主内存count的变量值为6, 产生了线程安全问题；

## 对象回收(内存怎么释放)

GC 有2种实现方式:

- 引用计数收集器 - 每次对象赋值时要进行 "引用计数"的增减, 有一定消耗,, 有时候一个对象被多个其他对象引用 Java中有大量这样的引用, 累计起来消耗不小, sun jdk 未采用这种方式
- 跟踪收集器 - 可能会使得应用暂停, 有三种实现算法 ...

# 有哪些虚拟机

Hotspot - 目前使用最广泛的

ZGC - 支持超大堆的同时保证STW很短

Graal - 一个特制版的的JavaVM。用Java写的JavaVM。能让不同语言跑在同一个VM上，还能互通, 号称全栈vm, 微服务框架Quarkus 给予他


# java线上问题诊断排查

https://blog.csdn.net/GitChat/article/details/79019454
http://ifeve.com/find-bug-online/
https://mp.weixin.qq.com/mp/profile_ext?action=home&__biz=MzA4NTg4Njk0Mw==&scene=161#wechat_redirect

一般流程如下:

## 日志检索


```sh

# 检索 exception 关键字或 error 关键字
# -E 使用正则表达式, 这里可选
# or 逻辑
grep -E "exception|error" app.log
# 等价
egrep "xx" xxx.log
# and 逻辑
grep -E ‘pattern1.*pattern2’ filename
grep -E ‘pattern1.*pattern2|pattern2.*pattern1’ filename
# not 逻辑
grep -v ‘pattern1’ filename


# 显示关键字上下 10 行日志
grep -C 10 exception app.log

# 检索 2020-05-05 19:23~25  分钟段日志
grep '2020-05-05 19:2[3-5]' app.log
sed -n '/2020-05-05 19:23/,/2020-05-05 19:25/p' app.log
# 检索 2020-05-05 19:23:10~15 秒段日志
grep '2020-05-05 19:23:[10-15]' app.log
sed -n '/2020-05-05 19:23:10/,/2020-05-05 19:23:15/p' app.log

# 查询 /data 目录下大于 500m 的文件
find /data -type f -size +500M
```

## 查看数据库连接

show processlist, 查看当前数据库的连接情况。确实因为慢查询造成。须要手动kill

## 查看CPU 负载

### CPU User高 and  CPU Load 高

(User：CPU 在用户态空间（用户进程）的运行时间比例)

#### 代码中存在非常消耗 CPU 的操作

```sh
#找出对应的 Java 进程 pid

ps -ef | grep java 

#找出 Java 进程中最消耗 CPU 的线程

top -H -p <pid>

jstack 获取 Java 的线程堆栈

jstack ...

# 将找出的线程 id 转换为 16 进制
# 根据 16 进制的 id 从线程堆栈中找到相关的堆栈信息

printf "%x\n" pid

```

#### 频繁gc

```sh
jstat -gcutil pid interval(ms)

```

### CPU System 高 and CPU Load 高

System：CPU 在内核态空间的运行时间比例。例如内存分配、IO 读写、线程创建和切换等

常见原因：

- 并发锁竞争严重
- 线程频繁切换

排查方法:

- jstack 打印线程堆栈，整体统计线程状态，如 WAITING、TIMED_WAITING、BLOCKED

- pidstat -w 可以查看 CPU 上下文切换的状态。

    cswch/s 每秒自愿上下文切换次数：进程无法获取资源，如内存、IO 等；
    
    nvcswch/s 每秒非自愿上下文切换次数：时间片耗尽系统强制调度，如进程频繁争抢 CPU。

如何优化:

- 减小锁粒度
- 使用无锁数据结构
- 检查死锁


### CPU Wait高 and CPU Load高 and CPU利用率低

说明CPU 等待磁盘写入的时间长。当 CPU 发起 IO 读写操作后，需要等待磁盘数据加载至内存

可能原因是:

- IO 操作频繁
- 资源未及时释放造成泄漏


排查方法:

```sh
# 查看设备的 IO 状态

iostat -x 2

参数含义：

rrqm/s : 每秒合并读操作的次数
wrqm/s: 每秒合并写操作的次数
r/s ：每秒读操作的次数
w/s : 每秒写操作的次数
rKB/s :每秒读取字节数
wKB/s: 每秒写入字节数
avgrq-sz：平均每次 IO 的数据大小，以扇区（512字节）为单位
avgqu-sz：平均 IO 请求队列长度
await：平均每个IO所需要的时间，包括在队列等待的时间和请求处理的时间。
r_wait：每个读操作平均所需要的时间，包括硬盘设备读操作的时间 + 内核队列中的时间。
w_wait: 每个写操平均所需要的时间，包括硬盘设备写操作的时间 + 队列中等待的时间。
%util: 每秒内用于 I/O 操作的时间占比

# 查找引起 iowait 高的进程
iotop

iostat -m 1 10
-m：某些使用block为单位的列强制使用MB为单位
1 10：数据显示每隔1秒刷新一次。共显示10次

# 查看引起 iowait 高的具体文件

lsof -p <pid>
```

优化方法:

- 有效控制资源数量，例如使用线程池等。
- 了解磁盘特性是必要的，一般随机写转顺序写，同步写转异步写，IO 合并都可以得到较好的改善。
- 压缩 & Dirty Page 优化。Linux 操作系统中，当 Dirty Page 的大小达到总物理内存大小 10%，操作系统会进行刷盘但不阻塞系统调用的写线程，若达到物理内存大小的 20%，写线程会被阻塞。通过合适的压缩算法减少落盘数据的大小通常效果显著。
- 预读取和读缓存。
- Zero Copy。
- MMap 内存映射。


## 查内存

内存占用高, 可能是内存泄漏

通过 内存映像分析工具分析 dump 文件,  看看是 memory leak 还是 memory overflow

- memory leak: 证明有无法回收的Object, 查看 leak Object 到 gc roots的引用链, 定位leak代码

- memory overflow: 查看 -Xms, -Xmx 是否可以增大, 代码上查看对象是否生命周期过长


```sh
# 内存整体情况

free -m 

free -m -c10 -s1
-m：以MB为单位显示。其它的有-k -g -b
-s: 间隔多少秒持续观察内存使用状况
-c:观察多少次

vmstat 1 10
1表示每隔1s输出一次,10 表示输出10次

# JVM 堆内存占用排行

jmap -histo <pid> | head -n 30

# JVM 内存占用信息

jstat -gccause <pid> 1000 1000
jstat -gcutil <pid>

# dump内存

jmap -F -dump:live,file=/home/admin/heap.bin <pid>

```

## 排查垃圾回收问题


## 网络问题

```
netstat -antp 

-a (all)显示全部选项。默认不显示LISTEN相关
-t (tcp)仅显示tcp相关选项
-u (udp)仅显示udp相关选项
-n 拒绝显示别名，能显示数字的所有转化成数字。
-l 仅列出有在 Listen (监听) 的服服务状态

-p 显示建立相关链接的程序名

```

# jdk 自带命令行工具

```
jps [-l]
- 列出jvm中的进程 (id, 主类)

jstat
- 监视jvm内存变化, gc情况

jinfo
- 查询jvm配置信息

jmap
- 生成Java heap 快照

jhat
- 分析 Java heap 快照
- 可在生产机器上直接访问分析结果html

jstack
- 生成线程快照
- 用于分析线程停顿, 如死锁

可视化工具
Jconsole
- 内存, 线程监控
VirtualVM
- 监控, 性能分析
- 基于插件
```

## java 启动 javac 编译 javap 反编译

1、不带任何可选参数(如果字节码文件就位于当前工作目录，可以不写类的包名)：

实际上等同于 javap -package Person
javap Person

2、反汇编指定目录下的字节码文件：

javap -classpath D:/java/lib cn.softown.Person

3、反汇编字节码文件为 JVM 可以识别、执行的字节码命令：

javap -c Person

## jstack

打印线程快照， 用于分析线程停顿, 如死锁

线程快照是当前 java 虚拟机内每一条线程正在执行的方法堆栈的集合，

生成线程快照的主要目的是定位线程出现长时间停顿的原因, 一般打印三次 dump，看看没有响应的线程到底在后台做什么事情，如线程间死锁、死循环、请求外部资源导致的长时间等待等。

```sh
# 默认输出 控制台
jstack 1102


```

dump 文件分析:

```
# 有一些 jvm 的 daemon thread，如 gc，低内存检测， 不关注

"VM Thread" os_prio=2 tid=0x0000000018167000 nid=0xfccc runnable
"GC task thread#0 (ParallelGC)" os_prio=0 tid=0x000000000352a000 nid=0x121b4 runnable
"GC task thread#1 (ParallelGC)" os_prio=0 tid=0x000000000352c000 nid=0x12168 runnable
"GC task thread#2 (ParallelGC)" os_prio=0 tid=0x000000000352d800 nid=0x121d0 runnable
"GC task thread#3 (ParallelGC)" os_prio=0 tid=0x0000000003530000 nid=0x115d4 runnable
"VM Periodic Task Thread" os_prio=2 tid=0x0000000019621000 nid=0x137b0 waiting on condition


# 关注u用户级别的 thread
- Runnable

    - Wait on condition  线程等待某个条件的发生

        常见的情况是线程在等待网络的读写，比如当网络数据没有准备好读时，线程处于这种等待状态，如果发现有大量的线程都在处在 Wait on condition 可能是一个网络瓶颈的征兆。

        - 一种情况是网络非常忙，几 乎消耗了所有的带宽，仍然有大量数据等待网络读 写

        - 另一种情况也可能是网络空闲，但由于路由等问题，导致包无法正常的到达,

        这时可以 netstat统计单位时间的发送包的数目, 可以观察 cpu的利用率，如果系统态的 CPU时间，相对于用户态的 CPU时间比例较高会是网络瓶颈

        外一种出现 Wait on condition的常见情况是该线程在 sleep，等待 sleep的时间到了时候，将被唤醒

    - Waiting for monitor entry 线程等待获取锁， 以便进入临界区， 一般是使用了 synchronizd

        如果在多线程的程序中，大量使用 synchronized，或者不适当的使用了它，会造成大量线程在临界区的入口等待，造成系统的性能大幅下降。如果在线程 DUMP中发现了这个情况，应该审查源码，改进程序。

    - in Object.wait() 线程释放锁， 等待再次获取锁

        当线程获得了 Monitor，进入了临界区之后，如果发现线程继续运行的条件没有满足，它则调用对象（一般就是被 synchronized 的对象）的 wait() 方法，放弃了 Monitor，进入 “Wait Set”队列。只有当别的线程在该对象上调用了 notify() 或者 notifyAll() ， “ Wait Set”队列中线程才得到机会去竞争


# 处理 http 的 thread， tomcat 开了一个 thread pool， 包括 http-nio-8090-exec-1，2，3.。。10
# 可以看到 thread state 为 waiting，调用栈，当前锁住的资源 0x000000001d7ee000
"http-nio-8090-exec-1" #23 daemon prio=5 os_prio=0 tid=0x000000001bc9d800 nid=0x13af4 waiting on condition [0x000000001d7ee000]
   java.lang.Thread.State: WAITING (parking)
        at sun.misc.Unsafe.park(Native Method)
        - parking to wait for  <0x00000000db266038> (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
        at java.util.concurrent.locks.LockSupport.park(LockSupport.java:175)
        at java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.await(AbstractQueuedSynchronizer.java:2039)
        at java.util.concurrent.LinkedBlockingQueue.take(LinkedBlockingQueue.java:442)
        at org.apache.tomcat.util.threads.TaskQueue.take(TaskQueue.java:107)
        at org.apache.tomcat.util.threads.TaskQueue.take(TaskQueue.java:33)
        at java.util.concurrent.ThreadPoolExecutor.getTask(ThreadPoolExecutor.java:1067)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1127)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:617)
        at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)
        at java.lang.Thread.run(Thread.java:745)


# 这是一个进入 临界区， 又退出临界区的线程
# 比如，在程序中，有多个服务线程，设计成从一个队列里面读取请求数据。这个队列就是 lock以及 waiting on的对象。当队列为空的时候，这些线程都会在这个队列上等待，直到队列有了数据，这些线程被 Notify，当然只有一个线程获得了 lock，继续执行
"Thread-1" prio=10 tid=0x08223250 nid=0xa in Object.wait() [0xef47a000..0xef47aa38]
        at java.lang.Object.wait(Native Method)
        - waiting on <0xef63beb8> (a java.util.ArrayList) # 在 临界区调用 obj.wait()，等待在这个对象上， 释放锁
        at java.lang.Object.wait(Object.java:474)
        at testthread.MyWaitThread.run(MyWaitThread.java:40)
        - locked <0xef63beb8> (a java.util.ArrayList) #  用 synchronized 获得了这个对象的 锁
        at java.lang.Thread.run(Thread.java:595)


# 死锁
"Thread-1" prio=5 tid=0x00acc490 nid=0xe50 waiting for monitor entry [0x02d3f000 ..0x02d3fd68]
    at deadlockthreads.TestThread.run(TestThread.java:31)
    - waiting to lock <0x22c19f18> (a java.lang.Object)  # 等待获取 0x22c19f18 锁
    - locked <0x22c19f20> (a java.lang.Object)  # 持有 0x22c19f20 锁
"Thread-0" prio=5 tid=0x00accdb0 nid=0xdec waiting for monitor entry [0x02cff000 ..0x02cff9e8]
    at deadlockthreads.TestThread.run(TestThread.java:31)
    - waiting to lock <0x22c19f20> (a java.lang.Object)
    - locked <0x22c19f18> (a java.lang.Object)


# 热锁：由于多个线程对临界区，或者锁的竞争，可能出现 频繁的线程的上下文切换， 进而造成 大部分 CPU开销用在 “系统态 ”
# 表现为 大多数的线程状态都是 Waiting for monitor entry或者 Wait on monitor
我们曾经遇到过这样的例子，程序运行时，出现了以上指出的各种现象，通过观察操作系统的资源使用统计信息，以及线程 DUMP信息，确定了程序中热锁的存在，并发现大多数的线程状态都是 Waiting for monitor entry或者 Wait on monitor，且是阻塞在压缩和解压缩的方法上。后来采用第三方的压缩包 javalib替代 JDK自带的压缩包后，系统的性能提高了几倍。

```

## jcmd

用于向正在运行的 JVM 发送诊断命令请求。

```sh
# 单独不带参数使用
# 返回 pid mainClass cli—args
# 类似 jps
jcmd [-l]

# 向指定进程 发送 诊断命令
jcmd 11022 name=\"Value of name argument\"
# 向多个有相同 main class  的进程 发送命令
jcmd com.xxx.AppClass name=\"Value of name argument\"
# 打印性能参数
jcmd 1102 PerfCounter.print
# 从指定 file 读取命令
jcmd 1102 -f xxx_file
```

## jps

本地当前用户所有运行的 java 进程/jvm 实例

jps 命令有个地方很不好，似乎只能显示当前用户的 java 进程，要显示其他用户的还是只能用 unix/linux 的 ps 命令。

```sh
# pid, mainClass
jps
# pid
jps -q
# 传递给 main class 的参数
jps -m
# main class 完整 package path
jps -l
# 输出传递给JVM的参数
jps -v

# 列出远程服务器192.168.0.77机器所有的jvm实例，采用rmi协议，默认连接端口为1099（前提是远程服务器提供jstatd服务）
jps 192.168.0.77
```

## jstat

虚拟机统计监控工具。可以用于服务性能优化。重要

```sh
# -class，监控什么数据， 这里是监控 class 加载情况
    -class：统计class loader行为信息
    -compile：统计编译行为信息
    -gc：统计jdk gc时heap信息
    -gccapacity：统计不同的generations（不知道怎么翻译好，包括新生区，老年区，permanent区）相应的heap容量情况
    -gccause：统计gc的情况，（同-gcutil）和引起gc的事件
    -gcnew：统计gc时，新生代的情况
    -gcnewcapacity：统计gc时，新生代heap容量
    -gcold：统计gc时，老年区的情况
    -gcoldcapacity：统计gc时，老年区heap容量
    -gcpermcapacity：统计gc时，permanent区heap容量
    -gcutil：统计gc时，heap情况
    -printcompilation：不知道干什么的，一直没用过。
# -t，显示 timestamp 列
# -h3，每n次打印一次 表格 title
# 55912，进程id
# 1000（ms），每1000ms监控打印一次
# 100，共打印多少次（不写的话就是一直打印下去）
jstat -class -t -h3 55912 1000 100

# 结果列：
# timestamp，-t的输出结果
# loaded，已加载的类数量（真多。。很多都是动态代理的）
# bytes，已加载的类总大小
# unloaded，已卸载的类数量（多态代理的情况下，是可以卸载类的）
# bytes，已卸载的类总大小
# time，classLoader操作类的总耗时


# jit 情况
jstat -compiler -t -h3 55912 1000 100

# 监控 gc 情况
jstat -gc -t -h3 55912 1000 100
# 结果列：
# S01,S1C,S0U,S1U from，to的大小和已用量大小(KB)
# EC,EU eden的大小和已用量
# OC,OU 老边代的大小和已用量
# MC,MU 永久区大小和已用量
# CCSC,CCSU 压缩类空间大小和已用量
# YGC,YGCT: 新生代GC次数和耗时
# FGC,FGCT: Full GC次数和耗时
# GCT GC总耗时


# 查看gc原因以及各个分代的百分信息
jstat -gccapacity -t -h3 55912 1000 100
# 结果列
# S0 ：年轻代中第一个survivor（幸存区）已使用的占当前容量百分比
# S1 ：年轻代中第二个survivor（幸存区）已使用的占当前容量百分比
# E ：年轻代中Eden（伊甸园）已使用的占当前容量百分比
# O ：old代已使用的占当前容量百分比
# M ：元空间已使用的占当前容量百分比
# CCS： 压缩
# YGC ：从应用程序启动到采样时年轻代中gc次数
# YGCT ：从应用程序启动到采样时年轻代中gc所用时间(s)
# FGC ：从应用程序启动到采样时old代(全gc)gc次数
# FGCT ：从应用程序启动到采样时old代(全gc)gc所用时间(s)
# GCT：从应用程序启动到采样时gc用的总时间(s)
# LGCC：最后一次GC原因
# GCC：当前GC原因（No GC 为当前没有执行GC）
```

## jvisualvm

检测 java 内存 的变化情况，下载内存快照, 可用于排查内存泄露, 

垃圾回收查看

CPU 占用

线程分析 : 绿色(运行, 正在运行的 thread), 紫色(休眠, 调用 sleep 的 thread), 黄色(等待, 调用 object.wait 的 thread), 驻留(线程池中的空闲线程), 监视(正在阻塞/等待锁的 thread)

是 jconsole 升级版

## jconsole

图形监控界面， 类似 jvisualvm

不要 生产环境使用, 因为会对应用有性能影响

## jhat

JDK 自带的 Java 堆内存分析工具。

jhat 命令解析一个 java heap dump 文件并启动一个 web 服务器，http://localhost:7000/oqlhelp/。

## jmap

打印指定 Java 进程堆内存细节 linux 下特有

```sh
# 选项获取一个运行时的heap dump
jmap -dump 1102
# 于64位的虚拟机上
jmap -J-d64 -heap pid
```

## jrunscript

解释执行 javascript、groovy、ruby 等脚本语言

```sh
# 进入交互模式
jrunscript
# 执行字符串
jrunscript -e "print('hello world')"
# 指定语言， 指定文件
jrunscript -l js -f test.js
# test.js是一个脚本文件，arg1，arg2和arg3是传递给脚本的脚本参数。脚本可以使用"arguments"数组来访问这些参数
jrunscript test.js arg1 arg2 arg3

```

## jinfo

## jprofile

可连接到 tomcat， 可监控线程， 分析服务器的资源消耗情况, 商业工具, 非开源

# 阿里 arthas

https://github.com/alibaba/arthas
https://arthas.aliyun.com/doc/arthas-tutorials.html?language=en

https://github.com/WangJi92/arthas-idea-plugin
https://wangji.blog.csdn.net/article/details/106964278

# jvm 性能调优 and 实用参数


http://ifeve.com/jvm-optimize/
http://ifeve.com/useful-jvm-flags/


## 有哪些参数

```
-XX:MetaspaceSize=128m （元空间默认大小）
-XX:MaxMetaspaceSize=128m （元空间最大大小）
-Xms1024m （堆默认大小）
-Xmx1024m （堆最大大小）
-Xmn256m （新生代大小） eden 区 大小(伊甸园区)
-Xss256k （棧最大深度大小）
-XX:SurvivorRatio=8 （新生代分区比例 8:2）
-XX:+UseConcMarkSweepGC （指定使用的垃圾收集器，这里使用CMS收集器）
-XX:+PrintGCDetails （打印详细的GC日志）


JDK8之后把-XX:PermSize 和 -XX:MaxPermGen移除了，取而代之的是
-XX:MetaspaceSize=128m （元空间默认大小）
-XX:MaxMetaspaceSize=128m （元空间最大大小）
JDK 8开始把类的元数据放到本地化的堆内存(native heap)中，这一块区域就叫Metaspace，中文名叫元空间。
使用本地化的内存有什么好处呢？最直接的表现就是java.lang.OutOfMemoryError: PermGen 空间问题将不复存在，因为默认的类的元数据分配只受本地内存大小的限制，也就是说本地内存剩余多少，理论上Metaspace就可以有多大（貌似容量还与操作系统的虚拟内存有关？这里不太清楚），这解决了空间不足的问题。不过，让Metaspace变得无限大显然是不现实的，因此我们也要限制Metaspace的大小：使用-XX:MaxMetaspaceSize参数来指定Metaspace区域的大小。JVM默认在运行时根据需要动态地设置MaxMetaspaceSize的大小。
```


## 配置方式

```
-XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=128m -Xms1024m -Xmx1024m -Xmn256m -Xss256k -XX:SurvivorRatio=8 -XX:+UseConcMarkSweepGC

1. idea VM options
1. java -jar -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=128m -Xms1024m -Xmx1024m -Xmn256m -Xss256k -XX:SurvivorRatio=8 -XX:+UseConcMarkSweepGC newframe-1.0.0.jar

```