---
title: html-note
tags:
  - html
date: 2014-03-11 10:59:27
categories: frontend
---
<div align="center">
HTML NOte
MDN web文档: https://developer.mozilla.org/zh-CN/
w3shcool国内镜像: https://www.quanzhanketang.com/default.html
一个html slide tool: https://github.com/hakimel/reveal.js
[chrome devtools](https://umaar.com/dev-tips/)
</div>

<!--more-->

<!-- TOC -->

- [语法](#语法)
- [html语义化](#html语义化)
- [dom结构](#dom结构)
- [html5新增内容](#html5新增内容)
  - [合在一起](#合在一起)
  - [视频](#视频)
  - [音频](#音频)
  - [拖放](#拖放)
- [兼容 ie](#兼容-ie)
- [demo](#demo)
  - [w3c经典页面](#w3c经典页面)
  - [简历典型页面](#简历典型页面)
  - [table中包含form](#table中包含form)

<!-- /TOC -->

# 语法

```html
<!--注释-->
<!-- 快捷键： ctrl+/ 是注释 -->
<!-- html文档 == 网页 
	元素：完整标签结构
-->
<!-- 文档类型声明 表明我们是用的HTML5的标准 -->
<!DOCTYPE html>
<!-- 告诉浏览器HTML文档从 html 开始到 /html 结束 
	head
	body
-->
<html  lang="zh">
	<!-- head:所有头部元素的容器，内部是HTMl网页的相关配置，比如，网页标题，文字的编码 -->
	<head>
		<!-- 提供关于HTML文档的元数据。比如文字的编码，关键字，网页作者，网站的关键字，网站内容，描述；这些设置不会在浏览器中显示
		charset="utf-8"：属性 key="value"
		 -->
		<meta charset="utf-8">
		<meta name="author" content="作者名字">
		<meta name="keywords" content="关键字1,关键字2">
		<!-- 文档标题 ：显示在网页的选项卡上，书签，网页上搜索的结果标题-->
        <title>网页的标题</title>
        
        <!-- base:基础标签 
		href：给页面所有的相对路径规定默认的基准URL
		target:a链接默认窗口模式，下面a链接会覆盖当前的设置
        -->
        <base href="www.xxx.com/img/">
        <!-- 
            以下图片地址为：www.xxx.com/img/1.jpg
            <img src="1.jpg"> 
        -->
        <!-- 默认设置链接从新的窗口打开 -->
        <base target="_blank" />
    </head>

	<!-- 用户能看的内容，比如文字。。图片。。 -->
	<body>
		<!-- h1-h6标题标签，文字的大小依次减小，在文档中的重要性也依次变小
	标题一般只用一次
	宽度独占一整行
	 -->
	<h1 style="background-color:red;">this is heading1</h1>
	<h2 style="background-color:red;">this is heading2</h2>
	<h3 style="background-color:red;">this is heading3</h3>
	<h4 style="background-color:red;">this is heading4</h4>
	<h5 style="background-color:red;">this is heading5</h5>
	<h6 style="background-color:red;">this is heading6</h6>
	<!--无h7标签 <h7>this is heading7</h7> -->

	<!-- p标签，段落标签，宽度占据一整行
	高度适应文本高度
	 -->
	<p style="background-color:green;">这是我的第一个段落文章，有点丑，因为没有样式</p>
	<p style="background-color:green;" id="htmltext">超文本标记语言，标准通用标记语言下的一个应用。 “超文本”就是指页面内可以包含图片、链接，甚至音乐、程序等非文字元素。<br> 超文本标记语言的结构包括“头”部分（英语：Head）、和“主体”部分（英语：Body），其中“头”部提供关于网页的信息，“主体”部分提供网页的具体内容。</p>

	<!-- hr：水平分割线单标签，没有结束标签 -->
	<hr>
	
	<!--br 换行标签，一般结合段落使用  -->
	小明：--------------
	<br>
	小红：--------------
	<br>
	
	<!--  qoute q 标签，引用标签(即加引号)，标签大小适应内容大小-->
	<q style="background:red;">传道授业解惑也</q>
	<q style="background:red;">传道授业解惑也</q>
	<q style="background:red;">传道授业解惑也</q>

	
	<p>111</p>
	<p>222</p>

	<!-- 引用一大段话，自动缩进(整段文字都缩进, 每行缩进) -->
	<blockquote style="background:green;">超文本标记语言，标准通用标记语言下的一个应用。 “超文本”就是指页面内可以包含图片、链接，甚至音乐、程序等非文字元素。超文本标记语言的结构包括“头”部分（英语：Head）、和“主体”部分（英语：Body），其中“头”部提供关于网页的信息，“主体”部分提供网页的具体内容</blockquote>


	<!-- 告诉浏览器内部是代码,大小适应内容, 字体较小 -->
	int a = 10; for(int i= 0;i&lt;10;i++)<br>
	<code>
		int a = 10; for(int i= 0;i&lt;10;i++)
	</code>
	<br>

	for(int a =10; i &lt; 100; i++){
		print("%d",i);
	}
	<br>

	<!-- pre:预处理标签，告诉浏览器内容愿样式显示，保留空格换行等-->
	<pre>
		for(int a =10; i &lt; 100; i++){
			print("%d",i);
		}
	</pre>

	<!-- strong标签 强调加粗 , 同b标签, 同 em 标签
		em标签：强调斜体
	-->
	<strong style="background-color:red;">强调加粗</strong>
	<p>房贷还款双方都<strong>强调加粗</strong>会发生大火焚烧发电是否合适的</p>
	<p>房贷还款双方都<em>强调唯一性</em>会发生大火焚烧发电是否合适的</p>

	<!-- b：加粗 
	-->
	<p>房贷还款双方都<b>强调加粗</b>会发生大火焚烧发电是否合适的</p>
	<!-- i: 倾斜 
	-->
	<p>房贷还款双方都<i>强调唯一性</i>会发生大火焚烧发电是否合适的</p>

	<!-- 上标：sup 
		下标：sub
	-->
	x<sup>2</sup> + y<sup>2</sup> - 1
	<br>
	x<sub>1</sub> + y<sub>1</sub> = 10
	<br>

	<!-- 
	img:图片标签
		src：图片路径 网络路径和本地路径
		alt: 图片描述 图片加载失败显示
	-->
	<img src="http://img73.nipic.com/file/20160414/9252150_120745103583_1.jpg" alt="">
	<br>

	<!-- a标签：超链接
	href:跳转位置（网络链接，本地地址）
	target:1._self:默认：当前窗口打开
			2._blank：新的页面
	 -->
	<a href="http://www.baidu.com" 	target="_blank">百度</a>
	<a href="index1.html" target="_blank">index1</a>

	<!-- 返回顶部 -->
	<a href="#">返回顶部</a>

	<!-- 回到某个标签位置 -->
    <a href="#htmltext">超文本标记语言</a>
    <!-- 发送邮件 -->
    <div class="button">
    <a class="mail" data-img="mail.png" href="mailto:example@gmail.com?subject=xxx&body=xxx"></a>
    </div>

    <!-- 段落跳转 -->
    <div id="html5"></div>
    <a name="user-content-html5" href="#html5" class="headeranchor-link" aria-hidden="true"><span class="headeranchor"></span></a>

	<!-- 
	列表：无序列表，有序列表，自定义列表
	 -->
	<!-- 无序 -->
	<ul>
		<li style="background-color:red;">H5</li>
		<li>IOS</li>
		<li>Android</li>
	</ul>
	<!-- 有序 -->
	<ol style="background-color:red;">
		<li>吃饭</li>
		<li>睡觉</li>
		<li>打豆豆</li>
	</ol>
	<!-- 自定义列表 -->
	<dl>
		<!-- 标题 -->
		<dt style="background-color:red;">内容标题</dt>
		<!-- 内容 -->
		<dd style="background-color:green;">标题内容1</dd>
		<dd>标题内容2</dd>
		<dd>标题内容3</dd>
		<!-- 标题 -->
		<dt>内容标题</dt>
		<!-- 内容 -->
		<dd>标题内容1</dd>
		<dd>标题内容2</dd>
		<dd>标题内容3</dd>
    </dl>
    
    <!-- 
		div：定义文档中的一个区域部分
		一般用于组合块级元素，可以方便css来对子元素进行设置

		div：块元素
		px:像素单位
	 -->
	<div style="color:pink;background-color:red;width:200px;">
		<p style="color:blue; background-color:yellow;width:100px;height:50px;">pppppppppppppp</p>
		<p>pppppppppppppp</p>
		<h3>pppppppppppppp</h3>
		<strong style="background-color:yellow;width:200px;height:100px;">我是一个内线元素</strong><span>111</span>
		<div style="width:100px;height:100px;background-color:blue;">.............</div>
    </div>
    
    <!-- <span>:本身没有任何的格式表现，主要用于对文档行内元素进行组合，给审判标签设置样式
	属于内嵌元素
	无法定义宽高		
	 -->
	<p style="color:red;">今天是个<span style="color:green;font-size:20px;">好日子</span>，是一个考试的好日子</p>

    <!-- 嵌套：
		1.块元素可以嵌套大部分块元素，可以嵌套行内元素
		特殊块元素P h1-h6 dt dl dd:不能嵌套块元素

        2.（内嵌元素）行元素可以嵌套内嵌元素，但是不可以嵌套块元素 -->
        
	<!-- 不能嵌套块元素，浏览器布局改变 -->
	<p>块元素可以嵌套大部分块元素，<p>我是一个嵌套p</p>可以嵌套行内元素</p>
	<!-- 不能嵌套块元素，浏览器布局改变 -->
    <h3>块元素可以嵌套<h2>大部分块元</h2>素，可以嵌套行内元素</h3>
    <br>
	<span style="">
		（内嵌元素）行<strong>元素</strong>可以嵌套内嵌元素，但是不可以嵌套块元素
	</span>
	<br><br>

	<!-- 不可以嵌套块元素 -->
	<span style="">
	（内嵌元素）行<div>元素</div>可以嵌套内嵌元素，但是不可以嵌套块元素</span>

    <!-- 
        tfoot 表格尾(尾一般在前面) 
		thead 表格头
		tbody 表格主体
		caption 标题
		
		表格样式重置
		table{border-collapse:collapse;} 

     -->
     <!-- 
		table:表格；tr:行；td:列；th：标题

		tbody：当表格内容非常多的时候，表格下载一点一点下载，但是加上tbody后，就会等表格body下载完成后显示。（内容多的话会有多个tbody）

		表格优化：将tfoot放到tbody上面，可以让浏览器有限解析加载，显示正常位置显示
	 -->
    <!-- border: 外层边框粗细
		border-collapse：设置表格边框是否被合并为单一的边框
			collapse：边框合并
			separate：边框分开
	-->
    <h3>table1 添加表头</h3>
	<table border="1" style="border-collapse:collapse; width:500px;height:200px; text-align:center;">
		<tr>
            <th rowspan="4">课程基本信息</th><!-- 多行合并单元格 -->
			<th>星期一</th><!-- 标识表头 -->
			<th>星期二</th>
			<th>星期三</th>
			<th>星期四</th>
			<th>星期五</th>
		</tr>
		<!-- tr：确定表格行 -->
		<tr>
			<!-- td：确定表格列 -->
			<td>语文</td>
			<td>数学</td>
			<td>英语</td>
			<td>体育</td>
			<td>美术</td>
		</tr>
		<!-- 第二行 -->
		<tr>
			<!-- td：确定表格列 -->
			<td>语文</td>
			<td>数学</td>
			<td>英语</td>
			<td>体育</td>
			<td>美术</td>
        </tr>
        <tr>
			<td colspan="5">课程安排计划总结</td><!-- 多列合并单元格 -->
		<!--<td></td>
			<td></td>
			<td></td>
			<td></td>
		-->
		</tr>
    </table>
    <h3>table7 列分组</h3>
	<table border="1" style="border-collapse:collapse; width:500px;height:200px; text-align:center; background-color:yellow;">
		<!-- 标题 -->
		<caption>课程表</caption>
		<!-- 设置列 -->
		<colgroup>
			<!-- 每个col中的span规定占据列数 -->
			<!-- 第一个col只起到占位作用，第二三个设置列样式 -->
			<col span="2">
			<col style="background-color:#ff0000;">
			<col style="background-color:blue;">
		</colgroup>
		<tr>
			<th rowspan="4">课程基本信息</th>
			<th>星期一</th>
			<th>星期二</th>
			<th>星期三</th>
			<th>星期四</th>
			<th>星期五</th>
		</tr>
		<!-- tr：确定表格行 -->
		<tr style="color:pink;">
			<!-- td：确定表格列 -->
			<td>语文</td>
			<td>数学</td>
			<td>英语</td>
			<td>体育</td>
			<td>美术</td>
		</tr>
		<!-- 第二行 -->
		<tr>
			<!-- td：确定表格列 -->
			<td>语文</td>
			<td>数学</td>
			<td>英语</td>
			<td>体育</td>
			<td>美术</td>
		</tr>
		<tr>
			<td colspan="5">课程安排计划总结</td>
		</tr>
	</table>

    <h3>表单-登陆类型界面</h3>
	<!-- action:可以为空，默认提交到本界面，也可以是#或者### -->
	<form action="#">

		<h4>1.type = text 输入框，默认20个字符左右</h4>
		用户名：<input type="text" name="user">
		密码：<input type="password" name="pass">

		<h4>2.type-submit 数据提交</h4>
		登陆：<input type="submit" value="登陆">
		注册：<input type="submit" value="注册">

		<h4>3.type = iamge，图片，</h4>
		<!-- 定义一张图片作为提交按钮 -->
		<input type="image" src="img/1.png">

		<h4>4.单选框 type=radio</h4>
		<!-- 单选框：name属性必须一样，才可以单选（保证提交到服务器的key相同）
		value:对应提交参数的值
		checked:默认选项，可以直接checked不加值
		disabled：禁用input元素
		 -->
		学历：
		<input type="radio" name="education" value="0" checked>小学
		<input type="radio" name="education" value="1" disabled>初中
		<input type="radio" name="education" value="2">高中
		<input type="radio" name="education" value="3">大学

		<h4>5.复选框 type=checkbox</h4>
		爱好：
		<input type="checkbox" name="hobby" value="0">吃饭
		<input type="checkbox" name="hobby" value="1">sleep
		<input type="checkbox" name="hobby" value="2">打豆豆

		<h4>6.type=button 按钮，不会出发form里面的action,而是单独用js交互处理的</h4>
		<input type="button" value="我是一个按钮">

		<h4>7.type=file 文件上传，添加附件 value:没有这个属性</h4>
		<input type="file" value="点击我选择文件">

		<h4>8.type=reset 重置，将输入框中的内容情况（慎用）</h4>
		<input type="reset" value="重置">

		<h4>9.type=hidden 隐藏input,一般是用来传递一些固定信息，不让用户看到</h4>
		<input type="hidden" name="order" value="1110000101">
		<!-- dispaly:隐藏标签，不会保存他的位置，直接隐藏掉 -->
		<input type="text" style="display:none;">
		<!-- visibillity:hidden 隐藏标签，占位隐藏，会留下空白的位置 -->
		<input type="text" style="visibility:hidden;">

		<h4>10.多行输入文本 textarea</h4>
		留言：<textarea name="message" id="" cols="20" rows="20"></textarea>

		<h4>11.选择下拉：select</h4>
		<select name="gender" id="">
			<option value="" checked></option>
			<option value="0">男</option>
			<option value="1">女</option>
			<option value="2">其他</option>
		</select>

		<h3>HTML5的新增type类型</h3>
		<!-- color、date、datetime、datetime-local、month、week、time、email、number、range、search、tel 和 url。 -->
		<input type="color"><br>
		date
		<input type="date"><br>
		datetime
		<input type="datetime"><br>
		datetime-local
		<input type="datetime-local"><br>
		month
		<input type="month"><br>
		week
		<input type="week"><br>
		time
		<input type="time"><br>
		<!-- email：输入类型应该对于的是包含邮件地址的字段，提交表单的时候，自动的对email进行检查 -->
		email
		<input type="email"><br>
		number
		<input type="number"><br>
		range
		<input type="range"><br>
		search
		<input type="search"><br>
		tel
		<input type="tel"><br>
		url
		<input type="url"><br>

		<!-- for是属性的值=input 下的id的属性值 , 这样, 点击 文字, 相当于点击 input, 他们合为一体了-->
		<!-- 若想省略指定 for, 需要将 input 放入 label 内部 -->
		<label for="upfile">文件上传</label>
		<input type="file" id="upfile">
	</form> 

	<!-- 总结
		1.块元素：p pre ul ol li dl h1-h6等
		{
			特点：默认都占一行
			支持设置宽高
			高度自适应（自适应内容）
			支持所有css样式
        }
        
		2.内嵌元素（行元素）：
		{
			特点：可以同行显示
			内容撑开宽高
			不支持宽高设置
			有些css样式不支持（margin）
        }

            3. div：定义文档中的一个区域部分
            一般用于组合块级元素，可以方便css来对子元素进行设置
        

	 -->

	</body>
</html>
```

# html语义化

# dom结构

DOM: Document Object Module, 文档对象模型。我们通过JavaScript操作页面的元素，进行添加、移动、改变或移除的方法和属性, 都是DOM提供的。

根据 W3C 的 HTML DOM 标准，HTML 文档中的所有内容都是节点：

整个文档是一个文档节点
每个 HTML 元素是元素节点
HTML 元素内的文本是文本节点
每个 HTML 属性是属性节点
注释是注释节点

<img src="Snipaste_2018-05-21_17-02-41.png">


# html5新增内容

HTML5 是对 HTML 标准的第五次修订。其主要的目标是将互联网语义化，以便更好地被人和机器阅读，并同时提供更好地支持各种媒体的嵌入

## 合在一起

一些新特性:

* 用于绘画的 canvas 元素
* 用于媒介回放的 video 和 audio 元素
* 对本地离线存储的更好的支持
* 新的特殊内容元素，比如 article、footer、header、nav、section
* 新的表单控件，比如 calendar、date、time、email、url、search


```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<style>
			mark{
				/* tarnsparent 透明色 */
				/* background-color: transparent; */
			}
			.wbr_c{
				width: 150px;
				background-color: red;
			}
		</style>
	</head>
	<body>
		<!--
			h5:新增标签
			新增标签作用：语义化，更加适合SEO，容易被搜索引擎爬虫识别
			
			视频，音频
			本地存储
			画布
			废弃的一些可以用css代替的标签，center标签,form标签等等
		-->
		<!--mark：标记，突出显示文本-->
		<p><mark>今天</mark>是个好天气</p>

		<!--meter:电池条，度量
			value:当前的值（默认范围是0-1），小于等于最小值不显示
			min: 最小值
			max：最大值
			high: 最高预警值，value大于high后颜色预警
			low: 最低预警值，value小于low后颜色预警
			
			标签文本内容： 浏览器不支持会被显示
		-->
		<meter value="20" min="10" max="100" high="70" low="20">浏览器不支持</meter>
		
		<!--progress:进度条
			value：当前值（默认0-1）
			max: 最大值
			
			没有最小值属性，最小默认是0
		-->
		<progress value="50" max="100" min="50"></progress>
		
		<!--wbr:单词间换行，拆分换行
			
		-->
		<p class="wbr_c">
			I am a loag words hahhahahhahahhhaaahahah.
		</p>
		<p class="wbr_c">
			I am a loag words hahhahahhahahhha<wbr>aahahah</wbr>.
		</p>
		
		<!--ruby: 象拼音一样标注
			rt: 标注内容
		-->
		<ruby>
			你好<rt>ni hao</rt>
			&nbsp;
			阿西吧<rt>叹气</rt>
		</ruby>
		
		<!--datalist:可选列表
			input标签的 list属性值 == datalist 的id值
		-->
		<form action="#" method="post" id="top_form">
			<input type="text" name="key" id="key" value="" list="gender_list" />
			<datalist id="gender_list">
				<option -value="1">男</option>
				<option -value="2">女</option>
				<option -value="3">其他</option>
			</datalist>
			
			<select name="">
				<option -value="">男</option>
				<option -value="">女</option>
			</select>
			<input type="submit" name="" id="" value="提交" />
		</form>
		<!--details: 折叠信息
			summary: 标题，内容可见
			open属性: 显示隐藏内容
		-->
		<details open="">
			<summary>电脑配置</summary>
			<p>内存：16G</p>
			<p>双卡双待</p>
		</details>
		
		<!--figure:注释，标记，一般用于图片
			figcaption：元素标题，显示位置和放置位置有关
		-->
		<figure>
			<figcaption>这是一个图片</figcaption>
			<img src="img/1.jpg" alt="" />
		</figure>
		
		<!--time: 标记时间，告诉浏览器是一个时间
			datetime:告诉浏览器是一个时间，时间由datetime决定
			T: 间隔
			datetime格式：YYYY-MM-DDThh:mm:ss
		-->
		<p>现在<time>8:00</time>整</p>
		<p><time datetime="2016-06-21T09:30:30">今天</time>是个好日子</p>
		
		<!--form: 新类型
			
		-->
		<!--novalidate： 不验证输入格式，form属性-->
		<form id="formId" action="#" novalidate>
			<!--自动匹配格式-->
			电子邮件：<input type="email" />
			<!--step: 每次加或者减得数值-->
			数字：<input type="number" name="number" value="10" max="20" min="5" step="5" />
			网址：<input type="url" name="url" /><br />
			取值范围： <input type="range" name="range" min="10" max="50" value="10" />
			
			<!--时间-->
			年月日: <input type="date" name="date" />
			<br />
			年月: <input type="month" name="month" />
			<br />
			年周: <input type="week" name="week" />
			<br />
			时分: <input type="time" name="time" />
			<br />
			本地时间：<input type="datetime-local" name="loacl" min="2016-11-25T00:00:00" />
			
			<br />
			颜色：<input type="color" name="color" />
			<br />
			搜索：<input type="search" />
			<br />

			<hr />
			
			<!--表单新增属性-->
			<!--
				autofocus: 自动获取焦点,只允许一个，默认第一个
			-->
			焦点: <input type="text" autofocus />
			<hr />
			<input type="text" autofocus="" />
			<hr />
			<!--placeholder: 占位文本，文本框为空时显示内容-->
			站位文本: <input type="text" placeholder="请输入名字" />
			<hr />
			<!--required:验证表单是否为空，为空不允许提交-->
			<input type="text" -required="" />
			<hr />
			<!--
				autocomplete:自动提示功能；
				默认：on
				off：关闭
			-->
			<input type="text" name="key_t" autocomplete="off"/>
			<hr />
			<!--
				multiple: 文件多选
			-->
			<input type="file" multiple="" />
			<hr />
			<input type="email" name="email_2" />
				
			<hr />
			提交: <input type="submit" value="提交" />
		</form>
			<!--form:属性值==form的id属性值-->
            <input type="submit" name="outside" form="fomtId" />
        

        <!--
			video:ie8或更早不支持
			视频格式：{
				mp4:ie高版本chrome，火狐
				webM:谷歌发明，火狐支持，主流模式
				OGG:火狐，chrome支持
			}
			controls:是否显示控件
		-->
		<!--controls:显示播放控件-->
		<!--autoplay：自动播放-->
		<!--<video src="img/1.mp4" width="400px" height="400px" controls autoplay=""><!--</video>-->

		<!--loop:循环播放-->
		<!--<video src="img/1.mp4" width="400px" height="400px" controls autoplay="" loop=""></video>-->

		<!--muted:静音-->
		<video src="img/1.mp4" width="400px" height="400px" controls autoplay="" loop="" muted=""></video>

		<!--poster:视频播放前的占位图-->
		<video src="img/1.mp4" width="400px" height="400px" controls -autoplay="" loop="" poster="img/weibo.jpg"></video>

		<!--兼容所有浏览器-->
		<video width="200px" height="200px" controls>
			<!--
				chrome和火狐
				type:视频类型
			-->
			<source src="img/1.mp4" type="video/mp4"></source>
			<source src="img/1.webm" type="video/webm"></source>
			<source src="img/1.ogg" type="video/ogg"></source>
		</video>

		<!--audio: 和视频一样
			mp3,ogg,wav
		-->
		<audio controls="">
			<source src="img/music.mp3" type="audio/mp3"></source>
			<source src="img/music.ogg" type="audio/ogg"></source>
			<source src="img/music.wav" type="audio/wav"></source>
		</audio>
		
		<!---->
		<div class="header"></div>
		<div class="nav"></div>
		<div class="section"></div>
		<div class="footer"></div>
		
		<!--头部标签-->
		<header></header>
		<!--标签导航-->
		<nav></nav>
		<!--管理h1-h6-->
		<hgroup>
			<h3>h3333</h3>
			<h4>h4444</h4>
		</hgroup>
		<!--文章侧边栏-->
		<aside></aside>
		<!--文章内容，一个评论区-->
		<artical></artical>
		<!--文章分区-->
		<section></section>
		<!--尾部-->
		<footer></footer>
		
		
		<header>头部</header>
		<nav>导航</nav>
		<section>
			<aside>侧边栏</aside>
			<artical>
				<hgroup>
					<h2>文章标题</h2>
				</hgroup>
				<footer>文章的尾部</footer>
			</artical>
		</section>
		<footer>结尾</footer>
	</body>
</html>
```

## 视频

```html
<video width="320" height="240" controls="controls">
  <source src="movie.ogg" type="video/ogg">
  <source src="movie.mp4" type="video/mp4">
Your browser does not support the video tag.
</video>
```

使用dom控制

```html
<!DOCTYPE html> 
<html> 
<body> 

<div style="text-align:center;">
  <button onclick="playPause()">播放/暂停</button> 
  <button onclick="makeBig()">大</button>
  <button onclick="makeNormal()">中</button>
  <button onclick="makeSmall()">小</button>
  <br /> 
  <video id="video1" width="420" style="margin-top:15px;">
    <source src="/example/html5/mov_bbb.mp4" type="video/mp4" />
    <source src="/example/html5/mov_bbb.ogg" type="video/ogg" />
    Your browser does not support HTML5 video.
  </video>
</div> 

<script type="text/javascript">
var myVideo=document.getElementById("video1");

function playPause()
{ 
if (myVideo.paused) 
  myVideo.play(); 
else 
  myVideo.pause(); 
} 

function makeBig()
{ 
myVideo.width=560; 
} 

function makeSmall()
{ 
myVideo.width=320; 
} 

function makeNormal()
{ 
myVideo.width=420; 
} 
</script> 

</body> 
</html>

```


## 音频

```html
<!DOCTYPE HTML>
<html>
<body>

<audio controls="controls">
  <source src="/i/song.ogg" type="audio/ogg">
  <source src="/i/song.mp3" type="audio/mpeg">
Your browser does not support the audio element.
</audio>

</body>
</html>

```

## 拖放

在 HTML5 中，拖放是标准的一部分，任何元素都能够拖放

```html
<!DOCTYPE HTML>
<html>
<head>
<style type="text/css">
#div1 {width:198px; height:66px;padding:10px;border:1px solid #aaaaaa;}
</style>
<script type="text/javascript">
function allowDrop(ev)
{
ev.preventDefault();
}

function drag(ev)
{
ev.dataTransfer.setData("Text",ev.target.id);
}

function drop(ev)
{
ev.preventDefault();
var data=ev.dataTransfer.getData("Text");
ev.target.appendChild(document.getElementById(data));
}
</script>
</head>
<body>

<p>请把 W3School 的图片拖放到矩形中：</p>

<div id="div1" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
<br />
<!-- 
    draggable="true", 元素可拖放 
    ondragstart 拖动时触发

-->
<img id="drag1" src="/i/eg_dragdrop_w3school.gif" draggable="true" ondragstart="drag(event)" />

</body>
</html>

```


# 兼容 ie

```html
<!DOCTYPE html>
<!--[if lt IE 7]>      <html class="no-js lt-ie9 lt-ie8 lt-ie7"> <![endif]-->
<!--[if IE 7]>         <html class="no-js lt-ie9 lt-ie8"> <![endif]-->
<!--[if IE 8]>         <html class="no-js lt-ie9"> <![endif]-->
<!--[if gt IE 8]>      <html class="no-js"> <!--<![endif]-->
<html>
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title></title>
    <meta name="description" content="">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="">
  </head>
  <body>
    <!--[if lt IE 7]>
      <p class="browsehappy">You are using an <strong>outdated</strong> browser. Please <a href="#">upgrade your browser</a> to improve your experience.</p>
    <![endif]-->
    
    <script src="" async defer></script>
  </body>
</html>
```


# demo

## w3c经典页面

```css
body{
	margin:40px 5px;
	font-size:100%;
}
h1{
	font-weight:800;/* 删掉也没啥变化 */
	font-size:1.6em;/* 跳脱h1标签原始字体size, 自己设定 */
	margin-bottom:0px;
}
hr{
	border:1px solid #B0B0B0;/* 设置hr公共属性, 下面可以分别设置; solid表示实心, 不加hr就消失了 */
}
.hr1{
	margin-top:0px;/* 如果不加这条, 默认都是有一定间距的, 希望没有间距, 必须加上这条 */
}
.pagebutton{
	margin-top:20px;
	height:28px;
	width:240px;
}
.button1{
	padding-top:.1em;
	height:25px;
	width:100px;
	border:1px solid #E3E3E3;
	background-color:#F3F3F3;
	float:left;/* 浮动到左侧 */
	margin-right:30px;
	text-align:center;
}
.button1 a{
	text-decoration:none;/* 去掉默认的下划线 */
	color:black;
	font-size:.7em;
}
.button2{
	padding-top:.1em;
	height:25px;
	width:100px;
	border:1px solid #E3E3E3;
	background-color:#F3F3F3;
	float:left;/* 浮动到左侧 */
	text-align:center;
}
.button2 a{
	text-decoration:none;
	color:black;
	font-size:.7em;
}
.hr2{
	margin-top:20px;
	margin-bottom:20px;
}
h2{	
	color:black;
	font-size:.9em;
	font-weight:bold;/* 加粗, 不过h2标签默认就是加粗了 */
}
p{
	font-size:.8em;
	line-height:1.5em;/* 行高 */
}
#color1{
	color:#F4A283;
}
b{
	font-size:.7em;
}
.demo{
	margin-top:10px;
	border:1px solid #D5D5D0;
	background-color:#F5F5F6;
	padding-left:15px;
	padding-top:0px;
}
.try{
	margin-top:10px;
	background-color:#F98D8A;
	width:85px;
	height:22px;
	color:#FFFFFF;
	font-size:.8px;
	text-align:center;
	padding-top:.5em;
	
}
.try a{
	color:#fff;
	text-decoration:none;/* 不加下划线 */
}
li{
	line-height:1.5em;
	font-size:.8em;
}
sub{
	width: 100px;
	height: 10%;
	margin: 5em;
	
}

```

```html
<!DOCTYPE html>
<html>

<head land="ch">
	<meta charset="utf-8">
	<title>w3cschool</title>
	<link rel="stylesheet" href="css/demo.css">
</head>

<body>
	<h1>基本的HTML标签-四个实例</h1>
	<hr class="hr1">
	<div class="pagebutton">
		<div class="button1">
			<a href="#">上一节</a>
		</div>
		<div class="button2">
			<a href="#">下一节</a>
		</div>
	</div>
	<hr class="hr2">
	<h2>本章通过实例向您演示最常用的HTML标签</h2>
	<p>
		<span id="color1">提示：</span>不要担心本章中您还没有学过的例子，您将在下面的章节中学到它们。</p>
	<p>
		<span id="color1">提示：</span>学习HTML最好的方式就是边学边做实验。我们为您准备了很好的HTML编辑器。使用这个编辑器，您可以任意编辑一段HTML源码。然后单机TRY按钮来产看结果。</p>
	<hr>
	<h2>HTML标题</h2>
	<p>HTML标题（Heading）是通过&lt;h1&gt;-&lt;h6&gt;等标签进行定义的</p>
	<b>实例</b>
	<br>
	<div class="demo">
		<pre><code>&lt;h1&gt;this is a heading&lt;/h1&gt;
&lt;h2&gt;this is a heading&lt;/h2&gt;
&lt;h3&gt;this is a heading&lt;/h3&gt;</code></pre>
	</div>

	<div class="try">
		<a href="#">
			亲自试一试
		</a>
	</div>

	<hr class="hr2">
	<h2>HTML段落</h2>
	<p>HTML段落是通过&lt;p&gt;-&lt;h6&gt;标签进行定义的.</p>
	<b>实例</b>
	<br>
	<div class="demo">
		<pre><code>&lt;p&gt;this is a paragraph&lt;/p&gt;
&lt;p&gt;this is another paragraph&lt;/p&gt;</code></pre>
	</div>
	<div class="try">
		<a href="#">
			亲自试一试
		</a>
	</div>
	<hr>
	<h2>HTML链接</h2>
	<p>HTML链接是通过&lt;a&gt;标签进行定义的.</p>
	<b>实例</b>
	<br>
	<div class="demo">
		<pre><code>&lt;a href=&quot;http://www.w3cschool.com.cn&quot;&gt;this is a link&lt;/a&gt;</code></pre>
	</div>
	<div class="try">
		<a href="#">
			亲自试一试
		</a>
	</div>
	<p>
		<b>注释：</b>在href属性中指定链接的地址。
	</p>
	<p>
		(您将在本教程稍后的章节中学习更多有关属性的知识)。
	</p>
	<hr>
	<h2>HTML图像</h2>
	<p>HTML图像是通过&lt;img&gt;标签进行定义的.</p>
	<b>实例</b>
	<br>
	<div class="demo">
		<pre><code>&lt;img src=&quot;w3school.jpg&quot; width=&quot;104&quot; height=&quot;142&quot; /&gt;</code></pre>
	</div>
	<div class="try">
		<a href="#">
			亲自试一试
		</a>
	</div>
	<p>
		<b>注释：</b>图像的名称和尺寸是以属性的形式提供的。
	</p>
	<hr>
	<div class="pagebutton">
		<div class="button1">
			<a href="#">上一节</a>
		</div>
		<div class="button2">
			<a href="#">下一节</a>
		</div>
	</div>
</body>

</html>
```

## 简历典型页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Table demo2</title>
</head>
<body>
	<div class="table">
		<!-- 
			border="2px" 整个table的外边框粗细
			margin:auto 或者 margin: 0 auto 
				设置整个元素水平居中
			font-size:.9em; 字体相对大小
			

		 -->
	<table border="2px" style="border-collapse:collapse; text-align:center; margin:auto; margin-top:50px; height:820px;font-size:.9em;">
		<colgroup>
			<col style="width:110px;">
			<col style="width:100px;">
			<col style="width:80px;">
			<col style="width:80px;">
			<col style="width:70px;">
			<col style="width:110px;">
			<col style="width:100px;">

		</colgroup>
		<!-- 
			font-weight:bold; 加粗
			letter-spacing:1em; 字间距
		 -->
		<caption style="font-weight:bold;margin-bottom:20px; font-size:1.1em; letter-spacing:1em;">个人简历</caption>
		<tbody>
			<tr>
				<td>姓名</td>
				<td>&nbsp;</td>
				<td>性别</td>
				<td>&nbsp;</td>
				<td>出生年月</td>
				<td>&nbsp;</td>
				<td rowspan="4">&nbsp;</td>
			</tr>
			<tr>
				<td>民族</td>
				<td>&nbsp;</td>
				<td>政治面貌</td>
				<td>&nbsp;</td>
				<td>身高</td>
				<td>&nbsp;</td>
			</tr>
			<tr>
				<td>学制</td>
				<td>&nbsp;</td>
				<td>学历</td>
				<td>&nbsp;</td>
				<td>户籍</td>
				<td>&nbsp;</td>
			</tr>
			<tr>
				<td>专业</td>
				<td>&nbsp;</td>
				<td colspan="2">毕业学校</td>
				<td colspan="2"></td>
			</tr>
			<tr>
				<th colspan="7" style="border-width:2px;">技能、特长或爱好</th>
			</tr>
			<tr>
				<td>外语等级</td>
				<td colspan="2">&nbsp;</td>
				<td>计算机</td>
				<td colspan="3">&nbsp;</td>
			</tr>
			<tr>
				<th colspan="7" style="border-width:2px;">个人履历</th>
			</tr>
			<tr>
				<td>时间</td>
				<td colspan="2">单位</td>
				<td colspan="4">经历</td>
			</tr>
			<tr>
				<td>2002年4月</td>
				<td colspan="2">&nbsp;</td>
				<td colspan="4">&nbsp;</td>
			</tr>
			<tr>
				<td>2003年3月</td>
				<td colspan="2">&nbsp;</td>
				<td colspan="4">&nbsp;</td>
			</tr>
			<tr>
				<td>2003年8月</td>
				<td colspan="2">&nbsp;</td>
				<td colspan="4">&nbsp;</td>
			</tr>
			<tr>
				<th colspan="7" style="border-width:2px;">联系方式</th>
			</tr>
			<tr>
				<td>通讯地址</td>
				<td colspan="3">&nbsp;</td>
				<td>联系电话</td>
				<td colspan="2">&nbsp;</td>
			</tr>
			<tr>
				<td>E-mail</td>
				<td colspan="3">&nbsp;</td>
				<td>邮编</td>
				<td colspan="2">&nbsp;</td>
			</tr>
			<tr>
				<th colspan="7" style="height:70px; border-width:2px;">自我评价</th>
			</tr>
			<tr>
				<th colspan="7" style="height:150px;">&nbsp;</th>
			</tr>
		</tbody>
	</table>
</body>
</html>
```

还一个

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Table demo1</title>
</head>
<body>
	<!-- border加px是可选的 -->
	<table border="1" style="border-collapse:collapse;height:250px; text-align:center; font-size:.9em; margin:auto; margin-top:50px;">
		<colgroup>
			<!-- 对于被th包住的单元格, 是无法设置字体粗细的, 这里的font:bold无效 -->
			<col style="background-color:rgb(155, 128, 128); font:bold; width:100px;">
			<col style=" width:120px;">
			<col style="background-color:#E0E0E0; font-weight:800;width:100px;">
			<col style=" width:120px;">
			<col style="width:120px">
		</colgroup>
		<caption style="font-size:1.1em; font-weight:bold; margin-bottom:20px;">个人简历</caption>
		<tbody>
			<tr>
				<th>姓名</th>
				<td>张小萌</td>
				<th>性别</th>
				<td>女</td>
				<td rowspan="5"></td>
			</tr>
			<tr>
				<th>民族</td>
				<td>汉族</td>
				<th>籍贯</th>
				<td>四川</td>
			</tr>
			<tr>
				<th>出生日期</th>
				<td>1991.5</td>
				<th>婚姻状况</th>
				<td>否</td>
			</tr>
			<tr>
				<th>学历</th>
				<td>大专</td>
				<th>体重身高</th>
				<td>165CM、49KG</td>
			</tr>
			<tr>
				<th>专业</th>
				<td>音乐舞蹈</td>
				<th>健康状况</th>
				<td>健康</td>
			</tr>
			<tr>
				<th>求职意向</th>
				<td colspan="4">幼儿教师，舞蹈老师</td>
			</tr>
		</tbody>
	</table>
</body>
</html>
```

## table中包含form

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>table+form</title>
	<style>
		table>tbody>tr>th{
			text-align:left;
			padding-left:5px;
		}
		.col2{
			text-align:left;
			padding-left:10px;
		}
		.col3{
			text-align:center;
		}
	</style>
</head>
<body>
	<table border="1px" 
	style="border-collapse:collapse; margin:auto; margin-top:20px; border-color:#C7A285">
		<colgrounp>
			<col style="width:100px;">
			<col style="width:160px;">
			<col style="width:250px;" class="col3">
		</colgrounp>
		<caption style="margin-bottom:3px; font-weight:bold; font-size:.8em;">表1 input元素type属性值</caption>
		<thead>
			<!-- 
				border:none 当前行无表框
				color:#fff; 文本颜色
			 -->
			<tr style="border:none; 
				background-color:#F99547;
				color:#fff;
				font-size:.9em;
			">
				<th>type属性值</th>
				<th>说明</th>
				<th>浏览器效果（参考效果）</th>
			</tr>
		</thead>
		<tbody>
			<tr>
				<th>text</th>
				<td class="col2">单行文本框</td>
				<td class="col3"><input type="text" name="text" value="admin"></td>
			</tr>
			<tr>
				<th>password</th>
				<td class="col2" class="col2">密码文本框</td>
				<td class="col3"><input type="password" value="123456"></td>
			</tr>
			<tr>
				<th>button</th>
				<td class="col2">按钮</td>
				<td class="col3"><input type="submit" value="注册"></td>
			</tr>
			<tr>
				<th>reset</th>
				<td class="col2">重置按钮</td>
				<td class="col3"><input type="reset"></td>
			</tr>
			<tr>
				<th>image</th>
				<td class="col2">图形形式的提交按钮</td>
				<td class="col3"><input type="image" src="img/imgsub.jpg"></td>
			</tr>
			<tr>
				<th>radio</th>
				<td class="col2">单选按钮</td>
				<td class="col3">性别：
				<input type="radio" name="gender" value="0" checked>男
				<input type="radio" name="gender" value="1">女
				</td>
			</tr>
			<tr>
				<th>checkbox</th>
				<td class="col2">复选框</td>
				<td class="col3">
					兴趣：
					<input type="checkbox" name="habby" value="0" checked>旅游
					<input type="checkbox" name="habby" value="1">摄影
					<input type="checkbox" name="habby" value="2" checked>运动
				</td>
			</tr>
			<tr>
				<th>hidden</th>
				<td class="col2">隐藏字段</td>
				<td><input type="hidden" name="sname" value="1111"></td>
			</tr>
			<tr>
				<th>file</th>
				<td class="col2">文件上传</td>
				<td class="col3">
					上传个人照片<br>
					<input type="file" name="upfile" style="margin-left:145px;">
				</td>
			</tr>
		</tbody>
	</table>
</body>
</html>
```







