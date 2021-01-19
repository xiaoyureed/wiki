---
title: Java Concurrent
tags:
  - java
  - concurrent
date: 2018-09-22 22:13:31
categories: java core
---


<div align="center">
并行计算带来计算能力的提升但也引入了软件的复杂性, 应该只用于 图形处理, 服务端编程(Linus)

"计算机硬件设计者将摩尔定律失效的责任推脱给了软件开发者"

Amdahl定律,   Gustafson定律   ----- cpu个数, 串行比例, 并行后加速比的关系

https://github.com/ZHENFENG13/concurrent-programming (《实战java高并发程序设计》)

https://www.zhihu.com/question/59725713/answer/168709945 

https://github.com/oldratlee/fucking-java-concurrency

TODO
</div>

<!--more-->

<!-- TOC -->

- [为什么需要并行(多线程)计算](#为什么需要并行多线程计算)
- [多线程需要解决的问题](#多线程需要解决的问题)
- [容易混淆的几个概念](#容易混淆的几个概念)
- [并发级别](#并发级别)
- [java内存模型-jmm](#java内存模型-jmm)
- [多线程线程安全的3个关键特性](#多线程线程安全的3个关键特性)
  - [原子性(atomicity)](#原子性atomicity)
  - [可见性-缓存一致性](#可见性-缓存一致性)
  - [有序性-指令重排](#有序性-指令重排)
- [语法基础](#语法基础)
  - [线程和进程](#线程和进程)
    - [两者的概念](#两者的概念)
    - [线程实现原理](#线程实现原理)
    - [线程的调度方式](#线程的调度方式)
    - [线程的几个状态](#线程的几个状态)
  - [thread基本api](#thread基本api)
    - [创建](#创建)
    - [终止](#终止)
    - [中断 InterruptedException 处理](#中断-interruptedexception-处理)
    - [等待和通知-wait-notify](#等待和通知-wait-notify)
    - [等待结束和谦让-join-yield](#等待结束和谦让-join-yield)
    - [线程组](#线程组)
    - [守护线程-daemon](#守护线程-daemon)
    - [线程优先级](#线程优先级)
  - [相关的关键字](#相关的关键字)
    - [volatile](#volatile)
    - [synchronized](#synchronized)
- [jdk中非线程安全的类库造成的隐蔽错误](#jdk中非线程安全的类库造成的隐蔽错误)
  - [先看一个隐蔽错误的例子-int型溢出](#先看一个隐蔽错误的例子-int型溢出)
  - [并发下的ArrayList](#并发下的arraylist)
  - [并发下的Hashmap](#并发下的hashmap)
  - [错误的使用锁](#错误的使用锁)
- [concurrent包](#concurrent包)
  - [怎么进行同步控制](#怎么进行同步控制)
    - [重入锁-锁的公平性](#重入锁-锁的公平性)
    - [Condition-重入锁好搭档](#condition-重入锁好搭档)
    - [semaphore-信号量-多个线程同时访问](#semaphore-信号量-多个线程同时访问)
    - [ReadWriteLock-读写锁](#readwritelock-读写锁)
    - [CountDownLatch-倒计时器](#countdownlatch-倒计时器)
    - [CyclicBarrier-循环栅栏](#cyclicbarrier-循环栅栏)
    - [LockSupport-线程阻塞工具类](#locksupport-线程阻塞工具类)
  - [线程复用-线程池](#线程复用-线程池)
    - [Executor的api](#executor的api)
      - [创建线程池](#创建线程池)
      - [怎么确定池子的容量](#怎么确定池子的容量)
      - [线程池的几个状态](#线程池的几个状态)
      - [关闭线程池](#关闭线程池)
    - [ThreadPoolExecutor-自定义线程池](#threadpoolexecutor-自定义线程池)
      - [核心 api](#核心-api)
      - [阻塞队列 BlockingQueue](#阻塞队列-blockingqueue)
      - [自定义拒绝策略](#自定义拒绝策略)
      - [自定义线程池名称](#自定义线程池名称)
      - [自定义线程 名称](#自定义线程-名称)
    - [无锁安全的队列](#无锁安全的队列)
    - [拓展线程池-在线程池中寻找堆栈](#拓展线程池-在线程池中寻找堆栈)
    - [Fork-Join框架](#fork-join框架)
    - [springboot中使用线程池](#springboot中使用线程池)
    - [hystrix-线程池隔离](#hystrix-线程池隔离)
  - [jdk中的并发容器](#jdk中的并发容器)
    - [整体介绍](#整体介绍)
    - [ConcurrentLinkedQueue原理](#concurrentlinkedqueue原理)
  - [aqs 无锁实现](#aqs-无锁实现)
- [java8中的新特性](#java8中的新特性)
- [怎么优化锁](#怎么优化锁)
  - [提高锁性能的原则](#提高锁性能的原则)
    - [减少锁持有时间-和锁粗化-需要权衡](#减少锁持有时间-和锁粗化-需要权衡)
    - [减小锁粒度](#减小锁粒度)
    - [读写分离锁代替独占锁](#读写分离锁代替独占锁)
    - [锁分离](#锁分离)
  - [jvm是怎么对锁优化的](#jvm是怎么对锁优化的)
    - [使锁进入偏向模式-偏向锁](#使锁进入偏向模式-偏向锁)
    - [使用轻量级锁](#使用轻量级锁)
    - [自旋锁](#自旋锁)
    - [锁消除](#锁消除)
  - [ThreadLocal](#threadlocal)
    - [threadlocal概念](#threadlocal概念)
    - [threadLocal 示例](#threadlocal-示例)
    - [再看一个问题例子以及改进](#再看一个问题例子以及改进)
    - [原理](#原理)
    - [看源码](#看源码)
  - [weakReference](#weakreference)
    - [什么是WeakReference](#什么是weakreference)
    - [为什使用](#为什使用)
    - [怎么使用](#怎么使用)
    - [四种引用](#四种引用)
    - [ReferenceQueue](#referencequeue)
    - [WeakHashMap & TreadLocalMap](#weakhashmap--treadlocalmap)
  - [不加锁-乐观锁](#不加锁-乐观锁)
    - [怎么理解不加锁的策略](#怎么理解不加锁的策略)
    - [AtomicInteger-无锁安全整数](#atomicinteger-无锁安全整数)
    - [Unsafe类-Java中的指针](#unsafe类-java中的指针)
    - [AtomicReference-无锁的对象引用](#atomicreference-无锁的对象引用)
    - [AtomicStampedReference-带时间戳的引用](#atomicstampedreference-带时间戳的引用)
    - [AtomicIntegerArray-无锁的数组](#atomicintegerarray-无锁的数组)
    - [AtomicIntegerFieldUpdater-升级普通变量支持原子操作](#atomicintegerfieldupdater-升级普通变量支持原子操作)
    - [SynchronousQueue实现](#synchronousqueue实现)
- [死锁](#死锁)
  - [怎么打印线程堆栈信息](#怎么打印线程堆栈信息)
- [并发相关的模式和算法](#并发相关的模式和算法)
  - [单例模式的最佳实现方式](#单例模式的最佳实现方式)
  - [不变模式](#不变模式)
  - [生产者-消费者模式](#生产者-消费者模式)
    - [典型实现](#典型实现)
    - [无锁的实现](#无锁的实现)
  - [Future模式](#future模式)
    - [典型Future模式实现](#典型future模式实现)
    - [jdk中的future模式](#jdk中的future模式)
  - [并行流水线](#并行流水线)
  - [并行搜索](#并行搜索)
  - [并行排序](#并行排序)
    - [改进冒泡排序-分离数据相关性-奇偶交换排序](#改进冒泡排序-分离数据相关性-奇偶交换排序)
    - [改进插入排序-希尔排序](#改进插入排序-希尔排序)
  - [网络nio](#网络nio)
- [使用akka构建高并发程序](#使用akka构建高并发程序)
- [并行程序怎么调试](#并行程序怎么调试)

<!-- /TOC -->

# 为什么需要并行(多线程)计算

- 为了获得更好的计算能力 - 比如图形处理程序, 一张图片就包含无数的像素, 要遍历这些像素需要花费巨额时间; 比如 服务端程序, 淘宝等等电商, 业务并发量巨大

- 真实世界的业务模型就是并行的

# 多线程需要解决的问题

- 上下文切换 - 线程切换的时候需要保存本次执行的信息, 再次获取 cpu 控制权需要恢复上次所保存的信息

    - 采用无锁编程，比如将数据按照 Hash(id) 进行取模分段，每个线程处理各自分段的数据，从而避免使用锁

    - 采用 CAS(compare and swap) 算法，如 Atomic 包就是采用 CAS 算法

    - 避免创建了一些线程但其中大部分都是处于 waiting 状态，因为每当从 waiting 状态切换到 running 状态都是一次上下文切换

- 死锁 - 线程 A 和线程 B 都在互相等待对方释放锁，或者是其中某个线程在释放锁的时候出现异常如死循环

    - 尽量一个线程只获取一个锁, 只占用一个资源
    
    - 使用定时锁，至少能保证锁最终会被释放。

- 资源限制 - 一个线程不能无限制的使用资源, 这是为了避免一个线程出现异常导致整个系统出问题

# 容易混淆的几个概念

可以参考 java-note 中的 "同步异步和阻塞非阻塞" 一节

- synchronous(同步) 和 asynchronous(异步) - 用来形容某一次方法调用

  - 同步方法一旦开始, 就必须等到方法结束后(如果有返回值, 则是等到返回值返回后), 再执行后续方法

  - 异步..........., 方法会立即返回, 继续进行后续任务; 异步任务通常会另外开一个 thread , 如果有返回结果则在 exec 完毕后会通知调用者

- concurrency(并发) 和 parallelism(并行) - 都可以表示多个任务一起执行

  - concurrency 表示多个任务交替执行, 这些任务可能仍旧是串行的 --- 单cpu, 因为一次只能执行一条指令

  - parallelism 是真正的 同时执行 ------ 多cpu

- 临界区 - 泛指公共资源, 被多个 thread 共享, 同一时间只能有一个 thread 能使用

- blocking(阻塞) 和 non-blocking(非阻塞) - 用来形容cpu状态; 如果 cpu 阻塞, 当前 thread 会被挂起, cpu控制权被其他thread夺去, 在另一个 thread 释放 resource 前, 当前 thread 只能阻塞, 不能执行; ---------- 因此也可形容某个操作是否是阻塞的 (是否会造成 cpu 阻塞)

  - blocking: 如果一个 thread 正在占用 临界区resources, 则其他的 thread 就需要在临界区等待(线程 被挂起) ------------- 一个耗时任务执行时, cpu空闲等待

  - non-blocking: 多个 thread 互不影响, 无论是否占用 临界区 resources, thread 都会尝试不断向前执行 -------- 一个耗时任务(如io任务)执行时, cpu不会空闲, 会去执行其他任务

- deadlock(死锁), starvation(饥饿), live lock(活锁)

  - deadlock - 多个线程互相等待对方释放资源, 无解

  - starvation - thread 无法得到需要的resources, 而一直等待. 过一段时间有可能缓解的; eg: thread优先级太低; 资源被其他thread一直霸占

  - live lock - resources 不断再两个 thread 间跳动, 没有一个 thread 同时拿到运行必须的所有 resources, 造成两个都不能正常执行

# 并发级别

- blocking - 一个 thread 是 blocking的, 在其他 thread 释放 resource 前, 该 thread 无法执行; 比如使用 `synchronized`, or `重入锁`, 得到的就是 blocking  thread

- starvation-free(无饥饿) - 在线程排队执行时无视优先级, 不允许高优先级的 thread 插队; 就是 采用 "公平锁" 而不用 "非公平锁"

- obstruction-free(无障碍) 

  - 最弱的 non-blocking 调度

  - 2个 thread 之间是 obstruction-free 的关系, 表示不会因为共享资源的问题导致一方被挂起等待, 都可以同时进入 临界区, 如果是 read-only 操作 都相安无事, 如果是 write 操作, 当 A thread 在 write时候检测到 B thread 也在 write, A 就会立即对自己的操作进行回滚.

  - 不一定能保证程序顺利运行, 比如 两个 thread 都不断的回滚自己的write 操作 , 这样就没有一个 thread 能够顺利的走出 临界区

    一种解决方法是: "一致性标记", 每个 write操作都需要更新这个标记 - 在某个write操作前先读取保存这个标记, 在操作完成后再次读取, 和保存的版本比较, 如果一致则证明在修改期间没有其他线程的 write操作, 如果不一致 需要重试or报错.

- lock-free(无锁) 

  - 所有的 thread 都能对 临界区访问, 并且保证一定有一个 thread 能够顺利离开临界区

  - 典型特征是, 包含一个`死循环`, thread 会不断尝试 修改 共享变量, 修改成功那么thread顺利退出, 否则继续尝试修改

- wait-free(无等待) 

  - 要求所有 thread 可以在有限步数内完成操作, 离开临界区

  - rcu(read-copy-update) 基本思想: read-only操作可以不加控制, 所有 read 操作都是 wart-free 的, 对 write操作, 先取得原始数据的 copy, 仅仅修改这份 copy即返回修改成功, 在合适的时间回写数据到数据库

# java内存模型-jmm

{% post_link java-memory-model 📚 java-memory-model %}

# 多线程线程安全的3个关键特性

## 原子性(atomicity)

在一个操作中就是cpu不可以在中途暂停然后再调度，既不被中断，要么执行完成，要不就不执行 - 一个操作一旦开始, 就不会被其他 thread 干扰

eg: 对于 静态全局变量 int i, 两个 thread 同时赋值, 1, -1, 那么最终i的结果只能是 1或-1中的一个, 不会是其他结果 ------ `给int类型值赋值操作具有原子性`

eg: 对于 静态全局变量 long i, .........................., 最终 i的值可能是怪异的结果 ---------------------------- `long 类型 占据8字节， 64bit, 在 32bit机器上读/写至少需要两条指令，不是原子性的`, 有可能thread1获取 cpu控制权, 一次读取 32bit, 也就是 long 型数据的二进制前一半, 正准备接着读取剩下一半, cpu控制权被 thread2 切走, thread2进行了修改操作， thread1接着读取， 拼出来的long就是怪异的结果；

在Java中可以使用 synchronized来保证方法和代码块内的操作是原子性的。底层原理是 `两个高级的字节码指令 monitorenter 和 monitorexit`

## 可见性-缓存一致性

保证读取到的变量都是最新的;

- 当多个线程访问同一个变量时，一个线程修改了这个变量的值，其他线程能够立即看得到修改的值。 ------------ 也可以说是 "缓存一致性" 问题, cpu读取到了 cache 中的脏数据

- 使用 volatile 声明 变量 i 来保证可见性 - 告诉jvm, 这个变量 i 是不稳定的, 随时会被多个thread 修改, 必须特殊对待 

    - 当写一个volatile变量时，JMM会把该线程对应的本地内存中的变量强制刷新到主内存中去；

    - 针对volatile变量的写操作会导致其他线程本地内存中的缓存无效。

    - 禁止指令重排序优化

## 有序性-指令重排

程序在执行时候 可能进行 指令重排, 重拍后的指令和原始指令不一样

重排前后, 串行程序运行结果都是一致的, 但是并行程序就不一定了

为什么又有 "指令重排"? ------ cpu执行一个汇编指令会分成很多步骤, 每个步骤涉及的硬件不同, 比如 step1进行完后, 对应的硬件就闲置了, 必须等到整个汇编指令对应的step都执行完才会被再次利用, cpu引入 "流水线"的工作机制, 指令重排后的命令可以减少流水线的中断

哪些指令不会被重排? ----------- 遵循这几个原则

- 程序顺序原则 - 保证重排前后, 串行程序结果不变

- volatile - 保证 volatile 变量的 write操作, 先发于 read操作, 这保证了 volatile变量的可见性

- 锁规则 - 保证 解锁 在随后的 加锁 前

- 传递性 - a先于b, b先于c, 那么 a先于c;

- 线程的 start() 先于 该 thread 的每个动作 - 也就是保证 thread 首先正常启动

- 线程的 interrupt() 先于 该 thread 被中断的代码

使用synchronized和volatile来保证多线程之间操作的有序性。实现方式有所区别：volatile关键字会禁止指令重排。synchronized关键字保证同一时刻只允许一条线程操作


# 语法基础

## 线程和进程

### 两者的概念

- 进程(process)代表一个内存中运行的应用程序

    - 每个进程都有自己独立的一块内存空间， 因为进程是资源分配和拥有的单位 (计算机在分配计算资源时, 是按照进程为单位分配的)

    - 一个进程中可以运行多个线程， 这些线程共享进程的资源（内存，文件io）; 

    - 至少拥有一个thread - main thread， 也就是运行 mian()方法的线程，它被称为主线程， 对于java， 所有 thread 执行完毕 程序退出， 对于golang，main thread 执行完毕程序就退出了， 这和两者的 thread实现原理有关

- 线程是指进程中的一个执行流程，

    - thread 是cpu调度的最小单位

    - 线程不能够独立执行， 必须存在于某个进程中

    - 每个线程都有一个调用栈， 一旦创建一个新的线程，就产生一个新的调用栈。

    - 线程总体分两类：用户线程和守候线程。对于Java， 当所有用户线程执行完毕的时候，JVM自动关闭

### 线程实现原理

在操作系统的OS Thread和编程语言的User Thread之间，实际上存在3中线程对应模型，也就是：1:1，1:N，M:N

- 使用内核线程实现 （Java） - 一个用户线程就只在一个内核线程上跑，这时可以利用多核，但是上下文switch很慢，频繁切换效率很低

    这种实现方式，线程直接由操作系统内核支持。程序一般不会直接去使用内核线程，而是去使用内核线程的一种高级接口——轻量级进程，轻量级进程与内核线程之间是1:1的关系，称为一对一的线程模型 

- 使用用户线程实现 - 协程 - 多个（N）用户线程始终在一个内核线程上跑，context上下文切换确实很快，但是无法真正的利用多核

    用户线程的建立、 同步、 销毁和调度完全在用户态中完成，不需要内核的帮助。内核线程与用户线程为1：N的关系

- 使用用户线程加轻量级进程混合实现 （Golang） - 多个 协程 （比如 goroutine）在多个内核线程上跑，这个看似可以集齐上面两者的优势，但是无疑增加了调度的难度。

    这种实现方式将内核线程与用户线程一起使用，在这种混合实现下，既存在用户线程，也存在轻量级进程。 用户线程还是完全建立在用户空间中 。用户线程与轻量级进程为N:M的关系。

### 线程的调度方式

协同式线程调度和抢占式线程调度。

- 协同式线程调度 - 线程的执行时间由线程本身来控制，线程把自己的工作执行完了之后，要主动通知系统切换到另外一个线程上。

    优点：实现简单，而且由于线程要把自己的事情干完后才会进行线程切换，切换操作对线程自己是可知的，所以没有什么线程同步的问题。  

    缺点：线程执行时间不可控制，甚至如果一个线程编写有问题，一直不告知系统进行线程切换，那么程序就会一直阻塞在那里。  

- 抢占式线程调度 （Java） - 每个线程由系统来分配执行时间，线程的切换不由线程本身来决定（在Java中，Thread.yield（）可以让出执行时间，但是要获取执行时间的话，线程本身是没有什么办法的）。

    线程的执行时间是系统可控的，也不会有一个线程导致整个进程阻塞的问题

### 线程的几个状态

thread 几种状态: [online](https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=Untitled%20Diagram.xml#R5VlLc5swEP41mmkP8SAwr2PtkPbQzmQmhzZHBWRQA5JHyLGdX18B4inseuJX0iYzGbEsy%2B633%2B5KBFjzbPOVo2Xyg0U4BaYRbYB1C0zT9yz5txBsK4FtupUg5iSqRLAVPJBXrISGkq5IhPOeomAsFWTZF4aMUhyKngxxztZ9tQVL%2B29dohhrgocQpbr0J4lEUkm9OopC%2Fg2TOKnfDB2%2FupOhWllFkicoYuuOyAqANeeMiWqVbeY4LbCrcameu9txt3GMYyoOeWBaPfCC0pWKTfkltnWwOJKxq0vGRcJiRlEatNIZZysa4cKiIa8SkaVyCeXyNxZiq9KHVoJJUWvhO2NLpReyjIRq3SBS2KqcKTzYGZ8S5WzFQ6Wl2CUQj7HSMhtsJScxy7DgW6nCcYoEeelbR4odcaPXAigXCsNxPF0Nz1y6IT591mAVeCP6aHGck1f0VCoUwS8ZoaL0xZ4B%2B1ZKUEpiKgWhjB5zKXjBXBDJyy%2FqRkaiqExJip5wOkPhc1wmZ85Sxsv3Wovyp8G2sIA3Y1WjPGm52kV9Oo6nMnRjTMypqTivavxGWT8YcmX9vkCho8IWi1xmdZiTxomD0uSfnvZdDneS2qPzofXwNtqbOu29C9G%2BbsxdQAMXzGbAt0DgAP8W%2BHf%2FWAn4fysB6Dr2%2By0BaOqtakvDhDMqCRppyTqI7XhDxC%2BlUqwfi%2FXEf2sdSKz5trQ3Me36%2BrFj%2Fx5zImMuiHDEvBgpHGgdWTmD1NX08o0eIyzD6JuofFBPHZdfR8vvGpGxSdTtdJTR87a2vSk9XfrsSzU%2B%2F2RwNrUzce1O%2BQxAgz3MJgZ0a8GwFq47eeClRk%2Bzkz5dBiDodi%2FX7mdAY%2B3%2FAjQcmfEe8AIwc0Fgg1kAfLnwgT8FHjxzUkx7Xy%2B58MYLXvHAoU9xvqK03LjsnuBFqOuECPywRGUQa3lM39Pcu6mJU5TnvX49sn%2FSANu5T7LM%2FkSEtrpetwdrWOsknUO1YxwPHrRHZySh8ccAz3QH2wnnguB5GnZy9mSEIrF39%2FiO4HOMAfcuCZ%2BlwUfx%2BqPgdsWS1XF7Sln4%2FFE4BwecM71Lgqd%2FnqJMkMX26qeC8b2vsWfndcS4tk8%2FrsePe5blTaDX%2FtafW%2Bt27Q1yWnmpnf50ux6cdMxC1%2BlzyjzfqXKk68tTR1QQ%2BCNNzqk3mJzT85WhvGw%2F6FeYt%2F8VsYI%2F)

![alt](Snipaste_2018-11-19_17-14-22.png)

## thread基本api

### 创建

- 使用 start(), 而不是 run()

- 实现 Runnable 接口 而不是 继承 Thread 类

### 终止

- 不要使用 stop() 

  - 因为太暴力, 强行将未执行完的 thread 终止, 会造成数据不一致的问题

  - 为什么会出现  `数据不一致`呢? - stop() 会直接终止 thread, 立即释放 thread 所持有的 锁, 如果此时 thread 正 write 数据到一半,   被强行终止, 对象就被改坏了, 同时 锁被释放, 这个错误的对象就被读取了

- 自己定义一个 "是否停止" 的 标记变量, 决定何时退出 - jdk提供更好的办法: interrupt(中断)

### 中断 InterruptedException 处理

thread 不立即退出, 只是接受到一个中断信号, 至于接收到这个信号干什么, 有我们自己写代码决定

api:

![alt](Snipaste_2018-11-19_20-24-24.png)

```java
public static void main(String args[]) throws InterruptedException {

  Thread thread = new Thread() {
      @Override
      public void run() {
          while (true) {
              if (Thread.currentThread().isInterrupted()) {// 收到中断信号
              // 执行我们自己的逻辑
                  System.out.println("收到中断信号,停止该线程!");
                  break;
              }
              try {
                  Thread.sleep(600);
                  // TimeUnit.SECONDS.sleep(60); // 优雅的 睡眠
              } catch (InterruptedException e) {
                  e.printStackTrace();// sleep 时候 被 interrupt, 抛异常, 会清除 interrupt 标记, 即没有收到中断信号
                  Thread.currentThread().interrupt();// 需要再次手动恢复 interrupt 标记, 即收到中断信号
              }
              System.out.println("Running!");
              Thread.yield(); // 谦让
          }
      }
  };

  thread.start();
  Thread.sleep(2000);
  thread.interrupt();//进行中断操作
}

```

### 等待和通知-wait-notify

- 必须在 synchronized 中使用, 等待, 唤醒操作都必须先获得目标对象的一个监视器（锁）

- 不推荐使用 - 会干扰 系统 api 工作 或被系统api影响, 比如 join(), 本质就是调用 wait(), 如果随便使用 wait(), 会干扰 这个api

- 这两个方法定义 在 Object 类 上, 任何对象都可以调用, 比如: thread A call了 obj.wait(), thread A 会进入obj的等待队列, 直到其他 thread 调用 了 obj.notify(), obj 会从自己的等待队列中随机选取一个 thread;

  这时 obj 就像是 thread A B 间的 通讯手段

- 不要使用 suspend()挂起, resume()恢复 ---- 因为这样挂起thread 锁不会释放, 而且由于指令重排, resume()在suspend()前执行会造成thread永远挂起

```java
/**
//wait()方法会释放目标对象的锁,而Thread.sleep()方法不会释放任何资源.
*/
public class SimpleWaitAndNotify {
    final static Object object = new Object();

    public static class Thread1 extends Thread {
        public void run() {
            synchronized (object) { // 执行 wait 方法前, 首先获取 object 对象锁
                System.out.println(System.currentTimeMillis() + ":thread1 start !");
                try {
                    System.out.println(System.currentTimeMillis() + ":thread1 wait for object !");
                    object.wait();// 执行完毕后 thread 会释放object锁, 等待, 一旦有 notify 信号, 则再次获取 object 锁, 被唤醒
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(System.currentTimeMillis() + ":thread1 end!");
            }
        }
    }

    public static class Thread2 extends Thread {
        public void run() {
            synchronized (object) { //首先获取 object 对象锁
                System.out.println(System.currentTimeMillis() + ":thread2 start ! notify one thread");
                object.notify();  

                try {
                    Thread.sleep(2000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(System.currentTimeMillis() + ":thread2 end!");
            }
        }
    }

    public static void main(String args[]) {
        Thread thread1 = new Thread1();
        Thread thread2 = new Thread2();
        thread1.start();
        thread2.start();
    }
}

```

### 等待结束和谦让-join-yield

threa1.join()表示 thread1加入了当前线程 - 会使得 current thread 等待 thread1执行完毕 再执行

thread1.yield() 让出 cpu 控制权, 重新争夺cpu

```java
public class JoinMain {

    public volatile static int i = 0;

    public static class AddThread extends Thread {
        public void run() {
            System.out.println("add!");

            for (i = 0; i < 1000000; i++) ;

            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String args[]) throws InterruptedException {

        AddThread at = new AddThread();
        at.start();
        at.join();//使用join()方法后,主线程会等待AddThread执行完毕,i输出为1000000,如果没有这条语句,i输出为0
        //可以查看join的底层代码,本质即让调用线程在当前线程对象实例上等待
        System.out.println(i);
    }
}

```

### 线程组

```java
public class ThreadGroupName implements Runnable {

    @Override
    public void run() {
        String groupAndName = 
            Thread.currentThread().getThreadGroup().getName() + "-" + Thread.currentThread().getName();
        while (true) {
            System.out.println("I am " + groupAndName);
            try {
                Thread.sleep(3000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String args[]) {
        ThreadGroup tg = new ThreadGroup("PrintGroup");
        Thread t1 = new Thread(tg, new ThreadGroupName(), "T1");
        Thread t2 = new Thread(tg, new ThreadGroupName(), "T2");
        t1.start();
        t2.start();
        System.out.println(tg.activeCount());// 活动线程的大概总数
        Thread t3 = new Thread(tg, new ThreadGroupName(), "T3");
        t3.start();
        System.out.println(tg.activeCount());
        tg.list();// 打印所有的线程
    }
}

```

### 守护线程-daemon

当系统只有 daemon thread 存在, jvm 就自动退出了

比如 GC 线程, JIT线程 就是 daemon

可通过 `thread.setDaemon(true);`

```java
public class DaemonDemo {
    public static class DaemonT extends Thread {
        public void run() {
            while (true) {
                System.out.println("I am alive");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    /**
     * thread被设置为守护线程,系统中只有主线程main为用户线程,因此
     *在main线程休眠3秒退出时,整个程序也随之结束,如果不把线程thread设置为守护线程,
     * main线程结束后,t线程还会不停的打印,永远也不会结束.
     *
     * tip:当一个Java应用内,只有守护线程时,Java虚拟机就会退出.
     *
     * @param args
     * @throws InterruptedException
     */
    public static void main(String args[]) throws InterruptedException {
        Thread thread = new DaemonT();
        thread.setDaemon(true);
        thread.start();
        Thread.sleep(3000);

    }
}

```

### 线程优先级

虽然Java线程调度是系统自动完成的，但是我们还是可以“建议”系统给某些线程多分配一点执行时间，Java语言一共设置了10个级别的线程优先级（Thread.MIN_PRIORITY至Thread.MAX_PRIORITY），在两个线程同时处于Ready状态时，优先级越高的线程越容易被系统选择执行， 但是 “并不是一定先被执行”

```java
public class PriorityDemo {
    public static class HightPriority extends Thread {
        static int count = 0;

        public void run() {
            while (true) {
                synchronized (PriorityDemo.class) {//此处产生资源竞争
                    count++;
                    if (count > 1000000) {
                        System.out.println("HightPriority is complete!");
                        break;
                    }
                }
            }
        }
    }

    public static class LowPriority extends Thread {
        static int count = 0;

        public void run() {
            while (true) {
                synchronized (PriorityDemo.class) {//此处产生资源竞争
                    count++;
                    if (count > 1000000) {
                        System.out.println("LowPriority is complete!");
                        break;
                    }
                }
            }
        }
    }


    /**
     * 低优先级的线程先启动,但是并不能保证每次都是LowPriority先完成,资
     源竞争的情况下还是会先确保优先级较高的线程 (这里的线程 hight) 获得资源.
     *
     * @param args
     */
    public static void main(String args[]) {
        Thread high = new HightPriority();
        Thread low = new LowPriority();
        high.setPriority(Thread.MAX_PRIORITY);// 有 3 个枚举量
        low.setPriority(Thread.MIN_PRIORITY);
        low.start();
        high.start();
    }

}

```

## 相关的关键字

### volatile

保证变量的 "可见性(变量更新立即刷入堆内存共享)", "有序性(禁止指令重排序)", 但是无法保证 "原子性(变量的读取,更新,保存可以由一个thread连续完成)"

- volatile 使得变量具有可见性, 对比于synchronized, lock所带来的庞大开销, 是轻量级的同步机制 ------最终效果就是：一条线程更新了变量，其他线程会立刻知道共享变量的最新版本值

  - 当写一个volatile变量时，JMM会把该线程对应的本地内存（线程栈）中的变量强制刷新到主内存（堆内存）中去；

  - 针对volatile变量的写操作会导致其他线程本地内存中的缓存无效。

  - 禁止指令重排序优化

- volatile对于单个的共享变量的读/写具有原子性，但是像num++这种复合操作，volatile无法保证其原子性，提出了解决方案，就是使用并发包中的原子操作类，通过循环CAS的方式来保证num++操作的原子性

用法: 直接修饰变量

底层的内存变化原理参见：java-memory-model.md 中的 #thread具体怎么访问对象中的值呢以及volatile关键字

使用场景:

- 变量仅仅作为一个`状态标志`, 如布尔值
- 构造单例模式时, 使用双重空检查, 变量需要声明为 volatile, 禁止指令重排序;
- 变量只会被一个thread更改, 其他thread只会读取这个变量
- 读取多,而写操作少时, 相比于直接读写都加锁, 采用 "写加锁读加volatile" 的策略实现高性能的 "读写锁"

```java
// 线程1执行doWork()的过程中，可能有另外的线程2调用了shutdown，所以boolean变量必须是volatile
//而如果使用 synchronized 块编写循环要比使用 volatile 状态标志编写麻烦很多
// 这里如果使用 synchronized 怎么实现呢? // todo
volatile boolean shutdownRequested;
 
...
 
public void shutdown() { 
    shutdownRequested = true; 
}
 
public void doWork() { 
    while (!shutdownRequested) { 
        // do stuff
    }
}

/////////////////////////////////

//注意volatile！！！！！！！！！！！！！！！！！  
private volatile static Singleton instace;   
  
public static Singleton getInstance(){   
    //第一次null检查     
    if(instance == null){            
        synchronized(Singleton.class) {    //1     
            //第二次null检查       
            if(instance == null){          //2  
                instance = new Singleton();//3  
            }  
        }           
    }  
    return instance; 
/////////////////////////////////////////

public class UserManager {
    public volatile String lastUser; //发布的信息
 
    public boolean authenticate(String user, String password) {
        boolean valid = passwordIsValid(user, password);
        if (valid) {
            User u = new User();
            activeUsers.add(u);
            lastUser = user;
        }
        return valid;
    }
} 
////////////////////////////////////////////////


public class CheesyCounter {
    private volatile int value;// 读取操作借助 volatile实现了可见性; 允许多个线程同时读
 
    //读操作，没有synchronized，提高性能
    public int getValue() { 
        return value; 
    } 
 
    //写操作，必须synchronized。因为x++不是原子操作
    // 只允许单个thread同时写操作
    public synchronized int increment() {
        return value++;

```

仅仅使用 volatile 无法保证线程安全

要使 volatile 变量提供理想的线程安全，必须同时满足下面两个条件：

- 对变量的写操作不依赖于当前值。
- 该变量没有包含在具有其他变量的不变式中。


以下是错误示例:

```java
// 错误示例: 对变量的写入操作依赖了当前值
// 需要对方法加锁
public class VolatileDemo {
    static volatile int i = 0;

    public static class PlusTask implements Runnable {

        @Override
        public void run() {
            for (int j = 0; j < 10000; j++) {
                i++;// i = i + 1; 违背了条件一
            }
        }
    }

    /**
     * volatile保证变量的可见性,但是无法保证一些复合操作的原子性, 现在启动10个线程对i进行累加操作,
     * 如果一切正常的话,结果应为100000,但是结果为94875或者其他小于100000,因此线程的并发中没有保证i的原子性.
     *
     * 比如: 刚开始, thread1, thread2 同时读取 i=0, 各自计算后得到 i=1, 并先后写入这个结果, 这样, 虽然 i++ 执行了 2次, 实际上 i 只增加了 1
     *
     * @param args
     * @throws InterruptedException
     */
    public static void main(String args[]) throws InterruptedException {
        long start = System.currentTimeMillis();
        Thread[] threads = new Thread[10];
        for (int j = 0; j < 10; j++) {
            threads[j] = new Thread(new PlusTask());
            threads[j].start();// 多个 thread 同时对变量 i 写入, 会产生覆盖 , so 最终 i 达不到 100000
        }
        for (int j = 0; j < 10; j++) {
            threads[j].join();// main thread wait until these thread end
        }
        System.out.println(i);
        long end = System.currentTimeMillis();
        long time = end - start;
        System.out.println("耗时:" + time);
    }
}

// 错误示例: 变量包含在了具有其他变量的不变式中
// 需要对方法加锁
//如果初始状态是(0, 5)，同一时间内，线程 A 调用setLower(4) 并且线程 B 调用setUpper(3)，显然这两个操作交叉存入的值是不符合条件的，那么两个线程都会通过用于保护不变式的检查，使得最后的范围值是(4, 3) —— 一个无效值。
public class NumberRange {
    private volatile  int lower, upper;
 
    public int getLower() { return lower; }
    public int getUpper() { return upper; }
 
    public void setLower(int value) { 
        if (value > upper) // 违背了规则二
            throw new IllegalArgumentException(...);
        lower = value;
    }

    public void setUpper(int value) { 
        if (value < lower) 
            throw new IllegalArgumentException(...);
        upper = value;
    }
}
```

### synchronized

synchronized使得代码块具有 `原子性（atomicity）`和 `可见性（visibility）`, 有序性

volatile 变量 能够确保自己被某个 thread 修改后, 立即(刷入堆内存)被 其他 thread 看到(解决了可见性), 但是 `无法禁止多个thread同时访问一个变量`, 多个 thread 同时修改这个变量 , 还是会冲突(不能保证线程安全)

synchronized 使得 同一时间, 只能有一个 thread 进入 同步块. 这样一次性解决了 线程安全, 可见性, 有序性

用法有三:

- 作用于实例方法 - 对当`前实例对象`加锁, 进入方法前, 先获取 当前实例的锁

- 指定加锁对象 - 使用synchronized修饰代码块. 锁住的是 `()` 中的对象, 进入同步代买块前需要先获取对象的锁，这里的对象必须是两个thread 的共享对象；

- 作用于静态方法. -  synchronized 修饰静态的方法或静态代码块(是`当前类的Class对象`作为锁的对象), 进入前需要获取当前类的锁

原理: JVM 是通过thread进入、退出对象监视器( Monitor, 就是锁在Hotspot虚拟机中的专门的名字 )来实现对方法、同步块的同步的; 编译后, 同步块的入口和出口分别有 `monitorenter`,`monitorexit` 指令，进入 Monitor 后线程具有排他性从而达到了同一时刻只能一个线程访问的目的。

`javap -c <class name>` 查看编译后信息, 可以看到在同步块的入口和出口分别有 monitorenter,monitorexit 指令。

```java
public class AccountSync implements Runnable {
    static AccountSync instance = new AccountSync();
    static volatile int i = 0;

    @Override
    public void run() {
        for (int j = 0; j < 10000000; j++) {
            /**
             * synchronized的作用是实现线程间的同步,对同步的代码加锁,使得每一次都只能有一个线程进入同步块从而保证线程间的安全性.
             *
             * 每次 thread 进入前, 需要先请求 instance 的锁, 如果当前有其他thread 持有instance的锁, 这个thread就等待
             */
            synchronized (instance) {
                i++;
            }
        }
    }
    public static void main(String args[]) throws InterruptedException {
        Thread thread1 = new Thread(instance);// 这里 thread1, thread2 都使用同一个 runnable 接口实例, 这样才能保证两个thread 关注同一个对象, 获取和释放的是同一个对象锁
        Thread thread2 = new Thread(instance);

        /**
        * wrong! 使用了不同的 runnable 接口实例, thread1,2关注的是不同的对象锁
        *
        * 如何改进? 将同步代码块搬到 静态方法中进行同步. 这样,即使两个 thread 指向不同的 runnable 对象, 但是同步块请求的是 "当前类" 的锁(唯一), 不是实例的锁, 仍旧可以保证同步
        */
        // Thread thread1 = new Thread(new AccountSync());
        // Thread thread2 = new Thread(new AccountSync());

        thread1.start();
        thread2.start();

        thread1.join();
        thread2.join();

        System.out.println(i);
    }
}

```

等效的 代码:

```java
public class AccountingVol implements Runnable {
    static AccountingVol instance = new AccountingVol();
    static volatile int i = 0;

    /**
    // thread 再进入这个实例方法前, 需要获取当前对象实例的锁, 也就是 instance 对象
    */
    public static void increase() {
        i++;
    }

    @Override
    public void run() {
        for (int j = 0; j < 10000000; j++) {
            increase();
        }
    }

    public static void main(String args[]) throws InterruptedException {
        Thread thread1 = new Thread(instance);
        Thread thread2 = new Thread(instance);

        thread1.start();
        thread2.start();

        thread1.join();
        thread2.join();

        System.out.println(i);
    }

}

```

# jdk中非线程安全的类库造成的隐蔽错误

## 先看一个隐蔽错误的例子-int型溢出

典型如 int 型变量溢出

```java
int v1 = 1023434540;
int v2 = 1448547380;
int v3 = (v1 + v2)/2;
System.out.println("v3: " + v3);// v3: -911492688

```

## 并发下的ArrayList

```java
public class ArrayListMultiThread {
    static ArrayList<Integer> arrayList = new ArrayList<Integer>(10);

    public static class AddThread implements Runnable {

        @Override
        public void run() {
            for (int i = 0; i < 10000; i++) {
                arrayList.add(i);
            }
        }
    }
    /**
     * ArrayList是一个线程不安全的容器,多线程操作时可能有3中情况
     
     * - 不出现错误 - arraylist 最终大小20000, 并行程序有问题也不一定每次都暴露出来
     * - 会出现冲突,报错信息如下:
     * Exception in thread "Thread-1" java.lang.ArrayIndexOutOfBoundsException: 22
     * at java.util.ArrayList.add(ArrayList.java:441)
     * at chapter2.ArrayListMultiThread$AddThread.run(ArrayListMultiThread.java:27)
     * at java.lang.Thread.run(Thread.java:745)
     * 这是因为 ArrayList 在动态扩容时候, 内部一致性被破坏, 在没有锁保护的情况下, 被另外一个thread访问到这种不一致状态  导致出现越界问题
     *
     * - 出现错误的结果, 但是不报错 -  这是由于2个thread同时对 array list同一个位置进行赋值
     *
     * Vector是一个线程安全的容器,可以代替ArrayList
     */
    public static void main(String args[]) throws InterruptedException {
        Thread thread1 = new Thread(new AddThread());
        Thread thread2 = new Thread(new AddThread());

        thread1.start();
        thread2.start();

        thread1.join();
        thread2.join();

        System.out.println(arrayList.size());
    }

}

```

## 并发下的Hashmap

```java
public class HashMapMultiThread {
    static Map<String, String> map = new HashMap<String, String>();

    public static class AddThread implements Runnable {

        int start = 0;

        public AddThread(int start) {
            this.start = start;
        }

        @Override
        public void run() {
            for (int i = start; i < 100000; i += 2) {
                map.put(Integer.toString(i), Integer.toBinaryString(i));
            }
        }
    }

    /**
     *  HashMap是一个线程不安全的容器,多线程操作时会出现冲突
     *
     *  jdk7下,可能会导致电脑死机,jdk8中问题已修复
     *
     * cpu使用率100%, 死循环
     *
     * 由于多线程冲突, hashmap.put() 时， 内部的链表成环了, 一迭代就成了死循环
     * @param args
     * @throws InterruptedException
     */
    public static void main(String args[]) throws InterruptedException {
        Thread thread1 = new Thread(new HashMapMultiThread.AddThread(0));
        Thread thread2 = new Thread(new HashMapMultiThread.AddThread(1));
        thread1.start();
        thread2.start();
        thread1.join();
        thread2.join();
        System.out.println(map.size());
    }
}

```

## 错误的使用锁

前面提到, 两个 thread 关注不同的 对象锁, 仍旧会造成线程安全问题

看看另外的一个错误案例

```java
public class BadLockOnInteger implements Runnable {

    public static Integer i = 0;
    static BadLockOnInteger instance = new BadLockOnInteger();

    @Override
    public void run() {
        for (int j = 0; j < 1000000; j++) {
            synchronized (i) {// 这里同步的并不是同一个对象,
                            //正确做法应该是 synchronized (instance)
                i++;// 实际执行 i=Integer.valueOf(i.intValue() + 1) // 实际上是创建了一个新的对象 i' , 
                      // 2个thread 不一定能够看到同一个 i, 因为 i 指向的对象一直在变化, 2个thread每次加锁都加在了不同的 i 对象上了
            }
        }
    }

    /**
     * 得到的结果并不是2000000,在多线程的操作中出现了错误
     *
     * @param args
     * @throws InterruptedException
     */
    public static void main(String args[]) throws InterruptedException {
        Thread thread1 = new Thread(instance);
        Thread thread2 = new Thread(instance);
        thread1.start();
        thread2.start();
        thread1.join();
        thread2.join();
        System.out.println(i);
    }
}
```

# concurrent包

## 怎么进行同步控制

### 重入锁-锁的公平性

- 重入锁可以替代 synchronized

- 借助 `java.util.concurrent.locks.ReentrantLock` 实现

- 提供新特性 - 对于 处理死锁, 线程饥饿 有奇效

  - "中断响应": 使用synchronized 加锁, 如果一个thread在等待, 那么结果只有2个, 要么一直等待, 要么获取锁执行后顺利执行. 重入锁提供了第三种行为: 这个thread可以中断, 取消对锁的请求

  - "锁申请等待限时": thread 等待另一个 thread释放锁, 超过一段时间, 那么放弃获取锁

  - "公平锁": 通过 synchronized 产生的锁, 是不公平锁, `public ReentrantLock(boolean fair)` 可以设置重入锁是否公平, 默认是 false;

```java
public class ReenterLock implements Runnable {
    public static ReentrantLock lock = new ReentrantLock();
    public static int i = 0;

    /**
    手动指定何时进入退出锁
    */
    @Override
    public void run() {
        for (int j = 0; j < 1000000; j++) {
            lock.lock();// 进入, 获得锁, 如果锁被占用, 等待
            // lock.lock() // 可多次进入 - 所以谓之 "重入锁"
            try {
                i++;
            } finally {
                lock.unlock();// 退出, 释放锁
                // lock.unlock() // 进入几次, 必须退出几次, 如果退出次数比进入次数多, 
                                // 则抛异常, 反之如果少, 则thread 仍然持有锁
            }

        }
    }

    public static void main(String args[]) throws InterruptedException {
        ReenterLock reenterLock = new ReenterLock();
        Thread thread1 = new Thread(reenterLock);
        Thread thread2 = new Thread(reenterLock);

        thread1.start();
        thread2.start();

        thread1.join();
        thread2.join();

        System.out.println(i);
    }

}

////////////////// 中断响应 ////////////////////////

public class IntLock implements Runnable {
    public static ReentrantLock lock1 = new ReentrantLock();
    public static ReentrantLock lock2 = new ReentrantLock();

    int lock; // 控制锁申请顺序, 构造死锁

    public IntLock(int lock) {
        this.lock = lock;
    }

    @Override
    public void run() {
        try {
            if (lock == 1) {
                lock1.lockInterruptibly();// 获取锁, 但优先响应中断 
                                    // 这种方式获得的重入锁使得 thread 在等待获取锁的过程中, 可对中断进行响应
                Thread.sleep(500);
                lock2.lockInterruptibly();
            } else {
                lock2.lockInterruptibly();
                Thread.sleep(500);
                lock1.lockInterruptibly();
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            if (lock1.isHeldByCurrentThread()) {
                lock1.unlock();
            }
            if (lock2.isHeldByCurrentThread()) {
                lock2.unlock();
            }
            System.out.println(Thread.currentThread().getId() + "线程退出");
        }

    }

    /**
     */
    public static void main(String args[]) throws InterruptedException {
        IntLock r1 = new IntLock(1);
        IntLock r2 = new IntLock(2);

        Thread thread1 = new Thread(r1);
        Thread thread2 = new Thread(r2);

        // 死锁
        thread1.start();
        thread2.start();

        Thread.sleep(1000);// main thread 休眠

        thread2.interrupt(); // thread2中断, thread2会放弃对 lock1 的申请, 同时释放 lock2, 那么 thread1可以得到lock2, 得以顺利执行
                            // 最终 两个 thread 双双顺利退出

    }
}

/////////////////// 锁请求等待限时 ////////////////////////////////

public class TimeLock implements Runnable {
    public static ReentrantLock lock = new ReentrantLock();

    @Override
    public void run() {
        try {
            if (lock.tryLock(5, TimeUnit.SECONDS)) { // 获取锁, 等待时间 5, 单位: 秒
                                  // 不带参数de lock.tryLock() 如果锁未被占用则申请成功, return true, 
                                  // 如果正在被占用, 则不等待立即 return false; 
                                  // 可以循环 tryLock(), 循环尝试获取锁, 这样不会引起死锁
                System.out.println(Thread.currentThread().getName());
                System.out.println("get lock success");
                Thread.sleep(6000);
            } else {
                System.out.println(Thread.currentThread().getName());
                System.out.println("get lock failed");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            if (lock.isHeldByCurrentThread()) {
                lock.unlock();
            }
        }
    }

    /**
     */
    public static void main(String args[]) {
        TimeLock timeLock = new TimeLock();
        Thread thread1 = new Thread(timeLock);
        Thread thread2 = new Thread(timeLock);

        thread1.start();
        thread2.start();
    }
}

///////////////////// 循环 tryLock() ////////////////////////////////

public class TryLock implements Runnable {
    public static ReentrantLock lock1 = new ReentrantLock();
    public static ReentrantLock lock2 = new ReentrantLock();
    int lock;

    public TryLock(int lock) {
        this.lock = lock;
    }

    @Override
    public void run() {
        if (lock == 1) {
            while (true) {
                if (lock1.tryLock()) {
                    try {
                        try {
                            Thread.sleep(500);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                        if (lock2.tryLock()) {
                            try {
                                System.out.println(Thread.currentThread().getId() + ":My Job done;");
                                return;
                            } finally {
                                lock2.unlock();
                            }
                        }
                    } finally {
                        lock1.unlock();
                    }
                }
            }
        } else {
            while (true) {
                if (lock2.tryLock()) {
                    try {
                        try {
                            Thread.sleep(500);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                        if (lock1.tryLock()) {
                            try {
                                System.out.println(Thread.currentThread().getId() + ":My Job done;");
                                return;
                            } finally {
                                lock1.unlock();
                            }
                        }
                    } finally {
                        lock2.unlock();
                    }
                }
            }

        }
    }

    /**
     * 上面代码中采用了非常容易死锁的加锁顺序,导致thread1和thread2由于锁的竞争而互相等待从而引起死锁
     *
     * 使用了tryLock后,线程不会一直等待而是不停的尝试去获得锁资源,只需要等待一定的时间,线程最终会获得所需要的资源
     *
     * @param args
     */
    public static void main(String args[]) {
        TryLock r1 = new TryLock(1);
        TryLock r2 = new TryLock(2);
        Thread thread1 = new Thread(r1);
        Thread thread2 = new Thread(r2);

        thread1.start();
        thread2.start();
    }

}

/////////////////////// 公平锁 //////////////////////////////////////

public class FairLock implements Runnable {

    public static ReentrantLock fairLock = new ReentrantLock(true);//设置true指定锁是公平的,也可以不设置
    //public static ReentrantLock unfairLock = new ReentrantLock();

    @Override
    public void run() {
        while (true) {
            try {
                fairLock.lock();
                // unfairLock.lock();
                System.out.println(Thread.currentThread().getName() + "获得锁");
            } finally {
                fairLock.unlock();
                // unfairLock.unlock();
            }
        }
    }

    /**
     * 公平锁的一个特点是:不会产生饥饿现象,只要排队最终都会得到资源.
     * <p/>
     * 但是实现公平锁要求系统维护一个有序队列,因此公平锁的实现成本较高,性能相对低下.
     *
     * @param args
     */
    public static void main(String args[]) {
        FairLock r1 = new FairLock();
        Thread thread1 = new Thread(r1, "Thread_t1");
        Thread thread2 = new Thread(r1, "Thread_t2");
        Thread thread3 = new Thread(r1, "Thread_t3");

        thread1.start();
        thread2.start();
        thread3.start();
    }

}

```

### Condition-重入锁好搭档

Condition的强大之处在于它可以为多个线程间建立不同的Condition， 使用synchronized/wait()只有一个阻塞队列，notifyAll会唤起所有阻塞队列下的线程，而使用lock/condition，可以实现多个阻塞队列，signalAll只会唤起某个阻塞队列下的阻塞线程 (典型使用就是 实现阻塞队列, 需要两个 condition, 分别作为 producer 的 condition 和 consumer 的 condition)

```java
public class ReenterLockCondition implements Runnable {
    public static ReentrantLock lock = new ReentrantLock();
    public static Condition condition = lock.newCondition();

    @Override
    public void run() {

        try {
            lock.lock();
            condition.await();// 当前thread 在 condition 对象上等待 // 类似 Object.wait()
                      // 当执行 await()时候, 要求 当前thread 持有 condition 相关的锁
                      // 调用 await() 完成后, 当前线程休眠, 会释放当前锁
            System.out.println("Thread is going on");
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }

    }

    public static void main(String args[]) throws InterruptedException {
        ReenterLockCondition reenterLockCondition = new ReenterLockCondition();
        Thread thread1 = new Thread(reenterLockCondition);
        thread1.start();
        Thread.sleep(2000);
        lock.lock(); // 首先获取condition相关的锁
        condition.signal();// 唤醒 condition 的等待队列中的 一个 thread // 类似 Object.notify()
                    // 一旦被唤醒, 这个thread会尝试重新获得 condition 绑定的 重入锁, 获取
                    // 成功才会正常继续执行, 因此 这里的 main thread 需要释放 lock, 让渡给唤醒的thread
        lock.unlock();
    }
}

```

### semaphore-信号量-多个线程同时访问

信号量 是一个语言无关的概念, check here: {% post_link cs-note 📚 cs-note %}#信号量

无论是内部锁(synchronized) 还是 重入锁(ReentrantLock) 同一时刻只允许一个 thread访问一个资源; 

semaphore(信号量) 可以指定多个thread, 同时访问一个资源

```java
public class SemapDemo implements Runnable {
    final Semaphore semp = new Semaphore(5); // 声明了包含 5 个 permission 的 semaphore
                                        // 即: 同时能申请 5 个许可; 当每个 thread 每次只申请一个许可, 则 这个 semaphore 同时允许 5 个thread 访问某个资源

    @Override
    public void run() {
        // 同时允许 5 个 thread 进入
        try {
            semp.acquire(); // 尝试获取一个permission, 如果无法获取, 等待直到有另外的 thread 释放 permission 或者 current thread 被 interrupt
            Thread.sleep(2000);
            System.out.println(Thread.currentThread().getId() + ":done!");
            semp.release(); // 释放
                        // 如果发生信号量泄漏(申请了但没有释放), 那么可以进入临界区的 thread将越来越少, 直到所有的thread均无法访问
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    /**
     * 总共20个线程,系统会以5个线程一组为单位,依次执行并输出
     *
     * @param args
     */
    public static void main(String args[]) {
        ExecutorService executorService = Executors.newFixedThreadPool(20);
        final SemapDemo demo = new SemapDemo();
        for (int i = 0; i < 20; i++) {
            executorService.submit(demo);
        }
    }
}

```

### ReadWriteLock-读写锁

- 读写锁 - 对于两个thread, "读读操作"之间的关系 是 非阻塞的, "读写操作", "写写操作" 是阻塞的

- 如果系统中 write 操作 << read操作, 则读写锁可以发挥最大功效

```java
public class ReadWriteLockDemo {
    private static Lock lock = new ReentrantLock(); // 普通重入锁

    private static ReentrantReadWriteLock reentrantReadWriteLock = new ReentrantReadWriteLock();// 读写锁
    private static Lock readLock = reentrantReadWriteLock.readLock();
    private static Lock writeLock = reentrantReadWriteLock.writeLock();
    
    private int value;

    public Object handleRead(Lock lock) throws InterruptedException {
        try {
            lock.lock();
            Thread.sleep(1000);//模拟读操作的耗时
            System.out.println("读操作:" + value);
            return value;
        } finally {
            lock.unlock();
        }
    }

    public void handleWrite(Lock lock, int index) throws InterruptedException {
        try {
            lock.lock();
            Thread.sleep(1000);//模拟写操作的耗时
            System.out.println("写操作:" + value);
            value = index;
        } finally {
            lock.unlock();
        }
    }

    public static void main(String args[]) {
        final ReadWriteLockDemo demo = new ReadWriteLockDemo();

        Runnable readRunnable = new Runnable() {
            @Override
            public void run() {
                //分别使用两种锁来运行,性能差别很直观的就体现出来,
                // 使用读写锁后读操作可以并行,节省了大量时间
                try {
                    demo.handleRead(readLock);
                    //demo.handleRead(lock);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

            }
        };

        Runnable writeRunnable = new Runnable() {
            @Override
            public void run() {
                //分别使用两种锁来运行,性能差别很直观的就体现出来
                try {
                    demo.handleWrite(writeLock, new Random().nextInt(100));
                    //demo.handleWrite(lock, new Random().nextInt(100));
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

            }
        };
        for (int i = 0; i < 18; i++) {
            new Thread(readRunnable).start();// 开启 18 个 read thread
                              // read thread 会 完全并行, 这里耗时可忽略不计
        }
        for (int i = 18; i < 20; i++) {
            new Thread(writeRunnable).start();// 开启 2 个 write thread
                                            // 主要是写操作耗时, 整个程序耗时 2 s
        }
    }
}

```

### CountDownLatch-倒计时器

意为 "倒计时栅栏"

使得某个 thread  在执行中碰到  `countdownlatch.wait()`, 则等待, 等到倒计时结束(即计数量归零)才开始执行;

![alt](Snipaste_2018-11-20_19-59-17.png)

```java
public class CountDownLatchDemo implements Runnable {
    static final CountDownLatch end = new CountDownLatch(10);// 创建倒计时器实例 计数量为10
                                        // 这表示需要有 10 个 thread 完成任务, 被 end倒计时器 设置为等待 的 thread才能开始执行
    static final CountDownLatchDemo demo = new CountDownLatchDemo();

    @Override
    public void run() {

        try {
            Thread.sleep(new Random().nextInt(3) * 1000);// 模拟检查任务耗时
            System.out.println("check complete");
            end.countDown(); // 通知 end, 有一个 thread 完成了任务, 计数量可以 -1 了
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    public static void main(String args[]) throws InterruptedException {
        ExecutorService executorService = Executors.newFixedThreadPool(10);
        for (int i = 0; i < 10; i++) {
            executorService.submit(demo);
        }
        end.await();// 使得 main thread 等待所有的 10 个 thread 完成 才开始继续执行
        System.out.println("Fire!");// 发射火箭
        executorService.shutdown();
    }
}

```

### CyclicBarrier-循环栅栏

每调用一次 `cyclicBarrier.await()` 就进入新的计量循环(进入新一轮等待)

示意图: from [实战Java高并发设计]

![alt](Snipaste_2018-11-20_19-57-31.png)

```java
public class CyclicBarrierDemo {
    public static class Soldier implements Runnable {
        private String soldier;
        private final CyclicBarrier cyclicBarrier;

        public Soldier(CyclicBarrier cyclicBarrier, String soldier) {
            this.soldier = soldier;
            this.cyclicBarrier = cyclicBarrier;
        }

        @Override
        public void run() {
            try {
                cyclicBarrier.await();// 等待, 开始第一次计量, 直到所有的 Soldier 线程 启动完毕, 才开始继续执行
                doWork();
                cyclicBarrier.await();// 再次等待, 开始第二次计量
            } catch (InterruptedException e) {
                e.printStackTrace();
            } catch (BrokenBarrierException e) {
                e.printStackTrace();
            }

        }

        void doWork() {
            try {
                Thread.sleep(Math.abs(new Random().nextInt() % 10000));
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(soldier + ":任务完成");
        }
    }

    public static class BarrierRun implements Runnable {

        boolean flag;
        int N;

        public BarrierRun(boolean flag, int N) {
            this.flag = flag;
            this.N = N;
        }

        @Override
        public void run() {
            if (flag) {// 第二次计量,进入这个分支
                System.out.println("司令:[士兵" + N + "个,任务完成!");
            } else {
                System.out.println("司令:[士兵" + N + "个,集合完毕!");// 第一次计量, 进入这个分支
                flag = true;
            }
        }
    }


    public static void main(String args[]) {
        final int N = 10;// 计数量
        Thread[] allSoldier = new Thread[N];
        boolean flag = false;
        CyclicBarrier cyclicBarrier = new CyclicBarrier(N, new BarrierRun(flag, N));// barrier实例, 计数量达到指标时候则执行 BarrierRun 线程
        System.out.println("集合队伍!");
        for (int i = 0; i < N; i++) {
            System.out.println("士兵" + i + "报道!");
            allSoldier[i] = new Thread(new Soldier(cyclicBarrier, "士兵" + i));
            allSoldier[i].start();// 启动一个 soldier 线程
                          // 当第十个启动完毕, 一次 计数量 完成, 执行一次 BarrierRun 线程, 计数量
                          // 恢复 10, 进行第二次 计量
        }
    }
}

```

### LockSupport-线程阻塞工具类

使得thread阻塞

和 Thread.suspend() 相比更好, 因为使用suspend()会挂起thread, 锁不会释放, 而且由于指令重排, resume()在suspend()前执行会造成thread永远挂起

和 Object.wait() 相比更好, 因为不需要先获得某个对象的锁

原理是使用了类似信号量的机制 (存疑)

```java
public class LockSupportDemo {
    public static Object u = new Object();
    static ChangeObjectThread t1 = new ChangeObjectThread("t1");
    static ChangeObjectThread t2 = new ChangeObjectThread("t2");

    public static class ChangeObjectThread extends Thread {
        public ChangeObjectThread(String name) {
            super.setName(name);
        }

        public void run() {
            synchronized (u) {
                System.out.println("in " + getName());
                LockSupport.park();
            }
        }
    }


    public static void main(String args[]) throws InterruptedException {
        t1.start();
        Thread.sleep(100);
        t2.start();
        LockSupport.unpark(t1);
        LockSupport.unpark(t2);
        t1.join();
        t2.join();
    }
}

```

## 线程复用-线程池

类似数据路连接池

jdk提供一套 Executor 框架, 本质就是一个 thread pool

### Executor的api

#### 创建线程池

Executors.newCachedThreadPool()：无限线程池。
Executors.newFixedThreadPool(nThreads)：创建固定大小的线程池。
Executors.newSingleThreadExecutor()：创建单个线程的线程池。
还有一个任务调度线程池

底层调用的还是 ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, RejectedExecutionHandler handler)

```java
public class ThreadPoolDemo {
    public static class MyTask implements Runnable {

        @Override
        public void run() {
            System.out.println(System.currentTimeMillis() + "Thread ID:" + Thread.currentThread().getId());

            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String args[]) {
        MyTask myTask = new MyTask();
        int poolSize = Runtime.getRuntime().availableProcessors();// 获取CPU核心,
        System.out.println("poolSize=" + poolSize);
        ExecutorService executorService = Executors.newFixedThreadPool(poolSize);
        for (int i = 0; i < 20; i++) {
            executorService.submit(myTask);
        }
    }
}

```

#### 怎么确定池子的容量

![alt](Snipaste_2018-11-21_14-22-24.png)

IO 密集型任务：由于线程并不是一直在运行，所以可以尽可能的多配置线程，比如 CPU 个数 * 2
CPU 密集型任务（大量复杂的运算）应当分配较少的线程，比如 CPU 个数相当的大小。

#### 线程池的几个状态

thread pool 的几个状态:

RUNNING 自然是运行状态，指可以接受任务执行队列里的任务
SHUTDOWN 指调用了 shutdown() 方法，不再接受新任务了，但是队列里的任务得执行完毕。
STOP 指调用了 shutdownNow() 方法，不再接受新任务，同时抛弃阻塞队列里的所有任务并中断所有正在执行任务。
TIDYING 所有任务都执行完毕，在调用 shutdown()/shutdownNow() 中都会尝试更新为这个状态。
TERMINATED 终止状态，当执行 terminated() 后会更新为这个状态。

#### 关闭线程池

无非就是两个方法 shutdown()/shutdownNow(), 但是区别大大的

shutdown() 执行后停止接受新任务，会把队列的任务执行完毕。
shutdownNow() 也是停止接受新任务，但会中断所有的任务，将线程池状态变为 stop。

```java
 long start = System.currentTimeMillis();
for (int i = 0; i <= 5; i++) {
    pool.execute(new Job());
}

pool.shutdown();// 关闭

//每隔一秒钟检查一次是否执行完毕（是否状态为 TERMINATED），当从 while 循环退出时就表明线程池已经完全终止了。
while (!pool.awaitTermination(1, TimeUnit.SECONDS)) {
    LOGGER.info("线程还在执行。。。");
}
long end = System.currentTimeMillis();
LOGGER.info("一共处理了【{}】", (end - start));

```

优雅的关闭:

```java

// 优雅一点关闭，先通知，再等待，最后强制关闭
this.executor.shutdown();
try {
    this.executor.awaitTermination(10, TimeUnit.SECONDS);
} catch (InterruptedException e) {
}
this.executor.shutdownNow();


```

### ThreadPoolExecutor-自定义线程池

#### 核心 api

ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, RejectedExecutionHandler handler)

```java
ThreadPoolExecutor(int corePoolSize, // 线程数
        int maximumPoolSize, // 最大线程
        long keepAliveTime, TimeUnit unit, // 容量超过 coreSize, 空闲多长时间会被销毁
        BlockingQueue<Runnable> workQueue,// 任务队列
        ThreadFactory factory, // 线程工厂, 使用默认即可
        RejectedExecutionHandler handler)// 拒绝策略

- 对于 workQueue, 允许多种, 先进先出

    - SynchronousQueue(直接提交队列) - 没有容量, 每个添加都要等待一个删除. 对于新的任务会尝试创建新的thread, 直到达到 maximumPoolSize之后执行拒绝策略

    - ArrayBlockingQueue(有界队列) - 带有容量. 只在容量满载时将线程数提升到 coreSize之上

    - LinkedBlockingQueue(无界队列) - 类似有界队列, 容量无限(也可以定义为指定容量), 直到资源耗尽也不会执行拒绝策略

    - PriorityBlockingQueue(优先任务队列) - 带有容量, 有自定义顺序

    - 自定义

- 对于 factory, 只有一个方法, 可以实现自己的逻辑来创建 thread

- 对于拒绝策略 handler, 有多种

    - AbortPolicy - 直接抛异常

    - CallerRunsPolicy - 直接在当前thread种, 运行当前被丢弃的任务

    - DiscardOledestPolicy - 丢弃最老的请求

    - DiscardPolicy - 直接丢弃, 不报异常

    - 自定义 RejectionExecutionHandler 接口

```

#### 阻塞队列 BlockingQueue

api 提供三类操作:

- 抛异常 (如果操作不能马上进行，则抛出异常)

    add(o), remove(o), element(o)

- 返回特定值 (如果操作不能马上进行，将会返回一个特殊的值，一般是true或者false)

    offer(o), poll(o), peek(o)

    offer(o, timeot, unit), poll(o, timeout, unit) 如果操作不能马上进行，操作会被阻塞指定的时间，如果指定时间没执行，则返回一个特殊值，一般是true或者false

- 阻塞 (如果操作不能马上进行，操作会被阻塞)

    put(o), take(o)


扩展 ThreadPoolExecutor

```java
public class ExtThreadPool {

    public static class MyTask implements Runnable {
        public String name;

        public MyTask(String name) {
            this.name = name;
        }

        @Override
        public void run() {
            System.out.println("正在执行:Thread ID:" + Thread.currentThread().getId() + ",Task Name:" + name);

            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }


    public static void main(String args[]) throws InterruptedException {
        ExecutorService executorService = new ThreadPoolExecutor(5, 5, 0L, TimeUnit.MILLISECONDS, new LinkedBlockingDeque<Runnable>()) {
            protected void beforeExecute(Thread t, Runnable r) {
                System.out.println("准备执行:" + ((MyTask) r).name);
            }

            protected void afterExecute(Thread t, Runnable r) {
                System.out.println("执行完成:" + ((MyTask) r).name);
            }

            protected void terminated() {
                System.out.println("线程池退出!");
            }
        };

        for (int i = 0; i < 5; i++) {
            MyTask task = new MyTask("TASK-GEYM-" + i);
            executorService.execute(task);// 另一种不同于 submit() 的提交任务方式
            Thread.sleep(10);
        }
        executorService.shutdown();// 温和的关闭
    }
}

```


#### 自定义拒绝策略

```java
public class RejectThreadPoolDemo {
    public static class MyTask implements Runnable {

        @Override
        public void run() {
            System.out.println(System.currentTimeMillis() + ":Thread ID:" + Thread.currentThread().getId());
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }


    public static void main(String args[]) throws InterruptedException {
        MyTask myTask = new MyTask();

        ExecutorService executorService = new ThreadPoolExecutor(
                 5, 5,                          // 5常驻线程, 5最大线程 , 等价于固定大小线程池
                 0L, TimeUnit.SECONDS, 
                 new LinkedBlockingDeque<Runnable>(10),// 容量10
                  Executors.defaultThreadFactory()
                , new RejectedExecutionHandler() { // 自定义策略
            @Override
            public void rejectedExecution(Runnable r, ThreadPoolExecutor executor) {
                System.out.println(r.toString() + " is discard");
            }
        });

        for (int i = 0; i < 100; i++) {
            executorService.submit(myTask);
            Thread.sleep(10);
        }
    }
}

```

#### 自定义线程池名称

```java
// 命名线程工厂
    static class NamedThreadFactory implements ThreadFactory {
        private static final AtomicInteger poolNumber = new AtomicInteger(1);
        private final ThreadGroup group;
        private final AtomicInteger threadNumber = new AtomicInteger(1);
        private final String namePrefix;

        NamedThreadFactory(String name) {

            SecurityManager s = System.getSecurityManager();
            group = (s != null) ? s.getThreadGroup() : Thread.currentThread().getThreadGroup();
            if (null == name || name.isEmpty()) {
                name = "pool";
            }

            namePrefix = name + "-" + poolNumber.getAndIncrement() + "-thread-";
        }

        public Thread newThread(Runnable r) {
            Thread t = new Thread(group, r, namePrefix + threadNumber.getAndIncrement(), 0);
            if (t.isDaemon())
                t.setDaemon(false);
            if (t.getPriority() != Thread.NORM_PRIORITY)
                t.setPriority(Thread.NORM_PRIORITY);
            return t;
        }
    }
// 使用:

ThreadPoolExecutor executorOne = new ThreadPoolExecutor(5, 5, 1, TimeUnit.MINUTES,
            new LinkedBlockingQueue<>(), new NamedThreadFactory("ASYN-ACCEPT-POOL"));
```

#### 自定义线程 名称

```java
ThreadFactory factory = new ThreadFactory() {

    // 自定义 thread name
    final AtomicInteger seq = new AtomicInteger();
    @Override
    public Thread newThread(Runnable r) {
        Thread t = new Thread(r);
        t.setName("rpc-" + seq.getAndIncrement());
        return t;
    }

};
// 闲置时间超过30秒的线程自动销毁
this.executor = new ThreadPoolExecutor(1, coreSize,
        30, TimeUnit.SECONDS, queue, factory,
        new ThreadPoolExecutor.CallerRunsPolicy());
```

### 无锁安全的队列

ConcurrentLinkedQueue

TODO

https://blog.csdn.net/qq_38293564/article/details/80798310

### 拓展线程池-在线程池中寻找堆栈

线程池会吃掉线程抛出的异常, 看看这个例子

```java
public class NoTraceDivTaskDemo {
    public static class DivTask implements Runnable {
        int a, b;

        public DivTask(int a, int b) {
            this.a = a;
            this.b = b;
        }

        @Override
        public void run() {
            double re = a / b;
            System.out.println(re);
        }
    }

    public static void main(String args[]) {
        ThreadPoolExecutor poolExecutor = new ThreadPoolExecutor(0, Integer.MAX_VALUE, 0L, TimeUnit.SECONDS, new SynchronousQueue<Runnable>());
        for (int i = 0; i < 5; i++) {
            /*poolExecutor.submit(new DivTask(100, i));*/ //没有报错提示,  但是 i=0 时明显除数不能为0应该报错;
            poolExecutor.execute(new DivTask(100, i));//通过execute()提交任务, 有报错提示
        }
        /**
        输出了结果, 但是只有4行结果, 正常的话应该有5行
        */
    }

}

```

更好的改进方法是直接拓展 线程池

```java
public class TraceThreadPoolExecutor extends ThreadPoolExecutor {
    
    public TraceThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue) {
        super(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue);
    }

    // 也可以覆盖 submit方法
    public void execute(Runnable task) {
        super.execute(wrap(task, clientTrace(), Thread.currentThread().getName()));
    }

    private Runnable wrap(final Runnable task, final Exception clientTrace, String name) {
        return new Runnable() {
            @Override
            public void run() {
                try {
                    task.run();
                } catch (Exception e) {
                    clientTrace.printStackTrace();
                    throw e;
                }
            }
        };
    }

    private Exception clientTrace() {
        return new Exception("Client stack trace");// 保存着提交任务的线程的堆栈信息
    }
}

// 测试
public class TraceDivTaskDemo {
    public static void main(String args[]) {
        ThreadPoolExecutor threadPoolExecutor = new TraceThreadPoolExecutor(0, Integer.MAX_VALUE, 0L, TimeUnit.SECONDS, new SynchronousQueue<Runnable>());
        for (int i = 0; i < 5; i++) {
            threadPoolExecutor.execute(new NoTraceDivTaskDemo.DivTask(100, i));
        }
    }
}

```

### Fork-Join框架

类似 MapReduce

jdk提供一个 `ForkJoinPool` 线程池, 对于 fork() 并不急于分叉出新的 线程, 而是将新任务提交给 ForkJoinPool ; 这样, 提交的任务数可能远远大于实际线程数, 每个线程拥有一个任务队列容纳任务, 每次从队列头部获取任务执行, 如果这个 线程将队列任务全部完成, 会去帮助其他线程 ---- 从其他 thread 的任务队列尾部获取任务执行

ForkJoinPool 提供一个方法 submit, 提交 `ForkJoinTask` 任务(这种任务支持 form()分解和 join()等待), 它包含2个子类 `RecursiveAction` (无返回值任务) 和 `RecursiveTask` (有返回值任务)

```java
public class CountTask extends RecursiveTask {
    private static final int THRESHOLD = 10000;// 超过这个分界点, 就使用 任务分解

    private long start;
    private long end;

    public CountTask(long start, long end) {
        this.start = start;
        this.end = end;
    }


    @Override
    protected Long compute() {
        long sum = 0;
        boolean canCompute = (end - start) < THRESHOLD;
        if (canCompute) {
            for (long i = start; i <= end; i++) {
                sum += i;
            }
        } else {
            long step = (start + end) / 100;

            ArrayList<CountTask> subTasks = new ArrayList<CountTask>();
            long pos = start;

            for (int i = 0; i < 100; i++) {
                long lastOne = pos + step;
                if (lastOne > end) {
                    lastOne = end;
                }
                CountTask subTask = new CountTask(pos, lastOne);
                pos += step + 1;
                subTasks.add(subTask);
                subTask.fork();// 提交子任务, 类似递归调用, 若调用层次太深, 可能会栈溢出
            }

            for (CountTask t : subTasks) {
                sum += (Long) t.join();// 等待所有子任务结束, 求和
            }
        }


        return sum;
    }


    public static void main(String args[]) {
        ForkJoinPool forkJoinPool = new ForkJoinPool();
        CountTask task = new CountTask(0, 200000L);
        ForkJoinTask<Long> result = forkJoinPool.submit(task);

        long res = 0;
        try {
            res = result.get();
            System.out.println("sum=" + res);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}

```

### springboot中使用线程池

### hystrix-线程池隔离

## jdk中的并发容器

### 整体介绍

- ConcurrentHashMap - 安全的 hashmap

- CopyOnWriteArrayList - 安全的 arraylist, 适合读多写少的场景, 性能远远好于 Vector

- ConcurrentLinkedQueue - 安全的LinkedList, 链表实现

- BlockingQueue - 阻塞队列, 作为数据共享的通道

- ConcurrentSkipListMap - 跳表的实现

另外 , Collections 工具类可以将任意集合包装为安全的集合, 当然性能就差远了

### ConcurrentLinkedQueue原理

## aqs 无锁实现

https://www.jianshu.com/p/7b0e11a1e605 //todo

# java8中的新特性

check {% post_link java-note 📚 java-note %}中的 #java8

# 怎么优化锁

## 提高锁性能的原则

### 减少锁持有时间-和锁粗化-需要权衡

减少锁持有时间就是说，将被同步方法拆分， 只锁定原方法的一部分

比如 一个同步方法可以分成三个小方法, 只有一个或两个小方法需要同步, 则同步块可以缩小到小方法上, 这样整个大方法持有锁的时间就缩短了

但是减少是有限度的, 如果频繁进行锁请求和释放, 也会消耗系统资源, 所以 jvm 在碰到这种情况时会合并锁请求, like this: (但是我们应该有意识地主动优化)

```java
private static void test() {
    synchronized (lock) {
        // do sth
    }

    doSth();

    synchronized(lock) {
        // do sth
    }

}

// 会被jvm优化成

private static void test() {
    synchronized (lock) {
        // do sth
        doSth();
        // do sth
    }

}

/////////////////////////////////////

for (int i = 0; i < 100; i++) {
    synchronized(lock) {
        do sth
    }
}
// 应该改成这样
synchronized(lock) {
    for (int i = 0; i < 100; i++) {
        do sth
    }
}

```

### 减小锁粒度

缩小锁定对象范围, 通过拆分被锁定对象的数据结构， 缩小锁定范围

典型如 ConcurrentHashMap 的实现

要保证hashmap的线程安全, 最粗略的方法就是对整个 hashmap加锁, 但是这样做就锁粒度太大;

实际上的实现是这样的, ConcurrentHashMap 内部进一步细分为多个 hashmap,称之为 "segment", 一个 ConcurrentHashMap默认分成了 16 个 segment, 也就是说有可能同时支持 16 个 thread 并行插入元素; 如果想向 map 中添加 entry, 并不是将整个 map 加锁, 而是根据 entry 计算得出的 hashcode 得到 该entry 会被存入 哪个 segment, 然后对该 segment 加锁, 随之完成 put()操作, 

但是减小锁粒度会引入新的问题: 要访问全局信息, 这是需要取得全局锁, 消耗的资源比较多; 如 ConcurrentHashMap 中想要取得 size 信息时候, 需要获取 所有 segment 的锁, 得到 size 后, 释放所有锁;

### 读写分离锁代替独占锁

使用读写分离锁

这样当两个thread都是read操作时， 不会阻塞

### 锁分离

典型如 LinkedBlockingQueue 的实现, 提供 put() 从队列尾部添加, take() 从队列头部获取, 这两个操作都需要获取队列的锁

如果使用 "独占锁", 那么put 和 take 就不能实现并行执行;

jdk真正的实现是, 使用两个不同的锁 takeLock 和 putLock (两个不同的 ReentrantLock), 分离 put, take 操作. 因此take和put 操作不存在锁竞争关系, 只有当个2个thread 是 put-put 关系, 或者是 take-take关系时候才有竞争关系

## jvm是怎么对锁优化的

这里所说的锁都是jvm的优化手段, 不是真正的语言上的锁

下面几个jvm的优化手段是循序渐进的, 一个thread真正被挂起之前, 会依次尝试如下的手段

### 使锁进入偏向模式-偏向锁

使用jvm参数: `-XX:+UsedBiasedLocking` 开启偏向锁

核心思想是: jvm开启偏向锁的情况下, 一个thread一旦获取锁, 这个锁就进入偏向模式(即这个锁偏向这个thread), 当同一个 thread 再次请求该锁时, 无需再次做任何同步操作, 这样就`节省了很多锁申请的操作`

场景: 对于锁竞争很少的场景很有效 - 因为 同一个 thread 可能多次请求相同的锁; 如果时锁竞争激烈的场合, 每次都是不同的 thread请求相同的锁, 偏向模式就失效了

### 使用轻量级锁

https://blog.csdn.net/shuaicenglou3032/article/details/77170068

轻量级锁主要思想是: 提供了一种新的判断 thread是否持有锁的判断方式 - 如果加锁对象的头部存在指针指向某个 thread , 则这个thread 拥有这个锁

当 偏向锁 失效, jvm 不会立即挂起 thread, 而是会判断thread是否拥有 轻量级锁, 如果拥有, 则顺利进入 临界区, 如果没有拥有, 则膨胀为重量级锁

### 自旋锁

锁膨胀后, jvm还会做最后的努力: 自旋锁 -     jvm会让 即将被挂起的thread 做几个空循环, 在经过几次循环后, 如果这个锁能够被获取了, 就顺利进入临界区, 如果还是不能获取   , 才会真正将 thread 挂起

### 锁消除

`-XX:+EliminateLocks` 打开jvm锁消除功能

jvm会在编译时, 对上下文扫描, 去除掉不可能存在资源竞争的锁, 比如: Vector 作为方法内的局部变量, 实在线程栈上分配空间的, 属于thread私有, 不会出现线程安全问题, 所以 Vecotr 中的 锁是没有必要的, jvm会在编译时消除这些锁

逃逸分析 
`-XX:+DoEscapeAnalysis`

## ThreadLocal

### threadlocal概念

根据字面意思理解就是 "线程本地的", 每个线程拥有自己的变量, 其他线程无法访问;

多线程开发中，要考虑好线程安全问题(同步, 互斥..)。而因为这些需求就出现了以下三种来实现线程安全的手段

*   互斥同步(阻塞同步)

    就是通过加锁(就是悲观锁)来实现对临界资源的访问限制。加锁方式有Synchorized和Lock

*   非阻塞同步

    * 非阻塞同步属于乐观锁机制。典型的实现方式就是 CAS 操作, 详细见: [1](https://www.ibm.com/developerworks/cn/java/j-jtp04186/), [2](http://lxy2330.iteye.com/blog/1153135), [3](http://blog.csdn.net/aesop_wubo/article/details/7537960)

    * CAS(compare and swap), 先比较, 如果变量没有变化, 再交换赋值, java.util.concurrent.atomic包中几乎大部分类都采用了CAS操作

        * CAS有三个操作参数：内存地址，期望值，要修改的新值，当期望值和内存当中的值进行比较不相等的时候，表示内存中的值已经被别线程改动过，这时候失败返回，只有相等时，才会将内存中的值改为新的值，并返回成功

        * ABA问题: ABA问题是指在CAS操作时，其他线程将变量值A改为了B，但是又被改回了A，等到本线程使用期望值A与当前变量进行比较时，发现变量A没有变，于是CAS就将A值进行了交换操作

        * ABA问题的解决思路是: 加版本号，每次变量更新的时候把变量的版本号加1，那么A-B-A就会变成A1-B2-A3，只要变量被某一线程修改过，改变量对应的版本号就会发生递增变化，从而解决了ABA问题(在JDK的java.util.concurrent.atomic包中提供了AtomicStampedReference来解决ABA问题)

    * 乐观锁和悲观锁: 是两种思想, 乐观锁就是对加锁对象比较乐观，假定它不存在很多并发更新请求。悲观锁反之. 可以有很多实现
        
        * 数据库中: 悲观锁就是for update; 乐观锁就是加 version字段

        * JDK方面：悲观锁就是sync; 乐观锁就是原子类（内部使用CAS实现）

*   无同步方案(维护自己的变量)

    要保证线程安全，并不是一定就需要同步，同步只是保证共享数据征用时正确性的手段，如果一个方法本来就不涉及共享数据，那它就不需要任何同步措施去保证正确性。由此引出 ThreadLocal

### threadLocal 示例

```java
/**
 * ThreadLocal  
 *
 * @version 0.1
 * @author xy
 * @date 2017年9月2日 下午5:15:54

结果:
init value: false
thread1 value: true
thread2 value: null
 */
@Data
public class Client {
    
    private ThreadLocal<Boolean> threadLocal = new ThreadLocal<Boolean>();
    
    public static void main(String[] args) {
        Client client = new Client();
        final ThreadLocal<Boolean> threadLocal = client.getThreadLocal();
        threadLocal.set(false);
        
        System.out.println("init value: " + threadLocal.get());
        
        new Thread(new Runnable() {
            
            public void run() {
                threadLocal.set(true);
                System.out.println("thread1 value: " + threadLocal.get());
            }
        }).start();;
        
        new Thread(new Runnable() {
            
            public void run() {
                System.out.println("thread2 value: " + threadLocal.get());
            }
        }).start();;
    }
}

```

可以看出不同的thread访问同一个threadLocal, 结果不同;


### 再看一个问题例子以及改进

为每个线程准备一份变量

下面这个demo会异常:

```java
public class ParseDateDemo {
    private static final SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    //private static ThreadLocal<SimpleDateFormat> threadLocal = new ThreadLocal<SimpleDateFormat>(); // 更好的方法

    public static class ParseDate implements Runnable {
        int i = 0;

        public ParseDate(int i) {
            this.i = i;
        }

        @Override
        public void run() {
            try {
                Date date = sdf.parse("2017-05-06 12:33:" + i % 60);// 这个方法不是安全的, 一个可行的改进方法是前后加锁,

               //但是更好的方法是为每个 thread准备一个 SimpleDateFormat
                // if (threadLocal.get() == null) {
                //     threadLocal.set(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"));
                // }
                // Date date = threadLocal.get().parse("2017-05-06 12:33:" + i % 60);

                System.out.println(i + ":" + date);
            } catch (ParseException e) {
                e.printStackTrace();
            }
        }
    }

    public static void main(String args[]) {
        ExecutorService executorService = Executors.newFixedThreadPool(10);
        for (int i = 0; i < 1000; i++) {
            executorService.execute(new ParseDate(i));
        }
    }
}

```

### 原理

Thread 内部有个成员 `ThreadLocalMap`, 类似 hashmap, key为 "当前线程访问的 threadlocal对象", value 为 "threadLocal保存的值", 

`ThreadLocalMap` 保存了当前线程的所有 "线程局部变量" - 也就是说, 所谓的 "线程局部变量" 是维护在 Thread内部的, 只要这个线程不退出, "线程局部变量" 将永远存在. Thread 退出, 将进行 ThreadLocalMap的清理.

使用完毕的 ThreadLocal, 记得进行清理 - 因为线程可能永远不会退出(比如使用了固定容量的线程池, tomcat 就使用了线程池处理请求), 线程内部将永远维护 "线程局部变量", 所以应该手动清除

### 看源码

- 一个 Thread 中存在一个 ThreadLocalMap field，ThreadLocalMap中的 key 是一个 Weakreference， 指向ThreadLocal，value代表通过Thread Local存储的变量。

- ThreadLocalMap 可以存储多个 key(即ThreadLocal)， 也就是说同一个 Thread 可以设置多个 “本地线程变量”

- 为什么不同的线程访问同一个ThreadLocal，得到的确是不同的数值, 因为不同的thread内部维护了不同的threadLocalMap; "本地线程变量" 实际上还是保存在 Thread 自身内部

先看看整体的示意图

![alt](Snipaste_2018-11-12_15-39-10.png) [check here](https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=%E6%9C%AA%E5%91%BD%E5%90%8D%E8%A1%A8%E5%8D%95.xml#R7Vpbb6M4FP41SDsPG4HNJTy2aTr7sJVGSqXdeXTBBauAI%2BM0yfz6tcHmYtNOt0PTTmaSh9iH42P7Oxd%2FQBy4Kg%2BfGdrmNzTFhQPc9ODAKwcAzw2B%2BJGSYysJQr8VZIykSqkXbMg3rEcq6Y6kuB4pckoLTrZjYUKrCid8JEOM0f1Y7Z4W41m3KMOWYJOgwpb%2BQ1Ket9IliHr5X5hkuZ7ZC%2BP2yh1KHjJGd5WazwHwvvm0l0ukbamN1jlK6X4ggmsHrhilvG2VhxUuJLYatnbc9RNXu3UzXPGXDIDtgEdU7LBecbMuftRYNLvBUt914OU%2BJxxvtiiRV%2FfC%2B0KW87IQPU80lTnMOD48uSSv26gIIExLzNlRqKgBvoJGhY6nodr3juh08oETQKyESDk%2F60z3AIiGwmAaj2BmPEr6iO6akVKZ4Zp8G%2FYpR3zQF2mEh32ckmFXRftAMgPcHb4a76WNtzeFtxcFP453aOF9mzOMUgt1sUE%2BhtYEc0tJxZulBJdOcCUkaMdp3dYWOQAVJKtEu8D30pREjYiUv1BiTqXvauFKUmW3snP1pz8PxoEBsWtDHExF9AwBHU0EdFjwdqeVaGey3YL%2BNxVw3IgQVhrC9lDpvHxixX04EffeG3llaXmlxvwPnpPaASu5xsPh07kXHt%2F9fqHvitHQAXPU%2BfiJutOkgHdekW4CHds4x28U5zp%2FnjtPE1qSRIFUc0YfOsoFZCRW6YUkdKJ7V9Dk4TYn1dgT97Ti16gkhdzeqjEG3A2qRCq5NxulsFHz9ZOsaEFZswLoNh8Z5zv22CRYT2VwajFFA3dBTRHLMB9ktu2K71R6LWO4QJw8jmeccoCa4YuMut7TYGmklG%2F4sKY7lmA1akgLDUN%2BZBiChqF2y5Yh4Sp0HKiprHhywdBcMBzRVdFoLfbB1mH6svjz7PhbL52LtXN57axjJ45k%2B6yyHbrBGFIvttJ9kj7Pke7AgvsBCwshKuV%2Bq7t622zSPbGoXc9ZudmDhpsnqvqb0RfPvm0c85fj8fjL8RcQgNPxF89%2FjsDYh%2BxPHeu%2BjjeNNLSRfjMG84InAh%2BVwQh42fFf6Qh34bqRlnyVHl%2FEQafyBTMigMFslBz%2Fl%2Fm0Tvoo1AdGr6U%2BRlrDMFzAsP%2F6JyFCMJyZCE08Bzh3IqTZiIbU909HhOz7SwvcQV2gW1zZZSFFdd7lci0CjWv9ilZYy66JXEV76lXpoDcqBXZZsmrIMpLf16c%2FeM%2F090PjRuK16Q%2BhcaqHhqG5Et54JgjcaNaE11H8m4qfnIoDeEIqDuw73N9%2BPpGfw2gRnNDT9s31z3OmdFTUXSz9eMhEZ2ahg7eo73EMmVU9jl53DAn%2BughMUwvfDYHx0m2u48gHxnzQBT9yIIlu%2Fxa7Ve%2F%2FKgDX%2FwE%3D)

从ThreadLocal#set进入, 看看实现:

片段1

```java
     // 将指定的值设置进当前线程的threadLocal中
    public void set(T value) {
        Thread t = Thread.currentThread();
        // 根据当前线程获取 ThreadLocalMap, 可
        //以猜测 ThreadLocalMap大概是一个存储键值对的容器
        //片段2是ThreadLocalMap的类结构
        ThreadLocalMap map = getMap(t);
        if (map != null)
            // 如果获取到了, 设置一个键值对, key 为当前线程所访问的 ThreadLocal,
            // value 为 存储的值. 从这里进入, 看看ThreadLocalMap#set 的实现, 见 片段3; 
            map.set(this, value);
        else 
            // 如果没有获取到map, 初始化一个: 根据 当前
            //线程 (第一个参数)获取到线程内部的threadLocalMap, 将键值对[threadLocal, value] 存入
            createMap(t, value);
    }

```

片段2: ThreadLocalMap的类结构

```java
static class ThreadLocalMap {
        // Entry类继承了WeakReference<ThreadLocal<?>>，即每个Entry对象都
        //有一个ThreadLocal的弱引用作为key，这是为了防止内存泄露。一旦
        //线程结束，外部的 threadLocal 引用被设置为 null , 则
        // 这里的key 看似仍旧指向 ThreadLocal 对象, 实际上这个指向无
        //效, ThreadLocal直接变为一个不可达的对象，这个Entry就可以被回收了。
        static class Entry extends WeakReference<ThreadLocal<?>> {
            /** The value associated with this ThreadLocal. */
            Object value;
            Entry(ThreadLocal<?> k, Object v) {
                super(k);// 将 k 作为 "弱引用"
                value = v;
            }
        }
        // ThreadLocalMap 的初始容量，必须为2的倍数
        private static final int INITIAL_CAPACITY = 16;

        // resized时候需要的table
        private Entry[] table;

        // table中的entry个数
        private int size = 0;

        // 扩容数值
        private int threshold; // Default to 0

         /**
         * Construct a new map initially containing (firstKey, firstValue).
         * ThreadLocalMaps are constructed lazily, so we only create
         * one when we have at least one entry to put in it.
         构造函数, 
         */
        ThreadLocalMap(ThreadLocal<?> firstKey, Object firstValue) {
            // 初始容量为16的entry
            table = new Entry[INITIAL_CAPACITY];
            // 计算 firstKey 的hashcode
            int i = firstKey.threadLocalHashCode & (INITIAL_CAPACITY - 1);
            // 存储到 table 中
            table[i] = new Entry(firstKey, firstValue);
            size = 1;
            setThreshold(INITIAL_CAPACITY);
        }

```

片段3: ThreadLocalMap#set 的实现

```java
private void set(ThreadLocal<?> key, Object value) {
    Entry[] tab = table;
    int len = tab.length;
    int i = key.threadLocalHashCode & (len-1);

    // 遍历 table, 为table 中的每个匹配上key(threadLocal) 的entry设置value
    for (Entry e = tab[i];
            e != null;
            e = tab[i = nextIndex(i, len)]) {
        ThreadLocal<?> k = e.get();

        if (k == key) {
            e.value = value;
            return;
        }

        if (k == null) {
            replaceStaleEntry(key, value, i);
            return;
        }
    }

    tab[i] = new Entry(key, value);
    int sz = ++size;
    if (!cleanSomeSlots(i, sz) && sz >= threshold) // cleanSomeSlots的作用主要是清理无用的entry
        rehash();
}

// 如果Entry在存放过程中冲突了，调用nextIndex来处理，如下所示。hashmap中对冲突的处理
private static int nextIndex(int i, int len) {
    return ((i + 1 < len) ? i + 1 : 0);
}
```

看看 ThreadLocal#get 的实现

code4
```java
public T get() {
    Thread t = Thread.currentThread();
    // 获取 thread对应的ThreadLocalMap
    ThreadLocalMap map = getMap(t);
    if (map != null) {
        ThreadLocalMap.Entry e = map.getEntry(this);
        if (e != null) {
            @SuppressWarnings("unchecked")
            T result = (T)e.value;
            return result;
        }
    }
    return setInitialValue(); // 如果map 为null, 跟进看看, 见code5
}
```

code5

```java
private T setInitialValue() {
    T value = initialValue(); 
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null)
        map.set(this, value);
    else
        createMap(t, value);
    return value; //  返回值见initialValue()
    }

protected T initialValue() {
        return null;// 如果不设置ThreadLocal的数值，默认就是null
    }

```

## weakReference

weakReference 并不能解决多线程的安全问题, 这里只是 ThreadLocal的源码中, 跟到内部类 ThreadLocalMap 里, 在类结构里发现了 WeakReference

### 什么是WeakReference

jdk 文档: https://docs.oracle.com/javase/7/docs/api/java/lang/ref/WeakReference.html

一个弱引用(WeakReference) 指向一个对象 o, GC可以随时回收对象 o, 不管这个指向是否存在

假设垃圾收集器在某个时间点决定一个对象是弱可达的(weakly reachable)（也就是说当前指向它的全都是弱引用），这时垃圾收集器会清除所有指向该对象的弱引用，然后垃圾收集器会把这个弱可达对象标记为可终结(finalizable)的，这样它们随后就会被回收。而这些没用的弱引用对象会加入 引用队列(Reference Queue), 方便集中处理

### 为什使用

现在有这样一个需求:

类 User 被标记为 final, 不可拓展, 但是现在需要为每个 user增加一个属性: 编号, 可行的方案是 使用 HashMap<User, Integer>, 但是可能会造成内存泄漏; 因为如果某个user被删除了, 内存中也就不再需要这个user指向的对象了, 指定 user=null (实际上可能不必这样显式的指定), 但是还有引用指向User对象: hashMap中的引用. 所以要想真正的回收User对象, 仅仅把它的强引用赋值为null是不够的，还要把相应的条目从HashMap中移除. 回收一个对象需要清除两处引用, 这不符合人的直觉.
这时 弱引用 就有用处了: 用一个指向User对象的弱引用作为hashMap的key就行了

此外, app使用缓存(cache)时, 由于cache的对象正是程序运行需要的, 那么只要程序正在运行, cache中的引用就不会被回收, 如果需要回收, 必须交给编写者手动回收, 而这显然背离了GC的本质, 这时也可以引入弱引用;

### 怎么使用

首先看一个简单的示例

User实体类
```java
/**
 * User
 *
 * @version 0.1
 * @author xy
 * @date 2017年9月2日
 */
@Data
public class User {

    private String password;
    private String name;
}
```

测试类
```java
/**
 * weakReference client
 *
 * @version 0.1
 * @author xy
结果:
通过weakReference获取到User: User(password=null, name=null)
通过weakReference获取到User: User(password=null, name=null)
通过weakReference获取到User: User(password=null, name=null)
...
...
User 被回收了

 */
public class Client {
 
    public static void main(String[] args) {
        User user = new User();
        WeakReference<User> weakReferenceOfUser = new WeakReference<User>(user);
        
        while(true) { // 进入循环, 强引用user已经没有被使用了, 相当于已经失效了; 如果在这个循环体中使用到了 强引用user, 则User对象不会被回收, 循环永远不会结束
            User userFromWeakReference = weakReferenceOfUser.get();
            if (userFromWeakReference != null) {
                System.out.println("通过weakReference获取到User: " + userFromWeakReference);
            }
            else {
                System.out.println("User 被回收了");
                break;
            }
        }
    }
}

```

WeakReference的一个特点是它何时被回收是不可确定的, 因为这是由GC运行的不确定性所确定的. 所以, 一般用weak reference引用的对象是有价值被cache, 而且很容易被重新被构建, 且很消耗内存的对象

### 四种引用

实际上 Java中存在四种引用:

*   Strong Reference

    最常用, 通过new创建对象返回的引用

*   SoftReference

    SoftReference 和 Weak Reference 的区别在于: 软引用指向的对象只有在内存不足时才会被回收, 而弱引用无论内存是否充足都会被回收

*   Weak Reference

*   PhantomRefrence

    最"弱"的引用, 通过它连对象都无法获取, 唯一作用就是通过指向一个对象, 当这个对象被回收, 虚引用会被加入到 引用队列(ReferenceQueue), 用于记录指向的对象已经被回收;

### ReferenceQueue

在weak reference指向的对象被回收后, weak reference本身也就没有用了. ReferenceQueue可以保存这些所指向的对象已经被回收的reference. 用法见第二个构造函数

```java
WeakReference(T referent) //创建一个指向给定对象的弱引用
WeakReference(T referent, ReferenceQueue<? super T> q) //创建一个指向给定对象并且登记到给定引用队列的弱引用
```

###  WeakHashMap & TreadLocalMap

两者原理有相似, 放在这里比较;

TreadLocalMap是ThreadLocal的一个内部类, 源码分析过了, 见 {% post_link ThreadLocal理解 这里 %};

WeakHashMap 里的entry可能会被GC自动删除如果没有额外的强引用指向entry中的key，即使我没有调用remove()或者clear()方法; 这个特点特别适用于需要缓存的场景. 在使用缓存时肯定不能缓存所有对象；对象缓存命中最好，但缓存没命中也不会造成错误，因为可以通过计算重新得到

## 不加锁-乐观锁

### 怎么理解不加锁的策略

加锁是一种 "悲观策略", 这种策略总是假设临界区的 并发冲突一定会存在, 所以一定要加锁

"无锁" 是 一种 "乐观策略", 这种策略假设临界区不会有冲突发生, 自然无需加锁 , 也就无阻塞(天然免疫死锁问题), 但是万一碰到了冲突怎么办呢 -- 使用 CAS(compare and swap)技术, 思路是: 比较然后交换, 一旦检测到冲突, 就循环重试直到没有冲突为止

优点: 📌 没有锁竞争带来的系统开销; 📌 没有线程切换带来的开销

具体算法: 一个 cas操作 包含三个参数 CAS(v, e, n), v-待更新的变量, e-变量v的预期值, n-新的值. 只有当 v == e, 才会设置 v = n, 如果 v != e, 说明更新操作期间有其他线程修改了 v 的值, 则当前thread 的 cas操作失败, 但是失败线程不会被挂起, 只是被告知失败, 为了保证成功, 一般都会写一个死循环, 只得到成功后跳出; 在大部分封装好的类库种 只需要传入 v, n即可

硬件上, 大部分 cpu 都支持 原子化的 cas操作了

### AtomicInteger-无锁安全整数

相比于 Integer, AtomicInteger 是可变的

```java
public class AtomicIntegerDemo {
    static AtomicInteger i = new AtomicInteger();

    public static class AddThread implements Runnable {

        @Override
        public void run() {
            for (int j = 0; j < 10000; j++) {
                i.incrementAndGet();// 使用 cas操作 将自己加1
            }
        }
    }

    public static void main(String args[]) throws InterruptedException {

        Thread[] threads = new Thread[10];

        for (int j = 0; j < 10; j++) {
            threads[j] = new Thread(new AddThread());
        }

        for (int j = 0; j < 10; j++) {
            threads[j].start();
        }
        for (int j = 0; j < 10; j++) {
            threads[j].join();
        }

        System.out.println(i);// 正常输出 100000
    }
}

```

### Unsafe类-Java中的指针

### AtomicReference-无锁的对象引用

类似 atomicinteger, 但是更具普遍性, AtomicInteger 是对 Intege对象的封装, AtomicReference 是对普通的对象引用的封装

线程判断被修改对象是否可以被写入, 条件是对象的当前值是否等于期望值(即修改操作前获取的对象值)

但是考虑一个场景: 当线程在判断是否可以写入时, 另外的某个thread将对象连续修改了2次, 第一次修改为某个值, 第二次又改回来了, 就像什么也没有修改一样, 那么当前线程就判断可以正常写入 - bug由此产生.看下面的demo: 给某个账户里充值代金券, 且只充值1次

```java
public class AtomicRefrenceDemo {

    public static void main(String args[]) {
        final AtomicReference<Integer> money = new AtomicReference<Integer>();
        money.set(19);

        // 开启100个赠送代金卷进程
        for (int i = 0; i < 100; i++) {
            new Thread() {
                public void run() {
                    while (true) {
                        Integer m = money.get();
                        if (m < 20) { // 小于 20 则充值
                            if (money.compareAndSet(m, m + 20)) {
                                System.out.println("余额小于20元,充值成功,余额:" + money.get() + "元");
                                break;
                            }
                        } else {
                            System.out.println("余额大于20,无需充值");
                            break;
                        }
                    }
                }
            }.start();
        }
        // 开启一个消费进程, 但是内部循环100次
        /**
        - 考虑这种场景: 账户已经被充值了一次, 这时有一个充值thread访问
            了账户, 正准备判断是否可以正常写入, 消费thread 连续消费
            了20元, 赠送thread会误认为这个账户没有被赠送过 , 会再次赠送

        - 原因: 账户余额被多个thread反复修改, 最终恢复成原样, 使得 cas 操作
            无法准确判断是否可以写入; ------ AtomicStampedReference 可以很好的解决
        */
        new Thread() {
            public void run() {
                for (int i = 0; i < 100; i++) {

                    while (true) {
                        Integer m = money.get();
                        if (m > 10) {
                            System.out.println("金额大于10元");
                            if (money.compareAndSet(m, m - 10)) {
                                System.out.println("成功消费10元,余额:" + money.get() + "元");
                                break;
                            }
                        } else {
                            System.out.println("没有足够的金额");
                            break;
                        }
                    }
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }.start();
    }
}

```

### AtomicStampedReference-带时间戳的引用

AtomicStampedReference 内部不经维护了对象值, 还由一个时间戳, 当对象值被修改, 时间戳也相应变化, cas操作判断是否可以写入时, 对象值和时间戳都必须符合期望值, 才会写入成功

```java
public class AtomicStampedReferenceDemo {
    static AtomicStampedReference<Integer> money = new AtomicStampedReference<Integer>(19, 0);// 0 表示初始时间戳

    public static void main(String args[]) {

        for (int i = 0; i < 100; i++) {
            final int timestap = money.getStamp();// 获得时间戳
            new Thread() {
                public void run() {
                    while (true) {
                        Integer m = money.getReference();
                        if (m < 20) {
                            // 每次写入, 时间戳也要修改
                            if (money.compareAndSet(m, m + 20, timestap, timestap + 1)) {
                                System.out.println("余额小于20元,充值成功,余额:" + money.getReference() + "元");
                                break;
                            }
                        } else {
                            System.out.println("余额大于20,无需充值");
                            break;
                        }
                    }
                }
            }.start();
        }

        new Thread() {
            public void run() {
                for (int i = 0; i < 100; i++) {

                    while (true) {
                        int timestap = money.getStamp();
                        Integer m = money.getReference();
                        if (m > 10) {
                            System.out.println("金额大于10元");
                            if (money.compareAndSet(m, m - 10, timestap, timestap + 1)) {
                                System.out.println("成功消费10元,余额:" + money.getReference() + "元");
                                break;
                            }
                        } else {
                            System.out.println("没有足够的金额");
                            break;
                        }
                    }
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }.start();

    }
}

```

### AtomicIntegerArray-无锁的数组

```java
public class AtomicInterArrayDemo {
    // 声明数组, 长度10
    static AtomicIntegerArray array = new AtomicIntegerArray(10);

    public static class AddThread implements Runnable {
        @Override
        public void run() {
            for (int i = 0; i < 10000; i++) {
                array.getAndIncrement(i % array.length());//每个元素各加1000次
            }
        }
    }

    public static void main(String args[]) throws InterruptedException {
        Thread[] threads = new Thread[10];
        for (int i = 0; i < 10; i++) {
            threads[i] = new Thread(new AddThread());
        }

        for (int i = 0; i < 10; i++) {
            threads[i].start();
        }
        for (int i = 0; i < 10; i++) {
            threads[i].join();
            System.out.println(array);
        }
    }
}

```

### AtomicIntegerFieldUpdater-升级普通变量支持原子操作

Integer, Long, 普通对象引用 都有自己的 updater, 用来支持 cas操作

- updater 只能修改自己可见范围内的变量, 比如 下面的 score 如果时 private 的, 就不行
- 变量必须是 volatile 的, 这是为了保证变量总是能被读取到
- updater 不支持 static 的变量 - 这是因为 cas操作会通过对象实例中的偏移量直接进行赋值, static的变量不再对象实例中

```java
// 模拟选举
public class AtomicIntegerFieldUpdaterDemo {
    public static class Candidate {// 候选人
        int id;
        volatile int score;// 并非线程安全
    }

    // 针对 Candidate 类的 score 成员的  updater
    public final static AtomicIntegerFieldUpdater<Candidate> scoreUpdater = AtomicIntegerFieldUpdater.newUpdater(Candidate.class, "score");
    //
    public static AtomicInteger allScore = new AtomicInteger(0);// 这个变量用来验证实验结果

    public static void main(String args[]) throws InterruptedException {
        final Candidate stu = new Candidate();
        Thread[] t = new Thread[10000];

        for (int i = 0; i < 10000; i++) {
            t[i] = new Thread() {
                public void run() {
                    if (Math.random() > 0.4) {
                        scoreUpdater.incrementAndGet(stu);
                        allScore.incrementAndGet();
                    }
                }
            };
            t[i].start();
        }

        for (int i = 0; i < 10000; i++) {
            t[i].join();
        }
        System.out.println("score=" + stu.score);
        System.out.println("allScore=" + allScore);
    }


}

```

### SynchronousQueue实现

# 死锁

## 怎么打印线程堆栈信息

死循环、死锁、阻塞、页面打开慢等问题，打线程dump是最好的解决问题的途径。所谓线程dump也就是线程堆栈，获取到线程堆栈有两步：

（1）获取到线程的pid，可以通过使用jps命令，在Linux环境下还可以使用ps -ef | grep java

（2）打印线程堆栈，可以通过使用jstack pid命令，在Linux环境下还可以使用kill -3 pid

另外提一点，Thread类提供了一个getStackTrace()方法也可以用于获取线程堆栈。这是一个实例方法，

# 并发相关的模式和算法

## 单例模式的最佳实现方式

对于使用频率高的对象, 可以节省new操作耗费的时间, 同时节省更多内存, GC 压力也减轻了

最简单的单例如下: 没有任何加锁操作, 并发下表现会很好; 但是实例不是 "懒加载" 的, 在类被装载的时候, 实例就会被创建

```java
public class Singleton {
    private Singleton() {
        System.out.println("Singleton is create");
    }

    private static Singleton instance = new Singleton();

    public static Singleton getInstance() {
        return instance;
    }
}

```

懒加载, 但是加了锁， 并发表现不好

```java
/**
 * LazySingleton
 */
public class LazySingleton {

    private LazySingleton() {
        System.out.println("Singleton is create");
    }

    private static LazySingleton instance;

    public static synchronized LazySingleton getInstance() {
        if (instance == null) {
            instance = new LazySingleton();
        }
        return instance;
    }
}
```

最好的方式：懒加载， 不加锁

```java
public class Singleton {
    /**
     * 类级内部类，用于缓存类实例

     * 内部类的初始化方式保证了该类将在被调用时才会被装载，从而实现了延迟加载
     * instance 修饰为静态， 保证了只会实例化一次， 也就是单例

     * 同时由于instance采用静态初始化的方式，因此JVM能保证其线程安全性
     */
    private static class Instance {
        private static Singleton instance = new Singleton();
    }
 
    /**
     * 私有化构造方法，使外部无法通过构造方法构造除instance外的类实例
     * 从而达到单例模式控制类实例数目的目的
     */
    private Singleton() {
    }
 
    /**
     * 类实例的全局访问方法
     * 添加static关键词使得外部可以通过类名直接调用该方法获取类实例
     * @return 单例类实例
     */
    public static Singleton getInstance() {
        return Instance.instance;
    }
}

```

## 不变模式

不变模式天生支持多线程 - 核心思路: 一个对象一旦被创建, 则他的内部状态/数据永远不会改变, so 没有一个 thread 可以修改这个对象的状态, 针对不变对象的多线程操作无需同步

什么是不变对象:

- 去除掉 setter;
- 所有 field 私有
- final 修饰 对象的 class, 确保不会被继承
- 有一个构造方法

典型不变对象 -- String, Integer, Long

适用于：📌 创建后不再发生任何变化 📌 对象需要被共享， 多线程访问

一个典型的不变对象

```java
public final class Product {
    private final String no;
    private final String name;
    private final String price;

    public Product(String no, String name, String price) {
        super();
        this.no = no;
        this.name = name;
        this.price = price;
    }

    public String getNo() {
        return no;
    }

    public String getName() {
        return name;
    }

    public String getPrice() {
        return price;
    }
}

```

## 生产者-消费者模式

### 典型实现

```java
/**
 * Data
 */
public class Data {

  private final int intDdata;

  public Data(int value) {
    this.intDdata = value;
  }

  /**
   * @return the intDdata
   */
  public int getIntDdata() {
    return intDdata;
  }

  @Override
  public String toString() {
    return "data:[intData=]" + this.intDdata;
  }

}

/////////////////////////////////////////////
/**
 * Consumer
 */
public class Consumer implements Runnable {

  private BlockingQueue<Data> queue;

  public Consumer(BlockingQueue<Data> queue) {
    this.queue = queue;
  }

  @Override
  public void run() {
    System.out.println("start [consumer], id=" + Thread.currentThread().getId());

    try {
      while (true) {
        if (Thread.currentThread().isInterrupted()) {
          System.out.println("stop [consumer], id=" + Thread.currentThread().getId());
          break;
        }

        Data data = queue.take();
        if (null != data) {
          int re = data.getIntDdata() * data.getIntDdata();
          Thread.sleep(new Random().nextInt(1000));// simulate the time consumption

          System.out.println(MessageFormat.format("{0} * {1} = {2}", data.getIntDdata, data.getIntDdata(), re));
        }
      }
    } catch (Exception e) {
      e.printStackTrace();
      Thread.currentThread().interrupt();// 中断
    }
  }
}

/////////////////////////////////////

/**
 * Producer
 */
public class Producer implements Runnable {

  private volatile boolean running = true;
  private BlockingQueue<Data> queue;
  private AtomicInteger count = new AtomicInteger(0);

  public Producer(BlockingQueue<Data> queue) {
    this.queue = queue;
  }

  @Override
  public void run() {
    System.out.println("start [producer], id=" + Thread.currentThread().getId());

    try {
      while (running) {
        // 构造数据
        Thread.sleep(new Random().nextInt(1000));
        Data data = new Data(count.incrementAndGet());
  
        boolean ok = queue.offer(data, 2, TimeUnit.SECONDS);
        if (!ok) {
          System.err.println("failed to put data, data: " + data);
        }
      }
    } catch (Exception e) {
      e.printStackTrace();
      Thread.currentThread().interrupt();
    }

  }

  public void stop() {
    this.running = false;
  }
}

//////////////////////////////////

/**
 * Client
 */
public class Client {

  public static void main(String[] args) {
    LinkedBlockingQueue<Data> queue = new LinkedBlockingQueue<Data>();

    Consumer consumer1 = new Consumer(queue);
    Consumer consumer2 = new Consumer(queue);
    Consumer consumer3 = new Consumer(queue);

    Producer producer1 = new Producer(queue);
    Producer producer2 = new Producer(queue);
    Producer producer3 = new Producer(queue);

    ExecutorService pool = new ThreadPoolExecutor(6, 6, 0L, TimeUnit.SECONDS, Executors.defaultThreadFactory());
    pool.execute(producer1);
    pool.execute(producer2);
    pool.execute(producer3);
    pool.execute(consumer1);
    pool.execute(consumer2);
    pool.execute(consumer3);

    try {
      Thread.sleep(10*1000);
    } catch (Exception e) {
      e.printStackTrace();
    }
    producer1.stop();
    producer2.stop();
    producer3.stop();

    try {
      Thread.sleep(3*1000);
    } catch (Exception e) {
      e.printStackTrace();
    }

    pool.shutdown();
  }
}

```

### 无锁的实现

BlockingQueue 不是高性能的实现， 完全使用锁来实现线程的同步

ConcurrentLinkedQueue 才是高性能的实现， 完全使用 无锁的方式(CAS操作) 来实现线程同步

disruptor框架 - 无锁方式的队列

## Future模式

### 典型Future模式实现

核心思想是 - 异步调用

future模式流程图:

![alt](Snipaste_2018-11-23_17-43-53.png)

main() 方法调用耗时的子函数, 会立即返回一个 "契约"(相当于伪造的数据), 这时 main() 会接着往下执行, 未来当耗时的子函数执行完, main()才获得真正的结果

```java
// 核心 interface - 表示希望获取的数据
public interface Data {
    public String getResult();

}

// 真实数据
public class RealData implements Data {

    protected final String result;

    public RealData(String param) {
        //RealData的构造很慢,需要用户等待很久,这里用sleep模拟
        StringBuffer stringBuffer = new StringBuffer();
        for (int i = 0; i < 10; i++) {
            stringBuffer.append(param);
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        result = stringBuffer.toString();
    }

    @Override
    public String getResult() {
        return result;
    }
}

// 核心 伪造的数据 - 会立即返回给调用者
public class FutureData implements Data {

    protected RealData realData = null; // 真实数据
    protected boolean isReady = false; // 真实数据是否构造完毕

    public synchronized void setRealData(RealData realData) {
        if (isReady) {
            return;
        }
        this.realData = realData;
        isReady = true;
        notifyAll(); // 唤醒所有等待的 thread
    }


    @Override
    public synchronized String getResult() {
        while (!isReady) {
            try {
                wait(); // 如果 realData 还没有构造好, 线程等待
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        return realData.result;
    }
}

// 客户端
public class Client {
    public Data request(final String queryStr) {
        final FutureData futureData = new FutureData();
        
        new Thread() {
            public void run() {//RealData的构造很慢,所以在单独的线程中运行
                RealData realData = new RealData(queryStr);
                futureData.setRealData(realData);
            }
        }.start();

        return futureData; // 直接返回包装数据
    }
}

// 测试
public class Main {
    public static void main(String args[]) {
        Client client = new Client();
        //这里会立即返回,因为得到的是FutureData而不是RealData
        Data data = client.request("name");
        System.out.println("请求完毕");

        try {

            //这里用一个sleep代替了对其他业务逻辑的处理
            //在处理这些业务逻辑的过程中,RealData被创建,从而充分利用了等待时间
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        //使用真实的数据
        System.out.println("数据=" + data.getResult());
    }
}

```

### jdk中的future模式

jdk内部提供了一套完整的实现

类结构图:

![alt](Snipaste_2018-11-23_18-12-18.png)

其他一些功能性api:

![alt](Snipaste_2018-11-23_18-11-09.png)

```java
// Callable 接口 - 构造真实数据
public class RealData2 implements Callable<String> {
    private String data;

    public RealData2(String data) {
        this.data = data;
    }

    @Override
    public String call() throws Exception {
        StringBuffer stringBuffer = new StringBuffer();
        for (int i = 0; i < 10; i++) {
            stringBuffer.append(data);
            Thread.sleep(100);// 模拟耗时操作
        }

        return stringBuffer.toString();
    }
}

public class FutureMain {

    public static void main(String args[]) throws ExecutionException, InterruptedException {
        //构造FutureTask - 有返回值
        // new RealData2("a") 告诉了 FutureTask 真实数据如何产生
        FutureTask<String> futureTask = new FutureTask<String>(new RealData2("a"));

        ExecutorService executorService = Executors.newFixedThreadPool(1);

        //执行FutureTask,相当于前一个例子中的client.request("a")发送请求
        //在这里开启线程进行RealData的call()执行
        executorService.submit(futureTask);

        System.out.println("请求完毕");

        try {
            //这里依然可以做额外的数据操作,使用sleep代替其他业务逻辑的处理
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        //相当于前一个例子中的data.getResult(),取得call()方法的返回值
        //如果此时call()方法没有执行完成,则依然会等待
        System.out.println("数据=" + futureTask.get());
    }
}

```

## 并行流水线

并发算法适合分解各个子运算没有依赖的计算来利用多核优势

![alt](Snipaste_2018-11-23_18-23-14.png)

如果不幸碰到了相互依赖的子运算组成的计算, 可以使用 "并行流水线" 来利用多核优势 - 可以类比 cpu的流水线工作机制

```java
// 定义一个在线程之间数据传递的载体
public class Msg {

    public double i;
    public double j;
    public String orgStr = null;

}

// 加法 - 流水线中的一个节点
public class Plus implements Runnable {

    public static BlockingDeque<Msg> blockingDeque = new LinkedBlockingDeque<Msg>();

    @Override
    public void run() {
        while (true) {
            Msg msg = null;
            try {
                msg = blockingDeque.take();
                msg.i = msg.j + msg.i;
                Multiply.blockingDeque.add(msg);// 传递给乘法线程中的queue
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

// 乘法
public class Multiply implements Runnable {
    public static BlockingDeque<Msg> blockingDeque = new LinkedBlockingDeque<Msg>();

    @Override
    public void run() {
        while (true) {
            Msg msg = null;
            try {
                msg = blockingDeque.take();
                msg.i = msg.j * msg.i;
                Div.blockingDeque.add(msg);// 传递给 除法
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

// 除法
public class Div implements Runnable {
    public static BlockingDeque<Msg> blockingDeque = new LinkedBlockingDeque<Msg>();

    @Override
    public void run() {
        while (true) {
            Msg msg = null;
            try {
                msg = blockingDeque.take();
                msg.i = msg.i / 2;
                System.out.println(msg.orgStr + "=" + msg.i);// 输出最终结果
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

// 测试
public class PStreamMain {
    public static void main(String args[]) {

        new Thread(new Plus()).start();
        new Thread(new Multiply()).start();
        new Thread(new Div()).start();

        // 提交 100 万个计算请求
        for (int i = 0; i <= 1000; i++) {
            for (int j = 0; j <= 1000; j++) {
                Msg msg = new Msg();
                msg.i = i;
                msg.j = j;
                msg.orgStr = "((" + i + "+" + j + ")*" + i + ")/2";
                Plus.blockingDeque.add(msg);
            }
        }
    }
}

```

## 并行搜索

有序 - 二分法
无需 - 挨个查找, 更好的方法: 并行

```java
/**
 * Main
 */
public class Main {

    private static int[] arr;
    private static ExecutorService pool = Executors.newCachedThreadPool();
    private static final int thread_num = 2;
    private static AtomicInteger result = new AtomicInteger(-1);// 多个 thread 通过这个共享信息

    private static class SearchTask implements Callable<Integer> {

        private int begin;
        private int end;
        private int target;

        public SearchTask(int begin, int end, int target) {
            this.begin = begin;
            this.end = end;
            this.target = target;
        }

        @Override
        public Integer call() throws Exception {
            return search(target, begin, end);
        }

    }

    public static void main(String[] args) throws InterruptedException, ExecutionException {
        arr = new int[] {2, 4, 5, 9, 11, 80, 294305, 777, 83};
        int index = parallelSearch(777);
        System.out.println("search finished, index=" + index);
    }

    private static int parallelSearch(int target) throws InterruptedException, ExecutionException {
        ArrayList<Future<Integer>> re = new ArrayList<Future<Integer>>();

        int subArrSize = arr.length / (thread_num + 1);// 宁愿每个子数组size小一些, 数组个数多一些
        for (int i = 0; i < arr.length; i += subArrSize) {
            int end = i + subArrSize;
            if (end > arr.length) {// 可以去掉
                end = arr.length;
            }
            re.add(pool.submit(new SearchTask(i, end, target)));
        }

        for (Future<Integer> fu : re) {
            Integer i = fu.get();
            if (i >= 0) {
                return i;
            }
        }
        return -1;
    }

    private static int search(int target, int begin, int end) {
        if (result.get() >= 0) {// another thread should has finished the search
            return result.get();
        }

        for (int i = begin; i < end; i++) {
            if (arr[i] == target) {
                boolean ok = result.compareAndSet(-1, i);// get the index of target, and put it into result;
                if (ok) {
                    return i;
                }
                // 如果设置失败, 表示其他线程抢先找到了
                return result.get();
            }
        }
        // 遍历完还是没找到, 返回 -1
        return -1;
    }

}

```

## 并行排序

一般的排序都是串行的, 这里介绍并行的排序算法

### 改进冒泡排序-分离数据相关性-奇偶交换排序

对于冒泡排序中的某个元素, 它可能和前一个元素交换, 也可能同后一个元素交换, 整个过程是一个相互依赖的整体, 怎么分离成多个部分呢?

奇偶交换排序 - 将整个排序过程分为两部分: 奇交换 - 只比较奇数索引的元素和相邻的下一个元素; 偶交换 - 只比较偶数索引的元素及其相邻的下一个元素

奇偶排序示意图: from [here](https://blog.csdn.net/lemon_tree12138/article/details/50605563)

![alt](Snipaste_2018-11-24_12-03-11.png)
![alt](Snipaste_2018-11-24_12-04-55.png)

将整个比较分割为 独立的 奇阶段, 偶阶段. 在每个阶段内, 所有的比较交换没有相互依赖, so 每次比较都可以独立执行


```java
public class Main {

    private static int[] arr;

    public static void main(String[] args) {
        arr = new int[] { 763, 203, 2, 32, 8, 99, 455, 777, 343, 234 };
        bubbleSort(arr);
        
        for (int i : arr) {
            
            System.out.print(i+", ");//2, 8, 32, 99, 203, 234, 343, 455, 763, 777,
        }
    }

    //普通的冒泡排序:
    public static void bubbleSort(int[] arr) {
        for (int i = arr.length - 1; i > 0; i--) {
            for (int j = 0; j < i; j++) {
                if (arr[j] > arr[j + 1]) {
                    int tmp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = tmp;
                }
            }
        }
    }

    // 串行的奇偶冒泡排序
    public static void oddEvenSort(int[] arr) {
        boolean exch = true;// 本次循环是否进行了数据交换
        int startIndex = 0;// 1: 奇交换, 0: 偶交换

        while (exch || startIndex == 1) {// 如果上一次循环发生了交换 or 奇交换, 重复循环
                                    // 当没有元素交换, 且当前进行的是偶交换 - 退出循环
            exch = false;

            for (int i = startIndex; i < arr.length - 1; i += 2) {
                if (arr[i] > arr[i+1]) {
                    int tmp = arr[i];
                    arr[i] = arr[i+1];
                    arr[i+1] = tmp;

                    exch = true;
                }
            }

            if (startIndex == 0) {
                startIndex = 1;
            } else {
                startIndex = 0;
            }
        }
    }
}

```

### 改进插入排序-希尔排序

```java


```

## 网络nio



# 使用akka构建高并发程序

# 并行程序怎么调试
