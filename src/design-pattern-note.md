---
title: Design Pattern Note
date: 2018-01-31 00:53:28
tags: [design pattern]
categories: computer science
---

<div align="center">
https://github.com/xiaoyureed/design-patern-note

https://www.zhihu.com/question/39972591


</div>

<!--more-->

<!-- TOC -->

- [实践 案例](#实践-案例)
- [uml 类图](#uml-类图)
- [程序 6 大设计原则](#程序-6-大设计原则)
    - [open close principle](#open-close-principle)
    - [single responsibility principle](#single-responsibility-principle)
    - [interface separation principle](#interface-separation-principle)
    - [里氏替换原则](#里氏替换原则)
    - [dependence inversion principle](#dependence-inversion-principle)
    - [Least Knowledge Principle](#least-knowledge-principle)
- [设计模式概要](#设计模式概要)
    - [设计模式总结](#设计模式总结)
    - [spring 中的设计模式](#spring-中的设计模式)
    - [不要过度使用设计模式](#不要过度使用设计模式)
- [工厂模式](#工厂模式)
    - [工厂方法](#工厂方法)
    - [抽象工厂](#抽象工厂)
- [模板模式](#模板模式)
- [策略模式](#策略模式)
- [观察者模式](#观察者模式)
    - [观察者模式定义](#观察者模式定义)
    - [jdk 中的观察者模式](#jdk-中的观察者模式)
        - [jdk 中的 api](#jdk-中的-api)
        - [jdk 观察者模式使用示例](#jdk-观察者模式使用示例)
    - [事件驱动模型](#事件驱动模型)
- [代理模式](#代理模式)
    - [静态代理](#静态代理)
    - [动态代理](#动态代理)
        - [看一个使用示例](#看一个使用示例)
        - [自定义数据库连接池](#自定义数据库连接池)
- [装饰器模式](#装饰器模式)
- [适配器模式](#适配器模式)
- [责任链模式](#责任链模式)
- [单例模式](#单例模式)
- [命令模式](#命令模式)
- [门面模式](#门面模式)
- [建造者模式](#建造者模式)

<!-- /TOC -->

# 实践 案例

https://mp.weixin.qq.com/s/3wHFNFQPtj86AocLhUQahw
TODO



# uml 类图

- 三角空心箭头, 虚线 - 实现关系, 代码中为`继承抽象类or接口`

- 三角空心箭头, 实线 - 泛化关系, 代码中为 `继承非抽象类`

- 普通箭头, 虚线 - 依赖关系, 代码中为 `类方法中的传入参数`

# 程序 6 大设计原则

ref: <<设计模式之禅>>

- 开放封闭原则（Open Close Principle,简称OCP） - 尽量通过扩展软件实体来解决需求变化，而不是通过修改已有的代码来完成变化

    "对新增开放, 对修改关闭"

    即抽象出一个 interface, 新的功能只需要增加实现类即可, 使用的时候, 通过 interface 的 api 使用.


- 单一职责原则(Single Responsibility Principle，简称SRP )

    一个类的职责应该尽可能少, 尽可能拆分类为多个

    如有类Class1完成职责T1，T2，当职责T1或T2有变更需要修改时，有可能影响到该类的另外一个职责正常工作。

- 接口隔离原则（Interface Separation Principle,简称ISP）;

    尽量细化接口，接口中的方法尽量少. (这样实现类就无需实现不关心的方法)

    和单一职责原则类似, 只不过针对接口来说的. 

    解决类似这种问题: 类A通过接口interface依赖类B，类C通过接口interface依赖类D，如果接口interface对于类A和类B来说不是最小接口，则类B和类D必须去实现他们不需要的方法。

- 里氏替换原则（Liskov Substitution Principle,简称LSP）

    在使用基类的的地方可以任意使用其子类，能保证子类完美替换基类

    也就是: 子类一般不会重写父类的方法, 尽量不要破坏继承体系

    需要多注意的是：有时候会为了灵活性牺牲这个原则

- 依赖倒置原则（Dependence Inversion Principle,简称DIP）

    即实现都是易变的，而只有抽象是稳定的，所以当依赖于抽象时，实现的变化并不会影响客户端的调用。

    也就是 "面向接口编程"

- 最少知识原则/迪米特法则 (Least Knowledge Principle, LKP)

    - 类间解耦: talk only to your immediate friends（只和直接的朋友交流）.
    
        出现在成员变量, 方法的输入输出参数中的类 就是直接的朋友。迪米特法则要求只和直接的朋友通信. 只出现在方法体内部的类就不是直接的朋友

    - 暴露尽量少的方法给外界 (能 private 就不要 public).


## open close principle

```java
// bad demo

/**
 * 信息发送类, 通过短信发送
 *
 * @version: 0.1
 * @author: xy
 * @date: 2018年1月23日 下午10:27:28
 */
public class MessageSend {

    public void send(String msg) {
        System.out.println("Text Message send : " + msg);

    }
}

/**
 * 服务类, 供客户端调用
 *
 * @version: 0.1
 * @author: xy
 * @date: 2018年1月23日 下午10:29:09
 */
public class MessageService {

    private MessageSend sender;

    public MessageService() {
        this.sender = new MessageSend();
    }

    public void send(String msg) {
        sender.send(msg);
    }
}

/**
 * 客户端调用
 *
 * @version: 0.1
 * @author: xy
 * @date: 2018年1月23日 下午10:30:11
 */
public class MessageClient {

    public static void main(String[] args) {
        MessageService messageService = new MessageService();
        messageService.send("Merry Christmas !");
    }
}

/////////////////////////////////////////////////////

// 现在需要新支持另一种发送方式: 微信, 只好修改源码

/**
 * 新增的信息发送帮助类, 支持email
 *
 * @version: 0.1
 * @author: xy
 * @date: 2018年1月23日 下午10:37:55
 */
public class EmailMessageSend {

    public  void send(String msg) {
        System.out.println("Email Message send: " + msg);
    }
}

/**
 * 新增的信息发送类型
 *
 * @version: 0.1
 * @author: xy
 * @date: 2018年1月23日 下午10:42:36
 */
public enum SendType {

    /**
     * 短信
     */
    TEXT,
    /**
     * 邮件
     */
    EMAIL 
}

/**
 * 服务类, 供客户端调用
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月23日 下午10:53:18
 */
public class MessageService {

    private MessageSend sender;
    private SendType t;
    public MessageService(SendType t) {
        this.t= t;
    }

    public void send(String msg) {
        // 根据不同的类型进行不同的处理
    }
}

// 然后是client增加新的调用

// 后续如果又要支持新的发送方式, 又需要大改MessageService源码;
// 如果遵循开闭原则设计, 会怎么样呢?


////////////////////////////////////////////////////////////////
// good demo

/**
 * 信息发送接口
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月23日 下午11:02:30
 */
public interface ISendable {

    void send();
}

// 各个信息发送类实现接口
public class MessageSend implements ISendable{}
public class EmailMessageSend implements ISendable{}

// messageService以后可以一直不变

public class MessageService {

    private ISendable sender;

    public MessageService(ISendable sender) {
        this.sender = messageHelper;
    }

    public void send(String msg) {
        sender.send(msg);
    }
}

// 客户端调用
public class MessageClient {

    public static void main(String[] args) {
        MessageService messageService = new MessageService(new MessageSend());
        messageService.send("Merry Christmas !");

        MessageService messageService2 = new MessageService(new EmailMessageSend());
        messageService2.send("Merry Christmas !");
    }
}

// 对于MessageService服务类来说，不用做任何修改，只需要扩展新的推送消息的工具类即可, 这就是遵循开闭原则的好处
```

## single responsibility principle

```java
// bad demo
public class Calculator {
    public int add() throws NumberFormatException, IOException{
        //这里读取和加法耦合在一起了，如果需要增加业务方法【减法】，就要更改代码
        File file = new File("E:/data.txt");
        BufferedReader br = new BufferedReader(new FileReader(file));
        int a = Integer.valueOf(br.readLine());
        int b = Integer.valueOf(br.readLine());
        return a+b;
    }
    public static void main(String[] args) throws NumberFormatException, IOException {
        Calculator calculator = new Calculator();
        System.out.println("result:" + calculator.add());
    }
}

// good demo
//遵循单一职责原则后, 是这样: 分离出来一个类用来读取数据，将一
//个类拆成了两个类，这样以后我们如果有减法，乘法等等，就不用出现那么多重复代码了

public class Reader {
    int a,b;
    public Reader(String path) throws NumberFormatException, IOException{
        BufferedReader br = new BufferedReader(new FileReader(new File(path)));
        a = Integer.valueOf(br.readLine());
        b = Integer.valueOf(br.readLine());
    }
    public int getA(){
        return a;
    }
    public int getB(){
        return b;
    }
}

//单独的计算类
public class Calculator {
    public int add(int a,int b){
        return a + b;
    }
    public static void main(String[] args) throws NumberFormatException, IOException {
        Reader reader = new Reader("E:/data.txt");
        Calculator calculator = new Calculator();
        System.out.println("result:" + calculator.add(reader.getA(),reader.getB()));
    }
}
```

## interface separation principle

```java
//这是错误示例，当实现此接口 时，必须实现不必要的方法 other()
public interface Mobile {
    public void call();//手机可以打电话
    public void sendMessage();//手机可以发短信
    public void game();//手机可以玩愤怒的小鸟？这是不合适的方法
}
//正确的方式是：新建一个接口来拓展Mobile接口
public interface SmartPhone extends Mobile{
    public void game();//智能手机的接口就可以加入这个方法了
}
```

## 里氏替换原则

基类子类可替换

```java
// 看一个破坏继承体系的例子
// 也就是不符合李氏替换原则

/* 父类 */
public class Parent {
    public void method(){
        System.out.println("parent method");
    }
}
/* 子类 */
public class SubClass extends Parent{
    //结果某一个子类重写了父类的方法，说不支持该操作了
    public void method() {
        throw new UnsupportedOperationException();
    }
}

//某一个类
public class SomeoneClass {
    //有某一个方法，使用了一个父类类型
    public void someoneMethod(Parent parent){
        parent.method();
    }
}
/* 测试类 */
public class Client {
    public static void main(String[] args) {
        SomeoneClass someoneClass = new SomeoneClass();
        someoneClass.someoneMethod(new Parent());
        someoneClass.someoneMethod(new SubClass());//此处报错, 运行时才产生报错 // 破坏了继承体系
    }
}
```

## dependence inversion principle

## Least Knowledge Principle

类间解耦

一个类应该尽量不要和陌生的类有太多接触。

```java
// 错误示例
class demo {
    public void a() {...}
    public void b() {...}
    public void c() {
        a();
        b();
    }
}

// 改进后
// 只暴露必须的方法
class demo {
    private void a() {...}
    private void b() {...}
    public void c() {
        a();
        b();
    }
}
```

# 设计模式概要


## 设计模式总结

常见的:

- 工厂模式 & 抽象工厂模式 - 不同条件下创建不同实例

- 模板模式 - 在父类中定义高层的算法执行顺序, 子类中具体实现每个步骤

- 策略模式 - 策略对象依赖注入到context对象，context对象根据它的策略改变而改变它的相关行为

- 适配器模式 - 使得某个不可修改的目标类符合特定要求的接口

- 装饰器模式 - 对被装饰类增强

- 责任链模式 - 拦截的类都实现统一接口，每个接收者都包含对下一个接收者的引用。将这些对象连接成一条链，并且沿着这条链传递请求，直到有对象处理它为止。

- 代理模式 - 为目标对象提供一种代理以控制对这个对象的访问

- 观察者模式 - 一对多的依赖关系，在观察目标类里有一个 ArrayList 存放观察者们。当观察目标对象的状态发生改变，所有依赖于它的观察者都将得到通知，使这些观察者能够自动更新（即使用推送方式）

- 单例模式 - 保证一个类仅有一个实例



不常见的:

- 建造者模式 - 用于构造复杂对象

- 原型模式 - 通过拷贝原型创建新的对象

- 过滤器模式 - 

- 桥接模式

- 门面模式 - 在客户端和复杂系统之间再加一层, 隐藏系统的复杂性

- 组合模式

## spring 中的设计模式

- 简单工厂

    - BeanFactory, 根据传入一个唯一的标识来获得Bean对象

- 工厂方法模式

    - FactoryBean接口。

        实现了FactoryBean接口的bean是一类叫做factory的bean。其特点是，spring会在使用getBean()调用获得该bean时，会自动调用该bean的getObject()方法，所以返回的不是factory这个bean，而是这个bean.getOjbect()方法的返回值。

- 单例模式

    - Spring依赖注入Bean实例默认是单例的。

- 代理模式

    - aop 底层，就是动态代理模式的实现。

- 观察者模式

    - spring的事件驱动模型使用的是 观察者模式 ，Spring中Observer模式常用的地方是listener的实现。

        底层是通过 jdk 提供的事件模型实现的. EventObject, EventListener

- 适配器模式

    - SpringMVC中的适配器HandlerAdatper

        HandlerAdatper使得Handler的扩展变得容易，只需要增加一个新的Handler和一个对应的HandlerAdapter即可。

- 装饰器模式

    - spring 中 类名中含有Wrapper, 都是, 如: httpServletRquestWrapper...


## 不要过度使用设计模式

https://www.zhihu.com/question/23757906/answer/25567356

除了Java, 其他提供动态编程的特性的语言无需设计模式, 因为:

- 如果你用的语言能把类型像变量一样赋值并传来传去，很多创建型模式就没用了。
- 如果你用的语言能把函数像变量一样赋值并传来传去，很多行为模式就没用了。
- 如果你用的语言 style 反对叠床架屋的 class hierarchy，很多结构模式就没用了。


# 工厂模式

优点:

- 将复杂的对象创建过程封装, 简化对象的创建
- 解耦

不同的工厂模式有不同特点:

- 简单工厂(静态工厂): 提供一个超级工厂类, 通过 if else 封装所有 product 的创建逻辑. 

    - 这样带来两个问题:

        - 工厂类违背 single responsibilities principle

            如果需要生产的产品过多，此模式会导致工厂类过于庞大，承担过多的职责，变成超级类。当苹果生产过程需要修改时，要来修改此工厂。梨子生产过程需要修改时，也要来修改此工厂。也就是说这个类不止一个引起修改的原因。违背了单一职责原则

        - 工厂类违背了 open close principle 

            当要生产新的产品时，必须在工厂类中添加新的分支。而开闭原则告诉我们：类应该对修改封闭。我们希望在添加新功能时，只需增加新的类，而不是修改既有的类，所以这就违背了开闭原则

    - 选择关键点：一种产品是否可根据某个参数决定它的种类

        工厂类负责创建的对象较少, 不会造成工厂方法太过复杂
        
        客户端只知道传入参数, 对于是什么对象不关心

- 工厂方法模式: 每个产品都提供一个单独的工厂类

    - 解决了简单工厂模式的两个问题.

        新增 product 创建 时, 增加 对应 factory class 即可, 无需修改已有的工厂类

    - 还是存在问题: 工厂类不是面向接口的

        工厂方法模式可以进一步优化，提取出工厂接口

    - 选择关键点：工厂类和产品类是否是同生同灭的关系

        客户端只知道具体工厂类和产品参数
        
        将创建对象的任务委托给多个工厂中的一个, client无需关心到底是哪个工厂类, 需要时再动态指定, 可以将工厂子类的类名存储在配置文件中or数据库中

- 抽象工厂模式: 不同产品的工厂类实现同一个 factory interface, 每个 具体的 工厂实现类用来创建一个产品族

    - 使用场景: 系统中有多于一个的产品族，而且每次只使用其中一个产品族。

        一个产品族使用一个工厂类.

        产品族表示不同的产品的集合, 之间没有继承关系



## 工厂方法



===============简单工厂 是工厂方法模式的特例

```java
public class FruitFactory {
    public Fruit create(String type){
        switch (type){
            case "苹果": return new Apple(); // 对象创建逻辑可能非常复杂
            case "梨子": return new Pear();
            default: throw new IllegalArgumentException("暂时没有这种水果");
        }
    }
}

// 测试
public class User {
    private void eat(){
        FruitFactory fruitFactory = new FruitFactory();
        Fruit apple = fruitFactory.create("苹果");
        Fruit pear = fruitFactory.create("梨子");
        apple.eat();
        pear.eat();
    }
}
```

==============工厂方法模式

为了解决简单工厂模式的这两个弊端，工厂方法模式应运而生，它规定每个产品都有一个专属工厂。

```java
public class AppleFactory {
    public Fruit create(){
        return new Apple();
    }
}
public class PearFactory {
    public Fruit create(){
        return new Pear();
    }
}

//测试

public class User {
    private void eat(){
        AppleFactory appleFactory = new AppleFactory();
        Fruit apple = appleFactory.create();
        PearFactory pearFactory = new PearFactory();
        Fruit pear = pearFactory.create();
        apple.eat();
        pear.eat();
    }
}
```


## 抽象工厂

```java
public interface IFactory {
    Fruit create();
}
public class AppleFactory implements IFactory {
    @Override
    public Fruit create(){
        return new Apple();
    }
}
public class PearFactory implements IFactory {
    @Override
    public Fruit create(){
        return new Pear();
    }
}

//测试

public class User {
    private void eat(){
        IFactory appleFactory = new AppleFactory();
        Fruit apple = appleFactory.create();
        IFactory pearFactory = new PearFactory();
        Fruit pear = pearFactory.create();
        apple.eat();
        pear.eat();
    }
}

```

# 模板模式

定义: 定义一个操作中的算法的骨架，而将一些步骤延迟到子类中

优点: 

- 模板方法模式通过把不变的行为搬移到超类，去除了子类中的重复代码。

```java
//举个例子，以准备去学校所要做的工作（prepareGotoSchool）为例，假设需要分三步：穿衣服（dressUp），吃早饭（eatBreakfast），带上东西（takeThings）。学生和老师要做得具体事情肯定有所区别
public abstract class AbstractPerson{  
     //抽象类定义整个流程骨架  
     public void prepareGotoSchool(){  
          dressUp();  
          eatBreakfast();  
          takeThings();  
     }  
     //以下是不同子类根据自身特性完成的具体步骤  
     protected abstract void dressUp();  
     protected abstract void eatBreakfast();  
     protected abstract void takeThings();  
}  

public class Student extends AbstractPerson{  
     @Override  
     protected void dressUp() {  
          System.out.println("穿校服");  
     }  
     @Override  
     protected void eatBreakfast() {  
          System.out.println("吃妈妈做好的早饭");  
     }  

     @Override  
     protected void takeThings() {  
          System.out.println("背书包，带上家庭作业和红领巾");  
     }  
}  

public class Teacher extends AbstractPerson{  
     @Override  
     protected void dressUp() {  
          System.out.println("穿工作服");  
     }  
     @Override  
     protected void eatBreakfast() {  
          System.out.println("做早饭，照顾孩子吃早饭");  
     }  

     @Override  
     protected void takeThings() {  
          System.out.println("带上昨晚准备的考卷");  
     }  
}  

// 测试
public class Client {  
     public static void main(String[] args) {  
     Student student = new Student()  
     student.prepareGotoSchool();  

     Teacher teacher  = new Teacher()  
     teacher.prepareGotoSchool();  
     }  
}
```

# 策略模式

定义了一组算法，将每个算法都封装起来，并且使它们之间可以互换. 

场景: (去除 if else)

涉及到三个角色:

- 环境(Context)角色：持有一个Strategy的引用。

- 抽象策略(Strategy)角色：这是一个抽象角色，通常由一个接口或抽象类实现。此角色给出所有的具体策略类所需的接口。

- 具体策略(ConcreteStrategy)角色：包装了相关的具体算法或行为

```java
//假设某店家推出三种会员，分别为普通会员，金牌会员和钻石会员，还有就是普通顾客，针对不同的会员顾客，购物结算时有不同的打折方式。购物后，客户的历史购物金额累计，可以自动升级到相应的会员级别。

// 策略接口
public interface IVipAlgo {
    int calc(int originalPrice);
}

//实现接口, 有多个实现, 每个实现的算法不同
public class VipAlgo1 {
    public int calc(int originalPrice) {
        ...
    }
} 
public class VipAlgo2 {
    public int calc(int originalPrice) {
        ...
    }
}

//上下文类, 具体执行某个算法
class VipCalculator {
    private IVipAlgo vipAlgo;
    public VipCalculator(IVipAlgo IVipAlgo) {
        this.vipAlgo = IVipAlgo;
    }

    public int execAlgo(int originalPrice) {
        return vipAlgo.calc(originalPrice)
    }
}

//测试
class Main {
    main() {
        VipCalculator calc = new VipCalculator(new VipAlgo1());
        int result = calc(112);
    }
}
```


===========在 spring 中这么使用:



# 观察者模式

## 观察者模式定义

别名 发布订阅模式

定义: 观察目标发生状态变化时, 会通知众多的观察者

这就要求观察目标内部维护一个观察者列表, 以便一一通知

一个观察目标有多个观察者, 这些观察者派生自同一个接口

优点:

- 解耦合

缺点:

- 观察者很多时耗时严重
- 观察者模式无法让观察者知道目标是如何变化的, 仅仅只是知道目标发生了变化


```java
/**
 * 抽象观察者
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月25日 下午9:29:32
 */
public interface IObserver {

    // 目标对象调用这个方法通知观察者
    void update(Target t);
}

/**
 * observe target
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月25日 下午11:19:52
 */
public abstract class Target {
    /**
     * observer list
     */
    private List<IObserver> observerList = new ArrayList<IObserver>();

    /**
     * add observer
     * @param observer
     */
    public void add(IObserver observer) {
        observerList.add(observer);
    }

    /**
     * remove observer
     * @param observer
     */
    public void remove(IObserver observer) {
        observerList.remove(observer);
    }

    /**
     * change status
     */
    public void change() {
        System.out.println("Target状态发生变化");
        this.inform();
    }

    /**
     * inform observers
     */
    private void inform() {
        for(IObserver observer: observerList) {
            observer.update(this);
        }
    }
}

///////////////////// 如何使用?  ////////////////////////////////////////////

// 先定义 observers, 多个, 实现自己的 update()
/**
 *  concrete observer1
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月25日 下午11:42:45
 */
public class Observer1 implements IObserver {

    @Override
    public void update(Target t) {
        System.out.println("Observer1观察到" + t.getClass().getSimpleName()+ "发生变化");
    }

}

/**
 * concrete observer2
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月25日 下午11:45:16
 */
public class Observer2 implements IObserver {

    @Override
    public void update(Target t) {
        System.out.println("Observer2观察到" + t.getClass().getSimpleName() + "发生变化.");
    }

}

/**
 * 测试
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月25日 下午11:53:28
 */
public class MyTarget extends Target {

    // ...

    // 测试
    public static void main(String[] args) {
        Target t = new MyTarget();

        IObserver observer1 = new Observer1();
        IObserver observer2=  new Observer2();

        t.add(observer1);
        t.add(observer2);

        t.change();
    }
}
```

## jdk 中的观察者模式

### jdk 中的 api

```java
//观察者接口，每一个观察者都必须实现这个接口
public interface Observer {
    //这个方法是观察者在观察对象产生变化时所做的响应动作，从中传入了观察的对象和一个预留参数
    void update(Observable o, Object arg);
}
//被观察者类
public class Observable {
    //这是一个改变标识，来标记该被观察者有没有改变
    private boolean changed = false;
    //持有一个观察者列表
    private Vector obs;
    public Observable() {
        obs = new Vector();
    }
    //添加观察者，添加时会去重
    public synchronized void addObserver(Observer o) {
        if (o == null)
            throw new NullPointerException();
        if (!obs.contains(o)) {
            obs.addElement(o);
        }
    }
    //删除观察者
    public synchronized void deleteObserver(Observer o) {
        obs.removeElement(o);
    }
    //notifyObservers(Object arg)的重载方法
    public void notifyObservers() {
        notifyObservers(null);
    }
    //通知所有观察者，被观察者改变了，可以执行update方法了。
    public void notifyObservers(Object arg) {
        //一个临时的数组，用于并发访问被观察者时，留住观察者列表的当前状态，这种处理方式其实也算是一种设计模式，即备忘录模式。
        Object[] arrLocal;
        //注意这个同步块，它表示在获取观察者列表时，该对象是被锁定的
        //也就是说，在我获取到观察者列表之前，不允许其他线程改变观察者列表
        synchronized (this) {
            //如果没变化直接返回
            if (!changed)
                return;
            //这里将当前的观察者列表放入临时数组
            arrLocal = obs.toArray();
            //将改变标识重新置回未改变
            clearChanged();
        }
        //注意这个for循环没有在同步块，此时已经释放了被观察者的锁，其他线程可以改变观察者列表
        //但是这并不影响我们当前进行的操作，因为我们已经将观察者列表复制到临时数组
        //在通知时我们只通知数组中的观察者，当前删除和添加观察者，都不会影响我们通知的对象
        for (int i = arrLocal.length-1; i>=0; i--)
            ((Observer)arrLocal[i]).update(this, arg);
    }
    //删除所有观察者
    public synchronized void deleteObservers() {
        obs.removeAllElements();
    }
    //标识被观察者被改变过了
    protected synchronized void setChanged() {
        changed = true;
    }
    //标识被观察者没改变
    protected synchronized void clearChanged() {
        changed = false;
    }
    //返回被观察者是否改变
    public synchronized boolean hasChanged() {
        return changed;
    }
    //返回观察者数量
    public synchronized int countObservers() {
        return obs.size();
    }
}
```

### jdk 观察者模式使用示例

```java
//读者类，要实现观察者接口
public class Reader implements Observer{
    private String name;
    public Reader(String name) {
        super();
        this.name = name;
    }
    public String getName() {
        return name;
    }
    //读者可以关注某一位作者，关注则代表把自己加到作者的观察者列表里
    public void subscribe(String writerName){
        WriterManager.getInstance().getWriter(writerName).addObserver(this);
    }
    //读者可以取消关注某一位作者，取消关注则代表把自己从作者的观察者列表里删除
    public void unsubscribe(String writerName){
        WriterManager.getInstance().getWriter(writerName).deleteObserver(this);
    }
    //当关注的作者发表新小说时，会通知读者去看
    public void update(Observable o, Object obj) {
        if (o instanceof Writer) {
            Writer writer = (Writer) o;
            System.out.println(name+"知道" + writer.getName() + "发布了新书《" + writer.getLastNovel() + "》，非要去看！");
        }
    }
}
//作者类，要继承自被观察者类
public class Writer extends Observable{
    private String name;//作者的名称
    private String lastNovel;//记录作者最新发布的小说
    public Writer(String name) {
        super();
        this.name = name;
        WriterManager.getInstance().add(this);
    }
    //作者发布新小说了，要通知所有关注自己的读者
    public void addNovel(String novel) {
        System.out.println(name + "发布了新书《" + novel + "》！");
        lastNovel = novel;
        setChanged();
        notifyObservers();
    }
    public String getLastNovel() {
        return lastNovel;
    }
    public String getName() {
        return name;
    }
}
//管理器，保持一份独有的作者列表
public class WriterManager{
    private Map<String, Writer> writerMap = new HashMap<String, Writer>();
    //添加作者
    public void add(Writer writer){
        writerMap.put(writer.getName(), writer);
    }
    //根据作者姓名获取作者
    public Writer getWriter(String name){
        return writerMap.get(name);
    }
    //单例
    private WriterManager(){}
    public static WriterManager getInstance(){
        return WriterManagerInstance.instance;
    }
    private static class WriterManagerInstance{
        private static WriterManager instance = new WriterManager();
    }
}
//客户端调用
public class Client {
    public static void main(String[] args) {
        //假设四个读者，两个作者
        Reader r1 = new Reader("谢广坤");
        Reader r2 = new Reader("赵四");
        Reader r3 = new Reader("七哥");
        Reader r4 = new Reader("刘能");
        Writer w1 = new Writer("谢大脚");
        Writer w2 = new Writer("王小蒙");
        //四人关注了谢大脚
        r1.subscribe("谢大脚");
        r2.subscribe("谢大脚");
        r3.subscribe("谢大脚");
        r4.subscribe("谢大脚");
        //七哥和刘能还关注了王小蒙
        r3.subscribe("王小蒙");
        r4.subscribe("王小蒙");
        //作者发布新书就会通知关注的读者
        //谢大脚写了设计模式
        w1.addNovel("设计模式");
        //王小蒙写了JAVA编程思想
        w2.addNovel("JAVA编程思想");
        //谢广坤取消关注谢大脚
        r1.unsubscribe("谢大脚");
        //谢大脚再写书将不会通知谢广坤
        w1.addNovel("观察者模式");
    }
}
```

## 事件驱动模型

观察者模式的另一种形态, 例如Tomcat中的监听器listener

两种形态区别:

- 观察者模式中, 只要 target 发生变化, 都会通知 观察者, 而而事件驱动则不是，因为我们可以定义自己感兴趣的事情, 忽略其他变化

- 观察者模式要求被观察者继承Observable类, 事件驱动中事件源则不需要，因为事件源所维护的监听器列表是给自己定制的，所以无法去制作一个通用的父类去完成这个工作。

- 被观察者传送给观察者的信息是模糊的，比如update中第二个参数，类型是Object，这需要观察者和被观察者之间有约定才可以使用这个参数。而在事件驱动模型中，这些信息是被封装在Event当中的，可以更清楚的告诉监听器，每个信息都是代表的什么。

```java
//定义作者事件, 这代表了一个作者事件: 发布新书
// 这个事件当中一般就是包含一个事件源，在这里就是作者, 通过 constructor 传入
// 
public class WriterEvent extends EventObject{
    private static final long serialVersionUID = 8546459078247503692L;
    public WriterEvent(Writer writer) {
        super(writer);
    }
    public Writer getWriter(){
        return (Writer) super.getSource();
    }
}
//对应的 需要有一个监听器监听这个事件, 对"指定事件" 做出反应
public interface WriterListener extends EventListener{
    // 参数: 被监听的事件
    void addNovel(WriterEvent writerEvent);
}
//作者类
// 可以看到，作者类的主要变化是添加了一个自己的监听器列表，我们使用set是为了它的天然去重效果，
public class Writer{
    private String name;//作者的名称
    private String lastNovel;//记录作者最新发布的小说
    private Set<WriterListener> writerListenerList = new HashSet<WriterListener>();//作者类要包含一个自己监听器的列表
    public Writer(String name) {
        super();
        this.name = name;
        WriterManager.getInstance().add(this);
    }
    //作者发布新小说了，要通知所有关注自己的读者
    public void addNovel(String novel) {
        System.out.println(name + "发布了新书《" + novel + "》！");
        lastNovel = novel;
        fireEvent();
    }
    //触发发布新书的事件，通知所有监听这件事的监听器
    private void fireEvent(){
        WriterEvent writerEvent = new WriterEvent(this);
        for (WriterListener writerListener : writerListenerList) {
            writerListener.addNovel(writerEvent);
        }
    }
    //提供给外部注册成为自己的监听器的方法
    public void registerListener(WriterListener writerListener){
        writerListenerList.add(writerListener);
    }
    //提供给外部注销的方法
    public void unregisterListener(WriterListener writerListener){
        writerListenerList.remove(writerListener);
    }
    public String getLastNovel() {
        return lastNovel;
    }
    public String getName() {
        return name;
    }
}
// 读者类的变化，首先本来是实现Observer接口，现在要实现WriterListener接口，响应的update方法就改为我们定义的addNovel方法，当中的响应基本没变。另外就是关注和取消关注的方法中，原来是给作者类添加观察者和删除观察者，现在是注册监听器和注销监听器，几乎是没什么变化的。
public class Reader implements WriterListener{
    private String name;
    public Reader(String name) {
        super();
        this.name = name;
    }
    public String getName() {
        return name;
    }
    //读者可以关注某一位作者，关注则代表把自己加到作者的监听器列表里
    public void subscribe(String writerName){
        WriterManager.getInstance().getWriter(writerName).registerListener(this);
    }
    //读者可以取消关注某一位作者，取消关注则代表把自己从作者的监听器列表里注销
    public void unsubscribe(String writerName){
        WriterManager.getInstance().getWriter(writerName).unregisterListener(this);
    }
    public void addNovel(WriterEvent writerEvent) {
        Writer writer = writerEvent.getWriter();
        System.out.println(name+"知道" + writer.getName() + "发布了新书《" + writer.getLastNovel() + "》，非要去看！");
    }
}
```

# 代理模式

## 静态代理

代理对象和被代理对象实现相同的接口

代理内部维护一个被代理对象

```java
/**
 * 代理目标和代理都要实现的统一接口
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午10:40:28
 */
public interface Moveable {

    void move();
}

/**
 * 代理目标
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午10:43:01
 */
public class Car implements Moveable {

    @Override
    public void move() {
        System.out.println("Car 开始移动...");
        try {
            Thread.sleep(new Random().nextInt(10000));// 单位是ms
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

}

/**
 * Car 代理类, 添加了时间统计功能
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午10:47:44
 */
public class CarTimingProxy implements Moveable {

    private Moveable car;
    public CarTimingProxy(Moveable car) {
        super();
        this.car = car;
    }

    @Override
    public void move() {
        long start = System.currentTimeMillis();
        car.move();
        long end = System.currentTimeMillis();
        System.out.println("move持续时间: " + (end-start));
    }

}

/**
 * car 代理类, 记录日志
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午10:57:13
 */
public class CarLogProxy implements Moveable {

    private Moveable car;

    public CarLogProxy(Moveable car) {
        super();
        this.car = car;
    }

    @Override
    public void move() {
        System.out.println("log: Car开始移动");
        car.move();
        System.out.println("log: Car结束移动");
    }

}

/**
 * 测试
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午11:00:13
 */
public class Client {

    public static void main(String[] args) {
        Moveable car = new CarLogProxy(new CarTimingProxy(new Car()));
        car.move();
    }
}
```

## 动态代理

如果需要代理的类只有一个，那么静态代理没什么问题，如果有很多类需要代理呢，用静态代理的话就需要为每一个类创建一个代理类，显然这么做太过繁琐也容易出错。

JDK 5引入的动态代理机制，允许开发人员在运行时刻动态的创建出代理类及其对象。也就是说，我们不用为每个类再单独创建一个代理对象了。

比如我们有两个业务，要为这两个业务添加日志打印功能。如果是静态代理，那么就需要分别为每个业务类写一个代理类，而如果用动态代理，只需要实现一个日志打印功能的handler即可，完全不需要自己再单独写代理类

定义: 在程序运行时, 动态为不同的对象生成代理对象. 动态代理主要是利用了Java的反射机制。

场景: 比如Spring的aop

### 看一个使用示例

```java
public class Client {

    public static void main(String[] args) {
        Car car = new Car();
        // 这里必须使用接口接收, 不然会转型异常
        Moveable proxy = (Moveable) Proxy.newProxyInstance(
                car.getClass().getClassLoader(), // 被代理对象的classloader
                new Class[] {Moveable.class},// 手动列出接口
                new InvocationHandler() {// 请求处理类, 因为知道不会复用, 这里直接使用匿名类

                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        Object rst = null;

                        String methodName = method.getName();
                        if ("move".equals(methodName)) {
                            System.out.println("方法被代理前...");
                            rst = method.invoke(car, args);
                            System.out.println("方法被代理后...");
                        }
                        else {
                            rst = method.invoke(car, args);
                        }
                        return rst;
                    }
                });
        proxy.move();
    }
}

```

### 自定义数据库连接池

```java
// 数据库连接池
// 现在需要手动实现一个数据库连接池, 没有使用代理模式的情况下是这样:

public class MyPool {
 
	private int init_count = 3;        // 初始化连接数目
	private int max_count = 6;        // 最大连接数
	private int current_count = 0;  // 记录当前使用连接数
	// 连接池 （存放所有的初始化连接）
	private LinkedList<Connection> pool = new LinkedList<Connection>();
	//1.  构造函数中，初始化连接放入连接池
	public MyPool() {
		// 初始化连接
		for (int i=0; i<init_count; i++){
			// 记录当前连接数目
			current_count++;
			// 创建原始的连接对象
			Connection con = createConnection();
			// 把连接加入连接池
			pool.addLast(con);
		}
	}
	//2. 创建一个新的连接的方法
	private Connection createConnection(){
		try {
			Class.forName("com.mysql.jdbc.Driver");
			// 原始的目标对象
			 Connection con = DriverManager.getConnection("jdbc:mysql:///jdbc_demo", "root", "root");
			return con;
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}
	//3. 获取连接
	public Connection getConnection(){
		// 3.1 判断连接池中是否有连接, 如果有连接，就直接从连接池取出
		if (pool.size() > 0){
			return pool.removeFirst();
		}
		// 3.2 连接池中没有连接： 判断，如果没有达到最大连接数，创建；
		if (current_count < max_count) {
			// 记录当前使用的连接数
			current_count++;
			// 创建连接
			return createConnection();
		}
		// 3.3 如果当前已经达到最大连接数，抛出异常
		throw new RuntimeException("当前连接已经达到最大连接数目 ！");
	}
	//4. 释放连接
	public void realeaseConnection(Connection con) {
		// 4.1 判断： 池的数目如果小于初始化连接，就放入池中
		if (pool.size() < init_count){
			pool.addLast(con);
		} else {
			try {
				// 4.2 关闭 
				current_count--;
				con.close();
			} catch (SQLException e) {
				throw new RuntimeException(e);
			}
		}
	}

	// 测试：
	public static void main(String[] args) throws SQLException {
		MyPool pool = new MyPool();
		System.out.println("当前连接: " + pool.current_count);  // 3
		// 使用连接
		pool.getConnection();
		pool.getConnection();
		Connection con4 = pool.getConnection();
		Connection con3 = pool.getConnection();
		Connection con2 = pool.getConnection();
		Connection con1 = pool.getConnection();
		// 释放连接, 连接放回连接池
//        pool.realeaseConnection(con1);
	/*
	 * 希望：当关闭连接的时候，要把连接放入连接池！【当调用Connection接口的close方法时候，希望触发pool.addLast(con);操作】把连接放入连接池
	 * 解决1：实现Connection接口，重写close方法   connection接口方法太多，都实现太麻烦，放弃
	 * 解决2：动态代理
	 */
	con1.close();
	// 再获取
	pool.getConnection();
	System.out.println("连接池：" + pool.pool.size());      // 0
	System.out.println("当前连接: " + pool.current_count);  // 3
	}
}


```

采用动态的代理后, 改进如下:

```java
/**
 *  JDK 动态代理  Object obj = Proxy.newProxyInstance(....)；
 *  1.参数1：ClassLoader loader ,确定类加载器。程序运行时动态创建类，需要类加载加载到内存。类加载器作用：class文件 --> Class对象
 *      * 一般情况使用都是当前类的类加载器
 *      * 类加载器获得方式：MyFactory.class.getClassLoader();
 *  2.参数2：Class[] interfaces  代理需要实现的接口们（可能有多个）
 *      * 方式1：userService.getClass().getInterfaces()【此方式只能在代理对象和接口是父子关系时使用】
 *      * 方式2：new Class[]{UserService.class}【当被代理对象和其实现接口之间是隔代关系时（即祖孙关系）(即:一个一个列出接口)
 *  3.参数3：InvocationHandler h 请求处理类，代理类方法执行时，需要请求处理类来处理。
 *      * 一般采用匿名内部类：new InvocationHandler(){}
 *      * 实现方法 invoke ，代理类每一个方法执行一次，将调用一次invoke
 *          参数1.1：Object proxy ,代理对象（即 proxyService，不是“代理之前对象”），一般不用。
 *          参数2.2：Method method ，当前执行的方法
 *              * 当前调用方法名：method.getName();
 *              * 执行目标类方法：Object obj = method.invoke(代理之前对象 , args)
 *          参数3.3：Object[] args
 *              * 当前方法实际参数
 */
public class MyPool {
 
	private int init_count = 3;        // 初始化连接数目
	private int max_count = 6;        // 最大连接数
	private int current_count = 0;  // 记录当前使用连接数
	// 连接池 （存放所有的初始化连接）
	private LinkedList<Connection> pool = new LinkedList<Connection>();
	//1.  构造函数中，初始化连接放入连接池
	public MyPool() {
		// 初始化连接
		for (int i=0; i<init_count; i++){
			// 记录当前连接数目
			current_count++;
			// 创建原始的连接对象
			Connection con = createConnection();
			// 把连接加入连接池
			pool.addLast(con);
		}
	}
	//2. 创建一个新的连接的方法
	private Connection createConnection(){
		try {
			Class.forName("com.mysql.jdbc.Driver");
			// 原始的目标对象
			final Connection con = DriverManager.getConnection("jdbc:mysql:///jdbc_demo", "root", "root");
			/**********对con对象代理**************/
			// 对con创建其代理对象
			Connection proxy = (Connection) Proxy.newProxyInstance(
					con.getClass().getClassLoader(),    // 类加载器
					//con.getClass().getInterfaces(),   // 当目标对象是一个具体的类的时候 
					new Class[]{Connection.class},      // 目标对象实现的接口
					new InvocationHandler() {            // 当调用con对象方法的时候， 自动触发事务处理器
						@Override
						public Object invoke(Object proxy, Method method, Object[] args)
								throws Throwable {
							// 方法返回值
							Object result = null;
							// 当前执行的方法的方法名
							String methodName = method.getName();
							// 判断当执行了close方法的时候，把连接放入连接池
							if ("close".equals(methodName)) {
								System.out.println("begin:当前执行close方法开始！");
								// 连接放入连接池
								pool.addLast(con);
								System.out.println("end: 当前连接已经放入连接池了！");
							} else {
								// 调用目标对象方法，注意这里不是代理对象
								result = method.invoke(con, args);
							}
							return result;
						}
					}
			);
			return proxy;
		} catch (Exception e) {
			throw new RuntimeException(e);
		}
	}
	//3. 获取连接
	public Connection getConnection(){
		// 3.1 判断连接池中是否有连接, 如果有连接，就直接从连接池取出
		if (pool.size() > 0){
			return pool.removeFirst();
		}
		// 3.2 连接池中没有连接： 判断，如果没有达到最大连接数，创建；
		if (current_count < max_count) {
			// 记录当前使用的连接数
			current_count++;
			// 创建连接
			return createConnection();
		}
		// 3.3 如果当前已经达到最大连接数，抛出异常
		throw new RuntimeException("当前连接已经达到最大连接数目 ！");
	}
	//4. 释放连接
	public void realeaseConnection(Connection con) {
		// 4.1 判断： 池的数目如果小于初始化连接，就放入池中
		if (pool.size() < init_count){
			pool.addLast(con);
		} else {
			try {
				// 4.2 关闭 
				current_count--;
				con.close();
			} catch (SQLException e) {
				throw new RuntimeException(e);
			}
		}
	}
	// 测试：
	public static void main(String[] args) throws SQLException {
		MyPool pool = new MyPool();
		System.out.println("当前连接: " + pool.current_count);  // 3
		// 使用连接
		pool.getConnection();
		pool.getConnection();
		Connection con4 = pool.getConnection();
		Connection con3 = pool.getConnection();
		Connection con2 = pool.getConnection();
		Connection con1 = pool.getConnection();
		// 释放连接, 连接放回连接池
//        pool.realeaseConnection(con1);
		/*
		 * 希望：当关闭连接的时候，要把连接放入连接池！【当调用Connection接口的close方法时候，希望触发pool.addLast(con);操作】把连接放入连接池
		 * 解决1：实现Connection接口，重写close方法
		 * 解决2：动态代理
		 */
		con1.close();
		// 再获取
		pool.getConnection();
		System.out.println("连接池：" + pool.pool.size());      // 0
		System.out.println("当前连接: " + pool.current_count);  // 3
	}
}

```

# 装饰器模式

希望给一个对象添加行为, 但是又希望这个改动对调用者透明

这个和适配器模式相似, 但是有区别: 适配器模式的目的在于适配接口，装饰器模式的目的在于动态的添加功能，且可以叠加

一般有两种方式可以实现给一个类或对象增加行为(和 adapter 类似)：

- 继承机制，使用继承机制是给现有类添加功能的一种有效途径，通过继承一个现有类可以使得子类在拥有自身方法的同时还拥有父类的方法。但是这种方法是静态的，用户不能控制增加行为的方式和时机。

- 关联机制，即将一个类的对象嵌入另一个对象中，由另一个对象来决定是否调用嵌入对象的行为以便扩展自己的行为，我们称这个嵌入的对象为装饰器(Decorator)

场景: 带 wrapper 的类, 如 servlet 中的 HttpServletRequestWrapper

```java
//待装饰接口
public interface Component {
    void method();
}
//具体待装饰类
public class ConcreteComponent implements Component{
    public void method() {
        System.out.println("原来的方法");
    }
}
//抽象装饰器父类
public abstract class Decorator implements Component{
    protected Component component;
    public Decorator(Component component) {
        super();
        this.component = component;
    }
    public void method() {
        component.method();
    }
}
//具体装饰器类A
public class ConcreteDecoratorA extends Decorator{
    public ConcreteDecoratorA(Component component) {
        super(component);
    }
    public void methodA(){
        System.out.println("被装饰器A扩展的功能");
    }
    // 重写父类方法, 进行增强
    public void method(){
        System.out.println("针对该方法加一层A包装");
        super.method();
        System.out.println("A包装结束");
    }
}
//具体装饰器类B
public class ConcreteDecoratorB extends Decorator{
    public ConcreteDecoratorB(Component component) {
        super(component);
    }
    public void methodB(){
        System.out.println("被装饰器B扩展的功能");
    }
    public void method(){
        System.out.println("针对该方法加一层B包装");
        super.method();
        System.out.println("B包装结束");
    }
}
```

# 适配器模式

将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的那些类可以一起工作

java.util.Arrays#asList()
java.util.Collections#list()

```java
// 待适配类, 已存在的、具有特殊功能、希望被重用, 但不符合我们既有的标准接口
class Adaptee {
	public void specificRequest() {
		System.out.println("被适配类具有 特殊功能...");
	}
}

// 目标接口，或称为标准接口
interface Target {
	public void request();
}

// 具体目标类，但是只提供普通功能
class ConcreteTarget implements Target {
	public void request() {
		System.out.println("普通类 具有 普通功能...");
	}
}
 
// 适配器类，继承了被适配类，同时实现标准接口
class Adapter extends Adaptee implements Target{
	public void request() {
		super.specificRequest();
	}
}
 
// 测试类public class Client {
	public static void main(String[] args) {
		// 使用普通功能类
		Target concreteTarget = new ConcreteTarget();
		concreteTarget.request();
		
		// 使用特殊功能类，即适配类
		Target adapter = new Adapter();
		adapter.request();
	}
}
```

对象适配器 ---------------- 使用组合(推荐)

```java
// 适配器类，直接关联被适配类，同时实现标准接口
class Adapter implements Target{
	// 直接关联被适配类
	private Adaptee adaptee;
	
	// 可以通过构造函数传入具体需要适配的被适配类对象
	public Adapter (Adaptee adaptee) {
		this.adaptee = adaptee;
	}
	
	public void request() {
		// 这里是使用委托的方式完成特殊功能
		this.adaptee.specificRequest();
	}
}

// 测试类
public class Client {
	public static void main(String[] args) {
		// 使用普通功能类
		Target concreteTarget = new ConcreteTarget();
		concreteTarget.request();
		
		// 使用特殊功能类，即适配类，
		// 需要先创建一个被适配类的对象作为参数
		Target adapter = new Adapter(new Adaptee());
		adapter.request();
	}
}
```

# 责任链模式

责任链通常需要先建立一个单向链表，然后调用方只需要调用头部节点就可以了，后面会自动流转下去。

典型如 流程审批

```java
/**
 * 信息处理接口, 链条上的每个过滤器都实现它
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午5:45:12
 */
public interface IFilter {

    /**
     * 处理信息方法
     * @param rqt 输入
     * @return
     */
    String doFilter(String rqt);
}

/***
 * 职责链, 面向处理节点接口编程, 和具体的处理节点没有关系
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午6:24:56
 */
public class FilterChain implements IFilter {

    private List<IFilter> filterChain =  new ArrayList<IFilter>();

    @Override
    public String doFilter(String rqt) {
        for(IFilter f: filterChain) {
            rqt = f.doFilter(rqt);
        }
        return rqt;
    }

    /**
     * 添加过滤节点
     * @param filter
     * @return
     */
    public FilterChain addFilter(IFilter filter) {
        this.filterChain.add(filter);
        return this;
    }

}

/**
 * 信息处理器, 封装了职责链, 这里相当与一个外观模式, 封装了内部细节,只提供给外部一个process()
 *  完全可以省略
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午6:01:33
 */
@Data
public class MessageProcessor {

    /**
     * 封装的职责链
     */
    private FilterChain fc;
    public MessageProcessor(FilterChain fc) {
        this.fc = fc;
    }

    public String process(String rqt) {
        return fc.doFilter(rqt);
    }

}

/**
 * 具体的处理节点, 将<>替换为[]
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午6:11:58
 */
public class HtmlFilter implements IFilter {

    @Override
    public String doFilter(String rqt) {
        String replace = rqt.replaceAll("<", "[")
                .replaceAll(">", "]");
        return replace;

    }

}

/**
 * 具体的处理节点, 敏感词过滤 将xxx换为yyy
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午6:15:37
 */
public class SensitiveFilter implements IFilter {

    @Override
    public String doFilter(String rqt) {
        return rqt.replaceAll("xxx", "yyy");
    }

}

/**
 * 简单职责链模式client
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午6:17:51
 */
public class Client {

    public static void main(String[] args) {
        String str = "hhh, xxx :) <script>";
        FilterChain fc = new FilterChain().addFilter(new HtmlFilter()).addFilter(new SensitiveFilter());
        MessageProcessor processor = new MessageProcessor(fc);

        String rst = processor.process(str);
        System.out.println(rst);
    }
}
```

===================复杂（请求响应有序）责任链

类比 servlet 中的 filter

```java
/**
 * request
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午8:09:54
 */
@Data
public class Request {

    private String content;
}

/**
 * response
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午8:10:24
 */
@Data
public class Response {

    private String content;
}

/**
 * 节点接口
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午8:08:13
 */
public interface IFilter {

    void doFilter(Request rqt, Response rsp, FilterChain chain);
}

/**
 * filter chain
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午8:20:58
 */
@Data
public class FilterChain implements IFilter {

    /**
     * 节点执行索引, 初始为0; 表示当前执行到哪个filter了
     */
    private int currentFilterIndex = 0;

    /**
     * 过滤节点容器
     */
    private List<IFilter> filters = new ArrayList<IFilter>();

    @Override
    public void doFilter(Request rqt, Response rsp, FilterChain chain) {
        // 当 chain 中的 filter 被遍历执行完, 跳出
        if (this.filters.size() == currentFilterIndex) {
            return;
        }
        else {
            IFilter filter = filters.get(currentFilterIndex);
            // 当前节点获取到后, 马上索引移动到下一个, 不然会循环调用, 一直调用第一个filter, 直到系统崩溃
            currentFilterIndex++;
            filter.doFilter(rqt, rsp, chain);
        }
    }

    public FilterChain addFilter(IFilter filter) {
        this.filters.add(filter);
        return this;
    }

}

/**
 * html filter
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午8:32:39
 */
public class HtmlFilter implements IFilter {

    @Override
    public void doFilter(Request rqt, Response rsp, FilterChain chain) {
        rqt.setContent(
                rqt.getContent().replace("<", "[").replace(">", "]"));
        System.out.println("HtmlFilter: 请求过程中被处理");
        chain.doFilter(rqt, rsp, chain);
        System.out.println("HtmlFilter: 返回过程中被处理");
    }

}


/**
 * sensitive filter
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午8:38:58
 */
public class SensitiveFilter implements IFilter {

    @Override
    public void doFilter(Request rqt, Response rsp, FilterChain chain) {
        rqt.setContent(rqt.getContent().replaceAll("xxx", "yyy"));
        System.out.println("SensitiveFilter: 请求过程被处理");
        chain.doFilter(rqt, rsp, chain);
        System.out.println("SensitiveFilter: 返回过程被处理");
    }

}

/**
 * 测试
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月26日 下午8:43:43
 */
public class Client {

    public static void main(String[] args) {
        String str = "hhh, xxx :) <script>, ";
        Request rqt = new Request();
        rqt.setContent(str);
        Response rsp = new Response();
        rsp.setContent("response-content");

        FilterChain fc = new FilterChain().addFilter(new HtmlFilter()).addFilter(new SensitiveFilter());
        fc.doFilter(rqt, rsp, fc);
        System.out.println(rsp.getContent());
    }
}
```

# 单例模式

某个对象全局只需要一个实例时，就可以使用单例模式。它的优点也显而易见：

- 它能够避免对象重复创建，节约空间并提升效率
- 避免由于操作不同实例导致的逻辑错误


线程安全的实现:

```java
/**
 * 带有双重判断的单例模式 ---- 不直观, 不推荐
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月25日 下午4:48:41
 */
public class SyncSingleton {

  //一个静态的实例
    private static SyncSingleton instance;
  //私有化构造函数
    private SyncSingleton() {}
  //给出一个公共的静态方法返回一个单一实例
    public static SyncSingleton getInstance() {
        if (instance == null) {
          //这里第二重判断：AB两个线程同时进入第一个判断内部，此时A当先拿到锁进入第二判断，创建了对象，B拿到锁后会再次进行判断，如果此处不判断，则会创建第二个对象；
            synchronized (SyncSingleton.class) {
                if (instance == null) {
                    instance = new SyncSingleton();
                }
            }
        }
        return instance;
    }
}

///////////////////////// 推荐这种写法: 借助 jvm 的特性帮助实现 singleton ////////////////////////////////////

/**
 * 更安全, 更简单的单例
 *
 *首先来说一下，这种方式为何会避免了上面莫名的错误，
 *主要是因为一个类的静态属性只会在第一次加载类时初始化，这是JVM帮我们保证的，
 *所以我们无需担心并发访问的问题。所以在初始化进行一半的时候，别的线程是无法使用的，因为JVM会帮我们强行同步这个过程。
 *另外由于静态变量只初始化一次，所以singleton仍然是单例的。
 *
 * @version 0.1
 * @author xy
 * @date 2018年1月25日 下午5:02:23
 */
public class Singleton {
    private Singleton(){}
    public static Singleton getInstance(){
        return SingletonInstance.instance;
    }
    //类的静态成员只加载一次，这保证了只有一个对象
    private static class SingletonInstance{
        private static Singleton instance = new Singleton();
    }
}
```


# 命令模式

# 门面模式

# 建造者模式


