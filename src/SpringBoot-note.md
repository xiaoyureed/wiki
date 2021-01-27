---
title: SpringBoot
tags:
  - springboot
date: 2017-12-30 16:06:29
categories: java web
---

<div align="center">
记录springboot学习中的问题, 总结;
https://github.com/h2pl web 项目
https://github.com/dreamhead/moco mock 框架

https://github.com/yinjihuan/monkey-api-encrypt api 加解密 
TODO

https://github.com/cloudfavorites/favorites-web 单体应用实例 hibernate jpa

</div>

<!--more-->


<!-- TOC -->

- [常用命令](#常用命令)
- [工具类](#工具类)
  - [bean 复制 克隆](#bean-复制-克隆)
  - [字符串](#字符串)
  - [编解码](#编解码)
  - [commons-lang3](#commons-lang3)
  - [commons-collections4](#commons-collections4)
- [Spring Boot中的注解](#spring-boot中的注解)
  - [@ConfigurationProperties 和 @Value](#configurationproperties-和-value)
  - [@EnableConfigurationProperties](#enableconfigurationproperties)
  - [@EnableAutoConfiguration](#enableautoconfiguration)
  - [@ComponentScan](#componentscan)
  - [@SpringBootApplication](#springbootapplication)
  - [@Component和@Bean和@Configration区别](#component和bean和configration区别)
  - [@Import和@ImportResource和@PropertySource比较](#import和importresource和propertysource比较)
- [静态注入](#静态注入)
  - [set注入](#set注入)
  - [@PostConstruct 注入](#postconstruct-注入)
  - [通过 ApplicationContext](#通过-applicationcontext)
- [使用starter](#使用starter)
  - [加载过程or原理](#加载过程or原理)
  - [自动配置类会用到的一些注解](#自动配置类会用到的一些注解)
  - [自动配置类的实例](#自动配置类的实例)
    - [spring 动态代理](#spring-动态代理)
  - [@Conditional](#conditional)
  - [封装组合条件](#封装组合条件)
  - [创建自己的 starter](#创建自己的-starter)
  - [反爬虫 starter](#反爬虫-starter)
- [监控 and 安全](#监控-and-安全)
  - [spring-boot-actuator 监控](#spring-boot-actuator-监控)
  - [spring boot admin 监控界面](#spring-boot-admin-监控界面)
  - [spring-boot-starter-security](#spring-boot-starter-security)
    - [security组件介绍](#security组件介绍)
    - [基本使用 security](#基本使用-security)
    - [配置跨域](#配置跨域)
  - [oauth2](#oauth2)
- [日志](#日志)
  - [Log4j2-Marker 写入到指定日志文件](#log4j2-marker-写入到指定日志文件)
- [数据层](#数据层)
  - [启动执行 SQL](#启动执行-sql)
    - [SQL 分隔符的坑](#sql-分隔符的坑)
    - [spring datasource 自带](#spring-datasource-自带)
    - [使用 jpa](#使用-jpa)
    - [DataSourceInitializer](#datasourceinitializer)
    - [maven-antrun-plugin](#maven-antrun-plugin)
  - [整合 spring jdbc](#整合-spring-jdbc)
  - [整合 hibernate](#整合-hibernate)
    - [审计 createdDate](#审计-createddate)
    - [执行原生 SQL](#执行原生-sql)
    - [动态 SQL](#动态-sql)
    - [spring-data-rest 配合 spring-data-jpa](#spring-data-rest-配合-spring-data-jpa)
  - [整合 mybatis-plus](#整合-mybatis-plus)
  - [整合 mybatis](#整合-mybatis)
    - [基本配置](#基本配置)
    - [使用 拦截器 interceptor plugins](#使用-拦截器-interceptor-plugins)
      - [principle of plugins](#principle-of-plugins)
      - [use scenarios](#use-scenarios)
      - [how to use plugins](#how-to-use-plugins)
    - [typehandler](#typehandler)
    - [全注解开发](#全注解开发)
    - [动态定时刷新 SQL mapper 实现热加载](#动态定时刷新-sql-mapper-实现热加载)
    - [自定义资源文件打包目录 and 开启占位符过滤](#自定义资源文件打包目录-and-开启占位符过滤)
  - [springboot 事务](#springboot-事务)
    - [事务基本使用](#事务基本使用)
    - [spring 事务传播失效的坑](#spring-事务传播失效的坑)
    - [Transactional 注解](#transactional-注解)
  - [缓存](#缓存)
    - [springboot-starter-cache](#springboot-starter-cache)
    - [caffeine](#caffeine)
  - [配合 h2 数据库进行开发](#配合-h2-数据库进行开发)
    - [h2 和 spring data jpa 配合](#h2-和-spring-data-jpa-配合)
    - [和 mybatis-plus 配合](#和-mybatis-plus-配合)
  - [多数据源](#多数据源)
    - [基本配置 全注解开发](#基本配置-全注解开发)
    - [AbstractRoutingDataSource 动态数据源切换](#abstractroutingdatasource-动态数据源切换)
  - [多数据库类型 databaseIdProvider](#多数据库类型-databaseidprovider)
- [web 相关](#web-相关)
  - [springboot 发送 https 客户端 client](#springboot-发送-https-客户端-client)
  - [RequestContextHolder](#requestcontextholder)
  - [解决 api 版本共存](#解决-api-版本共存)
    - [RequestMappingHandlerMapping](#requestmappinghandlermapping)
    - [request matcher](#request-matcher)
    - [request condition](#request-condition)
  - [自定义接收参数类型](#自定义接收参数类型)
  - [返回图片](#返回图片)
  - [rest api 文档](#rest-api-文档)
  - [过滤器 和 拦截器](#过滤器-和-拦截器)
    - [区别](#区别)
    - [使用 filter](#使用-filter)
    - [使用 interceptor](#使用-interceptor)
      - [基本使用拦截器](#基本使用拦截器)
      - [基于 url 拦截](#基于-url-拦截)
      - [基于注解拦截](#基于注解拦截)
  - [servlet Druid 监控](#servlet-druid-监控)
  - [aop 整合使用](#aop-整合使用)
    - [基本使用](#基本使用)
    - [切入点表达式](#切入点表达式)
  - [@controllerAdvice 异常处理](#controlleradvice-异常处理)
  - [HttpServletRequest 的输入流只能读取一次的问题](#httpservletrequest-的输入流只能读取一次的问题)
  - [路由处理](#路由处理)
    - [静态资源映射 暴露静态资源](#静态资源映射-暴露静态资源)
    - [添加 view controller](#添加-view-controller)
  - [spring-boot-starter-thymeleaf](#spring-boot-starter-thymeleaf)
- [运维部署](#运维部署)
  - [支持 https ssl](#支持-https-ssl)
    - [nginx 配置 https](#nginx-配置-https)
    - [支持 https 访问](#支持-https-访问)
    - [http 请求自动转为 https](#http-请求自动转为-https)
    - [同时支持 http https](#同时支持-http-https)
  - [容器化部署](#容器化部署)
  - [nginx 反向代理](#nginx-反向代理)
  - [内嵌 tomcat 调优](#内嵌-tomcat-调优)
  - [配置 war 包部署到 Tomcat](#配置-war-包部署到-tomcat)
- [spring boot 中的并发](#spring-boot-中的并发)
- [实现乐观锁悲观锁](#实现乐观锁悲观锁)
- [移动端消息推送](#移动端消息推送)
- [即时通信 IM系统](#即时通信-im系统)
- [辅助工具](#辅助工具)
  - [lombok notice](#lombok-notice)
  - [devtools](#devtools)
  - [maven wrapper](#maven-wrapper)
- [校验](#校验)
  - [校验配置文件](#校验配置文件)
  - [校验请求参数](#校验请求参数)
  - [自定义校验](#自定义校验)
  - [@Validated和@Valid](#validated和valid)
- [runner 获取命令行参数](#runner-获取命令行参数)
  - [获取命令行参数](#获取命令行参数)
  - [`ApplicationRunner` or `CommandLineRunner`](#applicationrunner-or-commandlinerunner)
- [开发命令行应用](#开发命令行应用)
- [整合 grpc](#整合-grpc)
- [任务调度](#任务调度)
  - [整合 XXL-JOB 任务调度平台](#整合-xxl-job-任务调度平台)
  - [异步任务](#异步任务)
  - [定时任务](#定时任务)
    - [springboot 内置](#springboot-内置)
    - [集成 Quartz](#集成-quartz)
- [配置文件 and 环境](#配置文件-and-环境)
  - [读取配置文件](#读取配置文件)
  - [配置文件优先级](#配置文件优先级)
  - [读取环境信息](#读取环境信息)
  - [两种引入 springboot 方式](#两种引入-springboot-方式)
- [序列化 反序列化](#序列化-反序列化)
  - [jackson 使用](#jackson-使用)
    - [@JsonComponent](#jsoncomponent)
    - [Jackson 在 springboot 中的配置](#jackson-在-springboot-中的配置)
    - [jackson注解](#jackson注解)
    - [objectmapper 定制](#objectmapper-定制)
    - [objectmapper 使用](#objectmapper-使用)
- [拾遗](#拾遗)
  - [bean 懒加载](#bean-懒加载)

<!-- /TOC -->

# 常用命令

```sh
du -sh *.jar # jar size

# 打包时没有指定主类，执行可以用java -cp {xxx.jar} {主类名称（绝对路径）}。
# -cp 指定类运行所依赖其他类的路径，通常是类库和jar包, 
# 等价 -classpath
# 多个jar包之间连接符：window上分号“;”.Linux下使用“:”
java -cp .;d:\work\other.jar;d:\work\my.jar packname.mainclassname 
java -cp .:/hone/myuser/work/other.jar:/hone/myuser/work/my.jar packname.mainclassname 
# 表达式支持通配符
java -cp .;c:\work\my.jar;c:\work\*.jar packname.mainclassname 


# 打包时指定了主类，执行可以直接用java -jar {xxx.jar}。
# 会用到目录META-INF\MANIFEST.MF文件，在该文件中，有一个叫Main－Class的参数，它说明了java -jar命令执行的类
# java -jar方式不可以指定附加依赖jar包。
java -jar *.jar

# 依赖多个 jar 不使用通配符也可以这样执行
# -D 设置系统属性, 通过System.getProperty("conf1");获得这个值
java -Djava.ext.dirs=lib MyClass 

# 跳过测试
# -D 表示 maven 中的 properties
mvn -Dmaven.test.skip=true clean package # skip unit test
mvn -DskipTests clean package

mvn clean package -Pxxx # 指定要激活 maven profile 的 id

java -Dserver.port=8010 -jar xxx.jar # specific server port
java -jar *.jar --server.port=8010 # 同上

# 指定 profile
java -jar *.jar --spring.properties.active=dev
java -jar -Dspring.profiles.active=dev demo-0.0.1-SNAPSHOT.jar
mvn spring-boot:run -Drun.profiles=dev




```


# 工具类

## bean 复制 克隆

```java
beancopier

https://github.com/mapstruct/mapstruct
```

## 字符串

```java
// >>> springboot 自带

RandomStringUtils 生成随机字符串, 随机数字
StringUtils.isEmpty() 并非严格的空

```

## 编解码

```java
// >>> springboot 自带

public static String getMD5(String str) {
        String base = str + "/" + salt;
        String md5 = DigestUtils.md5DigestAsHex(base.getBytes());
        return md5;
    }

```

## commons-lang3

## commons-collections4

```java
isSubCollection(smallColl, bigColl)


/**
    * 1、除非元素为null，否则向集合添加元素
    */
CollectionUtils.addIgnoreNull(personList,null);


// >>> 合并

/**
    * 2、将两个已排序的集合a和b合并为一个已排序的列表，以便保留元素的自然顺序
    */
CollectionUtils.collate(Iterable<? extends O> a, Iterable<? extends O> b)
/**
    * 3、将两个已排序的集合a和b合并到一个已排序的列表中，以便保留根据Comparator c的元素顺序。
    */
CollectionUtils.collate(Iterable<? extends O> a, Iterable<? extends O> b, Comparator<? super O> c)


// >>> 判空

/**
    * 5、如果参数是null，则返回不可变的空集合，否则返回参数本身。（很实用 ,最终返回List EMPTY_LIST = new EmptyList<>()）
    */
CollectionUtils.emptyIfNull(Collection<T> collection)
/**
    * 6、空安全检查指定的集合是否为空
    */
CollectionUtils.isEmpty(Collection<?> coll)
/**
    * 7、 空安全检查指定的集合是否为空。
    */
CollectionUtils.isNotEmpty(Collection<?> coll)



// >> 判断是否包含 

/**
    * 4、返回该个集合中是否含有至少有一个元素
    */
CollectionUtils.containsAny(Collection<?> coll1, T... coll2)


// >>> 翻转

/**
    * 8、反转给定数组的顺序。
    */
CollectionUtils.reverseArray(Object[] array);



/**
    * 9、差集   or   chaList.removeAll(listB);
    or          listA.stream().filter(item -> !listB.contains(item)).collect(toList());
    */
CollectionUtils.subtract(Iterable<? extends O> a, Iterable<? extends O> b)
/**
    * 10、并集    or     先 bingList.removeAll(listB), 再bingList.addAll(listB); ((先做差集再做添加所有）)
    */
CollectionUtils.union(Iterable<? extends O> a, Iterable<? extends O> b)
/**
    * 11、交集   也可用 jiaoList.retainAll(listB); 返回 Boolean, 会改变原有集合
    or          listA.stream().filter(item -> listB.contains(item)).collect(toList())
    */
CollectionUtils.intersection(Collection a, Collection b) // 返回新集合
/**
    *12、 交集的补集（析取）
    */
CollectionUtils.disjunction(Collection a, Collection b)
```

# Spring Boot中的注解

## @ConfigurationProperties 和 @Value

@Value 是core container的feature。不支持宽松绑定，不支持Meta-data。但支持spELl。
@ConfigurationProperties 则支持宽松绑定，支持Meta-data。但不支持spELl。

@ConfigurationProperties一般只用来处理环境信息，不用来注入自定义属性（这段不知道翻译得准不准，原文如下：Even if the configuration above will create a regular bean for FooProperties, we recommend that @ConfigurationProperties only deal with the environment and in particular does not inject other beans from the context. Having said that, The @EnableConfigurationProperties annotation is also automatically applied to your project so that any existing bean annotated with @ConfigurationProperties will be configured from the Environment.）   

配置文件：  
```yml
myProps: #自定义的属性和值    
  simpleProp: simplePropValue    
  arrayProps: 1,2,3,4,5    
  listProp1:    
    - name: abc    
      value: abcValue    
    - name: efg    
      value: efgValue    
  listProp2:    
    - config2Value1    
    - config2Vavlue2    
  mapProps:    
    key1: value1    
    key2: value2  
```

创建一个bean来接受信息：(这里不推荐这么用, 应该使用 @value 老老实实属性写全)
```java
@Component    
@ConfigurationProperties(prefix="myProps") //接收application.yml中的myProps下面的属性   
// 此时 idea 提示 spring boot config annotation processor not configured, 是因为他以为你要为配置文件指定自定义属性, 需要生成提示 
// 可以https://docs.spring.io/spring-boot/docs/2.3.4.RELEASE/reference/html/appendix-configuration-metadata.html#configuration-metadata-annotation-processor
public class MyProps {    
    private String simpleProp;    
    private String[] arrayProps;    
    private List<Map<String, String>> listProp1 = new ArrayList<>(); //接收prop1里面的属性值    
    private List<String> listProp2 = new ArrayList<>(); //接收prop2里面的属性值    
    private Map<String, String> mapProps = new HashMap<>(); //接收prop1里面的属性值    
}
```

除了可以使用@ConfigurationProperties注解一个类，还可以在@Bean方法上使用；需要绑定属性到不受你控制的第三方组件时，这种方式非常有用。  

为了从环境属性配置一个bean，将@ConfigurationProperties添加到它的bean注册过程,任何以foo为前缀的属性定义都会被映射到FooComponent上：  

```java
@ConfigurationProperties(prefix = "foo")
@Bean
public FooComponent fooComponent() {
...
}
```

对于无法转换的属性, 比如本来是一个 Boolean 类型, 但是配置文件中配置的是 string 类型, ignoreInvalidFields 可以决定是否抛出异常, 默认为 false, 表示要抛出异常

对于配置文件中多余的属性, ignoreUnknownFields 决定是否抛出异常, 默认 true, 表示不抛出异常

## @EnableConfigurationProperties

配置哪些类是用来接受配置文件信息的bean,当@EnableConfigurationProperties注解应用到你的@Configuration时，任何被@ConfigurationProperties注解的beans将自动被Environment属性配置  , configBean 无需 @component

    @EnableConfigurationProperties({LiquibaseProperties.class, ApplicationProperties.class})

当一个 @ConfigurationProperties bean 像如上这样注册，这个bean就有了一个名字，规范是这样：`<prefix>-<fqn>`, 即：前缀-全限定名，如：  

    myProps-com.xy.PropertiesBean


enableXXX 类注解原理就是 定义一个注解, 在注解上通过 @import 引入一个配置类


## @EnableAutoConfiguration

根据添加的jar依赖猜测你想如何配置Spring，如果发现应用了你不想要的特定自动配置类，你可以使用 @EnableAutoConfiguration 注解的exclude属性来禁用它们  

    @EnableAutoConfiguration(exclude = {MetricFilterAutoConfiguration.class, MetricRepositoryAutoConfiguration.class})

使用一般是@EnableAutoConfiguration加到一个主@configration类上，主配置类一般就是app的启动类  

## @ComponentScan

扫描所有Spring组件，通过@autowired注入使用  

如果使用上面建议的结构组织代码（将应用类放到根包下），你可以添加 @ComponentScan 注解而不需要任何参数。你的所有应用程序组件（ @Component , @Service , @Repository , @Controller 等）将被自动注册为Spring Beans。  

## @SpringBootApplication

@SpringBootApplication 注解等价于以默认属性使用 @Configuration ， @EnableAutoConfiguration 和 @ComponentScan   

## @Component和@Bean和@Configration区别

@Configuration 注解本质上还是 @Component, 

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Configuration {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";

    boolean proxyBeanMethods() default true;// 默认为 true, config bean 中的方法被 cglib 代理, 表示 在同个 config bean 中嵌套调用方法, 返回的对象默认从 ioc container 中获取, 不会新创建对象
}

```

@Configuration一般和@bean合用, @component和@bean不能合用

Spring Boot提倡基于Java的配置，通常简易启动类作为主配置类，其他配置类通过@import导入到主配置类；@ComponentScan也会扫描配置类  ,如果不得不使用基于xml的配置，仍旧可以从一个@configration类开始，通过@ImportResource 注解加载XML配置文件。  

@Component被用在要被自动扫描和装配的类上，Spring 注解@Component等效于@Service,@Controller,@Repository；@Bean只能用于方法(方法名作为ioc容器中的bean name)和注解上。用@Configuration注解该类，等价 与XML中配置beans；用@Bean标注方法等价于XML中配置bean。  

在项目中，本工程的bean注册，使用@Component，通过web service取得的bean，通过@Bean标注在获取方法上注册；

## @Import和@ImportResource和@PropertySource比较

*   前两者都需要和@configuration配合使用(Both @Import @ImportResource work with @Configuration), 表示导入额外的配置
*   @ImportResource用来引入外置的配置文件, 如@ImportResource({"classpath:datasource.xml"}), 一般用来导入Spring的配置文件，让配置文件里面功能生效
*   @Import引入另外的@configuration类
*   @PropertySource用于加载另外的属性配置文件, 如@PropertySource(value = {"classpath:person.properties"}), 一般和@ConfigurationProperties(prefix = "person")一起用
    springboot默认只会加载application.properties中的属性,

例子:

```java
@Configuration
@Import(AnotherConfig.class)
@ImportResource(“classpath:/com/acme/properties-config.xml”)
@PropertySource(“classpath:some.properties”)
public class AppConfig
{
// blah…
}

```

# 静态注入

为静态属性注入值, 使用通过 XXXClass.xxx 使用

## set注入

```java
@Component(value = "MongoConfig")
@ConfigurationProperties(prefix = "mongo.config")
public class MongoConfig {
 
 private static String chat_username;
 
 @Value("${mongo.config.username}")
 public void setChat_username(String chat_username) {
 MongoConfig.chat_username = chat_username;
 }
}
```

## @PostConstruct 注入

```java
@Component
public class ADUserUtils {
 
    @Resource
    private  ADConfig adc;
 
    private  static ADConfig adc1;
 
    @PostConstruct
    public void init() {
        this.adc1 = adc;
    }
 
    public static List<User> getADUsers() {
        adc1.readConf();
} 
```

## 通过 ApplicationContext

得到静态的 context, 然后通过 getBean(xxx.class) ...

```java
@SpringBootApplication
@MapperScan({"io.github.xiaoyureed.mockitomybatisplusdemo.mapper"})
public class MockitoDemoApplication {

    private static ConfigurableApplicationContext context;

    public static void main(String[] args) {
        context = SpringApplication.run(MockitoDemoApplication.class, args);
    }

}
```

# 使用starter

servlet request wrapper

https://blog.csdn.net/weixin_43935907/article/details/98476935
https://www.cnblogs.com/JAYIT/p/10943155.html
https://www.cnblogs.com/exmyth/p/10332328.html


## 加载过程or原理

@SpringBootApplication包含了@EnableAutoConfiguration，这个注解又包含@Import(AutoConfigurationImportSelector.class)。

spring boot启动时，会调用AutoConfigurationImportSelector的回调函数，让其解析所有包下的"META-INF/spring.factories" (位于项目结构的 `resources/META-INF/spring.factories` 下), 并把key为"...EnableAutoConfiguration"对应的value里的类都加载为Bean，而那些类基本都是注解了@Configuration的配置类。这便是spring boot能自动配置的原因。

## 自动配置类会用到的一些注解

https://my.oschina.net/u/566591/blog/2250290

```
@Configuration  表明是一个配置类
@EnableAutoConfiguration(exclude = {DataSourceAutoConfiguration.class}) 使用starter的时候排除自动配置


@ConditionalOnClass  当classpath下发现该类的情况下进行自动配置 (name, value 任选其一)
@ConditionalOnMissingClass 没有这个类时应用标注的配置类

@ConditionOnMissingBean 在spring容器中没找到 bean 时 (如果不指定, 默认 方法返回值类型/当前类)
@ConditionalOnBean  当容器中有指定的Bean的条件下
# 默认是检查bean的类型, 看看容器中是否有同类型 bean 存在, 不看 bean name;
# 开启按照 bean name 检查: 属性 name = {xxx"}
# @ConditionalOnBean(name = {"hah", "hoo"}): "and" 关系, 有 hah, 且有 hoo, return true. 任何一个缺失都return false
# @ConditionalOnMissingBean(name = {"hah", "hoo"}): "and " 关系, 没hah 且 没 hoo, return true. 任何一个出现, return false


@EnableConfigurationProperties(XxxProperties.class)  使 @ConfigurationProperties(prefix="aaa.bbb") 注解生效
@ConditionalOnProperty:指定的属性有指定的值时
# ConditionalOnProperty
    name/value (value 用于指定某配置一定要出现), havingValue 指定配置文件中的属性, 为 null返回 false, 不匹配返回false
    matchIfMissing 配置项如果缺失, 是否匹配, 默认 false
# 优先级比 onBean, onClass 这些都高
@ConditionalOnProperty(prefix = "spring.aop", name = "auto", havingValue = "true", matchIfMissing = true)




@ConditionalOnSingleCandidate: (和 @ConditionalOnBean 类似)当指定的Bean在容器中只有一个，或者在有多个Bean的情况下，用来指定首选的Bean 
@ConditionalOnExpression:基于SpEL表达式作为判断条件
@ConditionalOnResource(resources = {"/xxx.yml"}):类路径下是否有指定的资源
@ConditionalOnJava(javaversion.eight):基于JVM/Java版本作为判断条件
@ConditionalOnJndi:在JNDI存在的条件下查找指定的位置, 必须存在一个.否则,返回不匹配

@ConditionalOnNotWebApplication:当前项目不是Web项目的条件下
@ConditionalOnWebApplication:当前项目是Web项目的条件下


@AutoConfigureAfter(A.class) 配置类将在 A.class 之后加载
@AutoConfigureBefore   ... 之前加载
@AutoConfigureOrder

```

## 自动配置类的实例

### spring 动态代理

https://lolico.me/2020/05/03/SpringBoot2-AOP-uses-cglib-proxy-by-default/

```java
//SpringBoot1.5.x中的这个自动配置类会发现默认是使用jdk动态代理，那么为什么在SpringBoot2.x中改为默认使用Cglib了
@Configuration(proxyBeanMethods = false)
@ConditionalOnProperty(prefix = "spring.aop", name = "auto", havingValue = "true", matchIfMissing = true)
public class AopAutoConfiguration {

    @Configuration(proxyBeanMethods = false)
    @ConditionalOnClass(Advice.class)
    static class AspectJAutoProxyingConfiguration {

        @Configuration(proxyBeanMethods = false)
        @EnableAspectJAutoProxy(proxyTargetClass = false)
        @ConditionalOnProperty(prefix = "spring.aop", name = "proxy-target-class", havingValue = "false",
                matchIfMissing = false)
        static class jdkDynamicAutoProxyConfiguration {

        }

        @Configuration(proxyBeanMethods = false)
        @EnableAspectJAutoProxy(proxyTargetClass = true)
        @ConditionalOnProperty(prefix = "spring.aop", name = "proxy-target-class", havingValue = "true",
                matchIfMissing = true) // 能够看出AOP默认使用Cglib代理
        static class CglibAutoProxyConfiguration {

        }

    }
    // ...
}
```

## @Conditional

自动配置相关注解底层都是通过 @Conditional(OnClassCondition.class) 实现的

通过它, 可以自己定制条件

https://my.oschina.net/u/566591/blog/2250290

## 封装组合条件

继承 AllNestedConditions 封装多条件 (and 关系)

AnyNestedCondition (or 关系)

NoneNestedConditions (none 关系)

## 创建自己的 starter

[官方文档](https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/htmlsingle/#boot-features-custom-starter)

[spring-boot-swagger 示例](https://github.com/SpringForAll/spring-boot-starter-swagger)

- 创建一个starter 项目

    命名规则: spring boot 官方 starter 命名为 "spring-boot-starter-xxx", 自定义的starter 命名 "xxx-spring-boot-starter";

    引入 `spring-boot-configuration-processor` (编译时生成 spring-configuration-metadata.json ，在IDE中编辑配置文件时，会出现提示), 标注为 optional: true 避免依赖传递

    引入 `spring boot starter` (相关注解需要), 标注为 optional true,  idea 中 spring boot app 创建时自动导入了

    继承 `spring-boot-starter-parent`, [如果想不通过继承方式使用, 可以 dependencyManagement `spring boot dependencies`, 参考](https://stackoverflow.com/questions/21317006/spring-boot-parent-pom-when-you-already-have-a-parent-pom/21318359#21318359)

    如果是生成的 spring boot 项目, 删除 启动类, 然后替换 spring boot maven 插件 为 `maven-compile-plugin` (因为没有启动类, repackage 会报错), 设置 properties 下 maven.compiler.source, maven.compiler.target, java.version

- 创建一个 @ConfigurationProperties 类用于保存你的配置信息, 在 @EnableConfigurationProperties(...) 类上指定, (或者标注 component 然后配置componentScan 或者直接 @configurationPropertiesScan)

    （如果你的项目不使用配置信息则可以跳过这一步，不过这种情况非常少见）

- 创建一个AutoConfiguration，引用定义好的配置信息；在AutoConfiguration中实现所有starter应该完成的操作，并且把这个类加入classpath:/META-INF/spring.factories配置文件中进行声明

    自动配置类不应启用组件扫描以查找其他组件 (即 若生成的是spring boot 骨架, 应去掉启动类)。应该使用 @import 来导入其他配置类

- 打包发布

    如果 starter 就是本地自己使用, 可以不打包不发布, 直接在另外的项目中引入

spring.factories 示例:

```
org.springframework.boot.autoconfigure.EnableAutoConfiguration = \
com.mycorp.libx.autoconfigure.LibXAutoConfiguration,\
com.mycorp.libx.autoconfigure.LibXWebAutoConfiguration
```

## 反爬虫 starter

https://www.cnblogs.com/zouhao/p/12175653.html 

TODO

# 监控 and 安全

密码加密 Argon2

Spring Vault，它为HashiCorp Vault添加抽象 -> 密码存储


OWASP ZAP 渗透测试报告

## spring-boot-actuator 监控

https://www.cnblogs.com/zwqh/p/11851300.html
https://www.jianshu.com/p/8bfac9289c7e

spring boot 的 sub project, 提供产品级别的监控

spring cloud 中, refresh 的 post 端点被 actuator 接管, 配置 `management.endpoints.web.exposure.include=health,info` (默认) 需要添加 'refresh', 最终需要 `curl -d{} http://xxx:xxx/actuator/refresh`

[hystrix.stream 也被接管](https://github.com/Netflix/Hystrix/issues/1566), 配置添加 'hystrix.stream', 监控流为 `http://xxx:xxx/actuator/hystrix.stream`

[actuator 翻译](https://www.jianshu.com/p/d5943e303a1f)
[actuator 官方文档](https://docs.spring.io/spring-boot/docs/2.2.5.RELEASE/reference/html/production-ready-features.html#production-ready)


自己写监控, 可以:

```java
ConfigurableApplicationContext context = SpringApplication.run(PaymentServiceApp.class, args);
        ConfigurableEnvironment        env     = context.getEnvironment();

// 或者 直接 @autowired Environment env
```

ui展示: https://github.com/codecentric/spring-boot-admin

## spring boot admin 监控界面

https://www.jianshu.com/p/483adafc6b3a //todo




## spring-boot-starter-security

### security组件介绍

[ref]](https://www.cnblogs.com/demingblog/p/10874753.html)

spring security 是安全认证框架, 基于 Servlet filter, 核心就是一组过滤器链; 可在 Web 请求级和方法调用级处理身份确认和授权 

如果是 普通的 spring 项目, 使用 shiro 更简单



================================= 核心组件

SecurityContext - 用户 auth 通过后, authentication 信息存储在这里, 可以在任意地方使用 `SecurityContextHolder.getContext().set/getAuthentication`

SecurityContextHolder - 在任意地方可以获取 SecurityContext 

Authentication - 认证信息对象, 表示认证后的用户

UserDetails - 认证用户的详细信息

UserDetailsService - 可以用来获取UserDetails。我们会自定义一个CustomUserDetailsService来实现UserDetailsService接口, 通过 username 查询到 userDetails, 如果没查到 , `UsernameNotFoundException`

AuthenticationManager - 作用就是校验Authentication, 校验失败抛出 `BadCredentialsException`


### 基本使用 security

配合 starter , 可以零配置使用. 引入后如下配置即可

```yml
server:
  port: 8000
spring:
  application:
    name: spring-security-demo

  security:
    user:
      name: root
      password: root
```

再次进入任何page需要输入验证 root/root, 如果不配置, 默认为 user/控制台日志中看到默认密码

支持两种验证方式:

- 可以通过 form 表单来认证

    适合页面验证

- 可以通过 HttpBasic 来认证

    适合 api 接口的验证, 当然也适合页面


================================ 实现方法级别的全限控制

`@EnableGlobalMethodSecurity(prePostEnabled =true)` 使用表达式实现方法级别的安全性   ,  4个注解可用

- @PreAuthorize 在方法调用之前,基于表达式的计算结果来限制对方法的访问

- @PostAuthorize 允许方法调用,但是如果表达式计算结果为false,将抛出一个安全性异常

- @PostFilter 允许方法调用,但必须按照表达式来过滤方法的结果

- @PreFilter 允许方法调用,但必须在进入方法之前过滤输入值

```java
//表示如果用户具有admin角色，就能访问listAllUsers方法，但
//是如果方法前不加@preAuthorize注解，意味着所有用户都能访问listAllUsers
@PreAuthorize("hasRole(‘admin‘)")
@RequestMapping(value = "/user/", method = RequestMethod.GET)
@ResponseBody
publicList listAllUsers() {

    ……

}
```

此外还有 :

@EnableGlobalMethodSecurity(securedEnabled=true) 开启@Secured 注解过滤权限

@EnableGlobalMethodSecurity(jsr250Enabled=true)开启@RolesAllowed 注解过滤权限


### 配置跨域

https://lolico.me/2020/04/26/Spring-Security-CORS/
TODO

## oauth2

spring security oauth2


check outline-about-authentication-oauth2.md



# 日志

## Log4j2-Marker 写入到指定日志文件

https://www.jianshu.com/p/1ff824bc997a?utm_campaign


# 数据层

## 启动执行 SQL

### SQL 分隔符的坑

默认分隔符是 分号, 若SQL 中有执行过程/函数, 需要 `spring.datasource.separator=|` 

```sql
drop function if exists `getChildrenProductName`;|
create function `getChildrenProductName`(orgid varchar(50))
returns varchar(4000)
BEGIN
DECLARE `oTemp` VARCHAR(4000);
DECLARE `oTempChild` VARCHAR(4000);
DECLARE `oTempName` VARCHAR(4000);
DECLARE `oTempChildName` VARCHAR(4000);
DECLARE i int;
SET oTemp = CAST(orgid AS CHAR);
SET oTempChild = CAST(orgid AS CHAR);
set oTempName = '$';
set oTempChildName = '';
set i = 0;
WHILE oTempChild IS NOT NULL
DO
if i>0 then
set oTempName = concat(oTempName,",",oTempChildName);
end if;
SET oTemp = CONCAT(oTemp,',',oTempChild);
set i = i +1;
SELECT GROUP_CONCAT(p.product_name,p.partnum) into oTempChildName FROM product p
left join bom b on b.PartID = p.PartID
 WHERE FIND_IN_SET(b.ParentPartID,oTempChild) > 0;
SELECT GROUP_CONCAT(p.PartID) INTO oTempChild FROM product p
left join bom b on b.PartID = p.PartID
 WHERE FIND_IN_SET(b.ParentPartID,oTempChild) > 0;
END WHILE;
RETURN oTempName;
END;|
```

### spring datasource 自带

```yml
spring:
  datasource:
#   数据源基本配置
    username: root
    password: 123
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatis?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=UTC
    type: com.alibaba.druid.pool.DruidDataSource
#   数据源其他配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
#   配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
    schema:
        - classpath:sql/department.sql
        - classpath:sql/employee.sql
    initialization-mode: ALWAYS

```

### 使用 jpa

1、SpringBoot根据脚本初始化

结构初始化脚本文件由spring.datasource.schema属性指定，数据初始化脚本由文件spring.datasource.data属性指定。

这两个脚本是否执行的开关由spring.datasource.initialization-mode决定：always-一定执行，embedded-只对内存数据库执行(默认)，never-不执行。

2、JPA根据类结构初始化

　　@Entity注解的类将会被初始化一张数据库表。

　　是否执行的开关由spring.jpa.hibernate.ddl-auto属性控制，可为create/update/create-drop/none/validate，其中none和validate为不执行。

3、初始化时对于另一个属性spring.jpa.generate-dll在理解上的困惑

　　按照它的描述，spring.jpa.generate-dll是jpa层面对数据表生成策略的控制，而spring.jpa.hibernate.ddl-auto是jpa实现hibernate层面上对数据表生成策略的控制。

　　实践也验证了文章里所说的，spring.jpa.generate-dll比spring.jpa.hibernate.ddl-auto有更强的控制力度，即使spring.jpa.hibernate.ddl-auto为none，只要spring.jpa.generate-dll为true，也会根据@Entity注解的实体类生成对应数据表。

　　按照文章的建议，为了避免混淆和不好理解，这两者最好不要混用，只对JPA实现层面的控制属性spring.jpa.hibernate.ddl-auto进行设置即可。

4、让SpringBoot根据脚本和让JPA根据实体类进行初始化，两者之中选择一个即可。

5 spring.jpa.database-platform=org.hibernate.dialect.MySQL5InnoDBDialect 虽然 不同的数据库方言会自动选择, 但是若想使用特定引擎类型需要指定方言, 不加这句则不会默认创建MyISAM引擎的数据库

```yml

```

### DataSourceInitializer

```java

@Configuration
public class CustomizeDataSourceInitializer {


    @Value("classpath:testSql/test_farms.sql")
    private Resource functionScriptFarms;
    @Value("classpath:testSql/test_miners.sql")
    private Resource functionScriptMiners;
    @Value("classpath:testSql/test_pool_config.sql")
    private Resource functionScriptPoolConfig;

    @Bean
    public DataSourceInitializer dataSourceInitializer(final DataSource dataSource) {
        final DataSourceInitializer initializer = new DataSourceInitializer();
        // 设置数据源
        initializer.setDataSource(dataSource);
        initializer.setDatabasePopulator(databasePopulator());
        return initializer;
    }

    private DatabasePopulator databasePopulator() {
        final ResourceDatabasePopulator populator = new ResourceDatabasePopulator();
        populator.addScripts(functionScriptFarms);
        populator.addScripts(functionScriptMiners);
        populator.addScripts(functionScriptPoolConfig);

        return populator;
    }

}
```

###  maven-antrun-plugin

```xml

<profile> 
    <id>refresh-db</id> 
    <build> 
        <plugins> 
            <plugin> 
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-antrun-plugin</artifactId> 
               <configuration> 
               <target> 
                   <property file="src/main/resources/application.properties" /> 
                   <sql driver="${jdbc.driver}" url="${jdbc.url}" userid="${jdbc.username}" password="${jdbc.password}" onerror="continue" encoding="${project.build.sourceEncoding}">
                       <classpath refid="maven.test.classpath" /> 
                       <transaction src="src/main/resources/sql/h2/schema.sql"/>
                       <transaction src="src/test/resources/data/h2/import-data.sql"/>
                   </sql> 
              </target> 
              </configuration> 
            </plugin> 
        </plugins> 
    </build> 
</profile>
```

## 整合 spring jdbc

https://www.cnblogs.com/liyihua/p/12333967.html

## 整合 hibernate

### 审计 createdDate

https://blog.csdn.net/a972669015/article/details/104778172
TODO

### 执行原生 SQL

https://github.com/cloudfavorites/favorites-web

https://www.cnblogs.com/myknow/p/9559011.html
https://segmentfault.com/a/1190000022338883
https://liuyanzhao.com/8069.html
TODO

### 动态 SQL

https://www.cnblogs.com/kongxianghai/p/7575988.html

插入和修改可以在类实体上标注
@DynamicInsert和@DynamicUpdate
运行时动态生成非空字段的 插入和 更新语句


动态查询则需要用 Criteria API

### spring-data-rest 配合 spring-data-jpa

spring data rest 可以直接将 repository 发布为 rest service, 和 spring data jpa (自动生成 crud api) 配合

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-rest</artifactId>
        </dependency>
```

```java
@RepositoryRestResource // publish repository as rest service
interface ReservationRepository extends JpaRepository<Reservation, Long> {

    /**
     * customized rest service
     *
     * http://localhost:8080/reservations/search/by-name?name=hello
     */
    @RestResource(path = "by-name")
    Optional<Reservation> findByName(String name);
}

```

在分布式下, 在 feign client 接收 spring data rest 发布的 service 需要和 hateoas (spring-boot-starter-hateoas) 配合:([hateoas 版本更新, 接受数据的实体有改变](https://docs.spring.io/spring-hateoas/docs/current/reference/html/#migrate-to-1.0.changes))


```java
@FeignClient("reservation-service") // also support 'url' ( outside eureka)
interface ReservationResources {

    @RequestMapping("/reservations")
    CollectionModel<Reservation> reservations();

    @RequestMapping("/message")
    String message();
}
```



## 整合 mybatis-plus

mybatis-plus-boot-starter

```java
public interface IAccountService extends IService<Account> {
}
@Service
public class AccountService extends ServiceImpl<AccountMapper, Account> implements IAccountService {
}


@mapper
// 或者启动类 @MapperScan("io.github.xiaoyureed.workflow.mapper")
public interface AccountMapper extends BaseMapper<Account> {
}


```

AccountMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org/DTD Mapper 3.0" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="io.github.xiaoyureed.workflow.mapper.AccountMapper">
</mapper>


```

## 整合 mybatis

* 依赖：mybatis-spring-boot-starter
* @Mapper标注创建的xxxMapper接口；或者 在 启动类上 标注 @mapperscan(basepackage...)
* @Select("SELECT * FROM USER WHERE NAME = #{name}")标注方法，还有@insert等；方法参数@Param("name")标注
* 通过@Autowired注入使用

也可以直接引入 mybatisplus-spring-boot-starter 通过 mybatis plus 使用

### 基本配置 

```yml
server:
  port: 8080
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mall?useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    username: root
    password: root
mybatis:
  # config-location属性和configuration属性不能同时指定
  #config-location: classpath:mybatis.xml
  type-aliases-package: me.zingon.pagehelper.model
  mapper-locations: classpath:mapper/*.xml
  #mapper-locations: {"classpath:mapper/*.xml", "classpath*:com/**/mapper/*.xml"}
  configuration:
    map-underscore-to-camel-case: true
    default-fetch-size: 100
    default-statement-timeout: 30

```

### 使用 拦截器 interceptor plugins


#### principle of plugins

基于插件机制，基本上可以控制SQL执行的各个阶段，如执行阶段，参数处理阶段，语法构建阶段，结果集处理阶段，具体可以根据项目业务来实现对应业务逻辑。

设计模式：代理模式、责任链模式


#### use scenarios


- paging 拦截StatementHandler类的prepare方法，改变要执行的SQL语句为分页语句即可；
- Assign default values to fields before SQL execution
- SQL performance monitor 通过拦截Executor类的update, query等方法，用日志记录每个方法执行的时间




#### how to use plugins




实现 Interceptor 接口, 标注@Intercepts(@Signature) , 参考 https://www.jianshu.com/p/0a72bb1f6a21

scenario 1: 直接给自定义拦截器添加一个 @Component注解 , 启动时不会自动调用自定义拦截器的setProperties方法
scenario 2: 无需 @component ,只需要 @bean ConfigurationCustomizer 中 addInterceptor , 不会自动调用setProperties方法 --- mybatis plus  中的配置方法
scenario 3: 无需 @component ,需要 @bean SqlSessionFactoryBean 中 setPlugins --- (app 启动时, setProperties被自动调用)

https://github.com/miaoxinwei/mybatis-crypt/blob/master/src/main/java/org/apache/ibatis/plugin/CryptInterceptor.java 日志脱敏  
TODO




```java
/**
     * 在mybatis中可被拦截的类型有四种(按照拦截顺序):
     * <p>
     * Executor：执行 SQL 语句。
     * ParameterHandler：处理参数
     * ResultHandler：处理结果集。
     * StatementHandler：Sql语法构建。
     *
     * 每个类型可以被拦截的方法:
     *
     拦截的类	拦截的方法
     Executor	update, query, flushStatements, commit, rollback,getTransaction, close, isClosed
     ParameterHandler	getParameterObject, setParameters
     StatementHandler	prepare, parameterize, batch, update, query
     ResultSetHandler	handleResultSets, handleOutputParameters
     */
    //标识该类是一个拦截器；
    @Intercepts({
            //指明自定义拦截器需要拦截哪一个类型，哪一个方法(方法有重载, 所以还需要 指明 args 才能确定是哪个方法)
            @Signature(type = Executor.class, method = "update", args = {MappedStatement.class, Object.class}),
            @Signature(type = Executor.class, method = "query", args = {
                    MappedStatement.class, Object.class, RowBounds.class, ResultHandler.class})
    })
    private static class SqlInterceptor implements Interceptor {

        @Override
        public Object intercept(Invocation invocation) throws Throwable {
//            Object target = invocation.getTarget(); //被代理对象
//            Method method = invocation.getMethod(); //代理方法
//            Object[] args = invocation.getArgs(); //方法参数

            Object[] args = invocation.getArgs();
            Object param = null;
            MappedStatement mappedStatement = (MappedStatement) args[0];
            if (args.length > 1) {
                param = args[1];
            }
            String sql = sql(mappedStatement, param);
            return invocation.proceed();
        }

        private String sql(MappedStatement statement, Object param) {
            BoundSql boundSql = statement.getBoundSql(param);
            List<ParameterMapping> parameterMappings = boundSql.getParameterMappings();
            //将原始sql中的空白字符（\s包括换行符，制表符，空格符）替换为" "
            String sql = boundSql.getSql().replaceAll("[\\s]+", " ");
            if (param == null || parameterMappings.size() == 0) {
                return sql;
            }

            org.apache.ibatis.session.Configuration configuration = statement.getConfiguration();
            TypeHandlerRegistry typeHandlerRegistry = configuration.getTypeHandlerRegistry();
            if (typeHandlerRegistry.hasTypeHandler(param.getClass())) {
                sql = sql.replaceFirst("\\?", getParameterValue(param));
            } else {
                MetaObject metaObject = configuration.newMetaObject(param);
                for (ParameterMapping parameterMapping : parameterMappings) {
                    String propertyName = parameterMapping.getProperty();
                    if (metaObject.hasGetter(propertyName)) {
                        Object obj = metaObject.getValue(propertyName);
                        sql = sql.replaceFirst("\\?", getParameterValue(obj));
                    } else if (boundSql.hasAdditionalParameter(propertyName)) {
                        Object obj = boundSql.getAdditionalParameter(propertyName);
                        sql = sql.replaceFirst("\\?", getParameterValue(obj));
                    }
                }
            }
            return sql;
        }

        private String getParameterValue(Object obj) {
            String value = null;
            if (obj instanceof String) {
                value = "'" + obj.toString() + "'";
            } else if (obj instanceof Date) {
                DateFormat formatter = DateFormat.getDateTimeInstance(DateFormat.DEFAULT, DateFormat.DEFAULT, Locale.CHINA);
                value = "'" + formatter.format(obj) + "'";
            } else {
                if (obj != null) {
                    value = obj.toString();
                } else {
                    value = "";
                }
            }
            return value;
        }

        /**
         * 生成代理对象
         * 可选
         * 我们可以决定是否要进行拦截, 如果拦截, 就返回代理后的对象
         *       * 如果不拦截, 则返回原始对象
         *
         *  每经过一个拦截器对象都会调用插件的plugin方法，也就是说，该方法会调用4次。根据@Intercepts注解来决定是否进行拦截处理
         */
        @Override
        public Object plugin(Object target) {
            //返回四大接口对象的代理对象, 表示始终会执行拦截
            return Plugin.wrap(target, this);

            // 控制是否拦截
            // 若类上加了 @Signature 指明了 拦截类型, 这一步就省了
//            if (target instanceof StatementHandler) {
//                return Plugin.wrap(target, this);
//            }
//            return target;
        }

        /**
         * 可选
         * 用于在 Mybatis 配置文件中指定一些属性的。类似 spring 中有 @value,
         * 一般不用
         */
        @Override
        public void setProperties(Properties properties) {
        }
    }

```

### typehandler

https://blog.csdn.net/m0_37735176/article/details/103107759
https://www.cnblogs.com/haoliyou/p/13743698.html
https://www.cnblogs.com/dyf-stu/p/10162301.html

### 全注解开发

https://zhuanlan.zhihu.com/p/94554180
TODO

### 动态定时刷新 SQL mapper 实现热加载

https://veevv.com/2015/09/30/mybatis-mapper-xml-reloader/

```java
@component
public class XMLMapperReloader implements ApplicationContextAware {

    private ConfigurableApplicationContext context = null;
    private HashMap<String, Long> fileMapping = new HashMap<String, Long>();
    private Scanner scanner = null;
    private String packageSearchPath = "classpath:com/xx/xx/dao/*/*.xml";
    private ScheduledExecutorService service = null;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.context = (ConfigurableApplicationContext) applicationContext;

    }

    public XMLMapperReloader(){
        service = Executors.newScheduledThreadPool(1);
        service.scheduleAtFixedRate(new Task(), 5, 5, TimeUnit.SECONDS);
    }

    class Task implements Runnable {
        @Override
        public void run() {
            try {
                scanner = new Scanner();
                scanner.refreshMapper();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

    }

    @SuppressWarnings({ "rawtypes" })
    class Scanner {
        private Resource[] mapperLocations;

        public void refreshMapper() {
            try {
                SqlSessionFactory factory = context.getBean(SqlSessionFactory.class);
                Configuration configuration = factory.getConfiguration();

                // step.1 扫描文件
                try {
                    this.scanMapperXml();
                } catch (IOException e) {
                    return;
                }

                // step.2 判断是否有文件发生了变化
                if (this.isChanged()) {
                    System.out.println("==============检测到mapper有改变, 开始刷新...===============");
                    // step.2.1 清理
                    this.removeConfig(configuration);

                    // step.2.2 重新加载
                    for (Resource configLocation : mapperLocations) {
                        try {
                            XMLMapperBuilder xmlMapperBuilder = new XMLMapperBuilder(configLocation.getInputStream(), configuration, configLocation.toString(), configuration.getSqlFragments());
                            xmlMapperBuilder.parse();
                        } catch (IOException e) {
                            continue;
                        }
                    }
                    System.out.println("==============mapper刷新完毕===============");
                }


            } catch (Exception e) {
                e.printStackTrace();
            }
        }

        private void scanMapperXml() throws IOException {
            this.mapperLocations = new PathMatchingResourcePatternResolver().getResources(packageSearchPath);
        }

        private void removeConfig(Configuration configuration) throws Exception {
            Class<?> classConfig = configuration.getClass();
            clearMap(classConfig, configuration, "mappedStatements");
            clearMap(classConfig, configuration, "caches");
            clearMap(classConfig, configuration, "resultMaps");
            clearMap(classConfig, configuration, "parameterMaps");
            clearMap(classConfig, configuration, "keyGenerators");
            clearMap(classConfig, configuration, "sqlFragments");

            clearSet(classConfig, configuration, "loadedResources");

        }

        private void clearMap(Class<?> classConfig, Configuration configuration, String fieldName) throws Exception {
            Field field = classConfig.getDeclaredField(fieldName);
            field.setAccessible(true);
            Map mapConfig = (Map) field.get(configuration);
            mapConfig.clear();
        }

        private void clearSet(Class<?> classConfig, Configuration configuration, String fieldName) throws Exception {
            Field field = classConfig.getDeclaredField(fieldName);
            field.setAccessible(true);
            Set setConfig = (Set) field.get(configuration);
            setConfig.clear();
        }

        private boolean isChanged() throws IOException {
            boolean flag = false;
            for (Resource resource : mapperLocations) {
                String resourceName = resource.getFilename();

                boolean addFlag = !fileMapping.containsKey(resourceName);// 此为新增标识

                // 修改文件:判断文件内容是否有变化
                Long compareFrame = fileMapping.get(resourceName);
                long lastFrame = resource.contentLength() + resource.lastModified();
                boolean modifyFlag = null != compareFrame && compareFrame.longValue() != lastFrame;// 此为修改标识

                // 新增或是修改时,存储文件
                if(addFlag || modifyFlag) {
                    fileMapping.put(resourceName, Long.valueOf(lastFrame));// 文件内容帧值
                    flag = true;
                }
            }
            return flag;
        }
    }

    public String getPackageSearchPath() {
        return packageSearchPath;
    }

    public void setPackageSearchPath(String packageSearchPath) {
        this.packageSearchPath = packageSearchPath;
    }
}


```

### 自定义资源文件打包目录 and 开启占位符过滤

mapper.xml 和 dao interface 放在一起: (开启资源文件过滤)

```xml
<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>

		<!-- maven 默认仅仅 将src/main/resources 下的文件打包到 jar
            若 mapper.xml 不在标准目录下, 需要指定资源过滤处理 -->
		<resources>
			<resource>
				<directory>src/main/java</directory>
				<includes>
					<include>**/*.*</include>
				</includes>
                <!-- 打包到哪里? 可省略, 默认 jar 根目录 -->
                <!-- <targetPath>META-INF/plexus</targetPath> -->
                <!-- 是否开启资源文件占位符过滤 (如 ${project.version}), 默认 false -->
                <!-- <filtering>false</filtering> -->
			</resource>
            <!-- 可省略 -->
			<resource>
				<directory>src/main/resources</directory>
				<includes>
					<include>**/*.*</include>
				</includes>
			</resource>
            <!-- 添加额外的配置文件, 用来解析资源文件中的占位符 -->
            <filters>
                <filter>src/main/filters/filter.properties</filter>
            </filters>
		</resources>
	</build>

```



## springboot 事务

### 事务基本使用

Spring boot是默认启动事务的，只需要在类或者方法上添加@Transactional注解即可

不生效, 可能是:

1、首先要看数据库引擎是否支持注解，mysql默认引擎INNODB是支持的，但MYISAM是不支持的；

2、注解只能被应用到public方法上, 其它方法上不会报错，但不生效；

3、默认情况下只会对运行期异常(java.lang.RuntimeException及其子类)和 Error 进行回滚；

4、如果是其它异常, 手动指定`@Transactional(rollbackFor={Exception.class})`

如果还是不生效, 手动回滚: `TransactionAspectSupport.currentTransactionStatus().setRollbackOnly()`


https://blog.csdn.net/laoxilaoxi_/article/details/99896738 使用 cglib or jdk 动态代理 `@EnableTransactionManagement(proxyTargetClass = true)// 使用cglib`

### spring 事务传播失效的坑

同一个 service bean 中的 事务方法互相调用, 如 a() 调用 b(), 只会使用 a() 的 事务, b 的无效, 因为事务本质是基于动态代理, 在同个 bean 中的方法互调, 发生在主动调用方 a() 的代理对象中, 所以只有 a() 的事务生效

如果 a(), b() 处于不同 service bean 中, 则 事务传播生效.

解决:

引入 starter aop, 会引入 aspectJ, @enableAspectJAutoProxy(exposeProxy=true) 开启 aspectJ 并暴露代理对象, 在互调的部分, 使用代理对象互调

```java
public void a() {
    // b()
    // c()
    OrderService thisService = (OrderService) Aopcontext.currentProxy()
    thisService.b()
    thisService.c()
}
```

### Transactional 注解

事务超时

@transaction(timeout=...) unit: s


隔离级别

@transaction(isolation=...)

## 缓存

### springboot-starter-cache

引入 redis starter, cache starter

配置文件中配置类型为 redis

注解:

- @cacheable({"xxxCache"}, sync=true) 缓存数据, 方法上, 指定缓存分区, sync 加锁防止缓存击穿
- @cacheEvic(value="xxx", allEntries=true) 删除缓存, allEntries 表示删除分区 cache 下的全部缓存对
- @cacheput 更新
- caching 组合多个操作  
- @cacheconfig 在类级别共享缓存配置
- @enablecaching 开启缓存

自定义配置

```java
// 自定义缓存 key
// 自定义 json 序列化, 不用默认的 jdk 序列化
// 自定义过期时间, 默认-1 永不过期
```

优势是简单方便,

对于读模式:

- 缓存穿透 (查询一个 null 数据), 配置 cache-null-value=true 缓存 null 数据
- 缓存击穿 (大量并发同时请求一个快过期的key): 
- 雪崩(大量key 同时过期), 配置 time-to-live 过期时间, 

对于写模式: 没有很好的内置解决方法, 可以通过:

- 读写加锁
- canal 中间件

### caffeine

Caffeine是使用Java8对Guava缓存的重写版本，在Spring Boot 2.0中将取代Guava


## 配合 h2 数据库进行开发


### h2 和 spring data jpa 配合

无需任何配置文件

```java
@SpringBootApplication
@EnableDiscoveryClient // compared with @EnableEurekaClient, more comprehensive
public class ReservationServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(ReservationServiceApplication.class, args);
    }

    @Bean
    public ApplicationRunner initData(ReservationRepository reservationRepository) {
        return args -> {
            Stream.of("ehllo", "hello", "llhoe", "jojo", "猕猴桃")
                    .forEach(name -> reservationRepository.save(new Reservation(null, name)));
            reservationRepository.findAll().forEach(System.out::println);
        };
    }

}

@RepositoryRestResource // publish repository as rest service
        // 比如查询所有 /reservations, 查某个 /reservations/1, 搜索 /reservations/search ...
interface ReservationRepository extends JpaRepository<Reservation, Long> {

    /**
     * customized rest service
     *
     * http://localhost:8080/reservations/search/by-name?name=hello
     */
    @RestResource(path = "by-name")
    Optional<Reservation> findByName(String name);
}

@RestController
@RefreshScope
class ConfigRefreshTestController {

    private final String message;

    private final DiscoveryClient discoveryClient;

    public ConfigRefreshTestController(@Value("${message: defaultMsg}") String message,
                                       @Autowired DiscoveryClient discoveryClient) {
        this.message = message;
        this.discoveryClient = discoveryClient;
    }

    @RequestMapping(value = "/message", method = RequestMethod.GET)
    public String readMessage() {
        return this.message;
    }

    @RequestMapping("/eureka-services")
    public ResponseEntity<List<String>> eurekaServices() {
        return new ResponseEntity<>(discoveryClient.getServices(), HttpStatus.OK);
    }
}

/**
 * entity: 预定
 */
@Entity
@Data
@AllArgsConstructor
@NoArgsConstructor // if removed, jpa error may be produced
class Reservation {

    @Id
    @GeneratedValue
    private Long id;

    private String name;
}
```

### 和 mybatis-plus 配合

[h2 和 mybatis-plus 配合](https://mp.baomidou.com/guide/quick-start.html):

```yml
# DataSource Config
spring:
  datasource:
    driver-class-name: org.h2.Driver
    schema: classpath:db/schema-h2.sql
    data: classpath:db/data-h2.sql
    url: jdbc:h2:mem:test
    username: root
    password: test
```

`@MapperScan("mapper package")`

mapper extends BaseMapper


## 多数据源 

### 基本配置 全注解开发

启动类需要排除自动配置@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})

```props
server.port=8335
# 配置第一个数据源
spring.datasource.hikari.db1.jdbc-url=jdbc:mysql://127.0.0.1:3306/erp?useUnicode=true&characterEncoding=utf8&useSSL=true&serverTimezone=GMT%2B8
spring.datasource.hikari.db1.username=root
spring.datasource.hikari.db1.password=153963
spring.datasource.hikari.db1.driver-class-name=com.mysql.cj.jdbc.Driver
# 配置第二个数据源
spring.datasource.hikari.db2.jdbc-url=jdbc:mysql://127.0.0.1:3306/erp2?useUnicode=true&characterEncoding=utf8&useSSL=true&serverTimezone=GMT%2B8
spring.datasource.hikari.db2.username=root
spring.datasource.hikari.db2.password=153963
spring.datasource.hikari.db2.driver-class-name=com.mysql.cj.jdbc.Driver
```

```java
@Configuration
@MapperScan(basePackages = "top.snailclimb.db1.dao", sqlSessionTemplateRef = "db1SqlSessionTemplate")
public class DataSource1Config {

    /**
     * 生成数据源.  @Primary 注解声明为默认数据源
     */
    @Bean(name = "db1DataSource")
    @ConfigurationProperties(prefix = "spring.datasource.hikari.db1")
    @Primary // 声明为默认数据源, 防止 dao 层的类忘记指定 @Qualifier("db1SqlSessionTemplate"), 可以使用默认的
    public DataSource testDataSource() {
        return DataSourceBuilder.create().build();
    }

    /**
     * 创建 SqlSessionFactory
     */
    @Bean(name = "db1SqlSessionFactory")
    @Primary
    public SqlSessionFactory testSqlSessionFactory(@Qualifier("db1DataSource") DataSource dataSource) throws Exception {
        SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
        bean.setDataSource(dataSource);
        // 若有 mapper.xml , 需要指定位置
        // 若是全注解开发, 无需指定
        //  bean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mybatis/mapper/db1/*.xml"));
        return bean.getObject();
    }

    /**
     * 配置事务管理
     */
    @Bean(name = "db1TransactionManager")
    @Primary
    public DataSourceTransactionManager testTransactionManager(@Qualifier("db1DataSource") DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }

    /**
    SqlSessionTemplate是线程安全的，可以被多个DAO所共享使用。
    */
    @Bean(name = "db1SqlSessionTemplate")
    @Primary
    public SqlSessionTemplate testSqlSessionTemplate(@Qualifier("db1SqlSessionFactory") SqlSessionFactory sqlSessionFactory) throws Exception {
        return new SqlSessionTemplate(sqlSessionFactory);
    }

}




@Configuration
@MapperScan(basePackages = "top.snailclimb.db2.dao", sqlSessionTemplateRef = "db2SqlSessionTemplate")
public class DataSource2Config {

    @Bean(name = "db2DataSource")
    @ConfigurationProperties(prefix = "spring.datasource.hikari.db2")
    public DataSource testDataSource() {
        return DataSourceBuilder.create().build();
    }

    @Bean(name = "db2SqlSessionFactory")
    public SqlSessionFactory testSqlSessionFactory(@Qualifier("db2DataSource") DataSource dataSource) throws Exception {
        SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
        bean.setDataSource(dataSource);
        //bean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mybatis/mapper/db2/*.xml"));
        return bean.getObject();
    }

    @Bean(name = "db2TransactionManager")
    public DataSourceTransactionManager testTransactionManager(@Qualifier("db2DataSource") DataSource dataSource) {
        return new DataSourceTransactionManager(dataSource);
    }

    @Bean(name = "db2SqlSessionTemplate")
    public SqlSessionTemplate testSqlSessionTemplate(@Qualifier("db2SqlSessionFactory") SqlSessionFactory sqlSessionFactory) throws Exception {
        return new SqlSessionTemplate(sqlSessionFactory);
    }

}






dao 开发:

@Qualifier("db1SqlSessionTemplate")
public interface UserDao {
    /**
     * 通过名字查询用户信息

    mybatis中#{}和${}符号的区别 
    使用#{}意味着使用的预编译的语句，即 preparedStatement, 替换占位符时, 会为参数添加引号作为 string 传入, 防止注入攻击
    也不是说${}一无是处，比如在MyBatis动态排序时使用order by +${动态参数}，使用${}而不是#{}, 动态参数可以传入不定个数的字段.


     */
    @Select("SELECT * FROM user WHERE name = #{name}")
    User findUserByName(String name);

}
@Qualifier("db2SqlSessionTemplate")
public interface MoneyDao {

    /**
     * 通过id 查看工资详情
     */
    @Select("SELECT * FROM money WHERE id = #{id}")
    Money findMoneyById(@Param("id") int id);
}


```

### AbstractRoutingDataSource 动态数据源切换

https://blog.csdn.net/qq_37502106/article/details/91044952
https://www.cnblogs.com/wyb628/p/7240061.html

## 多数据库类型 databaseIdProvider

https://www.cnblogs.com/valten/p/12083128.html# 配置 MySQL Oracle 共同使用

```java

@Configuration
public class DataSourceConfig {
    @Value("${mybatis.mapper-locations}")
    private String mapperLocations;

    @Primary
    @Bean(name = "dataSource")
    @ConfigurationProperties("spring.datasource.hikari")
    public DataSource dataSource() {
        return DataSourceBuilder.create().build();
    }

    @Bean
    public JdbcTemplate jdbcTemplate() {
        return new JdbcTemplate(dataSource());
    }

    //如果配置了 databaseIdProvider，MyBatis 会加载所有的不带 databaseId 或匹配当前 databaseId 的语句；
    // databaseId 是 mybatis 的内置参数
    //如果带或者不带的语句都有，则不带的会被忽略。新增，修改和删除都有这个属性
    /**
    
    <!-- mybatis动态sql的两个内置参数 , 可以直接在 mapper.xml 中使用
           _parameter:代表整个参数
                单个参数：_parameter就是这个参数
                多个参数：参数会被封装为一个map:_parameter就是代表这个map             
           _databaseId:如果配置了databaseIdProvider标签
                _databaseId 就是代表当前数据库的别名Oracle
      -->
      <!-- List<Employee> getEmpsTestInnerParameter(Employee employee); -->
      <select id="getEmpsTestInnerParameter" resultType="com.hand.mybatis.bean.Employee">
          SELECT * FROM emp 
          <if test="_parameter!=null">      _parameter相当于传入的参数employee,判断employee是否为空，若不为空则执行where条件 
           where ename=#{_parameter.eName}   
          </if>
      </select>
    */
    @Bean
    public DatabaseIdProvider databaseIdProvider() {
        DatabaseIdProvider databaseIdProvider = new VendorDatabaseIdProvider();
        Properties p = new Properties();
        p.setProperty("Oracle", "oracle");
        p.setProperty("MySQL", "mysql");
        p.setProperty("PostgreSQL", "postgresql");
        p.setProperty("DB2", "db2");
        p.setProperty("SQL Server", "sqlserver");
        databaseIdProvider.setProperties(p);
        return databaseIdProvider;
    }

    @Primary
    @Bean
    public SqlSessionFactoryBean sqlSessionFactoryBean(@Qualifier("dataSource") DataSource dataSource) throws Exception {
        SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
        factoryBean.setDataSource(dataSource);
        factoryBean.setDatabaseIdProvider(databaseIdProvider());
        factoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources(mapperLocations));
        return factoryBean;
    }
}


```

```yml
# application.yml
# 切换对应的环境 postgresql mysql
spring:
  profiles:
    active: postgresql

# mybatis配置
mybatis:
  mapper-locations: classpath:mapper/**/*.xml
  type-aliases-package: com.valten.**.model

# showSql 控制台打印sql日志
logging:
  level:
    com:
      valten:
        dao: debug

---
# application-mysql.yml#
# 端口
server:
  port: 8001

# 数据源配置
spring:
  datasource:
    hikari:
      jdbc-url: jdbc:mysql://127.0.0.1:3306/test?&useSSL=false
      driver-class-name: com.mysql.jdbc.Driver
      username: root
      password: 123456

---      
# application-postgresql.yml#
# 端口
server:
  port: 8002

# 数据源配置
spring:
  datasource:
    hikari:
      jdbc-url: jdbc:postgresql://127.0.0.1:5432/test
      driver-class-name: org.postgresql.Driver
      username: root
      password: 123456
        
```

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!--oracle-->
<dependency>
    <groupId>com.oracle</groupId>
    <artifactId>ojdbc6</artifactId>
    <version>11.2.0.3</version>
</dependency>
<!--postgresql-->
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>9.4.1212</version>
</dependency>
<!-- 集成mybatis -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.1</version>
</dependency>
<!--fastjson-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.16</version>
</dependency>
<!-- druid数据库连接池组件 -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.10</version>
</dependency>
```



# web 相关

## springboot 发送 https 客户端 client

https://blog.csdn.net/defonds/article/details/86594441 (https://prasans.info/making-https-call-using-apache-httpclient/)
https://www.cnblogs.com/lfstudy/p/13793625.html
https://www.cnblogs.com/dbei/articles/12746191.html
https://segmentfault.com/a/1190000014456939 使用 resttemplate

## RequestContextHolder

```java
ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder
        .getRequestAttributes();
HttpServletRequest request = attributes.getRequest();

```

## 解决 api 版本共存

https://blog.csdn.net/j903829182/article/details/81836551, https://www.cnblogs.com/huanshilang/p/12097048.html 放在 url
TODO

### RequestMappingHandlerMapping



### request matcher

```java
private RequestMatcher matcherHtml = new AntPathRequestMatcher("/**", "GET");

把ServletRequest转化成HttpServletRequest,强转即可

调用matches方法进行匹配 boolean matches(HttpServletRequest request)

```

### request condition

在 spring mvc 中，通过DispatchServlet接收客户端发起的一个请求之后，会通过 HanderMapping 来获取对应的请求处理器；而 HanderMapping 如何找到可以处理这个请求的处理器呢，这就需要 RequestCondition 来决定了

最终的实现类可能是针对以下情况之一：路径匹配，头部匹配，请求参数匹配，可产生MIME匹配，可消费MIME匹配，请求方法匹配，或者是以上各种情况的匹配条件的一个组合

```java
public interface RequestCondition<T> {
    // 一个http接口上有多个条件规则时，用于合并
    //比如类上指定了@RequestMapping的 url 为 root - 而方法上指定的@RequestMapping的 url 为 method - 那么在获取这个接口的 url 匹配规则时，类上扫描一次，方法上扫描一次，这个时候就需要把这两个合并成一个，表示这个接口匹配root/method
	T combine(T other);

	// 判断是否成功，失败返回 null；否则，则返回匹配成功的条件
	// 举个例子来讲，如果当前请求匹配条件是一个路径匹配条件，包含多个路径匹配模板，
	// 并且其中有些模板和指定请求request匹配，那么返回的新建的请求匹配条件将仅仅
	// 包含和指定请求request匹配的那些路径模板。
	@Nullable
	T getMatchingCondition(HttpServletRequest request);

	// 多个都满足条件时，用来指定具体选择哪一个。
	int compareTo(T other, HttpServletRequest request);

}


```

AbstractRequestCondition 实现了equals,hashCode和toString 通用方法, 还通过protected抽象方法约定了实现类其他的一些内部通用逻辑


具体实现类:

- PatternsRequestCondition	路径匹配条件
- RequestMethodsRequestCondition	请求方法(get post delete..)匹配条件
- ParamsRequestCondition	请求参数匹配条件
- HeadersRequestCondition	头部信息匹配条件
- ConsumesRequestCondition	可消费MIME匹配条件
- ProducesRequestCondition	可生成MIME匹配条件

## 自定义接收参数类型

若果只是一个两个类型需要转换, 直接实现  org.springframework.core.convert.converter.Converter 然后标注 component 即可 (包含连个泛型, 第一个是请求中原始类型如 Integer, 第二个泛型是 希望转换后的类型如 enum),

对 get/post 请求都有效

```java
// 例子
@Component
public class StringToDateConverter implements Converter<String, Date> {

    private SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");

    @Override
    public Date convert(String s) {
        Date parse = null;
        try {
            parse = simpleDateFormat.parse(s);
        } catch (ParseException e) {
            e.printStackTrace();
        }
        return parse;
    }
}


// 实例案例: 枚举转换
@Data
public class QueryRequest {
    private GenderEnum gender;
}
@GetMapping("/get")//gender 只能接收到 MALE 、FEMALE 这样的参数，除此以外，均会报类型不匹配的错误信息，此时是无法处理 0 、1 这样的参数的
public Dict testGet(QueryRequest request) {
    log.info("【get-request】= {}", JSONUtil.toJsonStr(request));
    return Dict.create().set("get-request", request);
}

@PostMapping("/post")
public Dict testPost(@RequestBody QueryRequest request) {
    log.info("【post-request】= {}", JSONUtil.toJsonStr(request));
    return Dict.create().set("post-request", request);
}

// 增加一个转换器即可
@component
public class IntegerCodeToGenderEnumConverter implements Converter<Integer, GenderEnum> {
    private Map<Integer, GenderEnum> enumMap = Maps.newHashMap();
 
    public IntegerCodeToGenderEnumConverter() {
        for (GenderEnum genderEnum : GenderEnum.values()) {
            enumMap.put(genderEnum.getCode(), genderEnum);
        }
    }
 
    @Override
    public GenderEnum convert(Integer source) {
        GenderEnum genderEnum = enumMap.get(source);
        if (ObjectUtil.isNull(genderEnum)) {
            throw new IllegalArgumentException("无法匹配对应的枚举类型");
        }
        return genderEnum;
    }
}
```

当我们的枚举类特别多的时候, 不使用上面直接使用 Converter 的这种方法，我们引入 ConverterFactory 来解决这个问题

```java
//通用枚举接口
public interface BaseEnum {
    /**
     * 获取枚举编码
     *
     * @return 编码
     */
    Integer getCode();
}
@Getter
public enum GenderEnum implements BaseEnum {
    /**
     * 男
     */
    MALE(0),
    /**
     * 女
     */
    FEMALE(1);
 
    /**
     * 性别编码
     */
    private Integer code;
 
    GenderEnum(int code) {
        this.code = code;
    }
 
}

public class IntegerToEnumConverter<T extends BaseEnum> implements Converter<Integer, T> {
    private Map<Integer, T> enumMap = Maps.newHashMap();
 
    public IntegerToEnumConverter(Class<T> enumType) {
        T[] enums = enumType.getEnumConstants();
        for (T e : enums) {
            enumMap.put(e.getCode(), e);
        }
    }
 
    @Override
    public T convert(Integer source) {
        T t = enumMap.get(source);
        if (ObjectUtil.isNull(t)) {
            throw new IllegalArgumentException("无法匹配对应的枚举类型");
        }
        return t;
    }
}
//创建对应的自定义 ConverterFactory 工厂类
public class IntegerCodeToEnumConverterFactory implements ConverterFactory<Integer, BaseEnum> {
    private static final Map<Class, Converter> CONVERTERS = Maps.newHashMap();
 
    /**
     * 获取一个从 Integer 转化为 T 的转换器，T 是一个泛型，有多个实现
     *
     * @param targetType 转换后的类型
     * @return 返回一个转化器
     */
    @Override
    public <T extends BaseEnum> Converter<Integer, T> getConverter(Class<T> targetType) {
        Converter<Integer, T> converter = CONVERTERS.get(targetType);
        if (converter == null) {
            converter = new IntegerToEnumConverter<>(targetType);
            CONVERTERS.put(targetType, converter);
        }
        return converter;
    }
}
// 注册工厂
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
 
    /**
     * 枚举类的转换器工厂 addConverterFactory
     */
    @Override
    public void addFormatters(FormatterRegistry registry) {
        registry.addConverterFactory(new IntegerCodeToEnumConverterFactory());
        registry.addConverterFactory(new StringCodeToEnumConverterFactory());
    }
}
```

## 返回图片

```java
@Controller
@RequestMapping(value = "/image")
public class ImageController {
    @RequestMapping(value = "/get")
    @ResponseBody
    public void getImage(HttpServletResponse response) throws IOException {
        File file = new File("D:/test.jpg");
        FileInputStream inputStream = new FileInputStream(file);
        byte[] bytes = new byte[inputStream.available()];
        
        response.setContentType("image/jpeg");
        OutputStream out = response.getOutputStream();
       out.write(result);
       out.flush();
       //关闭响应输出流
       out.close();
       
    }
}



 
@Controller
@RequestMapping(value = "/image")
public class ImageController {
    @RequestMapping(value = "/get",produces = MediaType.IMAGE_JPEG_VALUE) //如果是网页的话，produces = MediaType.TEXT_HTML
    @ResponseBody
    public byte[] getImage() throws IOException {
        File file = new File("D:/test.jpg");
        FileInputStream inputStream = new FileInputStream(file);
        byte[] bytes = new byte[inputStream.available()];
        inputStream.read(bytes, 0, inputStream.available());
        return bytes;
    }
}
```

## rest api 文档

knife4j
swagger2



## 过滤器 和 拦截器

### 区别

servlet filter和spring mvc Interceptor区别：

- Filter 是基于 函数回调的，而 Interceptor 则是基于 Java 反射 和 动态代理。

- Filter 依赖于 Servlet 容器, 和servlet容器隔的近，而 Interceptor 依赖springmvc, 和应用隔的近, 在最里层。所以filter 先进入, 最后退出

- Filter 对几乎 所有的请求 起作用，而 Interceptor 只对 Controller 请求起作用。

- 拦截器可以获取IOC容器中的各个bean，而过滤器就不行，这点很重要，在拦截器里注入一个service，可以调用业务逻辑。






### 使用 filter

```java
scenario 1:
直接实现Filter接口，并使用@Component, 拦截所有路径 "/*", 这种方式可以注入 bean


scenario 2:
实现Filter接口，用@WebFilter注解，指定拦截路径以及一些参数，同时需要在启动类使用@ServletComponentScan扫描带@WebFilter、@WebServlet、@WebListener, 
这种方式无法注入 bean, 除非 和 @component 一起使用, 但是需要配置覆盖 bean
#启用覆盖同名bean
spring.main.allow-bean-definition-overriding=true

scenario 3:
@Configuration
public class MyFilterConfig {
    @Autowired
    MyFilter myFilter;
    @Bean
    public FilterRegistrationBean<MyFilter> thirdFilter() { // 泛型指定 filter 类型, so 多个 filter 每个都要一个 bean 方法
        FilterRegistrationBean<MyFilter> filterRegistrationBean = new FilterRegistrationBean<>();
        filterRegistrationBean.setOrder(2)
        filterRegistrationBean.setFilter(myFilter);

        filterRegistrationBean.setUrlPatterns(new ArrayList<>(Arrays.asList("/api/*")));

        return filterRegistrationBean;
    }
}


和 这里的 filter 不同, 这里是为了过滤 扫描
@ComponentScan(value="com.wxj",excludeFilters= {
        @Filter(type=FilterType.ANNOTATION,classes= {Controller.class})
})
```

### 使用 interceptor

#### 基本使用拦截器

https://www.baeldung.com/spring-mvc-handlerinterceptor


```java
实现 HandlerInterceptor 接口,
@component
class InterfaceAuthCheckInterceptor implements HandlerInterceptor {

        private Logger logger = LoggerFactory.getLogger(getClass());

        @Autowired
        StringRedisTemplate stringRedisTemplate;

        ...


  // 配置      
@Configuration
public class InterceptorConfig implements WebMvcConfigurer { // WebMvcConfigurerAdapter 过时, WebMvcConfigurationSupport, 对于 classpath:/META/resources/，classpath:/resources/，classpath:/static/，classpath:/public/）不生效, 所以选择WebMvcConfigurer 最好

    @autowire
    .... interceptor

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        // 多个拦截器组成一个拦截器链, 加入的顺序就是拦截器执行的顺序, 也可使用 @order(-1/0/1...)指定
        // addPathPatterns 用于添加拦截规则, 默认拦截所有 即 /**
        // excludePathPatterns 用户排除拦截
        registry.addInterceptor(interceptor).addPathPatterns("/api/**").exclude("...");
        
    }

```

#### 基于 url 拦截

一种方式是 在配置中 registry.addInterceptor(interceptor).addPathPatterns("/api/**").exclude("...");

一种是直接在 prehandle() 中处理 String path = request.getServletPath(), path.matches(Const.NO_INTERCEPTOR_PATH 就是基于正则匹配的url 


#### 基于注解拦截

interceptor 也属于 aop, 

```java
public class AuthorityInterceptor extends HandlerInterceptorAdapter{
	
	 @Override
	 public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
	 	// 如果不是映射到方法直接通过
        if (!(handler instanceof HandlerMethod)) {
            return true;
        }
        // ①:START 方法注解级拦截器
        HandlerMethod handlerMethod = (HandlerMethod) handler;
        Method method = handlerMethod.getMethod();
        // 判断接口是否需要登录
        LoginRequired methodAnnotation = method.getAnnotation(LoginRequired.class);
        // 有 @LoginRequired 注解，需要认证
        if (methodAnnotation != null) {
            // 这写你拦截需要干的事儿，比如取缓存，SESSION，权限判断等
            System.out.println("====================================");
            return true;
        }
        return true;
	}
}

```

## servlet Druid 监控

```java
@Configuration
public class DruidConfig {

    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druid(){
        return  new DruidDataSource();
    }

    //配置Druid的监控
    //1、配置一个管理后台的Servlet
    @Bean
    public ServletRegistrationBean statViewServlet(){
        ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");
        Map<String,String> initParams = new HashMap<>();

        initParams.put("loginUsername","admin");
        initParams.put("loginPassword","123456");
        initParams.put("allow","");//默认就是允许所有访问
        initParams.put("deny","192.168.15.21");

        bean.setInitParameters(initParams);
        return bean;
    }


    //2、配置一个web监控的filter
    @Bean
    public FilterRegistrationBean webStatFilter(){
        FilterRegistrationBean bean = new FilterRegistrationBean();
        bean.setFilter(new WebStatFilter());

        Map<String,String> initParams = new HashMap<>();
        initParams.put("exclusions","*.js,*.css,/druid/*");

        bean.setInitParameters(initParams);

        bean.setUrlPatterns(Arrays.asList("/*"));

        return  bean;
    }
}

```

## aop 整合使用

### 基本使用

通知执行顺序: around > before > around > after > afterReturning


```java
// 只需要spring-boot-starter-aop
//@EnableAspectJAutoProxy// 无需, 自动开启,引入依赖后

@Component
@Aspect
public class ReqCheckAop {

    @Autowired
    private ObjectMapper objectMapper;

    @Pointcut("execution(org.springframework.http.ResponseEntity<Resp> io.github.xiaoyureed.demo..*.*(..))")
    private void pointcut() {}

    @Around("pointcut()")
    public Object reqCheck(ProceedingJoinPoint jp) throws Throwable {
        Object[] args = jp.getArgs();
        System.out.println(">>> req = " + objectMapper.writeValueAsString(args[0]));
        return jp.proceed();
    }
}
```

### 切入点表达式

用来指定增强哪些方法

https://www.cnblogs.com/ityangshuai/p/11923696.html

```
1.execution(xxx)  用于确定方法
    表达式格式(就是方法签名)：修饰符  返回值  包.类.方法(参数列表) throws 异常
        1.1 修饰符 （可选），一般不使用 ----------- 接空格
            public        公共修饰符
            *            任意修饰符
        1.2 返回值， 复杂类型需要全限定名称，常用 ----------- 接空格
            String        返回字符串
            *            返回任意
        1.3 包    ，常用
            com.xiaoyu.spring.learn.service            指定service包
            com.xiaoyu.spring.learn.crm.*.service                crm项目下的任意子模块（子文件夹）的service包
            com.xiaoyu.spring.learn.service..            service目录包含的所有目录（包括次级子目录等等）
            com.xiaoyu.spring.learn.crm.*.service..                crm下所有的子模块中service目录包含的所有目录
        1.4 类，常用
            UserService                指定类名
            User*                    以User开头类
            *Service                    以Service结尾类（常用）
            *                        任意类
        1.5 方法，常用 与 1.4类相似
            addUser                    指定方法
            add*                        add开头方法
            *User                    以User结尾方法
            *                        任意方法
        1.6 参数列表，常用
            ()                        没有参数
            (int)                        一个参数
            (int,int)                    两个参数
            (..)                        参数任意
        1.7 异常 ，具体异常（可选）

    多表达式：
        <aop:pointcut experssion="exection(表达式1)  || exection(表达式2) ">    ：表达式1或 表达式2
    综合案例：execution(* com.xiaoyu.spring.learn.crm.*.service..*.*(..))
        含义：    返回值任意，crm项目下的任意子目录中的的service目录下的所有目录中的任意类的任意方法，参数任意。

- within(): 只能指定 包.类; 再下面的粒度就没办法了, 所以相当于 execution() 的简化版
    within(com.xiaoyu.spring.learn.aop..*)匹配aop 包/子包下的所有类的所有方法

    指定注解类亦可
    within(@org.springframework.web.bind.annotation.RestController *) 匹配 被 指定注解标注的所有类的所有方法
    
- this(): 用于向通知方法中传入代理对象的引用
    如:
    @Before("before() && this(proxy)")
    public void beforeAdvide(JoinPoint point, Object proxy){

- target(): 用于向通知方法中传入目标对象的引用

- args() ：用于将参数传入到通知方法中
    如:
    @Before("before() && args(age,username)")
    public void beforeAdvide(JoinPoint point, int age, String username){
    如 
    @Pointcut("execution(public * com.example.demo.aop.AopController.xxxMethod(int)) && args(point))") 会将拦截到的方法的 int 类型参数 传递到 aop 方法中, 使用 point 接受即可


-------


- @within ：用于匹配在类一级使用了注解的类，其所有方法都将被匹配
    @Pointcut("@within(com.cjm.annotation.AdviceAnnotation)") － 所有被@AdviceAnnotation标注的类都将匹配
    常用:
    @Pointcut("@annotation(AopLog) || @within(AopLog)") - 被 aopLog 标注的类 or 方法被拦截

- @target ：和@within的功能类似，但必须要指定注解的保留策略为RUNTIME。

    存疑?@target() 指定目标类型, 所有这个类型的类都会被匹配
    @target(com.xiaoyu.spring.learn.aop.user.IUserDAO)匹配 IUserDAO 的所有实现类, 类中的方法都会被代理, 包括未在接口中声明的方法

- @args() 指定一个注解, 表示要拦截方法的参数需要带有这个注解


- @bean(id值) 对指定的目标对象的类进行匹配

- @annotation(参数中注解参数名称) 匹配被指定注解标注的方法

    如: 下面表示 -> 匹配 web 包/子包下所有类的所有方法 且方法被 @CheckReqNo 标注 
    
    @Around("within(io.github.xiaoyureed.web..*) && @annotation(checkReqNo)")
    public void xxx(JoinPoint jp, CheckReqNo checkReqNo)

-  可以使用 &&, ||, ! 运算符来定义切点表达式
```


## @controllerAdvice 异常处理 

全局异常处理 全局数据绑定 全局请求数据预处理


```java
@ControllerAdvice //@ControllerAdvice注解 可以通过 assignableTypes 指定特定的类，让异常处理类只处理特定类抛出的异常
public class MyGlobalExceptionHandler {
    // 直接返回 web 页面
    @ExceptionHandler(Exception.class)
    public ModelAndView customException(Exception e) {
        ModelAndView mv = new ModelAndView();
        mv.addObject("message", e.getMessage());
        mv.setViewName("myerror");
        return mv;
    }

    // 返回 json
    @exception..
    @respbody // 如果类上使用的是@RestControllerAdvice, 那么 返回 json 无需 @ResponseBody, 只需要 @ExceptionHandler
    public respseentiyt xxx(Exception ex) {}


    // 绑定 md 到每个 controler 中的方法参数中
    @ModelAttribute(name = "md")
    public Map<String,Object> mydata() {
        HashMap<String, Object> map = new HashMap<>();
        map.put("age", 99);
        map.put("gender", "男");
        return map;
    }
    // controller 中这么使用
    @GetMapping("/hello")
    public String hello(Model model) {// 或者 @ModelAttribute("md") Map<String, Object> md
        Map<String, Object> map = model.asMap();
        System.out.println(map);
        int i = 1 / 0;
        return "hello controller advice";
    }


    // 请求数据预处理
    // 如此, 请求参数就可以传递这样的参数: a.name=xxx, b.name=yyy
    // controller 中这样获取 @ModelAttribute("b") Book book, @ModelAttribute("a") Author author
    @InitBinder("b")
    public void b(WebDataBinder binder) {
        binder.setFieldDefaultPrefix("b.");
    }
    @InitBinder("a")
    public void a(WebDataBinder binder) {
        binder.setFieldDefaultPrefix("a.");
    }
}

```

处理异常的其他方法:

```java
@ExceptionHandler(value = Exception.class)// 拦截所有异常
    public ResponseEntity<ErrorResponse> exceptionHandler(Exception e) {

        if (e instanceof IllegalArgumentException) {
            return ResponseEntity.status(400).body(illegalArgumentResponse);
        } else if (e instanceof ResourceNotFoundException) {
            return ResponseEntity.status(404).body(resourseNotFoundResponse);
        }
        return null;
    }



@GetMapping("/resourceNotFoundException2")
    public void throwException3() {
        throw new ResponseStatusException(HttpStatus.NOT_FOUND, "Sorry, the resourse not found!", new ResourceNotFoundException());
    }

```

## HttpServletRequest 的输入流只能读取一次的问题

https://www.cnblogs.com/JAYIT/p/10943155.html

还是推荐使用 aop 去切 controller 更方便一点

## 路由处理

### 静态资源映射 暴露静态资源

暴露图片资源

```java
@Configuration
public class ImageConfig implements WebMvcConfigure {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/images/**") //addResourceLocations指的是文件放置的目录，addResoureHandler指的是对外暴露的访问路径
                .addResourceLocations("file:./localdata/images/");
    }
}

```

分环境暴露配置文件

```java

// 配置文件访问地址 (注意每个访问路径后面的路径加 / !!!)
// ## windows系统下访问路径
// uploadWindow: E:\common\file\root\
// ## windows系统下访问路径
// uploadLinux: /user/img/
@Override
protected void addResourceHandlers(ResourceHandlerRegistry registry) {
    String os = System.getProperty("os.name");

    //如果是Windows系统
    if (os.toLowerCase().startsWith("win")) {
        registry.addResourceHandler("/app_file/**")
                // /app_file/**表示在磁盘filePathWindow目录下的所有资源会被解析为以下的路径
                .addResourceLocations("file:" + filePathWindow);
    } else {  //linux 和mac
        registry.addResourceHandler("/app_file/**")
                .addResourceLocations("file:" + filePathLinux) ;
    }

    super.addResourceHandlers(registry);
}
```

添加 swagger 静态资源处理

一般我们会设置 mvc 中, spring.resources.add-mappings=false, 对静态资源默认不处理, 这是需要为 swagger 添加例外 (还会设置 spring.mvc.throw-exception-if-no-handler-found=true 没找到 Handler时直接抛出异常)

```java
@Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/swagger-ui.html")
                .addResourceLocations("classpath:/META-INF/resources/", "/static", "/public");
 
        registry.addResourceHandler("/webjars/**")
                .addResourceLocations("classpath:/META-INF/resources/webjars/");
    }
```

### 添加 view controller

```java
@Configuration
class MvcConfig implements WebMvcConfigurer {
    /**
     * 添加 view controller
     * <p>
     * 将 url path 和 template 对应, 无需添加任何的 controller 代码
     */
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/home").setViewName("home");// 等价 访问 /home , controller 返回 "home"
        registry.addViewController("/").setViewName("home");
        registry.addViewController("/hello").setViewName("hello");
        registry.addViewController("/login").setViewName("login");
        registry.addViewController("/admin-page").setViewName("admin-page");
    }
}
```



## spring-boot-starter-thymeleaf

thymeleaf 和 spring security 配合使用 `sec` 标签: https://www.jianshu.com/p/bb3a3b388c4e


springboot默认  static/public 中放静态页面，而templates中放动态页面即 thymeleaf 模板

静态页面: 直接在static放一个hello.html,然后直接输入http://localhost:8080/hello.html 便能成功访问 (关闭这个特性需要 spring.resources.add-mappings: false), 也可以通过controller跳转 `return "hello.html";`

动态页面: 需要 `spring-boot-starter-thymeleaf`, 静态页面的return默认是跳转到/static/index.html，当在pom.xml中引入了thymeleaf组件，动态跳转会覆盖默认的静态跳转，默认就会跳转到/templates/index.html，注意看两者return代码也有区别，动态没有html后缀, 如果在使用动态页面时还想跳转到/static/index.html，可以使用重定向return "redirect:/index.html"

```java
@RequestMapping("/Hi")
    public String sayHello() {
        return "hello";
    }

@RequestMapping("/Hi")
public ModelAndView sayHello() {
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.setViewName("hello");
    modelAndView.addObject("key", 12345);
    //System.out.println("test");
    return modelAndView;
}

@RequestMapping("/Hi")
public String sayHello(Map<String,obj> map) {
    map.put("stu", new Stu())
    return "hello";
}
```

配置:

```yml

#spring配置
spring:
  mvc:
    servlet:
      load-on-startup: 1
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/springbootssm?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf-8
    username: root
    password: 123456
  thymeleaf:
    cache: false #关闭缓存
    mode: HTML5 #设置模板类型
    encoding: utf-8  #设置编码
    prefix: classpath:/templates/
    suffix: .html
    content-type=text/html 
```


# 运维部署

## 支持 https ssl 


### nginx 配置 https

Let's Encrypt 或者阿里云, 腾讯云 支持 面粉 ssl 证书 , 一年有效期, 过期再次申请 


https://segmentfault.com/a/1190000016760251 使用 lets encrypt 配置 nginx

https://www.cnblogs.com/to-red/p/12450616.html
https://www.cnblogs.com/chnmig/p/10343890.html
https://blog.csdn.net/fenglin0429/article/details/81347634
https://www.nonelonely.com/article/1584965619600 nginx 配置 http  转https, 代理 springboot
https://ask.csdn.net/questions/701009

### 支持 https 访问


```java
// 假设如下的 api, 希望支持 https 访问, 现在仅仅只是http访问。http://localhost:8080
@RestController
@SpringBootApplication
public class TestApplication {

    public static void main(String[] args) {
        SpringApplication.run(TestApplication.class, args);
    }
    @RequestMapping(value = "/",produces = "text/plain;charset=UTF-8")
    String index(){
        return "Hello Spring Boot!";
    }
}


//生成ssl证书, 系统的当前用户目录下会生成一个keystore.p12文件, 将keystore.p12文件拷贝到我们项目的根路径下
// 1.-storetype 指定密钥仓库类型
// 2.-keyalg 生证书的算法名称，RSA是一种非对称加密算法
// 3.-keysize 证书大小
// 4.-keystore 生成的证书文件的存储路径
// 5.-validity 证书的有效期
keytool -genkey -alias tomcat  -storetype PKCS12 -keyalg RSA -keysize 2048  -keystore keystore.p12 -validity 3650

// 修改配置文件
server.ssl.key-store=keystore.p12 //路径
server.ssl.key-store-password=123456 //生成ssl证书时设置的密钥库的口令
server.ssl.keyStoreType=PKCS12
server.ssl.keyAlias = tomcat

//此时工程已经支持https访问。 https://localhost:8080/

```

### http 请求自动转为 https

```java
// 配置访问http  urL 时自动转到https。
// 配置后, 访问 https://localhost:8081/ 正常, 访问 http://localhost:8080/，我们可以看到页面的url变成了https://localhost:8081/
class Config {
    @Bean
    public EmbeddedServletContainerFactory servletContainer() {
        TomcatEmbeddedServletContainerFactory tomcat = new TomcatEmbeddedServletContainerFactory() {
            @Override
            protected void postProcessContext(Context context) {
                SecurityConstraint constraint = new SecurityConstraint();
                constraint.setUserConstraint("CONFIDENTIAL");
                SecurityCollection collection = new SecurityCollection();
                collection.addPattern("/*");
                constraint.addCollection(collection);
                context.addConstraint(constraint);
            }
        };
        tomcat.addAdditionalTomcatConnectors(httpConnector());
        return tomcat;
    }

    @Bean
    public Connector httpConnector() {
        Connector connector = new Connector("org.apache.coyote.http11.Http11NioProtocol");
        connector.setScheme("http");
        //Connector监听的http的端口号
        connector.setPort(8080);
        connector.setSecure(false);
        //监听到http的端口号后转向到的https的端口号
        connector.setRedirectPort(8081); // 需要设置 server.port=8081

        return connector;
    }
}



```

### 同时支持 http https


用http请求时不再转换成https请求

只需要将代码中的connector.setSecure(false)中的false改成true就可以了


## 容器化部署

https://rovo98.github.io/posts/39885a36/ SpringBoot + Docker + Nginx 负载均衡实现
TODO

## nginx 反向代理

https://cloud.tencent.com/developer/article/1332603

## 内嵌 tomcat 调优

```yml
server:
  compression:
    #SpringBoot 默认是不开启 gzip 压缩的，需要我们手动开启
    enabled: true
    #optional, 在 spring2.0+的版本中，默认值已经有了
    mime-types: application/json,application/xml,text/html,text/plain,text/css,application/x-javascript
    #默认情况下，仅会压缩 2048 字节以上的内容, 指定超过多少就压缩
    min-response-size: 1024

  tomcat:
    # 排队数, 等待队列长度：队列做缓冲池用，但也不能无限长，消耗内存，出入队列也耗cpu
    accept-count: 1000   
    max-connections: 10000 #最大可被连接数，默认为10000 
    #最大工作线程数, 默认 200
    # 4核8G内存单进程调度线程数800-1000，超过这个并发数之后，将会花费巨大的时间在cpu调度上。
    max-threads: 800  
    min-spare-threads: 100 #最小工作线程数, 默认 10
    # 连接超时 30s
    connection-timeout: 30000
    # Maximum size, in bytes, of the HTTP message header
    max-http-header-size:
    # Maximum size, in bytes, of the HTTP post content.
    max-http-post-size:
```


通过 java code 配置:

修改keepalivetimeout和maxKeepAliveRequests开启长连接

```java
//当Spring容器内没有TomcatEmbeddedServletContainerFactory这个bean时，会吧此bean加载进spring容器中
@Component
public class  WebServerConfiguration implements WebServerFactoryCustomizer<ConfigurableWebServerFactory> {
    @Override
    public void customize(ConfigurableWebServerFactory configurableWebServerFactory) {
            //使用对应工厂类提供给我们的接口定制化我们的tomcat connector
        ((TomcatServletWebServerFactory)configurableWebServerFactory).addConnectorCustomizers(new TomcatConnectorCustomizer() {
            @Override
            public void customize(Connector connector) {
                Http11NioProtocol protocol = (Http11NioProtocol) connector.getProtocolHandler();

                //定制化keepalivetimeout,设置30秒内没有请求则服务端自动断开keepalive链接
                protocol.setKeepAliveTimeout(30000);
                //当客户端发送超过10000个请求则自动断开keepalive链接
                protocol.setMaxKeepAliveRequests(10000);

                // 设置其他....
            }
        });
    }
}

// 或者这样配置, 效果等同   
public class WebServerConfiguration {
    @Bean
    public ConfigurableServletWebServerFactory webServerFactory() {
        TomcatServletWebServerFactory factory = new TomcatServletWebServerFactory();
        factory.setPort(11359);//端口号
        factory.setUriEncoding(Charset.forName("utf-8"));//编码
        factory.addConnectorCustomizers(new MyTomcatConnectorCustomizer());
        return factory;
    }
    
    class MyTomcatConnectorCustomizer implements TomcatConnectorCustomizer {
        @Override
        public void customize(Connector connector) {
            // TODO Auto-generated method stub
            Http11NioProtocol handler = (Http11NioProtocol)connector.getProtocolHandler();
            
            handler.setAcceptCount(2000);//排队数
                        
            handler.setMaxConnections(5000);//最大连接数
            
            handler.setMaxThreads(2000);//线程池的最大线程数
            
            handler.setMinSpareThreads(100);//最小线程数
                        
            handler.setConnectionTimeout(30000);//超时时间                    
        }
    }
}
```



## 配置 war 包部署到 Tomcat

```java
public class ServletInitializer extends SpringBootServletInitializer {
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(Application.class);
    }

}

@Configuration
@ComponentScan(basePackageClasses = Application.class, useDefaultFilters = true)
public class ServletContextConfig extends WebMvcConfigurationSupport{
    /**
     * 配置servlet处理
     */
    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }
}

```

或者直接一步到位:


```java
@SpringbootApplication
public class Application extends SpringBootServletInitializer {
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(Application.class);
    }

    ...
}
```

# spring boot 中的并发

https://www.e4developer.com/2018/03/30/introduction-to-concurrency-in-spring-boot/ //todo
https://blog.csdn.net/qq_40606397/article/details/104945855


# 实现乐观锁悲观锁

https://www.cnblogs.com/cloudfloating/p/11461530.html 

TODO




# 移动端消息推送

https://juejin.im/post/6844903838495801357 
https://crossoverjie.top/2018/09/25/netty/million-sms-push/
https://juejin.im/post/6844903478200909837
TODO

# 即时通信 IM系统

https://github.com/tywo45

# 辅助工具

## lombok notice

- @data <=> @Setter、@Getter、@RequiredArgsConstructor、@ToString、@EqualsAndHashCode, 

    其中 @RequiredArgsConstructor 没什么用, 不会生成构造方法, 默认无参构造方法仍然有效

    若存在继承, 需要为 son 指定 @EqualsAndHashCode(callSuper = true)

    @RequiredArgsConstructor(staticName="of") 单独使用, 表示为必须field 生成 constructor, 也就是为 final 的成员或者@notnull成员生成 constructor, 其他field 不会作为参数, staticname=of 表示这个类不能用构造方法构造出来，必须通过 ThisClass.of(...)方法才能构造出来

- 通用的组合是 @data, @all... , @no...

- 和 Java8 配合良好, jdk升级很麻烦, 需要 delombok

- @builder 构造对象时底层用的是全参数构造器, 需要 @AllArgsConstructor一起用, 无法单独使用, 若果一定要用, 好的组合是 @NoArgsConstructor @AllArgsConstructor @Builder @Data, 但是这太复杂

- 代替@AutoWired注解: @RequiredArgsConstructor(onConstructor =@__(@Autowired))标注在类上




## devtools

开发者组件, 修改了 thymeleaf 后, 重新编译修改的文件, 页面即可更新, 不必重启整个 app, 但是若修改了 Java config , 还是要重启

```props
spring.thymeleaf.cache=true
spring.devtools.restart.enabled=true
... additional-paths=src/main/java // 监听目录  
```

## maven wrapper

wrapper：maven的包装器，通过mvnw命令替代mvn，简化mvn操作, 不需要在操作系统上安装Maven

受到 gradle wrappe 启发

/.mvn/wrapper/maven-wrapper.jar 是其主体程序

/.mvn/wrapper/maven-wrapper.properties 可指定 maven 版本及下载路径

mvnw, mvnw.cmd 命令行程序

mvnw第一次运行会检测$USER_HOME/.m2/wrapper/dists 目录下是否有maven-wrapper.properties中指定的Maven版本，如果没有就自动下载



# 校验

## 校验配置文件

任何添加@ConfigurationProperties的类，class上加 `@Validated` annotation, app都会进行属性校验, 确保引入了 JSR-303 javax.validation 规范的实现;
使用校验规范JSR-303 javax.validation中的注解即可实现校验（介绍见这里https://www.ibm.com/developerworks/cn/java/j-lo-jsr303/）

```java
@ConfigurationProperties(prefix="connection")
@Validated
public class FooProperties {

    @NotNull
    private InetAddress remoteAddress;

    @Valid // 这里是为了激活 Security类中的注解校验
    private final Security security = new Security();

    // ... getters and setters

    public static class Security {

        @NotEmpty
        public String username;

        // ... getters and setters

    }

}
```

@Validated 也可以可以加到 @Bean 方法上 来触发校验;

也可以使用自定的Spring Validator，bean id是 configurationPropertiesValidator 即可

spring-boot-actuator 模块有一个端点，对外暴露了所有的 @ConfigurationProperties beans。浏览器中访问 /configprops 即可

## 校验请求参数

https://blog.csdn.net/mzh_cn/article/details/80637015

## 自定义校验

## @Validated和@Valid

@Valid是 JSR-303 规范, hibernate-validation实现了这一规范

@Validated来自于spring, 是 spring 对于 @Valid 的实现, 提供"validation groups"功能(创建空接口作为分组), valid则不具备;(https://stackoverflow.com/questions/36173332/difference-between-valid-and-validated-in-spring) , 一旦controler 中使用 @Validated(group={...}), 那么实体类字段上的注解若指定 group 则约束生效, 不指定分组的不生效. 反之, 若 @validated 没有指定分组, 则 实体类字段指定了 group 的不生效, 不指定group 的字段生效






# runner 获取命令行参数

## 获取命令行参数

注入一个`org.springframework.boot.ApplicationArguments `bean

```java
@Component
public class MyBean {

    @Autowired
    public MyBean(ApplicationArguments args) {
        boolean debug = args.containsOption("debug");//option参数即“--”开头的参数
        List<String> files = args.getNonOptionArgs();// if run with "--debug logfile.txt" 那么结果是 debug=true, files=["logfile.txt"]
    }

}
```

另外的注入方法:

Spring Boot will also register a `CommandLinePropertySource` with the Spring `Environment`. This allows you to also inject single application arguments using the `@Value` annotation.

## `ApplicationRunner` or `CommandLineRunner`

`ApplicationRunner` or `CommandLineRunner`接口都有run()待实现(在SpringApplication.run(…​)完成之前调用)但是参数不同, 具体如下:

* ApplicationRunner接口, `void run(ApplicationArguments args)`

* CommandLineRunner接口, `void run(String... args)`

```java
@Component
public class MyBean implements CommandLineRunner {

    @Override
    public void run(String... args) {//可以获取命令行参数, 作为String[]
        // 这里的内容会在app完全启动前最后一步执行, 即SpringApplication.run(…​)完成之前, 也会在ApplicationReadyEvent之前
    }

}


// 另一种方法:
//java -jar commandline-app-0.0.1-SNAPSHOT.jar iamnonoption --app.name=CmdRulez --app.hosts=abc,def,ghi --app.name=2
implements ApplicationRunner
    @Override
    public void run(ApplicationArguments args) throws Exception {
        // 获取非 --xxx=yyy 的参数集合
        System.out.println("# NonOptionArgs: " + args.getNonOptionArgs().size());
        System.out.println("NonOptionArgs:");
        args.getNonOptionArgs().forEach(System.out::println);

        // 获取 --xxx=yy 的参数中的 key
        System.out.println("# OptionArgs: " + args.getOptionNames().size());
        System.out.println("OptionArgs:");

        args.getOptionNames().forEach(optionName -> {
            System.out.println(optionName + "=" + args.getOptionValues(optionName));// 获取 value
        });
    }
```

如果有多个CommandLineRunner or ApplicationRunner beans 被定义, 这时候涉及到调用数序问题, 两种方式:

* 实现`org.springframework.core.Ordered`接口

* 使用`org.springframework.core.annotation.Order`注解 eg: @Order(num), num为Int, 0优先级最高

# 开发命令行应用

加入开发一个工具 ab.jar, 希望这么使用:

- java -jar ab.jar 使用提示
- java -jar ab.jar  --spring.profiles.active=hello-world,receiver 激活不同的 profile

配置中指定一个默认 激活 profile

```
spring:
  profiles:
    active: usage_message
```

```java
// 工具使用提示
@Profile("usage_message")
@Bean
public CommandLineRunner usage() {
    return args -> {
        System.out.println("This app uses Spring Profiles to
            control its behavior.\n");
        System.out.println("Sample usage: java -jar
            rabbit-tutorials.jar
            --spring.profiles.active=hello-world,sender");
    };
}

// 若出传递了参数, 则 usage_message 就被覆盖, 这个 bean 被注册
@Profile("!usage_message")
@Bean
public CommandLineRunner tutorial() {
    return new RabbitAmqpTutorialsRunner();
}

// 传递不同的参数, 不同的 profile 会激活
@Profile("receiver")
@Bean
public Tut1Receiver receiver() {
    return new Tut1Receiver();
}

@Profile("sender")
@Bean
public Tut1Sender sender() {
    return new Tut1Sender();
}


// 具体操作
public class RabbitAmqpTutorialsRunner implements CommandLineRunner {

    @Value("${tutorial.client.duration:0}")
    private int duration;

    @Autowired
    private ConfigurableApplicationContext ctx;

    @Override
    public void run(String... arg0) throws Exception {
        System.out.println("Ready ... running for " + duration + "ms");
        Thread.sleep(duration);
        ctx.close();
    }
}


```





# 整合 grpc

https://github.com/ChinaSilence/spring-boot-starter-grpc



# 任务调度

## 整合 XXL-JOB 任务调度平台

https://www.tianheyu.top/archives/springboot-xxl-job-executor

## 异步任务

```java
@Configuration
@EnableAsync
public class AsyncConfig implements AsyncConfigurer {

    @Override
    public Executor getAsyncExecutor() {
        ThreadPoolTaskExecutor taskExecutor = new ThreadPoolTaskExecutor();
        //核心线程数
        taskExecutor.setCorePoolSize(8);
        //最大线程数
        taskExecutor.setMaxPoolSize(16);
        //队列大小
        taskExecutor.setQueueCapacity(100);
        // 当最大池已满时,此策略为我们提供可伸缩队列，此策略保证不会丢失任务请求，但是可能会影响应用程序整体性能。
    executor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
    executor.setThreadNamePrefix("My ThreadPoolTaskExecutor-");
        taskExecutor.initialize();
        return taskExecutor;
    }
}




// 使用
@Service
public class FutureService {

    //在需要执行的方法上加@Async表明该方法是个异步方法，如果加在类级别上，则表明类所有的方法都是异步方法
    @Async
    public Future<String> futureTest() throws InterruptedException {
        System.out.println("任务执行开始,需要：1000ms");
        for (int i = 0; i < 10; i++) {
            Thread.sleep(100);
            System.out.println("do:" + i);
        }
        System.out.println("完成任务");
        return new AsyncResult<>(Thread.currentThread().getName());
    }


    @Async
    public CompletableFuture<List<String>> completableFutureTask(String start) {
        // 打印日志
        logger.warn(Thread.currentThread().getName() + "start this task!");
        // 找到特定字符/字符串开头的电影
        List<String> results =
            movies.stream().filter(movie -> movie.startsWith(start)).collect(Collectors.toList());
        // 模拟这是一个耗时的任务
        try {
        Thread.sleep(1000L);
        } catch (InterruptedException e) {
        e.printStackTrace();
        }
        //返回一个已经用给定值完成的新的CompletableFuture。
        return CompletableFuture.completedFuture(results);
    }
}
```

## 定时任务

### springboot 内置

SpringBoot为我们内置了@Scheduled定时任务 (spring task), @EnableScheduling 开启

@Scheduled注解各参数详解: (https://www.jianshu.com/p/1defb0f22ed1)

- cron
- zone
- fixedDelay 上次完毕到下次开始(ms)
- fixedDelayString="${time.fixedDelay}" 同上
- fixedRate 上次开始到下次开始
- initialDelay 首次延迟

自定义线程池:

```java

//默认情况下，@Scheduled任务都在Spring创建的大小为1的默认线程池中执行

@Configuration
public class SchedulerConfig implements SchedulingConfigurer {
    private final int POOL_SIZE = 10;

    @Override
    public void configureTasks(ScheduledTaskRegistrar scheduledTaskRegistrar) {
        ThreadPoolTaskScheduler threadPoolTaskScheduler = new ThreadPoolTaskScheduler();

        threadPoolTaskScheduler.setPoolSize(POOL_SIZE);
        threadPoolTaskScheduler.setThreadNamePrefix("my-scheduled-task-pool-");
        threadPoolTaskScheduler.initialize();

        scheduledTaskRegistrar.setTaskScheduler(threadPoolTaskScheduler);
    }
}


```

### 集成 Quartz


# 配置文件 and 环境

## 读取配置文件

1. 使用 @Value("${property}") 读取比较简单的配置信息

2. 通过@ConfigurationProperties(prefix = "library")读取并与 bean 绑定, 需要 加 @component, 若不用 @component, 则必须 @EnableConfigurationProperties(...)

3. @ConfigurationProperties 配合 @bean 用在方法上, 方法调用空构造函数返回想要构造的对象

4. @PropertySource("classpath:website.properties") 读取指定 properties 文件

## 配置文件优先级

配置文件加载优先级: 命令行 > ${user.dir}/config/xxx > ${user.dir}/xxx (usr.dir 即为命令执行的当前目录) > classpath:config/xxx > classpath:xxx

--spring.profiles.active=prod,,,, --spring.config.location=target/application.properties

## 读取环境信息

```java
/**
 * @author xiaoyu
 * date: 2020/3/21
 */
@SpringBootApplication
public class SpringbootDemoApp {

    /**
     * springboot在启动的时候需要检测当前项目是否是一个web项目，
     * 检测方式为判断classpath中是否有以下final变量中定义的两个参数
     */
    private static final String[] WEB_ENVIRONMENT_CLASSES =
            new String[]{"javax.servlet.Servlet", "org.springframework.web.context.ConfigurableWebApplicationContext"};

    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(SpringbootDemoApp.class, args);
        ConfigurableEnvironment        env     = context.getEnvironment();

        String[] activeProfiles = env.getActiveProfiles();
        String   port           = env.getProperty("server.port");

        System.out.println("==============================");
        System.out.println("active profiles: " + Arrays.toString(activeProfiles));
        System.out.println("local uri: http://localhost:" + port);
        System.out.println("local area network uri: http://" + localAreaNetwork() + ":" + port);
        System.out.println("==============================");
    }

    private static String localAreaNetwork() {
        try {
            return InetAddress.getLocalHost().getHostAddress();
        } catch (UnknownHostException e) {
            e.printStackTrace();
            return "get local area network error";
        }
    }

    private boolean deduceWebEnv() {
        //https://blog.csdn.net/txba6868/article/details/80732475
        return true;
    }
}

@RestController
class DemoController {

    /**
     * file upload 文件上传
     */
    @RequestMapping(value = "/upload", method = RequestMethod.POST)
    public ResponseEntity<String> upload(HttpServletRequest request) throws IOException {
        MultipartHttpServletRequest req = (MultipartHttpServletRequest) request;
        MultipartFile aa = req.getFile("aa");
        MultipartFile bb = req.getFile("bb");
        aa.transferTo(Paths.get(System.getProperty("user.home") + "/tmp").toFile());

        return ResponseEntity.ok("ok");
    }
}

```

## 两种引入 springboot 方式

一种是通过 设置 parent 为 spring boot starter parent

一种是通过 dependencyManagement 加入 spring boot dependencies (需要 设置 maven  properties, `maven.compiler.source` 和 `maven.compiler.target` 防止编译使用 jdk 1.5)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

<!--    <parent>-->
<!--        <groupId>org.springframework.boot</groupId>-->
<!--        <artifactId>spring-boot-starter-parent</artifactId>-->
<!--        <version></version>-->
<!--        <relativePath/> &lt;!&ndash; lookup parent from repository &ndash;&gt;-->
<!--    </parent>-->

    <groupId>io.github.xiaoyureed</groupId>
    <artifactId>shopee</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>pom</packaging>
    <description>聚合服务</description>

    <properties>
        <java.version>1.8</java.version>
        <maven.compiler.source>${java.version}</maven.compiler.source>
        <maven.compiler.target>${java.version}</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <spring-boot.version>2.3.4.RELEASE</spring-boot.version>
        <spring-cloud.version>Hoxton.SR8</spring-cloud.version>
        <spring-cloud-alibaba.version>2.2.1.RELEASE</spring-cloud-alibaba.version>
        <aliyun-spring-boot.version>1.0.0</aliyun-spring-boot.version>
    </properties>

    <dependencyManagement>

        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>${spring-cloud-alibaba.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>aliyun-spring-boot-dependencies</artifactId>
                <version>${aliyun-spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>

            <dependency>
                <groupId>com.baomidou</groupId>
                <artifactId>mybatis-plus-boot-starter</artifactId>
                <version>3.4.0</version>
            </dependency>

            <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>8.0.21</version>
            </dependency>

            <dependency>
                <groupId>org.elasticsearch.client</groupId>
                <artifactId>elasticsearch-rest-high-level-client</artifactId>
                <version>7.9.2</version>
            </dependency>

<!--            代码生成 -->
            <dependency>
                <groupId>com.baomidou</groupId>
                <artifactId>mybatis-plus-generator</artifactId>
                <version>3.4.0</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/org.apache.velocity/velocity-engine-core -->
            <dependency>
                <groupId>org.apache.velocity</groupId>
                <artifactId>velocity-engine-core</artifactId>
                <version>2.2</version>
            </dependency>

            <!-- https://mvnrepository.com/artifact/org.apache.httpcomponents/httpcore -->
<!--            common module need this-->
            <dependency>
                <groupId>org.apache.httpcomponents</groupId>
                <artifactId>httpcore</artifactId>
                <version>4.4.13</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/org.apache.commons/commons-lang3 -->
            <dependency>
                <groupId>org.apache.commons</groupId>
                <artifactId>commons-lang3</artifactId>
                <version>3.11</version>
            </dependency>




        </dependencies>
    </dependencyManagement>

    <dependencies>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>


</project>
```

使用这种方式 spring boot maven plugin 可能失效, 需要手动配置

```xml
<build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

# 序列化 反序列化

## jackson 使用

springboot web starter 中包含了 Jackson-databind 依赖, 可以直接注入 objectmapper, 若自己定制:


### @JsonComponent

用于对特定属性定制序列化

```java
@Data
@AllArgsConstructor
public class User {
    private Color favoriteColor;
}


@JsonComponent
public class UserCombinedSerializer {

    public static class UserJsonSerializer // 若只需要序列化, 不需要反序列化, 则只需要这个类 并在这个类标注 @JsonComponent. 外层类和下面的内部类类都不需要, 
      extends JsonSerializer<User> {

        @Override
        public void serialize(User user, JsonGenerator jsonGenerator, 
          SerializerProvider serializerProvider) throws IOException, 
          JsonProcessingException {

            jsonGenerator.writeStartObject();
            jsonGenerator.writeStringField(
              "favoriteColor", getColorAsWebColor(user.getFavoriteColor()));

            // ... 其他属性...

            jsonGenerator.writeEndObject();
        }

        private static String getColorAsWebColor(Color color) {
            int r = (int) Math.round(color.getRed() * 255.0);
            int g = (int) Math.round(color.getGreen() * 255.0);
            int b = (int) Math.round(color.getBlue() * 255.0);
            return String.format("#%02x%02x%02x", r, g, b);
        }
    }

    public static class UserJsonDeserializer 
      extends JsonDeserializer<User> {

        @Override
        public User deserialize(JsonParser jsonParser, 
          DeserializationContext deserializationContext)
          throws IOException, JsonProcessingException {

            TreeNode treeNode = jsonParser.getCodec().readTree(jsonParser);
            TextNode favoriteColor = (TextNode) treeNode.get(
              "favoriteColor");
            return new User(Color.web(favoriteColor.asText()));
        }
    }
}
```

### Jackson 在 springboot 中的配置

```
spring.jackson.date-format指定日期格式，比如yyyy-MM-dd HH:mm:ss，或者具体的格式化类的全限定名

spring.jackson.deserialization是否开启Jackson的反序列化

spring.jackson.generator是否开启json的generators.

spring.jackson.joda-date-time-format指定Joda date/time的格式，比如yyyy-MM-ddHH:mm:ss). 如果没有配置的话，dateformat会作为backup

spring.jackson.locale指定json使用的Locale.

spring.jackson.mapper是否开启Jackson通用的特性.

spring.jackson.parser是否开启jackson的parser特性.

spring.jackson.property-naming-strategy指定PropertyNamingStrategy(CAMEL_CASE_TO_LOWER_CASE_WITH_UNDERSCORES)或者指定PropertyNamingStrategy子类的全限定类名.

spring.jackson.serialization是否开启jackson的序列化.

spring.jackson.serialization-inclusion指定序列化时属性的inclusion方式，具体查看JsonInclude.Include枚举.

spring.jackson.time-zone指定日期格式化时区，比如America/Los_Angeles或者GMT+10
```

### jackson注解

```java
@JsonIgnore 此注解用于属性上，作用是进行JSON操作时忽略该属性。
@JsonIgnoreProperties(value = { "intValue" }) on type, 忽略指定属性
@JsonIgnoreType on type 忽略所有属性
        //or @JsonIgnoreType on mixin type(empty type) to ignore fields of the type which we have no access to
        //        mapper.addMixIn(String[].class, MyMixInForIgnoreType.class);//MyMixInForIgnoreType is empty
        //Note: Since version 2.5 – it seems that we can not use this method to ignore primitive data types, but we can use it for custom data types and arrays.
@JsonFormat 此注解用于属性上，作用是把Date类型直接转化为想要的格式，如@JsonFormat(pattern = “yyyy-MM-dd HH-mm-ss”)。
@JsonProperty 此注解用于属性上，作用是把该属性的名称序列化为另外一个名称，如把trueName属性序列化为name，@JsonProperty(“name”)。
@JsonValue 用于属性/get方法上, 一个类只能用一个, 相当于自定义序列化, 当加上@JsonValue注解，序列化是只返回这一个字段的值/方法的值作为当前对象的值
@JsonRawValue on json string field to serialize raw json string
```

### objectmapper 定制

https://www.kancloud.cn/ahutchen/spring-boot-reference-guide/333370


```java
@Bean
    public ObjectMapper objectMapper() {
        ObjectMapper objectMapper = new ObjectMapper();

        /*
        >>> serialization config
         */
         //序列化的时候序列对象的所有属性
        //          JsonInclude.Include.NON_NULL 忽略 null 值
        //          Include.NON_EMPTY 如果为null或者 空字符串和空集合都不会被序列化
		objectMapper.setSerializationInclusion(Include.ALWAYS);
        
        //如果是空对象的时候,不抛异常
		objectMapper.configure(SerializationFeature.FAIL_ON_EMPTY_BEANS, false);

        // 格式化输出
        objectMapper.configure(SerializationFeature.INDENT_OUTPUT, true);
        //or
            mapper.enable(SerializationFeature.INDENT_OUTPUT);


        // 自定义时间格式
        //取消时间的转化格式,默认是时间戳,可以取消,同时需要设置要表现的时间格式
		objectMapper.configure(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS, false);// 默认为true,会显示时间戳
		objectMapper.setDateFormat(new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"))

        //  config filter
        //
//        new SimpleBeanPropertyFilter.FilterExceptFilter()
//        objectMapper.setFilterProvider()





        /*
        >>> deserialization config
         */
		//反序列化的时候如果多了其他属性,不抛出异常
        objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);





        //@JsonValue 可以用在get方法或者属性字段上，一个类只能用一个，当加上@JsonValue注解是，序列化是只返回这一个字段的值

        //@JsonRootName(value = "user") to wrapper a root property for output json

        //@JsonSerialize(using = CustomDateSerializer.class) on field to specify a customized serializer
        // ex: class CustomDateSerializer extends StdSerializer<Date>

        return objectMapper;
    }

```

### objectmapper 使用

```java

// >>> 序列化数组

Person person1 = new Person(1, "zxc", new Date());
Person person2 = new Person(2, "ldh", new Date());
List<Person> persons = new ArrayList<>();
persons.add(person1);
persons.add(person2);
String personStr = objectMapper.writeValueAsString(persons);

//反序列化为List<user> 集合,1需要通过 TypeReference 来具体传递值
List<Person> persons2 = objectMapper.readValue(personStr, new TypeReference<List<Person>>() {});
//or 
//通过 JavaType 来进行处理返回
JavaType javaType = objectMapper.getTypeFactory().constructParametricType(List.class, Person.class);
List<Person> persons3 = objectMapper.readValue(personStr, javaType);
```


# 拾遗

## bean 懒加载

一般情况下，Spring容器在启动时会创建所有的Bean对象，使用@Lazy注解 (和 @Component 共同使用) 可以将Bean对象的创建延迟到第一次使用Bean的时候