---
title: Spring
date: 2014-01-16 17:11:56
tags: [spring]
categories: java web
---

references： [1](https://docs.spring.io/spring/docs/5.1.2.RELEASE/spring-framework-reference/web.html#filters), [2](https://lfvepclr.gitbooks.io/spring-framework-5-doc-cn/content/32/32-1.html), [3](https://waylau.gitbooks.io/spring-framework-4-reference/)
<!--more-->

<!-- TOC -->

- [拾遗](#拾遗)
  - [classpath\*区别](#classpath\区别)
  - [各种注解怎么理解](#各种注解怎么理解)
- [认识](#认识)
  - [core 部分](#core-部分)
  - [aop 部分](#aop-部分)
  - [data access 部分](#data-access-部分)
  - [web 部分](#web-部分)
  - [test 部分&context-support](#test-部分context-support)
  - [spring4 新增](#spring4-新增)
  - [组件结构](#组件结构)
    - [ioc-container](#ioc-container)
- [api 使用](#api-使用)
- [quickstart](#quickstart)
- [配置的方式](#配置的方式)
  - [基于 xml](#基于-xml)
    - [xml 中的配置项](#xml-中的配置项)
    - [几种注入-实例化方式](#几种注入-实例化方式)
      - [通过构造函数](#通过构造函数)
        - [通过静态工厂方法](#通过静态工厂方法)
        - [通过实例工厂方法](#通过实例工厂方法)
      - [通过 setter 注入](#通过-setter-注入)
      - [通过 MethodInvokingFactoryBean 注入方法的返回值](#通过-methodinvokingfactorybean-注入方法的返回值)
      - [@Autowired 注入](#autowired-注入)
    - [springEL 表达式,注入表达式](#springel-表达式注入表达式)
    - [集合的注入](#集合的注入)
  - [基于注解](#基于注解)
  - [xml 和注解混用](#xml-和注解混用)
- [JUnit 整合](#junit-整合)
- [IoC](#ioc)
- [AOP](#aop)
  - [aop 概念](#aop-概念)
  - [aop 联盟规范](#aop-联盟规范)
    - [通知类型](#通知类型)
    - [Joinpoint 连接点](#joinpoint-连接点)
  - [不和 spring 集成(单独使用动态代理)](#不和-spring-集成单独使用动态代理)
    - [通过 jdk 动态生成代理](#通过-jdk-动态生成代理)
    - [通过 cglib 动态生成代理](#通过-cglib-动态生成代理)
  - [通过 ProxyFactoryBean 生成代理(半自动)](#通过-proxyfactorybean-生成代理半自动)
  - [全自动实现](#全自动实现)
  - [使用 AspectJ 框架生成代理(和 spring 集成)](#使用-aspectj-框架生成代理和-spring-集成)
    - [准备工作](#准备工作)
    - [AspectJ 通知类型](#aspectj-通知类型)
    - [AspectJ 编程,基于 xml](#aspectj-编程基于-xml)
    - [AspectJ 编程,基于注解](#aspectj-编程基于注解)
- [JDBC](#jdbc)
  - [数据库连接池](#数据库连接池)
    - [C3P0](#c3p0)
    - [DBCP](#dbcp)
    - [Druid](#druid)
    - [BoneCP](#bonecp)
    - [HikariCP](#hikaricp)
  - [JdbcTemplate](#jdbctemplate)
    - [quickstart](#quickstart-1)
    - [Dao 实现类继承 JdbcDaoSupport 支持类](#dao-实现类继承-jdbcdaosupport-支持类)
    - [数据库连接配置放入 Properties 文件](#数据库连接配置放入-properties-文件)
- [事务管理](#事务管理)
  - [事务实现原理](#事务实现原理)
  - [TransactionDefinition 事务传播](#transactiondefinition-事务传播)
  - [事务相关 api](#事务相关-api)
    - [PlatformTransactionManager](#platformtransactionmanager)
    - [TransactionStatus](#transactionstatus)
    - [使用 jdbc 操作事务](#使用-jdbc-操作事务)
  - [事务相关案例-转账](#事务相关案例-转账)
    - [准备](#准备)
    - [手动开发事务](#手动开发事务)
    - [半自动事务(生成代理)](#半自动事务生成代理)
    - [全自动事务基于 xml](#全自动事务基于-xml)
    - [全自动事务基于注解](#全自动事务基于注解)
- [spring web 开发](#spring-web-开发)
- [几种加载 xml 方式](#几种加载-xml-方式)
- [spring 中的设计模式](#spring-中的设计模式)
- [spring bean 生命周期](#spring-bean-生命周期)
- [常用的 spring bean 拓展接口](#常用的-spring-bean-拓展接口)
  - [BeanNameAware、ApplicationContextAware 和 BeanFactoryAware](#beannameawareapplicationcontextaware-和-beanfactoryaware)
  - [BeanPostProcessor 接口](#beanpostprocessor-接口)
  - [InitialingBean 和 DisposableBean](#initialingbean-和-disposablebean)
  - [FactoryBean 接口](#factorybean-接口)
    - [FactoryBean 解释](#factorybean-解释)
    - [FactoryBean 接口的 api](#factorybean-接口的-api)
    - [FactoryBean 简单的使用示例](#factorybean-简单的使用示例)
    - [factoryBean 在 mabatis-spring 集成中的应用](#factorybean-在-mabatis-spring-集成中的应用)
    - [factoryBean 案例-dao 切换](#factorybean-案例-dao-切换)
  - [BeanFactory 接口](#beanfactory-接口)
    - [BeanFactory 解释](#beanfactory-解释)
    - [api 有哪些](#api-有哪些)
  - [BeanFactoryPostProcessor](#beanfactorypostprocessor)
  - [InstantiationAwareBeanPostProcessor](#instantiationawarebeanpostprocessor)
- [动态注册 bean](#动态注册-bean)
  - [动态注册场景](#动态注册场景)
  - [动态注册 api](#动态注册-api)
  - [怎么使用动态注册](#怎么使用动态注册)
    - [通过 BeanFactoryPostProcessor 实现动态注册](#通过-beanfactorypostprocessor-实现动态注册)
    - [通过 BeanDefinitionRegistry 在普通 bean 中注册](#通过-beandefinitionregistry-在普通-bean-中注册)
    - [通过 ImportBeanDefinitionRegistrar](#通过-importbeandefinitionregistrar)
- [国际化 and 参数校验](#国际化-and-参数校验)
- [spring5 新特性](#spring5-新特性)
- [Guice](#guice)

<!-- /TOC -->

# 拾遗

## classpath\*区别

classpath 是指 WEB-INF 文件夹下的 classes 目录

编译后, src 下的代码&配置文件 都会被复制到 `/WEB-INF/classes` 文件夹下, 部署到服务器后真正使用的配置文件是 `WEB-INF/web.xml`

`classpath` ：只会到你的 class 路径中查找找文件; `classpath*`：不仅包含 class 路径，还包括 jar 文件中（class 路径）进行查找。

```xml
如果applicationContext.xml配置文件存放在src目录下，就好比上面的代码结构中的存放位置，那么在web.xml中的配置就如下所示：

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value> <!--classpath前缀表示 "/WEB-INF/classes"-->
    </context-param>

如果applicationContext.xml配置文件存放在WEB-INF下面，那么在web.xml中的配置就如下所示：

    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>WEB-INF/applicationContext*.xml</param-value>
    </context-param>

```

## 各种注解怎么理解

references: [常用注解分类汇总](http://blog.leanote.com/post/sheng91666@163.com/spring%E5%B8%B8%E7%94%A8%E6%B3%A8%E8%A7%A3%E6%B1%87%E6%80%BB)

`<context:component-scan base-package="cn.test"/>` 开启注解扫描

要让 JSR-250 的注释生效，除了在 Bean 类中标注这些注释外，还需要在 Spring 容器中注册一个负责处理这些注释的 `org.springframework.context.annotation.CommonAnnotationBeanPostProcessor` bean, 无需 bean id

```java

-> Bean 声明

Component
Controller
Service
Repository

-> Bean的初始化和销毁

@Bean           注解在方法上,声明当前方法的返回值为一个bean; eg: @Bean(initMethod="init",destroyMethod="destory") 注册前执行init方法, 销毁前执行destroy方法。

    JSR-250规范

@PostConstruct  注解在方法上, 在构造函数执行后执行
@PreDestroy     注解在方法上, 在Bean销毁之前执行
@DependsOn      注解在方法上, 定义Bean初始化及销毁时的顺序

-> bean注入

    spring原生提供

@Autowired   按照类型装配, 按名称装配，可以结合@Qualifier 注解一起使用

    JSR250提供的注解

@Resource    默认按名称装配，当找不到与名称匹配的bean才会按类型装配

    JSR330

@Inject  根基类型装配



-> Java配置

@EnableAutoConfiguration  根据添加的jar依赖猜测你想如何配置Spring，如果发现应用了你不想要的特定自动配
                        置类，可以使用 @EnableAutoConfiguration 注解的exclude属性来禁用它们
                        eg：@EnableAutoConfiguration(exclude = {MetricFilterAutoConfiguration.class, MetricRepositoryAutoConfiguration.class})
                        @SpringBootApplication 包含它
@Configuration   声明当前类是一个配置类，相当于一个Spring配置的xml文件。 和@Bean 搭配使用
                理解: 本质上还是 @Component; 通常选定一个主config类, 通过 import属性导入其他配置类 (甚至
                可以是xml配置类, 通过@ImportResource 注解加载XML配置文件)
                @SpringBootApplication 包含它
@Bean            声明当前方法的返回值为一个Bean, 用@Bean标注方法等价于XML中配置bean
@ComponentScan   自动扫描包名下所有使用@Service、@Compent、@Repository、@Controller 的类（主@configuration 类
                需要在root目录下），并注册为Bean, 一般在主@configuration 类上使用
                    通过@autowired 自动注入； @springbootapplication包含它
@Lazy(true)      表示延迟初始化
@Primary         自动装配时当出现多个Bean候选者时，被注解为@Primary的Bean将作为首选者，否则将抛出异常

@Scope           描述的是Spring容器如何新建Bean的实例的。实例：Scope("Prototype")
@Order           调整配置类加载顺序。

@Import          引入另外的@configuration类,  @Import @ImportResource都需要配合@Configuration使用
@ImportResource  用来引入外置的配置文件,
                eg: @ImportResource({"classpath:datasource.xml"})
@PropertySource  springboot默认只会加载application.properties中的属性, @PropertySource用于加载另外的属性配置文件
                eg: @PropertySource(value = {"classpath:person.properties"}),
                一般和@ConfigurationProperties(prefix = "person")一起用
@Value           注解在变量上, 调用资源（普通文件，网址，配置文件，系统环境变量等）,和 @component 合用
                eg： @Value("${com.neo.title}")

-> aop

@Aspect         注解在类上, 声明是一个切面。
@After          注解在方法上, 声明建言
@Before         注解在方法上, 声明建言
@Around         注解在方法上, 声明建言
@PointCut       注解在方法上, 定义拦截规则，声明切点。

@EnableAspectJAutoProxy      作用在配置类上, 开启对AspectJ自动代理的支持。

-> Spring EL 和 资源调用

@PropertySource  见↑

@EnbaleConfigurationProperties   开启对@ConfigurationProperties 注解配置Bean的支持
                                eg: @EnableConfigurationProperties({LiquibaseProperties.class, ApplicationProperties.class})
@ConfigurationProperties         注解在类上, 映射配置文件（支持 yml）到JavaBean eg：@ConfigurationProperties(prefix="myProps")
                        还可以在@Bean方法上使用， 作用类似注解在类上
                                另外， 当一个 @ConfigurationProperties bean 像如上这样注册，这个bean就有了一个名字，规
                                范是这样：`<prefix>-<fqn>`, 即：前缀-全限定名，如 : myProps-com.xy.PropertiesBean

-> 事务,缓存

@EnableTransactionManagement                    开启注解式事务的支持, 可能需要 proxyTargetClass = true, 表示创建基于子类的代理
@Transactional(transactionManager="xxx" ...)     方法or类上, 使用事务管理 ;@TransactionConfiguration 过时了, 被 @Transactional 代替

@EnableCaching                                  开启注解式的缓存支持

-> 异步,多线程

@EnableAsync    注解在配置类上, 开启对异步任务的支持
@Async          在实际执行的Bean的方法上, 声明这是一个异步任务

-> 定时任务

EnableScheduling    注解在配置类上, 开启对计划任务（定时任务）的支持
Scheduled           实际执行的方法上, 包含：cron 、fixDelay、fixRate等类型
                    eg: @Scheduled （cron="0 25 11 ？ * *"）
                    @Scheduled （fixedDelay=5000）延迟5秒执行
                    @Scheduled （fixedRate=5000）每隔5秒执行一次

-> 条件注解

Conditional         用在配置类中, 根据满足某一个特定条件创建一个特定的Bean。（Spring4提供）

-> WebMVC

EnableWebMvc        开启web mvc的配置支持
Controller          声明一个类为 mvc 里的 controller, Dispatcher Servlet会自动扫描注解了此注解的类，并将Web请求映射到@RequestMappin的方法上
RestController      组合注解 =@Controller+@ResponseBody, 用来创建REST类型的控制器
RequestMapping      类or方法上
ResponseBody        类or方法or方法返回值上; 将返回类型直接输入到HTTP response body中。json格式
RequestBody         参数上; 设置参数在请求体内
@RequestParam       参数上;将请求的参数绑定到方法中的参数上
PathVariable        参数上, 支持使用正则表达式,能在路径模板中使用占位符; 接受路径参数
InitBinder          用来设置WebDataBinder，WebDataBinder用来自动绑定前台请求参数到Model中。
ModelAttribute      方法or方法参数;
HttpEntity

ControllerAdvice    类上; 将对于控制器的全局代码放在同一个位置
ExceptionHandler    方法上; 全局处理控制器里的异常

-> test 测试相关

@RunWith(SpringJUnit4ClassRunner.class)  启动 Spring 对测试类的支持
@ContextConfiguration                    注释标签来指定 Spring 配置文件或者配置类的位置
@Rollback(value=false)                  事务回滚

```

# 认识

spring 的体系结构

先看看 spring3 的:

<img src="p1.png">

spring4: (去掉了 spring3 的 struts，添加了 messaging 和 websocket，其他模块保持不变)

![alt](Snipaste_2018-07-27_18-48-06.png)

至于 spring5 的新特性, 参见: https://www.ibm.com/developerworks/cn/java/j-whats-new-in-spring-framework-5-theedom/index.html

## core 部分

<img src="20161227105801579.png">

-   四个核心 jar

    -   Spring core

        这个 jar 文件包含 Spring 框架基本的核心工具类。Spring 其它组件要都要使用到这个包里的类，是其它组件的基本核心

        > Spring core 的运行依赖 Commons Logging 包

    -   Spring Beans

        这个 jar 文件是所有应用都要用到的，它包含访问配置文件、创建和管理 bean 以及进行 IOC/DI (控制反转/依赖注入)操作相关的所有类

        > 如果只需基本的 IOC/DI 支持，引入 spring-core.jar 及 spring-beans.jar 文件就可以了。
        > 如果是 maven 工程, 仅仅需要 Spring Context 即可

    -   Spring AOP

        这个 jar 文件包含在应用中使用 Spring 的 AOP 特性时所需的类和源码级元数据支持。使用基于 AOP 的 Spring 特性，如声明型事务管理（Declarative Transaction Management），也要在应用里包含这个 jar 包

    -   Spring Context

        这个 jar 文件为 Spring 核心提供了大量扩展。可以找到使用 Spring ApplicationContext 特性时所需的全部类，JDNI 所需的全部类，instrumentation 组件以及校验 Validation 方面的相关类。

        > 注:
        > spring-context-support: context 拓展包, 包含支持 mvc 相关, 缓存 Cache（ehcache）、JCA、JMX、 邮件服务（Java Mail、COS Mail）、任务计划 Scheduling（Timer、Quartz）方面的类

## aop 部分

<img src="Snipaste_2018-07-27_18-37-42.png" />

-   spring-aop：面向切面编程

-   spring-aspects：集成 AspectJ

## data access 部分

![alt](20161227105923275.png)

-   spring-jdbc：jdbc 的支持

-   spring-tx：事务控制

-   spring-orm：对象关系映射，集成 orm 框架

-   spring-oxm：对象 xml 映射

-   spring-jms：java 消息服务

## web 部分

![alt](Snipaste_2018-07-27_18-41-55.png)

-   spring-web：基础 web 功能，如文件上传

-   spring-webmvc：mvc 实现

## test 部分&context-support

![alt](20161227110008855.png)

-   spring-test：spring 测试，提供 junit 与 mock 测试功能

-   spring-context-support：spring 额外支持包，比如邮件服务、视图解析等

## spring4 新增

![alt](20161227110103559.png)

-   spring-websocket：为 web 应用提供的高效通信工具

-   spring-messaging：用于构建基于消息的应用程序

## 组件结构

### ioc-container

```java
BeanFactory - 顶级接口
    ApplicationContext - 代表了  Spring IoC 容器
        ClassPathXmlApplicationContext -
        FileSystemXmlApplicationContext

FactoryBean - 实现这个接口的bean 时特殊的 "工厂bean", getObject() 用来返回生产的对象

```

# api 使用

<img src="p2.png">

```java
public class TestDI {
    @Test
    public void demo01(){
        //依赖注入
        String xmlPath = "config/beans.xml";
        //加载完配置文件就构造了BookService对象
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
        BookService bookService = applicationContext.getBean("bookServiceId",BookService.class);
        bookService.addBook();
    }
    @Test
    public void demo02(){
        //beanFactory
        String xmlPath = "config/beans.xml";
        //Resource 在 beanFactory中用于描述文件位置
        // * ClassPathResource 描述类路径
        // * FileSystemResource 描述指定盘符
        BeanFactory beanFactory = new XmlBeanFactory(new ClassPathResource(xmlPath));
        //调用getBean()时，才构造BookService对象
        BookService bookService = beanFactory.getBean("bookServiceId",BookService.class);
        bookService.addBook();
    }

}

```

# quickstart

引入依赖

```xml
<!-- **************spring start******************************* -->
      <!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-core</artifactId>
          <version>${springframework.version}</version>
      </dependency>
      <!-- https://mvnrepository.com/artifact/org.springframework/spring-beans -->
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-beans</artifactId>
          <version>${springframework.version}</version>
      </dependency>
      <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
          <version>${springframework.version}</version>
      </dependency>
      <!-- https://mvnrepository.com/artifact/org.springframework/spring-context-support -->
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context-support</artifactId>
          <version>${springframework.version}</version>
      </dependency>
      <!-- https://mvnrepository.com/artifact/org.springframework/spring-web -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${springframework.version}</version>
        </dependency>

      <!-- ****************spring end******************** -->
```

创建 applicationContext.xml (默认名称)

从 Spring 文档 copy 配置文件的约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">

</beans>

```

配置文件内容

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--
    * id属性可以自动提示(一般使用id属性), name属性则没有
    * id只能有一个值, name可以有多个(逗号分隔)
    * 标识bean的作用域  <bean id="" class="", scope=""></bean>
     -->
    <bean id="userServiceId" class="com.xiaoyu.spring.learn.UserService"></bean>

</beans>

```

代码

```java
public class UserService {
    public void addUser(){
        System.out.println("a_hello addUser");
    }
}
public class TestHello {
    @Test
    public void demo01(){     //这是原来的写法
        UserService userService = new UserService();
        userService.addUser();
    }
    @Test
    public void demo02(){
        //1 加载配置文件
        String xmlPath = "config/applicationContext.xml";
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
        //2 从容器获得对象
        UserService userService = (UserService) applicationContext.getBean("userServiceId");
    //或者这样写
     UserService userService = applicationContext.getBean("userServiceId", UserService.class);
        userService.addUser();
    }

}

```

看一个 DI 的例子

```xml
<!--beans.xml :-->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--
        依赖注入：spring提供 <property> 用于配置setter方法进行操作
            格式：<property name="" ref="">
                name 当前操作对象属性名（即通过setter方法获得）
                ref 需要设置对象的bean 名称引用（即<bean>标签的id值）。
    -->
    <!-- spring 创建dao -->
    <bean id="bookDaoId" class="com.xiaoyu.spring.learn.BookDaoImpl"></bean>

    <!-- spring 创建service ,
        * 单实例可以通过lazy-init=”true”，在getBean时进行实例化
        * 对于prototype，则是在getBean的时候被实例化的。
        * 在beans里面default-lazy-init=”true”对所有bean进行延迟处理
        * init-method　在bean被实例化之后执行资源操作等方法
        * destroy-method 在bean销毁之前执行的方法(什么时候被销毁？随着spring容器被关闭时被销毁。调用spring容器的close方法来正常关闭。以前是随着应用程序执行完而关闭。)
        * InitializingBean接口、Disposable接口底层使用类型强转.方法名()进行直接方法调用，init-method、destory-method底层使用反射，前者和Spring耦合程度更高但效率高，后者解除了和Spring之间的耦合但是效率低
    -->
    <bean id="bookServiceId" class="com.xiaoyu.spring.learn.BookServiceImpl" lazy-init="true" init-method="init" destroy-method="destory">
        <!-- 注入属性值, 必须要有setter -->
        <!-- <property> 注入（设置）数据
            * name 使用属性名
            * value 设置普通数据
            * ref 引用类型（另一个bean id值）
    -->
        <property name="bookDao" ref="bookDaoId"></property>  <!-- 表示BookService里面的bookDao属性 -->
        <property name="name" value="123"/>
        <!-- 下面是另一种方式注入 -->
         <!--
        <constructor-arg> 配置构造方法的一个参数
            * name :参数的名称 ，一般不使用
                <constructor-arg name="username" value="jack"></constructor-arg>
                <constructor-arg name="age" value="18"></constructor-arg>
            * index : 参数的索引，从0开始
            * type : 参数类型
            ## 开发中一般index和type结合使用
            * value 普通数据
            * ref 引用数据（另一个bean id值）
        -->
        <!-- 对应构造方法
        public PersonServiceBean(String name, IDaoBean personDao) {
            this.name = name;
            this.personDao = personDao;
        } -->
        <constructor-arg index="0" value="构造注入的name" />
        <constructor-arg index="1" type="com.persia.IDaoBean" ref="personDao"/>


        <!-- 集合的注入 -->
        <!-- Set的注入 -->
        <property name="sets">
        <set>
            <value>sets：第一个值</value>
            <value>sets：第二个值</value>
            <value>sets：第三个值</value>
        </set>
        </property>

        <!-- List的注入 -->

        <property name="lists">
        <list>
            <value>lists：第一个值</value>
            <value>lists：第二个值</value>
            <value>lists：第三个值</value>
        </list>
        </property>

        <!-- Properties的注入 -->

        <property name="properties">
        <props>
            <prop key="props-key1">：第一个值</prop>
            <prop key="props-key2">：第二个值</prop>
            <prop key="props-key3">：第三个值</prop>
        </props>
        </property>

        <!-- Map的注入 -->

        <property name="maps">
        <map>
            <entry key="maps-key1" value="：第一个值" />
            <entry key="maps-key2" value="：第二个值" />
            <entry key="maps-key3" value="：第三个值" />
        </map>
        </property>

    </bean>

</beans>
```

```java
// BookDaoImpl & BookDao
public interface BookDao {
        public void save();
}
public class BookDaoImpl implements BookDao {
        @Override
        public void save() {
                System.out.println("b_di  book save");
        }
}


// BookService & BookServiceImpl
public interface BookService {
        public void addBook();
}
public class BookServiceImpl implements BookService {
    public BookServiceImpl() {
        System.out.println("你生了");
    }

    //之后编程
    private BookDao bookDao;
    public void setBookDao(BookDao bookDao) {
        this.bookDao = bookDao;
    }
    @Override
    public void addBook() {
        bookDao.save();
    }
}


// 测试：
@Test
public void demo01(){
    //依赖注入
    String xmlPath = "config/applicationContext.xml";
    //加载完配置文件就构造了BookService对象
    ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
    BookService bookService = applicationContext.getBean("bookServiceId",BookService.class);
    bookService.addBook();
}

```

# 配置的方式

## 基于 xml

### xml 中的配置项

-   id 属性可以自动提示(一般使用 id 属性), name 属性则没有

-   id 只能有一个值, name 可以有多个(逗号分隔)

-   标识 bean 的作用域 `<bean scope=""></bean>`

    -   singleton - 整个容器中，将只有一个实例， 对于那些无会话状态的 Bean（如辅助工具类、DAO 组件、业务逻辑组件等）是最理想的选择。

    -   prototype - 每次通过容器的 getBean 方法获取定义的 Bean 时，都将产生一个新的 Bean 实例， 适用于那些需要保持会话状态的 Bean 实例

    -   request - 对于每次 HTTP request，定义的 Bean 都将产生一个新实例； 只在 web app 中有效

    -   session - 对于每次 HTTP Session，定义的 Bean 都将产生一个新实例。同样只只在 web app 中有效

    -   globalsession：存疑 //todo

<img src="p3.png">

### 几种注入-实例化方式

https://blog.csdn.net/a909301740/article/details/78379720

#### 通过构造函数

```java
public class UserService implements IUserService {

    private IUserDao userDao;

    public UserService(IUserDao userDao) {
        this.userDao = userDao;
    }

    public void loginUser() {
        userDao.loginUser();
    }

}

```

```xml
<!-- 注册userService
    多个 field 注入 通过 name 属性指定
-->
<bean id="userService" class="com.lyu.spring.service.impl.UserService">
    <constructor-arg ref="userDaoJdbc"></constructor-arg>
</bean>
<!-- 注册jdbc实现的dao -->
<bean id="userDaoJdbc" class="com.lyu.spring.dao.impl.UserDaoJdbc"></bean>

```

##### 通过静态工厂方法

属于构造函数注入

使用场景: 与其他框架整合时用

类似参考 [特殊的 bean-FactoryBean](#特殊的bean-FactoryBean)

```java
// PersonService：
public class PersonService {
    public void addPerson(){
        System.out.println("Personservice  addPerson");
    }

}

// 静态工厂：配置到 bean#class
public class MyBeanFactory {
    // 所有方法都是静态方法: 方法名配置到 bean#factory-method
    public static PersonService createPerson(){
        return new PersonService();
    }
}

// 测试：
public class TestStatic {
    @Test
    public void demo01(){
        String xmlPath = "config/beans.xml";
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
        PersonService personService = (PersonService) applicationContext.getBean("personServiceId");
        personService.addPerson();
    }
}

```

xml 配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--  id表示静态工厂要生产的bean -->
<!--  class表示静态工厂类 -->
<!-- factory-method 表示静态工厂生产本bean的静态方法 -->

    <bean id="personServiceId"
        class="com.xiaoyu.spring.learn.MyBeanFactory"
        factory-method="createPerson">
    </bean>

    <!--test: static factory construct 工厂方法带有参数-->
    <bean id="bean3" class="spring.BeanFactory" factory-method="createBean3">
        <!--factory method args， bean可以通过 ref 注入到方法参数中-->
        <constructor-arg name="bean4" ref="bean4"></constructor-arg>
        <constructor-arg name="str" value="haha" />
    </bean>

</beans>

```

##### 通过实例工厂方法

属于构造函数注入

```java
// studentService：
public class StudentService {
    public void addStudent(){
        System.out.println("StudentService  addStudent");
    }
}
// 实例工厂：
public class MyBeanFactory2 {
    /**
     * 非静态方法
     * @return
     */
    public StudentService createStudent(){
        return new StudentService();
    }
}

// 测试：
public class TestFactory {
    @Test
    public void demo01(){
        String xmlPath = "config/beans.xml";
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
//直接获得studentService实例，实例工厂的实例对象spring自动创建
        StudentService studentService = (StudentService) applicationContext.getBean("studentServiceId");
        studentService.addStudent();
    }
}

```

```xml
<!-- 配置： -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- 配置工厂实例 -->
    <bean id="myFactoryId" class="com.xiaoyu.spring.learn.MyBeanFactory2"></bean>

    <!-- 配置 studentService实例 -->
    <bean id="studentServiceId" factory-bean="myFactoryId"  factory-method="createStudent" ></bean>

    <bean id="instanceFac" class="spring.InstanceFactory" />
    <bean id="bean3" factory-bean="instanceFac" factory-method="createBean3" >
        <constructor-arg name="bean4" ref="bean4" />
        <constructor-arg name="str" value="hoho" />
    </bean>
</beans>

```

#### 通过 setter 注入

(setter injection)

```xml
<!-- 注册userService -->
<bean id="userService" class="com.lyu.spring.service.impl.UserService">
 <!-- 写法一 -->
 <!-- <property name="UserDao" ref="userDaoMyBatis"></property> -->
 <!-- 写法二 -->
 <property name="userDao" ref="userDaoMyBatis"></property>
</bean>

<!-- 注册mybatis实现的dao -->
<bean id="userDaoMyBatis" class="com.lyu.spring.dao.impl.UserDaoMyBatis"></bean>

```

上面这两种写法都可以,spring 会将 name 值的每个单词首字母转换成大写，然后再在前面拼接上"set"构成一个方法名,然后去对应的类中查找该方法,`通过反射调用`,实现注入

如果通过 set 方法注入属性，那么 spring 会通过默认的空参构造方法来实例化对象，所以如果在类中写了一个带有参数的构造方法，`一定要把空参数的构造方法写上`，否则 spring 没有办法实例化对象，导致报错

xml 配置方式之外, 还提供注解的方式, 也是需要 setter 方法;

#### 通过 MethodInvokingFactoryBean 注入方法的返回值

MethodInvokingFactoryBean 可以注册某个方法的返回值到 spring 中, 这个方法可以是实例方法, 也可以是静态方法;

当然也可以就使用普通的静态工厂注入 or 实例工厂注入， 但是不够灵活

```java
import org.springframework.context.MessageSource;

import plm.common.exceptions.CheckException;

// 类无需标注为 component
public class CheckUtil {

  private static MessageSource resources;

    // 注入到这个方法的参数中了
    // 没有使用返回值
  public static void setResources(MessageSource resources) {
    CheckUtil.resources = resources;
  }

  public static void check(boolean condition, String msgKey, Object... args) {
    if (!condition) {
      fail(msgKey, args);
    }
  }

  public static void notEmpty(String str, String msgKey, Object... args) {
    if (str == null || str.isEmpty()) {
      fail(msgKey, args);
    }
  }

  public static void notNull(Object obj, String msgKey, Object... args) {
    if (obj == null) {
      fail(msgKey, args);
    }
  }

  private static void fail(String msgKey, Object... args) {
    throw new CheckException(resources.getMessage(msgKey, args, UserUtil.getLocale()));
  }
}

```

xml 配置

```xml
<bean
  class="org.springframework.beans.factory.config.MethodInvokingFactoryBean">
  <property name="staticMethod" value="plm.common.utils.CheckUtil.setResources" />
  <!-- 这里配置方法参数 -->
  <property name="arguments" ref="messageSource">
  </property>
</bean>

<!-- 国际化 -->
<bean id="messageSource"
  class="org.springframework.context.support.ResourceBundleMessageSource">
  <property name="basenames">
    <list>
      <value>format</value>
      <value>exceptions</value>
      <value>windows</value>
    </list>
  </property>
</bean>

```

或者不通过 xml 配置， 使用代码配置：

在 Configuration class 中

```java
/**
    * inject messageSource into I18nUtils
    */
@Bean
public MethodInvokingFactoryBean methodInvokingFactoryBean() {
    MethodInvokingFactoryBean methodInvokingFactoryBean = new MethodInvokingFactoryBean();
    methodInvokingFactoryBean.setStaticMethod("io.github.xiaoyureed.core.util.I18nUtils.setMessageSource");
                                // 这里设置使用哪个方法注入
    methodInvokingFactoryBean.setArguments(this.messageSource());// 在这里设置注入的参数
    return methodInvokingFactoryBean;
}

```

#### @Autowired 注入

底层是使用了反射, 不依赖构造函数, 不依赖 setter

@Autowired 有个属性为 required，可以配置为 false (默认 true)，如果配置为 false 之后，当没有找到相应 bean 的时候，系统不会抛错

```
@Resource默认是按照名称来装配注入的，只有当找不到与名称匹配的bean才会按照类型来装配注入；

@Autowired默认是按照类型装配注入的, 若同类型bean有多个则按照名称注入，如果想直接按照名称来转配注入，则需要结合@Qualifier一起使用；

@Resource注解是又J2EE提供，而@Autowired是由Spring提供，故减少系统对spring的依赖建议使用@Resource的方式；

@Resource和@Autowired都可以书写标注在字段或者该字段的setter方法之上。

@Inject是JSR330规范实现的, 根据类型进行自动装配的，如果需要按名称进行装配，则需要配合@Named
```

### springEL 表达式,注入表达式

```java
// 实体类Person：
public class Person {
    private String name = "jack";
    private Double pi; //java.lang.Math类中的静态常量
    private Address homeAddress;

//    setters&getters..

    @Override
    public String toString() {
        return "Person [name=" + name + ", pi=" + pi + ", homeAddress="
                + homeAddress + "]";
    }

}
// Person中的Address类：
public class Address {
    private String addr;
    private String tel;

//    getters&setters

    @Override
    public String toString() {
        return "Address [addr=" + addr + ", tel=" + tel + "]";
    }
}


```

```xml
<!-- 配置： -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- 表达式
        * #{数字} 、#{'字符串'} : 常量
        * #{bean的id值} : 获得引用
        * #{对象.属性}  ：获得属性值
        * #{对象.方法()}  ： 执行方法
        * #{对象.方法()?.方法()} : 执行方法, 避免null异常
        * #{T{类}.方法() | 属性} ：静态资源
    -->
    <bean id="personId" class="com.xiaoyu.spring.learn.Person">
        <property name="name" value="#{personId.name.toUpperCase()}"></property>
        <property name="pi" value="#{T(java.lang.Math).PI}"></property>
        <property name="homeAddress" value="#{addressId}"></property>
    </bean>

    <bean id="addressId" class="com.xiaoyu.spring.learn.Address"></bean>
</beans>

```

### 集合的注入

```java
// 实体类：
public class CollBean {
    private List<String> dataList;
    private String[] dataArray;
    private Set<String> dataSet;
    private Map<String, String> dataMap;
    private Properties dataProps;

   getters&setters

    @Override
    public String toString() {
        return "CollBean [\ndataList=" + dataList + ", \ndataArray="
                + Arrays.toString(dataArray) + ", \ndataSet=" + dataSet
                + ", \ndataMap=" + dataMap + ", \ndataProps=" + dataProps + "\n]";
    }
}
// 测试：
public class TestColl {
    @Test
    public void demo01() throws Exception{
        String xmlPath = "config/beans.xml";
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
        CollBean collBean = (CollBean) applicationContext.getBean("collBeanId");
        System.out.println(collBean);
    }
}

```

```xml
<!-- 配置： -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--
<property name="">
* 如果要注入数据进集合，且该集合只有一个值时，可以直接使用<property value="">标签的value属性赋值
* <property> 提供子标签
    <value> 子标签 等效 value属性 <property value="">
    <ref> 子标签 等效ref属性
    <list>子标签 list集合
    <array>
    <set>
    <map> map集合
        方式1：
        <entry key="ds" value="屌丝"></entry>
        方式2：
        <entry>
            <key><value>dzd</value></key>
            <value>屌中屌</value>
        </entry>
    <props> Properties对象
        <prop key="键">值</prop>
    -->
    <bean id="collBeanId" class="com.xiaoyu.spring.learn.CollBean">
        <property name="dataList">
            <list>
                <value>a</value>
                <value>b</value>
                <value>c</value>
            </list>
        </property>
        <property name="dataArray">
            <array>
                <value>a</value>
                <value>b</value>
                <value>c</value>
            </array>
        </property>
        <property name="dataSet">
            <set>
                <value>嬲</value>
                <value>嫐</value>
                <value>挊</value>
                <value>龘</value>
            </set>
        </property>
        <property name="dataMap">
            <map>
                <entry key="ds" value="efe"></entry>
                <entry>
                    <key><value>dzd</value></key>
                    <value>dfd</value>
                </entry>
            </map>
        </property>
        <property name="dataProps">
            <props>
                <prop key="DG1">dg</prop>
                <prop key="DG2">dg</prop>
                <prop key="DG3">dg</prop>
            </props>
        </property>
    </bean>

</beans>

```

## 基于注解

-   添加约束: 即 context schema,在 xsd-config.xml 中找

-   开启注解类扫描`<context:component-scan base-package="xxxxpackage"></context:component-scan>`

    -   功能是将@component 之类的注解标注的 bean 注册到 spring 中

    -   annotation-config 处理@autowired 之类的注解（共有四类） 前提是注解作用的类已经被注册到 spring 容器里（bean id=“” class=“”） ;component-scan 除了包含 annotation-config 的作用外，还能自动扫描和注册 base-package 下有@component 之类注解的类，将其作为 bean 注册到 spring 容器里; 所以配置文件如果有 component-scan 就不需要 annotation-config 了。

    -   也可以 scan 排除:

        ```xml
        <context:component-scan base-package="com.baobaotao">
            <context:include-filter type="regex"
                expression="com\.baobaotao\.service\..*"/>
            <context:exclude-filter type="aspectj"
                expression="com.baobaotao.util..*"/>
        </context:component-scan>

        ```

*   分层 web 开发中的注解: @Controller--web 层, @Service--service 层, @Repository--dao 层, @component--笼统的

*   依赖注入相关注解:

    -   按照类型注入: @Autowire, 使用在成员属性 or setter 方法上

    -   按照名称注入 : @Qualified 和@Autowired 结合使用, @Qualified("userDaoId") 【前提是 UserDao 配置了@Repository("userDaoId") 】

    -   按照名称注入（简化）：@Resource（name=“名称”）

    -   普通成员属性注入：@Value（“值”） 直接给成员属性注入普通值

-   初始化，销毁，作用域注解

    -   初始化和销毁: @PostConstruct, @PreDestroy

    -   作用域: @Scope("singleton"), @Scope("prototype")

分层开发:

```java
// UserDao：
@Repository("userDaoId")
public class UserDao {

    @Value("jack")  //直接设置值
    private String username ;
    public void save() {
        System.out.println("web dao user -->" + username);
    }

}
// UserService：
@Service
public class UserService {
    //也可以在这里配置【成员属性上配置@Autowired】
    private UserDao userDao;
    //方式1
    @Autowired//---【根据类型匹配，可单独使用】：如果是配置@Autowired【后面不用跟值】，则UserDao类只需要配置@Repository【不用跟值】
    @Qualifier("userDaoId")//---【根据名字匹配，必须和@Autowired一起使用】：UserDao类配置了@Repository("userDaoId")，则要使用这种方式
    //方式2：
    @Resource(name="userDaoId")            //--一个顶俩【一个@Resource(name="userDaoId")可顶@Autowired&&@Qualifier("userDaoId")】
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void saveUser() {
        System.out.println("web user service");
        userDao.save();
    }

}
// UserAction：
@Controller("userActionId")
public class UserAction {
    @Autowired  //相当于 <property  name="" ref="xxx">
    private UserService userService;
    //也可以在这里配置@Autowired ，和在成员属性上配置效果一样
    public void setUserService(UserService userService) {
        this.userService = userService;
    }

    public void addUser() {
        System.out.println("action add user");
        userService.saveUser();
    }

}
// 测试：
public class TestAnnoWeb {
    @Test
    public void demo01() throws Exception{
        String xmlPath = "com/itheima/h_annotation/b_web/beans.xml";
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
        UserAction userAction = (UserAction) applicationContext.getBean("userActionId");
        userAction.addUser();
    }
}

```

配置

```xml
配置：
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 注解类扫描：通知spring，哪些类添加注解,属于控制反转 -->
    <context:component-scan base-package="com.itheima.h_annotation.b_web"></context:component-scan>
</beans>

```

初始化, 销毁, 作用域:

```java
// CategoryService：
@Component("categoryServiceId")
@Scope("prototype")                  //-------------作用域注解
public class CategoryService {

    public void addCategory(){
        System.out.println("c_other addCategory");
    }
    @PostConstruct //--------初始化注解
    public void myInit(){
        System.out.println("anno 初始化");
    }
    @PreDestroy //------------销毁注解
    public void myDestroy(){
        System.out.println("anno 销毁");
    }
}
// 测试：
public class TestAnnoHello {
    @Test
    public void demo01() throws Exception{
        String xmlPath = "config/beans.xml";
        ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
        CategoryService categoryService1 = (CategoryService) applicationContext.getBean("categoryServiceId");
        categoryService1.addCategory();
        CategoryService categoryService2 = (CategoryService) applicationContext.getBean("categoryServiceId");
        categoryService2.addCategory();
        System.out.println(categoryService1);
        System.out.println(categoryService2);//给CategoryService类加了注解@Scope("prototype")；变成了多例；打印结果是两者地址不同
        applicationContext.close();  //需要手动关闭applicationContext，销毁注解才会生效
    }
}

```

```xml
<!-- 配置 -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 注解类扫描：通知spring，哪些类添加注解 -->
    <context:component-scan base-package="com.xiaoyu.spring.learn"></context:component-scan>
</beans>

```

## xml 和注解混用

-   xml 负责配置 bean(所以可以去掉`context:component-scan`配置), 注解负责配置注入(所以开启`<context:annotation-config/>`)

见 [基于注解](#基于注解)

```java
// UserDao:
public class UserDao {
    @Value("jack")
    private String username ;
    public void save() {
        System.out.println("web dao user -->" + username);
    }
}
// UserService:
public class UserService {
    private UserDao userDao;
    //方式1：（掌握）值为bean id值
    @Autowired
    @Qualifier("userDaoId")
    //方式2：
//  @Resource(name="userDaoId")
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }
    public void saveUser() {
        System.out.println("web user service");
        userDao.save();
    }
}
// UserAction:
public class UserAction {
    @Autowired  //取代 <property  name="" ref="按照类型配置">
    private UserService userService;
    public void setUserService(UserService userService) {
        this.userService = userService;
    }
    public void addUser() {
        System.out.println("action add user");
        userService.saveUser();
    }
}
// 测试：
public class TestAnnoWeb {
    @Test
    public void demo01() throws Exception{
        String xmlPath = "config/beans.xml";
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
        UserAction userAction = (UserAction) applicationContext.getBean("userActionId");
        userAction.addUser();
    }
}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">
    <!--
        已经配置bean，不需要扫描package了，
    <context:component-scan base-package="com.itheima.h_annotation.b_web"></context:component-scan>
    -->
    <!-- 【xml&注解混用时要特别配置】：配置使得注解式注入可以使用 -->
    <context:annotation-config></context:annotation-config>
    <!-- dao -->
    <bean id="userDaoId" class="com.xiaoyu.spring.learn.UserDao"></bean>
    <!-- service -->
    <bean id="userServiceId" class="com.xiaoyu.spring.learn.UserService"></bean>
    <!-- action -->
    <bean id="userActionId" class="com.xiaoyu.spring.learn.UserAction" scope="prototype"></bean>
</beans>

```

# JUnit 整合

https://www.ibm.com/developerworks/cn/java/j-lo-springunitest/index.html

引入依赖: http://mvnrepository.com/artifact/org.springframework/spring-test

```java
// UserService：
public class UserService {
    public void addUser(){
        System.out.println("a_hello addUser");
    }
}
// 测试：
@RunWith(SpringJUnit4ClassRunner.class)  //junit spring 整合
@ContextConfiguration(locations="classpath:config/beans.xml")  //加载配置文件 “classpath:”表示在/src目录下（也就是在WEB-INF/classes下）
@Transactional//insert 之后不必清理数据库, 会自动 rollback
public class TestJunit {
    @Autowired  //整合junit 自动注入(以类型方式注入)，“注入”不需要再在xml配置【】
    private UserService userService;
    @Test
    public void demo02(){
        //1 加载配置文件
//      String xmlPath = "com/itheima/i_test/beans.xml";
//      ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
//
//      //2 从容器获得对象
//      UserService userService = (UserService) applicationContext.getBean("userServiceId");

        userService.addUser();//现在简单一步搞定
    }
}

```

配置

```xml
<!-- 配置： -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--
    这里是配置的UserService这个bean，不是配置的属性注入
-->
    <bean id="userServiceId" class="com.xiaoyu.spring.learn.UserService"></bean>

</beans>

```

# IoC

Spring 中的 IoC 的实现原理就是工厂模式加反射机制。

[ioc 有什么好处](https://www.zhihu.com/question/23277575/answer/169698662)

# AOP

## aop 概念

在不修改源代码的情况下, 增强某个方法, 比如在这个方法逻辑执行之前, 之后, 前后, 返回值返回之后, 抛出异常之后

目前主流有两种方法

-   Spring AOP:使用纯 Java 实现，`不需要专门的编译器`，在`运行期`通过动态生成代理类, 如果类实现了接口, 使用 jdk 自带的动态代理, 如果类没有实现任何接口, 使用 cglib 生成动态代理;Spring AOP 虽然使用了 AspectJ 的 Annotation, 但是两者是不同的

-   AspectJ: 是一个基于 Java 语言的 AOP 框架,可以单独使用，Spring 2.0 开始，Spring 引入对 AspectJ 的支持，AspectJ 扩展了 Java 语言，提供了一个`专门的编译器`(相当于 javac 的增强版本)，在`编译时织入`, 而不是运行是动态生成;

经典应用

权限管理, 错误处理, 懒加载, 性能监视、事务管理、安全检查、缓存 、日志

名词概念

```
1.target 目标类 ，被增强类。

2.JoinPoint 连接点，目标类上那些有可能被增强的方法。（一般指所有方法）

3.PointCut 切入点，目标类被增强的方法，被增强的连接点。

4.Advice 增强/通知，增强的那些内容

5.weaving 织入，将通知（增强）应用到连接点上，生成代理的过程。

6.proxy 代理

7.aspect 切面，切入点 和 通知 结合

```

## aop 联盟规范

### 通知类型

```
1） aop联盟 定义 aop通知类型，spring 对 aop联盟规范支持。

2） aop联盟定义5种通知

    前置通知 org.springframework.aop.MethodBeforeAdvice
        在目标方法执行前实施增强

    后置通知 org.springframework.aop.AfterReturningAdvice
        在目标方法执行后实施增强

    环绕通知 org.aopalliance.intercept.MethodInterceptor 【注意：这个接口是aop联盟的】
        在目标方法执行前后实施增强

    异常抛出通知 org.springframework.aop.ThrowsAdvice
        在方法抛出异常后实施增强

    引介通知 org.springframework.aop.IntroductionInterceptor对一个类进行增强（添加方法或属性等）
        在目标类中添加一些新的方法和属性
            try{
               前置
               环绕
               后置
            } catch{
               异常抛出
            }

3） 如果使用aop联盟规范进行aop开发，所有的通知必须实现接口。（底层为了确定通知方法名称）

```

### Joinpoint 连接点

```
1） JoinPoint连接点的信息
2） 接口：org.aspectj.lang.JoinPoint
    • 获得目标对象(被代理对象)：getTarget()
    • 获得被增强方法签名：getSignature()
    • 获得被增强方法名称：getSignature().getName()
    * 获取被增强方法对象本身: getSignature().getMethod()
    • 获得实际参数：getArgs()
    • 获得当前指定方法的类型：getKind()
        method-execution 方法执行、
        constructor-execution构造方法执行
        field-get get方法
    * proceed() 执行原始方法
```



## 不和 spring 集成(单独使用动态代理)

### 通过 jdk 动态生成代理

spring 对于实现了接口的类, 会使用这种方式生成代理

```java
/**
步骤：
1. 接口 + 实现类
2. 编写切面类 MyAspect ，并提供通知
3. 编写工厂，生成代理
4. 测试

*/
// 接口：
public interface UserService {
    public void addUser();
    public void updateUser();

}
// 实现类：
public class UserServiceImpl implements UserService {

    @Override
    public void addUser() {
        System.out.println("a_jdk addUser()");
    }

    @Override
    public void updateUser() {
        System.out.println("a_jdk updateUser()");
    }

}
// 切面类：
/**
 * 切面类，存放所有的通知（增强）
 */
public class MyAspect {
    public void myBefore(){
        System.out.println(" a_proxy.a_jdk  前");
    }
    public void myAfter(){
        System.out.println(" a_proxy.a_jdk  后");
    }

}
// 工厂类：
public class MyFactory {
    /**
     *  JDK 动态代理  Object obj = Proxy.newProxyInstance(....)；
     *  1.参数1：ClassLoader loader ,确定类加载器。程序运行时动态创建类，需要类加载加载到内存。类加载器作用：class文件 --> Class对象
     *      * 一般情况使用都是当前类的类加载器
     *      * 类加载器获得方式：MyFactory.class.getClassLoader();
     *  2.参数2：Class[] interfaces  代理需要实现的接口们（可能有多个）
     *      * 方式1：userService.getClass().getInterfaces()【此方式只能在代理对象和接口是父子关系时使用】
     *      * 方式2：new Class[]{UserService.class}【当被代理对象和其实现接口之间是隔代关系时（即祖孙关系）必须使用方式2获取接口们】
     *  3.参数3：InvocationHandler h 请求处理类，代理类方法执行时，需要请求处理类来处理。
     *      * 一般采用匿名内部类：new InvocationHandler(){}
     *      * 实现方法 invoke ，代理类每一个方法执行一次，将调用一次invoke
     *          参数1.1：Object proxy ,代理成功对象（即 proxyService，不是“代理之前对象”），一般没用。
     *          参数2.2：Method method ，当前执行的方法
     *              * 当前调用方法名：method.getName();
     *              * 执行目标类方法：Object obj = method.invoke(代理之前对象 , args)
     *          参数3.3：Object[] args
     *              * 当前方法实际参数
     */
    public static UserService createService(){
        //1 创建目标
        final UserService userService = new UserServiceImpl();
        //2 切面类（通知）
        final MyAspect myAspect = new MyAspect();
        //3 编写代理，将通知 应用 到目标类方法上
        UserService proxyService = (UserService)Proxy.newProxyInstance(
                MyFactory.class.getClassLoader(),
                userService.getClass().getInterfaces(),
                new InvocationHandler(){
                    @Override
                    public Object invoke(Object proxy, Method method,
                            Object[] args) throws Throwable {
                        //前通知
                        myAspect.myBefore();
                        //执行目标类的方法, 注意用的不是 proxy对象而是原始对象
                        Object obj = method.invoke(userService, args);
                        //后通知
                        myAspect.myAfter();
                        return obj;
                    }
                });
        return proxyService;
    }

}

```

### 通过 cglib 动态生成代理

spring 对于没有实现接口的类, 会使用这种方式生成代理

-   cglib：字节码增强框架（spring 字节码增强框架，默认使用 cglib），运行时采用动态生成子类方式，子类就是代理类

-   spring 将 asm 和 cglib 内置到 spring-core...jar【spring 中使用时导入这个 jar 包】

```java
// UserService：
public class UserService {
    public void addUser() {
        System.out.println("b_cglib addUser()");
    }

    public void updateUser() {
        System.out.println("b_cglib updateUser()");
    }
}
// 切面类：
/**
 * 切面类，存放所有的通知（增强）
 */
public class MyAspect {
    public void myBefore(){
        System.out.println(" b_cglib  前");
    }
    public void myAfter(){
        System.out.println(" b_cglib  后");
    }
}
// 工厂类：
public class MyFactory {

    public static UserService createService() {
        //1 目标类  类 = new 类
        final UserService userService = new UserService();
        //2 切面类
        final MyAspect myAspect = new MyAspect();
        //3 代理类 --cglib (代理类【子】 和 目标类【父】 父子关系)
        // 3.1 创建核心类（Enhancer 增强）
        Enhancer enhancer = new Enhancer();
        // 3.2 确定即目标类
        enhancer.setSuperclass(userService.getClass());// 因为代理类就是target class 的 sub class
        // 3.3 设置回调，类似JDK代理中InvocationHandler
        // ** CallBack 接口处理所有内容，这里使用它的子接口
        // ** 子接口 MethodInterceptor 用于处理方法
        enhancer.setCallback(new MethodInterceptor() {
            /**
             * intercept() 等效 jdk invoke()方法
             *  * 参数1、参数2、参数3：与invoke一样的
             *  * 参数4：methodProxy---表示属于代理对象的方法
             */
            @Override
            public Object intercept(Object proxy, Method method, Object[] args,
                    MethodProxy methodProxy) throws Throwable {
                //前
                myAspect.myBefore();
                //执行目标类的当前方法
                Object obj = method.invoke(userService, args);
                // ******** 通过代理类执行“代理类的父类”【即目标类】的当前方法，和上句等效
                // methodProxy.invokeSuper(proxy, args);
                //后
                myAspect.myAfter();
                return obj;
            }
        });
        // 3.4 创建代理
        UserService proxyService = (UserService)enhancer.create();
        return proxyService;
    }

}
// 测试：
public class TestCglib {
    @Test
    public void demo01(){
        UserService userService = new UserService();
        userService.addUser();
        userService.updateUser();
    }
    @Test
    public void demo02(){
        UserService userService = MyFactory.createService();
        userService.addUser();
        userService.updateUser();
    }
}

```

## 通过 ProxyFactoryBean 生成代理(半自动)

添加 aop 开发包: http://mvnrepository.com/artifact/org.aopalliance/com.springsource.org.aopalliance/1.0.0, 还有实现类 spring-aop

```java
// 接口：
public interface UserService {
    public void addUser();
    public void updateUser();

}
// 实现类：
public class UserServiceImpl implements UserService {

    @Override
    public void addUser() {
        System.out.println("b_factory_bean addUser()");
    }

    @Override
    public void updateUser() {
        System.out.println("b_factory_bean updateUser()");
    }

}
// 切面类：
/**
 * 切面类，存放所有的通知，使用spring 联盟提供规范，必须实现相应接口，这里实现的是环绕通知的接口
 * * 环绕通知必须手动执行目标方法
 */
public class MyAspect implements MethodInterceptor {

    @Override
    public Object invoke(MethodInvocation mi) throws Throwable {
        System.out.println("前");
        //必须手动的执行目标方法
        Object obj = mi.proceed();
        System.out.println("后");
        return obj;
    }
}
// 测试：
public class TestFactoryBean {
    @Test
    public void demo01(){
        String xmlPath = "config/beans.xml";
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
        //从spring获得普通bean数据
        UserService userService = (UserService) applicationContext.getBean("userServiceId");
        userService.addUser();
        userService.updateUser();
    }
    @Test
    public void demo02(){
        String xmlPath = "config/beans.xml";
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
        //从spring 手动 获得代理bean，称为半自动就是因为这里，一旦删去代理对象的相关配置就报错
        UserService userService = (UserService) applicationContext.getBean("proxyServiceId");
        userService.addUser();
        userService.updateUser();
    }

}

```

```xml
<!-- 配置： -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 1 目标类 -->
    <bean id="userServiceId" class="com.xiaoyu.spring.learn.UserServiceImpl"></bean>
    <!-- 2 切面类 -->
    <bean id="myAspectId" class="com.xiaoyu.spring.learn.MyAspect"></bean>
    <!-- 3 生成代理对象：通知（增强） 和目标类 结合
    * ProxyFactoryBean（代理工厂bean） 用于生成一个专门生产代理bean的工厂bean。
        target 确定目标类
        interceptorNames 确定所有切面类，类型String[],需要切面类的bean id值，这里只有一个值，可直接用<property>的value属性赋值
        proxyInterfaces 确定代理类需要实现接口
        optimize 强制是否使用cglib【配置为true则配置的接口属性（proxyInterfaces）可以省略】
    -->
    <bean id="proxyServiceId" class="org.springframework.aop.framework.ProxyFactoryBean">
        <property name="target" ref="userServiceId"></property>
        <property name="interceptorNames" value="myAspectId"></property>
        <property name="proxyInterfaces" value="com.xiaoyu.spring.learn.UserService"></property>
        <property name="optimize" value="true"></property>
    </bean>
</beans>

```

## 全自动实现

-   配置文件中加入约束(使用 aop 编程，必须使用 aop schema（引用 aop 命名空间），在 index-config.html 中找)

-   在半自动的 pom 基础上添加: http://www.mvnrepository.com/artifact/org.aspectj/com.springsource.org.aspectj.weaver

```java
// 接口：
public interface UserService {
    public void addUser();
    public void updateUser();
}
// 实现类：
public class UserServiceImpl implements UserService {

    @Override
    public void addUser() {
        System.out.println("c_aop addUser()");
    }

    @Override
    public void updateUser() {
        System.out.println("c_aop updateUser()");
    }

}
// 切面类：
/**
 * 切面类，存放所有的通知，使用spring 联盟提供规范，必须实现接口MethodInterceptor
 * * 环绕通知必须手动执行目标方法
 */
public class MyAspect implements MethodInterceptor {

    @Override
    public Object invoke(MethodInvocation mi) throws Throwable {
        System.out.println("c_aop 前");
        //必须手动的执行目标方法
        Object obj = mi.proceed();
        System.out.println("c_aop 后");
        return obj;
    }
}
// 测试：
public class TestAOP {
    @Test
    public void demo01(){
        String xmlPath = "config/beans.xml";
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
        //经过配置，按照以前的普通方式获得bean数据，就是代理bean对象
        UserService userService = (UserService) applicationContext.getBean("userServiceId");
        userService.addUser();
        userService.updateUser();
    }
}

```

```xml
<!-- 配置： -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/aop
                           http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 1 目标类 -->
    <bean id="userServiceId" class="com.xiaoyu.spring.learn.UserServiceImpl"></bean>
    <!-- 2 切面类（通知） -->
    <bean id="myAspectId" class="com.xiaoyu.spring.learn.MyAspect"></bean>
    <!-- 3 spring aop 编程 配置
    * aop编程 <aop:config>
            proxy-target-class="true" 表示强制使用cglib
    * <aop:pointcut> ：配置切入点,确定目标类上那些方法需要增强
        expression ：切入点表达式，采用aspectJ（一种AOP规范）的切入点表达式
            execution(* com.xiaoyu.spring.learn.UserServiceImpl.addUser())  指定一种方法增强
            execution(* com.xiaoyu.spring.learn.UserServiceImpl.*())      所有代理对象方法增强
        id : 切入点名称，提供给别人使用。
    * <aop:advisor>: 这是一个特殊切面，【一个通知 和 一个切入点 结合】
            advice-ref 通知的引用
            pointcut-ref 切入点的引用
    -->
    <aop:config proxy-target-class="true">
        <aop:pointcut id="myPointCut" expression="execution(* com.xiaoyu.spring.learn.UserServiceImpl.*())" />
        <aop:advisor advice-ref="myAspectId" pointcut-ref="myPointCut"/>
    </aop:config>
</beans>

```

## 使用 AspectJ 框架生成代理(和 spring 集成)

### 准备工作

引入 jar 包

-   http://www.mvnrepository.com/artifact/org.aspectj/aspectjrt
-   http://www.mvnrepository.com/artifact/org.aspectj/aspectjweaver

导入的 jar 包还有另一种说法, 对错存疑:
[aop 联盟规范](http://mvnrepository.com/artifact/org.aopalliance/com.springsource.org.aopalliance/1.0.0, 还有实现类 spring-aop), 对应的 spring-aop 实现

<img src="p5.png" width="50%">

### AspectJ 通知类型

```
定义6中通知类型，。
    • before:前置通知(应用：各种校验)
            在方法执行前执行，如果通知抛出异常，阻止方法运行
    • afterReturning:后置通知(应用：常规数据处理)
            方法正常返回后执行，如果方法中抛出异常，通知无法执行
            必须在方法执行后才执行，所以可以获得方法的返回值。
    • around:环绕通知(应用：十分强大，可以做任何事情)
            方法执行前后分别执行，可以阻止方法的执行
    • afterThrowing:抛出异常通知(应用：包装异常信息)
            方法抛出异常后执行，如果方法没有抛出异常，无法执行
    • after:最终通知(应用：清理现场)
            方法执行完毕后执行，无论方法中是否出现异常
```

<img src="p6.png" width="50%">

```java
try{
   //前置
   Object = //环绕--手动执行目标方法
   //后置(可以获得返回值)
} catch(){
   //抛出异常
} finally{
   //最终
}

```

### AspectJ 编程,基于 xml

```java
/**
1.目标类
2.切面类（不需要实现接口）
3.spring配置
4.测试
*/
// 接口：
public interface UserService {
    public void addUser();
    public String updateUser();
}
// 实现类：
public class UserServiceImpl implements UserService {
    @Override
    public void addUser() {
        System.out.println("d_aspectj.a_xml addUser()");
    }
    @Override
    public String updateUser() {
        System.out.println("d_aspectj.a_xml updateUser()");
        //int i = 1/ 0;
        return "xxxxx";
    }
}
// 切面类：
/**
 * 切面类，定义需要通知，不用实现任何接口，但之后需要通过配置文件确定具体通知类型
 */
public class MyAspect{
    //前置通知方法参数类型：org.aspectj.lang.JoinPoint，可以获得方法名等内容
    public void myBefore(JoinPoint joinPoint){
        System.out.println("前置通知 :" + joinPoint.getSignature().getName());
    }
//  参数1，类型 JoinPoint 获得当前操作连接点详细内容
//  参数2：类型Object，目标对象被增强方法返回值；名称通过 returning属性 确定【需要在配置文件中配置】
    public void myAfterReturning(JoinPoint joinPoint,Object ret){
        System.out.println("后置通知：" + joinPoint.getSignature().getName() + " , " + ret);
    }

//环绕通知的增强内容方法签名有规定：
// 1.修饰符：public； 2.返回值：Object； 3.方法参数：ProceedingJoinPoint joinPoint； 4.抛出异常：throws Throwable
// 5.手动执行目标对象的被增强方法
    public Object myAround(ProceedingJoinPoint joinPoint) throws Throwable{
        System.out.println("环绕前");
        //执行目标方法
        Object obj = joinPoint.proceed();
        System.out.println("环绕后");
        return obj;
    }

//   参数1：类型 JoinPoint
//   参数2：类型 Throwable ，名称通过 throwing属性确定名称
    public void myAfterThrowing(JoinPoint joinPoint, Throwable e){
        System.out.println("抛出异常通知：" + joinPoint.getSignature().getName() + " , " + e.getMessage());
    }
    public void myAfter(JoinPoint joinPoint){
        System.out.println("最终"+ joinPoint.getSignature().getName() );
    }

}
// 测试：
public class TestXml {
    @Test
    public void demo01(){
        String xmlPath = "config/beans.xml";
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(xmlPath);
        //正常从spring获得bean数据即可
        UserService userService = (UserService) applicationContext.getBean("userServiceId");
        userService.addUser();
        userService.updateUser();
    }
}

```

```xml
<!-- 配置： -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/aop
                           http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 1 目标类 -->
    <bean id="userServiceId" class="com.xiaoyu.spring.learn.UserServiceImpl"></bean>
    <!-- 2 切面类（通知） -->
    <bean id="myAspectId" class="com.xiaoyu.spring.learn.MyAspect"></bean>

    <aop:config>
    <!-- 3 <aop:aspect> 进行aspectj编程
            * ref 指定切面类
    -->
        <aop:aspect ref="myAspectId">
            <!--
                1 定义“切入点”，确定目标类上，最后需要增强的方法
                2 aspect切面定义，确定切面类所有的通知
                3 将通知 应用到 切入点上
             -->
            <!-- 切入点 -->
            <aop:pointcut expression="execution(* com.itheima.d_aspectj.a_xml.*.*(..))" id="myPointcut"/>
            <!-- 具体通知
                1） 前置通知
                    格式1：<aop:before method="方法名称" pointcut-ref="切入点引用"/>
                    格式2：<aop:before method="方法名称" pointcut="切入点表达式"/>
                    （前置通知方法参数类型：org.aspectj.lang.JoinPoint，可以获得方法名等内容。）
                    例如：
                        <aop:before method="myBefore" pointcut-ref="myPointcut" />
                2） 后置通知
                    格式：<aop:after-returning method="" pointcut-ref="" returning="参数2名称"/>
                    例如：<aop:after-returning method="myAfterReturning" pointcut-ref="myPointcut" returning="ret"/>
                    通知方法参数：
                        参数1，类型 JoinPoint 获得当前操作连接点详细内容
                        参数2：类型Object，名称通过 returning配置项确定【即需要在配置文件中指定】
                    public void myAfterReturning(JoinPoint joinPoint,Object ret)

                3）环绕通知,通知方法有要求
                    环绕必须手动的执行目标方法
                    格式：<aop:around method="" pointcut-ref=""/>
                    通知方法要求
                        public Object myAround(ProceedingJoinPoint joinPoint) throws Throwable{

                            // something...

                            //执行目标方法
                            Object obj = joinPoint.proceed();

                            // something...

                            return obj;
                        }
                    例如：
                        <aop:around method="myAround" pointcut-ref="myPointcut"/>
                4）抛出异常通知
                    格式：<aop:after-throwing method="" pointcut-ref="" throwing="参数2名称"/>
                    通知方法参数
                        参数1：类型 JoinPoint
                        参数2：类型 Throwable ，名称通过 throwing属性确定名称
                        public void myAfterThrowing(JoinPoint joinPoint, Throwable e){}
                    例如：
                        <aop:after-throwing method="myAfterThrowing" pointcut-ref="myPointcut" throwing="e"/>
                5）最终通知
                    格式：<aop:after method="" pointcut-ref=""/>
                    通知方法参数类型 JoinPoint
            -->
            <aop:after method="myAfter" pointcut-ref="myPointcut"/>
        </aop:aspect>
    </aop:config>
</beans>

```

### AspectJ 编程,基于注解

```java
/**
1. 开启包扫描, 扫描注解类：@Service 、@Component
配置文件要配置：<context:component-scan></context:component-scan>

2. aspectj 注解自动代理：
    @Aspect 、标识切面类
    @PointCut、定义可复用的切入点表达式
    @Before、前置
    @AfterReturning 后置
    @Around 环绕
    @AfterThrowing 抛出异常
    @After 最终
3. 最后还要配置：
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
*/

// 接口：
public interface UserService {
    public void addUser();
    public String updateUser();
}
// 实现类：
@Service
public class UserServiceImpl implements UserService {
    @Override
    public void addUser() {
        System.out.println("d_aspectj.a_xml addUser()");
    }
    @Override
    public String updateUser() {
        System.out.println("d_aspectj.a_xml updateUser()");
        //int i = 1/ 0;
        return "xxxx";
    }
}
// 切面类：
/**
 * 切面类，定义需要通知，之后需要通过配置文件确定具体通知类型
 */
@Component //切面类不属于任何层，所以用通用的注解
@Aspect    //表示此类是切面类
public class MyAspect{

    //定义可复用的切入点表达式使用@Pointcut。其他增强内容方法的注解直接调用方法名myPointCut() eg：@before("myPointCut()")
    @Pointcut("execution(* com.xiaoyu.spring.learn.*.*(..))")
    private void myPointCut(){}

    //  这个注解只有一个属性value="",可以省略属性名value，直接写值
    //@Before("execution(* com.xiaoyu.spring.learn.*.*(..))")
    public void myBefore(JoinPoint joinPoint){
        System.out.println("前置通知 :" + joinPoint.getSignature().getName());
    }

    //这个注解有多个属性，不能省略属性名
    //@AfterReturning(value="execution(* com.xiaoyu.spring.learn.*.*(..))",returning="ret")
    public void myAfterReturning(JoinPoint joinPoint,Object ret){
        System.out.println("后置通知：" + joinPoint.getSignature().getName() + " , " + ret);
    }

    //这里的注解直接调用了可复用的切入点表达式，直接调用对应的方法
    //@Around("myPointCut()")
    public Object myAround(ProceedingJoinPoint joinPoint) throws Throwable{
        System.out.println("环绕前");
        //执行目标方法
        Object obj = joinPoint.proceed();
        System.out.println("环绕后");
        return obj;
    }

    //注解调用的是可复用的切入点表达式
    //@AfterThrowing(value="myPointCut()",throwing="e")
    public void myAfterThrowing(JoinPoint joinPoint, Throwable e){
        System.out.println("抛出异常通知：" + joinPoint.getSignature().getName() + " , " + e.getMessage());
    }

    @After("myPointCut()")
    public void myAfter(JoinPoint joinPoint){
        System.out.println("最终"+ joinPoint.getSignature().getName() );
    }
}
// 测试：
//要引入spring-test的jar包
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations="classpath:config/beans.xml")
public class TestAnno {
    @Autowired
    private UserService userService;
    @Test
    public void demo01(){
        userService.addUser();
        userService.updateUser();
    }
}

```

为了让注解生效, 需要配置注解扫描

```xml
<!-- 配置： -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/aop
                           http://www.springframework.org/schema/aop/spring-aop.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 1.1 用注解标识目标类
            @Service      // service层注解
            public class UserServiceImpl
    -->
    <!-- 1.2 为了让添加注解类生效，必须开启包扫描。需要context命名空间 -->
    <context:component-scan base-package="com.xiaoyu.spring.learn"></context:component-scan>
    <!-- 2 使用注解
        @Component  // 切面类不属于任何层，使用通用注解
        public class MyAspect{
    -->
    <!-- 3
        3.1 <aop:aspect ref="myAspectId">
            @Component      切面类不属于任何层
            @Aspect        标识切面类（表示让切面类成为切面）
            public class MyAspect{
        3.2 确定通知
            前置通知
                @Before("切入点表达式")  等效
                <aop:before method="方法名称" pointcut="切入点表达式"/>
            后置通知
                @AfterReturning(value="表达式",returning="参数2名称")
            环绕
                @Around("myPointCut()")
            抛出异常
                @AfterThrowing(value="myPointCut()",throwing="参数2名称")
            最终
                @After("myPointCut()")
        3.3 声明公共切入点表达式(可以复用)
            @Pointcut("execution(* com.itheima.d_aspectj.b_annotation.*.*(..))")
            private void myPointCut(){}
    -->
    <!-- 标明使用注解开发aspectj 必须进行的配置 -->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>

```

# JDBC

references: [1](https://blog.csdn.net/fysuccess/article/details/66972554)

## 数据库连接池

### C3P0

```xml
<dependency>
  <groupId>c3p0</groupId>
  <artifactId>c3p0</artifactId>
  <version>0.9.1.2</version>
</dependency>
```

```xml
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource" destroy-method="close">
        <property name="jdbcUrl" value="${jdbc.url}" />
        <property name="driverClass" value="${jdbc.driverClassName}" />
        <property name="user" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />
        <!--连接池中保留的最大连接数。Default: 15 -->
        <property name="maxPoolSize" value="100" />
        <!--连接池中保留的最小连接数。-->
        <property name="minPoolSize" value="1" />
        <!--初始化时获取的连接数，取值应在minPoolSize与maxPoolSize之间。Default: 3 -->
        <property name="initialPoolSize" value="10" />
        <!--最大空闲时间,60秒内未使用则连接被丢弃。若为0则永不丢弃。Default: 0 -->
        <property name="maxIdleTime" value="30" />
        <!--当连接池中的连接耗尽的时候c3p0一次同时获取的连接数。Default: 3 -->
        <property name="acquireIncrement" value="5" />
        <!--JDBC的标准参数，用以控制数据源内加载的PreparedStatements数量。Default: 0-->
        <property name="maxStatements" value="0" />
        <!--每60秒检查所有连接池中的空闲连接。Default: 0 -->
        <property name="idleConnectionTestPeriod" value="60" />
        <!--定义在从数据库获取新连接失败后重复尝试的次数。Default: 30 -->
        <property name="acquireRetryAttempts" value="30" />
        <!--获取连接失败将会引起所有等待连接池来获取连接的线程抛出异常。Default: false-->
        <property name="breakAfterAcquireFailure" value="true" />
        <!--因性能消耗大请只在需要的时候使用它。Default: false -->
        <property name="testConnectionOnCheckout"  value="false" />
    </bean>
```

### DBCP

引入 jar

```xml
<dependency>
  <groupId>commons-dbcp</groupId>
  <artifactId>commons-dbcp</artifactId>
  <version>1.4</version>
</dependency>
<dependency>
  <groupId>commons-collections</groupId>
  <artifactId>commons-collections</artifactId>
  <version>3.2.2</version>
</dependency>
<dependency>
  <groupId>commons-pool</groupId>
  <artifactId>commons-pool</artifactId>
  <version>1.6</version>
</dependency>
```

```xml
<bean id="dataSource" destroy-method="close" class="org.apache.commons.dbcp.BasicDataSource">
       <property name="driverClassName" value="${jdbc.driverClassName}"/>
       <property name="url" value="${jdbc.url}"/>
       <property name="username" value="${jdbc.username}"/>
       <property name="password" value="${jdbc.password}"/>
       <!-- 池启动时创建的连接数量 -->
       <property name="initialSize" value="5"/>
       <!-- 同一时间可以从池分配的最多连接数量。设置为0时表示无限制。 -->
       <property name="maxActive" value="50"/>
       <!-- 池里不会被释放的最多空闲连接数量。设置为0时表示无限制。 -->
       <property name="maxIdle" value="10"/>
       <!-- 在不新建连接的条件下，池中保持空闲的最少连接数。 -->
       <property name="minIdle" value="3"/>
       <!-- 设置自动回收超时连接 -->
       <property name="removeAbandoned" value="true" />
       <!-- 自动回收超时时间(以秒数为单位) -->
       <property name="removeAbandonedTimeout" value="200"/>
       <!-- 设置在自动回收超时连接的时候打印连接的超时错误  -->
       <property name="logAbandoned" value="true"/>
       <!-- 等待超时以毫秒为单位，在抛出异常之前，池等待连接被回收的最长时间（当没有可用连接时）。设置为-1表示无限等待。-->
       <property name="maxWait" value="100"/>
     </bean>
```

### Druid

数据源配置

```xml
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <!-- 基本属性 url、user、password -->
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />
        <!-- 配置初始化大小、最小、最大 -->
        <property name="initialSize" value="1" />
        <property name="minIdle" value="1" />
        <property name="maxActive" value="20" />
        <!-- 配置获取连接等待超时的时间 -->
        <property name="maxWait" value="60000" />
        <!-- 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒 -->
        <property name="timeBetweenEvictionRunsMillis" value="60000" />
        <!-- 配置一个连接在池中最小生存的时间，单位是毫秒 -->
        <property name="minEvictableIdleTimeMillis" value="300000" />
        <!-- 打开PSCache，并且指定每个连接上PSCache的大小 -->
        <property name="poolPreparedStatements" value="true" />
        <property name="maxPoolPreparedStatementPerConnectionSize" value="20" />
        <!-- 配置监控统计拦截的filters，去掉后监控界面sql无法统计 -->
        <property name="filters" value="stat" />
    </bean>
```

web.xml 中的配置

```xml
<!--druid WebStatFilter用于采集web-jdbc关联监控的数据-->
      <filter>
        <filter-name>DruidWebStatFilter</filter-name>
        <filter-class>com.alibaba.druid.support.http.WebStatFilter</filter-class>
        <init-param>
          <param-name>exclusions</param-name>
          <param-value>*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*</param-value>
        </init-param>
      </filter>
      <filter-mapping>
        <filter-name>DruidWebStatFilter</filter-name>
        <url-pattern>/*</url-pattern>
      </filter-mapping>
      <!--druid访问监控界面 /druid/index.html-->
      <servlet>
        <servlet-name>DruidStatView</servlet-name>
        <servlet-class>com.alibaba.druid.support.http.StatViewServlet</servlet-class>
      </servlet>
      <servlet-mapping>
        <servlet-name>DruidStatView</servlet-name>
        <url-pattern>/druid/*</url-pattern>
      </servlet-mapping>
```

### BoneCP

引入 jar

```xml
<dependency>
  <groupId>com.jolbox</groupId>
  <artifactId>bonecp-spring</artifactId>
  <version>0.8.0.RELEASE</version>
</dependency>
```

```xml
<!-- BoneCp Datasource -->
<bean id="dataSourceBoneCp" class="com.jolbox.bonecp.BoneCPDataSource" destroy-method="close">
 <property name="driverClass" value="${db.driverClass}" />
 <property name="jdbcUrl" value="${db.url}" />
 <property name="username" value="${db.username}" />
 <property name="password" value="${db.password}" />
 <property name="idleConnectionTestPeriodInMinutes" value="2" />
 <property name="idleMaxAgeInMinutes" value="2" />
 <property name="maxConnectionsPerPartition" value="2" />
 <property name="minConnectionsPerPartition" value="0" />
 <property name="partitionCount" value="2" />
 <property name="acquireIncrement" value="1" />
 <property name="statementsCacheSize" value="100" />
 <property name="lazyInit" value="true"/>
 <property name="maxConnectionAgeInSeconds" value="20"/>
 <property name="defaultReadOnly" value="true"/>
</bean>
```

### HikariCP

```xml
<!-- Hikari Datasource -->
<bean id="dataSourceHikari" class="com.zaxxer.hikari.HikariDataSource"  destroy-method="shutdown">
 <!-- <property name="driverClassName" value="${db.driverClass}" /> --> <!-- 无需指定，除非系统无法自动识别 -->
 <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/test?useUnicode=true&characterEncoding=UTF-8" />
 <property name="username" value="${db.username}" />
 <property name="password" value="${db.password}" />
  <!-- 连接只读数据库时配置为true， 保证安全 -->
 <property name="readOnly" value="false" />
 <!-- 等待连接池分配连接的最大时长（毫秒），超过这个时长还没可用的连接则发生SQLException， 缺省:30秒 -->
 <property name="connectionTimeout" value="30000" />
 <!-- 一个连接idle状态的最大时长（毫秒），超时则被释放（retired），缺省:10分钟 -->
 <property name="idleTimeout" value="600000" />
 <!-- 一个连接的生命时长（毫秒），超时而且没被使用则被释放（retired），缺省:30分钟，建议设置比数据库超时时长少30秒，参考MySQL wait_timeout参数（show variables like '%timeout%';） -->
 <property name="maxLifetime" value="1800000" />
 <!-- 连接池中允许的最大连接数。缺省值：10；推荐的公式：((core_count * 2) + effective_spindle_count) -->
 <property name="maximumPoolSize" value="15" />
</bean>
```

## JdbcTemplate

### quickstart

-   jar 包: spring-jdbc, spring-tx, 数据库驱动, 连接池 jar

```java
public class TestJDBCTemplate {

    @Test
    public void test1() {

        // JDBC模板依赖于连接池来获得数据的连接，所以必须先要构造连接池
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/spring");
        dataSource.setUsername("root");
        dataSource.setPassword("123456");

        // 创建JDBC模板
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        // 这里也可以使用构造方法
        jdbcTemplate.setDataSource(dataSource);

        // sql语句
        String sql = "select count(*)  from user";
        Long num = (long) jdbcTemplate.queryForObject(sql, Long.class);

        System.out.println(num);

    }

}
```

使用 spring 的特性后

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context
    http://www.springframework.org/schema/context/spring-context.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop.xsd
    http://www.springframework.org/schema/tx
    http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!-- IOC和DI的注解扫描 -->
    <context:component-scan base-package="com.spring" ></context:component-scan>

    <!-- 打开AOP的注解 -->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
    <!-- c3p0数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource" >
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/spring_03"></property>
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="user" value="root"></property>
        <property name="password" value="123456"></property>
    </bean>

    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <bean id="userDao" class="com.spring.dao.UserDao">
        <property name="jdbcTemplate" ref="jdbcTemplate"></property>
    </bean>

    <bean id="userService" class="com.spring.service.UserService">
        <property name="userDao" ref="userDao"></property>
    </bean>

</beans>

```

```java
public class UserDao {

    private JdbcTemplate jdbcTemplate;

    public JdbcTemplate getJdbcTemplate() {
        return jdbcTemplate;
    }

    public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public void addUser(User user) {

        String sql = "insert into user (username, password) values (?, ?)";

        jdbcTemplate.update(sql, user.getUsername(), user.getPassword());

    }

}
```

### Dao 实现类继承 JdbcDaoSupport 支持类

```java
// User:
public class User {
    private Integer id;
    private String username;
    private String password;
    getter&setter
}
// UserDao :
import org.springframework.jdbc.core.support.JdbcDaoSupport;
public class UserDao extends JdbcDaoSupport {
    //继承了JdbcDaoSupport工具类，省略如下代码，相应配置要更改
//  private JdbcTemplate jdbcTemplate;
//  public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
//      this.jdbcTemplate = jdbcTemplate;
//  }

    public List<User> findAll(){
        return getJdbcTemplate().query("select * from t_user", ParameterizedBeanPropertyRowMapper.newInstance(User.class));
    }

}
// 测试：
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations="classpath:config/beans.xml")
public class TestSupport {
        @Autowired
        @Qualifier("userDaoId")
        private UserDao userDao;
        @Test
        public void demo01(){
            List<User> findAll = userDao.findAll();
            for (User user : findAll) {
                System.out.println(user);
            }
        }
}

```

```xml
<!-- 配置： -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 配置数据源 c3p0 -->
    <bean id="dataSourceId" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql:///spring_day02_db"></property>
        <property name="user" value="root"></property>
        <property name="password" value="1234"></property>
    </bean>

    <!--这里删除了JdbcTemplate的bean-->

    <!-- dao , 将数据源DataSource 注入给dao，dao没有相关属性，就给它的父类 JdbcDaoSupport，而JdbcDaoSupport底层使用数据源创建了Jdbc模板 -->
    <bean id="userDaoId" class="com.itheima.e_jdbc.d_support.UserDao">
        <property name="dataSource" ref="dataSourceId"></property>
    </bean>
</beans>

```

### 数据库连接配置放入 Properties 文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context
                           http://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 加载properties文件 -->
    <context:property-placeholder location="classpath:config/jdbc.properties"/>

    <!-- 配置数据源 c3p0，使用配置文件中内容   ${key} -->
    <bean id="dataSourceId" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driverClass}"></property>
        <property name="jdbcUrl" value="${jdbc.jdbcUrl}"></property>
        <property name="user" value="${jdbc.user}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>

    <!-- dao , 将数据源DataSource 注入给dao父类 JdbcDaoSupport，底层使用数据源创建模板 -->
    <bean id="userDaoId" class="com.itheima.e_jdbc.e_properties.UserDao">
        <property name="dataSource" ref="dataSourceId"></property>
    </bean>
</beans>

```

properties 文件：

```
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.jdbcUrl=jdbc:mysql:///spring_day02_db
jdbc.user=root
jdbc.password=1234
```

# 事务管理

## 事务实现原理

Java 中我们平时用的最多的就是在 service 层的增删改方法上添加@Transactional 注解，让 spring 去帮我们管理事务。

底层会给我们的 service 组件生成一个对应的 proxy 动态代理，这样所有对 service 组件的方法都由它对应的 proxy 来接管，当 proxy 在调用对应业务方法比如 add()时，proxy 就会基于 AOP 的思想在调用真正的业务方法前执行 setAutoCommit（false）打开事务。

然后在业务方法执行完后执行 commit 提交事务，当在执行业务方法的过程中发生异常时就会执行 rollback 来回滚事务。




## TransactionDefinition 事务传播

@transaction(propagation=...)

Spring 在 TransactionDefinition 接口中规定了 7 种类型的事务传播行为, <font color="red">它们规定了事务方法和事务方法发生嵌套调用时是否加入调用方的事务</font>

传播行为：一个业务 A (事务方法A)，调用另一个业务 B (带有事务)，B是否加入 A 的事务

-   `PROPAGATION_REQUIRED` required 必须 （默认值）

    如果 A 使用事务，那么 B 使用同一个事务 (此时 B 的事务配置都无效, 被 A 的事务覆盖了)。如果 A 没有事务，B 创建一个新的事务。

-   `PROPAGATION_REQUIRES_NEW` requires new 必须新的

    无论 a 是否存在事务, b 都将创建新的事务

-   `PROPAGATION_NESTED` nested 嵌套

    AB 使用嵌套事务，底层使用 Savepoint

-   PROPAGATION_SUPPORTS supports 支持

    A 使用事务，B 使用同一个事务。A 没有事务，B 将以非事务执行

-   PROPAGATION_NOT_SUPPORTED not supported 不支持

    无论 a 是否存在事务, b 都以非事务执行

-   PROPAGATION_NEVER never 从不

    A 使用事务，B 将抛异常。A 没有事务，B 将以非事务执行

-   PROPAGATION_MANDATORY mandatory 强制

    A 使用事务，B 使用同一个事务。A 没有事务，B 抛出异常


## 事务相关 api

三个顶级接口(规范)

涉及到的 jar: spring-tx, spring-jdbc, orm 的 jar

### PlatformTransactionManager

spring dao 使用不同技术，需要的事务管理器不同。JDBC 事务管理器：DataSourceTransactionManager, Hibernate 事务管理器：HibernateTransactionManager

```java
// 事务状态 需要通过 “事务详情”的配置，从而获得事务状态，通过状态维护事务。
TransactionStatus getTransaction(TransactionDefinition definition);
conn.setAutocommit(false) // jdbc开启事务
commit(TransactionStatus) //提交事务
session.beginTransaction() //hibernate开启事务
rollback(TransactionStatus) //回滚事务

```


### TransactionStatus

事务状态，一般是 spring 底层自动使用

### 使用 jdbc 操作事务

<img src="p7.png" width="50%">

```java
// 某个事务, 包括ABCD 四个步骤(一个整体事务)
Connection conn = null;
try{
    // 获得连接
    conn = 连接池
    //#1 开启事务, 手动提交
    conn.setAutocommit(false);

    A
    B
    C
    D

} catch(){
    //#2 回滚事务
    conn.rollback();
} finally{
    //#3 提交事务
    conn.commit();

    //释放资源
}

//--Savepoint （保存点/回滚点）
// 在一个事务中, AB（必选），CD（可选）
Connection conn = null;
Savepoint savepoint = null;
try{
    // 获得连接
    conn = 连接池
    //#1 开启事务, 设置手动提交
    conn.setAutocommit(false);

    A
    B

    //#2 设置回滚点
    savepoint = conn.setSavepoint();

    C
    D

} catch(){
    if(savepoint != null){
        // savepoint 不为空, 证明 AB成功，CD异常
        // 回滚到CD之前
        conn.rollback(savepoint);
    } else {
        // AB异常，回滚到开始
        conn.rollback();
    }
} finally{
    //#4 提交事务
    conn.commit();

    //释放资源
}


```

## 事务相关案例-转账

### 准备

<img src="p8.png" width="50%">

依赖:

-   spring-core, spring-beans, spring-context, spring-context-support, (spring-web)
-   spring-test
-   spring-jdbc, spring-tx
-   aop 联盟规范接口, spring-aop(spring 实现); aspectj 规范接口, spring-aspect(spring 实现);
-   数据库驱动, 连接池

没有事务的代码

```java
AccountDao接口：
public interface AccountDao {
    /**
     * 汇款
     * @param outer 汇款人
     * @param money
     */
    public void out(String outer , int money);
    /**
     * 收款
     * @param inner 收款收款人
     * @param money
     */
    public void in(String inner , int money);

}
AccountDao实现：
public class AccountDaoImpl extends JdbcDaoSupport implements AccountDao {
    @Override
    public void out(String outer, int money) {
        this.getJdbcTemplate().update("update account set money = money - ? where username = ?", money,outer);
    }
    @Override
    public void in(String inner, int money) {
        this.getJdbcTemplate().update("update account set money = money + ? where username = ?", money,inner);
    }
}
AccountService接口：
public interface AccountService {
    /**
     * 转账
     * @param outer 汇款人
     * @param inner 收款人
     * @param mo
     */
    public void transfer(String outer,String inner,int money);

}
AccountService实现：
public class AccountServiceImpl implements AccountService {
    private AccountDao accountDao;
    public void setAccountDao(AccountDao accountDao) {
        this.accountDao = accountDao;
    }
    @Override
    public void transfer(String outer, String inner, int money) {
        //两个操作
        accountDao.out(outer, money);
        // 模拟断电
        //int i = 1/0;
        accountDao.in(inner, money);
    }
}

测试：
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations="classpath:applicationContext.xml")
public class TestApp {
    @Autowired
    private AccountService accountService;
    @Test
    public void demo01(){
        accountService.transfer("jack", "rose", 100);
    }

}

```

```xml
<!-- 配置： -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 1.数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql:///spring_day03_db"></property>
        <property name="user" value="root"></property>
        <property name="password" value="1234"></property>
    </bean>
    <!-- 2.dao，数据源配置给dao-->
    <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!-- 3.service-->
    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"></property>
    </bean>
</beans>

```

### 手动开发事务

```java
Dao接口：
public interface AccountDao {
    /**
     * 汇款
     * @param outer 汇款人
     * @param money
     */
    public void out(String outer , int money);
    /**
     * 收款
     * @param inner 收款收款人
     * @param money
     */
    public void in(String inner , int money);
}
Dao实现类：
import org.springframework.jdbc.core.support.JdbcDaoSupport;
import com.itheima.dao.AccountDao;
public class AccountDaoImpl extends JdbcDaoSupport implements AccountDao {
    @Override
    public void out(String outer, int money) {
        this.getJdbcTemplate().update("update account set money = money - ? where username = ?", money,outer);
    }
    @Override
    public void in(String inner, int money) {
        this.getJdbcTemplate().update("update account set money = money + ? where username = ?", money,inner);
    }
}
Service接口：
public interface AccountService {
    /**
     * 转账
     * @param outer 汇款人
     * @param inner 收款人
     * @param mo
     */
    public void transfer(String outer,String inner,int money);
}
service实现类：
public class AccountServiceImpl implements AccountService {
    private AccountDao accountDao;
    public void setAccountDao(AccountDao accountDao) {
        this.accountDao = accountDao;
    }
    private TransactionTemplate transactionTemplate;
    public void setTransactionTemplate(TransactionTemplate transactionTemplate) {
        this.transactionTemplate = transactionTemplate;
    }
    @Override   //手动将参数改成final的；
    public void transfer(final String outer, final String inner, final int money) {
        transactionTemplate.execute(new TransactionCallbackWithoutResult() {//没有返回结果的事务回调对象，匿名内部类
            @Override
            public void doInTransactionWithoutResult(TransactionStatus ts) {//业务代码
                //两个操作
                accountDao.out(outer, money);
                // 模拟断电
                //int i = 1/0;
                accountDao.in(inner, money);
            }
        });
    }
}
测试：
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations="classpath:applicationContext.xml")
public class TestApp {
    @Autowired
    private AccountService accountService;
    @Test
    public void demo01(){
        accountService.transfer("jack", "rose", 100);
    }
}


```

```xml
<!-- 配置： -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 1.数据源，连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql:///spring"></property>
        <property name="user" value="root"></property>
        <property name="password" value="1234"></property>
    </bean>
    <!-- 2.dao-->
    <bean id="accountDao" class="com.xiaoyu.spring.learn.AccountDaoImpl">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!-- 3.service-->
    <bean id="accountService" class="com.xiaoyu.spring.learn.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"></property>
        <property name="transactionTemplate" ref="transactionTemplate"></property>
    </bean>
    <!-- 4 创建模板，需要注入txManager -->
    <bean id="transactionTemplate" class="org.springframework.transaction.support.TransactionTemplate">
        <property name="transactionManager" ref="txManager"></property>
    </bean>
    <!-- 5 配置事务管理器,需要Connection连接，连接从连接池 -->
    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
</beans>

```

### 半自动事务(生成代理)

代码无需改动, 就是没有事务的代码

主要修改配置文件

```xml
配置：
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 1.数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql:///spring"></property>
        <property name="user" value="root"></property>
        <property name="password" value="1234"></property>
    </bean>
    <!-- 2.dao-->
    <bean id="accountDao" class="com.xiaoyu.spring.learn.AccountDaoImpl">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!-- 3.service-->
    <bean id="accountService" class="com.xiaoyu.spring.learn.service.impl.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"></property>
    </bean>
    <!-- 4 手动代理
    * TransactionProxyFactoryBean 生产一个具有事务管理能力的代理类。
    ** 1) 提供事务管理，管理事务 ：transactionManager
    ** 2) 接口：proxyInterfaces
    ** 3) 目标类：target
    ** 4) 事务详情（事务属性）：transactionAttributes ,需要类型Properties
        prop.key : 给指定方法配置详情，直接编写方法名称【不带（）】。
            例如：transfer 指定方法名
        prop.text: 详情（详情需要格式）
            格式：PROPAGATION,ISOLATION,readOnly,-Exception,+Exception
                传播行为        隔离级别        是否只读    -有异常就回滚     +有异常仍然提交
        例如：
            <prop key="transfer">PROPAGATION_REQUIRED,ISOLATION_REPEATABLE_READ</prop>
                传播行为和隔离级别 都是默认
    -->
    <bean id="proxyService" class="org.springframework.transaction.interceptor.TransactionProxyFactoryBean">
        <property name="transactionManager" ref="txManager"></property>
        <property name="proxyInterfaces" value="com.itheima.service.AccountService"></property>
        <property name="target" ref="accountService"></property>
        <property name="transactionAttributes">
            <props>
                <prop key="transfer">PROPAGATION_REQUIRED,ISOLATION_REPEATABLE_READ</prop>
            </props>
        </property>
    </bean>
    <!-- 5 事务管理器 -->
    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

</beans>

```

### 全自动事务基于 xml

同样代码无需改动, 只需设置好配置文件

配置文件需要添加约束
xmlns:aop="http://www.springframework.org/schema/aop"
xmlns:tx="http://www.springframework.org/schema/tx"

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/tx
                           http://www.springframework.org/schema/tx/spring-tx.xsd
                           http://www.springframework.org/schema/aop
                           http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 1.数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql:///spring_day03_db"></property>
        <property name="user" value="root"></property>
        <property name="password" value="1234"></property>
    </bean>
    <!-- 2.dao-->
    <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!-- 3.service-->
    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"></property>
    </bean>
    <!-- 4 使用aop进行事务管理 -->
    <!-- 4.1 事务管理器，需要注入数据源  -->
    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!-- 4.2 配置事务详情（事务属性）
    * <tx:advice id="txAdvice" transaction-manager="txManagerd">
        id 通知名称，用于aop引用
        transaction-manager 确定管理器
    * <tx:attributes> 配置事务详情
        * <tx:method name="transfer" ...>
            * name ： 目标对象被增强方法，不带括号【对选中的切入点方法们，每个单独配置，例如具体的读写权限】
        例如：
        1. <tx:method name="transfer" propagation="REQUIRED" isolation="DEFAULT"/>
            传播行为，隔离级别 默认
        2. <tx:method name="transfer" propagation="REQUIRED" isolation="DEFAULT" read-only="true"/>
            只读
        3。 <tx:method name="transfer" propagation="REQUIRED" isolation="DEFAULT" no-rollback-for="java.lang.ArithmeticException" />
            如果有 ArithmeticException异常 仍提交
                rollback-for=""  相当于  -Exception 回滚
                no-rollback-for="" 相当于 +Exception 提交
        * 方法名称支持通配符*
            <tx:method name="add*"/>
            <tx:method name="update*"/>
            <tx:method name="delete*"/>
            <tx:method name="find*" read-only="true"/>
    -->
    <tx:advice id="txAdvice" transaction-manager="txManager">
        <tx:attributes>
            <tx:method name="transfer" propagation="REQUIRED" isolation="DEFAULT" />
        </tx:attributes>
    </tx:advice>
    <!-- 4.3 AOP编程，确定切入点，并将通知应用到切入点 ## 切面
        * 方便理解：
            目标类：ABCD (连接点)
            使用AOP：ABC(切入点)  ## AOP在目标的方法中筛选。
     -->
    <aop:config>
        <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.itheima.service..*.*(..))"/>
    </aop:config>
</beans>

```

### 全自动事务基于注解

```java
Dao接口：
public interface AccountDao {
    /**
     * 汇款
     * @param outer 汇款人
     * @param money
     */
    public void out(String outer , int money);
    /**
     * 收款
     * @param inner 收款收款人
     * @param money
     */
    public void in(String inner , int money);
}
Dao实现：
import org.springframework.jdbc.core.support.JdbcDaoSupport;
import com.itheima.dao.AccountDao;
public class AccountDaoImpl extends JdbcDaoSupport implements AccountDao {
    @Override
    public void out(String outer, int money) {
        this.getJdbcTemplate().update("update account set money = money - ? where username = ?", money,outer);
    }
    @Override
    public void in(String inner, int money) {
      this.getJdbcTemplate().update("update account set money=money-? where username=?",money,inner);
    }
}
Service接口：
public interface AccountService {
    /**
     * 转账
     * @param outer 汇款人
     * @param inner 收款人
     * @param mo
     */
    public void transfer(String outer,String inner,int money);
}
Service实现：
//注解可加在类上，可加在某一个方法上
@Transactional(propagation=Propagation.REQUIRED , isolation=Isolation.DEFAULT)
public class AccountServiceImpl implements AccountService {
    private AccountDao accountDao;
    public void setAccountDao(AccountDao accountDao) {
        this.accountDao = accountDao;
    }
    @Override
    public void transfer(String outer, String inner, int money) {
        //两个操作
        accountDao.out(outer, money);
        // 模拟断电
        //int i = 1/0;
        accountDao.in(inner, money);
    }
}
测试：
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations="classpath:applicationContext.xml")
public class TestApp {
    @Autowired
    private AccountService accountService;
    @Test
    public void demo01(){
        accountService.transfer("jack", "rose", 100);
    }
}

```

```xml
配置：
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/tx
                           http://www.springframework.org/schema/tx/spring-tx.xsd
                           http://www.springframework.org/schema/aop
                           http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 1.数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql:///spring_day03_db"></property>
        <property name="user" value="root"></property>
        <property name="password" value="1234"></property>
    </bean>
    <!-- 2.dao -->
    <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!-- 3.service -->
    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
        <property name="accountDao" ref="accountDao"></property>
    </bean>
    <!-- 4.1 事务管理器 -->
    <bean id="txManager"
        class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    <!-- 4.2 将事务管理器，交予spring，方便进行注解的事务管理 -->
    <tx:annotation-driven transaction-manager="txManager" />
</beans>

```

# spring web 开发

普通的 spring 程序通过 main 函数加载 xml 即可完成启动

web app 不同:

-   需要依赖 spring-web

-   spring 容器存在于 application 作用域(ServletContext), 所有 servlet 共享

-   在 tomcat 启动时加载 xml 配置, 加载方案有三种

    -   Filter --> init()
    -   Listener --> ServletContextListener[spring 默认使用监听器初始化, spring 通过监听器加载 xml 中的 ContextLoaderListener]
    -   Servlet --> init()

-   通过 web 项目的初始化参数 配置 xml 文件位置 `contextConfigLocation`, 如果不配置, 默认加载位置为`/WEB-INF/applicationContext.xml`

```java
// UserService类：
public class UserService {
    public void addUser(){
        System.out.println("addUser");
    }
}
// HelloServlet：没有集成springmvc
public class HelloServlet extends HttpServlet {
    public void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //获得spring容器，
        // 1. ServletContext作用域获得内容
        // ** 方式1
        ApplicationContext applicationContext = (ApplicationContext)
                this.getServletContext().getAttribute(WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE);
        // **  方式2 通过spring提供工具类【推荐】
        ApplicationContext applicationContext =
                WebApplicationContextUtils.getWebApplicationContext(this.getServletContext());
        //2.再从容器获得service
        UserService userService = (UserService) applicationContext.getBean("userService");
        userService.addUser();
    }
    public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        this.doGet(request, response);
    }
}
```

```xml
<!-- web.xml配置： -->
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5"
    xmlns="http://java.sun.com/xml/ns/javaee"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee
    http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
  <!-- 配置xml位置 -->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>
  <!-- 配置spring监听器，加载xml配置文件 -->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  <servlet>
    <servlet-name>HelloServlet</servlet-name>
    <servlet-class>com.xiaoyu.spring.learn.web.servlet.HelloServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>HelloServlet</servlet-name>
    <url-pattern>/HelloServlet</url-pattern>
  </servlet-mapping>
</web-app>
```

```xml
<!-- applicationContext.xml配置： -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userService" class="com.xiaoyu.spring.learn.UserService"></bean>
</beans>
```

# 几种加载 xml 方式

```java
public class Client {
    @Test
    public void test1() {
        //引用资源
        Resource resource = new ClassPathResource("com/xy/xmltest/MyXml.xml");
        // 这个方法过时了, 不推荐
        XmlBeanFactory factory = new XmlBeanFactory(resource);
        XmlBean bean = factory.createBean(XmlBean.class);
        System.out.println(bean);
    }

    @Test
    public void test2() {
        //编译路径
        ApplicationContext factory=new ClassPathXmlApplicationContext("com/xy/xmltest/MyXml.xml");
        // or
        ApplicationContext factory=new ClassPathXmlApplicationContext(new String[] {"bean1.xml","bean2.xml"});
        XmlBean bean = factory.getBean(XmlBean.class);
        System.out.println(bean);
    }

    @Test
    public void test3() {
        //文件系统的路径
        ApplicationContext factory=new FileSystemXmlApplicationContext("src/com/xy/xmltest/MyXml.xml");
        // or
        //使用了  classpath:  前缀,作为标志,  这样,FileSystemXmlApplicationContext 也能够读入classpath下的相对路径
        ApplicationContext factory=new FileSystemXmlApplicationContext("classpath:appContext.xml");
        XmlBean bean = factory.getBean(XmlBean.class);
        System.out.println(bean);
    }

    @Test
    public void test4() {
        // 适用于Web工程。
        ServletContext servletContext = request.getSession().getServletContext();
        ApplicationContext ctx = WebApplicationContextUtils.getWebApplicationContext(servletContext );
    }
}
```

# spring 中的设计模式

-   工厂模式 - BeanFactory, ApplicationContext 都是工厂
-   单例 - spring 中的 bean 构造默认都是单例
-   代理模式 - AOP
-   装饰器模式 - 类名中含有 Wrapper 的类
-   模板模式 - jdbcTemplate, JpaTemplate, RedisTemplate
-   观察者模式 - Spring 中 Observer 模式常用的地方是 listener 的实现

# spring bean 生命周期

[详细的顺序](https://www.cnblogs.com/zrtqsk/p/3735273.html)

https://crossoverjie.top/2018/03/21/spring/spring-bean-lifecycle/
TODO

1. 首先当然是构造函数实例化 bean, 设置属性;

1. 然后是 Aware 这类接口, 用于在当前类注入某个成员

    (如果实现了这类接口的话, 会执行对应的方法), 比如 BeanNameAware.setBeanName(id), BeanFactoryAware.setBeanFactory, ApplicationContextAware.setApplicationContext

1. BeanPostProcessors 中的方法, postProcessBeforeInitialization ()

    1. 注解@PostConstruct 的方法

        1. InitializingBean.afterPropertiesSet (Bean 的全部属性设置成功后执行的初始化方法, 只有这一个方法)

            1. 我们自定义的 init 方法, 通过 `<bean> 的 init-method 属性指定`

1. BeanPostProcessors 中的方法, postProcessAfterInitialization (是在 Bean 初始化后执行的，时机不同 )

    1. DisposableBean.destroy (销毁前执行)

        1. `<bean>的destroy-method`属性指定的方法

# 常用的 spring bean 拓展接口

## BeanNameAware、ApplicationContextAware 和 BeanFactoryAware

"Aware"的意思是"感知到的", 都有自己 setXXX(xxx) 方法需要覆盖

-   实现 BeanNameAware 接口的 Bean，在 Bean 加载的过程中可以获取到该 Bean 的 id

-   实现 ApplicationContextAware 接口的 Bean，在 Bean 加载的过程中可以获取到 Spring 的 ApplicationContext，这个尤其重要，ApplicationContext 是 Spring 应用上下文，从 ApplicationContext 中可以获取包括任意的 Bean 在内的大量 Spring 容器内容和信息

-   实现 BeanFactoryAware 接口的 Bean，在 Bean 加载的过程中可以获取到加载该 Bean 的 BeanFactory

看看怎么使用

```java
public class AwareBean implements BeanNameAware, BeanFactoryAware, ApplicationContextAware{

// 如果BeanName、ApplicationContext、BeanFactory有用，那么就自己定义一个成员变量将它们保存下来，
// 如果没用，那么只需要实现setXXX()方法，用一下Spring注入进来的参数即可

    private String                     beanName;

    private ApplicationContext        applicationContext;

    private BeanFactory                beanFactory;

    @Override
    public void setBeanName(String beanName){
        System.out.println("Enter AwareBean.setBeanName(), beanName = " + beanName + "\n");
        this.beanName = beanName;
    }

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException{
        System.out.println("Enter AwareBean.setApplicationContext(), applicationContext = " + applicationContext + "\n");
        this.applicationContext = applicationContext;
    }

    @Override
    public void setBeanFactory(BeanFactory beanFactory) throws BeansException{
        System.out.println("Enter AwareBean.setBeanFactory(), beanfactory = " + beanFactory + "\n");
        this.beanFactory = beanFactory;
    }
}
```

## BeanPostProcessor 接口

不是针对单一 bean, 而是针对每个 Bean 的生成前后做一些逻辑操作, 有两个方法, 这两个方法是有返回值的，不要返回 null 否则`getBean`的时候拿不到对象

-   postProcessBeforeInitialization：在初始化 Bean 之前
-   postProcessAfterInitialization：在初始化 Bean 之后

attention: PostProcessorBean 本身也是一个 Bean, 会在初始化前后调用前置后置方法

```java
@Component
public class MyBeanPostProcessor implements BeanPostProcessor {
    //实现BeanPostProcessor接口必须实现两个方法：
    //1.【Object postProcessBeforeInitialization(final Object bean, String beanName)】
    //2.【Object postProcessAfterInitialization(Object bean, String beanName)】
    @Override
    public Object postProcessBeforeInitialization(final Object bean, String beanName)
            throws BeansException {
        System.out.println("初始化方法前");
        // 生成代理对象 javaweb Proxy--jdk动态代理，前提：需要使用接口
        return Proxy.newProxyInstance(
                MyBeanPostProcessor.class.getClassLoader(),     //类加载器
                bean.getClass().getInterfaces(),                //需要实现的接口
                new InvocationHandler(){                        //自定义处理类
                    @Override
                    public Object invoke(
                            Object proxy,                       //代理对象【即创建的动态代理对象】
                            Method method,                     //当前执行方法
                            Object[] args) throws Throwable {  //方法参数
                        String methodName = method.getName();
                        //需要增强的方法
                        if("addOrder".equals(methodName)){
                            System.out.println("代理前面");
                            // 执行目标类
                            Object obj = method.invoke(bean/*被代理的对象*/, args/*方法参数*/);
                            System.out.println("代理后面");
                            return obj;
                        }
                        //不需要增强的方法直接return
                        return method.invoke(bean, args);
                    }
                });
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName)
            throws BeansException {
        System.out.println("初始化方法后");
        return bean;
    }


}

```

## InitialingBean 和 DisposableBean

实现这两个接口以在 bean 构造前后执行自定义的代码; 各有一个唯一的方法, `afterPropertiesSet()`在 Bean 属性(setters())都设置完毕后调用, 做一些初始化的工作, `destory()`在 Bean 生命周期结束前调用 destory()方法做一些收尾工作

但是,Spring 官方不推荐使用上面这种方式;因为它不必要地将代码耦合到 Spring。另外，我们建议使用@PostConstruct注解或指定bean定义支持的通用方法;

-   和 xml 配置 `init-method`、`destory-method`作用类似. InitializingBean 接口、Disposable 接口底层使用`类型强转.方法名()`进行直接方法调用，init-method、destory-method 底层使用反射，前者和 Spring 耦合程度更高但效率高，后者解除了和 Spring 之间的耦合但是效率低; 并且 同时配置的情况下, 前者先执行

-   或者 JSR-250 为初始化之后/销毁之前方法的指定定义了两个注释类，分别是 @PostConstruct 和 @PreDestroy;  Java 9 后可能被废弃, 若还想使用, 可以导入 javax.annotation-api

    要让 JSR-250 的注释生效，除了在 Bean 类中标注这些注释外，还需要在 Spring 容器中注册一个负责处理这些注释的 `org.springframework.context.annotation.CommonAnnotationBeanPostProcessor`, 无需 id; 也可以使用 `<context:annotation-config/>` 简化配置 --> 进一步使用 `<context:component-scan package="" />`

## FactoryBean 接口

http://www.cnblogs.com/davidwang456/p/3688250.html

### FactoryBean 解释

是一个特殊的 bean, 而不是工厂通过 getObject() 可以自定义生成 bean 的逻辑

类似工厂模式, spring 提供了一个顶级工厂接口, 你自己实现具体的工厂 bean;

-   通常 Spring 容器加载一个 Bean 的整个过程，都是由 Spring 控制的, 如果实现 FactoryBean 接口, 我们可以更大的自主权来定制实例化 bean, 可通过 getObject() 定制获取 bean 的逻辑

-   根据该 FactoryBean 的 ID 从 BeanFactory 中获取的实际上是 FactoryBean 的 getObject()返回的对象，而不是 FactoryBean 本身，如果要获取 FactoryBean 对象，请在 id 前面加一个&符号来获取。

-   FactoryBean 这个接口在 Spring 容器中有大量的子实现。

### FactoryBean 接口的 api

```java
T getObject() throws Exception;// context.getBean(id)时, 底层就是调用的这个方法, id前添加"&"则返回的就是这个对象的FactoryBean
Class<?> getObjectType();
default boolean isSingleton() {return true;}

```

### FactoryBean 简单的使用示例

```java
// 一个接口
public interface Animal{
    public void move();
}

// 两个实现类
public class Monkey implements Animal{
    public void move()
    {
        System.out.println("Monkey move!");
    }
}
public class Tiger implements Animal{
    public void move(){
        System.out.println("Tiger move!");
    }
}

// FactoryBean实现
public class AnimalFactoryBean implements FactoryBean<Animal>{
    private String    animal;

    // 最重要的，控制Bean的实例化过程
    public Animal getObject() throws Exception{
        if ("Monkey".equals(animal)){
            return new Monkey();
        }
        else if ("Tiger".equals(animal)){
            return new Tiger();
        }
        else{
            return null;
        }
    }

    // 获取接口返回的实例的class
    public Class<?> getObjectType(){
        return Animal.class;
    }

    // 该Bean是否为一个单例的Bean
    public boolean isSingleton(){
        return true;
    }

    public void setAnimal(String animal){
        this.animal = animal;
    }
}

// 测试
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = {
        "classpath*:spring.xml",
})
public class BaseTest{
    @Resource
    private Animal    animal;

    @Test
    public void aa(){
        animal.move(); // 结果: Tiger move!
    }
}
```

当然还有 xml 配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.1.xsd">
    <!-- 最后得到的并不是FactoryBean本身，而是FactoryBean的泛型对象tiger -->
    <bean id="animal" class="org.xrq.bean.factory.AnimalFactoryBean">
        <property name="animal" value="Tiger"/>
    </bean>
</beans>
```

### factoryBean 在 mabatis-spring 集成中的应用

类似 spring 中的 "静态工厂方法注入"

很多开源项目在集成 Spring 时都使用到 FactoryBean，比如 MyBatis3 提供 mybatis-spring 项目中的 `org.mybatis.spring.SqlSessionFactoryBean`：

```xml
<!-- spring和MyBatis整合，不需要mybatis的配置映射文件 -->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">

    <!-- property 将注入成为 SqlSessionFactoryBean 的成员变量 -->

    <property name="dataSource" ref="dataSource"/>
    <!-- 自动扫描mapping.xml文件 -->
    <property name="mapperLocations" value="classpath:mapper/*.xml"></property>
</bean>

```

### factoryBean 案例-dao 切换

看一个案例: [check here](https://blog.csdn.net/ykdsg/article/details/18733041)

系统发展到一定程度就会面临分库分表的问题，我们要考虑的是怎么让系统平滑的从老的表迁移到新的表。通常新表的数据可以通过 binlog 同步的方式同步老表的数据，在某一个时间点通过开关的方式让读写老表的操作切换到新表。

那这里有个问题: 为什么 要设置一个"开关" 来切换 新/旧 dao 呢? 因为在数据迁移过程中, 系统不够稳定, 需要随时将 新 dao 换回 旧 dao 以保证系统不会崩. 等到数据迁移全部完成, 系统稳定下来后, 可以去掉开关, 直接用 新 dao 替换掉 旧 dao. 此时旧 dao 代码 的人生使命 就完全结束了.

那么具体怎么操作呢 - 通过 FactoryBean 生产 旧 dao 的代理 (作为 新 dao), 在 InvocationHandler 中做开关的逻辑判断

注入的配置时这样的:

```xml
<bean id="oldDao" class="com.taobao.misc.auction.repository.ProxyBidRepo"/> 
<bean id="newDao" class="com.taobao.misc.auction.dao.impl.BidProxyDAOImpl"/>

<bean id="oldDaoProxy" class="com.taobao.misc.auction.dao.impl.ProxyBidDAOProxyFactoryBean"/> 

```

这样系统原来引用 bidProxyDAO 的代码都不需要动就可以实现根据开关来切换新老 DAO 的实现。等系统上线稳定之后可以将 旧 dao 完全替换掉

代码如下:

```java
/*
- InvocationHandler - 代理需要invocationhandler
- FactoryBean       - 实现其 getObject（） 方法; 如果一个类实现了FactoryBean接口，那么context.getBean得到的不是他本身了，而是它所产生的对象,如果希望得到它本身，只需要加上&符号即可
*/
public class ProxyBidDAOProxyFactoryBean implements InvocationHandler, FactoryBean {
    private static Logger log =  LoggerFactory.getLogger(ProxyBidDAOProxyFactoryBean.class);
    /**
     * 新的dao
     */
    @Resource
    private BidProxyDAO bidProxyDAONew;
    /**
     * 老的dao
     */
    @Resource
    private BidProxyDAO proxyBidRepo;

    @Resource
    private DiamondSwitchConfig diamondSwitchConfig;


    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //如果开关打开那么调用新的DAO
        if(diamondSwitchConfig.isNewTableSwitch()){
            log.warn("bid_proxy user new table..........................................");
            return method.invoke(bidProxyDAONew, args);
        }
        return method.invoke(proxyBidRepo, args);
    }

    @Override
    public Object getObject() throws Exception {
        Object proxyObj=Proxy.newProxyInstance(this.getClass().getClassLoader(), new Class[]{BidProxyDAO.class}, this);
        return (BidProxyDAO) proxyObj;
    }

    @Override
    public Class getObjectType() {
        return BidProxyDAO.class;
    }

    @Override
    public boolean isSingleton() {
        return true;
    }
} 

```

## BeanFactory 接口

### BeanFactory 解释

是一个接口, 相当于容器, 管理所有的 bean, 包括 factory bean

-   spring 提供多个实现, 常用的 XmlBeanFactory

-   BeanFactory 和 ApplicationContext(通过前者派生)就是 spring 框架的两个 IOC 容器，现在一般使用 ApplicationnContext，其不但包含了 BeanFactory 的作用，同时还进行更多的扩展。

### api 有哪些

```java
　　boolean containsBean(String beanName) 判断工厂中是否包含给定名称的bean定义，若有则返回true
　　Object getBean(String) 返回给定名称注册的bean实例。根据bean的配置情况，如果是singleton模式将返回一个共享实例，否则将返回一个新建的实例，如果没有找到指定bean,该方法可能会抛出异常
　　Object getBean(String, Class) 返回以给定名称注册的bean实例，并转换为给定class类型
　　Class getType(String name) 返回给定名称的bean的Class,如果没有找到指定的bean实例，则排除NoSuchBeanDefinitionException异常
　　boolean isSingleton(String) 判断给定名称的bean定义是否为单例模式
　　String[] getAliases(String name) 返回给定bean名称的所有别名

```

## BeanFactoryPostProcessor

在所有的 Bean 创建之前，读取 Bean 的元属性(Scope 等等)，并根据自己的需求对元属性进行改变 ---------- 动态注册, 见下文

-   BeanFactoryPostProcessor 的执行先于 BeanPostProcessor

-   BeanFactoryPostProcessor 的 postProcessBeanFactory()方法只会执行一次

```java
public void postProcessBeanFactory(ConfigurableListableBeanFactory configurablelistablebeanfactory)
            throws BeansException{
    BeanDefinition beanDefinition = configurablelistablebeanfactory.getBeanDefinition("common0");
    MutablePropertyValues beanProperty = beanDefinition.getPropertyValues();
    System.out.println("scope before change：" + beanDefinition.getScope());
    beanDefinition.setScope("singleton");
    System.out.println("scope after change：" + beanDefinition.getScope());
    System.out.println("beanProperty：" + beanProperty);
}
```

## InstantiationAwareBeanPostProcessor

继承自 BeanPostProcessor 接口

bean 实例化前后做一些处理(区别于 bean 初始化); 1、实例化----实例化的过程是一个创建 Bean 的过程，即调用 Bean 的构造函数，单例的 Bean 放入单例池中. 2、初始化----初始化的过程是一个赋值的过程，即调用 Bean 的 setter，设置 Bean 的属性

-   Bean 构造出来之前调用 postProcessBeforeInstantiation()方法
-   Bean 构造出来之后调用 postProcessAfterInstantiation()方法

通常来讲，我们不会直接实现 InstantiationAwareBeanPostProcessor 接口，而是会采用继承 InstantiationAwareBeanPostProcessorAdapter 这个抽象类的方式来使用。

# 动态注册 bean

https://blog.csdn.net/neweastsun/article/details/79222256

## 动态注册场景

用户定义一个接口，而接口的实现则由框架生成，不需要用户自行编写，此时实现类就需要动态注册到容器中

## 动态注册 api

-   Spring 中的 bean 定义都保存在 **BeanDefinitionRegistry** 接口中，单例的 bean 的实例都保存在 **SingletonBeanRegistry** 接口中。

    DefaultListableBeanFactory 接口同时实现了这两个接口，在实践中通常会使用这个接口

    这种方式注册的 bean 没有 aop 支持

-   还可以通过 BeanFactoryPostProcessor 进行动态注册

-   ImportBeanDefinitionRegistrar 接口亦可

    所有实现了该接口的类的都会被 ConfigurationClassPostProcessor 处理，ConfigurationClassPostProcessor 实现了 BeanFactoryPostProcessor 接口，所以 ImportBeanDefinitionRegistrar 中动态注册的 bean 是优先与依赖其的 bean 初始化的，也能被 aop、validator 等机制处理。

## 怎么使用动态注册

### 通过 BeanFactoryPostProcessor 实现动态注册

bean 不是在 BeanFactoryPostProcessor 中被注册，那么该 bean 则无法被**BeanPostProcessor**处理，即无法对其应用 aop、Bean Validation 等功能。

BeanDefinitionRegistryPostProcessor

```java
@component
public class PersonBeanFactoryPostProcessor implements BeanFactoryPostProcessor {

    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        DefaultListableBeanFactory defaultListableBeanFactory
                = (DefaultListableBeanFactory) beanFactory;

        //注册Bean定义，容器根据定义返回bean
        log.info("register personManager1>>>>>>>>>>>>>>>>");
        // 构造bean 定义
        BeanDefinitionBuilder beanDefinitionBuilder =
                BeanDefinitionBuilder.genericBeanDefinition(PersonManager.class);
        // 构造依赖
        beanDefinitionBuilder.addPropertyReference("personDao", "personDao");
        BeanDefinition personManagerBeanDefinition = beanDefinitionBuilder.getRawBeanDefinition();
        // 注册
        defaultListableBeanFactory.registerBeanDefinition("personManager1", personManagerBeanDefinition);

        //直接通过编程式注册bean实例
        log.info("register personManager2>>>>>>>>>>>>>>>>");
        PersonDao personDao = beanFactory.getBean(PersonDao.class);
        PersonManager personManager = new PersonManager();
        personManager.setPersonDao(personDao);
        beanFactory.registerSingleton("personManager2", personManager);

    }
}

```

### 通过 BeanDefinitionRegistry 在普通 bean 中注册

bean 不是在 BeanFactoryPostProcessor 中被注册，那么该 bean 则无法被**BeanPostProcessor**处理，即无法对其应用 aop、Bean Validation 等功能。

```java
@RestController
@Slf4j
public class PersonManagerRegisterController {

    /**
     * The Application context.
     */
    @Autowired
    GenericApplicationContext applicationContext;

    /**
     * The Bean factory.
     继承了 SingletonBeanRegistry
     */
    @Autowired
    ConfigurableBeanFactory beanFactory;

    /**
     * 动态注册bean，此处注册的bean没有AOP的支持
     * curl http://localhost:8080/registerPersonManager
     */
    @GetMapping("/registerPersonManager")
    public void registerPersonManager() {
        PersonDao personDao = applicationContext.getBean(PersonDao.class);
        PersonManager personManager = new PersonManager();
        personManager.setPersonDao(personDao);
        beanFactory.registerSingleton("personManager3", personManager);

    }

    ...

```

### 通过 ImportBeanDefinitionRegistrar

https://github.com/pkpk1234/registerbean-ImportBeanDefinitionRegistrar

需求, 思路:

```java
/*
主要思路是利用ClassPathScanningCandidateComponentProvider获取标注了HTTPUtil注解的接口，并使用JDK动态代理生成代理对象。
然后使用DefaultListableBeanFactory将代理对象注册到容器中
*/
@Component
@HTTPUtil
public interface IRequestDemo {
    //调用test1时，会对http://abc.com发送get请求
    @HTTPRequest(url = "http://abc.com")
    HttpResult<String> test1();

    //调用test2时，会对http://test2.com发送post请求
    @HTTPRequest(url = "http://test2.com", httpMethod = HTTPMethod.POST)
    HttpResult<String> test2();
}

```

实现:

```java
// 1. 首先编写核心ImportBeanDefinitionRegistrar接口

//ImportBeanDefinitionRegistrar需要配合@Configuration和@Import注解
//@Configuration定义Java格式的Spring配置文件，@Import注解导入实现了ImportBeanDefinitionRegistrar接口的类。

public class HTTPRequestRegistrar implements ImportBeanDefinitionRegistrar,
       ResourceLoaderAware, BeanClassLoaderAware, EnvironmentAware, BeanFactoryAware {
   @Override
   public void registerBeanDefinitions(AnnotationMetadata annotationMetadata, BeanDefinitionRegistry beanDefinitionRegistry) {
       registerHttpRequest(beanDefinitionRegistry);
   }

   /**
    * 注册动态bean的主要方法
    *
    * @param beanDefinitionRegistry
    */
   private void registerHttpRequest(BeanDefinitionRegistry beanDefinitionRegistry) {
       ClassPathScanningCandidateComponentProvider classScanner = getClassScanner();
       classScanner.setResourceLoader(this.resourceLoader);
       //指定只关注标注了@HTTPUtil注解的接口
       AnnotationTypeFilter annotationTypeFilter = new AnnotationTypeFilter(HTTPUtil.class);
       classScanner.addIncludeFilter(annotationTypeFilter);
       //指定扫描的基础包
       String basePack = "com.example.registerbean";
       Set<BeanDefinition> beanDefinitionSet = classScanner.findCandidateComponents(basePack);
       for (BeanDefinition beanDefinition : beanDefinitionSet) {
           if (beanDefinition instanceof AnnotatedBeanDefinition) {
               registerBeans(((AnnotatedBeanDefinition) beanDefinition));
           }
       }
   }

   /**
    * 创建动态代理，并动态注册到容器中
    *
    * @param annotatedBeanDefinition
    */
   private void registerBeans(AnnotatedBeanDefinition annotatedBeanDefinition) {
       String className = annotatedBeanDefinition.getBeanClassName();
       ((DefaultListableBeanFactory) this.beanFactory).registerSingleton(className, createProxy(annotatedBeanDefinition));
   }

   /**
    * 构造Class扫描器，设置了只扫描顶级接口，不扫描内部类
    *
    * @return
    */
   private ClassPathScanningCandidateComponentProvider getClassScanner() {
       return new ClassPathScanningCandidateComponentProvider(false, this.environment) {

           @Override
           protected boolean isCandidateComponent(
                   AnnotatedBeanDefinition beanDefinition) {
               if (beanDefinition.getMetadata().isInterface()) {
                   try {
                       Class<?> target = ClassUtils.forName(
                               beanDefinition.getMetadata().getClassName(),
                               classLoader);
                       return !target.isAnnotation();
                   } catch (Exception ex) {
                       log.error("load class exception:", ex);
                   }
               }
               return false;
           }
       };
   }

   /**
    * 创建动态代理
    *
    * @param annotatedBeanDefinition
    * @return
    */
   private Object createProxy(AnnotatedBeanDefinition annotatedBeanDefinition) {
       try {
           AnnotationMetadata annotationMetadata = annotatedBeanDefinition.getMetadata();
           Class<?> target = Class.forName(annotationMetadata.getClassName());
           InvocationHandler invocationHandler = createInvocationHandler();
           Object proxy = Proxy.newProxyInstance(HTTPRequest.class.getClassLoader(), new Class[]{target}, invocationHandler);
           return proxy;
       } catch (ClassNotFoundException e) {
           log.error(e.getMessage());
       }
       return null;
   }

   /**
    * 创建InvocationHandler，将方法调用全部代理给DemoHttpHandler
    *
    * @return
    */
   private InvocationHandler createInvocationHandler() {
       return new InvocationHandler() {
           private DemoHttpHandler demoHttpHandler = new DemoHttpHandler();

           @Override
           public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

               return demoHttpHandler.handle(method);
           }
       };
   }
    ... 省略setter代码
}

////////////////////////////////////////////////////////////

//2. 编写注解，并在其中使用@Import导入第1步编写的HTTPRequestRegistrar
//3.将@EnableHttpUtil添加到@Configuration注解下

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Import(HTTPRequestRegistrar.class)
public @interface EnableHttpUtil {
}

```

# 国际化 and 参数校验

https://blog.csdn.net/shuangyidehudie/article/details/40586143
https://blog.csdn.net/catoop/article/details/51284638



# spring5 新特性

https://www.ibm.com/developerworks/cn/java/j-whats-new-in-spring-framework-5-theedom/index.html

# Guice

https://www.ibm.com/developerworks/cn/java/j-guice.html

编译器依赖注入：Dagger2 - https://zhuanlan.zhihu.com/p/24454466
