---
title: HTTP协议概述
tags:
  - http
  - outline
date: 2016-04-10 22:27:26
categories: java web
---


<div align="center">
http://www.httpbin.org/ 在线演示 http 标准
https://httpie.org/ http 访问工具, 代替 curl
</div>
<!--more-->

<!-- TOC -->

- [概念](#概念)
- [http和tcp/ip比较](#http和tcpip比较)
- [怎么查看](#怎么查看)
- [http请求](#http请求)
    - [常见请求方式](#常见请求方式)
    - [GET vs. POST](#get-vs-post)
    - [常见请求头](#常见请求头)
    - [获取请求信息](#获取请求信息)
    - [网页参数提交获取](#网页参数提交获取)
    - [service(),doGet(),doPost()关系](#servicedogetdopost关系)
    - [request中文乱码问题](#request中文乱码问题)
- [http响应](#http响应)
    - [响应行&状态码](#响应行状态码)
    - [常见响应头](#常见响应头)
    - [设置http响应](#设置http响应)
    - [api示例](#api示例)
    - [请求重定向](#请求重定向)
    - [响应中文乱码](#响应中文乱码)
- [url中文乱码](#url中文乱码)
    - [在JS中处理](#在js中处理)
    - [在java中处理](#在java中处理)
- [基于HTTP协议的实时数据获取](#基于http协议的实时数据获取)
    - [短轮询](#短轮询)
    - [长连接](#长连接)
    - [WebSocket](#websocket)
- [一次完整的http请求](#一次完整的http请求)
    - [http请求流程](#http请求流程)
    - [发起http请求](#发起http请求)
    - [http解析](#http解析)
        - [常用的请求头响应头，状态码](#常用的请求头响应头状态码)
    - [DNS域名解析](#dns域名解析)
        - [整个流程](#整个流程)
        - [清除DNS缓存](#清除dns缓存)
        - [域名解析方式](#域名解析方式)
    - [CDN 工作机制](#cdn-工作机制)
- [https](#https)
    - [和 http 对比](#和-http-对比)
    - [如何免费开启 https](#如何免费开启-https)

<!-- /TOC -->


# 概念

http协议是定义的一套client和server之间数据传输的规范

<img src="./sh-1.png" style="width:70%"></img>

# http和tcp/ip比较

![](./Screenshot_1.png)

# 怎么查看

1. 使用浏览器
2. 使用系统自带的telnet工具(远程访问工具)

# http请求

先看一个全景:

```
请求（浏览器 -> 服务器）
GET /day09/hello HTTP/1.1
Host: localhost:8080
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64; rv:35.0) Gecko/20100101 Firefox/35.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-cn,en-us;q=0.8,zh;q=0.5,en;q=0.3
Accept-Encoding: gzip, deflate
Connection: keep-alive


响应（服务器->浏览器）
HTTP/1.1 200 OK
Server: Apache-Coyote/1.1
Content-Length: 24
Date: Fri, 30 Jan 2015 01:54:57 GMT

this is hello servlet!!!

```

<img src="p1.png" style="width:90%"/>

## 常见请求方式

GET: 获取资源
POST:传输实体文本, Restful风格api里用于新增
HEAD: 获取报文首部, 和get类似, 只是不返回报文的主题部分
PUT: 传输文件, Restful风格api里用于修改; 类似文件上传, 但是HTTP/1.1的PUT方法自身不带验证机制，任何人都可以上传文件，存在安全问题，故一般不用。
DELETE: 删除文件
OPTIONS: 询问支持的方法(client询问server可以提交那些请求方法)
TRACE: 追踪路径(户端可以对请求消息的传输路径进行追踪，TRACE方法是让Web服务器端将之前的请求通信还给客户端的方法)
CONNECT：要求用隧道协议连接代理(主要使用SSL（安全套接层）和TLS（传输层安全）协议把通信内容加密后经网络隧道传输)

## GET vs. POST

*   get提交参数有限制, 不超过1kb.
*   GET参数通过URL传递，POST放在Request body中。
*   GET在浏览器回退时是无害的，而POST会再次提交请求。
*   GET产生的URL地址可以被Bookmark，而POST不可以。
*   GET请求会被浏览器主动cache，而POST不会，除非手动设置。
*   GET请求只能进行url编码，而POST支持多种编码方式。
*   GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
*   GET请求在URL中传送的参数是有长度限制的，而POST没有。
*   对参数的数据类型，GET只接受ASCII字符，而POST没有限制。

## 常见请求头

Accept: text/html,image/*　　-- 浏览器接受的数据类型
Accept-Charset: ISO-8859-1　　-- 浏览器接受的编码格式
Accept-Encoding: gzip,compress　　--浏览器接受的数据压缩格式
Accept-Language: en-us,zh-       　　--浏览器接受的语言
Host: www.it315.org:80          　　--（必须的）当前请求访问的目标地址（主机:端口）
If-Modified-Since: Tue, 11 Jul 2000 18:23:51 GMT　　  --浏览器最后的缓存时间
Referer: http://www.it315.org/index.jsp      　　-- 当前请求来自于哪里
User-Agent: Mozilla/4.0 (compatible; MSIE 5.5; Windows NT 5.0)  　　--浏览器类型
Cookie:name=eric　　                     -- 浏览器保存的cookie信息
Connection: close/Keep-Alive   　　         -- 浏览器跟服务器连接状态。close: 连接关闭  keep-alive：保持连接。使用http1.1时，如果想主动和服务器断开连接，发送包含此键值对的请求头
Date: Tue, 11 Jul 2000 18:23:51 GMTds　　--请求发出的时间

<img src="p2.png"/>

## 获取请求信息

<img src="p3.png"/>

demo:

```java
/**
 * 请求数据的获取
 */
public class RequestDemo1 extends HttpServlet {
	/**
	 * 1)tomcat服务器接收到浏览器发送的请求数据，然后封装到HttpServetRequest对象
	 * 2）tomcat服务器调用doGet方法，然后把request对象传入到servlet中。
	 */
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		/**
		 * 3)从request对象取出请求数据。
		 */
		t1(request);
		t2(request); 
	}
	// 为了接收POST方式提交的请求
	@Override
	protected void doPost(HttpServletRequest request, HttpServletResponse resp)
			throws ServletException, IOException {
		/**
		 * 3.3 请求的实体内容
		 */
		InputStream in = request.getInputStream(); //得到实体内容
		byte[] buf = new byte[1024];
		int len = 0;
		while(  (len=in.read(buf))!=-1 ){
			String str = new String(buf,0,len);
			System.out.println(str);
		}
	}
	private void t2(HttpServletRequest request) {
		/**
		 * 3.2 请求头
		 */
		String host = request.getHeader("Host"); //根据头名称的到头的内容
		System.out.println(host);
		//遍历所有请求头
		Enumeration<String> enums = request.getHeaderNames(); //得到所有的请求头名称列表
		while(enums.hasMoreElements()){//判断是否有下一个元素
			String headerName = enums.nextElement(); //取出下一个元素
			String headerValue = request.getHeader(headerName);
			System.out.println(headerName+":"+headerValue);
		}
	}
	private void t1(HttpServletRequest request) {
		/**
		 * 3.1 请求行   格式：（GET /day09/hello HTTP/1.1）
		 */
		System.out.println("请求方式："+request.getMethod());//请求方式
		System.out.println("URI:"+request.getRequestURI());//请求资源
		System.out.println("URL:"+request.getRequestURL());
		System.out.println("http协议版本："+request.getProtocol());//http协议
	}
}


/**
get browser type
*/
public class RequestDemo3 extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		//获取请求头： user-agent
		String userAgent = request.getHeader("user-agent");
		System.out.println(userAgent);
		
		//判断用户使用的浏览器类型
		if(userAgent.contains("Firefox")){
			response.getWriter().write("你正在使用火狐浏览器");
		}else if(userAgent.contains("Chrome")){
			response.getWriter().write("你正在使用谷歌浏览器");
		}else if(userAgent.contains("Trident")){
			response.getWriter().write("你正在使用IE浏览器");
		}else{
			response.getWriter().write("地球上没有这个浏览器，建议使用火狐浏览器");
		}
	}
}

/**
 * 案例- 防止非法链接
 */
public class RequestDemo4 extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8");
		//得到referer头
		String referer = request.getHeader("referer");
		System.out.println("referer="+referer);
		/**
		 * 判断非法链接：
		 * 	1）直接访问的话referer=null
		 *  2）如果当前请求不是来自广告   
		 */
		if(referer==null || !referer.contains("/day09/adv.html")){
			response.getWriter().write("当前是非法链接，请回到首页。<a href='/day09/adv.html'>首页</a>");
		}else{
			//正确的链接
			response.getWriter().write("资源正在下载...");
		}
	}
}


```

## 网页参数提交获取

<img src="p4.png"/>

```java
// 获取指定名称的参数
protected void doGet(HttpServletRequest req, HttpServletResponse resp)
		throws ServletException, IOException {
	System.out.println(req.getMethod() + "方式！");
	Enumeration<String> enums = req.getParameterNames();
	while(enums.hasMoreElements()) {     //遍历参数名
		String name = enums.nextElement();
		if("hobbit".equals(name)) {//如果找到hobbit
			String[] values = req.getParameterValues(name);
			System.out.print(name + ":");
			for(String str : values) {
				System.out.print(str + ", ");
			}
			System.out.println();
		}else {        //其他的参数
			String value = req.getParameter(name);
			System.out.println(name + "----" + value);
		}
	}
}

```

## service(),doGet(),doPost()关系

container server (tomcat/jetty...) 会首先 service(), 然后在 service() 内部根据 get/post 调用 doGet()/doPost()

<img src="p5.png"/>

servlet本质就是一个接口, 定义了一种规范, 自定义的servlet需要实现这个接口, 实现init(), service(), destroy()等方法.

## request中文乱码问题

<img src="Screenshot_2.png"/>
<img src="p6.png"/>

# http响应

<img src="p7.png"/>

## 响应行&状态码

http://www.w3school.com.cn/tags/html_ref_httpmessages.asp

<img src="Screenshot_3.png"/>

## 常见响应头

<img src="p8.png"/>

## 设置http响应

<img src="p9.png"/>


## api示例

```java
/**
 * 设置响应信息
 */
public class ResponseDemo1 extends HttpServlet {
	/**
	 * 1)tomcat服务器把请求信息封装到HttpServletRequest对象，且把响应信息封装到HttpServletResponse
	 * 2）tomcat服务器调用doGet方法，传入request，和response对象
	 */
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		/**
		 * 3）通过response对象改变响应信息
		 * 3.1 响应行
		 */
		response.setStatus(404);//修改状态码
		response.sendError(404); // 发送404的状态码+404的错误页面
		/**
		 * 3.2 响应头
		 */
		response.setHeader("server", "JBoss");
		/**
		 * 3.3 实体内容(浏览器直接能够看到的内容就是实体内容)
		 */
		response.getWriter().write("01.hello world"); //字符内容。
		response.getOutputStream().write("02.hello world".getBytes());//字节内容
	}
}


/**
 * 案例- content-Type作用: 设置响应类容格式
 *
 */
public class ResponseDemo4 extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		/**
		 * 设置响应实体内容编码
		 */
//		response.setCharacterEncoding("utf-8");
//		response.setHeader("content-type", "text/html");
		response.setContentType("text/html;charset=utf-8");//和上面代码等价。推荐使用此方法
		//response.setContentType("text/xml");
		//response.setContentType("image/jpg");
		
		response.getWriter().write("<html><head><title>this is tilte</title></head><body>中国</body></html>");
		response.getOutputStream().write("<html><head><title>this is tilte</title></head><body>中国</body></html>".getBytes("utf-8"));
		
		File file = new File("e:/mm.jpg");
		/**
		 * 设置以下载方式打开文件
		 */
		response.setHeader("Content-Disposition", "attachment; filename="+file.getName());
		/**
		 * 下载图片
		 */
		FileInputStream in = new FileInputStream(file);
		byte[] buf = new byte[1024];
		int len = 0;
		//把图片内容写出到浏览器
		while( (len=in.read(buf))!=-1 ){
			response.getOutputStream().write(buf, 0, len);
		}
	}
}


```

## 请求重定向

<img src="p10.png"/>

```java
/**
 * 案例- 请求重定向
 * （相当于超链接跳转页面）
 */
public class ResponseDemo2 extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		/**
		 * 需求： 跳转到adv.html
		 * 使用请求重定向： 发送一个302状态码+location的响应头
		 */
		/*response.setStatus(302);//发送一个302状态码
		response.setHeader("location", "/day09/adv.html"); //location的响应头
*/		
		//请求重定向简化写法
		response.sendRedirect("/day09/adv.html");          //此时这里的路径是给浏览器用，首位斜杠代表webapps目录
	}
}

/**
 * 案例- 定时刷新, 定时跳转
 */
public class ResponseDemo3 extends HttpServlet {
	public void doGet(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		/**
		 * 定时刷新
		 * 原理：浏览器认识refresh头，得到refresh头之后重新请求当前资源
		 */
		response.setHeader("refresh", "1"); //每隔1秒刷新次页面
		/**
		 * 隔n秒之后跳转另外的资源
		 */
		response.setHeader("refresh", "3;url=/day09/adv.html");//隔3秒之后跳转到adv.html
	}
}

/**
 * BufferedImage:内存图像
    Graphics：画笔
    ImageIO：输出图像
    放在html页面上<img src/>
 */
public class CaptchaDemo4 extends HttpServlet {
    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.setContentType("image/jpeg");// conf/web.xml中查jpg       文件类型的MIME值
        int width = 120,height=25;
        //1.得到一个内存中的图像
        BufferedImage bimage = new BufferedImage(width, height, BufferedImage.TYPE_INT_RGB);
        //2.得到画笔
        Graphics g = bimage.getGraphics();
        //3.设置画笔颜色
        g.setColor(Color.BLUE);
        //4.画一个矩形
        g.drawRect(0, 0, width, height);
        //5.设置填充色
        g.setColor(Color.YELLOW);
        g.fillRect(1, 1, width-1, height-1);
        //6.画随机干扰线条
        Random random = new Random();
        g.setColor(Color.GRAY);
        for (int i = 0; i < 20; i++) {
            g.drawLine(random.nextInt(width-1),random.nextInt(height-1),random.nextInt(width-1),random.nextInt(height-1));
        }
        //7.画随机数字
        g.setColor(Color.RED);
        g.setFont(new Font("黑体",Font.BOLD|Font.ITALIC,20));
        for (int i = 0; i < 4; i++) {
            g.drawString(random.nextInt(9)+"", 20+i*20, 18);
        }
        
        //不需要有缓存
        response.setHeader("Cache-Control", "no-cache");//http1.1
        response.setHeader("pragma", "no-cache"); //http1.0
        response.setHeader("Expires", "-1");  //只要小于1970-1-1距离现在的毫秒数就可以
        
        //8.输出内存中的图像
        ImageIO.write(bimage, "jpg", response.getOutputStream());
    }
}

```
jsp页面:
```jsp
<script>
   function changeImg(){
        var imgObj =   document.getElementById("mycode");
        imgObj.src=imgObj.src+"?time="+new Date().getTime();// 加时间戳, 表示不要使用缓存的图像
   }
</script>
  
<body>
<!-- servlet/CaptchaDemo4     -->
 验证码:<input type="text" name="code"> <img  id="mycode" alt="" src="servlet/CaptchaDemo4">  <a href="javascript:changeImg()">看不清，换一张</a>
</body>
```

转发&重定向:

<img src="p11.png"/>
<img src="p12.png"/>

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
		 */
		this.getServletContext().getRequestDispatcher("/GetDateServlet").forward(request, response);
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
		 */
		//response.sendRedirect("/day09/adv.html");
		response.sendRedirect("/day10/GetDataServlet");
	}

}

```

## 响应中文乱码

<img src="p13.png"/>

最便捷的办法是:
```java
response.setContentType("text/html;charset=utf-8");
```

# url中文乱码

## 在JS中处理

<img src="p14.png"/>
<img src="p15.png" />

## 在java中处理

```java
// 需要使用user-agent头信息，获取当前正在使用的浏览器
String agent = request.getHeader("user-agent");
// Mozilla/5.0 (Windows NT 6.1; WOW64; rv:39.0) Gecko/20100101 Firefox/39.0
// System.out.println(agent);

// 火狐采用的Base64编码 IE或者谷歌采用URL编码
if(agent.contains("Firefox")){
    // 使用的火狐
    filename = DownloadUtil.base64EncodeFileName(filename);
    
}else{
    // 使用的IE或者谷歌
    filename = URLEncoder.encode(filename, "UTF-8");
}

// downloadUtil代码：
import java.io.UnsupportedEncodingException;
import sun.misc.BASE64Encoder;
public class DownloadUtil {
    /**
     * 把文件名称进行Base64编码
     * @param fileName
     * @return
     */
    public static String base64EncodeFileName(String fileName) {
        
        BASE64Encoder base64Encoder = new BASE64Encoder();
        try {
            return "=?UTF-8?B?"
                    + new String(base64Encoder.encode(fileName
                            .getBytes("UTF-8"))) + "?=";
        } catch (UnsupportedEncodingException e) {
            e.printStackTrace();
            throw new RuntimeException(e);
        }
    }
}


```

# 基于HTTP协议的实时数据获取

服务器主动推数据给 client

## 短轮询


- 定义: 客户端向服务端请求数据，服务端立即将数据返回给客户端，客户端没有拿到想要的数据（比如返回结果告诉客户端，数据处理中），客户端继续发请求，服务端继续立即响应，周而复始

- 利弊: 浪费带宽和服务器的计算资源

- 场景: 数据变化比较频繁或者能预期到数据在短时间内会发生一次变化的场景可以使用短轮询

比如: 用户在PC端买了一个东西唤起网页端，由于PC端和网页端是不通的，我们预期到用户应该很快会完成付款，这种时候为了开发简单短轮询是一种可以使用的方式，直接服务端提供一个接口告诉客户端订单状态，客户端每5秒请求一次即可，拿到结果就可以不用请求了

Note: 注意要做好请求次数上限的控制，比如请求100次还没检测到用户付款，可以弹窗"请完成付款后去我的订单页面查询"就可以不用请求了

## 长连接

或者叫 长轮询

- 定义: 本质上没有改变，依然是客户端在没有收到自己想要数据的情况下不断发送请求给服务端，差别在于服务端收到请求不再直接给响应，而是将请求挂起，自己去定时判断数据的变化，有变化就立马返回给客户端，没有就等到超时为止。

- 利弊: 长轮询的优点就是客户端的请求少了很多避免了无谓的客户端请求，缺点则是服务端会挂起大量请求增加资源消耗且服务器对HTTP请求并发数量是有限制的

- 场景: 微信网页版的登陆, 客户端不断发送请求到服务器，服务器第一时间并没有给出回应，于是客户端等待，在超时的情况下继续发送请求。

## WebSocket

references: https://www.cnblogs.com/xrq730/p/9280404.html

当服务端数据有变化的时候，服务端可以主动推送到客户端, 建立了 server 和 browser 的全双工通信

![alt](Snipaste_2018-11-04_21-39-32.png)

WebSocket客户端首先通过HTTP协议发送几个特别的header到服务端，告诉服务端现在我发起的是HTTP请求，但我要升级到WebSocket了, 服务器响应后就建立了 websocket连接

Netty-Socketio 有现成的 websocket api, 真正的开发成本在于分布式环境下的数据同步问题

# 一次完整的http请求

## http请求流程

<img src="http_flow.png" width=50%/>

## 发起http请求

* 发起http请求本质就是建立socket

浏览器根据地址栏的URL访问DNS server解析出IP地址，与server建立socket连接&rArr;浏览器根据URL组装一个http request header，通过outputStream.write发送到目标server&rArr;服务器等待outputStream.read返回数据，最后断开连接

* 发起http的工具

Java中的httpclient、Linux中的curl+URL命令

## http解析

### 常用的请求头响应头，状态码

* http header：控制浏览器的渲染行为和server的执行逻辑

<img src="request_header.png" width=50%/>
<img src="screenshot_1_r.png" width=50%/>
<img src="screenshot_2_r.png" width=50%/>

* 浏览器缓存相关的header

Cache-Control：no-cache ---------------优先级最高的header，有多个可选值  
Expire：时间 -------------------------超过后失效  
lastModified：时间--------------------最后修改时间，响应头；对应if-modified-since请求头


## DNS域名解析

### 整个流程

当你在浏览器的地址栏中输入 http://xxx.yyy.com/  

1. 提取出域名: xxx.yyy.com  
2. 如果系统中配置了 Hosts 文件，那么电脑会先查询 Hosts 文件，看这个xxx.yyy.com 否已经在 Hosts 里面有了对应的记录。如果有，直接就可以拿到该记录中的 IP地址，过程就结束
3. 如果没有host文件这个别名，会向配置的DNS server 发送DNS查询请求，获取到IP地址
4. 浏览器根据这个IP地址发送http请求

但是，DNS server 如何知道IP这些信息？  

* DNS 缓存

    域名服务器叫“递归域名服务器”，server内部会有一个DNS记录的缓存，如果DNS 请求无法在缓存中查到，再进入下一步：缓存同步    

    缓存里每条记录都有一个过期时间，如果到了过期时间，就会被删除，然后重新同步  

* 缓存同步

    1. “递归服务器”发现缓存中没有DNS记录，那么它会问“根服务器”，“根服务器”会告诉“递归服务器”说：这个域名属于 com 这个分支之下，你去找 com 这个域名的“权威服务器”，这个权威服务器的 IP地址 是 mmm  
    2. 然后“递归服务器”根据拿到的这个 mmm 地址，又去找“com 域名的权威服务器”。“com 域名的权威服务器”告诉它：你应该去找“yyy.com 域名的权威服务器”，这个权威服务器的 IP地址 是 nnn
    3. 然后“递归服务器”又根据nnn找“yyy.com 域名的权威服务器”，这时候“yyy.com 域名的权威服务器”才会告诉它/xxx.yyy.com/这个域名的IP是多少  

整个缓存同步过程像是踢皮球，效率低下，所以需要一个DNS缓存


### 清除DNS缓存

* 主要在两个地方：1. LDNS,无法介入；2. 本机 cmd下：ipconfig /flushdns


### 域名解析方式

* A记录-------记录域名，IP映射
* MX记录-------邮件相关
* CNAME记录-------多个别名映射到一个域名
* NS记录---------为某个域名指定一个DNS server
* TXT记录-----------为某个主机/域名设置说明

## CDN 工作机制

cdn以缓存静态数据为主  
浏览器从主站请求动态数据后，再从cdn下载静态数据 

{% asset_img screenshot_4.png %}

# https

## 和 http 对比

http: http消息直接基于tcp传输

https: http消息通过 SSL/TLS 协议加密, 再通过 tcp 传输

ssl协议依靠证书来验证服务器的身份, 获取 ca 证书可以购买收费的(云厂商提供), 也能使用免费的 (Letsencrypt是一个免费、自动化和开放的证书颁发机构，其颁发的证书一次有效期为三个月，但是只要能持续更新，基本可以永久使用。)

## 如何免费开启 https

https://mp.weixin.qq.com/s/wtVxG0weBUiC0-Lwu7vVDw

Spring Boot Secured By Let’s Encrypt, Let's Encrypt提供免费的TLS证书

[tomcat 开启 https 支持](https://mp.weixin.qq.com/s?__biz=MzI3ODcxMzQzMw==&mid=2247484381&idx=1&sn=f6919853976da79e52602589ec164a15&chksm=eb5386ebdc240ffd9d9f0b869c2ecad755a8d1f5a78115da40634ce87e906251c919c8bb32cf&scene=21#wechat_redirect), spring boot也要做设置, 继承 WebSecurityConfigurerAdapter, 覆盖 configure(HttpSecurity http), http.requiresChannel().requiresSecure()

https://www.cnblogs.com/imfjj/p/9058443.html

TODO

https://github.com/Neilpang/acme.sh , 脚本acme.sh，实现了 acme 协议, 可以帮你持续自动从Letsencrypt更新CA证书

```sh
curl https://get.acme.sh | sh


```

