---
title: Jsp note
tags:
  - template
date: 2016-04-13 18:21:15
categories: java web
---

<div align="center">
'JSP note (jstl, el, 内置对象...)'
</div>
<!--more-->

<!-- TOC -->

- [总结](#总结)
- [jsp特性](#jsp特性)
- [jsp执行过程](#jsp执行过程)
- [jsp和servlet对比](#jsp和servlet对比)
- [jsp基本语法](#jsp基本语法)
- [jsp三指令(include,page,taglib)](#jsp三指令includepagetaglib)
    - [include](#include)
- [page](#page)
- [taglib](#taglib)
- [jsp标签](#jsp标签)
    - [作用](#作用)
    - [分类](#分类)
    - [内置标签](#内置标签)
    - [jstl标签](#jstl标签)
    - [自定义标签](#自定义标签)
    - [操作javabean](#操作javabean)
- [九个内置对象, 四个域对象](#九个内置对象-四个域对象)
    - [内置对象](#内置对象)
        - [out](#out)
        - [pageContext对象](#pagecontext对象)
    - [四个域对象](#四个域对象)
- [EL表达式](#el表达式)
- [常用的套路](#常用的套路)

<!-- /TOC -->

# 总结

```
1）Jsp的9大内置对象
	request       HttpServletRequet
	response     HttpServletResponse
	config         ServletConfig
	application     ServletContext
	exception      Throwable
	page          Object(翻译过来的java源文件)
	pageContext    PageContext
	out              JspWriter
	session        HttpSession
 
2）Jsp的4个域对象
	request    
	session
	application
	pageContext
	作用范围：
		pageContext ： 处于当前jsp页面中有效的！！
		request：      处于同一个请求中有效的！！
		session：      处于同一个会话中有效的！
		application：   处于同一个web应用中有效的！
3）EL表达式
	替代jsp表达式，用于向浏览器输出域对象中的变量值和表达式计算的结果。
	语法：
		${变量}
	  3.1 输出普通字符串： ${name}
	  3.2 输出对象属性：  ${student.name}  注意： .name 相当于  .getName（）方法
	  3.3 输出List集合：   ${list[0].name }   注意： [0]  相当于 get（下标）方法
	  3.4 输出map集合：  ${map[‘key’].name}  注意：　［key］相当于get（key）方法
 
4）jsp标签
	替代jsp脚本，用于在jsp页面中执行java代码
 
	4.1 内置标签：
		<jsp:foward/>   request.getRequesetDipsacher("/路径").foward(request,response);
		  <jsp:param/>   参数标签    ？name=eric
		<jsp:include/>   包含其他页面 ，动态包含
			静态包含： 先合并再翻译。不能传递参数
			动态包含： 先翻译再合并。可以传递参数
 
	4.2 jstl标签库 （java标准标签库）
		使用步骤：
			1）确保jstl支持的jar包存在于项目中
			2）在jsp页面中导入标签库
				 <%@taglib uri="标签库声明文件tld文件的标记" prefix="前缀"%>
			3)使用标签库中的标签
		核心标签库：
			<c:set />     保存数据到域对象中
			<c:out/>     从域中取出数据
			<c:if/>       单条件判断
			<c:choose/> + <c:when/> + <c:otherwise/>  多条件判断
			<c:forEach />  遍历数据 
			<c:forTokens/>  遍历特殊字符串
			<c:redirect/>   重定向
4.3自定义标签&编码实战
1）自定义标签步骤：
		1.1 编写标签处理器类，继承SimpleTagSupport类，覆盖doTag方法
		1.2 在WEB-INF目录下建立tld文件，在tld配置标签
		1.3 在jsp页面导入标签库，使用taglib指令
		1.4 在jsp页面中使用标签库中的标签
2）自定义标签生命周期：
		SimpleTag接口：
			setJspContext(JspContext context)  --传入pagContext对象
			setParent(JspTag tag)   --传入父标签对象
			setXXX(参数)     --给属性赋值
			setJspBody(JspFrament jspBody)  --传入标签体内容
			doTag()      --执行标签
3）自定义标签的作用：
		3.1 控制是否输出标签体内容
			输出： this.getJspBody().invoke(null)  
		  不输出:  不调用invoke(null)方法
		3.2 控制标签余下内容是否输出
			输出： 什么不都做！
			不输出： 抛出SkipPageException异常
		3.3 重复输出标签体内容
			重复调用： this.getJspBody().invoke(null)
		3.4 修改标签体内容
				StringWriter sw = new StringWriter();
				this.getJspBody().invoke(sw);
			String content = sw.toString();
				//修改内容
				//手动输出到浏览器
				this.getJspContext().getOut().writer(修改过的内容);
		3.5 带属性的标签
				a）在标签处理器类中声明成员变量和setter方法，用于给属性变量赋值。
				b）在tld文件中声明属性
				c） 使用属性
4）JavaBean规范
		   4.1 必须要有无参的构造方法
		   4.2 所有成员属性必须私有化 （private）
		   4.3 必须提供公开的getter和setter方法 
5）MVC开发模式
		  MVC就是servlet+jsp+javabean的开发模式
			M，Model，javabean实现，封装业务数据
			V，View，jsp实现，显示数据
			C，Controller，servlet实现，接收参数，调用业务逻辑，跳转视图
6）三层结构开发
		 dao层： 数据访问对象。实现对数据的操作相关的方法
		service层： 业务逻辑对象。实现对项目的业逻辑处理相关的方法
		 web层： 表现层。处理和用户直接相关的，接收参数，处理参数，跳转视图，展示数据。

```

# jsp特性


jsp本质上就是servlet, 如果和servlet对比着来看, Servlet可以看作是用java开发动态资源的技术, 而jsp是用java（+html语言）开发动态资源的技术, 此外, jsp最大的两个特点:

*   jsp的运行必须交给tomcat服务器(tomcat的work目录： tomcat服务器存放jsp运行时的临时文件)
*   JSP可以包含java代码(这个可以说是jsp的优点也可以说是jsp的缺点, 详细后面说)

# jsp执行过程

<img src="p01.png"/>

访问http://localhost:8080/day12/01.hello.jsp , 过程大概是这样:
 
1. 访问到01.hello.jsp页面，tomcat扫描jsp文件，在%tomcat%/work目录把jsp文件翻译成java源文件
	`01.hello.jsp   ->   _01_hello_jsp.java` （翻译）
2. tomcat服务器把java源文件编译成class字节码文件 
	`_01_hello_jsp.java  ->  _01_hello_jsp.class`(编译)
3. tomcat服务器构造_01_hello_jsp类对象
4. tomcat服务器调用_01_hello_jsp类里面方法，返回内容显示到浏览器。
 
第一次访问jsp：走（1）（2）（3）（4）
第n次访问jsp：免去了翻译编译过程
但是jsp文件修改了或jsp的临时文件被删除了，要重新走翻译（1）和编译（2）的过程

		
# jsp和servlet对比

从生命周期来看

```
Servlet的生命周期：
	1）构造方法（第1次访问）
	2）init方法（第1次访问）
	3）service方法
	4）destroy方法        
Jsp的生命周期
	1）翻译： jsp->java文件
	2）编译： java文件->class文件（servlet程序）
	3）构造方法（第1次访问）
	4）init方法（第1次访问）：_jspInit()
	5）service方法：_jspService()
	6）destroy方法：_jspDestroy()
```

jsp和servlet最常用的开发模式是这样的: 

*   servlet
	*   接收参数
	*   处理业务逻辑
	*   把结果保存到域对象中
	*   跳转到jsp页面
*   Jsp
	*   从域对象取出数据
	*   把数据显示到浏览器

# jsp基本语法

```
1 Jsp模板
	jsp页面中的html代码就是jsp的模板
2 Jsp表达式
	语法：`<%=变量或表达式%>`
	作用： 向浏览器输出变量的值或表达式计算的结果
	注意：        
		1）表达式的原理就是翻译成out.print(“变量” );通过该方法向浏览器写出内容
		2）表达式后面不需要带分号结束。
		3）%和=中间不能有空格；
3 Jsp的脚本
	语法：<%java代码 %>
	作用： 执行java代码    
	原理：把脚本中java代码原封不动拷贝到_jspService方法中执行。所以这里声明的变量是局部变量；不能声明方法；
4 Jsp的声明
	语法：<%! 变量或方法 %>
	作用： 声明jsp的变量或方法
	注意:
		1）变量翻译成成员变量，方法翻译成成员方法。
		2）jsp声明中不能重复定义翻译好的一些方法
				比如public void _jspInit(){ }
5 Jsp的注释
	语法： <%!--  jsp注释  --%>
	注意: html的注释会被翻译和执行。而jsp的注释不能被翻译和执行，不会在浏览器中显示。

```

jsp脚本demo：

```jsp
<%!-- jsp表达式  --%>
<%
    //变量
    String name = "eric";
    int a = 10;
    int b =20;
%>
<%=name %>  
<br/>
<%=(a-b) %>  

<%!-- 穿插html代码 --%>
<%
for(int i=1;i<=6;i++){ 	
%>
<h<%=i %>>标题<%=i %></h<%=i %>>
<%
}
%>
```


# jsp三指令(include,page,taglib)

## include

*   作用： 在当前页面用于包含其他页面
*   语法： `<%@include file="common/header.jsp"%>`
*   注意：
    *   1） 原理是把被包含的页面（header.jsp）的内容翻译到包含页面(index.jsp)中,合并成翻译成一个java源文件，再编译运行. 这种包含叫静态包含（源码包含）, 下文还会提到动态包含, 能够带参数
    *   2） 如果使用静态包含，被包含页面中不需要出现全局的html标签了.（如html、head、body）

# page

*   作用： 告诉tomcat服务器如何翻译jsp文件, 指定错误处理措施

    ```
    <%@ page 
        language="java"   --告诉服务器使用什么动态语言来翻译jsp文件
        import="java.util.*" --告诉服务器java文件使用什么包(导入包，多个包之间用","分割)
    jsp文件编码问题相关：
        pageEncoding="utf-8"  --告诉服务器使用什么编码翻译jsp文件（成java文件）
        contentType="text/html; charset=utf-8" 服务器发送浏览器的数据类型和内容编码
            注意：在开发工具中，以后只需要设置pageEncoding即可解决中文乱码问题
    异常错误相关的：
        errorPage="error.jsp"        --指定当前jsp页面的错误处理页面。（如果和web.xml同时配置错误信息，jsp页面上的错误优先）
        isErrorPage="false"           --指定当前页面是否为错误处理页面。false，不是错误处理页面，则不能使用 exception内置对象；true，是错误处理页面，可以使用exception内置对象。例如：可以直接在jsp页面    <%    out.write(exception.getMessage());   %>
        
        session:  是否开启session功能。false，不能用session内置对象；true，可以使用session内  置对象。
        buffer:  jsp页面的缓存区大小。
        isELIgnore： 是否忽略EL表达式。
    %>
    ```

    *    配置全局的错误处理页面：(在`<web-app>`标签下)
    
    ```xml
    <!-- 这里location不用写项目名 -->
    <!-- 全局错误处理页面配置 -->
        <error-page>
            <error-code>500</error-code>
            <location>/common/500.jsp</location>
        </error-page>
        <error-page>
            <error-code>404</error-code>
            <location>/common/404.html</location>  //这里location不用写项目名
        </error-page>
    ```


# taglib

taglib用于导入jsp标签库中的jsp标签;



# jsp标签

## 作用

最主要的作用是替换jsp脚本。
比如替换流程判断, 页面跳转

```jsp
<body>
    <%--转发 --%>
    <%
  //request.getRequestDispatcher("/09.action2.jsp?name=eric").forward(request,response);
     %>
    <%-- 转发带参数 --%>
    <%--
    <jsp:forward page="/09.action2.jsp">
    	<jsp:param value="jacky" name="name"/>
    	<jsp:param value="123456" name="password"/>
    </jsp:forward>
	
  <%--redrict:重定向 --%>
    <c:redirect url="http://www.baidu.com"></c:redirect>
      --%>
  <%--包含，动态包含 --%>
      <%--
   <jsp:include page="/common/header.jsp">
   		<jsp:param value="lucy" name="name"/>
    </jsp:include>
   	 --%>
等价于（静态包含）
   	 <%@include file="common/header.jsp" %>
      主页的内容
  </body>

```

## 分类

1）动作标签（内置标签）： 不需要在jsp页面导入标签
2）jstl标签（外置标签）： 需要在jsp页面中导入标签
3）自定义标签 ： 自行定义，需要在jsp页面导入标签

## 内置标签

转发标签：    `<jsp:forward />`
参数标签：  `<jsp:pararm/>`
包含标签：  `<jsp:include/>`-----动态包含

对于包含标签, 有如下几点要注意:

```
原理： 包含与被包含的页面先各自翻译成java源文件，然后再运行时合并在一起。
        （先翻译再合并），动态包含

    静态包含  vs  动态包含的区别？

1）语法不同
静态包含语法： <%@inclue file="被包含的页面"%>
动态包含语法：<jsp:include page="被包含的页面">

2）参数传递不同
静态包含不能向被包含页面传递参数
动态包含可以向被包含页面传递参数
3）原理不同
静态包含： 先合并再翻译
动态包含： 先翻译再合并
```


## jstl标签

JSTL (全名：java  standard  tag  libarary   -  java标准标签库  )
 
核心标签库 （c标签库） 
国际化标签（fmt标签库）
EL函数库（fn函数库）
xml标签库（x标签库）
sql标签库（sql标签库）

怎么使用?
1. 导入jstl支持的jar包（导入标签背后隐藏的java代码）
    * 注意：使用javaee5.0的项目自动导入了jstl支持jar包
2. 使用taglib指令导入标签库 , jsp三指令之一
`<%@taglib uri="tld文件的uri名称" prefix="简写" %>`
	使用：
	`<c:set></c:set>` 这里的c:就是prifix
	这里的uri就是tld文件里的<uri>里面的内容
3. 在jsp中使用标签 


常用的便签&提交参数的多种方式:

```jsp
	保存数据：
	<c:set></c:set>   
	
	 <%--set标签 :保存数据(保存到域中)默认保存到page域 --%>
	    <c:set var="name" value="rose" scope="request"></c:set>
	
	
	获取数据： 
             <c:out value=""></c:out>
	单条件判断
            <c:if test=""></c:if>
	多条件判断
          <c:choose></c:choose>
          <c:when test=""></c:when>
          <c:otherwise></c:otherwise>
    循环数据
          <c:forEach></c:forEach>
          <c:forTokens items="" delims=""></c:forTokens>
	重定向
          <c:redirect></c:redirect>
		  
URL地址标签
	<c:url var="url" context="/day14" value="fileServlet">    context=""可省略，默认表示当前项目环境
		<c:param name="method" value="down"></c:param>
		<c:param name="fileName" value="${fileName.value }"></c:param>
	</c:url>
	调用时用  <a href="${url}"></a>
	
另外一个提交参数的标签：<input type="hidden" name="" value=""/>

```

demo:

```jsp
<%@ page language="java" import="java.util.*,gz.itcast.b_entity.*" pageEncoding="utf-8"%>
<%--导入标签库 --%>
<%@taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head> 
    <title>核心标签库</title>  
  </head>
  <body>
    <%--使用标签 --%>
    <%--set标签 :保存数据(保存到域中)默认保存到page域 --%>
    <c:set var="name" value="rose" scope="request"></c:set>
    <%
    	String msg = null;
    	pageContext.setAttribute("msg",msg);
     %>
    ${msg }
    <br/>
    <%--out标签： 获取数据（从域中） 
    default： 当value值为null时，使用默认值
    escapeXml: 是否对value值进行转义，false，不转义，true，转义（默认）
    --%>
    <c:out value="${msg}" default="<h3>标题3</h3>" escapeXml="true"></c:out>
    <hr/>
    <%--if标签 ：单条件判断--%>
    <c:if test="${!empty msg}">
    	条件成立
    </c:if>
    
    <hr/>
    <%--choose标签+when标签+otherwirse标签: 多条件判断 --%>
    <c:set var="score" value="56"></c:set>
    
    <c:choose>
    	<c:when test="${score>=90 && score<=100}">
    		优秀
    	</c:when>
    	<c:when test="${score>=80 && score<90}">
    		良好
    	</c:when>
    	<c:when test="${score>=70 && score<80}">
    		一般
    	</c:when>
    	<c:when test="${score>=60 && score<70}">
    		及格
    	</c:when>
    	<c:otherwise>
    		不及格
    	</c:otherwise>
    </c:choose>
    
    <%-- forEach标签：循环 --%>
    <%
    	//List
     	List<Student>  list = new ArrayList<Student>();
     	list.add(new Student("rose",18));
     	list.add(new Student("jack",28));
     	list.add(new Student("lucy",38));
     	//放入域中
     	pageContext.setAttribute("list",list);
     	
     	//Map
     	Map<String,Student> map = new HashMap<String,Student>();
     	map.put("100",new Student("mark",20));
     	map.put("101",new Student("maxwell",30));
     	map.put("102",new Student("narci",40));
     	//放入域中
     	pageContext.setAttribute("map",map);
     %>
     <hr/>
     <%--
      begin="" : 从哪个元素开始遍历，从0开始.默认从0开始
      end="":     到哪个元素结束。默认到最后一个元素
      step="" ： 步长    (每次加几)  ，默认1
      items=""： 需要遍历的数据（集合） 
      var=""： 每个元素的名称 
      varStatus=""： 当前正在遍历元素的状态对象。（count属性：当前位置，从1开始）
      
     --%>
    <c:forEach items="${list}" var="student" varStatus="varSta">
    	序号：${varSta.count} - 姓名：${student.name } - 年龄：${student.age}<br/>
    </c:forEach>
    
    <hr/>
    
    <c:forEach items="${map}" var="entry">
    	${entry.key } - 姓名： ${entry.value.name } - 年龄：${entry.value.age }<br/>
    </c:forEach>
    <hr/>
    <%-- forToken标签： 循环特殊字符串 --%>
    <%
    	String str = "java-php-net-平面";
    	pageContext.setAttribute("str",str);
     %>
    
    <c:forTokens items="${str}" delims="-" var="s">
    	${s }<br/>
    </c:forTokens>
    
eg: 分隔符区别, 结果见下图

	使用"|"作为分隔符<br />
	<c:forTokens var="token" items="spring,summer|autumn,winter" delims="|">
	    ${token}&copy;
	</c:forTokens><br />
	使用"|"和","作为分隔符<br />
	<c:forTokens var="token" items="spring,summer|autumn,winter" delims="|," end="3">
	    ${token}&copy;
	</c:forTokens><br />
	使用"-"作为分隔符<br />
	<c:forTokens var="token" items="year--season--month-week" delims="-">
	    ${token}&copy;
	</c:forTokens>

    <%--redrict:重定向 --%>
    <c:redirect url="http://www.baidu.com"></c:redirect>
    
  </body>
</html>

<c:remove var="varName"     
		[scope="{page|request|session|application}"] />
		

如果没有指定scope属性，<c:remove>标签就调用PageContext.removeAttribute(varName)方法，
否则就调用PageContext.removeAttribute(varName, scope) 方法

```

<img src="Screenshot_1.png"/>

## 自定义标签

首先通过一个问题引入： http://localhost:8080/day14/01.hellotag.jsp  如何访问到自定义标签？
 
前提： tomcat服务器启动时，加载到每个web应用，加载每个web应用的WEB-INF目录下的所有文件例如web.xml, tld文件
1）访问01.hellotag.jsp资源
2）tomcat服务器把jsp文件翻译成java源文件->编译class->构造类对象->调用_jspService（）方法
3）检查jsp文件的taglib指令: 是否存在一个名为http://gz.itcast.cn的tld文件。如果没有，则报错
4）上一步读到itcast.tld文件
5）读到`<xy:showIp> `, 到xy.tld文件中查询是否存在`<name>`为showIp的`<tag>`标签
6）找到对应的`<tag>`标签，则读到`<tag-class>`内容
7）得到 com.xy.ShowIpTag
构造ShowIpTag对象，然后调用ShowIpTag里面的方法

自定义标签的生命周期方法:

```java
// SimpleTag接口： 
void     setJspContext(JspContext pc)  --设置pageContext对象，传入pageContext（一定调用）
                                        通过getJspCotext()方法得到pageContext对象
void     setParent(JspTag parent)  --设置父标签对象，传入父标签对象，如果没有父标签，则不  调用此方法。通过getParent()方法得到父标签对象。
void     setXXX(值)             --设置属性值。
void     setJspBody(JspFragment jspBody) --设置标签体内容。标签体内容封装到JspFragment对象   中，然后传入JspFragment对象。通过getJspBody()方法                                                                        得到标签体内容。如果没有标签体内容，则不会调 用此方法
void     doTag()                     --执行标签时调用的方法。（一定调用）

```

作用:
1）控制标签体内容是否输出
2）控制标签余下内容是否输出
3）控制重复输出标签体内容
4）改变标签体内容

如何编写自定义标签?

```
需求： 向浏览器输出当前客户的IP地址 （只能使用jsp标签），不带主体内容

1）编写一个普通的java类，继承SimpleTagSupport类，叫标签处理器类

/**
 * 标签处理器类
 * @author APPle
 * 1）继承SimpleTagSupport
 *
 */
public class ShowIpTag extends SimpleTagSupport{
	private JspContext context; //jspContext是pageContext父类
	/**
	 * 传入pageContext
	 */
	@Override
	public void setJspContext(JspContext pc) {
			this.context = pc;
	}
	//###以上两句可以省略，下面的相应语句改变####################################
 
	/**
	 * 2）覆盖doTag方法
	 */
	@Override
	public void doTag() throws JspException, IOException {
			//向浏览器输出客户的ip地址
			PageContext pageContext = (PageContext)context;//若上面两句省了，这句改变如下：
			
			//上句代码相应改变为：
			PageContext pageContext = (PageContext)this.getJspContext();
			
			HttpServletRequest request = (HttpServletRequest)pageContext.getRequest();
			String ip = request.getRemoteHost();//获取客户端IP
			JspWriter out = pageContext.getOut();
			out.write("使用自定义标签输出客户的IP地址："+ip);
	}
}
 
2）在web项目的WEB-INF目录下建立itcast.tld文件，这个tld叫标签库的声明文件。（参考核心标签库的tld文件）
 
<?xml version="1.0" encoding="UTF-8" ?>
 
<taglib xmlns="http://java.sun.com/xml/ns/javaee"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-jsptaglibrary_2_1.xsd"
    version="2.1">
  <!-- 标签库的版本 -->
  <tlib-version>1.1</tlib-version>
  <!-- 标签库前缀 -->
  <short-name>itcast</short-name>
  <!-- tld文件的唯一标记 -->
  <uri>http://gz.itcast.cn</uri>
 
  <!-- 一个标签的声明 -->
  <tag>
    <!-- 标签名称 -->
    <name>showIp</name>
    <!-- 标签处理器类的全名 -->
    <tag-class>gz.itcast.a_tag.ShowIpTag</tag-class>
    <!-- 输出标签体内容格式 -->
<body-content>scriptless</body-content>   // scriptless或者empty表示没有主体内容
2.6 输出标签体内容格式 <body-content>scriptless</body-content>
				JSP：   在传统标签中使用的。可以写和执行jsp的java代码。
				scriptless:  标签体不可以写jsp的java代码
				empty:    必须是空标签。
				tagdependent : 标签体内容可以写jsp的java代码，但不会执行。
  </tag>
 
</taglib>
 
 
3）在jsp页面的头部导入自定义标签库
<%@taglib uri="http://gz.itcast.cn" prefix="itcast"%>
 
4）在jsp中使用自定义标签
<itcast:showIp></itcast:showIp>


```

## 操作javabean

一般不使用

# 九个内置对象, 四个域对象

在jsp页面加载完毕之后就会自动帮开发者创建好这些对象，而开发者只需要直接使用这些对象调用方法即可

举个例子

```
servlet: 
    HttpSession session = request.getSession(true); （需要开发者做）
jsp:
    tomcat服务器做好了：    HttpSession session = request.getSession(true);(不需要开发者做)
    开发者做的： session.getId();

```

## 内置对象

内置对象名 |         类型||
--|--|--
request  |     HttpServletRequest      |    常用, Javax.servlet.http.Request
response    |  HttpServletResponse   |HttpServletResponse
config      |  ServletConfig        | Javax.servlet.ServletConfig
application |       ServletContext   |      常用, Javax.servlet.ServletContext
session     |    HttpSession         |        常用, 在page指令中设置 Session=true为开
exception   |     Throwable          |isErrorPage=true开启,默认是false
page        |    this               | Javax.servlet.jsp.PageContext，可获取其他8个
out         |    JspWriter          | 和Response.getWriter()作用一样
pageContext |    PageContext|        常用

### out

```
out对象类型，JspWriter类，相当于带缓存的PrintWriter
PrintWriter： 
		wrier(内容)： 直接向浏览器写出内容。
JspWriter
		writer(内容): 向jsp缓冲区写出内容
当满足以下条件之一，缓冲区内容写出：
	1）缓冲区满了
	2）刷新缓存区 out.flush();
	3）关闭缓存区 buffer="0kb"
	4）执行完毕jsp页面

```

<img src="w1.png" width="50%" >

### pageContext对象

```
pageContext对象的类型是PageContext，叫jsp的上下文对象
 
 1）可以获取其他八个内置对象
使用场景： 在自定义标签的时候，PageContext对象频繁使用到
 
tomcat翻译出的源代码：
	public class 01_hello_jsp {
		public void _jspService(request,response){
			创建内置对象
			HttpSession session =....;
			ServletConfig config = ....;
 
			把8个经常使用的内置对象封装到PageContext对象中
			PageContext pageContext  = 封装；
			调用method1方法
			method1(pageContext);
		}
		public void method1(PageContext pageContext){
			希望使用内置对象
			从PageContext对象中获取其他8个内置对象
			JspWriter out =pageContext.getOut();
			HttpServletRequest rquest =     pageContext.getRequest();
			........
		}
	}
 2）本身是一个域对象
	ServletContext context域
	HttpServletRequet  request域
	HttpSession    session域     
	PageContext   page域          
 
作用： 保存数据和获取数据，用于共享数据
	#保存数据
        1）默认情况下，保存到page域
                pageContext.setAttribute("name"，“value”);
        2）可以向四个域对象保存数据
                pageContext.setAttribute("name"，“value”,域范围常量)
 
	#获取数据
        1）默认情况下，从page域获取
                pageContext.getAttribute("name")    返回的是Object
        2）可以从四个域中获取数据
                pageContext.getAttribute("name",域范围常量)
    
        域范围常量:
            PageContext.PAGE_SCOPE
            PageContext.REQUEST_SCOPE
            PageContext..SESSION_SCOPE
            PageContext.APPLICATION_SCOPE

        3）自动在四个域中搜索数据
			pageContext.findAttribute("name");
                顺序： page域 -> request域 -> session域- > context域（application域）
        <%--
            代码示例：pageContext作为域对象使用
            1）可以往不同域对象中存取数据
        --%>
        <%
        //保存数据。默认情况下，保存在page域中
        pageContext.setAttribute("message","page's message");
        pageContext.setAttribute("message","request's messsage",PageContext.REQUEST_SCOPE);//保存到request域中
        pageContext.setAttribute("message","session's messsage",PageContext.SESSION_SCOPE);//保存到sessio域中
        pageContext.setAttribute("message","application's messsage",PageContext.APPLICATION_SCOPE);//保存到context域中
        //request.setAttribute("message","request's message"); 等价于上面的代码
        %>
        <%
        //获取数据
        //String message = (String)pageContext.getAttribute("message");
        //out.write(message);
        %>
        <%--从request域中取出数据 --%>
        <%--
            原则： 
            1）在哪个域中保存数据，就必须从哪个域取出数据！！！
        --%>
        <%=pageContext.getAttribute("message",PageContext.PAGE_SCOPE) %><br/>
        <%=pageContext.getAttribute("message",PageContext.REQUEST_SCOPE) %><br/>
        <%=pageContext.getAttribute("message",PageContext.SESSION_SCOPE) %><br/>
        <%=pageContext.getAttribute("message",PageContext.APPLICATION_SCOPE) %><br/>
        <hr/>
        <%--
            findAttribute(): 在四个域自动搜索数据
                顺序： page域 -> request域  -> session域 -> context域
        --%>
        <%=pageContext.findAttribute("message") %>
        <% //request.getAttribute("message") %><br/>

```

## 四个域对象

作用: 保存数据  和 获取数据 ，用于数据共享

```java
setAttribute("name",Object) 保存数据
getAttribute("name")  获取数据
removeAttribute("name") 清除数据
```

变量名|域范围|类型
--|--|--
pageContext    |  page域, 只能在当前jsp页面中使用（当前页面）        |   PageContext                
request        |  request域, (只能在同一个请求中使用（即跳转必须使用转发技术才能维护变量）)    |      HttpServletRequest
session        |  session域 ,只能在同一个会话（session对象）中使用（私有的）    |       HttpSession
application    |   context域 ,只能在同一个web应用中使用。（全局的）   |    ServletContext          


# EL表达式

jsp 中 el 表达式无效: https://blog.csdn.net/wolf_soul/article/details/50388005 (servlet 2.3 or earlier 默认不解析 el 表达式, 需要显式的生命 "<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>", servlet 2.4 开始, 默认解析)

* 作用：向浏览器输出域对象中的变量值或表达式计算的结果

* 语法：       ${变量or表达式}

* 开发jsp的原则： 尽量在jsp页面中少写甚至不写java代码(如jsp表达式 <%=    %>和 jsp脚本<%  %>)。用EL表达式替换

看看示例:

```jsp
<body>
<%
 String name = "rose";  
 //放入域中
 //pageContext.setAttribute("name",name);默认保存到page域
 pageContext.setAttribute("name",name,PageContext.REQUEST_SCOPE); 
  %>
  <%=name %>//输出
  <br/>
  <%--
1)从四个域自动搜索
   --%>
  EL表达式： ${name }---------------------name值是存入键值对中的key，key变化，则这里的name要变化
  <%--
	${name } 等价于
		<%=pageContext.findAttribute("name")%>
   --%>
   <%--
2） 从指定的域中获取数据
	--%>
	EL表达式： ${pageScope.name }
	<%--
		${pageScope.name } 等价于
		 <%= pageContext.getAttribute("name",PageContext.PAGE_SCOPE)%>
		
	 --%>
</body>

```

# 常用的套路

```jsp
<base href='<%=request.getScheme() + "://" + request.getServerName() + ":" + request.getServerPort() + request.getContextPath() + "/"%>' />

<link href="${pageContext.request.contextPath}/themes/css/core.css" rel="stylesheet" type="text/css" />
<link href="${pageContext.request.contextPath}/themes/css/core-custom.css" rel="stylesheet" type="text/css" />

<img src="<%=request.getContextPath()%>/styles/default/images/icon_03.jpg" />
<script src="${pageContext.request.contextPath}/scripts/centitui/jquery.validate.js" type="text/javascript"></script>

<form action="${pageContext.request.contextPath}/dde/mapInfoTrigger!saveTrigger.do" clas...



```
