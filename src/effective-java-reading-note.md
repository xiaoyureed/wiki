---
title: Effective Java Reading Note
tags:
  - reading note
date: 2018-07-01 21:47:18
categories: java core
---


<div align="center">
effective java reading note。
</div>

<!--more-->

<!-- TOC -->

- [创建和销毁](#创建和销毁)
  - [考虑使用static factory method代替构造函数](#考虑使用static-factory-method代替构造函数)
    - [优点](#优点)
    - [缺点](#缺点)
    - [场景(何时可以替换构造器)](#场景何时可以替换构造器)
  - [遇到很多可选构造器参数时，用构建器(Builder模式)](#遇到很多可选构造器参数时用构建器builder模式)
    - [场景(何时使用builder模式)](#场景何时使用builder模式)
    - [Builder模式](#builder模式)
  - [Private构造器或者使用枚举类型来强化singleton](#private构造器或者使用枚举类型来强化singleton)
  - [避免创建不必要的对象](#避免创建不必要的对象)
    - [应用场景](#应用场景)
    - [对于字符串](#对于字符串)
    - [通过静态工厂方法重用对象](#通过静态工厂方法重用对象)
    - [重用已知不会被修改的可变对象](#重用已知不会被修改的可变对象)
    - [视图(适配器)的实例不需要创建多个(存疑)](#视图适配器的实例不需要创建多个存疑)
  - [消除过期的对象引用](#消除过期的对象引用)
    - [内存泄漏:栈的内存泄露](#内存泄漏栈的内存泄露)
    - [内存泄漏:缓存判断是否过期](#内存泄漏缓存判断是否过期)
    - [内存泄漏:注册回调后不要忘记取消注册](#内存泄漏注册回调后不要忘记取消注册)
  - [避免使用终结方法](#避免使用终结方法)
- [覆盖object中的方法](#覆盖object中的方法)
  - [覆盖equals()](#覆盖equals)
    - [何时覆盖](#何时覆盖)
    - [覆盖时需要遵守的约定](#覆盖时需要遵守的约定)
    - [怎么实现高质量的equals方法](#怎么实现高质量的equals方法)
  - [覆盖hashCode()](#覆盖hashcode)
    - [一种覆盖方式](#一种覆盖方式)
    - [如果一个类是不可变的，且计算hashcode开销比较大，可以考虑把散列码缓存在对象内部](#如果一个类是不可变的且计算hashcode开销比较大可以考虑把散列码缓存在对象内部)
  - [最好始终覆盖toString()](#最好始终覆盖tostring)
  - [谨慎地覆盖clone()](#谨慎地覆盖clone)
    - [常规做法](#常规做法)
    - [更好的做法](#更好的做法)
  - [考虑实现comparable接口](#考虑实现comparable接口)
- [类and接口](#类and接口)
  - [使类和成员的可访问性最小化](#使类和成员的可访问性最小化)
  - [使可变性最小化(不可变类)](#使可变性最小化不可变类)
    - [不可变类](#不可变类)
    - [一个不可变类例子demo](#一个不可变类例子demo)
    - [不可变类其他实现方式：私有化构造器，提供static工厂方法](#不可变类其他实现方式私有化构造器提供static工厂方法)
    - [不可变类优缺点](#不可变类优缺点)
  - [复合(组合)优于继承](#复合组合优于继承)
    - [使用复合的场景](#使用复合的场景)
    - [一个具体实例](#一个具体实例)
  - [要么为继承而设计，并提供文档，要么禁止继承](#要么为继承而设计并提供文档要么禁止继承)
  - [接口优于抽象类](#接口优于抽象类)
    - [接口和抽象类优缺点](#接口和抽象类优缺点)
    - [抽象类和接口合作：骨架实现类](#抽象类和接口合作骨架实现类)
  - [接口只用于定义类型](#接口只用于定义类型)
  - [类层次优于标签类](#类层次优于标签类)
  - [用函数对象(函数接口)表示策略](#用函数对象函数接口表示策略)
    - [策略模式](#策略模式)
    - [函数对象](#函数对象)
  - [优先考虑静态成员类](#优先考虑静态成员类)
  - [使用泛型而不要使用原生态类型](#使用泛型而不要使用原生态类型)
  - [列表优于数组](#列表优于数组)
  - [优先考虑类型安全的异构容器](#优先考虑类型安全的异构容器)
- [枚举和注解](#枚举和注解)
  - [用enum代替int常量](#用enum代替int常量)
  - [在枚举enum中使用实例域](#在枚举enum中使用实例域)
  - [实现接口以编写可扩展的枚举类型](#实现接口以编写可扩展的枚举类型)
  - [使用注解or接口做标识](#使用注解or接口做标识)
  - [坚持使用@Override注解](#坚持使用override注解)
- [方法](#方法)
  - [在方法开始检查参数的有效性](#在方法开始检查参数的有效性)
  - [进行必要的保护性拷贝](#进行必要的保护性拷贝)
  - [慎用重载](#慎用重载)
  - [应该返回0长度的数组或集合，而不是null](#应该返回0长度的数组或集合而不是null)
- [通用程序设计](#通用程序设计)
  - [使局部变量的作用域最小化](#使局部变量的作用域最小化)
  - [Foreach循环优于传统for循环](#foreach循环优于传统for循环)
  - [如果要使用精确的答案，避免使用float double](#如果要使用精确的答案避免使用float-double)
  - [基本类型优于装箱类型](#基本类型优于装箱类型)
  - [通过接口引用对象](#通过接口引用对象)
  - [接口优于反射(存疑)](#接口优于反射存疑)
- [异常](#异常)
  - [对异常的多种处理方式](#对异常的多种处理方式)
  - [使失败保持原子性](#使失败保持原子性)
- [jdk中的反面案例](#jdk中的反面案例)
  - [滥用继承:stack类](#滥用继承stack类)
  - [值对象应该被设计为不可变对象:Date类](#值对象应该被设计为不可变对象date类)
  - [滥用常量接口](#滥用常量接口)

<!-- /TOC -->

# 创建和销毁

## 考虑使用static factory method代替构造函数

### 优点

- 有名字

- 不必每次调用都创建新对象，为重复的调用返回相同对象

  看 Boolean 的实现：

  ```java
  public final class Boolean implements java.io.Serializable,
          Comparable<Boolean> {

      public static final Boolean TRUE = new Boolean(true);
      public static final Boolean FALSE = new Boolean(false);
          
      public static Boolean valueOf(boolean b) {
          return (b ? TRUE : FALSE);
      }
  }


  ```

- 可以返回子对象，更灵活，适合面向接口编程

  ```java
  class Father {  
      private Father() {  
      }  
    
      public static Father newInstance(String type) {  
          if (type.equals("ChildA")) { // 根据类型判断返回那个子类对象  
              return new ChildA();  
          } else {  
              return new ChildB();  
          }  
      }  
    
      public void getName() {   
          System.out.println("My name is father");  
      }  
    
      private static class ChildA extends Father {  
          public void getName() {   
              System.out.println("My name is child A");  
          }  
      }  
    
      private static class ChildB extends Father {  
          public void getName() {  
              System.out.println("My name is child B");  
          }  
      }  
  }  
    
  public class Test {  
      public static void main(String[] args) {  
          Father c1 = Father.newInstance("ChildA");  
          c1.getName();  
          Father c2 = Father.newInstance("ChildB");  
          c2.getName();  
      }  
  }  

  ```

  典型应用是：服务提供者框架，如jdbc实现；看下面的案例

  ```java
    /**
  * 服务接口，让提供者实现，
  * 相当于jdbc的connection接口
  * @author xiaoyu
  *
  */
  public interface Service {
    // 。。。
  }

  /**
  * Service provider interface，负责创建服务实现的实例
  * 是可选的，如果没有，service interface的实现就按照类名称注册，通过反射创建实例
  * 相当于jdbc的Driver接口
  * @author xiaoyu
  *
  */
  public interface Provider {

      Service newInstance();
  }
  /**
  * 管理器，负责服务注册和API访问
  * 核心是用一个map维护 name 和 provider 的一一对应
  * @author xiaoyu
  *
  */
  public class ServiceManager {

      /**
      * 组织实例化
      */
      private ServiceManager() {};
      
      private static final HashMap<String, Provider> providers = new HashMap<String, Provider>();
      private static final String DEFAULT_PROVIDER_NAME = "def";//默认
      
      public static void registDefault(Provider p) {
          providers.put(DEFAULT_PROVIDER_NAME, p);
      }
      
      /**
      * 服务注册API，相当于jdbc中DriverManager.registerDriver();
      * @param name
      * @param provider
      */
      public static void registProvider(String name, Provider provider) {
          providers.put(name, provider);
          
      }
      
      /**
      * jdbc中的Drivermanager.getConnection
      * @return
      * @throws Exception
      */
      public static Service newInstance() throws Exception {
          return newInstance(DEFAULT_PROVIDER_NAME);
      }
      
      public static Service newInstance(String name) throws Exception {
          Provider provider = providers.get(name);
          if (provider == null) {
              throw new Exception("no provider regist  with name: " + name);
          }
          return provider.newInstance();
          
      }
      
  }
  ```

- 在创建参数化实例时，使代码变得简洁（语法糖）

  ```java
  private Map<String, List<String>> map = new HashMap<String, List<String>>();  
  public static <K, V> HashMap<K, V> newInstance() {  
      return new HashMap<K, V>();  
  }  

  ```

### 缺点

- 与其他静态方法不易区分，因此经常采用习惯的命名：valueOf、getInstance、newInstance

- 类如果不含 public 或者 protect 的构造器.就不能被子类化，无法产生子类，不能有继承关系

### 场景(何时可以替换构造器)

- 静态工厂通常更加合适
- 需要有继承时，必须要用构造器

## 遇到很多可选构造器参数时，用构建器(Builder模式)

有很多参数时，使用构造器不方便

### 场景(何时使用builder模式)

若类的构造器or静态工厂中有多个参数（4个or更多），适用，特别是有很多可选参数时

### Builder模式

一种不合适的方法是: Javabean，通过setter设置参数，弊端：1.构造过程被分散到多个setter中，无法仅仅通过检验构造器参数的有效性来 保证一致性，不能保证线程安全；2.Javabean模式阻止了把类做成不可变类的可能

更好的方式是 Builder

```java
/**
 * 外部类构造器为private，内部类构造器为public，build()也是public的
 * 内部类为static
 * @author xiaoyu
 *
 */
public class Apple {

    private final String require1;
    private final String require2;
    
    private final String optional1;
    private final String optional2;
    private final String optional3;
    
    private Apple(Builder b) {//这里相当于简化了参数，在这里进行参数检验
        this.require1 = b.require1;
        this.require2 = b.require2;
        this.optional1 = b.optional1;
        this.optional2 = b.optional2;
        this.optional3 = b.optional3;
    }
    
    //这个类可用interface+泛型进行抽象
    public static class Builder {
        private final String require1;
        private final String require2;
        
        //可选参数初始化
        private String optional1 = "";
        private String optional2 = "";
        private String optional3 = "";
        
        public Builder(String require1, String require2) {
            this.require1 = require1;
            this.require2 = require2;
        }
        
        public Builder optional1(String optional1) {
            this.optional1 = optional1;
            return this;
        }
        public Builder optional2(String optional2) {
            this.optional2 = optional2;
            return this;
        }
        public Builder optional3(String optional3) {
            this.optional3 = optional3;
            return this;
        }
        
        public Apple build() {
            return new Apple(this);
        }
    }
}
public class Test {

    public static void main(String[] args) {
        Apple apple = new Apple.Builder("r1", "r2").optional1("o1").optional2("o2").optional3("o3").build();
        System.out.println(apple);
    }
}

```

## Private构造器或者使用枚举类型来强化singleton

想要实现一个单例的类，可能会有如下的经历:

- 私有化构造器 - 但是在反序列化时会产生新对象
- 使用包含单个元素的Enum (单元素的Enum是实现singleton的最佳方式, 当然还有别的, 比如 使用静态内部类, 借助 jvm自身特性保证线程安全)

类似的, 通过私有化构造器强化 "不可实例化", 比如 工具类

## 避免创建不必要的对象

### 应用场景

通过创建附加的小对象， 提升程序的清晰性、简洁性和功能性，这通常是件好事。

反之，通过维护自己的对象池（object pool)来避免创達对象并不是一种好的做法，除非 池中的对象是非常重量级的；因为维护对象池会把代码弄乱，增加内存耗用


### 对于字符串

```java
String s = "stringette";//
String s = new String("stringette");//don't do this

```

### 通过静态工厂方法重用对象

例如静态工厂方法Boolean.valueOf(String)回返回一个固定的Boolean实例，而构造器Boolean(String)在每次被调用都会创建一个新的对象

### 重用已知不会被修改的可变对象

```java
// 判断 婴儿潮
public class Person {
    private final Date birthDate;
    public Person(Date birthDate) {
        this.birthDate = birthDate;
    }
    private static final Date BOOM_START;
    private static final Date BOOM_END;

  //这里的static块中的对象在每次Person创建时会被重用(只在 class 被 load 时候执行一次)
    static{
        Calendar gmtCal=Calendar.getInstance(TimeZone.getTimeZone("GMT"));
        gmtCal.set(1946,Calendar.JANUARY,1,0,0,0);
        BOOM_START=gmtCal.getTime();
        gmtCal.set(1965,Calendar.JANUARY,1,0,0,0);
        BOOM_END=gmtCal.getTime();

    }
    public boolean isBabyBoomer(){
      // 不推荐的做法: 将 BOOM_END BOOM_START 初始化放在这里来延迟初始化 (如果 isBabyBoomer() 从来不被调用, 这么做可以消除这些不必要的初始化工作，但是不建议这样做。这样做会使方法的实现更加复杂，而性能提升很小, 划不来.)
        return birthDate.compareTo(BOOM_START)>=0&&birthDate.compareTo(BOOM_END)<0;
    }
}

```

### 视图(适配器)的实例不需要创建多个(存疑)

针对给定对象的特定适配器，不需要创建多个适配器实例。虽然适配器适配的对象可能改变，但是所有返回对象在功能上是同等的。

例如，Map接口的keySet方法返回该Map对象的Set视图，其中包含该Map中所有的键 (key)。粗看起来，好像毎次调用keyset都应该创建一个新的Set实例，但是，对干一个给定的 Map对象，实际上每次调用keySet都返回同样的Set实例。虽然被返回的Set实例一般是可改变 的，但是所有返回的对象在功能上是等同的：当其中一个返回对象发生变化的时候，所有其 他的返回对象也要发生变化，因为它们是由同一个Map实例支律的•虽然创建keySet视图对象 的多个实例并无害处，却也是没有必要的。

## 消除过期的对象引用

核心就是避免 内存泄露

### 内存泄漏:栈的内存泄露

应用实例:

```java
public class Stack {

    private Object[] elements;
    private int size = 0;
    private static final int DEFAULT_INIT_CAPACITY = 16;
   
    public Stack() {
        elements = new Object[DEFAULT_INIT_CAPACITY];
    } 
    
    public void push(Object o) {
        if (elements.length == size) {
            elements = Arrays.copyOf(elements, 2*size + 1);
        }
        
        elements[size++] = o;
    }
    
    public Object pop() throws Exception {
        if (size == 0) {
            throw new Exception();
        }
        Object o = elements[--size];
        elements[size] = null;//-----消除过期对象，防止内存泄露---------
        return o;
    }
    
}

```

### 内存泄漏:缓存判断是否过期

- 解决方案1：当缓存对象是否过期由外部是否有他的键的引用决定，可用WeakHashMap代表缓存

- 解决方案2：对象是否过期不易确定，随时间推移，对象会变得越来越没价值

  - 这时，缓存应该时不时地清除没用项，这个工作可由一个后台线程来完成

  - 或者可以在给缓存添加新项时顺便清理过期项，LinkedHashMap利用removeEldestEntry很好实现

  - 对于更复杂缓存体系，必须使用java.lang.ref


### 内存泄漏:注册回调后不要忘记取消注册

如果你实现了一个API,客户端在这个 API中注册回调，却没有显式地取消注册，那么除非你采取某些动作，否则它们就会积聚。确保 回调立即被当作垃圾回收的最佳方法是只保存它们的弱引用（weak reference),例如，只将它们 保存成WeakHashMap中的键。

## 避免使用终结方法

终结方法（finalizer）通常是不可预测的，
通常使用try。。。finally+显式终结方法如：close()来终结资源

# 覆盖object中的方法

## 覆盖equals()

### 何时覆盖

如果类具有自己特有的“逻辑相等”概念（不 同于对象等同的概念），而且超类还没有榷盖equals以实现期望的行为，这时我们就需要覆盖 equals方法

### 覆盖时需要遵守的约定

1.	自反性 x.equals(x) == true
2.	对称性 x.euqals(y) == y.equals(x)
3.	传递性 x.equals(y) , y.equlas(z) -> x.equlas(z)
4.	一致性 多次x.euqlas(y)的结果必选是一致的
5.	非空性 x!=null&&x.equlas(null)==false ==true

### 怎么实现高质量的equals方法

1.	使用 == 操作符检查“参数是否为这个对象的引用”
2.	使用instanceof操作符 检查“参数是否是正确的类型”
3.	把参数转换为正确的类型
4.	对于每一个关键域 逐个对比
5.	编写完成后 请检查和测试他们是否是传递的、对称的、一致的
6.	覆盖equals是总是要覆盖hashCode方法！


## 覆盖hashCode()

一般和 equals() 同时覆盖

### 一种覆盖方式

```java
1. 把某个非零的常数值，比如说17，保存到一个名为result的int类型变量中。(int result = 17)

2. 对于对象中每个关键域f (指equals方法中涉及的每个域)，完下面的步骤：

    a. 为该域计算int类型的散列码 c:

        i. 如果该域是boolean类型的，则计算(f ? 1 : 0)

        ii. 如果该域是 byte、char、short、或者int类型的，则计算(int)f。

        iii. 如果是 long 类型的，则计算 (int)(f^(f>>>32))。

        iv. 如果该域是 float 类型，则计算 FLoat.floatToIntBits(f)。

        v. 如果该域是 double 类型，则计算 Double.doubleToLongBits(f)，然后按照步骤2.a.iii，为得到 long 类型值计算散列值。

        vi. 如果该域是一个对象引用，并且该类的equals方法通过递归地调用equals的方式来比较这个域，则同样为这个域递归地调用hashCode。如果需要更复杂的比较，则为这个域计算一个范式(canonical representation)，然后针对这个范式调用hashCode。如果这个域为null,则返回0。

        vii. 如果该域是一个数组，则要把每个元素当作单独的域来处理。

    b. 按照下面的公式，将 2.a 计算得到的散列码 c 合并到 result 中：result = 31 * result + c;

3. 返回result。

4. 写完了之后，检查是否符合上述的三条规定。

注：在计算过程中选择31的原因，是因为31有个很好的性能，现代的JVM可以自动的优化计算过程，将31 * i优化成为(i << 5) - i。用位运算和减法替代了乘法


@Override
public int hashCode() {
    int result = 17;
    result = 31 * result + areaCode;
    result = 31 * result + prefix;
    result = 31 * result + lineNumber;
    return result;
}

```

### 如果一个类是不可变的，且计算hashcode开销比较大，可以考虑把散列码缓存在对象内部

```java
private volatile static int hashcode;
@Override
public int hashCode() {
    int result = hashcode;
    if (result == 0){
        result = 31 * result + areaCode;
        result = 31 * result + prefix;
        result = 31 * result + lineNumber;
        hashcode = result;
    }
    return result;
}

```

## 最好始终覆盖toString()

## 谨慎地覆盖clone()

### 常规做法

实现cloneable接口，覆盖clone()

```java
@Override 
public PhoneNumber clone() { 
     super.clone(); 
}

// 此公有方法首先调用super.clone(),然后修正任何需要修正的域(浅克隆、深度克隆)。

```

### 更好的做法

提供一个拷贝构造器(copy constructor)或者拷贝工厂(copy factory) 

相比Cloneable/clone优点： 

不依赖于某一种很有风险的，语言之外的对象创建机制； 
不要求遵守尚未制定好的文档规范； 
不会与final域的正常使用发生冲突； 
不会抛出不必要的受检查异常CloneNotSupportedException； 
不需要强制进行类型转换；   

```java
public class GoodClone {
    private int type;
    private final String special;

    public GoodClone(int type, String spe) {
        this.type = type;
        this.special = spe;
    }

    /**
     * 拷贝构造器
     * 
     * @param good
     */
    public GoodClone(GoodClone good) {
        this.special = good.special;
        this.type = good.type;
    }

    /** 
     * 拷贝工厂 
     * @param good 
     * @return 
     */  
    public static GoodClone newInstance(GoodClone good) {  
        return new GoodClone(good.type, good.special);  
    }
}

```

## 考虑实现comparable接口

如果编写的 类有明显的排序关系, 实现 Compareble<T> 接口


# 类and接口

## 使类和成员的可访问性最小化

访问修饰符

```
访问权限   类   包  子类  其他包

public     ∨   ∨    ∨     ∨          （对任何人都是可用的）

protect    ∨   ∨    ∨     ×　　　 （继承的类可以访问以及和private一样的权限）

(default)  ∨   ∨    ×     ×　　　 （包访问权限，即在整个包内均可被访问）

private    ∨   ×    ×     ×　　　 （除类型创建者和类型的内部方法之外的任何人都不能访问的元素）

----------------------

private     私有        只有在声明这个成员的类内部可以访问
(default)   包级私有    同个 package 可访问, 子类不可访问
protected   受保护的    同个 package 可访问, 子类可访问
public      公有的       任何地方可以访问
```

private 和 default 成员不会影响API
Protected 和public 会被导出到API
实例域绝对不能是public (因为 包含public域的类都不是线程安全的)
在public class中使用public的方法访问private的域

## 使可变性最小化(不可变类)

### 不可变类

不可变对象本质上是线程安全的，它们不要求同步; 可以自由地被共享; 但是也有缺点: 对于每个不同的值(状态)都需要一个单独的对象实例来表示, 耗费内存

定义: 实例不能被修改的类，如：String，Integer等基本类型的包装类

使类不可变，有如下5条规范：

1.	不提供任何修改对象状态的方法（mutator）
2.	保证类不可扩展（如继承），一般使用final修饰类
3.	Final修饰所有域
4.	Private修饰所有域
5.	确保对任何可变元素的互斥访问: 🐍 客户端不能获得类的可变域所指向对象的引用; 🐍 不要用客户端提供的对象初始化类的可变域


### 一个不可变类例子demo

```java
/**
 * 实现一个标准的不可变类,复数，有实数和虚数
 * @author xiaoyu
 *
 */
public final class Complex {
    private final double re;//实数
    private final double im;//虚数
    public Complex(double re, double im) {
       this.re = re;
       this.im = im;
    }
    
    public double getRe() {
        return re;
    }
    public double getIm() {
        return im;
    }
    //加法
    public Complex add(Complex c) {
        
        return new Complex(re+c.re, im+c.im);
    }
    
    @Override
    public boolean equals(Object obj) {
        if (obj == this) {
            return true;
        }
        if (!(obj instanceof Complex)) {
            return false;
        }
        Complex temp = (Complex) obj;
        return Double.compare(re, temp.re) == 0 &&
                Double.compare(im, temp.im) == 0;
    }
    @Override
    public int hashCode() {
        // TODO Auto-generated method stub
        return super.hashCode();
    }

}

```

### 不可变类其他实现方式：私有化构造器，提供static工厂方法

```java
//好处是更灵活，可以为静态工厂提供缓存，比如：对于hashcode计算复杂的类，缓存code值
public class Complex {
    private final double re;
    private final double im;

    private Complex(double re, double im) {
      this.re = re;
      this.im = im;
    }

    public static Complex valueOf(double re, double im) {
      return new Complex(re, im);
    }
  }


```

### 不可变类优缺点

线程安全的，它们不要求同步, 可以自由地被共享

但是  对于每个不同的值都需要一个单独的对象

缺点的解决办法:

- 对于频繁会用到值，在类内部提供public static final 的常量

- 或者 对于不可变类，创建一个与之配套的“配套类”，是可变的，如：String和StringBuilder

## 复合(组合)优于继承

### 使用复合的场景

比较抽象的说法是，只有子类和父类确实存在"is-a"关系的时候使用继承，否则使用复合。
或者比较实际点的说法是，如果TypeB只需要TypeA的部分行为，则考虑使用复合。

### 一个具体实例

```java
public class InstrumentedHashSet<E> extends HashSet<E> {
    // The number of attempted element insertions
    private int addCount = 0;

    public InstrumentedHashSet() {
    }

    public InstrumentedHashSet(int initCap, float loadFactor) {
        super(initCap, loadFactor);
    }

    //覆盖hashset（父类）的两个方法，但是内部还是调用父类的对应方法
    @Override
    public boolean add(E e) {
        addCount++;
        return super.add(e);
    }

    //父类addAll内部是通过调用add方法实现，这里计数重复了
    @Override
    public boolean addAll(Collection<? extends E> c) {
        addCount += c.size();
        return super.addAll(c);
    }

    public int getAddCount() {
        return addCount;
    }
}

///////////////////////////////////////

//解决办法是：
// 即，在一个forwarding class中增加一个private field引用现有类的实例，forwarding class中的方法对应现有类的方法。【forwarding class】相当于是一个包装类


public class ForwardingSet<E> implements Set<E> {
    private final Set<E> s; // 组合, 而不是继承

    public ForwardingSet(Set<E> s) {
        this.s = s;
    }

   //省略一些接口 。。。。。

    @Override
    public boolean equals(Object o) {
        return s.equals(o);
    }

    @Override
    public int hashCode() {
        return s.hashCode();
    }

    @Override
    public String toString() {
        return s.toString();
    }
}

//使用时直接继承forwarding class：

public class InstrumentedSet<E> extends ForwardingSet<E> {
    private int addCount = 0;

    public InstrumentedSet(Set<E> s) {
        super(s);
    }

    @Override
    public boolean add(E e) {
        addCount++;
        return super.add(e);
    }

    @Override
    public boolean addAll(Collection<? extends E> c) {
        addCount += c.size();
        return super.addAll(c);
    }

    public int getAddCount() {
        return addCount;
    }
}


```

## 要么为继承而设计，并提供文档，要么禁止继承

一个类设计为可继承，要遵循：

1.	构造器不能调用可被覆盖的方法
2.	如果实现了cloneable、serielizable接口，则clone方法和readObject方法不能调用可被覆盖的方法

## 接口优于抽象类

### 接口和抽象类优缺点

```
对于 接口:

- 现有的类可以很容易被更新，以实现新的接口。

- 接口是定义mixin（混合类型）的理想选择。

- 接口允许我们构造非层次结构的类型框架

对于 抽象类:

- 可以对于一些高层方法提供实现, 而把低层基本方法交给子类去实现(实际上高层方法就是调用的低层方法)
```

### 抽象类和接口合作：骨架实现类

在选择抽象类和接口时，并不是二选一的答案，或干脆枪毙掉抽象类。其实，可以把接口和抽象类的优点结合起来，对于你希望导出（对外提供）的每一个重要接口都提供一个抽象类（骨架实现类）。接口的作用仍然是定义类型，骨架实现类负责实现无需子类关心的方法。

按照惯例，骨架实现类的命名方法为： AbstractInterface，这里的Interface指的是接口的名字。JDK的的集合框架中，具有大量这样的骨架实现类：AbstractCollection，AbstractSet，AbstractList，AbstractMap

```java
//假设有一个接口，它可以实现一组对象的求和，代码如下：

public interface Summation<T> {
  //实现两个对象的相加
  T twoEleAdd(T obj01, T obj02);

  //实现List求和
  T listEleSum(List<T> list);

  //实现数组求和
  T arrayEleSum(T[] array);
}
//根据观察，它的基本方法只有一个T twoEleAdd(T obj01, T obj02);,现在我们可以来实现他的“骨架”了：

public abstract class AbstractSummation<T> implements Summation<T> {

  @Override
  public abstract T towEleAdd(T obj01, T obj02);

  @Override
  public T listEleSum(List<T> list) {
    T firstEle = null;
    for (T t : list) {

      if (firstEle == null) {
        firstEle = t;
        continue;
      }

      firstEle = towEleAdd(firstEle, t);
    }
    return firstEle;
  }

  @Override
  public T arrayEleSum(T[] array) {
    T firstEle = null;
    for (T t : array) {

      if (firstEle == null) {
        firstEle = t;
        continue;
      }

      firstEle = towEleAdd(firstEle, t);
    }
    return firstEle;
  }
}
//继承这个骨架类就只用实现towEleAdd方法，就可以完成一组对象的求和工作了。

```

## 接口只用于定义类型

类实现接口时，接口就冲当该类实例的类型，为了其他目的（如：引入常量）而定义接口是不好的

## 类层次优于标签类

标签类: 一个类可以有多个风格的实例，如：类Figure（图形），可以有实例Circle（圆），Rectangle（方形），square（正方）; 这些标签类（tagged class）有着许多优点，但是破坏了可读性


类层次: 继承, 实现

## 用函数对象(函数接口)表示策略

### 策略模式

策略模式定义了一系列的算法，并将每一个算法封装起来，而且使它们还可以相互替换。策略模式让算法独立于使用它的客户端而独立变化

### 函数对象

定义这样一种对象，它的方法执行其他对象上的操作，如果一个类仅仅导出这样的一个方法，它的实例实际上就等同于一个指向该方法的指针。这样的实例被称为函数对象

```java
class StringLengthComparator {
    public int compare(String s1, String s2) {
        return s1.length() - s2.length();
    }
}

////////////////////////////////////////////////////////////

//这个类是无状态的，没有域，这类的所有实例是功能上等价的，所以可以作为单例存在

class StringLengthComparator {
    private StringLengthComparator() {}
    public static final StringLengthComparator 
        INSTANCE = new StringLengthComparator();
    public int compare(String s1, String s2) {
        return s1.length() - s2.length();
    }
}

///////////////////////////////////////////////////

// 但是，用这述这种方法有个问题，就是规定了参数的类型，这样就无法传递任何其他的比较策略。相反，对于这种情况，应该定义一个Comparator接口，并修改StringLengthComparator来实现这个接口。换句话说， 在设计具体的策略类时，还需要定义一个策略接口：

// Strategy interface
public interface Comparator<T> {
    public int compare(T t1, T t2);
}
//当下，前面的具体策略类声名如下：
class StringLengthComparator implements Comparator<String> {
//    ... 
}

//这样，在传递具体策略类的对象的时候，只需要将参数类型定为接口类型(使用接口做类型定义)，现在可以传递其他的比较策略了。
//        具体策略类往往使用匿名类声名，如下：

Arrays.sort(stringArray, new Comparator<String>() { // 1.	当一个具体策略只被使用一次时，通常使用匿名类来声名和实例化这个具体策略。
    public int compare(String s1, String s2) {
        return s1.length() - s2.length();
    }
});

////////////////////////////////////////

// 2.	当一个具体策略是设计用来重复使用的时候，它的类通常就要被实现为私有的静态成员类，并通过公有的静态final域或静态工厂方法导出，其类型为策略接口。

// Exporting a concrete strategy
class Host {
    private static class StrlenCmp implements Comparator<String>, Serializable {
        public int compare(String s1, String s2) {
            return s1.length() - s2.length();
        }
    }

    // Returned comparator is serializable
    public static final Comparator<String> STRING_LENGTH_COMPARATOR = new StrlenCmp();
}
```

## 优先考虑静态成员类

静态成员类 非静态成员类 区别:

1.	非静态成员类 的 实例 与外围类的一个实例 关联 (在非静态成员类内部，可以调用外围的方法，其实例包含一个外围实例的引用)---------------所以会增加构造的时间开销; 所以当不访问外围类时，要声明成静态的static，如果不static，则每个实例将包含一个指向外围类实例的引用，要消耗时间空间，导致外围实例不被回收

1.	如果嵌套类实例 能在 外围类实例 之外独立存在，必须是静态成员类 --------------- 所以在没有外围类实例时，不能创建非静态成员类，非静态成员实例的必须要有一个外围实例

使用场景: 如果一个嵌套类需要在一个以上的地方使用，或者它太长了，不适合放在一个方法（注意：不是类）内部，那么应该使用成员类。如果成员类的每个实例都需要一个指向外围实例的应用（需要使用this），则该成员类做成非静态的；否则就应该做成静态的。

## 使用泛型而不要使用原生态类型

ref: https://www.jianshu.com/p/000070fc7267

A:原生态类型如 List:不带任何类型参数的泛型名称
B:参数化类型如List<String> ：表示元素类型为String的列表
C:无限制的通配符类型如List<?>:表示元素为未知类型

## 列表优于数组

- 数组是协变的(convariant)，如果 Sub 是 Super 的子类型，那么数组类型 Sub[] 就是 Super[] 的子类型。泛型是不可变的，List<Sub>不是List<Super>的子类型, 所以优先使用列表可以用到泛型

- 数组是具体化的(reified)，因此数组在运行时才知道并检查它们的元素类型约束, 而泛型约束在编译期间就可以检查.

## 优先考虑类型安全的异构容器

普通容器: 泛型中的 "泛型参数"将元素类型定死了, 如：Set<String>，只有一个类型参数，可以存“jjj”“xxxx”等；HashMap<String, Object>集合有两个类型参数，可以存 “小明”-new People(), “小红”-new People()

异构容器: 一个容器里面它存放的类型参数数目是不固定的，那么它就是一个异构的容器; 如：Map<Class<T>, T>，可以存Student.class-new Student(), Car.class-new Car()

一个使用场景: 使用一个容器来存放数据库中有任意列的一个行。因为一行，列的数目是不固定的，每个列的类型也是不确定的，那么就可以使用一个异构的容器来表示这个行

```java
//表示一行记录中的一列， T是类型，valClass是值
public class Column<T> {
 
  private final T valClass ;
 
  @SuppressWarnings( "unchecked" )
  public Column(Class<T> valClass) {
    this. valClass = (T) valClass;
  }
 
  @SuppressWarnings( "unchecked" )
  public T cast(Object obj) {
    return obj == null ? null : ((Class<T>) valClass).cast(obj);
  }
}
 //表示一行记录，包含多个column
public class DatabaseRow {
 
  private Map<Column<?>, Object> row = new HashMap<>();
 
  public <T> void putColumn(Column<T> type, T instance) {
    if (type == null )
      throw new NullPointerException("Type is null" );
    row.put(type, instance);
  }
 
  public <T> T getColumn(Column<T> type) {
    return type.cast( row.get(type));
  }
 
  public static void main(String[] args) {
    DatabaseRow db = new DatabaseRow();
 
    Column<Integer> colInt = new Column<Integer>(Integer. class);
    Column<Double> colDouble = new Column<Double>(Double. class);
    Column<Float> colFloat = new Column<Float>(Float. class);
 
    db.putColumn(colInt, 1);
    db.putColumn(colDouble, 10.0);
    db.putColumn(colFloat, 12.3f);
 
    System. out.println(colInt.getClass() + " " + colDouble.getClass());
    System. out.println(db.getColumn(colInt) + " " + db.getColumn(colDouble));
  }
 
}

```


# 枚举和注解

## 用enum代替int常量

## 在枚举enum中使用实例域

## 实现接口以编写可扩展的枚举类型

## 使用注解or接口做标识

Java1.5之后，使用注解如：@Test 标明有些元素需要通过xx工具或xx框架特殊处理

空接口，如：cloneable，serielizable接口
相对于标记注解的优点：
1.	使用接口，定义了类型，可以在	编译时发现（使用注解情况下）运行时才能显现的错
2.	可以被更加精确的锁定--------这个还不理解


## 坚持使用@Override注解

可以避免一大类非法错误
如：覆盖equals，hashcode方法是出错

# 方法

## 在方法开始检查参数的有效性

## 进行必要的保护性拷贝

将传入参数拷贝一份之后再传入

```java
//在编写不可变类时，或者要让客户端的对象进入内部数据结构中时

public final class Period {  
    private final Date start;  //这里看似不可变，但是Date类是可变的
    private final Date end;  
    public Period(Date start,Date end) {  
        if(start.compareTo(end) > 0){  
            throw new IllegalArgumentException(start + " after " + end);  
        }  
        this.start = start;  
        this.end = end;  
    }  
      
    public Date start(){  
        return start;  
    }  
      
    public Date end(){  
        return end;  
    }  
    //remainder omitted  
} 

//由于date是可变的，如下操作即可改变date状态：
Date start = new Date();  
Date end = new Date();  
Period period = new Period(start, end);  
end.setYear(78);  // end 被改变了
System.out.println(period.end());  

////////////////////////////////

// 对 构造函数 改进

public Period(Date start,Date end) {  
    this.start = new Date(start.getTime());  // 重新赋值一份, 切断 client 和 函数内部的联系
    this.end = new Date(end.getTime());  
    if(this.start.compareTo(this.end) > 0){  
        throw new IllegalArgumentException(this.start + " after " + this.end);  
    }  
} 

```

## 慎用重载

重载是发生在编译时的，所以严格的说，它并不是多态, 要调用哪个重载方法是在编译时做出决定的

对于重载方法的选择是静态（编译时的对象类型决定）的，对于覆盖的方法的选择则是动态（运行时的对象类型决定）的

是否重载, 只和参数个数, 类型有关, 和返回值无关

```java
public class OverloadTest {
    public static String mothod(Collection<?> col) {
        return "unknown collection";
    }
    public static String mothod(List<?> list) {
        return "list";
    }
    public static void main(String[] args) {
        Collection<?>[] coll = {
            new HashSet<String>(),
            new ArrayList<String>()
        };
        for(Collection<?> c : coll) {
            System.out.println(mothod(c));
             //都调用mothod(Collection<?> col)方法，程序将打印两次unknown collection
             // 因为程序调用哪个重载方法是在编译时确定的，在for循环中参数的编译时类型为Collection<?>，所以每次迭代都将调用mothod(Collection<?> col)方法
        }
    }
} 


```

## 应该返回0长度的数组或集合，而不是null

调用者不必做额外的特例处理，如对是null的情况做额外处理

# 通用程序设计

## 使局部变量的作用域最小化

1.	在第一次使用的时候声明变量
2.	局部变量都应该初始化，如果不能初始化，就应该推迟声明

## Foreach循环优于传统for循环

因为隐藏了索引 i

## 如果要使用精确的答案，避免使用float double

## 基本类型优于装箱类型

节省了内存

## 通过接口引用对象

## 接口优于反射(存疑)

# 异常

## 对异常的多种处理方式

- 异常转译（exception translation） - 如果方法B抛出了NoSuchElementException这个受检异常，然而在方法A中调用方法B时，根据方法A中的逻辑，当遇到NoSuchElementException异常时，抛出一个IndexsOutOfBoundsException异常更为合适。那么就不应该选择向上传播抛出NoSuchElementException，而是应该选择捕获NoSuchElementException，然后抛出IndexsOutOfBoundsException。

- 转换为非受检异常

## 使失败保持原子性

操作抛出异常，对象的状态不变，和异常之前的状态一致

方法1：设计不可变对象
方法2：在执行操作之前，检查参数有效性
方法3：依靠回复代码
方法4：在对象的一份临时拷贝中操作，之后用结果代替原来对象内容


# jdk中的反面案例

## 滥用继承:stack类

util包有一个工具类，明显是滥用继承的代表。没错，这个类就是鼎鼎大名的Stack类了。

我们知道Statck数据结构本身的特点就是“后进先出”。那么它理想就应该只有两个方法来操纵数据，一个是push()方法，另一个是pop()方法。但是由于继承了Vector，Stack从父亲那里继承了诸如add(),remove(),set()等违反游戏规则的方法。

## 值对象应该被设计为不可变对象:Date类

不可变对象是指，实例属性经过初始化后在对象的整个生命周期内固定不变。例如jdk的String以及各种基本类型的包装类。

不可变对象易于设计，并且在并发环境下更加安全，无需额外的同步机制。

util包里的Date类属于值对象，类似于人民币一元，十元的概念，应该被设计为不可变对象。也就是说，不应该提供各种setter方法来修改对象的属性域。

## 滥用常量接口

在接口中定义常量，让使用的类实现该接口---------错误

常量接口是指没有任何方法，只包含表态final域的接口对象，每一个域都导出一个。实现该接口的类即可获得接口的所有常量属性，看起来非常方便！
然而，常量接口模式是对接口的不良使用。首先，常量的使用应属于内部实现细节，实现常量接口会把这样的细节暴露到导出的API中。其次，如果在后续升级中发现一个类不再需要以前的常量，这个类依然必须实现这个常量接口，以确保向下兼容性。

jdk平台io包里的ObjectStreamConstants就属于常量接口，不值得效仿。

那么，如果需要导出常量，最好的选择方案是jdk5引入的枚举类型(enum)。当然，如果某些常量跟某个类或接口联系非常紧密，也可以把这个常量绑定在该类，例如Integer.MAX__VALUE或Math.PI等。

然而，某些开发人员还是喜欢选择常量接口，因为枚举略显繁琐，而类又必须写一串长长的"public static final "修饰。但是他们的使用还是有底线的，当需要某个常量的，代码只会用XXConstants.XX的模式，而不会直接让类实现这个接口。这种使用算是一种折中方案。采用哪种方法，还是看项目规范吧。


