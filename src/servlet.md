---
title: Servlet
tags:
  - servlet
date: 2016-04-13 22:16:29
categories: java web
---

<div align="center">
Servlet note
Servlet 是单实例多线程的, 因此会有 thread-safe 问题
</div>

<!--more-->

<!-- TOC -->

- [从资源请求过程说起](#从资源请求过程说起)
- [什么是动态资源](#什么是动态资源)
    - [手动开发Servlet](#手动开发servlet)
    - [借助工具开发Servlet](#借助工具开发servlet)
- [目录规范](#目录规范)
- [解决web项目中的乱码](#解决web项目中的乱码)
- [web项目中的路径问题](#web项目中的路径问题)
- [和路径相关的api](#和路径相关的api)
- [Servlet details](#servlet-details)
    - [总结](#总结)
    - [Tomcat构造Servlet过程](#tomcat构造servlet过程)
    - [Servlet映射路径](#servlet映射路径)
    - [Servlet缺省路径](#servlet缺省路径)
    - [Servlet生命周期](#servlet生命周期)
    - [Servlet提前加载(load-on-startup)](#servlet提前加载load-on-startup)
    - [init(),init(xxx)区别](#initinitxxx区别)
    - [Servlet多线程并发访问](#servlet多线程并发访问)
    - [ServletConfig对象](#servletconfig对象)
    - [ServletContext对象](#servletcontext对象)
    - [demo](#demo)
    - [域对象共享数据](#域对象共享数据)
- [jsp九个内置对象/四个域对象,servlet三大域对象](#jsp九个内置对象四个域对象servlet三大域对象)
- [三种方式读取资源文件](#三种方式读取资源文件)
- [转发&重定向](#转发重定向)
    - [调用方式](#调用方式)
    - [本质区别](#本质区别)
    - [代码例子](#代码例子)
- [会话管理](#会话管理)
- [过滤器filter](#过滤器filter)
    - [传统配置方法开发 filter](#传统配置方法开发-filter)
    - [案例:统一处理编码](#案例统一处理编码)
    - [案例:登录过滤](#案例登录过滤)
    - [案例:自动登录](#案例自动登录)
    - [spring boot 中使用filter](#spring-boot-中使用filter)
- [监听器listener](#监听器listener)
    - [概念](#概念)
    - [有哪些监听器](#有哪些监听器)
    - [生命周期监听器](#生命周期监听器)
    - [属性监听器](#属性监听器)
    - [session绑定监听器](#session绑定监听器)
- [i18n](#i18n)
- [文件上传下载](#文件上传下载)
    - [form中只有纯文本数据](#form中只有纯文本数据)
    - [form中含有文本和二进制文件](#form中含有文本和二进制文件)
- [javamail邮件开发](#javamail邮件开发)
- [servlet3.0](#servlet30)
    - [异步支持](#异步支持)
        - [实现请求线程池隔离](#实现请求线程池隔离)
    - [插件式开发](#插件式开发)
    - [注解开发](#注解开发)
- [xxxWapper](#xxxwapper)
- [servlet和线程安全问题](#servlet和线程安全问题)

<!-- /TOC -->

# 从资源请求过程说起

<img src="Screenshot_1.png"/>

# 什么是动态资源

动态资源是相对于静态资源来说的, 当多个user访问动态资源, 页面源代码可能会发生改变

Servlet本质是一个接口, 是定义的一套开发规范, 自定义的servlet需要实现该接口, 实现对应的方法, 如init(), service(), destroy();

## 手动开发Servlet

<img src="p1.png"/>

```java
/**
 * 第一个servlet程序
 */
public class HelloServlet extends HttpServlet{
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
                    throws ServletException, IOException {
        //解决中文乱码问题
        resp.setContentType("text/html;charset=utf-8");
        //向浏览器输出内容
        resp.getWriter().write("这是第一个servlet程序。当前时间为："+new Date());
    }
}

```

编译后,拷贝helloServlet到Tomcat里的web项目的WEB-INF/classes.

配置web.xml:

```xml
<!-- 配置一个servlet程序 -->
<servlet>
    <!-- servlet的内部名称 ，可以自定义-->
    <servlet-name>HelloServlet</servlet-name>
    <!-- servlet类名： 包名+简单类名-->
    <servlet-class>gz.itcast.d_servlet.HelloServlet</servlet-class>
</servlet>
<servlet-mapping>
    <!-- servlet的内部名称，和上面的名称保持一致！！！-->
    <servlet-name>HelloServlet</servlet-name>
    <!-- servlet的访问名称： /名称 -->
    <url-pattern>/hello</url-pattern>
</servlet-mapping>

```

访问: http://localhost:8080/projectName/hello

## 借助工具开发Servlet

1. 常见web project
2. 在webroot文件夹下创建静态资源文件, 如js, css等
3. src文件夹下创建动态资源文件
4. 在工具中关联Tomcat
5. 部署(拷贝web app 到Tomcat的webapps文件夹下)
6. 启动Tomcat, 访问

# 目录规范

[java web项目WEB-INF与META-INF的作用](https://blog.csdn.net/u011687186/article/details/61194580)

使用 eclipse 生成的 maven web 项目 目录规范如下:

![alt](Snipaste_2018-09-08_16-43-24.png)

# 解决web项目中的乱码

https://blog.csdn.net/xiazdong/article/details/7217022

spring web 包提供一个filter 解决 post 请求乱码

web.xml配置

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

# web项目中的路径问题

根据request 获取的各种路径(以及, 登录成功后怎么根据记住的URL跳回用户最后访问的页面): https://www.cnblogs.com/JemBai/archive/2010/11/10/1873764.html

转发和重定向中的路径有什么区别? 看看这个例子

```java
/**
 * web应用中路径问题
 */
public class PathDemo extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		//目标资源： target.html
		/**
		 * 思考： 目标资源是给谁使用的。
		 * 		给服务器使用的：   / 表示在当前web应用的根目录（webRoot下）
		 * 		给浏览器使用的： /  表示在webapps的根目录下-----意味着地址要加项目名
		 */
		// 1.转发, 给server用, "/"代表当前项目的webroot
		request.getRequestDispatcher("/target.html").forward(request, response);
		//  2.请求重定向, 给client用, "/"代表Tomcat的webapps目录下, 需要带上项目名称
		response.sendRedirect("/day11/target.html");
		 // 3.html页面的超连接href
		response.getWriter().write("<html><body><a href='/day11/target.html'>超链接</a></body></html>");
		
		/**
		 * 4.html页面中的form提交地址
		 */
		response.getWriter().write("<html><body><form action='/day11/target.html'><input type='submit'/></form></body></html>");
	}
}

```

# 和路径相关的api

```java
//假定你的web application 名称为news,你在浏览器中输入请求路径：http://localhost:8080/news/main/list.jsp
//则执行下面代码后打印出如下结果：
 System.out.println(request.getContextPath());
//打印结果：/news

System.out.println(request.getServletPath());
//打印结果：/main/list.jsp

System.out.println(request.getRequestURI());
//打印结果：/news/main/list.jsp

System.out.println(request.getRequestURL());
//打印结果：http://localhost:8080/news/main/list.jsp

System.out.println(request.getRealPath("/"));
//打印结果：F:\Tomcat 6.0\webapps\news\test

```

*   如果是Servlet , Action , Controller, 或者Filter , Listener , 拦截器等相关类时， 我们只需要获得ServletContext， 然后通过ServletContext.getRealPath("/")来获取当前应用在服务器上的物理地址。
*   如果在类中取不到ServletContext时，利用Java的类加载机制：调用XXX.class.getClassLoader().getResource(""); 方法来获取到ClassPath , 然后处理获得WebRoot目录。
*   这种方式只能是该class在WebRoot/WEB-INF/classes下才能生效， 如果该class被打包到一个jar文件中， 则该方法失效。


# Servlet details

## 总结

```
Servlet编程
1）Servlet生命周期
	构造方法： 创建servlet对象。默认情况下，第一次访问servlet对象时。只调用1次。
	init方法（有参）： 创建完servlet对象后调用。只调用1次。
			注意： 会调用无参的init方法。
	service方法： servlet提供服务的方法。每次发出请求调用。
			注意： request对象 ，response对象
	destroy方法： tomcat服务器停止或web应用重新部署，servlet对象销毁，destroy方法被调用。
2）ServletConfig对象
		获取servlet的初始化参数：
			getInitParameter("name    ");
			getInitParameterNames();
3）ServletContext对象
		得到web应用路径：
				context.getContextPath();
				request.getContextPath();  等价于上面的代码
		得到web应用参数：
				context.getInitParameter("name");
				context.getInitParameterNames();
		域对象:
				context.setAttribute("name",Object): 保存数据
				context.getAttribute("name")   得到数据
				context.removeAttribue("name")  清除数据
		转发
				context.getRequestDispatcher("路径").forward(request,response);
				request.getRequestDispacher("路径").forward(request,response);  等价于上面的代码
		得到web应用中的资源文件
				context.getRealPath("路径")          这里的路径一般是/WEB-INF/classes/db.properties，得到的是绝对路径
				context.getResourceAsStream("路径");
				clazz.getResourceAcStream("/xxx");----------------带有斜杠，src目录下
				clazz.getClassLoader().getResourceAsStream(“xxx”);------------不带斜杠，src目录下
```

<img src="p3.png" style="width:60%"/>

## Tomcat构造Servlet过程

<img src="p4.png"/>

## Servlet映射路径

servlet 的 url pattern

<img src="p5.png"/>

demo:

<img src="p6.png"/>

## Servlet缺省路径

<img src="p7.png"/>

## Servlet生命周期

Servlet生命周期由Tomcat控制(何时创建, 何时调用, 何时销毁)

四个生命周期方法:

<img src="p8.png"/>

## Servlet提前加载(load-on-startup)

<img src="p9.png"/>

```xml
<servlet>
    <servlet-name>LifeDemo</servlet-name>
    <servlet-class>gz.itcast.c_life.LifeDemo</servlet-class>
    <!-- 让servlet对象自动加载 -->
		<!-- 当值 >=0 时，表示容器在应用启动时就加载这个servlet；整数值越大，创建优先级越低
					当 <0 或者 没有指定时，则指示容器在该servlet被选择时才加载。
					默认值 = -1 -->
    <load-on-startup>1</load-on-startup>
  </servlet>

```

## init(),init(xxx)区别

看下面demo

```java
/**
 * 有参数的init和无参的init方法
 */
public class InitDemo extends HttpServlet {
	/**
	 * 有参数的init方法
	 * 该方法是servlet的生命周期方法，一定会被tomcat服务器调用
	 */
	/**
	 * 注意：如果要编写初始代码，不需要覆盖有参数的init方法
	 */
	/*@Override------------这是错误的
	public void init(ServletConfig config) throws ServletException {
		System.out.println("有参数的init方法");
	}*/
	/**
	 * 无参数的init方法
	 * 该方法是servlet的编写初始化代码的方法。是Sun公司设计出来专门给开发者进行覆盖，然后在里面编写servlet的初始逻辑代码的方法。
	 */
	@Override
	public void init() throws ServletException {
		System.out.println("无参数的init方法");
	}
}

```

## Servlet多线程并发访问

<img src="Screenshot_2.png"/>

demo: 记录第几位访客

```java
/**
 * servlet的多线程并发问题
 */
public class TheradDemo extends HttpServlet {
    int count = 1;
    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("text/html;charset=utf-8");
        
        synchronized (TheradDemo.class) {//锁对象必须唯一。建议使用类对象
            response.getWriter().write("你现在是当前网站的第"+count+"个访客");   //线程1执行完  ， 线程2执行
            count++;
        }
    }
}

```

## ServletConfig对象

<img src="Screenshot_3.png"/>

每个 servlet 都有一个 ServletConfig

```xml
<servlet>
  <servlet-name>ConfigDemo</servlet-name>
  <servlet-class>com.xy.ConfigDemo</servlet-class>
  <!-- 初始参数： 这些参数会在加载web应用的时候，封装到ServletConfig对象中, servlet的参数只能由当前的这个sevlet获取-->
  <init-param>
      <param-name>path</param-name>
      <param-value>e:/b.txt</param-value>
  </init-param>
  <init-param>
    	<param-name>BBB</param-name>
    	<param-value>BBB's value</param-value>
    </init-param>
     <init-param>
    	<param-name>CCCC</param-name>
    	<param-value>CCCC's value</param-value>
    </init-param>

</servlet>

```

获取初始化参数的api

<img src="p10.png"/>

```java
public class ConfigDemo extends HttpServlet {
	/**
	 *  1）tomcat服务器把这些参数会在加载web应用的时候，封装到ServletConfig对象中 
	 *  2）tomcat服务器调用init方法传入ServletConfig对象
	 */
	/* 这是内部隐藏的代码:
    private ServletConfig config;
	public void init(ServletConfig config) throws ServletException {
		this.config = config;
	}*/
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		/**
		 * 读取servlet的初始参数
		 */
		String path = this.getServletConfig().getInitParameter("path");
		File file = new File(path);
		//读取内容
		BufferedReader br = new BufferedReader(new FileReader(file));
		String str = null;
		while( (str=br.readLine())!=null ){
			System.out.println(str);
		}
		//查询当前servlet的所有初始化参数
		Enumeration<String> enums = this.getServletConfig().getInitParameterNames();
		while(enums.hasMoreElements()){
			String paramName = enums.nextElement();
			String paramValue = this.getServletConfig().getInitParameter(paramName);
			System.out.println(paramName+"="+paramValue);
		}
		//得到servlet的名称
		String servletName = this.getServletConfig().getServletName();
		System.out.println(servletName);
	}
}


```

## ServletContext对象

https://www.zhihu.com/question/38481443

![alt](servletcontext.png)

*   每个应用程序都有一个唯一的ServletContext对象。它不能跨应用程序。它可以实现应用程序内的数据共享。
*   它的内部维护了一个Map集合，该集合中放的就是共享的数据。
*   用什么方法可以操作这个集合呢？
    *   setAttribute(key,value);//放key-value对
    *   Object obj = getAttribute(key);  //取值
    *   removeAttribute(key);       //移除key-value对
    *   ServletContext的生命周期，与应用程序共存。

ServletContext的设计是这样的

<img src="p11.png"/>

api:

<img src="p12.png"/>

如何获取?

*	config.getServletContext();
*	this.getServletContext()


有哪些作用?

*   获取全局（web应用）初始化参数
*	转发(获取转发器)
*	根据相对路径得到绝对路径
* 缓存(Web缓存----把不经常更改的内容读入内存，所以服务器响应请求的时候就不需要进行慢速的磁盘I/O了。)

## demo

*	得到web应用路径, 转发, 重定向

```java
/**
 * 得到web应用路径, 转发, 重定向
 */
public class ContextDemo1 extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		//1.得到ServletContext对象
		//ServletContext context = this.getServletConfig().getServletContext();
		ServletContext context = this.getServletContext(); //（推荐使用）
		//2.得到web应用路径  /day10
		/**
		 * web应用路径：部署到tomcat服务器上运行的web应用名称
		 */
		String contextPath = context.getContextPath();
		System.out.println(contextPath);    //--     /day10
		/**
		 * 案例：应用到请求重定向
		 */
		response.sendRedirect(contextPath+"/index.html");//重定向
		context.getRequestDispatcher("servlet名").forward(request,response);//转发
	}
}

```

*	获取Context参数(web参数可以让应用中所有的Servlet获取, 而Servlet参数只能让对应的一个servlet获取)

web.xml配置文件是这样:
```xml
<web-app>
<!-- 配置web应用参数 -->
	<context-param>
		<param-name>AAA</param-name>
		<param-value>AAA's value</param-value>
	</context-param>
	<context-param>
		<param-name>BBB</param-name>
		<param-value>BBB's value</param-value>
	</context-param>
	<context-param>
		<param-name>CCC</param-name>
		<param-value>CCC's value</param-value>
	</context-param>

</web-app>

```
```java
/**
 * 得到web应用参数
 */
public class ContextDemo2 extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		//得到SErvletContext对象
		ServletContext context = this.getServletContext();
		
		System.out.println("参数"+context.getInitParameter("AAA"));
		
		Enumeration<String> enums = context.getInitParameterNames();
		while(enums.hasMoreElements()){
			String paramName = enums.nextElement();
			String paramValue  =context.getInitParameter(paramName);
			System.out.println(paramName+"="+paramValue);
		}
		
		//尝试得到ConfigDemo中的servlet参数，但是得不到, servlet参数不互通
		String path = this.getServletConfig().getInitParameter("path");
		System.out.println("path="+path);
	}
}

```

*	读取应用下的资源文件（如properties文件）

```java
/**
 * 读取web应用下的资源文件（例如properties）
 */
public class ResourceDemo extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		/**
		 * “ .” 代表java命令运行目录。此时 在tomcat/bin目录下
		 *  在web项目中， . 代表在tomcat/bin目录下开始，所以不能使用这种相对路径。
		 */
		/*File file = new File("./src/db.properties");
		FileInputStream in = new FileInputStream(file);*/
		/**
		 * 使用web应用下加载资源文件的方法
		 */
		/**
		 * 1. getRealPath读取,返回资源文件的绝对路径
		 */
		String path = this.getServletContext().getRealPath("/WEB-INF/classes/db.properties");
		System.out.println(path);
		File file = new File(path);
		FileInputStream in = new FileInputStream(file);
		/**
		 * 2. getResourceAsStream()，返回的是输入流
		 */
		InputStream in = this.getServletContext().getResourceAsStream("/WEB-INF/classes/db.properties");
		Properties prop = new Properties();
		//读取资源文件
		prop.load(in);
		String user = prop.getProperty("user");
		String password = prop.getProperty("password");
		System.out.println("user="+user);
		System.out.println("password="+password);
	}
}

```

## 域对象共享数据

<img src="Screenshot_5.png"/>

# jsp九个内置对象/四个域对象,servlet三大域对象

搜索"jsp-note"⬅️

# 三种方式读取资源文件

* servletContext.getRealPath("xxx")
* ResourceBundle.getBundle("xxx")
* this.getClass().getClassLoader().getResourceAsStram("xxx")

demo:

<img src="p16.png"/>
<img src="p17.png"/>
<img src="p18.png"/>

# 转发&重定向

<img src="p19.png"/>
<img src="p20.png"/>

[转发和重定向时候的路径问题](#web项目中的路径问题)

## 调用方式

```
我们知道，在servlet中调用转发、重定向的语句如下：
request.getRequestDispatcher("new.jsp").forward(request, response);//转发到new.jsp
response.sendRedirect("new.jsp");//重定向到new.jsp

在jsp页面中你也会看到通过下面的方式实现转发：
<jsp:forward page="apage.jsp" />

当然也可以在jsp页面中实现重定向：
<%response.sendRedirect("new.jsp");//重定向到new.jsp%>
```

## 本质区别

https://blog.csdn.net/meiyalei/article/details/2129120

转发是服务器行为，重定向是客户端行为

## 代码例子

```java
/**
 * 转发（效果：跳转页面）
 */
public class ForwardDemo1 extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		//转发	
		/**
		 * 注意：不能转发当前web应用以外的资源。
		 * 
		 */
		this.getServletContext().getRequestDispatcher("/GetDateServlet").forward(request, response);// 再 server服务器内部就完成了, so 路径无需加项目名称
		//包含
		this.getServletContext().getRequestDispatcher("").include(request, response);//将目标结果包含到源；
	}

}
/*重定向效果
 */
public class RedirectDemo1 extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		/**
		 * 保存数据到request域对象
		 */
		request.setAttribute("name", "rose");
		//重定向
		/**
		 * 注意：可以跳转到web应用内，或其他web应用，甚至其他外部域名。
		 				可以跳转到 servlet, 页面
		 */
		//response.sendRedirect("/day09/adv.html");
		response.sendRedirect("/day10/GetDataServlet");// 路径需要加上项目名
	}

}

```

# 会话管理


```
1) 会话管理： 管理浏览器客户端 和 服务器端之间会话过程中产生的会话数据。

一次会话： 打开浏览器 -> 访问一些服务器内容 -> 关闭浏览器
 eg ：
登录场景：
	打开浏览器 -> 浏览到登陆页面 -> 输入用户名和密码 -> 访问到用户主页(显示用户名)
购物场景：
	 打开浏览器 -> 浏览商品列表  -> 加入购物车(把商品信息保存下来)  -> 关闭浏览器
	 打开浏览器->  直接进入购物车 -> 查看到上次加入购物车的商品 -> 下订单 -> 支付

但是这里有个问题： 会话过程中，如何保存商品信息或者用户信息？？

域对象： 实现资源之间的数据共享。
	request域对象
	context域对象

尝试解决保存数据的问题
登录场景：
	小张： 输入“张三” （保存数据： context.setAttribute("name","张三")） -> 用户主页（显示“张三”）
	小李： 输入“李四”(保存数据：context.setAttribute("name","李四")) ->     用户主页（显示“李四”）
问题： context是所有用户公有的资源！会覆盖数据。

其实最好的办法：
	可以使用session域对象来保存会话数据

会话技术: 
	Cookie技术：会话数据保存在浏览器客户端。
	Session技术：会话数据保存在服务器端。

2）Cookie技术： 会话数据保存在浏览器客户端。

Cookie核心的API/原理：
	2.1 在服务器端创建Cookie对象
		Cookeie cookie = new Cookie("name","value");
		void setPath(java.lang.String uri)   ：设置cookie的有效访问路径, 当访问的resource不再有效路径, 则不带cookies;
			eg1: cookie.setPath("/day17"), 浏览器再次访问/day17/xxx/xxx.jsp（就是项目/day17下的资源）时，就携带cookie信息
			eg2: cookie.setPath("/");，则表示访问访问任何资源都带cookie信息
		void setMaxAge(int expiry) ： 设置cookie的有效时间
			正整数：表示cookie数据保存浏览器的缓存目录（硬盘中），数值表示保存的时间。
			负整数：表示cookie数据保存浏览器的内存中。浏览器关闭cookie就丢失了！！
			零：表示删除同名的cookie数据
		void setValue(java.lang.String newValue) ：设置cookie的值

	2.2 把cookie发送给浏览器端（隐式发送了一个响应头：set-cookie: name=value）保存起来
		response.addCookie(cookie);
	2.3 再次访问, 浏览器带着cookie信息访问服务器（通过请求头：cookie），服务器得到cookie信息
		Cookie[] cookies = request.getCookies();

局限：
	1）只能保存字符串类型，不能保存中文
	2）可以保存多个cookie，但是浏览器一般只允许存放300个Cookie，每个站点最多存放20个Cookie，每个Cookie的大小限制为4KB
	
3）Session技术：会话数据保存在服务器端(内存中)。服务器能够识别不同的浏览者

Session核心的API/原理：
	3.1 创建或得到session对象    
		HttpSession session = request.getSession(); //创建或得到session对象, 若没有匹配的session编号, 则返回新创建的session
		request.getSession(false); //得到session对象, 若没有匹配的编号, 返回null
	3.2 会话数据保存session对象中，和得到会话数据
		session.setAttribute("name",Object);  保存数据
		session.getAttribute("name")   得到数据
		void removeAttribute(java.lang.String name) ： 清除数据
	3.3 设置session对象
		void setMaxInactiveInterval(int interval)  ： 设置session的有效时间
			默认情况30分钟服务器自动回收
			<!-- 修改session全局有效时间:   分钟； 在<web-app>里面-->
			<session-config>
                <session-timeout>1</session-timeout>
        	</session-config>

		void invalidate()     ： 手动销毁session对象
		String getId()  ： 得到session编号

session原理:

1）第一次访问创建session对象，给session对象分配一个唯一的ID，叫JSESSIONID
2）把JSESSIONID作为Cookie的值发送给浏览器保存
		Cookie cookie = new Cookie("JSESSIONID", sessionID);
		response.addCookie(cookie);
3）第二次访问的时候，浏览器带着JSESSIONID的cookie访问服务器
4）服务器得到JSESSIONID，在服务器的内存中搜索是否存放对应编号的session对象。
	即搜索这样的数据结构
	Map<String,HttpSession>
	<"s001", s1>
	<"s001,"s2>

	if(找到){
		return map.get(sessionID);
	}
	else {
		创建新的session对象，继续走1的流程
	}
		
结论：通过存储早浏览器中的JSESSION的cookie值在服务器找session对象

问题: 如何避免浏览器的JSESSIONID的cookie随着浏览器关闭而丢失的问题
/**
 * 手动发送一个硬盘保存的cookie给浏览器
 */
Cookie c = new Cookie("JSESSIONID",session.getId());
c.setMaxAge(60*60);
response.addCookie(c);


```

cookies demo:

```java
/**
 * 第一个cookie的程序
 */
public class CookieDemo1 extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		//1.创建Cookie对象
		Cookie cookie1 = new Cookie("name","eric");
		//Cookie cookie2 = new Cookie("email","jacky@qq.com");
		//Cookie cookie1 = new Cookie("email","eric@qq.com");
		/**
		 * 1）设置cookie的有效路径。默认情况：有效路径在当前web应用下。 /day11
		 */
		//cookie1.setPath("/day11");
		//cookie2.setPath("/day12");
		/**
		 * 2)设置cookie的有效时间
		 * 正整数：表示cookie数据保存浏览器的缓存目录（硬盘中），数值表示保存的时间。
			负整数：表示cookie数据保存浏览器的内存中。浏览器关闭cookie就丢失了！！
			零：表示删除同名的cookie数据
		 */
		//cookie1.setMaxAge(20); //20秒，从最后不调用cookie开始计算
		cookie1.setMaxAge(-1); //cookie保存在浏览器内存（会话cookie）
		//cookie1.setMaxAge(0);
		
		//2.把cookie数据发送到浏览器（通过响应头发送： set-cookie名称）
		//response.setHeader("set-cookie", cookie.getName()+"="+cookie.getValue()+",email=eric@qq.com");
		//推荐使用这种方法，避免手动发送cookie信息
		response.addCookie(cookie1);
		//response.addCookie(cookie2);
		//response.addCookie(cookie1);
		
		//3.接收浏览器发送的cookie信息
		String name = request.getHeader("cookie");
		System.out.println(name);
		//获取所有的cookies
		Cookie[] cookies = request.getCookies();
		//注意：判断null,否则空指针
		if(cookies!=null){
			//遍历
			for(Cookie c:cookies){
				String name = c.getName();
				String value = c.getValue();
				System.out.println(name+"="+value);
			}
		}else{
			System.out.println("没有接收cookie数据");
		}
	}
}

```

session demo: 

```java
/**
 * 保存会话数据到session域对象
 */
public class SessionDemo1 extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		//1.创建session对象
		HttpSession session = request.getSession();
		/**
		 * 得到session编号
		 */
		System.out.println("id="+session.getId());
		/**
		 * 修改session的有效时间
		 */
		session.setMaxInactiveInterval(20);
		/**
		 * 手动发送一个硬盘保存的cookie给浏览器
		 */
		Cookie c = new Cookie("JSESSIONID",session.getId());
		c.setMaxAge(60*60);
		response.addCookie(c);
		
		//2.保存会话数据
		session.setAttribute("name", "rose");
	}
}

/**
 * 从session域对象中取出会话数据
 */
public class SessionDemo2 extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		//1.得到session对象
		HttpSession session = request.getSession(false);
		if(session==null){
			System.out.println("没有找到对应的sessino对象");
			return;
		}
		/**
		 * 得到session编号
		 */
		System.out.println("id="+session.getId());
		
		//2.取出数据
		String name = (String)session.getAttribute("name");
		System.out.println("name="+name);
	}
}
/**
 * 销毁session对象
 */
public class DeleteSession extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		HttpSession session = request.getSession(false);
		if(session!=null){
			session.invalidate();//手动销毁
		}
		System.out.println("销毁成功");
	}
}

```

案例: 上次访问时间

```java
/**
 * 案例-用户上次访问时间
 */
public class HistServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		//获取当前时间
		SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
		String curTime = format.format(new Date());
		
		//取得cookie
		Cookie[] cookies = request.getCookies();
		String lastTime = null;
		/**
		 * 第n次访问
		 */
		if(cookies!=null){
			for (Cookie cookie : cookies) {
				if(cookie.getName().equals("lastTime")){
					//有lastTime的cookie，已经是第n次访问
					lastTime = cookie.getValue();//上次访问的时间
					//第n次访问
					//1.把上次显示时间显示到浏览器
					response.getWriter().write("欢迎回来，你上次访问的时间为："+lastTime+",当前时间为："+curTime);
					//2.更新cookie
					cookie.setValue(curTime);
					cookie.setMaxAge(1*30*24*60*60);
					//3.把更新后的cookie发送到浏览器
					response.addCookie(cookie);
					break;
				}
			}
		}
		/**
		 * 第一次访问（没有cookie 或 有cookie，但没有名为lastTime的cookie）
		 */
		if(cookies==null || lastTime==null){
			//1.显示当前时间到浏览器
			response.getWriter().write("你是首次访问本网站，当前时间为："+curTime);
			//2.创建Cookie对象
			Cookie cookie = new Cookie("lastTime",curTime);
			cookie.setMaxAge(1*30*24*60*60);//保存一个月，单位s
			//3.把cookie发送到浏览器保存
			response.addCookie(cookie);
		}
	}
}

```

案例: 登录/注销场景

```java
/**
 * 处理登录的逻辑
 */
public class LoginServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		request.setCharacterEncoding("utf-8");
		//1.接收参数
		String userName = request.getParameter("userName");
		String userPwd = request.getParameter("userPwd");
		
		//2.判断逻辑（这里写死了，正常应该要到数据库中比对）
		if("eric".equals(userName)
				 && "123456".equals(userPwd)){
			//登录成功
			/**
			 * 分析：选取哪个域对象？
			 * 	  context域对象：不合适，可能会覆盖数据。
			 *    request域对象： 不合适，整个网站必须得使用转发技术来跳转页面
			 *    session域对象：合适，每个用户对应一个session。
			 */
			/**
			 * 一、登录成功后，把用户数据保存session对象中
			 */
			//1.创建session对象
			HttpSession session = request.getSession();
			//2.把数据保存到session域中
			session.setAttribute("loginName", userName);
			//3.跳转到用户主页
			response.sendRedirect(request.getContextPath()+"/IndexServlet");
		}else{
			//登录失败
			//请求重定向
			response.sendRedirect(request.getContextPath()+"/fail.html");
		}
	}
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		doGet(request, response);
	}
}
/**
 * 用户主页的逻辑
 */
public class IndexServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		PrintWriter writer = response.getWriter();
		
		String html = "";
		/**
		 * 二、在用户主页，判断session不为空且存在指定的属性才视为登录成功！才能访问资源。
		 * 从session域中获取会话数据
		 */
		//1.得到session对象
		HttpSession session = request.getSession(false);
		if(session==null){
			//没有登录成功，跳转到登录页面
			response.sendRedirect(request.getContextPath()+"/login.html");
			return;
		}
		//2.取出会话数据
		String loginName = (String)session.getAttribute("loginName");
		if(loginName==null){
			//没有登录成功，跳转到登录页面
			response.sendRedirect(request.getContextPath()+"/login.html");
			return;
		}
		html = "<html><body>欢迎回来，"+loginName+"，<a href='"+request.getContextPath()+"/LogoutServlet'>安全退出</a></body></html>";
		writer.write(html);
	}
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		doGet(request, response);
	}
}
/**
 * 退出逻辑
 */
public class LogoutServlet extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		/**
		 * 三、安全退出：
		 * 		删除掉session对象中指定的loginName属性即可！  
		 */
		//1.得到session对象
		HttpSession session = request.getSession(false);
		if(session!=null){
			//2.删除属性
			session.removeAttribute("loginName");
		}
		//2.回来登录页面
		response.sendRedirect(request.getContextPath()+"/login.html");
	}
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		doGet(request, response);
	}
}

```

# 过滤器filter

[filter排除指定servlet](https://blog.csdn.net/hanghangde/article/details/51298221)

filter init() 会在程序启动时执行, 而 dofilter() 会在拦截请求成功时执行

过滤器，设计执行流程：

1. 用户访问服务器
2. 过滤器： 对Servlet请求进行拦截
3. 先进入过滤器， 过滤器处理
4. 过滤器处理完后， 再放行， 此时，请求到达Servlet/JSP
5. Servlet处理
6. Servlet处理完后，再回到过滤器, 最后在由tomcat服务器回应用户

<img src="p21.png"/>

## 传统配置方法开发 filter

1. 编写java类实现Filter接口，并实现其doFilter方法。

2. 在web.xml文件中使用`<filter>`和`<filter-mapping>`元素对编写的filter类进行注册，并设置它所能拦截的资源。

```java
/**
 * 过滤器，测试
 */
public class HelloFilter implements Filter{
	// 创建实例
	public HelloFilter(){
			System.out.println("1. 创建过滤器实例");
	}
 
	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
			System.out.println("2. 执行过滤器初始化方法");
			// 获取过滤器在web.xml中配置的初始化参数
			String encoding = filterConfig.getInitParameter("encoding");
			System.out.println(encoding);
			// 获取过滤器在web.xml中配置的初始化参数 的名称
			Enumeration<String> enums =  filterConfig.getInitParameterNames();
			while (enums.hasMoreElements()){
					// 获取所有参数名称：encoding、path
					String name = enums.nextElement();
					// 获取名称对应的值
					String value = filterConfig.getInitParameter(name);
			System.out.println(name + "\t" + value);
			}
	}
 
	// 过滤器业务处理方法： 在请求到达servlet之前先进入此方法处理公用的业务逻辑操作
	@Override
	public void doFilter(ServletRequest request, ServletResponse response,
					FilterChain chain) throws IOException, ServletException {
			System.out.println("3. 执行过滤器业务处理方法");
			// 放行 (去到Servlet)
			// 如果有下一个过滤器，进入下一个过滤器，否则就执行访问servlet
			chain.doFilter(request, response);     //必须要此行代码才能访问资源
			System.out.println("5. Servlet处理完成，又回到过滤器");
	}
 
	@Override
	public void destroy() {
			System.out.println("6. 销毁过滤器实例");
	}
 
}


```

```xml
<!-- 过滤器配置 -->
<filter>
	<!-- 配置初始化参数 -->
	<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
	</init-param>
	<init-param>
			<param-name>path</param-name>
			<param-value>c:/...</param-value>
	</init-param>
	<!-- 内部名称 -->
	<filter-name>hello_filter</filter-name>
	<!-- 过滤器类的全名 -->
	<filter-class>cn.itcast.a_filter_hello.HelloFilter</filter-class>
</filter>

<filter-mapping>
	<!-- filter内部名称 -->
	<filter-name>hello_filter</filter-name>
	<!-- 拦截资源, /* 表示对所有请求过滤 -->
	<url-pattern>/*</url-pattern>

	<!-- 1. 拦截所有
	<url-pattern>/*</url-pattern>
	 -->
	 
	 <!-- 2. 拦截指定的jsp ，拦截多个资源
	 <url-pattern>/index.jsp</url-pattern>
	 <url-pattern>/list.jsp</url-pattern>
	 -->
	 <!-- 拦截所有的jsp
	 <url-pattern>*.jsp</url-pattern>
	  -->
	  <!-- 3. 根据servlet的内部名称拦截
	  <servlet-name>IndexServlet</servlet-name>
	   -->
	  <!-- 拦截指定的servlet 
	  <url-pattern>/index</url-pattern>
	  -->
	  
	  <!-- 4. 拦截指定的类型，<dispatcher>指定过滤器所拦截的资源被 Servlet 容器调用的方式，可以是
	  		REQUEST(默认): 拦截浏览器直接访问or重定向,
			INCLUDE: 拦截转发,
			FORWARD: 拦截包含的页面(RequestDispatcher.include("/page.jsp")),
			ERROR: 拦截声明式异常信息
	，用户可以设置多个<dispatcher> 子元素用来指定 Filter 对资源的多种调用方式进行拦截
 -->
	  <!-- 
	  <url-pattern>/*</url-pattern>
	  <dispatcher>REQUEST</dispatcher>
	  <dispatcher>FORWARD</dispatcher>
	   -->

</filter-mapping>

```

## 案例:统一处理编码

```java
过滤器：
 
/**
 * 编码处理统一写到这里(servlet中不需要再处理编码)
 */
public class EncodingFilter implements Filter {
 
	// 过滤器业务处理方法：包含的是公用的业务逻辑操作
	@Override
	public void doFilter(ServletRequest req, ServletResponse res,
					FilterChain chain) throws IOException, ServletException {
		// 转型，
		final HttpServletRequest request = (HttpServletRequest) req;    
		HttpServletResponse response = (HttpServletResponse) res;
		// 一、处理公用业务（公共代码）// 但是只对POST提交有效
		request.setCharacterEncoding("UTF-8");                    
		response.setContentType("text/html;charset=UTF-8");
		/*
		 * 当时GET提交方式时，出现中文乱码，是因为在request.getParameter方法内部没有进行提交方式判断并处理。
		 * String name = request.getParameter("userName");
		 * 
		 * 解决：对指定接口的某一个方法进行功能扩展，可以使用代理!
		 *      对request对象(目标对象)，创建代理对象！
		 */
		HttpServletRequest proxy =  (HttpServletRequest) Proxy.newProxyInstance(
			request.getClass().getClassLoader(),         // 指定当前使用的类加载器
			new Class[]{HttpServletRequest.class},         // 目标对象实现了哪些接口类型
			new InvocationHandler() {                    // 事件处理器
				@Override
				public Object invoke(Object proxy, Method method, Object[] args)
								throws Throwable {
					// 定义方法返回值
					Object returnValue = null;
					// 获取方法名
					String methodName = method.getName();
					// 判断：对getParameter方法添加进行GET提交中文处理的代码
					if ("getParameter".equals(methodName)) {
						// 获取请求数据值【 <input type="text" name="userName">】
						String value = request.getParameter(args[0].toString());    // 调用目标对象的getPrameter()方法得到未处理的结果
						// 获取提交方式 GET   POST
						String methodSubmit = request.getMethod(); // 直接调用目标对象的方法
						// 判断如果是GET提交，需要对数据进行处理  (POST提交已经处理过了)
						if ("GET".equals(methodSubmit)) {
							if (value != null && !"".equals(value.trim())){
								// 如果不空，处理GET中文
								value = new String(value.getBytes("ISO8859-1"),"UTF-8");
							}
						} 
						
						/*这里还可以插入特殊词过滤的代码*/
						
						return value;
					}
					else {
							// 执行request对象的其他方法
						returnValue = method.invoke(request, args);
					}
					return returnValue;
				}
			});
		// 二、放行 (执行下一个过滤器或者servlet)
		chain.doFilter(proxy, response);        // 传入request的代理对象
	}
 
	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
	}
 
	@Override
	public void destroy() {
	}
}

```

过滤器配置:

```xml
过滤器配置：
<!-- 编码处理过滤器配置 -->
<filter>
		<filter-name>encoding</filter-name>
		<filter-class>cn.itcast.a_loginFilter.EncodingFilter</filter-class>
</filter>
<filter-mapping>
		<filter-name>encoding</filter-name>
		<url-pattern>/*</url-pattern>
</filter-mapping>

```

## 案例:登录过滤

```java
//  过滤器，根据访问页面做出不同处理
public class LoginFilter implements Filter {
 
	public void destroy() {
			// TODO Auto-generated method stub
	}
	public void init(FilterConfig arg0) throws ServletException {
			// TODO Auto-generated method stub
	}
	public void doFilter(ServletRequest arg0, ServletResponse arg1,
		FilterChain arg2) throws IOException, ServletException {
			
		 String uri = null;
		 
		HttpServletRequest request = (HttpServletRequest)arg0;
		HttpServletResponse response = (HttpServletResponse)arg1;
		
		String requestUri = request.getRequestURI();
		String requestPath = requestUri.substring(requestUri.lastIndexOf("/") + 1, requestUri.length());
		if("login".equals(requestPath) || "login.jsp".equals(requestPath)) {
			//如果访问的是login.jsp, 放行
				arg2.doFilter(request, response);
		}else {
			//如果不是
			HttpSession session = request.getSession(false);
			if(session != null) {//判断是否登录
				Admin admin = (Admin)session.getAttribute("loginInfo");
				if(admin != null) {
						arg2.doFilter(request, response);
				}else {
						uri = "/login.jsp";
				}
			}else {
					uri = "/login.jsp";
			}
			request.getRequestDispatcher(uri).forward(request, response);
		}
	}
}

```

## 案例:自动登录

分析: 
* 需要使用的cookie的技术，保存用户名和密码
    * 用户如果选择了自动登录的功能，才会保存cookie     
* 需要使用Filter的技术，完成自动登陆的功能

<img src="Screenshot_6.png"/>

```java
loginServlet代码：

/**
 * 登录功能
 */
public class LoginServlet extends HttpServlet {
 
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		/**
		 * 接收用户名和密码
		 * 通过用户名和密码去数据库中查询
		 *  * 如果查询到了用户，说明没有问题，登录成功
		 *  * 如果查询不到用户，说明登录失败
		 */
		// 接收用户名和密码
		String username = request.getParameter("username");
		String password = request.getParameter("password");
		
		// 调用持久层查询数据库
		UserDao dao = new UserDao();
		try {
			// 登录功能
			User existUser = dao.login(username, password);
			// 如果，返回existUser为null，说明登录失败
			if(existUser == null){
				request.setAttribute("msg", "用户名或者密码错误");
				request.getRequestDispatcher("/pages/login.jsp").forward(request, response);
			}else{// 登录, session中保存user
				// 先判断，用户是否已经勾选自动登录
				String autologin = request.getParameter("autologin");
				if("auto_ok".equals(autologin)){// 如果勾选
					// 把用户名和密码，保存到cookie中，回写cookie, 保存到客户端
					String value = username+"#xxx#"+password;
					Cookie cookie = new Cookie("autologin",value);
					// 设置cookie的一些属性
					// 设置cookie的有效时间，单位是秒, 1h之内能够自动登录
					cookie.setMaxAge(60*60);
					// 设置cookie的有效路径
					cookie.setPath("/");
					// 回写cookie
					response.addCookie(cookie);
				}
				// 保存user
				request.getSession().setAttribute("existUser", existUser);
				response.sendRedirect(request.getContextPath()+"/pages/home.jsp");
			}
			
		} catch (SQLException e) {
			e.printStackTrace();
		}
	}
 
	public void doPost(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		doGet(request, response);
	}
 
}

Filter代码：

/**
 * 自动登录的过滤器
 */
public class AutologinFilter implements Filter{
	
	public void doFilter(ServletRequest request, ServletResponse response,
			FilterChain chain) throws IOException, ServletException {
		
		// 从session中获取到用户
		HttpServletRequest req = (HttpServletRequest) request;
		HttpSession session = req.getSession();
		// 获取用户
		User user = (User) session.getAttribute("existUser");
		// 如果user不为null
		if(user != null){
			// 放行
			chain.doFilter(req, response);
		}else{
			// 说明，用户为null了
			// 获取指定名称的cookie
			Cookie[] cookies = req.getCookies();
			// 通过指定名称查找cookie
			Cookie cookie = MyCookieUtil.findCookieByName(cookies, "autologin");
			// 判断
			if(cookie == null){
				// 说明，没找到，登录时没有勾选自动登录功能,放行
				chain.doFilter(req, response);
			}else{
				// 说明已经找到了该cookie
				// 获取到cookie中的value
				String value = cookie.getValue();
				String username = value.split("#xxx#")[0];
				String password = value.split("#xxx#")[1];
				// 通过用户名和密码去数据库查询
				UserDao dao = new UserDao();
				
				try {
					// 查询用户
					User existUser = dao.login(username, password);
					// 判断，如果用户为null，说明登录失败了
					if(existUser == null){
						// 放行
						chain.doFilter(req, response);
					}else{
						// 不为null，登录成功,帮用户做自动登录功能
						session.setAttribute("existUser", existUser);
						// 放行
						chain.doFilter(req, response);
					}
				} catch (SQLException e) {
					e.printStackTrace();
				}
			}
		}
	}
	
	public void destroy() {
	}
	public void init(FilterConfig arg0) throws ServletException {
	}
}
```

## spring boot 中使用filter

spring 中开发 filter 继承 抽象类 GenericFilterBean , 该类提供了 spring 中的许多信息, 实现了如下 5 个接口:

```java
javax.servlet.Filter、
org.springframework.beans.factory.BeanNameAware、
org.springframework.context.EnvironmentAware、
org.springframework.web.context.ServletContextAware、
org.springframework.beans.factory.InitializingBean
org.springframework.beans.factory.DisposableBean
```


如果下往所有的请求只拦截一次, 推荐 OncePerRequestFilter, 并不是所有的container都像我们期望的对于加入了filter的请求只过滤一次，servlet版本不同，表现也不同, 比如 服务器内部使用forward转发的请求, servlet-2.3 都会拦截, 但是 servlet-2.4中Filter默认下只拦截外部提交的请求，forward和include这些内部转发都不会


========================原始的 filter 开发如下:

```java

//启动类需要配置 servlet 扫描 @ServletComponentScan(basePackages={""})
////////////////////////////////////

@WebFilter(urlPatterns="/*",filterName="helloFilter",initParams= {@WebInitParam(name="paramName",value="paramValue"),@WebInitParam(name="paramName2",value="paramValue2")})
@order(0)
public class UrlFilter implements Filter {
 
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
 
        System.out.println("----------------------->过滤器被创建");
    }
 
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
 
        HttpServletRequest req = (HttpServletRequest) servletRequest;
        String requestURI = req.getRequestURI();
        System.out.println("--------------------->过滤器：请求地址"+requestURI);
        if(!requestURI.contains("info")){
            servletRequest.getRequestDispatcher("/failed").forward(servletRequest, servletResponse);
        }else{
            filterChain.doFilter(servletRequest, servletResponse);
        }
    }
 
    @Override
    public void destroy() {
 
        System.out.println("----------------------->过滤器被销毁");
    }
}
```

如果不使用注解, 可以在配置类中指定 interceptor

```java
@Configuration   //相当于spring中的<beans>标签
public class WebConfiguration {

	@Bean   //相当于spring中<bean>标签
	public FilterRegistrationBean<MyFilter> testFilterRegistration() {
		FilterRegistrationBean<MyFilter> registration = new FilterRegistrationBean<>();
		registration.setFilter(new MyFilter());
		registration.addUrlPatterns("/*");//配置过滤路径
		registration.addInitParameter("paramName", "paramValue");//添加初始值
		registration.setName("myFilter");//设置filter名称
		registration.setOrder(1);//请求中过滤器执行的先后顺序，值越小越先执行
		return registration;
	}
```


# 监听器listener

## 概念

监听器：主要是用来监听特定对象的创建或销毁、属性的变化的. 是一个实现特定接口的普通java类

主要是一些容器创建的对象需要监听, 如request / session / servletContext, 分别对应的是request监听器、session相关监听器、servletContext监听器

## 有哪些监听器

```

三类:

1. ServletContext 监听 (创建, 销毁, 属性变化)

	Interface ServletContextListener     监听servletContext对象的创建或销毁
	Interface ServletContextAttributeListener  监听servletContext对象属性变化

2. HttpSession 监听 (创建, 销毁, 属性变化)

	Interface HttpSessionListener        监听session对象的创建或销毁
	Interface HttpSessionAttributeListener    监听session对象属性变化: 添加、移除、修改

3. ServletRequest 监听 (创建, 销毁, 属性变化)

	Interface ServletRequestListener     监听request对象的创建或销毁
	Interface ServletRequestAttributeListener 监听request对象属性变化: 添加、移除、修改

session对象绑定相关监听器

	Interface HttpSessionBindingListener   监听对象绑定到session上的事件    
				HttpSessionListener只需要设置到web.xml中就可以监听整个应用中的所有session。
				 HttpSessionBindingListener必须实例化后通过session.setAttribute放入某一个session中，才可以进行监听。

```

## 生命周期监听器

```java
/**
 *  监听request对象的创建或销毁
 */
public class MyRequestListener implements ServletRequestListener{
 
	// 对象销毁
	@Override
	public void requestDestroyed(ServletRequestEvent sre) {
			// 获取request中存放的数据
			Object obj = sre.getServletRequest().getAttribute("cn");
			System.out.println(obj);
			System.out.println("MyRequestListener.requestDestroyed()");
	}
 
	// 对象创建
	@Override
	public void requestInitialized(ServletRequestEvent sre) {
			System.out.println("MyRequestListener.requestInitialized()");
	}
}

```
```xml
<!-- Web.xml , <web-app>下 -->
<!-- 监听request对象创建、销毁 -->
<listener>
		<listener-class>cn.itcast.a_life.MyRequestListener</listener-class>
</listener>

<!-- Jsp文件： -->
<%request.setAttribute("cn","China");%>
```

## 属性监听器

监听session域属性变化

```java
/**
 * 监听session对象属性的变化
 */
public class MySessionAttrListener implements HttpSessionAttributeListener {

	// 属性添加
	@Override
	public void attributeAdded(HttpSessionBindingEvent se) {
		// 先获取session对象
		HttpSession session = se.getSession();
		// 获取添加的属性
		Object obj = session.getAttribute("userName");
		// 测试
		System.out.println("添加的属性：" + obj);
	}

	// 属性移除
	@Override
	public void attributeRemoved(HttpSessionBindingEvent se) {
		System.out.println("属性移除");
	}

	// 属性被替换
	@Override
	public void attributeReplaced(HttpSessionBindingEvent se) {
		// 获取sesison对象
		HttpSession session = se.getSession();
		
		// 获取替换前的值
		Object old = se.getValue();
		System.out.println("原来的值：" + old);
		
		// 获取新值
		Object obj_new = session.getAttribute("userName");
		System.out.println("新值：" + obj_new);
		
	}

}

```

## session绑定监听器

```java
/* HttpSessionBindingListener   
监听对象绑定/解除绑定到sesison上的事件!
 
步骤：
	对象实现接口
	session.setAttribute("onlineUserBindingListener", new OnlineUserBindingListener(username));
	不用web.xml配置(原因:因为监听的对象是自己创建的对象，不是服务器对象！不用配置web.xml)
典型应用：
	(登陆后,显示上线提醒！) */
 
/**
 * 监听此对象绑定到session上的过程，需要实现session特定接口
 */
public class Admin implements HttpSessionBindingListener {
     省略getter/setter…………
// 对象放入session
        @Override
        public void valueBound(HttpSessionBindingEvent event) {
                System.out.println("Admin对象已经放入session");
				这里可以显示上线提醒
        }
        // 对象从session中移除
        @Override
        public void valueUnbound(HttpSessionBindingEvent event) {
                System.out.println("Admin对象从session中移除！");
        }
}
 
```

实现用户上线提醒, 分析:
需求：做一个在线列表提醒的功能!
显示登陆信息，列表展示。(list.jsp)
显示在线用户列表        (list.jsp)
列表点击进入“在线列表页面”   onlineuser.jsp

实现：
1. 先增加退出功能；  再把session活跃时间1min;
2. 写监听器，监听servletContext对象的创建： 初始化集合(onlineuserlist)
3. 登陆功能： 用户登陆时候，把数据保存到servletContext中的onlineList集合
4. List.jsp  增加超链接， 点击时候提交直接跳转到online.jsp
5. 写监听器： 监听session销毁，把当前登陆用户从onlineuserlist移除！



# i18n

# 文件上传下载

怎么获取表单中的数据? 有2中情况:

## form中只有纯文本数据

```java
String username = request.getParameter("username");
String sex = request.getParameter("sex");
String education = request.getParameter("education");
String[] hobby = request.getParameterValues("hobby");

```

数据编码： 对于纯文本的表单，其字段enctype=application/x-www-form-urlencoded 是默认值，表示表单的数据进行url编码，如：表单的数据被编码成username=123&password=222 。

传输方式： 数据传输方式method ： 如果使用get ，那么该字符串会追加到请求的url地址后面；而如果使用post ，那么打开调试台，查看Form Data 项也可以查看到字符串。

## form中含有文本和二进制文件

具体参考 [这里](https://docs.oracle.com/javaee/7/tutorial/servlets011.htm#BABFGCHB))

比如:

```html
<form action="/webapp/formServlet" method="post" enctype="multipart/form-data">
    <input type="text" name="username">
    <input type="password" name="password">
    <input type="file" name="file">
    <input type="submit" value="提交">
</form>

```

servlet 3.0 将二进制文件封装为 Part 对象

传输方式： 因为是二进制流， 所以method 只能使用post 进行提交，无法使用get 。


# javamail邮件开发

# servlet3.0

[main conception (sos)](https://stackoverflow.com/questions/1638865/what-are-the-differences-between-servlet-2-5-and-3)

https://developer.ibm.com/zh/articles/j-lo-servlet30/

## 异步支持

### 实现请求线程池隔离

https://blog.csdn.net/sumlyl/article/details/108749779
https://blog.csdn.net/liyantianmin/article/details/103760241
https://www.jianshu.com/p/0529c126e166?utm_campaign


## 插件式开发



## 注解开发


```java
////////////////
//     web.xml 可选, 新增注解
/////////////////

@WebServlet(name="myServlet",
    urlPatterns={"/path/to/my/servlet"},
    initParams={@InitParam(name="configFile", value="config.xml")})
public class MyAwesomeServlet extends HttpServlet { ... }

@ServletFilter(name="myFilter", urlPatterns={"/path/to/my/filter"})
public class MyAwesomeFilter implements Filter { ... }

@WebServletContextListener
public class MyAwesomeListener implements ServletContextListener { ... }

/////////////////
//    Modularization of web.xml
// n servlets 3, each "loadable" jar can have a web-fragment.xml in its META-INF directory specifying servlets, filters, etc.
//////////////////////

/////////////////
//       动态增加 servlets, filters and listeners
// following methods added to SevletContext: addServlet(), addFilter() and addListener()
///////////////////

///////////////////
//     异步的 servlet
/////////////////////
// servlet 2.5
public class MyAwesomeServlet extends HttpSerlvet {

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response) {
        // ...

        runSlowProcess();
        // no async support, thread will be free when runSlowProcess() and
        // doGet finish

        // ...
    }

}
// servlet 3.0
@WebServlet(name="myServlet",
             urlPatterns={"/mySlowProcess"},
             asyncSupported=true) // asyncSupported MUST be specified for
                                  // servlets that support asynchronous
                                  // processing
public class MyAwesomeServlet extends HttpSerlvet {

    @Override
    public void doGet(HttpServletRequest request, HttpServletResponse response) {


        // an AsyncContext is created, now the response will be completed
        // not when doGet finalizes its execution, but when
        // myAsyncContext.complete() is called.
        AsyncContext myAsyncContext = request.startAsync(request, response);

        // ...

        // myAsyncContext is passed to another thread
        delegateExecutionToProcessingThread(myAsyncContext);

        // done, now this thread is free to serve another request
    }

}

// ... and somewhere in another part of the code:

public class MyProcessingObject {

    public void doSlowProcess() {

        // ...

        runSlowProcess();
        myAsyncContext.complete(); // request is now completed.

        // ...

    }
}

// 另外的一个示例

@WebServlet(urlPatterns = "/demo", asyncSupported = true)
public class AsyncDemoServlet extends HttpServlet {
    @Override
    public void doGet(HttpServletRequest req, HttpServletResponse resp)
    throws IOException, ServletException {
        resp.setContentType("text/html;charset=UTF-8");
        PrintWriter out = resp.getWriter();
        out.println("进入Servlet的时间：" + new Date() + ".");
        out.flush();
 
        //在子线程中执行业务调用，并由其负责输出响应，主线程退出
        AsyncContext ctx = req.startAsync();
        new Thread(new Executor(ctx)).start();
 
        out.println("结束Servlet的时间：" + new Date() + ".");
        out.flush();
    }
}
 
public class Executor implements Runnable {
    private AsyncContext ctx = null;
    public Executor(AsyncContext ctx){
        this.ctx = ctx;
    }
 
    public void run(){
        try {
            //等待十秒钟，以模拟业务方法的执行
            Thread.sleep(10000);
            PrintWriter out = ctx.getResponse().getWriter();
            out.println("业务处理完毕的时间：" + new Date() + ".");
            out.flush();
            ctx.complete();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

////////////////////
//    可编程的 登陆 登出
// In servlets 3, the interface HttpServletRequest has been added two new methods: login(username, password) and logout().	
///////////////////////

```

# xxxWapper

HttpServletRequestWrapper ...

尽管不能改变ServletRequest不变对象本身，但却可以通过使用装饰模式(创建xxxWapper)来改变其状态. 一般和 filter 联合使用

http://uule.iteye.com/blog/1947192

# servlet和线程安全问题

https://www.cnblogs.com/chanshuyi/p/5052426.html
http://tutorials.jenkov.com/java-servlets/servlet-concurrency.html

```js
if (同时有多个 http request 同一个 servlet
			&& 这个 servlet定义了 成员变量/静态变量) {
	存在线程安全问题
}
```

[并发访问 servlet](#Servlet多线程并发访问)
