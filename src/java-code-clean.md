---
title: Java Clean Code Tips
date: 2018-01-01 01:03:19
tags: [java]
categories: java core
---

<div align="center">
记录一些java代码优化的小经验.
代码的优化可以考虑两方面: 1、减小代码的体积; 2、提高代码运行的效率
</div>

<!--more-->

<!-- TOC -->

- [指定类, 方法为final](#指定类-方法为final)
- [重用对象](#重用对象)
- [使用局部变量](#使用局部变量)
- [及时关闭流](#及时关闭流)
- [减少对变量的重复计算](#减少对变量的重复计算)
- [采用懒加载的策略](#采用懒加载的策略)
- [慎用异常](#慎用异常)
- [循环中禁用try...catch](#循环中禁用trycatch)
- [给容器类组建指定初始容量](#给容器类组建指定初始容量)
- [当复制大量数据时，使用System.arraycopy()命令](#当复制大量数据时使用systemarraycopy命令)
- [乘法和除法使用移位操作](#乘法和除法使用移位操作)
- [循环内不要不断创建对象引用](#循环内不要不断创建对象引用)
- [尽可能使用array，无法确定数组大小时才使用ArrayList](#尽可能使用array无法确定数组大小时才使用arraylist)
- [容器组件和线程安全](#容器组件和线程安全)
- [禁止将数组声明为public static final](#禁止将数组声明为public-static-final)
- [避免随意使用静态变量](#避免随意使用静态变量)
- [使用同步代码块替代同步方法](#使用同步代码块替代同步方法)
- [程序运行过程中避免使用反射](#程序运行过程中避免使用反射)
- [使用带缓冲的输入输出流进行IO操作](#使用带缓冲的输入输出流进行io操作)
- [基本数据类型转为字符串](#基本数据类型转为字符串)
- [遍历Map](#遍历map)
- [对于ThreadLocal使用前或者使用后一定要先remove](#对于threadlocal使用前或者使用后一定要先remove)
- [静态类、单例类、工厂类将它们的构造函数置为private](#静态类单例类工厂类将它们的构造函数置为private)

<!-- /TOC -->

# 指定类, 方法为final

*   为类指定final修饰符可以让类不可以被继承，为方法指定final修饰符可以让方法不可以被重写
*   如果指定了一个类为final，则该类所有的方法都是final的
*   Java编译器会寻找机会内联所有的final方法，内联对于提升Java运行效率作用重大
*   在Java核心API中，有许多应用final的例子，例如java.lang.String，整个类都是final的


# 重用对象

*   特别是String对象的使用，出现字符串连接时应该使用StringBuilder/StringBuffer代替
*   由于Java虚拟机不仅要花时间生成对象，以后可能还需要花时间对这些对象进行垃圾回收和处理，因此，生成过多的对象将会给程序的性能带来很大的影响

# 使用局部变量

*   调用方法时传递的参数以及在方法体中创建的临时变量都保存在栈中，速度较快，其他变量，如静态变量、实例变量等，都在堆中创建，速度较慢。
*   另外，栈中创建的变量，随着方法的运行结束，这些内容就没了，不需要额外的垃圾回收。

# 及时关闭流

*   数据库连接、I/O流操作时务必小心，在使用完毕后，及时关闭以释放资源
*   对资源的close()建议分开操作, 能避免资源泄露

```java
try
{
    XXX.close();
    YYY.close();
}
catch (Exception e)
{
    ...
}

建议修改为：

try
{
    XXX.close();
}
catch (Exception e)
{
    ...
}
try
{
    YYY.close();
}
catch (Exception e)
{
    ...
}
```

# 减少对变量的重复计算

```java
for (int i = 0; i < list.size(); i++)
{...}

建议替换为：

for (int i = 0, length = list.size(); i < length; i++)
{...}
```

这样，在list.size()很大的时候，就减少了很多的消耗

# 采用懒加载的策略

即在需要的时候才创建

```java
String str = "aaa";
if (i == 1)
{
　　list.add(str);
}
建议替换为：

if (i == 1)
{
　　String str = "aaa";
　　list.add(str);
}
```

# 慎用异常

*   异常对性能不利, 异常只能用于错误处理，不应该用来控制程序流程。
*   不捕获Java类库中定义的继承自RuntimeException的运行时异常类

# 循环中禁用try...catch

应该把其放在最外层

# 给容器类组建指定初始容量

如果能估计到待添加的内容长度，为底层以数组方式实现的集合、工具类指定初始长度

比如ArrayList、LinkedLlist、StringBuilder、StringBuffer、HashMap、HashSet等等，以StringBuilder为例：

```java
StringBuilder()　　　　　　// 默认分配16个字符的空间
StringBuilder(int size)　　// 默认分配size个字符的空间
StringBuilder(String str)　// 默认分配16个字符+str.length()个字符空间
```

因为如果没有指定初始容量, 每次元素达到最大容量, 容器需要自动扩容, 这是一个十分耗费资源的操作, 如果事前指定好合适的容量, 避免自动扩容, 性能将极大提升

# 当复制大量数据时，使用System.arraycopy()命令

# 乘法和除法使用移位操作

# 循环内不要不断创建对象引用

```java
for (int i = 1; i <= count; i++)
{
    Object obj = new Object();    
}
这种做法会导致内存中有count份Object对象引用存在，count很大的话，就耗费内存了，建议为改为：

Object obj = null;
for (int i = 0; i <= count; i++)
{
    obj = new Object();
}
```
这样的话，内存中只有一份Object对象引用，每次new Object()的时候，Object对象引用指向不同的Object罢了，但是内存中只有一份，这样就大大节省了内存空间了

# 尽可能使用array，无法确定数组大小时才使用ArrayList

基于效率和类型检查的考虑

# 容器组件和线程安全

尽量使用HashMap、ArrayList、StringBuilder，除非线程安全需要，否则不推荐使用Hashtable、Vector、StringBuffer，后三者由于使用同步机制而导致了性能开销

# 禁止将数组声明为public static final

因为这毫无意义，这样只是定义了引用为static final，实际数组的内容还是可以随意改变的，将数组声明为public更是一个安全漏洞，这意味着这个数组可以被外部类所改变

# 避免随意使用静态变量

当某个对象被定义为static的变量所引用，那么gc通常是不会回收这个对象所占有的堆内存的

```java
public class A
{
    private static B b = new B();  
}
```

此时静态变量b的生命周期与A类相同，如果A类不被卸载，那么引用b指向的B对象会常驻内存，直到程序终止

# 使用同步代码块替代同步方法

除非能确定一整个方法都是需要进行同步的，否则尽量使用同步代码块，避免对那些不需要进行同步的代码也进行了同步，影响了代码执行效率。

# 程序运行过程中避免使用反射

因为耗时严重, 特别是Method的invoke方法，如果确实有必要，一种建议性的做法是`将那些需要通过反射加载的类在项目启动的时候通过反射实例化出一个对象并放入内存`, 将时间消耗转移到项目启动的时候.

# 使用带缓冲的输入输出流进行IO操作

带缓冲的输入输出流，即BufferedReader、BufferedWriter、BufferedInputStream、BufferedOutputStream，这可以极大地提升IO效率

# 基本数据类型转为字符串

把一个基本数据类型转为字符串，基本数据类型.toString()是最快的方式、String.valueOf(数据)次之、数据+""最慢

# 遍历Map

使用iterator遍历

```java
public static void main(String[] args)
{
    HashMap<String, String> hm = new HashMap<String, String>();
    hm.put("111", "222");
        
    Set<Map.Entry<String, String>> entrySet = hm.entrySet();
    Iterator<Map.Entry<String, String>> iter = entrySet.iterator();
    while (iter.hasNext())
    {
        Map.Entry<String, String> entry = iter.next();
        System.out.println(entry.getKey() + "\t" + entry.getValue());
    }
}
```

如果你只是想遍历一下这个Map的key值，那用"Set<String> keySet = hm.keySet();"

# 对于ThreadLocal使用前或者使用后一定要先remove

可以避免内存泄漏

# 静态类、单例类、工厂类将它们的构造函数置为private






