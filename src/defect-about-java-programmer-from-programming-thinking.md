---
title: 标注编程随想的文章
date: 2017-09-23 20:02:09
tags: [java]
categories: java core
---

<div align="center">
来自: [编程随想](https://program-think.blogspot.com/2009/01/defect-of-java-beginner-0-overview.html); Java 程序猿的毛病
</div>


<!--more-->

# 对算法和数据结构不熟悉

★什么时候该用数组型容器、什么时候该用链表型容器？<font color="green"> > 前者适用于查询较多的场景增删特别慢因为涉及到数组创建复制, 后者适合增删多的场景</font>
★什么是散列函数？HashMap 的实现原理是什么？<font color="green"> > 散列函数 see [link](https://www.zhihu.com/question/26762707), hashmap通过哈希表实现, 博客内搜索 "java core Note" </font>
★什么是递归？如果你以前从来没写过递归函数，尝试着写一个（比如用递归函数进行目录树遍历）。<font color="green"> > 简单理解就是函数调用自身, 需要有结束条件, 不然就死循环了 </font>
★什么是算法复杂度？
★你是否理解空间换时间的思想？
★写一个针对整数数组的冒泡排序函数，看看你要修改几次才能跑通。
★写一个针对整数数组的二分查找函数，看看你要修改几次才能跑通。


# 离开开发框架活不了

对 Spring、Hibernate 等框架了如指掌；但如果给他一个简单需求，让他写一个脱离 Web 框架的独立 Application，他就不知所措了

★基于接口的继承和基于实现的继承各有什么优缺点？
★继承（包括 extend 和 implement）有什么【缺点】？
★多态（polymorphism）有什么【缺点】？
★为什么 Java 可以多继承 interface，而不可以多继承 class？<font color="green"> > 多继承class中有相同方法时应该选用哪个版本呢? 另外, java8开始已经允许接口里面有 default method了, 发现继承多个默认方法实现并且没有override时报错, 避免了集成多个class选择哪个版本的问题 </font>
★假如让你写一个小游戏（比如人机对战的五子棋），你会如何设计类结构？
★类结构设计时，如何考虑可扩展性？


# 编程习惯

## 随意地命名
    
反面: 使用单字母命名变量；使用一些没太大意义的变量名（例如 s1、s2、s3）；对同一个业务概念使用不同的术语/缩写（容易让读代码的人神经分裂）；使用拼音命名（如果你团队中有港台人士或者老外，就惨了）

## 习惯于代码的 copy & paste

反面：如果发现要写的某个函数和前几天写的某个函数差不多，就把原来的那个函数贴过来，然后稍微改几下，心中还暗喜：“又快速搞定了一个功能”....... 这种做法是代码臭味（借用《重构 - 改善既有代码的设计》的提法）的主要来源，导致代码可维护性大大下降。当你将来需要增加功能或修改 bug 的时候，要同时改动多个地方，而那时你估计已经想不起来这砣代码有几个克隆了

## Magic Number 满天飞

假设有个业务逻辑中需要进行10秒的超时等待，你会怎么写这个sleep语句？我估计大部分人不外乎下面三种写法:
1、直接写上 sleep(10*1000); 了事 ----最差, 缺乏可读性, 代码臭味
2、定义一个常量 TIMEOUT_XXX = 10*1000; 然后 sleep(TIMEOUT_XXX); ---- 比1稍好, 但是，将来一旦发生需求变更，要求在【运行时】调整超时间隔（甚至要求让用户来配制超时间隔），则写法2的缺点立马暴露无遗
3、`在配制文件中加入一个超时项，然后程序读取配制文件获得超时值，然后调用 sleep`。------  正确方式.

## 代码耦合度太大

合理使用  MVC或者设计模式 (代码耦合分别由哪些情况引起？什么是正交的设计？)

## 不注意资源回收

Java 在语言层面提供了内存的垃圾回收机制，程序员只管申请内存，不需要再关心释放的问题. 此很多新手养成了坏习惯，对于其它资源（比如数据库连接）也只申请不释放（有些人甚至天真地以为 JVM 会帮你搞定资源回收）. 这个坏习惯会导致资源的泄露，而资源泄露往往比内存泄露更要命。如果你写的程序是长时间运行的（比如运行在 Web Server 上），时间长了会由于资源耗尽而导致整个进程出问题

# 异常处理使用不当

<img src="Snipaste_2018-05-13_16-07-17.png">

[异常的处理原则](http://codebuild.blogspot.com/2012/01/15-best-practices-about-exception.html), 此外还有一个关于异常处理的经验 [link1](https://www.ibm.com/developerworks/cn/java/j-lo-exception-misdirection/index.html), [link2](https://www.ibm.com/developerworks/cn/java/j-lo-exception/index.html)

*   异常的名字必须清晰而且有具体的意思，表示异常发生的问题，例如 FileNotFoundException 就很清晰直观

*   定义你自己的异常类层次，例如 UserException 和 SystemException 分别代表用户级别的异常信息和系统级别的异常信息，而其他的异常在这两个基类上进行扩展

*   受检异常和非受检异常: 前者用在[错误需要进一步处理和恢复, 如SQLException, 需要进一步关闭connection, transaction回滚], 后者用在 [程序不能继续执行, 也没必要进一步对异常处理, 如服务器连接不上、参数不正确]

*   避免抛出异常对代码层次污染(设计隔离原则)

    *   例如dao层某个方法抛出sqlexception, 此时就不要继续向上抛了, 应该在catch之后处理成runtimeException抛出


*   空的 catch 语句块

    *   即使有ex.printStacktrace()也算空的catch块, 因为该方法只是打印堆栈信息, 而且异常并没有使得程序终端执行, 导致更多的异常。
    *   可以catch后抛出非受检异常, finally里面处理其他后续操作如关闭资源等等


*   没有使用 finally 可能会产生资源泄露

*   不要使用函数返回值进行错误处理

    *   放着 Java 的异常机制不用，而用函数返回值来表示成功/失败, 简直是“捧着金碗要饭”

        1. 返回值一般用整数值或布尔值表示，传递的信息过于简陋；
        2. 一旦调用者忽略了错误返回码，就会导致和“空 catch 语句块”类似的问题；
        3. 对同一个函数的多处调用，都需要对返回值进行重复判断，导致代码冗余

*   循环块中不要有try...catch

    *   异常处理占用系统资源
    *   同样, 这样的例子也要避免: 类 A 中执行了一段循环，循环中调用了 B 类的方法，B 类中被调用的方法却又包含 try-catch 这样的语句块


*   可以自定义一个runtimeException, 构造函数接受各类受检Exception, 并将有效的异常信息复制到自身属性, 这样可以在使用时统一捕捉Exception, 可以省不少力气(多时候没有必要每个不同类型的 Exception 写一段 catch 语句，对于开发来说，任何一种异常都足够说明了程序的具体问题。)

    ```java
    /**
    * 自定义 RuntimeException
    * 添加错误代码属性
    */
    public class RuntimeException extends java.lang.RuntimeException { 
        //默认错误代码 
        public static final Integer GENERIC = 1000000; 
        //错误代码
        private Integer errorCode; 

        private String msg;

        public RuntimeException(Integer errorCode, Throwable cause) {
                this(errorCode, null, cause);
        }
        public RuntimeException(String message, Throwable cause) {
                //利用通用错误代码
                this(GENERIC, message, cause);
        }
        public RuntimeException(Integer errorCode, String message, Throwable cause) {
                // 那么当 Exception 的类型已经是 RuntimeException 时，我们又做了一次封装。将 RuntimeException 又重新封装了一次，进而丢失了原有的 RuntimeException 携带的有效信息。
                // 添加相关的检查，确认参数 Throwable 不是 RuntimeException 的实例。如果是，将拷贝相应的属性到新建的实例上
                if (cause instanceof RuntimeException) {
                    this.msg = ...
                }
                this.errorCode = errorCode;
                super(message, cause);
        }
        public Integer getErrorCode() {
                return errorCode;
        } 
    }
    ```

    后面使用的时候就可以这样了

    ```java
    try{
    //可能抛出 RuntimeException、IOExeption 或者其它；
    //注意这里和误区六的区别，这里是一段代码抛出多种异常。以上是多段代码，各自抛出不同的异常
    }catch(Exception e){
        //一如既往的将 Exception 转换成 RuntimeException，但是这里的 e 其实是 RuntimeException 的实例，已经在前段代码中封装过
        throw new RuntimeException(/**/code, /**/, e);
    }

    ```

*   避免打印多层次的重复的异常

    ```java
    public class A {
    private static Logger logger = LoggerFactory.getLogger(A.class);
    public void process(){
        try{
        //实例化 B 类，可以换成其它注入等方式
        B b = new B();
        b.process(); // 解决办法是: 这里的部分应该拿出try...catch块

        //other code might cause exception
        
        } catch(XXXException e){
        //如果 B 类 process 方法抛出异常，异常会在 B 类中被打印，在这里也会被打印，从而会打印 2 次
        // 如果层次再复杂一点，不去考虑打印日志消耗的系统性能，仅仅在异常日志中去定位异常具体的问题已经够头疼的了。
        // 其实打印日志只需要在代码的最外层捕捉打印就可以了，异常打印也可以写成 AOP，织入到框架的最外层
        logger.error(e);
        throw new RuntimeException(/* 错误代码 */ errorCode, /*异常信息*/msg, e);
        }
        }
    }
    public class B{
    private static Logger logger = LoggerFactory.getLogger(B.class);
        public void process(){
            try{
                //可能抛出异常的代码
            }
            catch(XXXException e){
                logger.error(e);
                throw new RuntimeException(/* 错误代码 */ errorCode, /*异常信息*/msg, e);
            }
    }
    }
    ```


# 关于基本类型和引用类型

## 这两种类型在内存存储上有什么区别？

```java
StringBuffer str = new StringBuffer();
int n = 123;
// 该 StringBuffer 【对象】的内容是存储在堆（Heap）上的，需要申请堆内存。而变量 str 只不过是针对该 StringBuffer 对象的一个引用（或者叫地址）。变量 str 的【值】（也就是 StringBuffer 对象的地址）是存储在【栈】上的。
// 变量 n 的【值】也是存储在栈（Stack）上的，但是这个语句不需要再从堆中申请内存了。
```


## 这两种类型在性能上有什么区别？

Stack: 只能被本线程访问; 内存申请简单; 内存有限
Heap: 能够被进程中的所有线程访问; 内存申请复杂(由于多线程共享, 需要加锁); 内存充足

## 这两种类型对于 GC 有什么区别


# 关于垃圾回收（Garbage Collection）

## JVM 的内存空间

◇栈内存（Stack）：每个线程私有的。
◇堆内存（Heap）：所有线程公用的。
◇方法区（Method Area）：有点像以前常说的“进程代码段”，这里面存放了每个加载类的反射信息、类函数的代码、编译时常量等信息。
◇原生方法栈（Native Method Stack）：主要用于 JNI 中的原生代码，平时很少涉及。

## 垃圾回收机制简介

垃圾回收机制取决于不同jvm, 这里以官方jvm介绍

* 何时回收?
    * 当 JVM 发现堆内存比较紧张、不太够用时，它就会着手进行垃圾回收工作。JVM 进行 GC 的时间点是无法准确预知的。因为 GC 启动的时刻会受到各种运行环境因素的影响，随机性太大
    * 通过 JVM 的命令行参数“-XX:+PrintGC”把相关回收信息打印出来
* 谁负责回收?
    * JVM 会有一个或多个专门的垃圾回收线程，由它们负责清理回收垃圾内存。
* 如何发现垃圾对象？
    * 垃圾回收线程会从“根集（Root Set）”开始进行对象引用的遍历。所谓的“根集”，就是正在运行的线程中，可以访问的【引用变量】的集合（比如所有线程当前函数的参数和局部变量、当前类的成员变量等等）。垃圾回收线程先找出被根集直接引用的所有对象（不妨叫集合1），然后再找出被集合1直接引用的所有对象（不妨叫集合2），然后再找出被集合2直接引用的所有对象......如此循环往复，直到把能遍历到的对象都遍历完。如果能够遍历到, 则有效, 没有遍历到则回收
    * 把垃圾内存重新变为可用内存、进行内存的整理以消除内存碎片、等等。这个过程会涉及到若干算法
* 分代
    * 早期的jvm没有分代技术, 所有被 GC 管理的对象都存放在同一个堆里面,每次进行GC都要遍历所有对象，开销很大,从 JDK 1.2 之后，JVM 开始使用分代的垃圾回收（Generational Garbage Collection）
    * “年老代”（Tenured）和“年轻代”（Nursery）、“持久代”（Permanent，对应于 JVM 规范的“方法区”）
    * 【大部分】对象在刚创建时，都位于“年轻代”。如果某对象经历了几轮 GC 还活着（大龄对象），就把它移到“年老代”。另外，如果某个对象在创建时比较大，可能就直接被丢到年老代。经过这种策略，使得年轻代总是保存那些短命的小对象。在空间尺寸上，“年轻代”相对较小，而“年老代”相对较大。
    * 因为有了分代技术，JVM 的 GC 也相应分为两种——主要收集（Major Collection）和次要收集（Minor Collection）。“主要收集”同时清理年老代和年轻代，因此开销很大，不常进行；“次要收集”仅仅清理年轻代，开销很小，经常进行。

## GC对性能会有啥影响

* 造成当前运行线程的停顿
* 遍历对象引用的开销
* 清理和回收垃圾的开销(部分开销可能包括复制内存块、更新对象引用等等。)

## 几种收集器

衡量 GC 性能的两个重要指标：吞吐量（Throughput）和停顿时间（Pause Time）。吞吐量这个词不是很直观，解释一下：就是 JVM【不用于】GC 的时间占总时间的比率。“吞吐量”是越大越好，“停顿时间”是越小越好。

* 串行收集器（Serial Collector）
* 并行收集器（Parallel Throughput Collector）
* 并发收集器（Concurrent Low Pause Collector）

## 如何降低GC的影响？

* 尽量减少堆内存的使用
* 设置合适的堆内存大小
    * JVM 的堆内存是有讲究的，不能太大也不能太小。如果堆内存太小，JVM 老是感觉内存不够用，可能会导致频繁进行垃圾回收，影响了性能；如果堆内存太大，以至于操作系统的大部分物理内存都被 JVM 自个儿霸占了，那可能会影响其它应用程序甚至操作系统本身的性能。另外，年轻代的大小（或者说“年轻代”与“年老代”的比值）对于 GC 的性能也有明显影响。如果年轻代太小，可能导致次要收集很频繁；如果年轻代太大，导致次要收集的停顿很明显。
* 吞吐量和停顿的取舍
    * (1)看重吞吐量，对停顿时间无所谓；(2)侧重于停顿时间。
    * 对于某些在后台的、单纯运算密集型的应用，属于第一种。比如某些科学计算的应用。这时候建议使用并行收集器。
    * 对于涉及用户 UI 交互的、实时性要求比较高、程序需要快速响应的，属于第二种。比如某些桌面游戏、某些电信交换系统。这时候建议使用并发收集器。




# 关于范型（Generic Programming）

◇GP 是在编译时实现的还是在运行时实现的？为什么要这么实现？
◇GP 的类型擦除机制是咋回事？有啥优点/缺点？
◇使用范型容器（相对于传统容器）在性能上有啥影响？为什么？

# 关于多线程

◇synchronized 关键字是怎么起作用滴？
◇synchronized 的颗粒度（或者说作用域）如何？是针对某个类还是针对某个类对象实例？
◇synchronized 对性能有没有影响？为什么？
◇volatile 关键字又是派啥用滴？啥时候需要用这个关键字捏？
