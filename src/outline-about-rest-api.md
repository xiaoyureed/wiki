---
title: RESTful api 及 其他webService技术
date: 2017-08-18 19:29:09
tags: [restful]
categories: java web
---

http://www.ruanyifeng.com/blog/2014/05/restful_api.html,

java的rmi使用https://blog.csdn.net/zhaowen25/article/details/45443951, 

xml-rpc使用http://www.cnblogs.com/flyoung2008/archive/2011/11/12/2246282.html

http://www.ruanyifeng.com/blog/2016/04/cors.html

https://kalasearch.cn/blog/rest-api-best-practices/
<!--more-->

<!-- TOC -->

- [Web Service](#web-service)
- [什么是restful架构？](#什么是restful架构)
- [RESTful最大的优势](#restful最大的优势)
- [典型的设计错误](#典型的设计错误)
- [设计合理的restful API](#设计合理的restful-api)
- [接口安全设计](#接口安全设计)
- [实践](#实践)
  - [设计统一的响应结构](#设计统一的响应结构)
  - [实现对象序列化](#实现对象序列化)
  - [处理异常](#处理异常)
  - [支持参数验证](#支持参数验证)
  - [解决跨域问题](#解决跨域问题)
  - [API访问权](#api访问权)

<!-- /TOC -->


# Web Service

服务器如何向客户端提供服务

常用的方法有:

RPC 所谓的远程过程调用 (面向方法, 强调动作): 像调用本地服务(方法)一样调用服务器的服务(方法), 比如xml-rpc, json-rpc
SOA 所谓的面向服务的架构(面向消息), 和具体语言无关
REST 所谓的 Representational state transfer 表现层状态转移 (面向资源, 强调资源)
Hessian 见[ref](https://blog.csdn.net/weigao_easy/article/details/52351042)

[webservice关注点](#webservice关注点)

# 什么是restful架构？

* 一个架构符合rest原则则称之为restful架构

* rest原则（资源的“表现层状态转化”）

    * 每个URI代表一个资源（资源可以使一段文本，一张图片，甚至是一个动作），URI中只能出现名词

    * client 和server间传递资源的某种表现层（文本表现层有text，json，xml；图片表现层有JPG，png等）

    * 客户端通过http四个动词对服务器资源进行操作，实现“表现层状态转化”

    * 无状态原则：即http的无状态，指的是服务器没有保存客户端的状态信息，客户端必须每次带上自己的状态去请求服务器。比如登陆，我们使用Cookie通过客户端保持登陆状态。有利于实现负载均衡（因为请求自身带有状态信息，任何server均可以处理client请求）

    > 如：如果遥控器与空调之间是有状态的，假设你离开空调接收范围调整了遥控器温度，变成19，那回到范围内你按一次升高一度，基于原先温度状态，遥控器给空调发送一个”提高1度“的指令，就会出现遥控器提高到20，而空调变成21。如果要空间与空调之前是无状态的，假设你离开空调接收范围调整了遥控器温度，变成19，那回到范围内你按一次升高一度，遥控器给空调发送一个”设定温度值20“，这样两者最终还是相同的值。

    * 如果一个动作是http动词表现不了的，应该把这个动作变为一个资源，以名词形式通过URI发布出来

# RESTful最大的优势

要求从资源的角度对系统进行拆分, 而不是像以前从动作/操作的角度

我们平时搞系统是这样的：

1. 有新建用户功能
1. 新建用户需要一个URL
1. 往这个URL发送的数据要定义好
1. 开始写后端和前端

这种方式当然可以架构出一个系统，甚至是一个好系统，但是偶尔会有些问题

* 操作之间是会有关联，你的设计容易变成“第2个操作要求第1个操作进行过”，这种关系多起来系统就乱了
* URL设计会缺乏一致性
* 操作通常被认为是有副作用（Side Effect）的，所以很少有人基于操作去设计缓存之类的东西

遵循Restful风格后, 系统会获得这些好处:

* 各个资源虽然可能有关联，但依旧是能够简单地切掉这些关联导致相互独立的，所以不会有非常乱的耦合性
* 对资源的操作就这么几种，所以很容易设计一致的URL
* 我们明白对资源的读操作是无副作用的，所以能方便的接入缓存

# 典型的设计错误

* URI中包含动词

    URI中不应该有动词，因为URI表示一个资源，只能是名词，动词放在http协议中，如：  

    ```
    /posts/show/1--------错误    
    /posts/1-----------正确，在http中用get表示show方法
    ```

    如果某个动作四个http动词表现不了，如：transfer 转账,用户1向用户2转账500元  

    ```
    POST /accounts/1/transfer/500/to/2-------错误

    ----------
    
    POST /transaction HTTP/1.1  
    Host: 127.0.0.1

    from=1&to=2&amount=500.00  ------------正确，资源可以是一种服务，将动作 作为一种资源对外提供
    ```

    再比如: 登录场景. 我们可以将登录这个动作资源化, 将登录视作一种资源

    把“登录”作为一个资源，所以登录是POST /logins，退出是DELETE /logins; 当然还有其他的解释比如, 登录登出视作session的增删, 而session是资源


* URI中加入资源的版本号
    
    API的版本号可以放进URI中，资源的版本号则不要放入  

    不同的版本是同一个资源的不同表现形式，如下是错误的：

    ```
    http://www.example.com/app/1.0/foo
    http://www.example.com/app/1.1/foo
    http://www.example.com/app/2.0/foo
    ```

    正确的做法是采用同一个URI，版本号在http的请求头accept字段中标明：

    ```
    Accept: vnd.example-com.foo+json; version=1.0
    Accept: vnd.example-com.foo+json; version=1.1
    Accept: vnd.example-com.foo+json; version=2.0
    ```


# 设计合理的restful API

主要考虑如下几个要素：

* 协议

    http 或者 https  

* 身份认证

    API的身份认证应该使用OAuth 2.0框架

* 数据格式

    json避免使用xml

* 域名

    尽量部署在专有域名下，如：https://api.example.com  
    如果确定API很简单，不会有进一步扩展，可以考虑放在主域名下：https://example.org/api/

* 版本

    应该将API版本号放入URL：https://api.example.com/v1/  
    or 
    将版本信息放在http header中，但是不如放在URL中直观

* 路径（endpoint)

    只含有名词，一般为复数形式，如：  

    ```
    https://api.example.com/v1/zoos
    https://api.example.com/v1/animals
    https://api.example.com/v1/employees
    ```

* http动词

    常用的有如下5种：  

    ```
    GET（SELECT）：从服务器取出资源（一项或多项）。
    POST（CREATE）：在服务器新建一个资源。
    PUT（UPDATE）：在服务器更新资源（客户端提供改变后的完整资源）。
    PATCH（UPDATE）：在服务器更新资源（客户端提供改变的属性）。
    DELETE（DELETE）：从服务器删除资源。
    ```

    还有两个不常用的：----[ref](https://stackoverflow.com/questions/6660019/restful-api-methods-head-options)
    
    <img src="Snipaste_2018-05-07_22-35-43.png" align="left">

    ```
    HEAD: 获取某些header, 和get类似, 但是response没有body, 确定缓存哪些信息, 哪些缓存信息需要更新
    OPTIONS：对于某个resources可以使用哪些http动词
    ```


    下面是一些例子

    ```
    GET /zoos：列出所有动物园
    POST /zoos：新建一个动物园
    GET /zoos/ID：获取某个指定动物园的信息
    PUT /zoos/ID：更新某个指定动物园的信息（提供该动物园的全部信息）
    PATCH /zoos/ID：更新某个指定动物园的信息（提供该动物园的部分信息）
    DELETE /zoos/ID：删除某个动物园
    GET /zoos/ID/animals：列出某个指定动物园的所有动物
    DELETE /zoos/ID/animals/ID：删除某个指定动物园的指定动物
    ```

* 过滤信息（filtering）

    如果记录数量很多，服务器不可能都将它们返回给用户。API应该提供参数，过滤返回结果。下面是一些常见的参数：

    ```
    ?limit=10：指定返回记录的数量
    ?offset=10：指定返回记录的开始位置。
    ?page=2&per_page=100：指定第几页，以及每页的记录数。
    ?sortby=name&order=asc：指定返回结果按照哪个属性排序，以及排序顺序。
    ?animal_type_id=1：指定筛选条件
    ```

* 状态码（status code）

    常用的如下：  

    ```
    200 OK - [GET]：服务器成功返回用户请求的数据，该操作是幂等的（Idempotent）。
    201 CREATED - [POST/PUT/PATCH]：用户新建或修改数据成功。
    202 Accepted - [*]：表示一个请求已经进入后台排队（异步任务）
    204 NO CONTENT - [DELETE]：用户删除数据成功。
    400 INVALID REQUEST - [POST/PUT/PATCH]：用户发出的请求有错误，服务器没有进行新建或修改数据的操作，该操作是幂等的。
    401 Unauthorized - [*]：表示用户没有权限（令牌、用户名、密码错误）。
    403 Forbidden - [*] 表示用户得到授权（与401错误相对），但是访问是被禁止的。
    404 NOT FOUND - [*]：用户发出的请求针对的是不存在的记录，服务器没有进行操作，该操作是幂等的。
    406 Not Acceptable - [GET]：用户请求的格式不可得（比如用户请求JSON格式，但是只有XML格式）。
    410 Gone -[GET]：用户请求的资源被永久删除，且不会再得到的。
    422 Unprocesable entity - [POST/PUT/PATCH] 当创建一个对象时，发生一个验证错误。
    500 INTERNAL SERVER ERROR - [*]：服务器发生错误，用户将无法判断发出的请求是否成功。
    ```

* 错误处理（error handling）

    如果状态码是4xx，就应该向用户返回出错信息。一般来说，返回的信息中将error作为键名，出错信息作为键值即可。

    ```
    {
        error: "Invalid API key"
    }
    ```

* 返回结果

    针对不同的操作，服务器的返回结果应该符合下面的规范：

    ```
    GET /collection：返回资源对象的列表（数组）
    GET /collection/resource：返回单个资源对象
    POST /collection：返回新生成的资源对象
    PUT /collection/resource：返回完整的资源对象
    PATCH /collection/resource：返回完整的资源对象
    DELETE /collection/resource：返回一个空文档
    ```


* Hypermedia API

# 接口安全设计

[1](https://www.cnblogs.com/jurendage/p/12653865.html)

https://blog.csdn.net/angryjiji/article/details/99476028

对于安全敏感的接口, 需要作出特殊的安全设计, 当然首先就是推荐使用 https

-   token - 一般为 一个 UUID, 用于标识接口调用者的身份, 减少用户名和密码的传输次数  

    一般情况下客户端(接口调用方)需要先向服务器端申请一个接口调用的账号，服务器会给出一个appId和多个 (appkey, appSecret)对, key用于参数签名使用 (同一个 app_id 可以对应多个 app_key+app_secret, 这样 平台就可以分配你不一样的权限, 比如 app_key1 + app_secect1 只有只读权限 但是 app_key2+app_secret2 有读写权限)

    server 生成 token 后, 将 token 作为 key, 将其他关联信息作为 value 存入 redis, 当一个 req 过来, 会带着 token, 到 redis 查这个 token, 如果存在, 则放行, 不存在, 则拦截

    细分为:

    -   接口令牌 (api token) - 访问无需登录的接口, 如 登录, 注册 需要拿appId、timestamp和sign来换，sign=加密(appId + timestamp + key)
    -   用户令牌 (user token) - 用于访问需要登录后调用的接口 需要拿用户名和密码来换

-   sign - 用于参数签名，防止参数被非法篡改, 比如涉及到转账的接口

    sign的值一般是将所有非空参数按照升续排序然后+token+key+timestamp+nonce拼接在一起，然后使用某种加密算法进行加密，作为接口中的一个参数sign来传递，也可以将sign放到请求头中 (nonce 为随机数, 客户端随机生成, 目的是增加sign签名的多变性, 一般是数字和字母的组合，6位长度)

    当服务器调用接口前会按照sign的规则重新计算出sign的值然后和接口传递的sign参数的值做比较, 相同则放行, 否则拦截

    server 也可以返回一个 sign 给 client, 用于防止 response 被篡改

- 防重复提交(幂等)

    当请求第一次提交时将sign作为key保存到redis，并设置超时时间，超时时间和Timestamp中设置的差值相同; 这样, 当同一个请求第二次访问时会先检测redis是否存在该sign，如果存在则证明重复提交了，接口就不再继续调用了。如果sign在缓存服务器中因过期时间到了，而被删除了，此时当这个url再次请求服务器时，因token的过期时间和sign的过期时间一直，sign过期也意味着token过期，那样同样的url再访问服务器会因token错误会被拦截掉

    timestamp - 是客户端调用接口时对应的当前时间戳，用于防止DoS攻击; 每次调用接口时接口都会判断服务器当前系统时间和接口中传的的timestamp的差值，如果这个差值超过某个设置的时间(假如5分钟)，那么这个请求将被拦截掉，防止 ddos 攻击






API KEY跟API SECRET KEY: secert key是不向外公开的，用其加入各个请求参数和api key，组合成一个字符串，然后做摘要运算，生成的摘要一起发送给请求服务器，这样服务器端验证当前摘要是否合法，可以得知当前api key对应的secert key是否合法了 (https://blog.csdn.net/weixin_41964962/article/details/105383504), http://www.hello1010.com/api-sign


# 实践

## 设计统一的响应结构

json格式如下：

```json
{
    "meta": {//元数据
        "success": true,
        "message": "ok"
    },
    "data": ...//数据
}
```

相应的response类：

```java
public class Response {

    private static final String OK = "ok";
    private static final String ERROR = "error";

    private Meta meta;
    private Object data;

    public Response success() {
        this.meta = new Meta(true, OK);
        return this;
    }

    public Response success(Object data) {
        this.meta = new Meta(true, OK);
        this.data = data;
        return this;
    }

    public Response failure() {
        this.meta = new Meta(false, ERROR);
        return this;
    }

    public Response failure(String message) {
        this.meta = new Meta(false, message);
        return this;
    }

    public Meta getMeta() {
        return meta;
    }

    public Object getData() {
        return data;
    }

    public class Meta {

        private boolean success;
        private String message;

        public Meta(boolean success) {
            this.success = success;
        }

        public Meta(boolean success, String message) {
            this.success = success;
            this.message = message;
        }

        public boolean isSuccess() {
            return success;
        }

        public String getMessage() {
            return message;
        }
    }
}
```

## 实现对象序列化

Spring MVC 已经为我们提供了这类序列化特性，只需在 Controller 的方法参数中使用 @RequestBody 注解定义需要反序列化的参数即可  

若需要对 Controller 的方法返回值进行序列化，则需要在该返回值上使用 @ResponseBody 注解来定义  

```java
@Controller
public class AdvertiserController {

    @RequestMapping(value = "/advertiser/{id}", method = RequestMethod.GET)
    public @ResponseBody Response getAdvertiser(@PathVariable("id") String advertiserId) {
        ...
    }
}
```

由于经常会使用到 @ResponseBody 注解，所以 Spring 提供了一个名为 @RestController 的注解来取代以上的 @Controller 注解，这样我们就可以省略返回值前面的 @ResponseBody 注解了，但参数前面的 @RequestBody 注解是无法省略的  

```java
@RestController
public class AdvertiserController {

    @RequestMapping(value = "/advertiser", method = RequestMethod.POST)
    public Response createAdvertiser(@RequestBody AdvertiserParam advertiserParam) {
        ...
    }

    @RequestMapping(value = "/advertiser/{id}", method = RequestMethod.GET)
    public Response getAdvertiser(@PathVariable("id") String advertiserId) {
        ...
    }
}
```

使用 Jackson 来提供 JSON 的序列化操作，在 Spring 配置文件中只需添加以下配置即可：

```xml
<mvc:annotation-driven>
    <mvc:message-converters>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
    </mvc:message-converters>
</mvc:annotation-driven>
```

但是这是最基础的json序列化操作，如果需要排除值为空属性、进行缩进输出、将驼峰转为下划线、进行日期格式化等，这又如何实现呢？  

第一步：扩展 Jackson 提供的 ObjectMapper 类  

```java
public class CustomObjectMapper extends ObjectMapper {

    private boolean camelCaseToLowerCaseWithUnderscores = false;
    private String dateFormatPattern;

    public void setCamelCaseToLowerCaseWithUnderscores(boolean camelCaseToLowerCaseWithUnderscores) {
        this.camelCaseToLowerCaseWithUnderscores = camelCaseToLowerCaseWithUnderscores;
    }

    public void setDateFormatPattern(String dateFormatPattern) {
        this.dateFormatPattern = dateFormatPattern;
    }

    public void init() {
        // 排除值为空属性
        setSerializationInclusion(JsonInclude.Include.NON_NULL);
        // 进行缩进输出
        configure(SerializationFeature.INDENT_OUTPUT, true);
        // 将驼峰转为下划线
        if (camelCaseToLowerCaseWithUnderscores) {
            setPropertyNamingStrategy(PropertyNamingStrategy.CAMEL_CASE_TO_LOWER_CASE_WITH_UNDERSCORES);
        }
        // 进行日期格式化
        if (StringUtil.isNotEmpty(dateFormatPattern)) {
            DateFormat dateFormat = new SimpleDateFormat(dateFormatPattern);
            setDateFormat(dateFormat);
        }
    }
}
```

第二步：将 CustomObjectMapper 注入到 MappingJackson2HttpMessageConverter 中，Spring 配置如下：  

```xml
<bean id="objectMapper" class="com.xxx.api.json.CustomObjectMapper" init-method="init">
    <property name="camelCaseToLowerCaseWithUnderscores" value="true"/>
    <property name="dateFormatPattern" value="yyyy-MM-dd HH:mm:ss"/>
</bean>

<mvc:annotation-driven>
    <mvc:message-converters>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper" ref="objectMapper"/>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

## 处理异常

使用 AOP 技术，编写一个全局的异常处理切面类，用它来统一处理所有的异常行为;定义一个类，并通过 @ControllerAdvice 注解将其标注即可，同时需要使用 @ResponseBody 注解表示返回值可序列化为 JSON 字符串    

在运行时从上往下依次调用每个异常处理方法，匹配当前异常类型是否与 @ExceptionHandler 注解所定义的异常相匹配，若匹配，则执行该方法，同时忽略后续所有的异常处理方法，最终会返回经 JSON 序列化后的 Response 对象

```java
@ControllerAdvice
@ResponseBody
public class ExceptionAdvice {

    /**
     * 400 - Bad Request
     */
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    @ExceptionHandler(HttpMessageNotReadableException.class)
    public Response handleHttpMessageNotReadableException(HttpMessageNotReadableException e) {
        logger.error("参数解析失败", e);
        return new Response().failure("could_not_read_json");
    }

    /**
     * 405 - Method Not Allowed
     */
    @ResponseStatus(HttpStatus.METHOD_NOT_ALLOWED)
    @ExceptionHandler(HttpRequestMethodNotSupportedException.class)
    public Response handleHttpRequestMethodNotSupportedException(HttpRequestMethodNotSupportedException e) {
        logger.error("不支持当前请求方法", e);
        return new Response().failure("request_method_not_supported");
    }

    /**
     * 415 - Unsupported Media Type
     */
    @ResponseStatus(HttpStatus.UNSUPPORTED_MEDIA_TYPE)
    @ExceptionHandler(HttpMediaTypeNotSupportedException.class)
    public Response handleHttpMediaTypeNotSupportedException(Exception e) {
        logger.error("不支持当前媒体类型", e);
        return new Response().failure("content_type_not_supported");
    }

    /**
     * 500 - Internal Server Error
     */
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    @ExceptionHandler(Exception.class)
    public Response handleException(Exception e) {
        logger.error("服务运行异常", e);
        return new Response().failure(e.getMessage());
    }
}
```

## 支持参数验证

提供一个 @Valid 注解来定义 AdvertiserParam 参数，并在 AdvertiserParam 类中通过 @NotEmpty 注解来定义 advertiserName 属性, like this:  

```java
@RestController
public class AdvertiserController {

    @RequestMapping(value = "/advertiser", method = RequestMethod.POST)
    public Response createAdvertiser(@RequestBody @Valid AdvertiserParam advertiserParam) {//这里的@Valid需要jar包支持，pom引入
        ...
    }
}

public class AdvertiserParam {

    @NotEmpty
    private String advertiserName;

    private String description;

    // 省略 getter/setter 方法
}

//定义@NotEmpty 注解类
@Documented
@Target({ElementType.FIELD, ElementType.PARAMETER})
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = NotEmptyValidator.class)//指定一个验证器类
public @interface NotEmpty {
//以下三个属性为必须，是规范所要求
    String message() default "not_empty";

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};
}

//定义验证器类
public class NotEmptyValidator implements ConstraintValidator<NotEmpty, String> {//NotEmpty是验证注解类型，String是需要验证的参数类型

    @Override
    public void initialize(NotEmpty constraintAnnotation) {
    }

    @Override
    public boolean isValid(String value, ConstraintValidatorContext context) {
        return StringUtil.isNotEmpty(value);
    }
}

```

这里的 @Valid 注解实际上是 Validation Bean 规范提供的注解，该规范已由 Hibernate Validator 框架实现，因此需要添加以下 Maven 依赖到 pom.xml 文件中:  

```xml
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>${hibernate-validator.version}</version>
</dependency>
```

最后，需要在 Spring 配置文件中开启校验，需添加如下配置

```xml
<bean class="org.springframework.validation.beanvalidation.MethodValidationPostProcessor"/>
```

最后，需要在全局异常处理类中添加参数验证处理方法，代码如下：

```java
@ControllerAdvice
@ResponseBody
public class ExceptionAdvice {

    /**
     * 400 - Bad Request
     */
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    @ExceptionHandler(ValidationException.class)
    public Response handleValidationException(ValidationException e) {
        logger.error("参数验证失败", e);
        return new Response().failure("validation_exception");
    }
}
```

## 解决跨域问题

整个架构包含两个应用，前端应用提供纯静态的 HTML 页面，后端应用发布 REST API，前端需要通过 AJAX 调用后端发布的 REST API，然而 AJAX 是不支持跨域访问的  

浏览器访问某个 server  中的 js 资源, js 中向另外不同源的 server 发送了 ajax 请求, 这是浏览器不允许的, 也就是不支持跨域访问


如, A网页设置的 Cookie，B网页不能打开，除非这两个网页"同源"   , 防止恶意的网站窃取数据

同源必须符合一下三点相同：

1. 协议相同
2. 域名相同
3. 端口相同

如果判定为非同源，有三点行为受限制：

    （1） Cookie、LocalStorage 和 IndexDB 无法读取。
    （2） DOM 无法获得。
    （3） AJAX 请求不能发送。



解决这个问题需要前后端互相配合，后端的工作只要能将 CORS 响应头写入 response 对象中即可, SpringBoot配合前端实现跨域 - https://blog.csdn.net/saytime/article/details/74937204

csrf 跨站请求伪造攻击
https://www.cnblogs.com/Infernal/p/11282443.html
https://www.cnblogs.com/lailailai/p/4528092.html
TODO

## API访问权

由于 REST 是无状态的，后端应用发布的 REST API 可在用户未登录的情况下被任意调用，这显然是不安全的  

实现思路：  

1. 登录成功后，server生成一个token，JVM中保存一份，同时返回客户端一份
2. 客户端中将token写入cookie，每次请求都带上
3. 提供一个aop切面，拦截所有请求，判断token有效性
4. 登出时，清楚cookie中的token，服务端的token可设置过期时间，自行移除

如果需要做到分布式集群，建议基于 Redis 提供一个实现类  
如下是一个简单的实现：  

```java
public interface TokenManager {

    String createToken(String username);

    boolean checkToken(String token);
}

//简单版的实现
public class DefaultTokenManager implements TokenManager {

    private static Map<String, String> tokenMap = new ConcurrentHashMap<>();

    @Override
    public String createToken(String username) {
        String token = CodecUtil.createUUID();
        tokenMap.put(token, username);
        return token;
    }

    @Override
    public boolean checkToken(String token) {
        return !StringUtil.isEmpty(token) && tokenMap.containsKey(token);
    }
}

//切面类
public class SecurityAspect {

    private static final String DEFAULT_TOKEN_NAME = "X-Token";

    private TokenManager tokenManager;
    private String tokenName;

    public void setTokenManager(TokenManager tokenManager) {
        this.tokenManager = tokenManager;
    }

    public void setTokenName(String tokenName) {
        if (StringUtil.isEmpty(tokenName)) {
            tokenName = DEFAULT_TOKEN_NAME;
        }
        this.tokenName = tokenName;
    }

    public Object execute(ProceedingJoinPoint pjp) throws Throwable {
        // 从切点上获取目标方法
        MethodSignature methodSignature = (MethodSignature) pjp.getSignature();
        Method method = methodSignature.getMethod();
        // 若目标方法忽略了安全性检查，则直接调用目标方法
        if (method.isAnnotationPresent(IgnoreSecurity.class)) {
            return pjp.proceed();
        }
        // 从 request header 中获取当前 token
        String token = WebContext.getRequest().getHeader(tokenName);
        // 检查 token 有效性
        if (!tokenManager.checkToken(token)) {
            String message = String.format("token [%s] is invalid", token);
            throw new TokenException(message);
        }
        // 调用目标方法
        return pjp.proceed();
    }
}
```

使切面生效，添加如下Spring配置：

```xml
<bean id="securityAspect" class="com.xxx.api.security.SecurityAspect">
    <property name="tokenManager" ref="tokenManager"/>
    <property name="tokenName" value="X-Token"/>
</bean>

<aop:config>
    <aop:aspect ref="securityAspect">
        <aop:around method="execute" pointcut="@annotation(org.springframework.web.bind.annotation.RequestMapping)"/>
    </aop:aspect>
</aop:config>
```

最后，在 web.xml 中添加允许的 X-Token 响应头，配置如下：
```xml
<init-param>
    <param-name>allowHeaders</param-name>
    <param-value>Content-Type,X-Token</param-value>
</init-param>
```
