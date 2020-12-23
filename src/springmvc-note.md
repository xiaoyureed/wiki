---
title: Spring MVC
tags:
  - mvc
date: 2014-01-16 17:42:41
categories: java web
---

<div align="center">
基于mvc模式的web框架, spring的一个模块，它与spring集成无需中间整合包。
</div>
<!--more-->

<!-- TOC -->

- [什么是mvc](#什么是mvc)
- [springmvc和struts2区别](#springmvc和struts2区别)
- [quickstart](#quickstart)
- [springmvc.xml配置](#springmvcxml配置)
    - [视图解析器](#视图解析器)
    - [映射器 Mapping](#映射器-mapping)
    - [适配器 Adapter](#适配器-adapter)
    - [控制器 Controller](#控制器-controller)
    - [注解开发](#注解开发)
        - [Spring3.1前后区别](#spring31前后区别)
        - [配置](#配置)
- [Controller详解](#controller详解)
    - [单个 controller 中写多个业务方法](#单个-controller-中写多个业务方法)
    - [数据传递方式](#数据传递方式)
        - [收集页面的参数](#收集页面的参数)
        - [收集数组参数](#收集数组参数)
        - [手机List参数](#手机list参数)
        - [传递参数给页面](#传递参数给页面)
- [拦截器interceptor](#拦截器interceptor)
    - [什么是interceptor](#什么是interceptor)
    - [interceptor和filter对比](#interceptor和filter对比)
    - [怎么使用interceptor](#怎么使用interceptor)
        - [通过自定义web配置](#通过自定义web配置)
        - [通过注解](#通过注解)
        - [自定义注解拦截器](#自定义注解拦截器)
    - [HandlerMethod和MethodHandle](#handlermethod和methodhandle)
    - [通过Java动态代理实现拦截器功能](#通过java动态代理实现拦截器功能)
- [限定请求方式GET/POST](#限定请求方式getpost)
- [日期转换器 编码转换器](#日期转换器-编码转换器)
- [@InitBind解决字符串转日期类型](#initbind解决字符串转日期类型)
- [结果的转发和重定向](#结果的转发和重定向)
- [返回json, @ResponseBody](#返回json-responsebody)
- [静态资源放行](#静态资源放行)
- [RequestContextHolder获取request,response](#requestcontextholder获取requestresponse)
- [统一异常捕获](#统一异常捕获)

<!-- /TOC -->

# 什么是mvc

<img src="1.png"><img src="3.png"><img src="2.png">


# springmvc和struts2区别

* springmvc的入口是一个servlet，即前端控制器，例如：*.action; struts2入口是一个filter过虑器，即前端过滤器，例如：/*
   
* springmvc是基于方法开发，传递参数是通过方法形参，可以设计为单例; struts2是基于类开发，传递参数是通过类的属性，只能设计为多例
   
* springmvc通过参数解析器是将request对象内容进行解析成方法形参，将响应数据和页面封装成ModelAndView对象，最后又将模型数据通过request对象传输到页面; struts采用值栈存储请求和响应的数据，通过OGNL存取数据

# quickstart

依赖: spring的基本jar外, 还需要spring-webmvc ([spring-web vs spring-webmvc](https://stackoverflow.com/questions/13533700/maven-dependency-spring-web-vs-spring-webmvc))


配置前端控制器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app >
  <servlet>
  	<servlet-name>springmvc</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	
  	<!-- 指定springmvc.xml配置文件的路径，若不指定，默认读取WEB-INF下的springmvc-servlet.xml;
        这里文件名称的springmvc由<servlet-name>xxxxx</servlet-name>指定 -->
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:springmvc.xml</param-value>
  	</init-param>
  </servlet>
  <servlet-mapping>
  	<servlet-name>springmvc</servlet-name>
  	<url-pattern>*.action</url-pattern>
    <!-- 
    url-patern配置:
    第一种: *.action, 以[.action]结尾的访问由DispatcherServlet进行解析
    第二种: /, 所有访问都通过DispatcherServlet解析, 对于静态资源需要配置不让DS解析, 使用这种配置可以实现RESTful风格的url
    第三种: /*, 这样配置不对, 因为如果最终要转发到一个jsp, 仍然会由DS解析jsp地址, 不能根据jsp找到Handler, 会报错
     -->
  </servlet-mapping>
 
</web-app>

```

springmvc的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xmlns:task="http://www.springframework.org/schema/task"  
	xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-3.1.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.1.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.1.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.1.xsd
		http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task-3.0.xsd">

<!-- 第一个控制器；name值用于URL访问，URL：http://localhost:8080/项目名/queryItems1.do -->
	<bean name="/queryItems1.do" class="com.xy.controller.Controller1"></bean>
		
<!-- 第二个控制器 -->
	<bean name="/queryItems2.do" class="com.xy.controller.Controller2"></bean>
	
<!-- 第三个控制器，通过注解配置，这里使用组件扫描器加载控制器 -->
	<context:component-scan base-package="com.xy.controller"></context:component-scan>
	
<!-- 映射器(框架已经完成，没有特殊需求则可省略) -->  
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>  
<!-- 适配器(框架。。同上) -->  
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean>  
<!-- 视图解析器(框架。。。同上) -->  
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"></bean>

</beans>

```

如果基于注解开发, 映射器, 适配器, 视图解析器可省略, 实际框架默认加上了:

```xml
<!-- 基于注解的映射器(可选) -->
<bean class="org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping"/>

<!-- 基于注解的适配器(可选) -->
<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter"/>

<!-- 视图解析器(可选) -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"/>

```


开发处理器Controller

```java
/**
Springmvc开发处理器方式有多种，主要三种：
    实现HttpRequestHandler接口、
    实现Controller接口、
    使用注解开发

*/
//继承HttpRequestHandler
public class Controller1 implements HttpRequestHandler {

	@Override
	public void handleRequest(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {

		List<Item> itemList = new ArrayList<Item>();
		
		Item item1 = new Item();
		Item item2 = new Item();
		item1.setName("item1");
		item1.setId("1");
		item2.setName("item2");
		item2.setId("2");
		itemList.add(item2);
		itemList.add(item1);
		
		request.setAttribute("items", itemList);
		request.getRequestDispatcher("WEB-INF/jsp/test.jsp").forward(request, response);
		
	}

}

//继承Controller接口
public class Controller2 implements Controller {

	@Override
	public ModelAndView handleRequest(HttpServletRequest arg0, HttpServletResponse arg1) throws Exception {
		
		List<Item> itemList = new ArrayList<Item>();
		
		Item item1 = new Item();
		Item item2 = new Item();
		item1.setName("item1");
		item1.setId("1");
		item2.setName("item2");
		item2.setId("2");
		itemList.add(item2);
		itemList.add(item1);
		
		ModelAndView mv = new ModelAndView();
		mv.addObject("items", itemList); //相当于放入request域中
		mv.setViewName("/WEB-INF/jsp/test2.jsp");
		
		return mv;
	}

}


//使用注解
@Controller
public class Controller3 {

	@RequestMapping("/queryItem3")//这里无论加不加.action，访问地址均为：http://127.0.0.1:8080/springmvc-test/queryItem3.action
	public ModelAndView queryItem() {
		List<Item> itemList = new ArrayList<Item>();
		
		Item item1 = new Item();
		Item item2 = new Item();
		item1.setName("item1");
		item1.setId("1");
		item2.setName("item2");
		item2.setId("2");
		itemList.add(item2);
		itemList.add(item1);
		
		ModelAndView mv = new ModelAndView();
		mv.addObject("items", itemList); //相当于放入request域中
		mv.setViewName("/WEB-INF/jsp/test2.jsp");
		
		return mv;
	}
}
```

jsp页面:

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>

	springmvc demo<br/>
<%--
	<%=request.getAttribute("items") %>
 --%>
 
 ${requestScope.items[0].name}
 

</body>
</html>
```

# springmvc.xml配置

## 视图解析器 

解析视图逻辑名对应的真实路径

```xml

<!-- 如果需要html的视图解析器, 可以使用jsp的, 也可以使用Freemarker的视图解析器 -->
<bean id="htmlviewResolver"   
    class="org.springframework.web.servlet.view.freemarker.FreeMarkerViewResolver">   
    <property name="suffix" value=".html" /><!-- html是没有prefix前缀属性的 -->
    <!--order越小，优先级越高 -->
    <property name="order" value="0"></property>  
    <property name="contentType" value="text/html;charset=UTF-8"></property>          
</bean>
<!-- 或者 -->


<!-- 视图解析器(框架默认提供, 解析jsp) -->  
<!-- 而id为viewResolver的viewResolver的优先级是最低的 -->
<bean id="jspViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/jsp/"/>
    <property name="suffix" value=".jsp"/>

    <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
    <property name="contentType" value="text/html;charset=UTF-8"/><!-- 网页mimetype  -->
    <property name="order" value="10"/><!-- 优先级,0为最高, 优先解析 -->
    <!-- InternalResourceViewResolver的优先级必须设置为最低，也就是order要最大。不然它会阻碍其他视图解析器
        因为, 这个解析器如果不能解析页面, 不会返回null, 解析不会继续进行, 解析链就断了
     -->
</bean>

<!-- 配置FreeMark视图 -->  
<bean id="freeMarkerViewResolver" class="org.springframework.web.servlet.view.freemarker.FreeMarkerViewResolver">  
    <property name="contentType" value="text/html;charset=UTF-8"/>        
    <property name="viewClass" value="org.springframework.web.servlet.view.freemarker.FreeMarkerView"/>  
    <property name="suffix" value=".ftl"/>  <!-- 这里没有配置 prefix, 留在下面配置 -->
    <property name="cache" value="true"/>  
    <property name="exposeSessionAttributes" value="true"/>  
    <property name="exposeRequestAttributes" value="true"/>       
    <property name="exposeSpringMacroHelpers" value="true"/>  
    <!-- 在页面中使用${rc.contextPath}就可获得contextPath -->  
    <property name="requestContextAttribute" value="rc"/>  
    <property name="order" value="0"/>  
</bean>  

<bean id="FreeMarkerConfigurer" class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">  
    <property name="templateLoaderPath" value="/WEB-INF/ftl/"/>  <!-- 这里相当于prefix -->
    <property name="defaultEncoding" value="UTF-8"/>  
    <property name="freemarkerSettings" ref="freemarkConfig"/>  
    <property name="freemarkerVariables">  
        <map>  
            <entry key="xml_escape" value-ref="fmXmlEscape"/>  
        </map>  
    </property>  
</bean>

<bean id="freemarkConfig" class="org.springframework.beans.factory.config.PropertiesFactoryBean">  
        <property name="location" value="classpath:freemark.properties"/> <!-- 见下面 --> 
</bean>  

<bean id="fmXmlEscape" class="freemarker.template.utility.XmlEscape"/> 
<!-- 配置FreeMark视图--结束 -->

<!-- json视图 -->
<bean id="mappingJacksonHttpMessageConverter" class="org.springframework.http.converter.json.MappingJacksonHttpMessageConverter">  
    <property name="supportedMediaTypes">  
        <list>  
            <value>application/json;charset=UTF-8</value>  
        </list>  
    </property>       
    <property name="objectMapper">  
        <bean class="org.codehaus.jackson.map.ObjectMapper">  
            <property name="dateFormat">  
                <bean class="java.text.SimpleDateFormat">  
                    <constructor-arg index="0" type="java.lang.String" value="yyyy-MM-dd HH:mm:ss"/>  
                </bean>  
            </property>  
        </bean>  
    </property>  
</bean>  
<bean id="stringHttpMessageConverter" class="org.springframework.http.converter.StringHttpMessageConverter"/>  
<bean id="requestMappingHandlerAdapter" class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">  
<property name="messageConverters">  
    <list>  
        <ref bean="mappingJacksonHttpMessageConverter"/>  
        <ref bean="stringHttpMessageConverter"/>  
    </list>  
</property>  
</bean> 
<!-- json视图配置--- end -->

<!-- 文件上传配置注意:这里申明的id必须为multipartResolver -->  
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">  
    <property name="maxUploadSize" value="500000"/>  
</bean> 

<!-- 简单的异常处理 -->  
<bean id="exceptionResolver" class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">  
    <property name="exceptionMappings">  
        <props>  
            <!-- 映射目录为/WEB-INF/jsp/error/upload_error.jsp -->  
            <prop key="org.springframework.web.multipart.MaxUploadSizeExceededException">/error/upload_error</prop>  
        </props>  
    </property>  
</bean>
```

文件中的freemarker.properties文件配置内容如下

```
#FreeMarker settings:  
   
#0 is for development only! Use higher value otherwise.  
template_update_delay=0  
locale=zh_CN  
default_encoding=UTF-8  
number_format=0.##########  
date_format=yyyy-MM-dd 
```

如果不希望单独抽出freemarker.properties文件, 另一种配置Freemarker视图的方式:

```xml
<?xml version="1.0" encoding="UTF-8"?>  
<beans xmlns="http://www.springframework.org/schema/beans"   
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
    xmlns:p="http://www.springframework.org/schema/p"   
    xmlns:context="http://www.springframework.org/schema/context"  
    xmlns:util="http://www.springframework.org/schema/util"  
    xmlns:mvc="http://www.springframework.org/schema/mvc"  
    xsi:schemaLocation="  
        http://www.springframework.org/schema/util  
        http://www.springframework.org/schema/util/spring-util-3.1.xsd  
        http://www.springframework.org/schema/beans   
        http://www.springframework.org/schema/beans/spring-beans-3.1.xsd  
        http://www.springframework.org/schema/context   
        http://www.springframework.org/schema/context/spring-context-3.1.xsd  
        http://www.springframework.org/schema/mvc  
       http://www.springframework.org/schema/mvc/spring-mvc-3.1.xsd" >  
     <!-- 开启注解 -->  
    <mvc:annotation-driven />  
    <!-- 扫描包 -->  
    <context:component-scan base-package="com.misl.*" />  
      
    <!-- FreeMarker settings -->  
    <bean id="freeMarkerConfigurer" class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">  
        <property name="templateLoaderPath" value="" />  
        <property name="freemarkerSettings">  
            <props>  
                <prop key="template_update_delay">0</prop>  
                <prop key="default_encoding">UTF-8</prop>  
                <prop key="locale">zh_CN</prop>  
            </props>  
        </property>  
    </bean>  
    <!-- FreeMarker view Reslover -->  
    <bean id="freeMarkerViewResolver" class="org.springframework.web.servlet.view.freemarker.FreeMarkerViewResolver">  
        <property name="cache" value="false" />  
        <property name="prefix" value="/WEB-INF/views/" />  
        <property name="suffix" value=".ftl" />  
        <property name="contentType" value="text/html;charset=UTF-8" />  
    </bean>  
      
</beans> 
```

## 映射器 Mapping

为发送的请求找到合适的Handler Adapter

* org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping(默认)
    Action所对应的`<bean>`标签的name属性作为请求路径

    ```xml
        <!-- 注册控制器(程序员) -->
        <bean name="/add.action" class="com.xiaoyu.springmvc.mapping.UserAction"></bean>

        <!-- 注册映射器(handler包)(框架默认提供了, 这里显式配置了默认的就失效了) -->
        <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>
    ```

* org.springframework.web.servlet.handler.SimpleUrlHandlerMapping
    多个路径对应同一个Action

    ```xml
    <!-- 注册控制器(程序员) -->
    <bean id="userActionID" class="com.xiaoyu.springmvc.mapping.UserAction"></bean>
    <!-- 注册映射器(handler包)(框架) -->
    <bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
            <property name="mappings">
                <props>
                    <prop key="/delete.action">userActionID</prop>
                    <prop key="/update.action">userActionID</prop>
                    <prop key="/find.action">userActionID</prop>
                </props>
            </property>
    </bean>
    ```
    
    or

    ```xml
    <beans ...>
    
    <bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
       <property name="mappings">
        <value>
           /welcome.htm=welcomeController
           /*/welcome.htm=welcomeController
           /helloGuest.htm=helloGuestController
        </value>
       </property>
    </bean>
    
    <bean id="welcomeController" 
        class="com.mkyong.common.controller.WelcomeController" />
        
    <bean id="helloGuestController" 
        class="com.mkyong.common.controller.HelloGuestController" />
        
    </beans>
    ```

## 适配器 Adapter

找到具体的action

关注【Action实现什么接口】

* Action实现Controller接口, org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter

    ```java
    public class EmpAction implements Controller{
        public ModelAndView handleRequest(HttpServletRequest request,HttpServletResponse response) throws Exception {
            System.out.println("EmpAction::handleRequest");
            request.setCharacterEncoding("UTF-8");
            String username = request.getParameter("username");
            ModelAndView modelAndView = new ModelAndView();
            modelAndView.addObject("message",username);
            modelAndView.setViewName("/jsp/success.jsp");
            return modelAndView;
        }
    }

    ```

* Action实现了HttpRequestHandler接口, HttpRequestHandlerAdapter

    ```xml
    <!-- 适配器(框架默认提供, 这里显式配置了, 默认的就失效了) -->  
    <bean class="org.springframework.web.servlet.mvc. HttpRequestHandlerAdapter"></bean>

    ```

## 控制器 Controller

【Action继承什么类】

* org.springframework.web.servlet.mvc.ParameterizableViewController

    ```xml
    <!-- 如果请求是/index.action的请求路径，则直接跳转到/jsp/success.jsp页面，不经过程序员定义的控制器，无需写controller代码，即缺省控制器 -->
    <!-- /index.action请求，直接转发到/index.jsp页面 -->
    <bean name="/index.action" class="org.springframework.web.servlet.mvc.ParameterizableViewController">
            <property name="viewName" value="/index.jsp"/>
    </bean>

    ```

    另外, 提供一个简单的操作:

    ```xml
    <!-- 如果当前路径是/ 则交给相应的视图解析器直接解析为视图 -->
    <mvc:view-controller path="/" view-name="home"/>

    <!-- 即如果当前路径是/ 则重定向到/admin/index -->
    <mvc:view-controller path="/" view-name="redirect:/admin/index"/> 
    ```

* org.springframework.web.servlet.mvc.AbstractCommandController

    ```java
    // 能够以实体的形式，收集客户端参数
    public class AdminAction extends AbstractCommandController{
        public AdminAction(){
                this.setCommandClass(Admin.class);
        }
        @Override
        protected ModelAndView handle(HttpServletRequest request,HttpServletResponse response, Object obj, BindException bindException)throws Exception {
            System.out.println("AdminAction::handle");
            ModelAndView modelAndView = new ModelAndView();
            Admin admin = null;
            if(obj instanceof Admin){
                    admin = (Admin) obj;
            }
            modelAndView.addObject("username",admin.getUsername());
            modelAndView.addObject("gender",admin.getGender());
            modelAndView.addObject("hiredate",admin.getHiredate());
            modelAndView.setViewName("/jsp/success.jsp");
            return modelAndView;
        }
    }

    ```

    ```xml
    <!-- 请求处理类 -->
    <bean name="/add.action" class="com.xiaoyu.springmvc.controller.AdminAction">
    </bean>

    ```

## 注解开发

### Spring3.1前后区别

1、注解方式的映射器和适配器在3.1版本前后是不一样的
2、注解方式的映射器和适配器必须配对使用，不能与非注解的映射器和适配器混用

```
spring3.1之前:
org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping 是在spring3.1之前使用的注解映射器。
org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter 是在spring3.1之前使用的注解适配器。
spring3.1之后:
org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping是在spring3.1之后使用的注解映射器。
org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter是在spring3.1之后使用的注解适配器。

```

### 配置

方法1: 通过bean标签配置

```xml
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"></bean>
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"></bean>
```

方法2: 通过mvc标签配置

```xml
<mvc:annotation-driven/>
```

# Controller详解

## 单个 controller 中写多个业务方法

```java
// 模块根路径 + 功能子路径 = 访问模块下子功能的路径
@Controller
@RequestMapping(value="/user")
public class UserAction{
	@RequestMapping(value="/add")
	public String add(Model model) throws Exception{
			System.out.println("HelloAction::add()");
			model.addAttribute("message","增加用户");
			return "/success.jsp";
	}
	@RequestMapping(value="/find")
	public String find(Model model) throws Exception{
			System.out.println("HelloAction::find()");
			model.addAttribute("message","查询用户");
			return "/success.jsp";
	}    
}
```

@RequestMapping 用法:

- value/path 配置路径
- method    配置支持的 http method
- params  配置支持的请求参数和对应值 `params={"username=kolbe","password=123456"}`
- headers  支持的 headers `headers="Host=localhost:8080"`


注意: 

@RequestMapping("/login/") 和 @RequestMapping("/login") 路径末尾是否有斜杠 `是不同的路径`

## 数据传递方式

总结: 

- get 请求 使用 @requestParam 注解方法参数, 可省略, 可以有多个
- post 请求使用 @RequestBody 注解参数, 不可省略, 最多只能一个

### 收集页面的参数

第1种: 使用HttpServletRequest获取

```java
@RequestMapping("/login.do")  
	public String login(HttpServletRequest request){  
	    String name = request.getParameter("name")  ;
	    String pass = request.getParameter("pass")  ;
	}

```

第2种: 普通变量作为方法参数收集数据

```java
// Spring会自动将表单参数注入到方法参数，和表单标签的name属性保持一致。
@Controller
@RequestMapping(value="/user")
public class UserAction{
	@RequestMapping(value="/add")
	public String add(Model model,int id,String name,Double sal) throws Exception{
		System.out.println("HelloAction::add()");
		System.out.println(id + ":" + name + ":" + sal);
		model.addAttribute("message","增加用户");
		return "/success.jsp";
	}    
}
    // http://127.0.0.1:8080/myspringmvc/user/add.action?id=1&name=zhaojun&sal=5000


@RequestMapping("/login.do")  
// 表单属性是pass,用变量password接收  ----------表单属性和形参参数名不同
public String login(HttpServletRequest request, String name, @RequestParam("pass")String password) {
    
    syso(name);  
    syso(password)  
}  

```

第3种: 封装普通变量为一个Object来收集

表单:

```html
<form action="login.do">  
	用户名：<input name="name"/>  
	密码：<input name="pass"/>  
	<input type="submit" value="登陆">  
	</form> 

```

将表单参数封装为一个对象User

```java
public class User{  
	  private String name;  
	   private String pass;  
	 } 

```

Controller方法:

```java
@RequestMapping("/login.do")  
	public String login(User user)  
	{  
	   syso(user.getName());  
	   syso(user.getPass());  
	}  

```

第4种: Object包Object(封装两次)来收集参数

```java
/**
1） 如果多个模型中有相同的属性时，可以在jsp中用user.name或admin.name来收集客户端参数（作为标签name值）
2） 用一个新的模型（Bean）将User和Admin再封装一次，JSP传过来的值会自动封装到Bean中
*/
// User.java
public class User {
        private Integer id;
        private String name;
        private Double sal;
        private Date hiredate;
        
		// getter&setter
}

// Admin.java

// Bean.java
public class Bean {
        private User user;
        private Admin admin;
        // getter&setter
}

// PersonAction.java
@Controller
@RequestMapping(value = "/person")
public class PersonAction {
	@InitBinder
	protected void initBinder(HttpServletRequest request,ServletRequestDataBinder binder) throws Exception {
		binder.registerCustomEditor(
			Date.class, 
			new CustomDateEditor(new SimpleDateFormat("yyyy-MM-dd"),true));
	}
	@RequestMapping(value = "/add", method = RequestMethod.POST)
	public String add(Bean bean,Model model) throws Exception {
			System.out.println(bean.getUser());
			System.out.println(bean.getAdmin());
			System.out.println("PersonAction::add()::POST");
			model.addAttribute("bean",bean);
			return "/register.jsp";
	}
}

```

```html
    <!-- 普通用户 -->
<form action="${pageContext.request.contextPath}/person/add.action" method="POST">
        <!-- name元素的值不加bean, 会自动封装到Bean对象 -->
        编号：<input type="text" name="user.id" value="${bean.user.id}"/><br/>
        姓名：<input type="text" name="user.name" value="${bean.user.name}"/><br/>
        薪水：<input type="text" name="user.sal" value="${bean.user.sal}"/><br/>
        入职时间：<input type="text" name="user.hiredate" value='<fmt:formatDate value="${bean.user.hiredate}" type="both" />'/><br/>
        <input type="submit" value="注册"/>
</form>
```

### 收集数组参数

```java
// 批量删除用户
@Controller
@RequestMapping(value="/user")
public class UserAction {
	@RequestMapping(value="/delete")
	public String deleteMethod(int[] ids,Model model) throws Exception{
			System.out.println("UserAction::deleteMethod()");
            System.out.println("需要删除的id为：");
			for(int id : ids){
					System.out.print(id+" ");
			}
			model.addAttribute("message","批量删除成功");
			return "/success.jsp";
	}
}


```

### 手机List参数

```java
// 批量注册用户
// UserAction.java
@Controller
@RequestMapping(value="/user")
public class UserAction {
	@RequestMapping(value="/addAll")
	public String addAll(Bean bean,Model model) throws Exception{
		for(User user : bean.getUserList()){
			System.out.println(user.getName()+":"+user.getGender());
		}
		model.addAttribute("message","批量增加用户成功");
		return "/success.jsp";
	}
}

    // Bean.java
public class Bean {
	
	private List<User> userList = new ArrayList<User>();
	public Bean(){}
	public List<User> getUserList() {
			return userList;
	}
	public void setUserList(List<User> userList) {
			this.userList = userList;
	}
}
```

jsp

```jsp
<!-- registerAll.java -->
<form action="${pageContext.request.contextPath}/user/addAll.action" method="POST"> 
            
        姓名：<input type="text" name="userList[0].name" value="hh"/>
        性别：<input type="text" name="userList[0].gender" value="男"/>
        <hr/>
        姓名：<input type="text" name="userList[1].name" value="zz"/>
        性别：<input type="text" name="userList[1].gender" value="男"/>
        <hr/>

        姓名：<input type="text" name="userList[2].name" value="yy"/>
        性别：<input type="text" name="userList[2].gender" value="女"/>
        <hr/>
        <input type="submit" value="批量注册"/>
</form>
 
```

### 传递参数给页面

经过Controller组件处理后，向jsp页面传值，有如下几种：
1，使用HttpServletRequest 和 Session  然后setAttribute()，就和Servlet中一样
2，使用ModelMap, Model, Map对象
3，使用ModelAndView对象
4，使用@ModelAttribute注解


第1种: 使用HttpServletRequest 或 session

```java
@RequestMapping("/login.do")  
public String login(String name,String pwd  
                            ModelMap model,HttpServletRequest request){  
        User user = serService.login(name,pwd);  
        HttpSession session = request.getSession();  
        session.setAttribute("user",user);  
        model.addAttribute("user",user);  
        return "success";  
} 

```

第2种: 使用ModelMap，Model，Map对象

```java
// 据会利用HttpServletRequest的Attribute传值到success.jsp中
@RequestMapping("/test")
public String test(Map<String,Object> map,Model model,ModelMap modelMap){

    map.put("names", Arrays.asList("caoyc","zhh","cjx"));
	
    model.addAttribute("time", new Date());
	
    modelMap.addAttribute("city", "ChengDu");
    modelMap.put("gender", "male");

    return "hello";
}

```

在jsp中如何获取?

```jsp
time:${requestScope.time}
names:${requestScope.names }
city:${requestScope.city }
gender:${requestScope.gender }

```

第3种: 使用ModelAndView对象

```java
@RequestMapping("/login.do")  
public ModelAndView  login(String name,String pass){  
    User user = userService.login(name,pwd);  
    Map<String,Object> data = new HashMap<String,Object>();  
    data.put("user",user);  
    return new ModelAndView("success",data);  
} 

```

第4种: 使用@ModelAttribute注解

```java
// 在Controller方法的参数部分或Bean属性方法上使用
// @ModelAttribute数据会利用HttpServletRequest的Attribute传值到success.jsp中
@RequestMapping("/login.do")  
public String login(@ModelAttribute("user") User user){  
    //TODO  
    return "success";  
}  
    
@ModelAttribute("name")  
public String getName(){  
    return name;  
}  


```

# 拦截器interceptor

## 什么是interceptor

对请求进行拦截, 每个请求都需要的前置/后置处理

spring mvc中的Interceptor拦截请求是通过HandlerInterceptor来实现的。所以HandlerInteceptor拦截器只有在Spring Web MVC环境下才能使用

## interceptor和filter对比

references: https://www.jianshu.com/p/685c65ed6944, https://juejin.im/post/5b2ddbcef265da59a76c92a4#heading-6

servlet filter和spring mvc Interceptor区别：

- Filter 是基于 函数回调的，而 Interceptor 则是基于 Java 反射 和 动态代理。

- Filter 依赖于 Servlet 容器, 和servlet容器隔的近，而 Interceptor 依赖springmvc, 和应用隔的近, 在最里层。所以filter 先进入, 最后退出

- Filter 对几乎 所有的请求 起作用，而 Interceptor 只对 Controller 请求起作用。

- 拦截器可以获取IOC容器中的各个bean，而过滤器就不行，这点很重要，在拦截器里注入一个service，可以调用业务逻辑。

servlet filter和spring mvc Interceptor执行顺序：

![alt](Snipaste_2018-11-01_16-57-14.png)

## 怎么使用interceptor

### 通过自定义web配置

- 第一种方式是要实现Spring的`HandlerInterceptor`接口 or `HandlerInterceptorAdapter` (推荐) 适配器

    - 三个方法: 
    
        preHandle(eg:登陆检查), 
        
        postHandle(eg:操纵modelAndView), 
        
        afterCompletion(eg:资源清理工作, 性能监控记录结束时间); 
    
    - 顺序: preHandle -> handle -> postHandle -> 视图渲染 -> afterCompletion

- 第二种方式是实现WebRequestInterceptor接口 or 对应的适配器

使用代码配置:

```java
@configuration
class WebMvcConfig extends WebMvcConfigurerAdapter{

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //创建自定义的拦截器
        Interceptor interceptor = new Interceptor();
        //添加拦截器
        registry.addInterceptor(interceptor)
        		//添加需要拦截的路径
                .addPathPatterns("");
    }
}
```


也可以使用  xml 配置:

```xml
<mvc:interceptors>  
    <!-- 使用bean定义一个Interceptor，直接定义在mvc:interceptors根下面的Interceptor将拦截所有的请求-->  
    <bean class="com.host.app.web.interceptor.HandlerInterceptorAll"/>

    <mvc:interceptor>  
        <!-- 定义在mvc:interceptor下面的表示是对特定的请求才进行拦截的， 支持通配符， 如 “/emp/*” -->  
        <mvc:mapping path="/test/number.do"/> 
        <!-- 排除url -->
        <mvc:exclude-mapping path="/test/goLogin.*"/>
        <bean class="com.host.app.web.interceptor.HandlerInterceptor1"/>  
        <!-- <ref bean="handlerInterceptor1"/> -->
    </mvc:interceptor>  

    <mvc:interceptor>
        <mvc:mapping path="/**"/>
        <bean class="common.interceptor.LoginInterceptor">
        <!-- 排除多个 url -->
            <property name="exceptUrls">
                <list>
                    <value>/user/login</value>
                    <value>/user/register</value>
                </list>
            </property>
        </bean>
    </mvc:interceptor>

    <mvc:interceptor>
        <!--这样配置，将拦截所有springmvc的url-->
        <mvc:mapping path="/**"/>
        <bean class="interceptor.MyInterceptor1"></bean>
    </mvc:interceptor>
</mvc:interceptors>  

```

### 通过注解

```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface LoginRequired {
}


public class AuthenticationInterceptor implements HandlerInterceptor {
    @Autowired
    private UserService userService;

    public boolean preHandle(HttpServletRequest request,
                             HttpServletResponse response, Object handler) throws Exception {
        // 如果不是映射到方法直接通过
        if (!(handler instanceof HandlerMethod)) {
            return true;
        }
        HandlerMethod handlerMethod = (HandlerMethod) handler;
        Method method = handlerMethod.getMethod();

        // 判断接口是否需要登录
        LoginRequired methodAnnotation = method.getAnnotation(LoginRequired.class);
        // 有 @LoginRequired 注解，需要认证
        if (methodAnnotation != null) {
            // 执行认证
            String token = request.getHeader("token");  // 从 http 请求头中取出 token
            if (token == null) {
                throw new RuntimeException("无token，请重新登录");
            }
            int userId;
            try {
                userId = Integer.parseInt(JWT.decode(token).getAudience().get(0));  // 获取 token 中的 user id
            } catch (JWTDecodeException e) {
                throw new RuntimeException("token无效，请重新登录");
            }
            User user = userService.findById(userId);
            if (user == null) {
                throw new RuntimeException("用户不存在，请重新登录");
            }
            // 验证 token
            try {
                JWTVerifier verifier =  JWT.require(Algorithm.HMAC256(user.getPassword())).build();
                try {
                    verifier.verify(token);
                } catch (JWTVerificationException e) {
                    throw new RuntimeException("token无效，请重新登录");
                }
            } catch (UnsupportedEncodingException ignore) {}
            request.setAttribute("currentUser", user);
            return true;
        }
        return true;
    }


@Configuration
public class WebMvcConfigurer extends WebMvcConfigurerAdapter {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(authenticationInterceptor())
                .addPathPatterns("/**");    // 拦截所有请求，通过判断是否有 @LoginRequired 注解 决定是否需要登录
        super.addInterceptors(registry);
    }

    @Bean
    public AuthenticationInterceptor authenticationInterceptor() {
        return new AuthenticationInterceptor();
    }
}
```

### 自定义注解拦截器

https://blog.csdn.net/xwq911/article/details/51384387

## HandlerMethod和MethodHandle

HandlerMethod - springmvc 拦截器接口的 preHandle（。。。） 中的最后一个参数， 表示 springmvc 执行的方法
MethodHandle - java7新特性之方法句柄； ref：https://blog.csdn.net/zmx729618/article/details/78968810

## 通过Java动态代理实现拦截器功能

https://blog.csdn.net/qq_35246620/article/details/68484407

# 限定请求方式GET/POST

```java
// 可以在业务控制方法前，指明该业务控制方法只能接收GET或POST的请求
// 如果不书写method=RequestMethod.POST的话，GET和POST请求都支持
@Controller
@RequestMapping(value="/user")
public class UserAction{
	@RequestMapping(value="/add",method=RequestMethod.POST)
	public String add(Model model,int id,String name,double sal) throws Exception{
			System.out.println("HelloAction::add()::POST");
			System.out.println(id + ":" + name + ":" + sal);
			model.addAttribute("message","增加用户");
			return "/success.jsp";
	}    
}
```

# 日期转换器 编码转换器

在默认情况下，springmvc不能将String类型转成Date类型，必须自定义类型转换器: Controller中覆盖initBinder方法

```java
@Controller
@RequestMapping("/xxx")
public class AdminAction {
	@Override
	protected void initBinder(HttpServletRequest request,ServletRequestDataBinder binder) throws Exception {
			binder.registerCustomEditor(Date.class,new CustomDateEditor(new SimpleDateFormat("yyyy-MM-dd"),true));
	} 
	
}

```

spring提供的，专用于解决POST提交中文乱码问题，需要在web.xml文件中配置
	
```xml
<!-- 编码过滤器 -->
	<filter>
			<filter-name>CharacterEncodingFilter</filter-name>
			<filter-class>
	org.springframework.web.filter.CharacterEncodingFilter
			</filter-class>
			<init-param>
					<param-name>encoding</param-name>
					<param-value>UTF-8</param-value>
			</init-param>
	</filter>
	<filter-mapping>
			<filter-name>CharacterEncodingFilter</filter-name>
			<url-pattern>/*</url-pattern>
	</filter-mapping>
```


# @InitBind解决字符串转日期类型

[@InitBind注解](https://stackoverflow.com/questions/5211323/what-is-the-purpose-of-init-binder-in-spring-mvc): 参考这里https://stackoverflow.com/questions/5211323/what-is-the-purpose-of-init-binder-in-spring-mvc

之前我们需要解析前台传来的日期String, 

```java
public void webmethod(@RequestParam("date") String strDate) {
    Date date = ... // manually parse the date
 }
```

现在没必要了, spring会尝试检查前台传来的string凭借注册的editors能否转为object, 以便我们直接使用

```java
// 1）在默认情况下，springmvc不能将String类型转成java.util.Date类型，所有我们只能在Action中自定义类型转换器
@Controller
@RequestMapping(value = "/user")
public class UserAction {
	@InitBinder
	protected void initBinder(HttpServletRequest request,ServletRequestDataBinder binder) throws Exception {
		binder.registerCustomEditor(
					Date.class, 
					new CustomDateEditor(new SimpleDateFormat("yyyy-MM-dd"),true));
	}
	@RequestMapping(value = "/add", method = RequestMethod.POST)
	public String add(int id, String name, double sal, Date hiredate,
					Model model) throws Exception {
		System.out.println("HelloAction::add()::POST");
		model.addAttribute("id", id);
		model.addAttribute("name", name);
		model.addAttribute("sal", sal);
		model.addAttribute("hiredate", hiredate);
		return "/register.jsp";
	}
}
```

```jsp
jsp：
<form action="${pageContext.request.contextPath}/user/add.action" method="POST">
	ID<input type="text" name="id" value="${id}"/><br/>
	name<input type="text" name="name" value="${name}"/><br/>
	sal<input type="text" name="sal" value="${sal}"/><br/>
	hiredate <input type="text" name="hiredate" value='<fmt:formatDate value="${hiredate}" type="date"/>'/><br/>
	<input type="submit" value="×¢²á"/>
</form> 
```

# 结果的转发和重定向

1） 在转发情况下，共享request域对象，会将参数从第一个业务控制方法传入第二个业务控制方法，重定向则不行 
```java
// 删除id=10 号的用户，再查询用户
@Controller
@RequestMapping(value="/user")
public class UserAction {
 
        @RequestMapping(value="/delete")
        public String delete(int id) throws Exception{
                System.out.println("删除用户->" + id);
                //转发到find()
                return "forward:/user/find.action";
                //重定向到find()
                //return "redirect:/user/find.action";
        }
        @RequestMapping(value="/find")
        public String find(int id) throws Exception{
                System.out.println("查询用户->" + id);
                return "/success.jsp";
        }
}
```

2）Spring MVC 默认采用的是转发来定位视图，如果要使用重定向，可以
    1，使用RedirectView
    2，使用redirect:前缀

```java
public ModelAndView login(){  
    RedirectView view = new RedirectView("regist.do");  
    return new ModelAndView(view);  
}  
```

or (更常用)

```java
public String login(){  
    //TODO  
    return "redirect:/regist";// 后面需要增加一个方法用于处理 /regist 访问  
}

```

请求路径可以配置多个对应一个Controller方法

```java
@RequestMapping({ "/", "/index" })  
    public String index() throws Exception {  
        return "index";  
    } 
```

# 返回json, @ResponseBody 

提交表单后，将JavaBean信息以JSON文本形式返回到浏览器

```java
       
    // User.java
public class User {
        private Integer id;
        private String name;
        private Double sal;
    //    setter&getter
}

// UserAction.java
@Controller
@RequestMapping(value="/user")
public class UserAction {
 
	@RequestMapping(value="/add")
	public @ResponseBody User add(User user) throws Exception{
		System.out.println(user.getId()+":"+user.getName()+":"+user.getSal());
		return user;//由于使用了ResponseBody注解，这里user会作为http正文放回
	}
}
```

springmvc.xml

```xml
<!-- Action控制器 -->
<context:component-scan base-package="com.xiaoyu.springmvc"/>      

<!-- 配适器 -->
<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
    <property name="messageConverters">  <!--将JavaBean信息以JSON文本形式返回到浏览器-->
        <list>
            <bean class="org.springframework.http.converter.json.MappingJacksonHttpMessageConverter"/>
        </list>
    </property>
</bean>
```

# 静态资源放行

```xml
<mvc:resources mapping="/css/**" location="/css/"/>  
<mvc:resources mapping="/js/**" location="/js/"/>  
<mvc:resources mapping="/fonts/**" location="/fonts/"/>  
<mvc:resources mapping="/style/**" location="/style/"/> 
<!-- or 通过后缀确定资源 -->
<mvc:resources location="/" mapping="/**/*.html"/> 
<mvc:resources location="/" mapping="/**/*.js"/> 
<mvc:resources location="/" mapping="/**/*.css"/> 
<mvc:resources location="/" mapping="/**/*.png"/> 
<mvc:resources location="/" mapping="/**/*.gif"/>

```

或者直接所有静态资源直接全部放行:

```xml
<mvc:default-servlet-handler />  
```

还可以通过在web.xml中配置servlet过滤静态资源

```xml
<servlet-mapping>
	   <servlet-name>default</servlet-name>
	    <url-pattern>*.css</url-pattern>
	</servlet-mapping>
	 
	<servlet-mapping>
	   <servlet-name>default</servlet-name>
	  <url-pattern>*.gif</url-pattern>
	 </servlet-mapping>
	  
	 <servlet-mapping>
	   <servlet-name>default</servlet-name>
	   <url-pattern>*.jpg</url-pattern>
	 </servlet-mapping>
	  
	 <servlet-mapping>
	   <servlet-name>default</servlet-name>
	   <url-pattern>*.js</url-pattern>
	 </servlet-mapping>

```

# RequestContextHolder获取request,response


# 统一异常捕获

```java
// 首先自定义一个Controller的父类，需要捕获异常的Controller继承此类就可以了
public class BaseController {  
    
    /** 
    * 使用@ExceptionHandler注解，继承此类的Controller发生异常时会自动执行该方法 
    * @param request 
    * @param e 
    * @return 
    */  
    @ExceptionHandler  
    public String exception(HttpServletRequest request, Exception e) {  
        //对异常进行判断做相应的处理  
        if(e instanceof NullPointerException){  
            return "/error1.html";  
        }else if(e instanceof IllegalArgumentException){  
            return "/error2.html";  
        }else{  
            return "/error3.html";  
        }  
    }  
}  

// 使用
@Controller  
public class TestAction extends BaseController{  
    
    @RequestMapping(value = "/manager/test1f.aspx", method = { RequestMethod.GET, RequestMethod.POST })  
    public void test1f(HttpServletRequest request, HttpServletResponse response, ModelMap model) throws IOException {  
        throw new NullPointerException();  
    }  
}  
// 这样访问"/manager/test1f.aspx"时会抛出异常并被处理，跳转至/error1.html
// 如果是处理不同情况下的异常，可以自定义多个父Controller，根据不同业务继承对应的Controller就可以了


```