---
title: SpringBoot reference Doc
date: 2017-12-30 22:40:32
tags: [springboot]
categories: java web
---
<div align="center">
springboot reference doc 翻译
开箱即用，提供各种默认配置来简化项目配置|内嵌式容器简化Web项目| 没有冗余代码生成和XML配置的要求
</div>

<!--more-->

<!-- TOC -->

- [quickstart](#quickstart)
  - [生成骨架](#生成骨架)
  - [项目代码的结构 Structuring your code](#项目代码的结构-structuring-your-code)
  - [代码](#代码)
- [starter](#starter)
- [标识入口类](#标识入口类)
- [配置类(Configuration classes )](#配置类configuration-classes-)
  - [添加配置类(Importing additional configuration classes)](#添加配置类importing-additional-configuration-classes)
  - [Importing XML configuration 添加xml配置](#importing-xml-configuration-添加xml配置)
- [开启自动配置(Auto-configuration)](#开启自动配置auto-configuration)
  - [取消特定的自动配置(Disabling specific auto-configuration )](#取消特定的自动配置disabling-specific-auto-configuration-)
- [bean注册 依赖注入](#bean注册-依赖注入)
- [@SpringBootApplication使用](#springbootapplication使用)
- [工程跑起来(Running your application)](#工程跑起来running-your-application)
- [(开发者组件)Developer tools](#开发者组件developer-tools)
  - [特性：自动设置默认值](#特性自动设置默认值)
  - [特性：自动重启(Automatic restart)](#特性自动重启automatic-restart)
    - [重启时的日志记录（Logging changes in condition evaluation）](#重启时的日志记录logging-changes-in-condition-evaluation)
    - [排除受监控资源(Excluding resources)](#排除受监控资源excluding-resources)
    - [添加额外监控路径(Watching additional paths )](#添加额外监控路径watching-additional-paths-)
    - [重启失效（Disabling restart）](#重启失效disabling-restart)
    - [设置一个trigger file](#设置一个trigger-file)
    - [自定义restart classloader](#自定义restart-classloader)
    - [devtools的局限](#devtools的局限)
  - [浏览器自动刷新(LiveReload)](#浏览器自动刷新livereload)
  - [全局设置(Global settings )](#全局设置global-settings-)
  - [远程调试app(Remote applications)](#远程调试appremote-applications)
    - [启动本地的客户端](#启动本地的客户端)
    - [远程调试](#远程调试)
- [打包到生产环境](#打包到生产环境)
- [SpringApplication](#springapplication)
  - [启动失败(Startup failure)](#启动失败startup-failure)
  - [定制banner](#定制banner)
    - [直接通过设置file](#直接通过设置file)
    - [通过编程方式](#通过编程方式)
    - [控制banner打印到的地方](#控制banner打印到的地方)
  - [定制SpringApplication](#定制springapplication)
  - [应用的事件和监听器](#应用的事件和监听器)
  - [注入ApplicationContext](#注入applicationcontext)
  - [自动设置web环境](#自动设置web环境)
  - [Application exit](#application-exit)
  - [获取admin属性(Admin features)](#获取admin属性admin-features)
- [引入配置文件(Externalized Configuration)](#引入配置文件externalized-configuration)
  - [怎么引入呢](#怎么引入呢)
  - [配置文件的优先级](#配置文件的优先级)
  - [不同文件夹中的application.properties](#不同文件夹中的applicationproperties)
  - [多环境配置(Profile-specific properties)](#多环境配置profile-specific-properties)
  - [注入自定义属性-占位符](#注入自定义属性-占位符)
  - [使用YAML](#使用yaml)
    - [载入yaml](#载入yaml)
    - [将yml转为properties](#将yml转为properties)
    - [yaml的多环境配置](#yaml的多环境配置)
    - [yaml不能使用@PropertySource](#yaml不能使用propertysource)
    - [ymal中的list](#ymal中的list)
  - [@ConfigurationProperties](#configurationproperties)
    - [统一前缀](#统一前缀)
    - [第三方组件去配置化](#第三方组件去配置化)
    - [松绑定（relaxed binding）](#松绑定relaxed-binding)
    - [复杂类型的混合(merge)](#复杂类型的混合merge)
    - [属性 转换](#属性-转换)
- [profiles](#profiles)
  - [添加active-profiles](#添加active-profiles)
- [springboot单元测试](#springboot单元测试)
- [日志](#日志)
  - [输出配置](#输出配置)
    - [输出格式](#输出格式)
    - [console控制台输出](#console控制台输出)
    - [带颜色输出](#带颜色输出)
    - [file文件输出](#file文件输出)
  - [日志级别(Log Levels)](#日志级别log-levels)
  - [自定义log configration](#自定义log-configration)
  - [MDC特性](#mdc特性)
  - [logback详细配置](#logback详细配置)
    - [依赖](#依赖)
    - [logback-spring.xml](#logback-springxml)
    - [看一个具体日志需求](#看一个具体日志需求)
    - [排除不需要的日志](#排除不需要的日志)
  - [logback扩展特性](#logback扩展特性)
    - [logback的多环境配置](#logback的多环境配置)
    - [配置中使用Environment properties](#配置中使用environment-properties)
- [开发 web app](#开发-web-app)
  - [引入WebJars](#引入webjars)
    - [什么是webjars](#什么是webjars)
    - [为什么使用webjars](#为什么使用webjars)
    - [如何使用 webjars](#如何使用-webjars)
  - [Gradle 构建工具](#gradle-构建工具)
  - [开发filter](#开发filter)
  - [Spring Web MVC框架](#spring-web-mvc框架)
    - [自动配置Springmvc](#自动配置springmvc)
    - [http消息转换器(HttpMessageConverters)](#http消息转换器httpmessageconverters)
    - [MessageCodesResolver](#messagecodesresolver)
    - [静态资源](#静态资源)
    - [欢迎页](#欢迎页)
    - [自定义网站图标](#自定义网站图标)
    - [请求匹配和资源导航(Path Matching and Content Negotiation)](#请求匹配和资源导航path-matching-and-content-negotiation)
    - [ConfigurableWebBindingInitializer](#configurablewebbindinginitializer)
    - [模板引擎](#模板引擎)
    - [全局异常处理](#全局异常处理)
      - [如果返回视图](#如果返回视图)
      - [如果返回json](#如果返回json)
      - [定制自己的错误页面](#定制自己的错误页面)
      - [不使用springmvc](#不使用springmvc)
    - [配置允许跨域访问](#配置允许跨域访问)
  - [Spring WebFlux Framework](#spring-webflux-framework)
  - [JAX-RS and Jersey](#jax-rs-and-jersey)
  - [嵌入式servlet容器](#嵌入式servlet容器)
    - [自定义内嵌容器](#自定义内嵌容器)
    - [Servlets, Filters, and listeners](#servlets-filters-and-listeners)
    - [servlet上下文初始化](#servlet上下文初始化)
    - [对jsp的限制](#对jsp的限制)
- [安全(Security)](#安全security)
  - [OAuth2](#oauth2)
- [实践](#实践)
  - [接口开发](#接口开发)
  - [使用模板引擎](#使用模板引擎)
    - [themeleaf](#themeleaf)
    - [freemarker](#freemarker)
    - [velocity](#velocity)
  - [集成swagger2](#集成swagger2)
  - [统一异常处理](#统一异常处理)
  - [使用JdbcTemplate](#使用jdbctemplate)
  - [使用Spring-data-jpa](#使用spring-data-jpa)
  - [多数据源配置](#多数据源配置)
    - [mybatis](#mybatis)
    - [JdbcTemplate](#jdbctemplate)
    - [Spring-data-jpa](#spring-data-jpa)
  - [使用redis作为数据库](#使用redis作为数据库)
  - [@Async异步调用](#async异步调用)
  - [log4j支持](#log4j支持)
  - [多环境下的日志配置](#多环境下的日志配置)
  - [aop统一处理请求日志](#aop统一处理请求日志)
  - [缓存支持](#缓存支持)
  - [消息服务](#消息服务)
  - [监控管理](#监控管理)

<!-- /TOC -->

# quickstart

## 生成骨架

访问：http://start.spring.io/  

生成的Application和ApplicationTests类都可以直接运行来启动当前创建的项目，由于目前该项目未配合任何数据访问或Web模块，程序会在加载完Spring之后结束运行。  

此时的pom.xml内容如下，仅引入了两个模块  

* spring-boot-starter：核心模块，包括自动配置支持、日志和YAML
* spring-boot-starter-test：测试模块，包括JUnit、Hamcrest、Mockito

为了引入Web模块，需添加spring-boot-starter-web模块  

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

* 这里比较下`<dependency>`和`<dependencyManagement>`区别：  

    * `<dependencyManagement>`只是对jar版本号进行管理，不会引入，只是声明,需在子pom中显式引入(子pom中可以不加版本号 )  

        例如：父pom在dependencyManagement中引入spring-boot-dependencies，则所有子pom中引入依赖时无需版本号

    * `<dependency>`真正引入，子pom均会继承


## 项目代码的结构 Structuring your code

其中src/main/java中推荐的结构如下：  

```
com
+- example
    +- myproject
        +- Application.java
        |
        +- domain
        |   +- Customer.java
        |   +- CustomerRepository.java
        |
        +- service
        |   +- CustomerService.java
        |
        +- web
            +- CustomerController.java
```

## 代码

```java
@RestController
@RequestMapping("/")
public class DemoController {

    @GetMapping("index")
    public String index() {
        return "hello";
    }
}

```

通过ide启动主程序，打开浏览器访问http://localhost:8080/index，可以看到页面输出Hello

通过maven启动：进入根目录，`mvn spring-boot:run`

通过命令行, `java -jar xxx`启动. 先打包，生成jar：`mvn package `,如果要忽略测试：`mvn package -DskipTests`,jar包在target目录

如果没有继承spring-boot-starter-parent，pom需要引入：

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>

```

单元测试用例：不用启动即可测试

```java
/**
注意引入下面内容，让status、content、equalTo函数可用

    import static org.hamcrest.Matchers.equalTo;
    import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
    import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;

*/
@RunWith(SpringRunner.class)
@SpringBootTest
public class Chapter1ApplicationTests {

    private MockMvc mvc;

    @Before
    public void setUp() throws Exception {
        mvc = MockMvcBuilders.standaloneSetup(new HelloController()).build();
    }

	@Test
	public void getHello() throws Exception {
	    mvc.perform(MockMvcRequestBuilders.get("/index").accept(MediaType.APPLICATION_JSON))
        .andExpect(status().isOk())
        .andExpect(content().string(equalTo("Hello World!")));
	}

}
```


# starter

https://www.nosuchfield.com/2017/10/15/Spring-Boot-Starters/
https://www.cnblogs.com/yuansc/p/9088212.html

# 标识入口类

`@EnableAutoConfiguration` 一般设置在入口类上, 他隐式地将当前类所在包设置为"base search package", 整个app将搜寻这个包及其子包中的items(eg: 一个jpa app, 被该注解表示类所在的包将会被用来搜寻@Entity items).

使用了`@EnableAutoConfiguration`标识base package之后,  使用`@ComponentScan`就无需设置`basePackage `属性了

`@Configuration` 用@Configuration注解的类，等价 与XML中配置beans；用@Bean标注方法等价于XML中配置的bean

例子如下: (相当于 配置spring开启注解扫描`<context:component-scan base-package="com.xiaoyu.demo" />`)
```java
@Configuration  
public class ExampleConfiguration {  
  
    @Value("${batch.jdbc.driver}")  
    private String driverClassName;  
  
    @Value("${batch.jdbc.url}")  
    private String driverUrl;  
  
    @Value("${batch.jdbc.user}")  
    private String driverUsername;  
  
    @Value("${batch.jdbc.password}")  
    private String driverPassword;  
  
    @Bean(name = "dataSource")  
    public DataSource dataSource() {  
        BasicDataSource dataSource = new BasicDataSource();  
        dataSource.setDriverClassName(driverClassName);  
        dataSource.setUrl(driverUrl);  
        dataSource.setUsername(driverUsername);  
        dataSource.setPassword(driverPassword);  
        return dataSource;  
    }  
  
    @Bean  
    public PlatformTransactionManager transactionManager() {  
        return new DataSourceTransactionManager(dataSource());  
    }  
  
}  
```

# 配置类(Configuration classes )

入口类也是作为@configuration class 的 好选择(Usually the class that defines the main method is also a good candidate as the primary `@Configuration`.)

想要添加配置文件时, 先试试搜索`@Enable*`注解, 如果不能解决, 再考虑添加xml配置

## 添加配置类(Importing additional configuration classes)

如果不止一个@Configuration class, 可以在选定的主配置类上(比如入口类)使用`@Import({xxxConfig.class})`来导入另外的配置类,这种方式情况下, 只需要主配置类使用`@Configuration和@Import({xxxConfig.class})`, 其他配置类无需任何注解

或者也可以`@ComponentScan`自动扫描所有spring component, 当然也包括@Configuration classes, 此时每个配置类都需要@Configuration

## Importing XML configuration 添加xml配置

首先创建一个`@Configuration`类, 然后在类上通过`@ImportResource("classpath:cons-injec.xml")`来导入xml配置文件

用来兼容老的 spring 项目

# 开启自动配置(Auto-configuration)

选择一个`@Configuration` classes 添加 `@EnableAutoConfiguration` or `@SpringBootApplication`, 推荐入口类

自动配置开启后, 引入其他特定的组件那么相应的自动配置会被自动替换, 通过`--debug`运行查看有哪些自动配置(Auto-configuration is noninvasive, at any point you can start to define your own configuration to replace specific parts of the auto-configuration. For example, if you add your own DataSource bean, the default embedded database support will back away)

## 取消特定的自动配置(Disabling specific auto-configuration )

`@EnableAutoConfiguration(exclude={xxxAutoConfiguration.class})`

```java
@Configuration
@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
public class MyConfiguration {
}
```

如果想取消的 自动配置类 不在classpath下, 使用该注解的`excludeName ` 来指定fully qualified name

同时, 也可使用`spring.autoconfigure.exclude` property.

#  bean注册 依赖注入

`@ComponentScan` 和 `@Autowired` 联合使用

如果按照推荐的代码结构, `@ComponentScan`无需配置任何额外属性, 就能将 All of your application components (`@Component, @Service, @Repository, @Controller` etc.) will be automatically registered as Spring Beans.

```java
@Service
public class DatabaseAccountService implements AccountService {

    private final RiskAssessor riskAssessor = null;// 这里是final的, 意味着不能修改它了

    // @Autowired// 此时由于有了构造函数, @Autowired其实可以省略
    public DatabaseAccountService(RiskAssessor riskAssessor) {
        this.riskAssessor = riskAssessor;
    }

    // ...

}

// 或者直接
@Service
public class DatabaseAccountService implements AccountService {

    @Autowired
    private final RiskAssessor riskAssessor;// 这里是final的, 意味着不能修改它了

    // ...

}
```

# @SpringBootApplication使用

The `@SpringBootApplication` annotation is equivalent to using `@Configuration, @EnableAutoConfiguration and @ComponentScan` with their default attributes

`@SpringBootApplication`也提供属性来排除auto-configration class, 设置 base package

```java
@SpringBootApplication // same as @Configuration @EnableAutoConfiguration @ComponentScan
public class Application {

	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}

}

//@Configuration, @EnableAutoConfiguration and @ComponentScan不是非有不可的
// 比如不使用@ComponentScan
@Configuration
@EnableAutoConfiguration
@Import({ MyConfig.class, MyAnotherConfig.class })
public class Application {

	public static void main(String[] args) {
			SpringApplication.run(Application.class, args);
	}

}

```

# 工程跑起来(Running your application)

Running from an IDE就不说了

Running as a packaged application `java -jar target/myproject-0.0.1-SNAPSHOT.jar`

Using the Maven plugin `mvn spring-boot:run`, 设置合适的环境变量: `export MAVEN_OPTS=-Xmx1024m -XX:MaxPermSize=128M`


热拔插(热部署)效果(Hot Swapping): springboot工程是一个简单的java app, 所以热部署是out of the box的,但是在字节码替换方面有限制( JVM hot swapping is somewhat limited with the bytecode that it can replace), 更强的功能可以看看 [🚪](https://zeroturnaround.com/software/jrebel/)


# (开发者组件)Developer tools

spring-boot-devtools 能够提供开发阶段的一系列特性(比如模版引擎会缓存解析后的模版, DevTools会禁用它, 再比如在为静态资源提供服务时，Spring MVC可以将HTTP缓存头添加到响应中, DevTool也禁用他)，当运行一个打包好的程序APP时(比如:java -jar xxx)，这些禁用会自动失效：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional> <!--  最佳实践, 防止传递到其他module -->
    </dependency>
</dependencies>
```

## 特性：自动设置默认值

例如：无需手动设置spring.thymeleaf.cache=false，还有其他一些默认属性：

```java
properties.put("spring.thymeleaf.cache", "false");
properties.put("spring.freemarker.cache", "false");
properties.put("spring.groovy.template.cache", "false");
properties.put("spring.mustache.cache", "false");
properties.put("server.session.persistent", "true");
properties.put("spring.h2.console.enabled", "true");
properties.put("spring.resources.cache-period", "0");
properties.put("spring.resources.chain.cache", "false");
properties.put("spring.template.provider.cache", "false");
properties.put("spring.mvc.log-resolved-exception", "true");
properties.put("server.jsp-servlet.init-parameters.development", "true");
```

具体有那些default properties, see https://github.com/spring-projects/spring-boot/blob/v1.5.9.RELEASE/spring-boot-devtools/src/main/java/org/springframework/boot/devtools/env/DevToolsPropertyDefaultsPostProcessor.java

## 特性：自动重启(Automatic restart)

classpath下的文件改动，APP自动重启, 通过spring-boot-devtools实现

autorestart和LiveReload合作良好， 和Jrebel合作自动重启会关闭以支持动态类重新加载(此时也没必要自动重启了，JRebel牛🐮多了)

如何关闭?

```java
//spring-boot-devtools依赖 application context’s shutdown hook 
// 会使得自动重启失效 ,如果不指定默认是开启的 
SpringApplication.setRegisterShutdownHook(false) 
```

当然，触发重启会忽略这些名称的project：spring-boot, spring-boot-devtools, spring-boot-autoconfigure, spring-boot-actuator, and spring-boot-starter.

### 重启时的日志记录（Logging changes in condition evaluation）

默认下， 每次restart， 日志都会记录造成重启的配置修改， 如果想disable， `spring.devtools.restart.log-condition-evaluation-delta=false`

### 排除受监控资源(Excluding resources)

默认情况下, `/META-INF/maven, /META-INF/resources, /resources, /static, /public or /templates `中的文件改变不会触发restart, 但会触发 live reload. 如果希望自定义排除监控的文件, `spring.devtools.restart.exclude`

如排除/static包，/public包下文件：  `spring.devtools.restart.exclude=static/**,public/**`

如果希望保持默认, 在默认的基础上添加排除监控的文件, `spring.devtools.restart.additional-exclude`

### 添加额外监控路径(Watching additional paths )

如果需要添加额外的监控文件, 如将非classpath下文件纳入监控  `spring.devtools.restart.additional-paths `

### 重启失效（Disabling restart）

如何使重启失效？  

```java
// 简单的方法是：配置文件中设置
spring.devtools.restart.enabled=false  

// 当然还有更“全局”的方法：  
public static void main(String[] args) {
    System.setProperty("spring.devtools.restart.enabled", "false");
    SpringApplication.run(MyApp.class, args);

    // 另一种方法关闭
    SpringApplication.setRegisterShutdownHook(false) ;
}
```

### 设置一个trigger file

如果你的IDE会随改随编译，你可能会倾向于只在特定时刻引发重启（否则会很烦人，而且性能下降）。这时，你可以使用“trigger file”，就是一个特定的文件，只有修改这个文件时才会触发重启。使用 `spring.devtools.restart.trigger-file` 属性即可。

比如: spring.devtools.restart.trigger-file=.reloadtregger

全局设置 用户文件夹下的`.spring-boot-devtools.properties  `  在 $HOME folder ，见 [全局trigger-file](#全局设置global-settings-)

### 自定义restart classloader

当一个项目有多个module，而并不是每个module都导入ide时，DevTools 需要自定义“restart classloader” ，定义哪个model是重启监控对象

> restart vs. reload：
>“自动重启”由两个classloader实现，不变的jar（如引入的第三方jar）会被“base” classloaer载入，ide中打开的项目自身会被“restart” classloader载入, 触发重启时, 只有“restart” classloader会被销毁重建, 所以restart速度更快.替代技术JRebel。

通过创建`META-INF/spring-devtools.properties`实现（classpath下所有的META-INF/spring-devtools.properties均会被载入,即使是jar包中的）

`spring-devtools.properties`介绍
properties文件中properties值为正则表达式, 会匹配整个classpath, 其中包含两类properties, 一类以`restart.exclude.`作为前缀, 表示会被放进“base” classloader中的jar, 一类以`restart.include.`开头, 表示会被放进“restart” classloader中的jar

后缀任意即可

eg:
```js
restart.exclude.companycommonlibs=/mycorp-common-[\\w-]+\.jar
restart.include.projectcommon=/mycorp-myproj-[\\w-]+\.jar
```

### devtools的局限

如果项目中的对象是通过`ObjectInputStream`进行反序列化得来的, 监控重启特性就不好使了(此时应该使用Spring’s `ConfigurableObjectInputStream` in combination with `Thread.currentThread().getContextClassLoader()`.)

## 浏览器自动刷新(LiveReload)

`spring-boot-devtools`模块内置了一个LiveReload server，资源改动会触发浏览器自动刷新，需要拓展配合：http://livereload.com/extensions/  

如果想关闭：set the `spring.devtools.livereload.enabled` property to `false`.

注意: LiveReload server一次只可能启动一个, 如果同时启动多个带DevTools 的app, 只有第一个启动的app有 livereload功能.

## 全局设置(Global settings )

$HOME 文件夹下添加一个文件` .spring-boot-devtools.properties `，该文件中的内容会被作用于所有的Spring Boot项目。例如设置 触发文件：在 `~/.spring-boot-devtools.properties` 里面设置 `spring.devtools.reload.trigger-file=.reloadtrigger`


## 远程调试app(Remote applications)

(这个特性具有安全风险, 生产环境需要禁用该特性)

Spring Boot developer tools不止可以在local app使用, 也能在 remote app使用, 远程特性是可选择的, 为了打开这个特性, 需要确定remote app包含如下配置:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <excludeDevtools>false</excludeDevtools>
            </configuration>
        </plugin>
    </plugins>
</build>
```

此外, 还需设置一个 `spring.devtools.remote.secret` properties作为口令, 比如spring.devtools.remote.secret=mysecret

remote devtools支持在两方面体现：一方面server端有endpoint接受connection，另一方面client app 运行在IDE; 当设置`spring.devtools.remote.secret` 之后，server端会自动可用，client端需手动启动；

### 启动本地的客户端

运行`org.springframework.boot.devtools.RemoteSpringApplication `

![alt](Snipaste_2018-06-13_11-03-52.png)![alt](Snipaste_2018-06-13_11-10-17.png)

启动成功后, 类似这样:

```
  .   ____          _                                              __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _          ___               _      \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` |        | _ \___ _ __  ___| |_ ___ \ \ \ \
 \\/  ___)| |_)| | | | | || (_| []::::::[]   / -_) '  \/ _ \  _/ -_) ) ) ) )
  '  |____| .__|_| |_|_| |_\__, |        |_|_\___|_|_|_\___/\__\___|/ / / /
 =========|_|==============|___/===================================/_/_/_/
 :: Spring Boot Remote :: 2.0.2.RELEASE

2015-06-10 18:25:06.632  INFO 14938 --- [           main] o.s.b.devtools.RemoteSpringApplication   : Starting RemoteSpringApplication on pwmbp with PID 14938 (/Users/pwebb/projects/spring-boot/code/spring-boot-devtools/target/classes started by pwebb in /Users/pwebb/projects/spring-boot/code/spring-boot-samples/spring-boot-sample-devtools)
2015-06-10 18:25:06.671  INFO 14938 --- [           main] s.c.a.AnnotationConfigApplicationContext : Refreshing org.springframework.context.annotation.AnnotationConfigApplicationContext@2a17b7b6: startup date [Wed Jun 10 18:25:06 PDT 2015]; root of context hierarchy
2015-06-10 18:25:07.043  WARN 14938 --- [           main] o.s.b.d.r.c.RemoteClientConfiguration    : The connection to http://localhost:8080 is insecure. You should use a URL starting with 'https://'.
2015-06-10 18:25:07.074  INFO 14938 --- [           main] o.s.b.d.a.OptionalLiveReloadServer       : LiveReload server is running on port 35729
2015-06-10 18:25:07.130  INFO 14938 --- [           main] o.s.b.devtools.RemoteSpringApplication   : Started RemoteSpringApplication in 0.74 seconds (JVM running for 1.105)
```

远程方式下，客户端的任何更新都会被push到服务器端，并按设置触发restart。比较快

使用代理的话, 设置spring.devtools.remote.proxy.host 和 spring.devtools.remote.proxy.port properties.

### 远程调试

devtools支持Http协议的远程调试通道。远程客户端提供了一个本地服务器（默认8000端口，可修改），用于绑定远程调试器。当一个连接被创建时，debug信息就会通过HTTP发送到远程应用。
修改默认端口： spring.devtools.remote.debug.local-port 。但是，首先，你需要确认远程应用以远程调试方式启动。通常，配置JAVA_OPTS即可达到目的。例如，在Cloud Foundry上，你可以在 manifest.yml 中添加如下信息：

```
---
  env:
    JAVA_OPTS: "-Xdebug -Xrunjdwp:server=y,transport=dt_socket,suspend=n"
```

过网络进行远程调试，可能很慢，所以你需要增加超时时间。Eclipse中：Java -> Debug -> Debugger timeout (ms)，设成60000很不错。

# 打包到生产环境

打成可执行包即可, 更多适用于生产环境的特性比如监控等添加`spring-boot-actuator` starter

# SpringApplication

启动引导程序  

```java 
public static void main(String[] args) {
    // 使用默认配置启动
    SpringApplication.run(MySpringConfiguration.class, args);
}
```

## 启动失败(Startup failure)

如果启动失败, 注册的`FailureAnalyzers`会提供相应的失败信息, 同时尝试解决

springboot中有很多FailureAnalyzer 的实现, 也可以自己定义一个实现;

debug模式下会显示更多错误信息 `java -jar myproject-0.0.1-SNAPSHOT.jar --debug`

## 定制banner

### 直接通过设置file

classpath中加入banner.txt ，or 设置 `spring.banner.location`属性, `spring.banner.charset`设置banner编码(默认utf-8), 支持banner.gif, banner.jpg, or banner.png格式(或者spring.banner.image.location设置动图), 

[banner中可以使用一些预定义的变量](https://docs.spring.io/spring-boot/docs/2.0.2.RELEASE/reference/htmlsingle/#boot-features-banner)


### 通过编程方式

```java
// 实现org.springframework.boot.Banner 接口 and implement your own printBanner() method
SpringApplication.setBanner(...)
```

### 控制banner打印到的地方

设置`spring.main.banner-mode`属性(console|log|off)

如果是yml文件, 加引号

```yml
spring:
	main:
		banner-mode: "off"
```


## 定制SpringApplication

```java
public static void main(String[] args) {
    // 这种启动方式可以自定义一些特性
    SpringApplication app = new SpringApplication(MySpringConfiguration.class);//MySpringConfiguration配置类，被@Configuration标注，也可以是xml文件，或者一个包路径(全包扫描)；也可以通过application.properties配置
    app.setBannerMode(Banner.Mode.OFF);//banner不输出到console
    app.run(args);
}

// app还可以做哪些设置, 见https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/api/org/springframework/boot/SpringApplication.html

//这种方式也行，允许层级结构
// 限制: Web components must be contained within the child context, and the same Environment will be used for both parent and child contexts. 
// 层级结构存疑(https://docs.spring.io/spring-boot/docs/2.0.2.RELEASE/reference/htmlsingle/#boot-features-fluent-builder-api)
new SpringApplicationBuilder()
        .sources(Parent.class)
        .child(Application.class)
        .bannerMode(Banner.Mode.OFF)
        .run(args);

```

## 应用的事件和监听器

除了传统的Spring Framework events, 如: ContextRefreshedEvent, springboot app提供一些额外的event

有些event在ApplicationContext 创建之前就被触发, 因此不能作为@Bean注册listener, 此时有如下方法:
一种方法是：`SpringApplication.addListeners(…​)` or `SpringApplicationBuilder.listeners(…​)` methods.  

另一种：` META-INF/spring.factories`文件中添加一行:  `org.springframework.context.ApplicationListener=com.example.project.MyListener  `这种方法会忽略app是以何种方式创建的;

共有如下几种application events：同时也是他们的触发顺序

1. `ApplicationStartingEvent` :在注册listeners和initializers之后, 此时还未进行任何processing, 
1. `ApplicationEnvironmentPreparedEvent` : Environment 准备好之后, context创建之前, 
1. `ApplicationPreparedEvent` : bean被载入之后, context重建之前, 标志app正在准备中
1. `ApplicationStartedEvent `: context 重建之后,  application and command-line runners调用之前
1. `ApplicationReadyEvent` : application and command-line runners 调用之后, 标志着app已经准备好提供服务
1. `ApplicationFailedEvent` : 启动时碰到exception触发

Application events 是通过spring framework的事件机制触发的, 这就保证了在具有层级结构的springContext中, child context中触发的事件会传递到 ancestors context, 所以一个listener可能会接收到多个事件实例

一个listener为了区分它自己的context中的事件和层级结构中的事件, 需要该listener的application context被注入, 然后比较listener的context和event的context, 如何注入listener的 app context? The context can be injected by implementing `ApplicationContextAware` or, if the listener is a bean, using `@Autowired`.


## 注入ApplicationContext

The context can be injected by implementing `ApplicationContextAware` or, if the listener is a bean, by using `@Autowired`.

可以方便的比较event所处的context和app的context

## 自动设置web环境

* SpringApplication 会试图创建正确的 ApplicationContext。默认的，使用 AnnotationConfigApplicationContext 或者 AnnotationConfigEmbeddedWebApplicationContext ，这取决于是否web应用。

* 可以使用覆盖 setWebEnvironment(boolean webEnvironment) 默认设置。

* 也可以使用 setApplicationContextClass(…) 来控制 ApplicationContext 的类型。

* 注意：当在JUnit环境下使用SpringApplication时，通常需要设置 setWebEnvironment(false) 或者setWebApplicationType(WebApplicationType.NONE) when using SpringApplication within a JUnit test。



## Application exit

spring boot app会注册一个 a shutdown hook 到 the JVM中 以保证 ApplicationContext 顺利关闭. 所有的Spring生命周期中的回调（如 DisposableBean 接口，或者 @PreDestroy 注解）都可以使用。

另外，如果想在应用退出时返回特定的exit code，那beans可以实现 org.springframework.boot.ExitCodeGenerator 接口。(同样可以使用@Order控制顺序，只不过相反)

```java
@SpringBootApplication
public class ExitCodeApplication {

	@Bean
	public ExitCodeGenerator exitCodeGenerator() {
		/* return new ExitCodeGenerator() {
			@Override
			public int getExitCode() {
				return 42;
			}
		}; */
        // 拉姆达表达式更简单
        return () -> 42;
	}

	public static void main(String[] args) {
		System.exit(SpringApplication
				.exit(SpringApplication.run(ExitCodeApplication.class, args)));
	}

}
```

##  获取admin属性(Admin features)

It is possible to enable admin-related features for the application by specifying the `spring.application.admin.enabled` property

eg: If you want to know on which HTTP port the application is running, get the property with key `local.server.port`

# 引入配置文件(Externalized Configuration)

properties files, YAML files, environment variables and command-line arguments to externalize configuration.  

## 怎么引入呢

两种方法

其中Property values可以通过`@Value`直接注入到bean; 通过spring的`Environment` 获取; 不通用(@Value 的工作是在SpringApplication启动完成之后进行的，在此之前值为null)

通过`@ConfigurationProperties`绑定到结构化的object上

```java
@ConfigurationProperties(prefix = "spring.datasource", ignoreUnknownFields = false)// 加上ignoreUnknownFields = false, 那么配置文件中有多余的配置项(bean中没有的属性项)则启动报错
public class SomeconfigurationBean {
}

```

具体使用见 [注入自定义属性-占位符](#注入自定义属性-占位符)

## 配置文件的优先级

各种Properties 优先级：(由高到低, 优先级高 👉 读取顺序为最后)

* Devtools的全局设置文件: `~/.spring-boot-devtools.properties`，当然是devtools激活的状态下.

* `@TestPropertySource` annotations ，测试时候使用

* `@SpringBootTest中的#properties 属性`，也是测试情况下

* 命令行设置的参数

    系统默认会把命令行option参数转换到Environment中, 如果不希望转换到Environment(可能是正式环境中需要禁用来自命令行的参数), 可以这么禁用`SpringApplication.setAddCommandLineProperties(false);`

* `SPRING_APPLICATION_JSON` 中的 properties

    在命令行中的用法：（作为一个 environment variable设置）

    ```sh
    $ SPRING_APPLICATION_JSON='{"foo":{"bar":"spam"}}' java -jar myapp.jar
    $ java -Dspring.application.json='{"foo":"bar"}' -jar myapp.jar
    $ java -jar myapp.jar --spring.application.json='{"foo":"bar"}'
    ```

    最终在spring的environment中 foo.bar=spam;


* `ServletConfig`中的初始化参数 init parameters

* `ServletContext`中的初始化参数

* JNDI attributes from java:comp/env.(存疑)

* Java System properties (System.getProperties()).

* OS的环境变量

* 通过RandomValuePropertySource 得到的随机值

    在配置文件中这样配置
    
    ```js
    my.secret=${random.value}
    my.number=${random.int}
    my.bignumber=${random.long}
    my.uuid=${random.uuid}
    /*${random.int*} 语法: OPEN value (,max) CLOSE , OPEN,CLOSE是任意符号, value,max是int类型, max可选, 如果提供, value为最小值,max为最大值 */
    my.number.less.than.ten=${random.int(10)} 
    my.number.in.range=${random.int[1024,65536]}
    ```

* jar包`外部`的 `application-{profile}.properties and YAML variants`

* jar包`内部`的 `application-{profile}.properties and YAML variants`

* jar包`外部`的 `application.properties and YAML variants`

* jar包`内部`的 `application.properties and YAML variants`

* 配置类上的`@PropertySource("classpath:/com/myco/app.properties")` (`@PropertySource` annotations on your `@Configuration` classes)

* Default properties (specified using `SpringApplication.setDefaultProperties`).

举个🌰:

假设开发一个component

```java
@Component
public class MyBean {

    @Value("${name}")
    private String name;

    // ...

}
```

那么class path中需要有application.properties包含一行"name=xxx";

打包这个app成为jar后运行于新的环境, 此时可以在jar包外部提供一个新的application.properties文件覆盖jar内部的那个配置文件

如果仅仅是一次性的测试, 可以使用命令行参数(For one-off testing, you can launch with a specific command line switch (for example,    `java -jar app.jar --name="Spring"`).)


## 不同文件夹中的application.properties

springboot app从 application.properties获取properties, 并将其设置进spring `Environment`;

分为两种文件application-{profile}.properties 或者 application.properties

这里先看application.properties ，SpringApplication默认从以下默认地址顺序搜寻(搜到即停止)，并添加到Spring Environment 中：  

1. 当前目录的`./config`子目录（file:./config/）
1. 当前目录`./`（file:./）
1. classpath下的 `/config` package（classpath:/config/）
1. The classpath root`classpath:/`（classpath:/ ）

配置文件默认为application.properties, 如果配置文件有其他的名字，用 `spring.config.name`  environment property来指定  

如果配置文件还有其他的位置，用 `spring.config.location` 来指定可以是路径，可以是具体文件位置，以逗号分开，路径都以'/'结尾  
命令行启动的话，like this:  

```sh
# 可以使用SPRING_CONFIG_NAME 代替 spring.config.name 。
$ java -jar myproject.jar --spring.config.name=myproject
# or
$ java -jar myproject.jar --spring.config.location=classpath:/default.properties,classpath:/override.properties
```

默认情况下, spring app按照Config locations的配置逆序搜寻文件(默认的加载地址永远有效), 默认提供配置是
`spring.config.location=classpath:/,classpath:/config/,file:./,file:./config/`, 那么默认搜寻顺序如下:

1. file:./config/
1. file:./
1. classpath:/config/
1. classpath:/

如果自定义了`spring.config.location`，原来的默认位置会被替换  , 如`spring.config.location=classpath:/custom-config/,file:./custom-config/`, 搜寻顺序变为:

1. file:./custom-config/
1. classpath:custom-config/

如果使用`spring.config.additional-location`制定位置, 自定义路径会被加到默认路径之前. 比如: 如果`spring.config.additional-location=classpath:/custom-config/,file:./custom-config/`，那么顺序如下：

1. file:./custom-config/
1. classpath:custom-config/
1. file:./config/
1. file:./
1. classpath:/config/
1. classpath:/

不同的优先级使得我们能够给属性一个默认值（如：application.properties中设置），然后通过`spring.config.location`指定新的文件代替默认文件 

## 多环境配置(Profile-specific properties)

在Spring Boot中多环境配置文件名需要满足`application-{profile}.properties`的格式，其中{profile}对应你的环境标识  

    application-dev.properties：开发环境
    application-test.properties：测试环境
    application-prod.properties：生产环境

至于哪个具体的配置文件会被加载，需要在application.properties文件中通过`spring.profiles.active`属性来设置(如果spring.profiles.active和SpringApplication API 中同时指定了specific profile, 以前者为准)，其值对应{profile}值。  如果没有profile被激活, `application-default.properties`会默认激活

总结多环境配置思路：  

* application.properties中配置通用内容，并设置spring.profiles.active=dev，以开发环境为默认配置

* application-{profile}.properties中配置各个环境不同的内容

* 通过命令行方式去激活不同环境的配置

这里讨论 `application-{profile}.properties`这样的配置文件.

`Environment` 本来就有很多默认属性是通过`application-default.properties`加载而来.

`application-{profile}.properties`的优先级总是要高于`application.properties`的, 不管 `application-{profile}.properties`是在jar包外还是在jar内部


## 注入自定义属性-占位符

application.properties中默认了很多配置，如果添加自己的属性怎么办呢？  

定义如下属性：


```sh
com.xy.blog.name=xy
com.xy.blog.title=reed

# 参数间引用, 注意只能调用前面定义过的变量
com.xy.blog.desc=${com.xy.blog.title}xxx

# 随机值

my.secret=${random.value}
my.number=${random.int}
my.bignumber=${random.long}
my.uuid=${random.uuid}
my.number.less.than.ten=${random.int(10)}
my.number.in.range=${random.int[1024,65536]}
```

如果这些属性是定义在application.properties中，那么：

相应Javabean：

```java
@Data
@Component
public class Blog {

    @Value("${com.xy.blog.name}")
    private String name;
    
    @Value("${com.xy.blog.title}")
    private String title;
}

//或者（推荐这种方式）
@Component
@ConfigurationProperties(prefix="com.xy.blog")
public class ConnectionSettings {

    private String name;
    private String title;
}

```

测试：  

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class Chapter1ApplicationTests {
    
    @Autowired
    private Blog blog;

	@Test
	public void testBlog() {
	    System.out.println(blog.getName());
	    System.out.println(blog.getTitle());
	}

}
```

若果定义在新的配置文件中：aaa.properties，那么：  

```java
@ConfigurationProperties(prefix = "spring.test"/*, ignoreUnknownFields = true*/)
@PropertySource(value = {"classpath:apptest.properties"}, encoding = "utf-8", ignoreResourceNotFound = false)// classpath后面不要加空格
public class ConnectionSettings {

    private String name;
    private String title;
}
```

最后注意：如果发现@ConfigurationProperties不生效，有可能是项目的目录结构问题，你可以通过在入口类加上@EnableConfigurationProperties(ConnectionSettings.class)来明确指定需要用哪个实体类来装载配置信息。如下:

```java
@SpringBootApplication  
@EnableConfigurationProperties({aaa.class,bbb.class})  
public class DemoApplication {  
  
    public static void main(String[] args) {  
        SpringApplication.run(DemoApplication.class, args);  
    }  
} 
```

## 使用YAML

spring-boot-starter会提供yaml的相关支持  , or 提供 [SnakeYAML](http://www.snakeyaml.org/) lib ([入门](https://www.jianshu.com/p/d8136c913e52)) 来支持yaml配置文件

springboot提供`YamlPropertiesFactoryBean` will load YAML as `Properties` and the `YamlMapFactoryBean` will load YAML as a `Map`


### 载入yaml

```yml
environments:
    dev:
        url: http://dev.bar.com
        name: Developer Setup
    prod:
        url: http://foo.bar.com
        name: My Cool App
my:
   servers:
       - dev.bar.com
       - foo.bar.com
# 为了保证list类型能够被覆盖, 建议定义成如下类型
my:
   servers: dev.bar.com,foo.bar.com


```

上面会被转为如下properties:

```sh
# 会被转成：
environments.dev.url=http://dev.bar.com
environments.dev.name=Developer Setup
environments.prod.url=http://foo.bar.com
environments.prod.name=My Cool App
my.servers[0]=dev.bar.com
my.servers[1]=foo.bar.com
```

怎么在app内部使用? `@Value` or `@ConfigurationProperties()`

### 将yml转为properties

`YamlPropertySourceLoader `可以将变量从yaml形式转为properties形式注入到`Environment`中, 方便直接通过`@Value`使用yaml(默认下, @value只能使用properties)

### yaml的多环境配置

借助`spring.profiles`key

```yml
# 如果development和production文件都不可用，为192.168.1.100
server:
    address: 192.168.1.100
---
# 如果development文件被激活，server.address=127.0.0.1
spring:
    profiles: development
server:
    address: 127.0.0.1
---
# 如果production文件被激活，server.address=192.168.1.120
spring:
    profiles: production
server:
    address: 192.168.1.120
---
# 如果没有profile被指定
spring:
  profiles: default
  security:
    user:
      password: weak
```

### yaml不能使用@PropertySource

不支持 @PropertySource  ，只有properties文件才支持@PropertySource  

@PropertySource 可以指定是哪个配置文件，读取的编码格式

```java
@RestController  
@PropertySource(value = {"classpath:application.properties"},encoding="utf-8")  
@RequestMapping(value="hello")  
public class HelloController {  
    /** 使用@value注解，从配置文件读取值 */  
    @Value("${TestValue}")  
    private String testValueAnno;  
      
    @RequestMapping(value="sayHello")  
    @ResponseBody  
    private String sayHello() {  
        System.out.println("测试:"+testValueAnno);  
        return "hello world!";  
    }  
} 
```

### ymal中的list

```yml
foo:
  list: # 表示默认，如果dev profile么有被激活，那么list中的Mypojo就是这个
    - name: my name
      description: my description
---
spring:
  profiles: dev # 如果dev被激活，list中的pojo就是这个，总之，list中的元素只有一个
foo:
  list:
    - name: my another name
```

```java
@ConfigurationProperties("foo")
public class FooProperties {

    private final List<MyPojo> list = new ArrayList<>();//mypojo中定义的有name，description

    public List<MyPojo> getList() {
        return this.list;
    }

}
```

## @ConfigurationProperties

### 统一前缀

通过@Value("${property}") 的方式实太过笨重  ，另一种方式如下：  

```java
@Component// 保证注册到了context中
@ConfigurationProperties("foo")
public class FooProperties {

    private boolean enabled;//定义了：foo.enabled, false by default

    private InetAddress remoteAddress;//定义了：foo.remote-address，ip地址, 能够从string强转

    private final Security security = new Security();//定义了：foo.security.username，foo.security.password，foo.security.roles；这里的属性名security，对应配置文件中的security


    public static class Security {

        private String username;

        private String password;

        private List<String> roles = new ArrayList<>(Collections.singleton("USER"));//（一个string的集合）


    }
}
```

其中有些属性可以不要setter方法，具体参见：[这里](https://docs.spring.io/spring-boot/docs/1.5.6.RELEASE/reference/htmlsingle/#boot-features-external-config-typesafe-configuration-properties)  

如果 @ConfigurationProperties 不生效(比如指定的前缀无效), 可以通过在主配置类上加@EnableConfigurationProperties来列出有哪些类用来装载配置变量

```java

@Configuration
// 这样注册后, FooProperties在context中会有一个默认名字, <prefix>-<fullQualifiedName>, 比如此处即为: foo-com.xxx.xxx.FooProperties 
@EnableConfigurationProperties(FooProperties.class)
public class MyConfiguration {
}
```

### 第三方组件去配置化

@ConfigurationProperties除了可以标注class, 还可以标注 @Bean 方法;

用在将environment中的properties配置到一个第三方bean中特别方便，将@ConfigurationProperties添加到这个bean的注册过程：  

```java
@ConfigurationProperties(prefix = "bar")// 任何以bar为前缀的属性定义都会被映射到BarComponent上
@Bean
public BarComponent barComponent() {
    ...
}
```

[Springboot整合quartz的例子](http://blog.csdn.net/liuchuanhong1/article/details/70105193)

### 松绑定（relaxed binding）

```java
@Data
@ConfigurationProperties(prefix="person")
public class OwnerProperties {

    private String name;

}
```

如下形式书写的属性均可匹配：  

`person.name`
`person.first-name`--------(推荐)
`person.first_name`
`PERSON_FIRST_NAME`

### 复杂类型的混合(merge)

对于 List

```java
@ConfigurationProperties("acme")
public class AcmeProperties {

	private final List<MyPojo> list = new ArrayList<>();// MyPojo 有name, description属性

	public List<MyPojo> getList() {
		return this.list;
	}

}

```

```yml
acme:
  list:
    - name: my name
      description: my description
---
spring:
  profiles: dev
acme:
  list:
    - name: my another name
```

如果 dev 没有激活, 启用默认值, list含有一个元素, 如果 dev 激活, 含有一个元素, description = null;(这里没有merge)

对于 Map

```java
@ConfigurationProperties("acme")
public class AcmeProperties {

	private final Map<String, MyPojo> map = new HashMap<>();

	public Map<String, MyPojo> getMap() {
		return this.map;
	}

}

```

```yml
acme:
  map:
    key1:
      name: my name 1
      description: my description 1
---
spring:
  profiles: dev
acme:
  map:
    key1:
      name: dev name 1
    key2:
      name: dev name 2
      description: dev description 2
```

如果 dev 没有激活, map 包含一个元素
如果 dev 激活, map包含2个元素, 元素1 key1:{name: dev name 1, description: my description 1}, 元素2 key2: {...}

这里混合了

### 属性 转换

可能和@ConfigurationPropertiesBinding 有关  ，这里Mark一下，后面完成。

自定义类型转换, 三种方式:

* 提供一个 ConversionService  bean(命名该bean为 conversionService)

* 定制 property editors (通过 CustomEditorConfigurer bean)

* 定制 Converters  (通过注解 @ConfigurationPropertiesBinding)

https://docs.spring.io/spring-boot/docs/2.0.2.RELEASE/reference/htmlsingle/#boot-features-external-config-conversion



# profiles

## 添加active-profiles

Spring profiles 给这种需求提供了实 ----> 根据不同的环境启用不同的属性配置文件

任何被@component、@@Configuration 标注的类，都能通过@Profile来实现限制何时被加载：

```java
@Configuration
@Profile("production")
public class ProductionConfiguration {

    // ...

}
```

当然，也有其他的方法：  

* 在属性配置文件中（如：application.properties）配置环境属性： `spring.profiles.active=dev,hsqldb`

* 命令行启动时，加参数：`--spring.profiles.active=dev,hsqldb`

* 但是，这些方法都是直接替换，有时候添加比替换更好，环境属性：`spring.profiles.include`提供了这种特性. 

* 也可通过Java API在app run()之前设置： `SpringApplication.setAdditionalProfiles(…​)  `

* 也可以通过`ConfigurableEnvironment interface.`(不常用)

下面看看例子:

当指定prod启用时，proddb 和 prodmq也会被启用  

```yml
---
my.property: fromyamlfile
---
spring.profiles: prod
spring.profiles.include:
  - proddb
  - prodmq
```

# springboot单元测试

https://www.cnblogs.com/aston/p/7259825.html

# 日志

Springboot 内部日志系统采用 Commons Logging，但是开放了日志接口，提供了这些日志的默认配置： Java Util Logging, Log4J2 and Logback；默认情况下，如果使用starters，Logback日志会被采用, 默认console输出, file输出可选

```properties
logging.path=/user/local/log
logging.level.com.favorites=DEBUG
logging.level.org.springframework.web=INFO
logging.level.org.hibernate=ERROR

```

## 输出配置

### 输出格式

默认的日志输出如下：  

    2014-03-05 10:57:51.112  INFO 45469 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet Engine: Apache Tomcat/7.0.52
    2014-03-05 10:57:51.253  INFO 45469 --- [ost-startStop-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
    2014-03-05 10:57:51.253  INFO 45469 --- [ost-startStop-1] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 1358 ms
    2014-03-05 10:57:51.698  INFO 45469 --- [ost-startStop-1] o.s.b.c.e.ServletRegistrationBean        : Mapping servlet: 'dispatcherServlet' to [/]
    2014-03-05 10:57:51.702  INFO 45469 --- [ost-startStop-1] o.s.b.c.embedded.FilterRegistrationBean  : Mapping filter: 'hiddenHttpMethodFilter' to: [/*]

有这些信息：  

* 日期和时间
* 日志级别，ERROR, WARN, INFO, DEBUG or TRACE.
* 进程ID
* “---”，表示后面是真正的日志信息
* “[xxx]”,线程名
* Logger名，通常是source class name
* 真正的日志信息

注: Logback does not have a FATAL level (it is mapped to ERROR)

### console控制台输出

By default ERROR, WARN and INFO level messages are logged. 如果希望debug级别日志也记录, ` java -jar myapp.jar --debug`(You can also specify debug=true in your application.properties.)

### 带颜色输出

需要设置 spring.output.ansi.enabled 。
see: https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#boot-features-logging-color-coded-output

### file文件输出

spring boot默认只输出日志到console, 如果希望输出到文件, 设置`logging.file` or `logging.path`属性到application.properties

| logging.file  | logging.path       | Example  | Description                                                                                                        |
|---------------|--------------------|----------|--------------------------------------------------------------------------------------------------------------------|
|               |                    |          | Console only logging.                                                                                              |
| Specific file |                    | my.log   | 写入到指定的文件                                                                                                   |
|               | Specific directory | /var/log | Writes `spring.log` to the specified directory.  |

Log files 达到 10 MB 自动另提一个文件记录, 通过`logging.file.max-size` 更改 file size

`logging.file.max-history=30`日志文件保存30天的

## 日志级别(Log Levels)

`logging.level.<logger-name>=<level>` where level is one of TRACE, DEBUG, INFO, WARN, ERROR, FATAL, or OFF

The root logger can be configured by using `logging.level.root`.

输出优先级: fatal > error > warn >  info > debug > TRACE

如果不配置, application.properties中已经默认配置了如下属性:

```yml
logging.level.root=WARN # root logger
logging.level.org.springframework.web=DEBUG
logging.level.org.hibernate=ERROR
```

## 自定义log configration

* springboot支持多种日志系统的自动配置, 当引入一个特定的日志lib, 这个日志系统会被自动激活,而日志系统的配置文件一般在classpath的根目录下, or 通过`logging.config` property指定日志的配置文件位置(如:logging.config=classpath:log/logback-spring.xml);

* 可以通过`org.springframework.boot.logging.LoggingSystem`这个system property来强制springboot使用某个日志系统, 属性值是` fully-qualified class name of a LoggingSystem implementation`, 值为`none`时, app的日志系统被禁用;

注: 日志系统在ApplicationContext 创建前就进行了初始化, 因此通过@PropertySources指定@Configuration配置文件不可行,  `System properties` and the conventional `Spring Boot external configuration files`则可以控制日志系统;

根据不同的日志系统, 如下的文件会被系统自动读取:

![](./SpringBoot文档翻译/Screenshot_2.png)

此外, 为了方便自定义log, 一些String Environment 被转为了 System property

Spring Environment|	System Property|	Comments
--|--|--
logging.exception-conversion-word|LOG_EXCEPTION_CONVERSION_WORD|The conversion word used when logging exceptions.
logging.file|LOG_FILE|If defined, it is used in the default log configuration.
logging.file.max-size|LOG_FILE_MAX_SIZE|Maximum log file size (if LOG_FILE enabled). (Only supported with the default Logback setup.)
logging.file.max-history|LOG_FILE_MAX_HISTORY|Maximum number of archive log files to keep (if LOG_FILE enabled). (Only supported with the default Logback setup.)
logging.path|LOG_PATH|If defined, it is used in the default log configuration.
logging.pattern.console|CONSOLE_LOG_PATTERN|The log pattern to use on the console (stdout). (Only supported with the default Logback setup.)
logging.pattern.dateformat|LOG_DATEFORMAT_PATTERN|Appender pattern for log date format. (Only supported with the default Logback setup.)
logging.pattern.file|FILE_LOG_PATTERN|The log pattern to use in a file (if LOG_FILE is enabled). (Only supported with the default Logback setup.)
logging.pattern.level|LOG_LEVEL_PATTERN|The format to use when rendering the log level (default %5p). (Only supported with the default Logback setup.)
PID|PID|The current process ID (discovered if possible and when not already defined as an OS environment variable).

在 spring-boot.jar 中查阅配置信息 [logback](https://github.com/spring-projects/spring-boot/tree/v2.0.2.RELEASE/spring-boot-project/spring-boot/src/main/resources/org/springframework/boot/logging/logback/defaults.xml) | [log4j 2](https://github.com/spring-projects/spring-boot/tree/v2.0.2.RELEASE/spring-boot-project/spring-boot/src/main/resources/org/springframework/boot/logging/log4j2/log4j2.xml)

## MDC特性

https://blog.csdn.net/n447194252/article/details/73864046
https://logging.apache.org/log4j/2.x/manual/thread-context.html

log4j 2中借助[MDC ] 特性将当前访问user加入日志: 覆盖`LOG_LEVEL_PATTERN`or Logback 中的`logging.pattern.level`, 举个 🌰:  

设置 `logging.pattern.level=user:%X{user} %5p`, 默认的日志格式包含一个 MDC entry  "user", 如果这个user存在, 打印如下:

```
2015-09-30 12:30:04.031 user:someone INFO 22174 --- [  nio-8080-exec-0] demo.Controller
Handling authenticated request
```

[logback利用mdc机制为日志增加traceId](https://blog.csdn.net/xingbaozhen1210/article/details/89230570)

## logback详细配置

### 依赖

ogback当前分成三个模块：logback-core,logback- classic和logback-access。

logback-core是其它两个模块的基础模块。logback-classic是log4j的一个 改良版本。此外logback-classic完整实现SLF4J API, logback-access访问模块与Servlet容器集成提供通过Http来访问日志的功能。

单独在spring项目中使用logback需要导入: classic包, core包, slf4j-api的包

在springboot中引入 `spring-boot-starter-web` starter即可使用logback

注意: 🐖logback的依赖不要混在一个项目, 如果引入第三方包, 注意一定要 排除`slf4j-log4j12.jar`

### logback-spring.xml

https://www.cnblogs.com/cb0327/p/5759441.html

logback-spring.xml

另外一份配置, 存在这里, 存疑

```xml
<?xml version="1.0" encoding="UTF-8"?>

<configuration scan="true">
    <include resource="org/springframework/boot/logging/logback/base.xml"/>

<!-- The FILE and ASYNC appenders are here as examples for a production configuration -->
<!--
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>logFile.%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>90</maxHistory>
        </rollingPolicy>
        <encoder>
            <charset>utf-8</charset>
            <Pattern>%d %-5level [%thread] %logger{0}: %msg%n</Pattern>
        </encoder>
    </appender>

    <appender name="ASYNC" class="ch.qos.logback.classic.AsyncAppender">
        <queueSize>512</queueSize>
        <appender-ref ref="FILE"/>
    </appender>
-->

    <logger name="com.paasit.pai.core.rest" level="#logback.loglevel#"/>

    <logger name="io.github.jhipster" level="DEBUG"/>

    <logger name="javax.activation" level="WARN"/>
    <logger name="javax.mail" level="WARN"/>
    <logger name="javax.xml.bind" level="WARN"/>
    <logger name="ch.qos.logback" level="WARN"/>
    <logger name="com.codahale.metrics" level="WARN"/>
    <logger name="com.netflix" level="WARN"/>
    <logger name="com.netflix.discovery" level="INFO"/>
    <logger name="com.ryantenney" level="WARN"/>
    <logger name="com.sun" level="WARN"/>
    <logger name="com.zaxxer" level="WARN"/>
    <logger name="io.undertow" level="WARN"/>
    <logger name="io.undertow.websockets.jsr" level="ERROR"/>
    <logger name="org.apache" level="WARN"/>
    <logger name="org.apache.catalina.startup.DigesterFactory" level="OFF"/>
    <logger name="org.bson" level="WARN"/>
    <logger name="org.hibernate.validator" level="WARN"/>
    <logger name="org.hibernate" level="WARN"/>
    <logger name="org.hibernate.ejb.HibernatePersistence" level="OFF"/>
    <logger name="org.springframework" level="WARN"/>
    <logger name="org.springframework.web" level="WARN"/>
    <logger name="org.springframework.security" level="WARN"/>
    <logger name="org.springframework.cache" level="WARN"/>
    <logger name="org.thymeleaf" level="WARN"/>
    <logger name="org.xnio" level="WARN"/>
    <logger name="springfox" level="WARN"/>
    <logger name="sun.rmi" level="WARN"/>
    <logger name="liquibase" level="WARN"/>
    <logger name="LiquibaseSchemaResolver" level="INFO"/>
    <logger name="sun.net.www" level="INFO"/>
    <logger name="sun.rmi.transport" level="WARN"/>

    <contextListener class="ch.qos.logback.classic.jul.LevelChangePropagator">
        <resetJUL>true</resetJUL>
    </contextListener>

    <root level="#logback.loglevel#">
        <appender-ref ref="CONSOLE"/>
    </root>

</configuration>

```

### 看一个具体日志需求

https://blog.csdn.net/wohaqiyi/article/details/72853962

* 日志需要控制台打印和文件打印两种。
* 其中文件打印按照日志级别分别保存到各自的文件里。
* 文件日志每天一个日志，并且保存30天。
* 文件日志可以自由指定保存路径、打印格式等。
* 控制台打印可指定打印格式，并且自由增加删除某些日志。

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<configuration scan="true" scanPeriod="60 seconds">  
    <!-- 都说spring boot使用日志需要引入这个，但是我引入了之后总是打印两份日志，所以我去除了，并不影响使用 -->
    <!-- <include resource="org/springframework/boot/logging/logback/base.xml"/> -->
    <!-- 控制台设置 -->  
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">  
        <encoder>  
            <pattern>${CONSOLE_LOG_PATTERN}</pattern>  
        </encoder>  
    </appender>  
    <!-- INFO -->  
    <appender name="infoAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">  
        <!-- 文件路径 ，注意LOG_PATH是默认值，
            它的配置对应application.properties里的logging.path值-->  
        <file>${LOG_PATH}/info/info.log</file>  
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">  
            <!-- 文件名称 -->  
            <fileNamePattern>info/info-%d{yyyy-MM-dd}.log  
            </fileNamePattern>  
            <!-- 文件最大保存历史数量 -->  
            <MaxHistory>30</MaxHistory>  
        </rollingPolicy>  
        <encoder>  
            <pattern>${FILE_LOG_PATTERN}</pattern>  
        </encoder>  
        <filter class="ch.qos.logback.classic.filter.LevelFilter">  
            <level>INFO</level>  
            <onMatch>ACCEPT</onMatch>    
            <onMismatch>DENY</onMismatch>    
        </filter>  
    </appender>

    <!-- DEBUG -->  
    <appender name="debugAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">  
        <!-- 文件路径 ，注意LOG_PATH是默认值，
            它的配置对应application.properties里的logging.path值-->  
        <file>${LOG_PATH}/debug/debug.log</file>  
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">  
            <!-- 文件名称 -->  
            <fileNamePattern>debug/debug-%d{yyyy-MM-dd}.log</fileNamePattern>  
            <!-- 文件最大保存历史数量 -->  
            <MaxHistory>30</MaxHistory>  
        </rollingPolicy>  
        <encoder>  
            <pattern>${FILE_LOG_PATTERN}</pattern>  
        </encoder>  
        <filter class="ch.qos.logback.classic.filter.LevelFilter">  
            <level>DEBUG</level>  
            <onMatch>ACCEPT</onMatch>    
            <onMismatch>DENY</onMismatch>    
        </filter>  
    </appender> 
     <!-- WARN -->  
    <appender name="warnAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">  
        <!-- 文件路径 ，注意LOG_PATH是默认值，
            它的配置对应application.properties里的logging.path值-->   
        <file>${LOG_PATH}/warn/warn.log</file>  
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">  
            <!-- 文件名称 -->  
            <fileNamePattern>warn/warn-%d{yyyy-MM-dd}.log  
            </fileNamePattern>  
            <!-- 文件最大保存历史数量 -->  
            <MaxHistory>30</MaxHistory>  
        </rollingPolicy>  
        <encoder>  
            <pattern>${FILE_LOG_PATTERN}</pattern>  
        </encoder>  
        <filter class="ch.qos.logback.classic.filter.LevelFilter">  
            <level>WARN</level>  
            <onMatch>ACCEPT</onMatch>    
            <onMismatch>DENY</onMismatch>    
        </filter>  
    </appender> 

    <!-- ERROR -->  
    <appender name="errorAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">  
        <!-- 文件路径 ，注意LOG_PATH是默认值，
            它的配置对应application.properties里的logging.path值-->  
        <file>${LOG_PATH}/error/error.log</file>  
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">  
            <!-- 文件名称 -->  
            <fileNamePattern>error/error-%d{yyyy-MM-dd}.log  
            </fileNamePattern>  
            <!-- 文件最大保存历史数量 -->  
            <MaxHistory>30</MaxHistory>  
        </rollingPolicy>  
        <encoder>  
            <pattern>${FILE_LOG_PATTERN}</pattern>  
        </encoder>  
        <filter class="ch.qos.logback.classic.filter.LevelFilter">  
            <level>ERROR</level>  
            <onMatch>ACCEPT</onMatch>    
            <onMismatch>DENY</onMismatch>    
        </filter>  
    </appender>
      <logger name="org.springframework" additivity="false">
        <level value="ERROR" />
        <appender-ref ref="STDOUT" />
        <appender-ref ref="errorAppender" />
    </logger>

    <!-- 由于启动的时候，以下两个包下打印debug级别日志很多 ，所以调到ERROR-->
    <logger name="org.apache.tomcat.util" additivity="false">
        <level value="ERROR"/>
        <appender-ref ref="STDOUT"/>
        <appender-ref ref="errorAppender"/>
    </logger>

    <!-- 默认spring boot导入hibernate很多的依赖包，启动的时候，会有hibernate相关的内容，直接去除 -->
    <logger name="org.hibernate.validator" additivity="false">
        <level value="ERROR"/>
        <appender-ref ref="STDOUT"/>
        <appender-ref ref="errorAppender"/>
    </logger>
    <root level="DEBUG">
         <appender-ref ref="STDOUT"/>  
         <appender-ref ref="infoAppender"/>
         <appender-ref ref="debugAppender"/>
          <appender-ref ref="warnAppender"/>
         <appender-ref ref="errorAppender"/>
    </root> 
</configuration>  
```

application.properties里配置:

```props
logging.path=d:/logs/springBoot
logging.config=classpath:logback-spring.xml
# 以下是控制台打印日志格式设置，注意在logback-spring.xml里用CONSOLE_LOG_PATTERN才能获取
logging.pattern.console=[%d{yyyy-MM-dd HH:mm:ss}] -- [%-5p]: [%c] -- %m%n
# 以下是文件打印日志格式设置，注意在logback-spring.xml里用FILE_LOG_PATTERN才能获取到
logging.pattern.file=[%d{yyyy-MM-dd HH:mm:ss}] -- [%-5p]: [%c] -- %m%n

```

### 排除不需要的日志

比如不需要下面三行

```
[2017-06-05 19:14:23] -- [INFO ]: [org.I0Itec.zkclient.ZkClient] -- zookeeper state changed (SyncConnected)
[2017-06-05 19:14:23] -- [DEBUG]: [org.I0Itec.zkclient.ZkClient] -- Leaving process event
[2017-06-05 19:14:23] -- [DEBUG]: [org.I0Itec.zkclient.ZkClient] -- State is SyncConnected
```

如下配置即可

```xml
<!-- 
- name表示日志的打印位置，从上边可以看出是来自该类。 
- additivity设置为false表示该日志打印设置（控制台打印还是文件打印等具体设置）不会向根root标签传递，也就是说该logger里怎么设置的那就会怎么打印，跟root无关。 
- level value=’error’表示将该类日志级别设置为error级才会打印。 
- 最后两行表示error级时会打印控制台和error文件同时打印日志。
 -->
<logger name="org.I0Itec.zkclient.ZkClient" additivity="false">
        <level value="ERROR" />
        <appender-ref ref="STDOUT" />
        <appender-ref ref="errorAppender" />
</logger>
```

## logback扩展特性

LogBack有很多拓展特性, 可以在`logback-spring.xml`中定义

推荐使用名称`logback-spring.xml`而不是`logback.xml`, 因为后者在启动时就会加载，加载非常早，会导致配置文件中的springboot标签不识别。所以推荐命名`xxxxx-spring.xml`形式

### logback的多环境配置

以下配置, 处于`<configuration>`标签

```xml
<springProfile name="staging">
    <!-- configuration to be enabled when the "staging" profile is active -->
</springProfile>

<springProfile name="dev, staging">
    <!-- configuration to be enabled when the "dev" or "staging" profiles are active -->
</springProfile>

<springProfile name="!production">
    <!-- configuration to be enabled when the "production" profile is not active -->
</springProfile>
```

实例: 通过application.properties文件中 `spring.profiles.active=test `来指定logback-spring.xml 中使用哪一段来记录日志

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- 从高到地低 OFF 、 FATAL 、 ERROR 、 WARN 、 INFO 、 DEBUG 、 TRACE 、 ALL -->  
<!-- 日志输出规则  根据当前ROOT 级别，日志输出时，级别高于root默认的级别时  会输出 -->  
<!-- 以下  每个配置的 filter 是过滤掉输出文件里面，会出现高级别文件，依然出现低级别的日志信息，通过filter 过滤只记录本级别的日志--> 
<configuration>
    <!-- 文件输出格式 -->
    <property name="PATTERN"
        value="%-12(%d{yyyy-MM-dd HH:mm:ss.SSS}) |-%-5level [%thread] %c [%L] -| %msg%n" />
    <!-- 文件路径 -->
    <!--<property name="FILE_PATH" value="D:/DevData/kmcmsLogs" /> -->

    <!-- 开发环境 -->
    <springProfile name="dev">
        <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
            <encoder>
                <pattern>${PATTERN}</pattern>
            </encoder>
        </appender>

        <logger name="com.km.controller" level="debug" />

        <root level="info">
            <appender-ref ref="CONSOLE" />
        </root>
    </springProfile>

    <!-- 测试环境 -->
    <springProfile name="test">
        <appender name="ROLLING"
            class="ch.qos.logback.core.rolling.RollingFileAppender">
            <!-- 文件路径 -->
            <file>D:/DevData/kmAppletLogs/kmapplet.log</file>
            <rollingPolicy
                class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
                <!-- rollover daily -->
                <fileNamePattern>D:/DevData/kmAppletLogs/kmapplet_%d{yyyy-MM-dd}.%i.log
                </fileNamePattern>
                <!-- each file should be at most 100MB, keep 60 days worth of history, 
                    but at most 20GB -->
                <maxFileSize>1MB</maxFileSize>
                <maxHistory>60</maxHistory>
                <totalSizeCap>10MB</totalSizeCap>
            </rollingPolicy>
            <!-- <layout class="ch.qos.logback.classic.PatternLayout"> <pattern>${PATTERN}</pattern> 
                </layout> -->
            <encoder>
                <pattern>${PATTERN}</pattern>
            </encoder>
        </appender>

        <root level="DEBUG">
            <appender-ref ref="ROLLING" />
        </root>
    </springProfile>

    <!-- 生产环境 -->
    <springProfile name="prod">
        <appender name="ROLLING"
            class="ch.qos.logback.core.rolling.RollingFileAppender">
            <!-- 文件路径 -->
            <file>/ftp/private/kmAppletLogs/kmapplet.log</file>
            <rollingPolicy
                class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
                <!-- rollover daily -->
                <fileNamePattern>/ftp/private/kmAppletLogs/kmapplet_%d{yyyy-MM-dd}.%i.log
                </fileNamePattern>
                <!-- each file should be at most 10MB, keep 30 days worth of history, 
                    but at most 1GB -->
                <!-- 每个日志文件最大10MB, 保留30天的日志文件, 但是最多总文件大小为 5GB -->
                <maxFileSize>10MB</maxFileSize>
                <maxHistory>30</maxHistory>
                <totalSizeCap>5GB</totalSizeCap>
            </rollingPolicy>
            <!-- <layout class="ch.qos.logback.classic.PatternLayout"> <pattern>${PATTERN}</pattern> 
                </layout> -->
            <encoder>
                <pattern>${PATTERN}</pattern>
            </encoder>
        </appender>

        <!--这里控制日志输出级别 -->
        <root level="DEBUG">
            <appender-ref ref="ROLLING" />
        </root>
    </springProfile>

</configuration>
```

### 配置中使用Environment properties

logback配置文件中可以获取application.properties中的属性

这里用到了`<springProperty>`, 和`<property>`用法类似

```xml
<!-- 
scope: context 希望将这个属性作用域在context级别, 
scope: local  仅仅存储在本地范围
 -->
<springProperty scope="context" name="fluentHost" source="myapp.fluentd.host"
        defaultValue="localhost"/>
<appender name="FLUENT" class="ch.qos.logback.more.appenders.DataFluentAppender">
    <remoteHost>${fluentHost}</remoteHost>
    ...
</appender>
```

注: 这里会默认采用松绑定匹配, 指定了source="aa.bb.cc", 那么,属性"aaBbCc, aa-bb-cc, 等等" 也会被匹配上.



# 开发 web app

一般会引入`spring-boot-starter-web` module. 此时不必引入 spring-boot-starter 了， 前者已经包含了

也可以引入`spring-boot-starter-webflux` 开发响应式web app

## 引入WebJars

### 什么是webjars

以 Jar 包的形式来使用前端的各种框架、组件。

WebJars 是将客户端（浏览器）资源（JavaScript，Css等）打成 Jar 包文件，以对资源进行统一依赖管理。WebJars 的 Jar 包部署在 Maven 中央仓库上。

### 为什么使用webjars

对于 JavaScript，Css 等前端资源包，我们只能采用拷贝到 webapp 下的方式，这样做就无法对这些资源进行依赖管理。那么 WebJars 就提供给我们这些前端资源的 Jar 包形势，我们就可以进行依赖管理。

### 如何使用 webjars

在 https://www.webjars.org/ 查找需要框架的maven依赖

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>vue</artifactId>
    <version>2.5.16</version>
</dependency>
```

thymeleaf页面中引入：

```html
<link th:href="@{/webjars/bootstrap/3.3.6/dist/css/bootstrap.css}" rel="stylesheet"></link>

```

## Gradle 构建工具

```gradle
buildscript {
    repositories {
        maven { url "http://repo.spring.io/libs-snapshot" }
        mavenLocal()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:1.3.6.RELEASE")
    }
}

apply plugin: 'java'  //添加 Java 插件, 表明这是一个 Java 项目
apply plugin: 'spring-boot' //添加 Spring-boot支持
apply plugin: 'war'  //添加 War 插件, 可以导出 War 包
apply plugin: 'eclipse' //添加 Eclipse 插件, 添加 Eclipse IDE 支持, Intellij Idea 为 "idea"

war {
    baseName = 'favorites'
    version =  '0.1.0'
}

sourceCompatibility = 1.7  //最低兼容版本 JDK1.7
targetCompatibility = 1.7  //目标兼容版本 JDK1.7

repositories {     //  Maven 仓库
    mavenLocal()        //使用本地仓库
    mavenCentral()      //使用中央仓库
    maven { url "http://repo.spring.io/libs-snapshot" } //使用远程仓库
}
 
dependencies {   // 各种 依赖的jar包
    compile("org.springframework.boot:spring-boot-starter-web:1.3.6.RELEASE")
    compile("org.springframework.boot:spring-boot-starter-thymeleaf:1.3.6.RELEASE")
    compile("org.springframework.boot:spring-boot-starter-data-jpa:1.3.6.RELEASE")
    compile group: 'mysql', name: 'mysql-connector-java', version: '5.1.6'
    compile group: 'org.apache.commons', name: 'commons-lang3', version: '3.4'
    compile("org.springframework.boot:spring-boot-devtools:1.3.6.RELEASE")
    compile("org.springframework.boot:spring-boot-starter-test:1.3.6.RELEASE")
    compile 'org.webjars.bower:bootstrap:3.3.6'
	compile 'org.webjars.bower:jquery:2.2.4'
    compile("org.webjars:vue:1.0.24")
	compile 'org.webjars.bower:vue-resource:0.7.0'

}

bootRun {
    addResources = true
}

```

## 开发filter

Spring Boot 自动添加了 OrderedCharacterEncodingFilter 和 HiddenHttpMethodFilter，并且我们可以自定义 Filter

- 实现 Filter 接口，实现 Filter 方法
- 添加@Configuration 注解，将自定义Filter加入过滤链

```java
public class MyFilter implements Filter

@Bean
public FilterRegistrationBean testFilterRegistration() {

    FilterRegistrationBean registration = new FilterRegistrationBean();
    registration.setFilter(new MyFilter());
    registration.addUrlPatterns("/*");
    registration.addInitParameter("paramName", "paramValue");
    registration.setName("MyFilter");
    registration.setOrder(1);
    return registration;
}

```

## Spring Web MVC框架

相关的注解: @Controller or @RestController, @RequestMapping

### 自动配置Springmvc

Spring boot 已经自动配置了如下特性：  

* Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.
* 对静态资源的支持，包括对WebJars的支持.
* 自动注册 Converter, GenericConverter, Formatter beans.
* Support for `HttpMessageConverters` (see below).
* 自动注册` MessageCodesResolver` (see below).
* Static `index.html` support.
* Custom Favicon support (see below).
* Automatic use of a `ConfigurableWebBindingInitializer` bean (see below).

如果希望添加额外的mvc特性（如：interceptors, formatters, view controllers etc.），创建@Configuration类继承WebMvcConfigurerAdapter(继承WebMvcConfigurer 亦可貌似?存疑),但是不能有@EnableWebMvc，例如：给app添加跨域访问支持   

解决跨域问题:

```java
@Configuration  
public class CorsConfig extends WebMvcConfigurerAdapter {  

    @Override  
    public void addCorsMappings(CorsRegistry registry) {  
        registry.addMapping("/**")  
                .allowedOrigins("*")  
                .allowCredentials(true)  
                .allowedMethods("OPTIONS", "GET", "POST", "DELETE", "PUT")
                .maxAge(3600);  
    }
}  
```

如果希望自己定制`RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter` or `ExceptionHandlerExceptionResolver`，可以声明一个`WebMvcRegistrationsAdapter`实例来提供这些组件  

如果希望完全控制Springmvc，可以提供自己的@Configration类，用@EnableWebMvc标记

### http消息转换器(HttpMessageConverters)

springmvc使用HttpMessageConverters接口来转换http request和response，框架已经默认实现了一些特性，如：object自动转json(Jackson lib)或xml(Jackson lib, 如果没有Jackson, 则Jaxb)。  String默认使用utf-8编码; 

如果需要自定义消息转换器，可以利用Spring boot的HttpMessageConverters类：  

```java
@Configuration
public class MyConfiguration {

    @Bean
    public HttpMessageConverters customConverters() {
        HttpMessageConverter<?> additional = ...//这些HttpMessageConverter 会被放入HttpMessageConverters 的一个列表中，呈现在上下文中, 默认的list会被覆盖
        HttpMessageConverter<?> another = ...
        return new HttpMessageConverters(additional, another);
    }

}
```



### MessageCodesResolver

Spring boot会使用MessageCodesResolver来生成error code，环境属性`spring.mvc.message-codes-resolver.format` 需要配置为 `PREFIX_ERROR_CODE` or `POSTFIX_ERROR_CODE`  , 具体可设置那些值, 见`DefaultMessageCodesResolver.Format`枚举类

### 静态资源

存疑, https://docs.spring.io/spring-boot/docs/2.0.2.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-static-content

* Spring boot的规范中，静态资源默认放在classpath下的 /static (or /public or /resources or /META-INF/resources），使用ResourceHttpRequestHandler 处理，所以我们可以添加自己的WebMvcConfigurerAdapter覆盖addResourceHandlers方法来定制自己的资源目录  

* 或者使用environment property：`spring.mvc.static-path-pattern=/resources/**` or `spring.resources.static-locations=# a list of directory locations`(如果使用后面这个属性，index.html将会使用你自己的目录下的index.html), 默认下, `spring.resources.static-locations=classpath:/META-INF/resources/,classpath:/resources/,classpath:/static/,classpath:/public/`, 优先级从从高到低

* 在默认规范中，有一个目录：classpath: /webjars/**  是为 Webjars content 留的。所有/webjars/**中的资源将从jar 文件中读取，如果他们以符合Webjars 规范的方式打包了的话。  

* 不要使用src/main/webapp目录，如果应用被打包成jar包。因为这个目录只有在war包中才会工作，而且会被大部分构建工具忽略

* 对于引用第三方静态资源, 可以做到版本无感知的效果, 比如 "/webjars/jquery/jquery.min.js" results in "/webjars/jquery/x.y.z/jquery.min.js". where x.y.z is the Webjar version.需要引入`webjars-locator-core `依赖

* 版本无感知, 还一种方法

    ```
    spring.resources.chain.strategy.fixed.enabled=true
    spring.resources.chain.strategy.fixed.paths=/js/lib/
    spring.resources.chain.strategy.fixed.version=v12
    ```

    使用以上策略，JavaScript模块加载器加载"/js/lib/"下的文件时会使用一个固定的版本策略"/v12/js/lib/mymodule.js"

* 静态资源的缓存清除

    ```
    spring.resources.chain.strategy.content.enabled=true
    spring.resources.chain.strategy.content.paths=/**
    ```

    原理实际上是将内容hash添加到URLs中，比如`<link href="/css/spring-2a2d595e6ed9a0b24f027f2b63b134d6.css"/>`

    注 实现该功能的是ResourceUrlEncodingFilter，它在模板运行期会重写资源链接，Thymeleaf，Velocity和FreeMarker会自动配置该filter，JSP需要手动配置。其他模板引擎还没自动支持，不过你可以使用ResourceUrlProvider自定义模块宏或帮助类

### 欢迎页

先找 index.html在静态资源目录中, 没找到然后在模板文件中找index

### 自定义网站图标

Spring boot会自动搜寻网站图标`favicon.ico`，在如下目录：  

* 静态资源目录
* classpath根目录

### 请求匹配和资源导航(Path Matching and Content Negotiation)

Spring MVC可以通过查看请求路径并将其与应用程序中定义的映射进行匹配，将传入的HTTP请求映射到处理程序, 比如： @GetMapping("/projects/spring-boot")...

springboot默认禁用后缀匹配， 比如请求路径“"GET /projects/spring-boot.json"” 无法匹配到 @GetMapping("/projects/spring-boot")

存疑， https://docs.spring.io/spring-boot/docs/2.0.2.RELEASE/reference/htmlsingle/#boot-features-spring-mvc-pathmatch

### ConfigurableWebBindingInitializer

Spring MVC使用WebBindingInitializer为每个特殊的请求初始化相应的WebDataBinder，如果你创建自己的ConfigurableWebBindingInitializer @Bean，Spring Boot会自动配置Spring MVC使用它

### 模板引擎

Springboot为这些template engine提供自动配置：  

    FreeMarker
    Groovy
    Thymeleaf
    Mustache

模板文件默认放置在src/main/resources/templates  

### 全局异常处理

Springboot默认提供了一个错误处理机制，所有的异常会匹配/error. 但是这明显不够。我们需要自定义。

为了完全取代默认设置, 可以实现`ErrorController`接口, 并将这个bean注册; 或者直接注册一个 `ErrorAttributes `类型的bean;

一种方法是：创建controller实现ErrorController接口；BasicErrorController可以作为errorcontroller的基类，添加一个方法用@requestmappong（produces =xxx）标记；这种方法尤其在这种情况下有用：当你想为新的content-type（默认的一般是text/html）添加一个Handler

另种方法：（推荐）

#### 如果返回视图

```java
/**
 * 全局异常处理类
 *
 * @version: 1.0.0
 * @author: 肖雨
 * @date: 2017年8月23日 下午11:32:23
 */
@ControllerAdvice
public class ExceptionAdvice {

    private static final String DEFAULT_ERROR_VIEW = "error";
    
    @ExceptionHandler(Exception.class)
    public ModelAndView defaultErrorHandler(HttpServletRequest req, Exception e) {
        ModelAndView mav = new ModelAndView();
        mav.addObject("exception", e);
        mav.addObject("url", req.getRequestURL());
        mav.setViewName(DEFAULT_ERROR_VIEW);
        return mav;
    }
    
    //后面可以继续添加，匹配不同异常
}

```

```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8" />
    <title>统一异常处理</title>
</head>
<body>
    <h1>Error Handler</h1>
    <div th:text="${url}"></div>
    <div th:text="${exception.message}"></div>
</body>
</html>
```

#### 如果返回json  

```java
@ControllerAdvice(basePackageClasses = FooController.class)//basePackageClasses属性表示服务特定的controller
public class FooControllerAdvice extends ResponseEntityExceptionHandler {

    @ExceptionHandler(YourException.class)// 为特定的exception服务
    @ResponseBody
    ResponseEntity<?> handleControllerException(HttpServletRequest request, Throwable ex) {
        HttpStatus status = getStatus(request);
        return new ResponseEntity<>(new CustomErrorType(status.value(), ex.getMessage()), status);
    }

    private HttpStatus getStatus(HttpServletRequest request) {
        Integer statusCode = (Integer) request.getAttribute("javax.servlet.error.status_code");
        if (statusCode == null) {
            return HttpStatus.INTERNAL_SERVER_ERROR;
        }
        return HttpStatus.valueOf(statusCode);
    }

}
```

#### 定制自己的错误页面

根据返回的 error code 匹配到不同页面  

（匹配到404）

    src/
    +- main/
        +- java/
        |   + <source code>
        +- resources/
            +- public/
                +- error/
                |   +- 404.html
                +- <other public assets>

（匹配所有以5开头的 error code）

    src/
    +- main/
        +- java/
        |   + <source code>
        +- resources/
            +- templates/
                +- error/
                |   +- 5xx.ftl
                +- <other templates>

对于更复杂的情况，可以添加一个bean实现ErrorViewResolver接口：  

```java
public class MyErrorViewResolver implements ErrorViewResolver {

    @Override
    public ModelAndView resolveErrorView(HttpServletRequest request, HttpStatus status, Map<String, Object> model) {
        // Use the request or status to optionally return a ModelAndView
        return ...
    }

}
```

也可以使用Spring MVC特性，比如@ExceptionHandler方法和@ControllerAdvice，ErrorController将处理所有未处理的异常

存疑， https://docs.spring.io/spring-boot/docs/2.0.2.RELEASE/reference/htmlsingle/#boot-features-error-handling-custom-error-pages

#### 不使用springmvc

但是，如果不用Springmvc，该如何匹配error pages呢 ，通过ErrorPageRegistrar接口直接注册ErrorPages  

```java
@Bean
public ErrorPageRegistrar errorPageRegistrar(){
    return new MyErrorPageRegistrar();
}

// ...

private static class MyErrorPageRegistrar implements ErrorPageRegistrar {

    @Override
    public void registerErrorPages(ErrorPageRegistry registry) {
        registry.addErrorPages(new ErrorPage(HttpStatus.BAD_REQUEST, "/400"));
    }

}
```
这里需要注意的是：如果你注册一个ErrorPage，该页面需要被一个Filter处理（在一些非Spring web框架中很常见，比如Jersey，Wicket），那么该Filter需要明确注册为一个ERROR分发器（dispatcher），如：

```java
@Bean
public FilterRegistrationBean myFilter() {
    FilterRegistrationBean registration = new FilterRegistrationBean();
    registration.setFilter(new MyFilter());
    ...
    registration.setDispatcherTypes(EnumSet.allOf(DispatcherType.class));
    return registration;
}
```

### 配置允许跨域访问

只需要在Spring Boot应用的controller方法上注解@CrossOrigin，并添加CORS配置。通过注册一个自定义addCorsMappings(CorsRegistry)方法的WebMvcConfigurer bean可以指定全局CORS配置

```java
@Configuration  
public class CorsConfig extends WebMvcConfigurerAdapter {  

    @Override  
    public void addCorsMappings(CorsRegistry registry) {  
        registry.addMapping("/**")  
                .allowedOrigins("*")  
                .allowCredentials(true)  
                .allowedMethods("OPTIONS", "GET", "POST", "DELETE", "PUT")
                .maxAge(3600);  
    }
}  

```

或者：  

```java
@Configuration//标明这是一个配置类
public class MyConfiguration {

    @Bean//@bean表示返回值被Spring管理
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurerAdapter() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/api/**");
            }
        };
    }
}
```

## Spring WebFlux Framework

https://docs.spring.io/spring-boot/docs/2.0.2.RELEASE/reference/htmlsingle/#boot-features-webflux

## JAX-RS and Jersey 

https://docs.spring.io/spring-boot/docs/1.5.6.RELEASE/reference/htmlsingle/#boot-features-jersey

## 嵌入式servlet容器

Springboot内嵌 Tomcat, Jetty, and Undertow servers，只需要引入合适的Starter获取一个完全配置好的实例即可，内嵌服务器默认监听8080端口的HTTP请求

如果使用的内嵌server是Tomcat，需要自定义tmpwatch配置 or 设置server.tomcat.basedir  

### 自定义内嵌容器

https://docs.spring.io/spring-boot/docs/2.0.2.RELEASE/reference/htmlsingle/#boot-features-customizing-embedded-containers

通过程序的方式定制：https://docs.spring.io/spring-boot/docs/2.0.2.RELEASE/reference/htmlsingle/#boot-features-programmatic-embedded-container-customization



### Servlets, Filters, and listeners

注册Servlets, Filters, and listeners，可以有如下方法：  

* 使用 Spring beans
* 通过扫描 servlet 组件

默认情况下，如果context只包括一个servlet，将会匹配 /, 如果有多个servlet bean，bean name将会作为前缀，filters将会匹配 /*

如果这些惯例无法满足需求，`ServletRegistrationBean`, `FilterRegistrationBean` and `ServletListenerRegistrationBean` classes 可以提供更完全的控制  

扫描Servlets, Filters和listeners：

当使用一个内嵌容器时，通过@ServletComponentScan可以启用对注解@WebServlet，@WebFilter和@WebListener类的自动注册。

### servlet上下文初始化

内嵌的servlet容器不会执行Servlet 3.0+ javax.servlet.ServletContainerInitializer接口，或者Spring’s org.springframework.web.WebApplicationInitializer接口；这是有意的设计，为的是防止被设计在war中运行的第三方库阻断Springboot的运行；  

如果希望初始化，使用这种方法：  

注册一个bean，实现`org.springframework.boot.context.embedded.ServletContextInitializer`接口， onStartup方法可以获取ServletContext，如果需要的话可以轻松用来适配一个已存在的WebApplicationInitializer



### 对jsp的限制

当使用内嵌servlet容器运行Spring Boot应用时（并打包成一个可执行的存档archive），容器对JSP的支持有一些限制：

* Tomcat只支持war的打包方式，不支持可执行jar。

* Jetty只支持war的打包方式。

* Undertow不支持JSPs。

* 创建的自定义error.jsp页面不会覆盖默认的error handling视图。

jsp实例：https://github.com/spring-projects/spring-boot/blob/v2.0.2.RELEASE/spring-boot-samples/spring-boot-sample-web-jsp/src/main/resources/application.properties

# 安全(Security)

如果引入了  Spring Security 依赖， app默认就被保护了， 添加方法级别的安全自定义设置使用@EnableGlobalMethodSecurity

默认的 UserDetailsService 提供一个 user 实例，  name为“user”， password为随机， 打印在info级别日志， 确保org.springframework.boot.autoconfigure.security的日志级别为info， 可以通过 `spring.security.user.name `和` spring.security.user.password`修改默认的username, password

## OAuth2

如果添加了spring-security-oauth2依赖，你可以利用自动配置简化认证（Authorization）或资源服务器（Resource Server）的设置






# 实践

## 接口开发

* @restcontroller注解相当于@controller+@responsebody
* 使用了模板引擎就不要用@restcontroller了
* @getmapping，@postmapping。。。

## 使用模板引擎

### themeleaf

* 依赖spring-boot-starter-thymeleaf

### freemarker

* 依赖：spring-boot-starter-freemarker

### velocity

* 依赖：spring-boot-starter-velocity

## 集成swagger2 

* 引入依赖 springfox-swagger2 https://mvnrepository.com/artifact/io.springfox/springfox-swagger2
* 各个操作接口上：@ApiOperation，@ApiImplicitParam
* swagger2配置类，和APP启动类同级，@Configuration，@EnableSwagger2
* 访问：http://localhost:8080/swagger-ui.html
* 配置mvc时候， 如果继承了WebMvcConfigurationSupport， 需要重新注入资源文件， 否则访问不到swagger-ui.html;([ref](https://blog.csdn.net/zjcjava/article/details/78064264))

## 统一异常处理

* 创建@ControllerAdvice类，@ExceptionHandler(xxxException.class)方法

## 使用JdbcTemplate

* 依赖：spring-boot-starter-jdbc
* JdbcTemplate通过@autowired注入直接使用

## 使用Spring-data-jpa

* 依赖spring-boot-starter-data-jpa
* 通过env属性配置表生成策略：spring.jpa.properties.hibernate.hbm2ddl.auto=create-drop
* 创建xxxRepository接口继承JpaRepository接口，方法无需实现，特殊的可通过@Query("from User u where u.name=:name")注解
* 通过@Autowired注入直接使用
* @Entity标注实体，和表名对应；@Column标注字段

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/test
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.jdbc.Driver

spring.jpa.properties.hibernate.hbm2ddl.auto=update # 第一次加载 hibernate 时根据 model 类会自动建立起表的结构（前提是先建立好数据库），以后加载 hibernate 时根据 model 类自动更新表结构
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect # 存储引擎 innodb
spring.jpa.show-sql= true

```



## 多数据源配置

首先需要配置文件中两个数据源的连接信息

### mybatis

https://github.com/imcloudfloating/rw-separate-spring-boot-starter, https://www.cnblogs.com/cloudfloating/archive/2019/09/17/11536531.html //todo

https://www.cnblogs.com/wuyoucao/p/10965903.html 数据库集群

https://www.luvnaxx.com/post/%E6%8A%80/spring-boot/multiple-data-source/

### JdbcTemplate

* 数据源@Configuration类
* 注册数据源，@Bean(name = "secondaryDataSource")注解 注册bean，name指定bean名字；@Primary指定默认数据源；@ConfigurationProperties匹配指定属性
* 注册jdbctemplate，@Bean(name = "primaryJdbcTemplate")
* @Autowired+@Qualifier("primaryJdbcTemplate")注入使用


### Spring-data-jpa

* 数据源@Configuration类
* 数据源注册

待完成

##使用redis作为数据库

* 依赖：spring-boot-starter-redis
* 添加配置文件 

Spring Boot 1.0 默认使用的是 Jedis 客户端，2.0 替换成 Lettuce

```props
# Redis数据库索引（默认为0）
spring.redis.database=0  
# Redis服务器地址
spring.redis.host=localhost
# Redis服务器连接端口
spring.redis.port=6379  
# Redis服务器连接密码（默认为空）
spring.redis.password=
# 连接池最大连接数（使用负值表示没有限制） 默认 8
spring.redis.lettuce.pool.max-active=8
# 连接池最大阻塞等待时间（使用负值表示没有限制） 默认 -1
spring.redis.lettuce.pool.max-wait=-1
# 连接池中的最大空闲连接 默认 8
spring.redis.lettuce.pool.max-idle=8
# 连接池中的最小空闲连接 默认 0
spring.redis.lettuce.pool.min-idle=0
```


## @Async异步调用

* 任务类@Component
* 方法上@Async
* 返回值Future<String>，AsyncResult<>

## log4j支持

* 依赖：spring-boot-starter-log4j
* 排除掉Springboot自己的日志系统：logback

        <dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
			<exclusions>
				<exclusion>
					<groupId>org.springframework.boot</groupId>
					<artifactId>spring-boot-starter-logging</artifactId>
				</exclusion>
			</exclusions>
		</dependency>

* properties文件

        # LOG4J配置
        log4j.rootCategory=INFO, stdout, file, errorfile
        log4j.category.com.didispace=DEBUG, didifile # com.didispace包下
        log4j.logger.error=errorfile

        # 控制台输出
        log4j.appender.stdout=org.apache.log4j.ConsoleAppender
        log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
        log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSS} %5p %c{1}:%L - %m%n

        # root日志输出
        log4j.appender.file=org.apache.log4j.DailyRollingFileAppender # 按天分文件
        log4j.appender.file.file=logs/all.log
        log4j.appender.file.DatePattern='.'yyyy-MM-dd
        log4j.appender.file.layout=org.apache.log4j.PatternLayout
        log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSS} %5p %c{1}:%L - %m%n

        # error日志输出
        log4j.appender.errorfile=org.apache.log4j.DailyRollingFileAppender
        log4j.appender.errorfile.file=logs/error.log
        log4j.appender.errorfile.DatePattern='.'yyyy-MM-dd
        log4j.appender.errorfile.Threshold = ERROR
        log4j.appender.errorfile.layout=org.apache.log4j.PatternLayout
        log4j.appender.errorfile.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSS} %5p %c{1}:%L - %m%n

        # com.didispace下的日志输出，
        log4j.appender.didifile=org.apache.log4j.DailyRollingFileAppender
        log4j.appender.didifile.file=logs/my.log
        log4j.appender.didifile.DatePattern='.'yyyy-MM-dd
        log4j.appender.didifile.layout=org.apache.log4j.PatternLayout
        log4j.appender.didifile.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSS} %5p %c{1}:%L ---- %m%n

* 使用

        private Logger logger = Logger.getLogger(getClass());

## 多环境下的日志配置

* log4j.properties中使用占位符${xxx}
* 在不同的application-profiles.properties中指定占位符的值
* 命令行启动：java -jar xxx.jar --spring.profiles.active=prod；或者直接更改application.properties中的spring.profiles.active

## aop统一处理请求日志

* 依赖：spring-boot-starter-aop
* 需要使用CGLIB来实现AOP的时候，需要配置spring.aop.proxy-target-class=true，不然默认使用的是标准Java的实现
* @Order(i)标明切面类的优先级，i越小，优先级越高；@Component，@Aspect

## 缓存支持

## 消息服务

## 监控管理









