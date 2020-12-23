---
title: JQuery Backup
tags:
  - jquery
date: 2014-03-12 13:04:16
categories: frontend
---

<div align="center">
JQuery NOte.
ref: https://jquery.com/
</div>

<!--more-->
<!-- TOC -->

- [基本语法](#基本语法)
- [dom对象，jquery对象互转](#dom对象jquery对象互转)
- [加载顺序,window.onload区别](#加载顺序windowonload区别)
- [选择器](#选择器)
    - [基本选择器](#基本选择器)
    - [属性选择器](#属性选择器)
    - [伪类选择器](#伪类选择器)
    - [借助过滤函数选择元素](#借助过滤函数选择元素)
- [添加css样式](#添加css样式)
- [添加css的class类](#添加css的class类)
- [添加html或text](#添加html或text)
- [添加属性](#添加属性)
- [jq遍历](#jq遍历)
- [显示or隐藏](#显示or隐藏)
- [淡入淡出](#淡入淡出)
- [滑动](#滑动)
- [自定义动画](#自定义动画)
- [放大镜](#放大镜)
- [宽度, 位置, 回到顶部](#宽度-位置-回到顶部)
- [事件绑定](#事件绑定)
    - [当前元素绑定](#当前元素绑定)
    - [给子元素绑定](#给子元素绑定)
- [document增添处理](#document增添处理)
- [编写插件](#编写插件)
- [ajax](#ajax)
    - [$(xxx).load()](#xxxload)
    - [$.get()](#get)
    - [$.post()](#post)
    - [$.ajax()](#ajax)
    - [form表单序列化](#form表单序列化)
    - [ajax请求本地json文件](#ajax请求本地json文件)
    - [demo](#demo)

<!-- /TOC -->
# 基本语法

首先要引入JQ的依赖包 http://jquery.com/download/

```html
<script type="text/javascript">
   //1 js获得方式
   var username = document.getElementById("username");
   alert(username.value);
   //2 jQuery 获得方式
   // 语法：$("选择器")   或   jQuery("选择器")
   //  这里 $ 和 jQuery 等效
   var username2 = $("#username");
   alert(username2.val());
   // * 建议：jQuery对象变量的命名使用$开头，username2 建议写作： $username
</script>

```

# dom对象，jquery对象互转

函数中的 this 都是 原生对象, 使用时都要转换一下, 比如:  $(this).val();

```html
<script type="text/javascript">
   // <script>必须放在最后，加载顺序问题
   //1 获得dom
   var username = document.getElementById("username");
   //#1 使用dom 属性value获得值
   alert(username.value);
   
   //2 将dom 转换成  jQuery对象 --> $(dom对象)
   var $username = $(username);
   //#2 使用jQuery val()方法获得内容
   alert($username.val());
   
   //3 将jQuery 对象转换 dom对象
   //	jQuery对象内容，使用一个数组，存放所有内容，如果只有一个，下标为0.
   //#3.1 方式1 : 数组下标
   var username31 = $username[0];
   alert(username31.value);
   //#3.2 方法2：jQuery 提供get(index)方法
   var username32 = $username.get(0);
   alert(username32.value);
  
   //属性使用方法或属性
   alert($username.size());  //获得大小
   alert($username.length);  //获得大小
</script>


```

# 加载顺序,window.onload区别

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		
		<script src="js/jquery-3.1.0.min.js"></script>
		<script>
            // 和window.onload:区别 
            // window.onlead -> 页面所有资源加载完成后调用; 此外, onload 只会执行最后一个函数
        	//ready -> ：ready是在html,css加载之后，图片加载之前调用
			$(function(){
				console.log(document.body);
			});
			window.onload = function(){
				console.log('页面onload完成')
			};
			
		</script>
	</head>
	<body>
	<!--
		1.jQuery
		:是一个对原生JS的封装类库（处理好了所有兼容性问题）
		
		使用：本地路径
			：百度或者谷歌的cdn
		
	-->	
	</body>
	<!--<script src="http://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
	<script>
		//window.onload 
		
		jQuery(document).ready(function(){
			alert(111);
		});
		jQuery(document).ready(function(){
			alert(22222);
		});
		
		// jQuery == $
		$(document).ready(function(){
			alert(3333);
		});
		
		//简写:对以上形式简写
		$(function(){
			alert(4444);
		});
	</script>-->
</html>

```

# 选择器

## 基本选择器

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<p id="p1">我是第一个P1111</p>
		<p id="p2" class="classP2">我是第二个pppp</p>
		<div id="div1">
			<p id="div1P0">div1P0</p>
			<p id="div1P1">div1P1</p>
			<p id="div1P2">div1P2</p>
			<p id="div1P3">div1P3</p>
			<p id="div1P4">div1P4</p>
			<div>
				<p>马上下课</p>
			</div>
		</div>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		$(function(){
			/*
			 选择器查找元素
			 语法：$('选择器名')
			 获取到元素：以jQuery数组形式保存，无论是id还是class
			 
			 
			 * */
			var p1 = $("#p1");
			console.log(p1);//通过id获取，是以jq数组形式保存，不是原生p1对象
			// 选择所有 p 元素
			console.log($('p'));
			
			//class selector
			console.log($(".classP2"));
			
			//族群选择器 -- 同时选多个
			console.log($("#p1,.classP2"));

			//选择所有
			console.log($("*"));
			
			/**
			这两个对比:
			*/
			// 结果包含孙子元素, 选中有6个对象
			console.log($("#div1 p"));
			// 只包含子代元素, 选中5个对象
			console.log($("#div1>p"));
			
			// + :选中相邻的下一个兄弟元素
			console.log($("#div1P1+p"));
			
			//~:兄弟选择器:获取div1P1之后的所有兄弟p, 3个对象
			console.log($("#div1P1~p"));
		});
		
	</script>
</html>

```

## 属性选择器

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<p name="shitou">我是石头</p>
		<div name="shitou">我是div石头</div>
		<p name="xiaopeng">我是小鹏</p>
		<p name="anjing">我是安静</p>
		<p name="anfen" id="p4">我是安分</p>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		$(function(){
			/*
			 属性选择器
			 * */
			//[属性名]：获取具备该属性的所有元素, 5个
			console.log($("[name]"));
			// [属性名=属性值], 得到2个
			console.log($("[name='shitou']"));
			// 遍历数组, 打印text, 2个
			console.log($("[name='shitou']").text());//text()获取文本内容
			// 只获取 p 元素的text, 1个
			console.log($("p[name='shitou']").text());
			// 如果获取不到，默认返回document对象
			console.log($("p[name='shitou1']"));
			
			// [name *= value]:模糊查询 只要属性值包含u都被获取, 1个
			console.log($("p[name*='u']").text());
			
			//[name ^= value]:属性值以value开头, 2个
			console.log($("p[name ^= 'an']").text());
			
			//[name $= value]：属性值以value结尾, 1个
			console.log($("p[name $= 'tou']").text());
			
			//[name != value]:属性不等于value的所有元素, 3个
			console.log($("p[name != 'shitou']").text());
			
			//[name=属性选择器][name=属性选择器]....., 1个
			console.log($("p[name = 'anfen'][id='p4']").text());
		});
	</script>
</html>

```

## 伪类选择器

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>伪类选择器</title>
	</head>
	<body>
		<div id="div1">
			<span>学习</span>
			<p id='p1'>p1111111111</p>
			<p id='p2'>p2222222222</p>
		</div>
		<div>
			<span>JQ学习2</span>
			<p>唯一p</p>
		</div>
		
		<div style="display: none;">我是被隐藏的1</div>
		<div style="display: none;">我是被隐藏的2</div>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		$(function(){
			//首先要满足"第一个", div-div1下的 span, 且为第一个子元素
			console.log($("#div1 span:first-child").text());// 这里满足, 1个
			console.log($("#div1 p:first-child").text());// 不满足, 0个

			//在同类型里面找, 同类型的第一个(选取每一坨里面的同类型的第一个)------常用
			console.log($("#div1 span:first-of-type").text());// 1个
			console.log($("#div1 p:first-of-type").text());// 1个
			
			// 首先满足"最后一个", div1下的p, 且为最后一个子元素
			console.log($("#div1 p:last-child").text());// 1个

			// 同类型, 最后一个, 1个
			console.log($("#div1 p:last-of-type").text());
			
			// 当前元素的第index个子元素(从1开始数), 且为p元素, 结果找到p1
			console.log("当前元素的第index个子元素(从1开始数): "+$("#div1 p:nth-child(2)").text());

			// //同类型对比, 找到p2
			console.log("同类型里面找: "+$("#div1 p:nth-of-type(2)").text());
			// //所有奇数
			console.log("首先满足[所有奇数], 然后是p元素"+$("#div1 p:nth-child(odd)").text());
			// //所有偶数
			console.log("所有偶数"+$("#div1 p:nth-child(even)").text());
			
			console.log("选择所有"+$("#div1 p:nth-child(n)").text());
			console.log("同上"+$("#div1 p").text());
			
			// //选择div 下p ： p满足是一个唯一的子元素
			console.log("这里不满足"+$("div p:only-child").text());
			// //同类型对比
			console.log("选择div下的p, p必须是唯一子元素: "+$("div p:only-of-type").text());
			
			
			console.log("--------JQ新增----");
			// //JQ新增
			// //同类型对比
			console.log($("#div1 p:first").text());
			console.log($("#div1 p:last").text());
			
			// //not(选择器):找div1下的p,排除掉id是#p1的p
			console.log("找div1下的p,排除掉id是#p1的p: "+$("#div1 p:not(#p1)").text());
			
			// //eq(等于) 下标从0开始 处理选择结果数组 找到数组第n个元素
			console.log("下标从0开始 处理选择结果数组 找到数组第n个元素: "+$("#div1 p:eq(0)").text());
			
			// //gt(大于) greater than 下标从0开始  找出大于给定下标的所有元素
			console.log("找出大于给定下标的所有元素"+$("#div1 p:gt(0)").text());
			
			// //lt(小于下标) less than 
			console.log("小于: "+$("#div1 p:lt(1)").text());
			
			// //root 获取一个数组, 有一个document元素
			// get(0), 获取html原生对象
			console.log($(':root').get(0));
						
			// //hidden:当前隐藏的所有div
			console.log("当前隐藏的所有div"+$("div:hidden").text());
			// //hidden:当前可见的所有div
			console.log("可见的all div: "+$("div:visible").text());
			
			// //has(选择器) 选择div：div必须满足包含#p1选择器
			// 会把span, p里卖年的text都打印
			console.log("div必须满足包含#p1: "+$("div:has(#p1)").text());
			
			// //parent 是父元素
			// body里面的所有text都被打印
			console.log("parent: "+$("div:parent").text());
			
		});
	</script>
</html>

```

## 借助过滤函数选择元素

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>筛选</title>
		<style>
			
		</style>
	</head>
	<body>
		<div id="wrap">
			<div id="one"></div>
			<div id="two">
				<div id="cld1"></div>
				<div id="cld2"></div>
			</div>
			<div id="three"></div>
		</div>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		$(function(){
			//parent():父元素
			console.log($("#one").parent());
			//parents():祖先元素
			console.log($("#cld1").parents());
			
			//parents():指定祖先元素
			console.log($("#cld1").parents("#wrap"));
			
			//parentsUntil 指定到body元素之前结束，不包括body
			console.log($("#cld1").parentsUntil("body"));
			
			//children:第一代子元素
			console.log($("#wrap").children());
			
			//后代
			//找到所有后代
			console.log($("#wrap").find("*"));
			
			//找wrap下的cld1
			console.log($("#wrap").find("#cld1"));
			
			//兄弟
			/*
			 向前找兄弟：pre
			 * */
			console.log($("#three").prev());
			console.log($("#three").prevAll());
			
			//找到所有前面兄弟的  #one
			console.log($("#three").prevAll("#one"));
			console.log($("#three").prevUntil("#one"));
			
			//next
			console.log($("#one").next());
			console.log($("#one").nextAll());
			console.log($("#one").nextAll("#three"));
			console.log($("#one").nextUntil("#three"));
			
			//
			console.log($("#two").siblings());
			console.log($("#two").siblings("#one"));
			
		});
	</script>
</html>


```

# 添加css样式

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style>
			#div1{
				width: 100px;
				height: 100px;
				background-color: red;
			}
		</style>
	</head>
	<body>
		<div id="div1">hello world</div>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		/*
		 css函数或者设置样式:获取到行间样式和style标签下设置的样式
		 
		 * */
		var div1 = $("#div1");
		$(function(){
			console.log(div1.width(),typeof div1.width());// 100 "number"		
			console.log(div1.height(),typeof div1.height());		
			//字符串类型，有单位
			console.log(div1.css("width"),typeof div1.css("width"));// 100px string
			
			console.log(div1.css('background'));// rgb(255, 0, 0) none repeat scroll 0% 0% / auto padding-box border-box
			
			//设置
			div1.css("color","white");
			div1.css("font-size","30px");
			
			//一次性设置多个
			div1.css({
				width: 200,
				height: '200px',
				textAlign: 'center',
				lineHeight: '200px',
				border: "2px solid silver",
			});
			
			div1.css('height',function(index,value){
				console.log(index,value);
				//parseFloat():参数可以是number或者string类型，转化为number，只有数字
				console.log(parseFloat(value));
				console.log(parseInt(value));
				return 300 + parseFloat(value);
			});
			
			div1.css({
				width:function(index,value){
					console.log(index,parseFloat(value));
					return 100 + parseFloat(value);
				},
			});
			
			$(document).click(function(){
				div1.css({
					width: "+=100",
				});
			});
		});
	</script>
</html>

```

# 添加css的class类

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>添加属性</title>
		<style>
			#p1{
				width: 100px;
				height: 100px;
				/*background: red;*/
			}
			.one{
				background: blue;
			}
			.two{
				background: green;
			}
			.three{
				background: darkcyan;
			}
			.newClass{
				background: yellow;
			}
		</style>
	</head>
	<body>
		<p id="p1">一个p标签</p>
		<span>111111111</span>
		<span>222222222</span>
		<span>333333333</span>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		$(function(){
			//给一个元素添加类名
			/*
			 addClass(类名/函数) 返回值jQ对象
			 
			 * */
			
			var obj = $("#p1").addClass('one');
			//可以一次添加多个class
			obj.addClass("two three");
			console.log(obj);
			
			
			//使用函数作为参数添加类名，如果('#p1')选择出多个元素，那么遍历回调该函数，函数有两个参数
			//参数1：获取到元素对应下标，从0开始
			//参数2：以前设置的类名，
			// return: 如果要设置新的类名，通过设置函数返回值
			$('#p1').addClass(function(index,value){
				console.log(index,value);//0 "one two three"
				return 'newClass';
			});
			
			
			$("span").addClass(function(index,value){
				console.log(index,value);
				if(index == 1){
					return "one";
				}else{
					return "two";
				}
			});
			
			//JQ可以把所有获取到的元素添加上类名
			$('span').addClass('three');
			//eq(index):获取到JQ数组对象下对应下标的元素（jq对象）, 从0开始
			$("span").eq(1).addClass('newClass');

			console.log($("span").get(1));// 获取的是原生对象元素
			console.log($("span").eq(1));// 获取的是jq对象


			$("span").get(1).addClass("xxxyyy");// 没有$()就是原生对象
			$($("span").get(1)).addClass("xxxyyy");// 加上了
			
			//删除类名
			//删除$("span")查询结果所有
			//$("span").removeClass("three newClass one");
			
			//函数参数形式：如果想删除：return '类名'
			$('span').removeClass(function(index,value){
				console.log(index,value);
				//return 'two three';
				if(index == 0){
					return "three two";
				}else if(index == 1){
					return "three newClass";
				}else{
					return "three";
				}
			});
			
			//单独删除
			$('span').eq(1).removeClass('newClass');
			

			// 切换(添加or删除)
			//toggleClass(参数1，参数2)
			//参数1：类名 , 如果只有这个参数, 那么就是切换
			// 参数2：boolean：控制删除或者添加 true添加 false删除
			//删除
			$('#p1').toggleClass('one two three newClass',false);
			//添加
			$('#p1').toggleClass('one',true);
			
			//$('#p1').toggleClass('one');
			//$('#p1').toggleClass('one');
			
			//toggleClass:只传参数1类名，如果元素有对应的类名，删除，如果没有添加
			document.onclick = function(){
				$('#p1').toggleClass('one');
			};
			
		});
	</script>
</html>

```

# 添加html或text

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<p>111</p>
		<p>222</p>
		<p>333</p>
		
		<input type="text" />
		<input type="button" name="btn" value="点击" id="btn"/>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		$(function(){
			//如果$('p').html()：默认返回jq数组第一个元素内容
			var pHtml = $('p').html();
			console.log("默认返回jq数组第一个元素内容"+pHtml);
			// 返回指定的一个
			pHtml = $('p').eq(1).html();
			console.log(pHtml);
			
			//获取：不需要参数，
			// 如果加上参数：赋值
			//默认赋值所有
			$('p').html('哈哈');
			// 给指定的元素赋值
			$('p').eq(1).html('222');
			
			//结构标签赋值
			$('p').eq(0).html("<a href='###'>链接</a>");
			pHtml = $('p').eq(0).html();
			console.log("结构标签赋值: "+pHtml);
			
			//通过函数参数
			$('p').html(function(index,value){
				console.log(index,value);
				if(index == 0){
					return value+'新内容'+index;
				}
				return value+'新内容';
			});
			
			//text():innerText (纯文本)
			console.log($('p').eq(0).text());
			$('p').eq(1).text("<a href='###'>链接</a>");
			
			//val
			$('#btn').click(function(){
				//alert(111);
				//赋值
				$("input:text").val('111111');
				//函数赋值
				$("input:text").val(function(index,value){
					return value + '新值' + index;
				});
				console.log($('input:text').val());
			});
		});
	</script>
</html>

```

# 添加属性

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>添加属性</title>
	</head>
	<body>
		<p id="p1">ppppppppppppppp</p>
		<p id="p2">ppppppppppppppp</p>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		//attr():主要用来自定义属性
		//attr(属性名，属性值/函数)
		
		$(function(){
			//设置(默认全都被设置)
			$('p').attr('name','shitou');
			//添加多个属性，参数是对象类型
			$('#p1').attr({
				age:10,
				//修改
				name:'xiaoshitou',
				gender:'boy',
			});
			//使用函数自定义属性
			//attr(1,2)
			//参数1：修改或者添加的属性，
			// 参数2：函数对象，返回值是修改或者设置的值; value是old value
			$('#p1').attr('age',function(index,value){
				return +value+40;
			});
			//修改
			$('#p1').attr('age','100');
			//删除
			$('#p1').removeAttr('age');
			
			//prop():用来处理元素本身自带属性----只有自带的属性才会被读到
			// name不是自带的
			var value = $("#p1").prop('name');
			console.log(value);//undefined
			// id是自带属性
			var value2 = $("#p1").prop('id');
			console.log(value2);//p1
			
			//属性设置
			$('#p1').prop('classA','valueTest');
			
			//处理系统自带属性正常
			$('#p1').prop('class','valueTest');
		});
	</script>
</html>

```

# jq遍历

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<style>
			
		</style>
	</head>
	<body>
		<div id="choose"></div>
		<button id="btn1">全选</button>
		<button id="btn2">反选</button>
		<ul id="ul1">
			<li><input type="checkbox" /></li>
			<li><input type="checkbox" /></li>
			<li><input type="checkbox" /></li>
			<li><input type="checkbox" /></li>
			<li><input type="checkbox" /></li>
			<li><input type="checkbox" /></li>
		</ul>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		$(function(){
			//全选
			$("#btn1").click(function(){
				//所有checkbox选中
				//console.log($("input:checkbox"));
				//不需要循环，默认全部选中
				$("input:checkbox").prop("checked",true);
			});
			//反选
			$("#btn2").click(function(){
				
				//each:遍历jq数组，参数是函数指针，回调时返回对应下标和对应元素对象（原生对象）
				$("input:checkbox").each(function(index,ele){
					//console.log(index,ele);
					//console.log(ele.checked)
					//ele.checked = !ele.checked;
					
					//this就是每次对应取到的input对象，jq内部给回调处理
					//console.log(this);
					//方法2：
					//this.checked = !this.checked;
					
					//获取
					var value = $(this).prop("checked");
					//设置
					$(this).prop("checked",!value);
				});
			});
		});
	</script>
</html>

```

# 显示or隐藏

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>显示隐藏</title>
		<style>
			#div1{
				width: 200px;
				height: 200px;
				background-color: red;
			}
		</style>
	</head>
	<body>
		<button id="hidden">隐藏</button>
		<button id="show">显示</button>
		<button id="toggle">显示或者隐藏</button>
		<div id="div1"></div>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		//隐藏
		
		var div1 = $("#div1");
		$(function(){
			$("#hidden").click(function(){
				/*
		 hide(参数1，参数2，参数3)
		 参数1：动画持续时间 时间1000ms, 单位毫秒 也可以'slow' 'normal' 'fast'
		 参数2：动画效果 linear:匀速， 默认：swing:慢->快->慢  ，可以省略
		 参数3：动画执行完成后的回调函数：动画完成后才回调
		 * */
				//div1.hide();//没有动画，没有参数

				//修改宽高和透明度，为0时隐藏（display:none）
				div1.hide(1000,'linear',function(){
					console.log('隐藏动画完成');
				});
			});
			$("#show").click(function(){
				// 和hide类似
				div1.show('slow',function(){
					console.log("显示动画完成，可以做其他处理了");
				});
			});
			// 切换
			$("#toggle").click(function(){
				div1.toggle('slow',function(){
					console.log('显示或者隐藏，动画完成');
					var ifHide = div1.css('display') == "block" ? false : true;
					//ifHide ? 成立，执行：前，否则执行：后面
					ifHide ? $("#toggle").text("显示") : $("#toggle").text("隐藏");
				});
			});
		});
	</script>
</html>

```

# 淡入淡出

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>显示隐藏</title>
		<style>
			#div1{
				width: 200px;
				height: 200px;
				background-color: red;
			}
		</style>
	</head>
	<body>
		<button id="fadeIn">淡入</button>
		<button id="fadeOut">淡出</button>
		<button id="fadeToggle">淡入或者淡出</button>
		<button id="fadeTo">淡入到指定程度</button>
		<div id="div1"></div>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		
		var div1 = $("#div1");
		
		//淡出
		$("#fadeOut").click(function(){
			//透明度opacity为零时隐藏
			div1.fadeOut(1000,function(){
				console.log("淡出隐藏完成");
			});
		});
		
		//淡入：显示
		$("#fadeIn").click(function(){
			div1.fadeIn(1000,function(){
				console.log("淡入完成");
			});
		});
		// 切换
		$("#fadeToggle").click(function(){
			div1.fadeToggle(1000,function(){
				console.log("淡入或淡出完成");
				var ifHide = div1.css('display') == "block" ? false : true;
					//ifHide ? 成立，执行：前，否则执行：后面
					ifHide ? $("#fadeToggle").text("淡入") : $("#fadeToggle").text("淡出");
			});
		});
		
		//fadeTo
		/*
		 参数2变化：指定透明度
		 * */
		$("#fadeTo").click(function(){
			div1.fadeTo("slow",0.5,function(){
				console.log("动画完成");
			});
		});
	</script>
</html>

```

# 滑动

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>显示隐藏</title>
		<style>
			#div1{
				width: 200px;
				height: 200px;
				background-color: red;
			}
		</style>
	</head>
	<body>
		<button id="slideUp">上滑</button>
		<button id="slideDown">下滑</button>
		<button id="slideToggle">上滑或者下滑</button>
		<button id="slideUpAndDown">上滑下滑</button>
		<div id="div1"></div>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		var div1 = $("#div1");
		$(function(){
			//高度减小，为0时隐藏
			$("#slideUp").click(function(){
				$("#div1").slideUp(3000,function(){
					console.log("上滑完成");
				});
			});
			$("#slideDown").click(function(){
				$("#div1").slideDown(3000,function(){
					console.log("下滑完成");	
				});
				
			});
			$("#slideToggle").click(function(){
				$("#div1").stop().delay().slideToggle(1000,function(){
					console.log("上滑或下滑完成");
					var isHide = $("#div1").css("display") == "block" ? false : true;
					isHide ? $("#slideToggle").text("下滑") : $("#slideToggle").text("上滑");
				});
			});
			//slideUpAndDown:先上滑，完后下滑
			$("#slideUpAndDown").click(function(){
				//1：动画完成回调函数处理
//				div1.slideUp(2000,function(){
//					div1.slideDown("slow");
//				});
				//2：代码回继续往下执行，动画是异步操作，不会阻塞代码执行，但是未被执行的动画效果会被放到等待队列中，slideDown会被放在队列中，只有等上一个动画执行完成后，从队列去除第一个动画执行
//				div1.slideUp(2000);
//				div1.slideDown(2000);
//				div1.fadeOut(2000);
//				div1.fadeIn(2000);
//				div1.slideUp(2000);
//				div1.slideDown(2000);
				
				div1.slideUp(2000).slideDown(2000);
			});
		});
		
		
	</script>
</html>

```

# 自定义动画

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>显示隐藏</title>
		<style>
			#div1{
				width: 200px;
				height: 200px;
				background-color: red;
				position: absolute;
			}
		</style>
	</head>
	<body>
		<button id="custom1">自定义动画1</button>
		<button id="custom2">自定义动画2</button>
		<button id="custom3">自定义动画3</button>
		<button id="stop">流程控制</button>
		<div id="div1">文字</div>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		var div1 = $("#div1");
		/*
		 animate(属性列表，时间，时长，变化曲线，回调函数)
		 * */
		$(function(){
			$("#custom1").click(function(){
				div1.animate({
					width:300,
					height:300,
					//复合属性使用驼峰命名
					fontSize: 100,
					//背景渐变不支持
					backgroundColor: "blue",
					left:100,
					top:100,
				},1000,function(){
					console.log('动画完成');
				});
				
			});
			$("#custom2").click(function(){
				//字符串或者数字，单位可以加可以省略
				//+= 在之前基础上加
				div1.animate({
					//width:'+=300',
					//height:'+=100',
					left: '+=50',
					top:'+=50',
				},2000);
			});
			$("#custom3").click(function(){
				div1.animate({width:300,},2000);
				//div1.delay(2000,'fx');//等待2秒继续下一个队列动画
				div1.animate({height:300},2000);
				div1.animate({width:300},2000);
				div1.animate({height:200},2000);
			});
			//停止动画
			/*
			 stop(参数1，参数2)
			 ：两个参数是boolean值，默认都是false
			 参数1：stopAll(默认false是只清除正在执行动画，马上开启队列中第一个等待对话，如果给true:会清除正在执行动画和队列中所有的动画)
			 参数2：goEnd:默认false：直接停止动画，如果是true：停止动画，但是动画最终效果会显示
			 * */
			$("#stop").click(function(){
				//div1.stop();
				//div1.stop(true);
				//div1.stop(false,true);
				
				//停止当前动画和队列中所有动画，并且显示当前动画的效果
				//div1.stop(true,true);
				
				//延迟动画执行
				//div1.delay(2000);
				
				//结束动画（立即停止当前对话，和队列中所有等待动画，并且：元素效果是动画结束后的最终效果）
				div1.finish();
			});
		});
		
	</script>
</html>

```

# 放大镜

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style>
			.jzoom{
				width: 320px;
				height:372px;
				border: 2px solid silver;
			}
		</style>
	</head>
	<body>
		<div class="jzoom">
  			<img src="img/mm.jpg">
		</div> 
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script src="js/jzoom.min.js"></script>
	<script src="js/jzoom.js"></script>
	<script>
		$('.jzoom').jzoom({
			 width: 400,
             height: 400,
             position: "right",
             offsetX: 20,
             offsetY: 0,
             opacity: 0.6,
             bgColor: "#fff",
             loading: "Loading..."
		});
	</script>
</html>

```

# 宽度, 位置, 回到顶部

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<style>
			body{
				height: 2000px;
			}
			#div1{
				width: 100px;
				height: 100px;
				padding: 50px;
				margin: 50px;
				border: 10px solid silver;
				background-color: red;
				position: absolute;
				left: 10px;
				top: 10px;
			}
			#box{
				width: 300px;
				height: 300px;
				background: silver;
				position: relative;
				left: 100px;
				top: 400px;
			}
			#div2{
				width: 100px;
				height: 100px;
				position: absolute;
				background: yellow;
				border: 2px solid brown;
				left: 50px;
				top: 40px;
			}
		</style>
	</head>
	<body>
		<div id="div1"></div>
		<div id="box">
			<div id="div2"></div>
		</div>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		var div1 = $("#div1");
		var div2 = $("#div2");
		$(function(){
			console.log(div1.width());
			console.log(div1.height());
			//设置宽高
			//div1.width(200);
			//div1.height(200);
			
			//innerWidth:内容宽度+padding左右
			//innerHeight:内容高度+padding上下
			console.log(div1.innerWidth());
			console.log(div1.innerHeight());
			
			
			//outerHeght()内容高度+上下padding+上下边框 = offSetHeight
			console.log(div1.outerWidth());
			console.log(div1.outerHeight());
			
			//outerHeight:参数：布尔值：默认false,不会计算margin值，如果是true，会计算margin
			console.log(div1.outerHeight(true));
			
			
			//offset()：当前元素基于页面文档的位置
			console.log(div1.offset());
			console.log(div1.offset().left);
			console.log(div1.offset().top);
			
			//只能接受number类型
			div1.offset({
				left: 100,
				top: 100,
			});
			
			//div1.css({});
			
			//position():当前元素基于最近定位父元素的距离
			console.log(div2.position());
			console.log(div2.position().left);
			
			
			console.log(div2.offset());
			
			$(document).click(function(){
				//可以获取
				console.log($("body").scrollTop());
				console.log($("body").scrollLeft());
				//不可以获取
				console.log($("html").scrollTop());
				//可以获取
				console.log($(window).scrollTop());
				
				//回到顶部
				//$(window).scrollTop(0);
				
				$("body").animate({
					scrollTop: 0,
				},500);
			});
		});
	</script>
</html>

```

# 事件绑定

## 当前元素绑定

```html
<!doctype html>
<html lang="en">
<head>
	<meta charset="UTF-8" />
	<title>Document</title>
	<style>
		#div1{
			width: 100px;
			height: 100px;
			background: red;
		}
		#div2{
			width: 100px;
			height: 100px;
			background: blue;
			position: absolute;
		}
	</style>
</head>
<body>
	<div id="div1"></div>
	<div id="div2"></div>
</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		//1.支持冒泡
		var div1 =$("#div1");
		var div2 =$("#div2");
		
		div1.click(clickFn);
		function clickFn(event){
			console.log(event);
			console.log(event.clientX,event.clientY);
			console.log(event.offsetX,event.offsetY);
			console.log(event.pageX,event.pageY);
		}
		
		//同类型函数不会被覆盖
		div1.click(function(){
			console.log("11111111");
		});
		
		//参数1：事件类型 参数2：回调函数
		div1.on('click',function(){
			console.log('点击效果');
		});
		
//		div1.on("mouseover mouseout",function(){
//			console.log("移入或者移出");
//			$("#div2").toggle("slow");
//		});
		
		//hover(参数1，参数2)：参数1：鼠标移入调用 参数2：鼠标移出调用
		div1.hover(function(){
			$("#div2").hide('slow');
		},function(){
			$("#div2").show('slow');
		});
		
		//off():参数1：要删除的事件 ，参数2：方法名（地址），可以省略，省略的话会删除所有点击
		div1.off('click',clickFn);
		//删除div1上所有点击事件
		div1.off('click');
		
		
		//绑定的函数只会执行一次
		div1.one('click',function(){
			alert(1111);
		});
		
		
		//mousedown
		//mousemove
		//mouseup
		
		div2.mousedown(function(ev){
//			var offsetX = ev.offsetX;
			var offsetX = ev.clientX - div2.offset().left;
			var offsetY = ev.offsetY;
			
			//move
			$(document).mousemove(function(ev){
				var x = ev.clientX - offsetX;
				var y = ev.clientY - offsetY;
				//赋值
//				div2.css({
//					left:x,
//					top:y
//				});
				
				var domObj = div2.get(0);
				domObj.style.left = x + "px";
				domObj.style.top = y + "px";
			});
			//up
			$(document).mouseup(function(){
				$(document).off("mouseup mousemove");
			});
		});
		
		//键盘事件
		$(document).keydown(function(ev){
			//which == keyCode
			console.log(ev.keyCode);
			console.log(ev.which);
		});
	</script>
</html>
```

## 给子元素绑定

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>子元素绑定事件</title>
		<style>
			#div1{
				width: 200px;
				height: 200px;
				background:red;
			}
			#inner{
				width:100px;
				height: 100px;
				background:yellow;
			}
			p{
				width: 100px;
				height: 100px;
				background: blue;
			}
		</style>
	</head>
	<body>
		<div id="div1">
			<div id="inner"></div>
			<!--<p></p>-->
		</div>
		<button id="btn">点击</button>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		$(function(){
			//参数2：必须是div1的子元素，相当于给div1下的#inner绑定事件
			$("#div1").on("click","#inner",function(){
				alert(11);
			});
			
			
			//给p绑定事件
			$("#div1").on("click","p",function(){
				alert("点击p事件");
			});
			
			//创建p标签
			$("button").click(function(){
				$("<p></p>").appendTo($("#div1"));
			});
			
		});
	</script>
	
</html>

```

# document增添处理

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title></title>
		<style>
			#div1{
				width: 100px;
				height: 100px;
				background: red;
			}
		</style>
	</head>
	<body>
		<div id="div1"></div>
		<input type="button" name="btn" id="btn" value="点击我"/>
		<input type="button" name="btn" id="clone" value="点击克隆"/>
		<ul>
			<li>1111</li>
			<li>2222</li>
			<li>3333</li>
		</ul>
	</body>
	
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		$(function(){
			var div1 = $("#div1");
			//a.append(b):把b放入a的尾部
			div1.append("<p><span>11111</span></p>");
			// a.appendTo(b):把a添加到b的尾部
			$("<a href='#'>link</a>").appendTo(div1);
			
			//a.prepend(b):把b放到a前面
			div1.prepend('<b>prepend</b>');
			
			//a.prependTo(b):把a放到b前面
			$("<i>prependTo</i>").prependTo(div1);
			
			//a.after(b):把b放到a的下面
			div1.after("<a href='##' class='aaa'>第二个来接</a>")
			
			//a.insertAfter(b):把a放到b的后面
			$("<p>insertAfter</p>").insertAfter(div1);
			
			//a.before(b):把b放到前面
			div1.before("<a href='#' >before</a>");
			
			//a.insertBefore(b):把a放到b的前面
			$("<p>insertBefore</p>").insertBefore(div1);
			
			
			//删除：remove()
			//要删除对象调用remove方法，把对象对应删除
			//$("a").remove();//删除所有a
			$("a").eq(0).remove();//删除第一个a
			//remove(添加选择器)
			//找到所有a，删除类是.aaa的a
			$('a').remove(".aaa");//删除class="aaa"的a
			
			$("#btn").on("click",function(){
				alert(1111);
			});
			
			//detach():删除界面效果和remove()一样，也都会把对应删除对象返回，但是remove会把之前绑定的方法删除，detach保留方法
			var btn = $("#btn").detach();
			//var btn = $("#btn").remove();
			console.log(btn);
			
			//再次添加
			btn.appendTo($("body"));
			
			//empty()：把所哟子元素和自身文本节点全部清空
			//div1.empty();
			
			//包裹元素
			//wrap:对应每一个li都对应包裹一个div
			//$("li").wrap("<div></div>");
			//wrapALl把所有li被div包裹
			//$("li").wrapAll("<div></div>");
			//wrapInner每个li的内容被对应div包裹
			$("li").wrapInner("<div></div>");
			
			
			//替换
			$("a").replaceWith("<span>替换a</span>");
			//把所有p替换成span
			$("<span>替换p</span>").replaceAll("p");
			
			//克隆
			//clone(true)深克隆 把方法也克隆
			$("#clone").click(function(){
				$(this).clone(true).insertAfter(this);
			});
		});
	</script>
</html>

```

# 编写插件

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title>插件</title>
	</head>
	<body>
		<ul>
			<li>1</li>
			<li>2</li>
			<li>3</li>
			<li>4</li>
			<li>5</li>
		</ul>
	</body>
	<script src="js/jquery-3.1.0.min.js"></script>
	<script>
		//改$ JQ对象拖拽添加
		$.extend({
			hello:function(){
				console.log("你好");
			},
			eat:function(){
				console.log("吃东西");
			},
			
		});
		
		//给所有jq对象都添加了toAlert
		$.fn.extend({
			toRed:function(){
				//this:调用对象
				console.log(this);
				this.css('background','red');
			},
			toColor:function(color){
				color = color || '#ccc';
				this.each(function(index,ele){
					//this=ele=每次遍历出的对象
					console.log(this,ele,index);
					if(index % 2 != 0){
						$(this).css("background",color);
					}
				});
			},
			dianji:function(fn){
					//this:是调用者（li集合）
				this.click(function(event){
					//this:是每一个对应点击对象，是jq内部修改的指向
					//alert("text内部方法");
					//回调
					//fn(event);
					//call();参数1，要修改的this指向
					//修改fn这个方法的this指向，call第一个参数是要修改的this指向，后面的参数和fn参数顺序个数一样
					fn.call(this,event);
				});
				
				
			},
		});
	</script>
	<script>
		$(document).click(function(){
			//$("ul li").toColor('red');
		});
		$("ul li").dianji(function(ev){
			//salert("点击我了");
			//alert(this);
			console.log(ev,this);
		});
	</script>
</html>

```

# ajax

* jQuery对 JS AJAX 进行封装，回顾js ajax引擎 （XMLHttpRequest）

* jQuery 提供三个层次 ajax处理api。
	
    * 第一层：$.ajax  最底层ajax操作，可以完成非常强大的功能，但操作繁琐。【】

	* 第二层：load、$.get()、$.post  开发使用比较多的，对$.ajax 简化。【】

	* 第三层：$.getJSON  、$.getScript()  一般不用。
		
        * $.getJSON  获得json数据，可以用于js跨域操作。
        
		* $.getScript()  动态加载js文件。

## $(xxx).load()

```js
/*2.1 load方法 ，不是全局的，必须有jQuery对象
     * load(url,[data],[callback]) 
     *     参数1：请求路径
     *     参数2：请求参数
     *     参数3：回调函数
     *      
     *  1)load方法如果没有请求参数，底层使用get请求方式
     *  2)load方法如果有请求参数，底层使用post请求方式
     *  3)load方法获得数据即使是json，也是以字符串方法展现，需要手动转换json对象
     */
    $(this).load(url,params,function(data){
            //手动转换
            var jsonData = eval("("+data+")");
            alert(jsonData.message);
    });

```

## $.get()

```js
/* 2.2 $.get()方法 ,只能发送get请求
         * $.get(url,[data],[fn],[type]) 
         *     参数1：请求路径
         *     参数2：请求参数,如果是中文存放get请求的中文乱码问题。一般get方法不发送中文。
         *      	servlet解决方法  new String(username.getByte("ISO-8859-1"),"UTF-8");
         *     参数3：回调函数
         *     参数4：处理回调函数的第一个参数，确定数据类型
         *     		 例如：返回内容格式，xml, html, script, json, text, _default。
         * $.get(....,"json") 表示数据是json数据。如果servlet发送 application/json 不需要使用，如果发送 text/html就需要使用
         */
        $.get(url,params,function(data){
                alert(data);
        },"json");

```

## $.post()

```js
/* 2.3 $.post 只能发送post请求
     * $.post(url,[data],[fn],[type]) 
     *     参数与get雷同
     */
    $.post(url,params,function(data){
            alert(data);
    });

```

## $.ajax()

```js
/* 2.4 $.ajax 底层ajax请求
     * $.ajax(url,[settings]) 
     * 一般使用格式：$.ajax(settings)
     *         url ：请求路径
     *         data ：请求参数
     *         type ：请求方式，get、post。默认: GET
     *         success ：成功后回调函数
     *         error ：异常时回调函数
     *         dataType ：务器返回的数据类型。例如：json、html、xml 等
     *             "xml": 返回 XML 文档，可用 jQuery 处理。
     *             "html": 返回纯文本 HTML 信息；包含的script标签会在插入dom时执行。
     *             "script": 返回纯文本 JavaScript 代码。不会自动缓存结果。除非设置了"cache"参数。'''注意：'''在远程请求时(不在同一个域下)，所有POST请求都将转为GET请求。(因为将使用DOM的script标签来加载)
     *             "json": 返回 JSON 数据 。
     *             "jsonp": JSONP 格式。使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数。
     *             "text": 返回纯文本字符串
     */
    $.ajax({
            "url":url,
            "data":params,
            "type":"post",
            "success":function(data){
                    alert(data);
            },
            "error":function(){
                    alert("服务器异常");
            },
            "dataType":"json"
    });

```

## form表单序列化

serialize()  将表单中所有内容转成字符串 。格式：username=jack&password=1234&....
serializeArray()  将表单中所有内容转成json格式

## ajax请求本地json文件

https://blog.csdn.net/xuanpl/article/details/68928586

## demo

```html
<!-- Html页面： -->
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
    <script type="text/javascript" src="../js/jquery-1.8.3.js"></script>
    <script type="text/javascript">
        $(function(){
            //点击事件
            $("#buttonId").click(function(){
            1.1 请求路径，这里写的是固定的地址，也可以写动态的：  ${pageContext.request.contextPath}
                var url = "/jquery_day02/GetDataServlet";
            1.2 请求参数----必须是json格式
                var params = {"username":"杰克","password":"1234"};
                
            2.1 load方法 ，不是全局的，必须有jQuery对象----- $(xxx).load()
                 * load(url,[data],[callback]) 
                 *  参数1：请求路径
                 *  参数2：请求参数
                 *  参数3：回调函数
                 * 
                * 注意：
                 *  1)load方法如果没有请求参数，底层使用get请求方式
                 *  2)load方法如果有请求参数，底层使用post请求方式
                 *  3)load方法获得数据即使是json，也是以字符串方法展现，需要手动转换json对象
             $(this).load(url,params,function(data){
                 //手动转换
                 var jsonData = eval("("+data+")");
                 alert(jsonData.message);
             });
              
                
             2.2 $.get()方法  ,只能发送get请求
                 * $.get(url,[data],[fn],[type]) 
                 *  参数1：请求路径
                 *  参数2：请求参数,如果参数含有中文，则存在get请求的中文乱码问题。一般get方法不发送中文。
                 *      servlet处理乱码：  new String(username.getByte("ISO-8859-1"),"UTF-8");
                 *  参数3：回调函数
                 *  参数4：处理回调函数的第一个参数，--------确定数据类型
                 *     例如：返回内容格式可以有：xml, html, script, json, text, _default。
                 *        $.get(....,"json") 表示接收的数据是json数据。如果servlet设置响应格式： application/json 则不需要使用，如果设置的是 text/html就需要使用
                $.get(url,params,function(data){---这里servlet设置了响应格式：application/json，get（）无需参数type
                    alert(data);
                },"json");
                
             2.3 $.post 只能发送post请求
                 * $.post(url,[data],[fn],[type]) 
                 *  参数与get雷同
                $.post(url,params,function(data){
                    alert(data);
                });
                 */
                
     2.4 $.ajax   底层ajax请求
         * $.ajax(url,[settings]) 
         * 一般使用格式：$.ajax(settings)
         *      url ：请求路径
         *      data ：请求参数
         *      type ：请求方式，get、post。默认: GET
         *      success ：成功后回调函数
         *      error ：异常时回调函数
         *      dataType ：务器返回的数据类型。例如：json、html、xml 等
         *          "xml": 返回 XML 文档，可用 jQuery 处理。
         *          "html": 返回纯文本 HTML 信息；包含的script标签会在插入dom时执行。
         *          "script": 返回纯文本 JavaScript 代码。不会自动缓存结果。除非设置了"cache"参数。'''注意：'''在远程请求时(不在同一个域下)，所有POST请求都将转为GET请求。(因为将使用DOM的script标签来加载)
         *          "json": 返回 JSON 数据 。
         *          "jsonp": JSONP 格式。使用 JSONP 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数。
         *          "text": 返回纯文本字符串
         
                $.ajax({
                    "url":url,
                    "data":params,
                    "type":"post",
                    "success":function(data){-----------------data是从action传过来的数据；
                        alert(data);
                    },
                    "error":function(){
                        alert("服务器异常");
                    },
                    "dataType":"json"
                });
            });
        });
    </script>
</head>
<body>
    <input id="buttonId" type="button" value="发送ajax" />
</body>
</html>

```
