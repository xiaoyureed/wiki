---
title: Css Tips
tags:
  - css
date: 2014-03-11 10:27:12
categories: frontend
---

<div align="center">

https://www.runoob.com/css/css-rwd-grid.html

https://developer.mozilla.org/en-US/docs/Web/HTML


ref: http://zh.learnlayout.com/display.html, https://github.com/leiqikui/CSS-tutorial

hover动画lib: https://github.com/IanLunn/Hover

动画库: https://github.com/daneden/animate.css

demo: [📚 github](https://github.com/xiaoyureed/css-layout)

DSL: 领域特定语言, css就是一种dsl [=>](https://zhuanlan.zhihu.com/p/22824177)
</div>

<!--more-->
<!-- TOC -->

- [兼容性处理](#兼容性处理)
- [flex 布局](#flex-布局)
- [best-practice](#best-practice)
  - [css 重置 reset](#css-重置-reset)
  - [overflow-hidden](#overflow-hidden)
    - [溢出隐藏](#溢出隐藏)
    - [清除浮动](#清除浮动)
    - [防止 parent margin-top 跟着一起下来](#防止-parent-margin-top-跟着一起下来)
  - [定位类](#定位类)
  - [文本字体类](#文本字体类)
  - [图像图片](#图像图片)
  - [其他](#其他)
- [引入css三种方式](#引入css三种方式)
- [长度单位总结](#长度单位总结)
- [responsive-web-design 响应式布局](#responsive-web-design-响应式布局)
  - [百分比宽度](#百分比宽度)
  - [rem 布局](#rem-布局)
  - [视口单位布局](#视口单位布局)
  - [媒体查询 @media](#媒体查询-media)
- [盒子模型](#盒子模型)
  - [消除padding和border影响](#消除padding和border影响)
  - [margin重合](#margin重合)
  - [子margin-top传递给父亲问题](#子margin-top传递给父亲问题)
  - [auto的用法](#auto的用法)
  - [页面自适应的盒子图](#页面自适应的盒子图)
- [常用CSS属性](#常用css属性)
  - [CSS背景属性(Background)](#css背景属性background)
  - [CSS边框属性(BorderfflOutline)](#css边框属性borderffloutline)
  - [CSS文本属性 text](#css文本属性-text)
  - [CSS字体属性 font](#css字体属性-font)
  - [CSS外边距属性(Margin)](#css外边距属性margin)
  - [CSS内边距属性(Padding)](#css内边距属性padding)
  - [CSS列表属性(List)](#css列表属性list)
  - [CSS尺寸属性(Dimension)](#css尺寸属性dimension)
  - [CSS定位属性(Positioning)](#css定位属性positioning)
  - [CSS表格属性(Table)](#css表格属性table)
- [选择器](#选择器)
  - [元素选择器 类选择器 id选择器](#元素选择器-类选择器-id选择器)
  - [属性选择器](#属性选择器)
  - [伪类选择器](#伪类选择器)
  - [条件选择器(first-child,focus,checked)](#条件选择器first-childfocuschecked)
  - [伪元素选择器(first-letter,first-line,before,after)](#伪元素选择器first-letterfirst-linebeforeafter)
  - [css3新增选择器](#css3新增选择器)
- [CSS-tips](#css-tips)
  - [选择器总结](#选择器总结)
    - [星号选择器[*]](#星号选择器)
    - [id选择器[#xxx]](#id选择器xxx)
    - [类选择器[.xxx]](#类选择器xxx)
    - [后代选择器[x y][x>y]](#后代选择器x-yxy)
    - [元素选择器[x]](#元素选择器x)
    - [伪类选择器[a:link]](#伪类选择器alink)
    - [邻近元素选择器[x+y][x~y]](#邻近元素选择器xyxy)
    - [属性选择器(x[attrName])](#属性选择器xattrname)
  - [display](#display)
  - [margin: 0 auto](#margin-0-auto)
  - [max-width](#max-width)
  - [position](#position)
  - [float](#float)
    - [clear:left|right|both](#clearleftrightboth)
    - [浮动元素溢出](#浮动元素溢出)
  - [column,文字多列布局](#column文字多列布局)
  - [创建小圆形](#创建小圆形)
  - [clearfix(清除浮动)](#clearfix清除浮动)
  - [响应式方块](#响应式方块)
  - [给无序列表添加数字](#给无序列表添加数字)
  - [自定义文本选择颜色](#自定义文本选择颜色)
  - [在css中使用自定义变量](#在css中使用自定义变量)
  - [禁用文本选择](#禁用文本选择)
- [字体](#字体)
- [引入媒体元素](#引入媒体元素)
  - [背景图片](#背景图片)
- [隐藏](#隐藏)
  - [伪元素解决浮动造成父元素高度为0的问题](#伪元素解决浮动造成父元素高度为0的问题)
- [outline(轮廓)](#outline轮廓)
- [修改列表的样式](#修改列表的样式)
- [文本设置](#文本设置)
- [垂直居中](#垂直居中)
- [修复图片间隙问题](#修复图片间隙问题)
- [修复元素间隙问题](#修复元素间隙问题)
- [块元素, 内联元素, inline-block](#块元素-内联元素-inline-block)
- [table样式](#table样式)
- [响应式布局](#响应式布局)
  - [media type](#media-type)
  - [media query 媒体查询](#media-query-媒体查询)
  - [viewport虚拟窗口](#viewport虚拟窗口)
- [超出隐藏or滚动,css雪碧图](#超出隐藏or滚动css雪碧图)
  - [雪碧图-demo](#雪碧图-demo)
- [浮动](#浮动)
  - [块元素同行显示,消除元素间隙](#块元素同行显示消除元素间隙)
  - [脱离文档流](#脱离文档流)
  - [左右浮动, 清除浮动影响](#左右浮动-清除浮动影响)
  - [浮动造成父元素高度为0的问题](#浮动造成父元素高度为0的问题)
  - [浮动-demo](#浮动-demo)
    - [九宫格](#九宫格)
    - [float,div布局经典用法](#floatdiv布局经典用法)
- [定位](#定位)
  - [z-index层级](#z-index层级)
  - [relative相对定位](#relative相对定位)
    - [relative相对定位的优先级问题](#relative相对定位的优先级问题)
  - [absolute绝对定位](#absolute绝对定位)
    - [div在div内部居中](#div在div内部居中)
    - [图片上的灰色蒙板,居中](#图片上的灰色蒙板居中)
  - [窗口定位](#窗口定位)
- [过场动画](#过场动画)
- [seo](#seo)
- [demo](#demo)
  - [典型注册表单](#典型注册表单)
  - [典型的图片文字排版](#典型的图片文字排版)
  - [选中则变换颜色](#选中则变换颜色)
  - [小方块信息展示](#小方块信息展示)

<!-- /TOC -->

# 兼容性处理

- 添加 css 前缀, 可以借助 Prefixr, Autoprefixer

# flex 布局

http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html 图示
http://www.ruanyifeng.com/blog/2015/07/flex-examples.html 实例

将容器规定了坐标轴, 水平的主轴（main axis）和垂直的交叉轴（cross axis）; 主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end

单个子元素占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size

容器属性

```

display: flex; ----- 任何一个容器都可以指定为 Flex 布局
display: inline-flex; -----行内元素也可以使用 Flex 布局



flex-direction 决定  main axis 方向
    row（默认值）：主轴为水平方向，起点在左端。
    row-reverse：主轴为水平方向，起点在右端。
    column：主轴为垂直方向，起点在上沿。
    column-reverse：主轴为垂直方向，起点在下沿

flex-wrap 决定 如果一条轴线排不下，子元素如何换行
    nowrap（默认）：不换行, 每个子元素缩小宽度
    wrap：换行，第一行在上方
    wrap-reverse：换行，第一行在下方

flex-flow 是flex-direction属性和flex-wrap属性的简写形式
    row nowrap 默认值

justify-content 定义子元素在主轴上的对齐方式
    flex-start（默认值）：左对齐
    flex-end：右对齐
    center： 居中
    space-between：两端对齐，项目之间的间隔都相等。
    space-around：均匀分布, 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

align-items 定义项目在交叉轴上如何对齐
    flex-start：交叉轴的起点对齐。
    flex-end：交叉轴的终点对齐。
    center：交叉轴的中点对齐。
    baseline: 子元素的第一行文字的基线对齐。
    stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度

align-content 定义了多根主轴线存在时候的对齐方式。如果项目只有一根轴线，该属性不起作用
    flex-start：与交叉轴的起点对齐。
    flex-end：与交叉轴的终点对齐。
    center：与交叉轴的中点对齐。
    space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
    space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
    stretch（默认值）：每行主轴上的元素尺寸刚好占满交叉轴。
```

子元素属性

```

order 定义项目的排列顺序。数值越小，排列越靠前，默认为0。
flex-grow 定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大
    如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。
flex-shrink 属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小, 为 0 不缩小, 负值对该属性无效。
    如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。
flex-basis 定义了在分配多余空间之前，项目占据的主轴空间（main size）默认值为auto，即项目的本来大小
    可以设为跟width或height属性一样的值（比如350px），则项目将占据固定空间
flex 是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选
    有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。
    建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值
align-self 允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch
    除了auto，其他属性值都与align-items属性完全一致

```


# best-practice


https://juejin.im/entry/6844903462287704077
https://coderlmn.github.io/code-standards/#_web_typography

## css 重置 reset

```css
* {
  margin: 0;
  padding: 0;
  /* Chrome浏览器box-sizing默认是content-box 这意味着元素的border和padding等不能算在元素的width和height中 ，
  padding和border的改变不能改变width和height的值 */
  /* 使用这种盒子模型后, padding 和 border 就包含在宽高里面了 */
  /* 指定 宽高后, 想要增加内容边距, 要使用  父元素的 padding */
  box-sizing: border-box;
}

html, body, #root {
    height: 100%;
}

body {
  background-color: #f3f3f3; /* #f5f5f5 也可 */
}



```

## overflow-hidden

https://blog.csdn.net/qq_41638795/article/details/83304388

### 溢出隐藏

```css
overflow: hidden;  /*溢出隐藏*/

/* 文本省略 */
div{ 
    width: 150px;
    background: skyblue;
    overflow: hidden;      /*溢出隐藏*/
    white-space: nowrap;	/*规定文本不进行换行*/
    text-overflow: ellipsis;	/*当对象内文本溢出时显示省略标记（...）*/
}
```

### 清除浮动

```html
<style>
    .box {
    background: red;
    /* 若不加, parent 会坍缩高度为 0, 加上后, 高度会自适应 */
    overflow: hidden;
    /* 兼容 ie6 */
    zoom: 1;
    }
    .kid {
    width: 100px;
    height: 100px;
    /* son 向左浮动 */
    float: left;
    }
    .kid1 {
    background: yellow;
    }
    .kid2 {
    background: orange;
    }
    /* 若 parent box 不加清除浮动, 这个元素会顶上去  */
    .wrap {
    width: 300px;
    height: 150px;
    background: blue;
    color: white;
    }
</style>
<body>
  <div class="box">
    <div class="kid kid1">子元素1</div>
    <div class="kid kid2">子元素2</div>
  </div>
  <div class="wrap">其他部分</div>
</body>

```

### 防止 parent margin-top 跟着一起下来

```html
<style>
    .box {
        background: red;
        overflow: hidden; /*防止parent的 margin top跟着一起下来*/ 
    }
    .kid {
        width: 100px;
        height: 100px;
        background: yellow;
        margin-top: 20px;
    }

</style>
<body>
  <div class="box">
    <div class="kid">子元素1</div>
  </div>
</body>
```


## 定位类


1. 对于block元素正常来说宽度由外部决定, 但是在绝对定位(absolute, fixed)时, 宽度由内部内容决定 (包裹性), 当然, 如果此时设置了 left/right, 就变为外部决定了

1. height:100% 要想生效, 需要父元素设置有效的 height, 如 html, body {height: 100%;}; 或者对当前元素使用绝对定位, 如 div {position: absolute; height: 100%}

1. 文字的水平居中

    将一段文字置于容器的水平中点，只要设置text-align属性即可

1. 容器的水平居中

    先为该容器设置一个明确宽度，然后将margin的水平值设为auto即可, 如:

    ```css
    div#container {
    　　　　width:760px;
    　　　　margin:0 auto;
    　　 }
    ```

1. 文字的垂直居中

    单行文字的垂直居中，只要将行高与容器高设为相等即可, 如果有n行文字，那么将行高设为容器高度的n分之一即可;

    ```css
    div#container {height: 35px; line-height: 35px;}
    ```

1. 容器的垂直居中

    首先，将大容器的定位为relative, 然后，将小容器定位为absolute，再将它的左上角沿y轴下移50%，最后将它margin-top上移本身高度的50%即可

    ```css
    div#big{
    　　　　position:relative;
    　　　　height:480px;
    　　}
    div#small {
    　　　　position: absolute;
    　　　　top: 50%;
    　　　　height: 240px;
    　　　　margin-top: -120px;
    　　}
    ```

    或者:

    ```css
        html, body {
    height: 100%;
    margin: 0;
    }

    body {
    -webkit-align-items: center;  
    -ms-flex-align: center;  
    align-items: center;
    display: -webkit-flex;
    display: flex;
    }
    ```

## 文本字体类

1.  font-size基准, 浏览器的缺省字体大小是16px，你可以先将基准字体大小设为10px, 后面统一采用em作为字体单位，2.4em就表示24px。

1. 禁止自动换行 `h1 { white-space:nowrap; }`

## 图像图片

1. 图片宽度的自适应容器宽度 `img {max-width: 100%}  `

1. 将一个容器设为透明 (第一行是IE专用的，第二行用于Firefox，第三行用于webkit核心的浏览器，第四行用于Opera)

    ```css
    .element {
    　　　　filter:alpha(opacity=50);
    　　　　-moz-opacity:0.5;
    　　　　-khtml-opacity: 0.5;
    　　　　opacity: 0.5;
    　　}

    ```

1. 图像替换文字标题

    ```css
    .header{
        text-indent:-9999px;
        background:url('someimage.jpg') no-repeat;
        height: 100px; /*dimensions equal to image size*/
        width:500px;
    }
    ```

1. 黑白照片

    ```css
    img.desaturate {
        filter: grayscale(100%);
        -webkit-filter: grayscale(100%);
        -moz-filter: grayscale(100%);
        -ms-filter: grayscale(100%);
        -o-filter: grayscale(100%);
    }
    ```

## 其他

1. 移除 ie 文本域滚动条

    ```css
    textarea{
        overflow:auto;
    }
    ```

1. link状态的设置顺序: link的四种状态，需要按照下面的前后顺序进行设置

    ```css
    a:link
    a:visited
    a:hover
    a:active
    ```

# 引入css三种方式

css:cascading style sheets(层叠样式表)

属性/行间样式：通过任意标签的  style 属性指定

内部样式：通过 style 元素 指定

外部样式：通过 link 标签从外部引入

优先级：行间样式最大, 内部样式和外部样式优先级和位置有关，下面的覆盖上面的; Id选择器中的样式 > 类选择器 > 标签选择器 > 通用选择器 (选择器选择范围越小越精确, 其样式优先级越高)


```html
<!-- 第一种： 在style标签中编写css代码。   只能用于本页面中，复用性不强。 -->
    <style>
        a{
            color:#F00;
            text-decoration:none;
        }
    </style>   
     
<!-- 第二种： 可以引入外部的css文件。   -->
    <!-- 方式1： 使用link标签。rel表示relation（外部CSS和本页面的关系）   -->
    <link href="1.css" rel="stylesheet"/>
    
    <!-- 方式2：使用<style>引入
        特点：  import需要等网页加载完成后才会加载css样式文件
                link:浏览器加载文件同时加载css样式文件 -->
    <style type="text/css" >
        @import url("1.css");
    </style>

<!-- 第三种方式：直接在html标签使用style属性编写。    只能用于本标签中，复用性较差。 -->
    <a style="color:#0F0; text-decoration:none" href="#">新闻的标题1</a>
 
```


# 长度单位总结

[ref1](https://www.cnblogs.com/xiaohuochai/p/5485683.html)
[ref2](https://www.cnblogs.com/xiaohuochai/p/5485683.html#anchor1))
[ref3](https://medium.com/code-better/css-units-for-font-size-px-em-rem-79f7e592bb97)

- 绝对/相对: em、ex、ch、rem是字体相关的相对长度单位

- 有何区别

    - px 在缩放页面时无法调整那些使用它作为单位的字体、按钮等的大小；

    - em 的值并不是固定的，会继承父级元素的字体大小，代表倍数；

    - rem 的值并不是固定的，始终是基于根元素 `<html>` 的，也代表倍数。

- vw, vh, vm: 只适用于非定位元素的高度相关属性上([ref](https://www.cnblogs.com/zhu-shixin/p/6384396.html))

    - vw  相对于视口的宽度。视口被均分为100单位的vw(即浏览器可视区) 100vw = 可视区宽度

    - vh  相对于视口的高度。视口被均分为100单位的vh(即浏览器可视区) 100vh  = 可视区高度

    - vmin/vm 相对于视口的宽度或高度中较小的那个。其中最小的那个被均分为100单位的vmin（即vm）

    - 很重要的运用场景是: 手机适应屏幕尺寸时; 活动页，分享等单页面;

```html
<style>
.box{font-size: 20px;}
.in{
    /* 相对于父元素，所以2*2px=40px */
    font-size: 2em;
    /* 相对于本身元素，所以5*40px=200px */
    height: 5em;
    /* 10*40px=400px */
    width: 10em;
    background-color: lightblue;
}
</style>
<div class="box">
    <div class="in">测试文字</div>
</div>

<!-- ///////////////////////////////////// -->

<style>
/* 浏览器默认字体大小为16px，则2*16=32px，所以根元素字体大小为32px */
/*默认地，浏览器的字体大小font-size是16px，也就是1rem=16px。而如果将HTML的font-size设置为100px，方便后续计算，不设置为10px是因为chrome下最小字体大小为12px*/
html{font-size: 2rem;}
/* 2*32=64px */
.box{font-size: 2rem;}
.in{
    /* 1*32=32px */
    font-size: 1rem;
    /* 1*32=32px */
    border-left: 1rem solid black;
    /* 4*32=128px */
    height: 4rem;
    /* 6*32=192px */
    width: 6rem;
    background-color: lightblue;
}
</style>
<div class="box">
    <div class="in" id="test">测试文字</div>    
</div>

<!-- //////////////////////////////////// -->

1.手机适应屏幕尺寸时，如需订列表缩略图的宽高时，如（1:1）且可自适应，

.img-wrap{
width: 30vw;
height: 30vw;
}
2.活动页，分享等单页面

body{
height:100vh;
}
```

# responsive-web-design 响应式布局

https://www.zhihu.com/question/20732368
https://www.zhihu.com/question/20976405
http://www.cnblogs.com/Wayou/p/responsive_design_step_by_step.html
https://www.ibm.com/developerworks/cn/web/1507_wangqf_firstrppage/index.html
http://www.ruanyifeng.com/blog/2012/05/responsive_web_design.html

## 百分比宽度

```css
nav {
  float: left;
  /* 相对于parent来说 */
  width: 25%;
}
section {
  margin-left: 25%;
}
```

<img src="Snipaste_2018-05-23_23-37-04.png" >

## rem 布局

相对于根节点 HTML 元素 中的字号布局

## 视口单位布局

vm/vh


## 媒体查询 @media

不推荐

```css
/*
使用百分比宽度来布局，然后在浏览器变窄到无法容纳侧边栏中的菜单时，把布局显示成一列
*/
@media screen and (min-width:600px) {
  nav {
    float: left;
    width: 25%;
  }
  section {
    margin-left: 25%;
  }
}
@media screen and (max-width:599px) {
  nav li {
    display: inline;
  }
}
```

<img src="Snipaste_2018-05-23_23-40-45.png"><img src="Snipaste_2018-05-23_23-41-43.png" width="50%">


# 盒子模型

默认的盒子模型, 元素的边框和内边距会撑开元素


<img src="Snipaste_2018-05-21_18-40-13.png">

<img src="Snipaste_2018-05-23_22-32-01.png" width="30%">

## 消除padding和border影响

`box-sizing: border-box`

当你设置一个元素为 box-sizing: border-box; 时，此元素的内边距和边框不再会增加它的宽度

```css
.simple {
  width: 500px;
  margin: 20px auto;
  -webkit-box-sizing: border-box;/*  box-sizing 是个很新的属性，目前你还应该像我上面例子中那样使用 -webkit- 和 -moz- 前缀。这可以启用特定浏览器实验中的特性 */
     -moz-box-sizing: border-box;
          box-sizing: border-box;
}

.fancy {
  width: 500px;
  margin: 20px auto;
  padding: 50px;
  border: solid blue 10px;
  -webkit-box-sizing: border-box;
     -moz-box-sizing: border-box;
          box-sizing: border-box;
}

/**
经常这么做, 
*/
// 然后html元素内部的元素通过"box-sizing: inherit;"继承这个盒子类型
html {
  box-sizing: border-box;
}

```

<img src="Snipaste_2018-05-23_22-36-52.png" width="30%">




```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>盒模型</title>
    <style>
        /*内容宽高*/
        .one{ 
            width: 100px;
            height: 100px;
            background: red;
            /*padding填充*/
            /*padding-top: 50px;*/
            /*左填充*/
            /*padding-left: 50px;*/
            /*右填充*/
            /*padding-right: 50px;*/
            /*下填充*/
            /*padding-bottom: 50px;*/
            
            /*四个值必须一样*/
            /*padding: 50px;*/
            /*四个值不一样
            顺序：上 右 下 左
            */
            /*padding: 10px 20px 30px 40px;*/

            /*上下相同
            上下：20 
            左右：40
            */
            /*padding: 20px 40px;*/

            /*三个值 上 左右 下
            上：10
            左右：50
            下：30
             */
            padding: 10px 50px 30px;
            
            /*边框*/
            /*宽度*/
            border-width: 10px;
            border-left-width: 20px;
            /*边框颜色*/
            border-color: gray;
            border-left-color: yellow;
            /*边框样式
                solid:实线;
                dashed:虚线;
                dotted:点线;
            */
            border-style: dashed;

            /*简写*/
            border: 5px solid green;
            /*左边简写*/
            border-left: 3px dotted gray;
            border-right: 3px dotted gray;

            /*去除边框*/
            border-top: none;

            /*margin*/
            margin-left: 50px;
            margin-top: 50px;
            /*margin-bottom margin-bottom
            必须有参照物*/
            margin-bottom: 50px;
            /*右margin*/
            margin-right: 50px;

            /*简写*/
            /*margin:30px;*/
            /*上下 左右*/
            /*margin: 10px 30px;*/
            /*上 左右 下*/
            margin: 10px 30px 40px;
        }
        .two{
            height: 100px;
            width: 100px;
            background: blue;
        }
        span{
            background: red;
        }
        .span1{
            margin-right: 20px;
        }
    </style>
</head>
<body>
    <!-- 
    盒模型：html 元素都可以看成是一个盒模型
    div span i.........

    盒模型的组成：内容（width+height）+ padding(内边距) + border(边框) + margin(外边距)
     -->

     <!-- 
     盒模型大小 
        盒子宽度：左margin + 左border + 左padding + width + 右padding + 右border + 右margin
        盒子高度：上margin + 上border + 上padding + height + 下padding + 下border + 下margin
     -->
     <div class="one"></div>
     <div class="two">
        <span class="span1">1111</span>
        <span>2222</span>
     </div>
</body>
</html>

```

## margin重合

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>margin上下重合</title>
    <style>
        /*同级div 上下margin会有重合现象
        div1margin的下和div2的上margin重合
        重合后的值是设置的最大值
        */
        div{ width:100px; height:100px; background: red; }
        /* 哪个的大, 就以哪个的为准 */
        .div1{ margin-bottom: 10px; }
        .div2{ margin-top: 20px; }
    </style>
</head>
<body>
    <div class="div1">11</div>
    <div class="div2">22</div>
</body>
</html>
```

## 子margin-top传递给父亲问题

<img src="Snipaste_2018-05-22_23-54-22.png" width="30%"><img src="Snipaste_2018-05-22_23-54-55.png">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title></title>
    <style>
        .box{ 
            background: red; 
            height: 200px; 
            width: 200px; 
            /*padding-top: 20px;*/
            overflow: hidden;
            /* border:1px solid black; */
        }
        .inner{ 
            width: 100px; 
            height: 100px; 
            background: blue; 
            margin-left: 20px; 
            /*子元素的margin-top会传给父元素*/
            /*解决办法：
            1.用父元素的padding-top代替
            2.给父元素添加overflow: hidden;
            3.给父元素添加border(除了none以外)
            */
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="inner"></div>
    </div>
</body>
</html>
```

## auto的用法

<img src="Snipaste_2018-05-23_00-16-41.png" width="30%">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        .box{
            width: 400px;
            height: 100px;
            background-color: red;
        }
        .box div{
            height: 50px;
            margin-left: 50px;
            margin-right: 50px;
            background-color: blue;
            /*width默认大小为：元素宽度-margin-left - margin-right*/
            width: auto;
        }
        .box2{
            width: 400px;
            height: 100px;
            background-color: red;
        }
        .box2 div{
            width: 100px;
            height: 100px;
            -margin-left: 30px;/* 加"-", 则无效 */
            background-color: blue;
            /*margin-right: auto;
            margin-right: 元素宽度-margin-left-元素宽度
             */
            
            /*靠左边显示*/
            margin-right: auto;
            margin-left: 0;
            /* 如果希望靠右边, margin-right:0;margin-left:auto;即可 */
        }
        .center{
            width: 300px;
            height: 50px;
            background: blue;
            /* 上 左右 下 */
            margin: 10px auto 0;
        }


    </style>
</head>
<body>
    <div class="box">
        <div></div>
    </div>
    <div class="box2">
        <div></div>
    </div>

    <div class="center">
    </div>
</body>
</html>
```

## 页面自适应的盒子图

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>盒子图-从外到内+自适应</title>
    <style>
        body{ 
            background-color: #EEF4FB; 
            width: 100%;
            height: 100%;
        }
        .box{ 
            width: 50%;
            height: 100%;
            background-color: #fff; 
            margin: auto;
            overflow: hidden;
        }
        .box_1{ 
            width: 90%; /*405/450*/ 
            margin: 4.444%; /*20/450 */
            border: 1px dashed black; /*1/450*/            
        }
        .box_2{
            width: 83.95%;  /*340/405*/
            margin: 4.41%; /*30/405*/
            border: 5px solid #D8EFFF;
        }
        .box_3{
            width: 88.23%; /*300/340*/
            margin: 5.88%; /*20/340*/
            background-color: #FFA1DF;
            overflow: hidden;
        }
        .box_4{
            width: 72.33%; /*217/300*/
            margin: 13.33%; /*40/300 */
            border: 1px dotted white;
        }
        .box_5{
            width: 94.47%; /*205/217*/
            margin: 2.21%; /*5/217*/
            border: 1px dashed white;
        }
        .box_6{
            width: 48.78%; /*100/205*/
            
            margin: 24.39%; /*50/205*/
            border:5px solid yellow;
            background-color: #97FF38;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="box_1">
            <div class="box_2">
                <div class="box_3">
                    <div class="box_4">
                        <div class="box_5">
                            <div class="box_6">
                                &nbsp;
                            </div>                
                        </div>                                
                    </div>
                </div>            
            </div>
        </div>
    </div>
</body>
</html>
```



# 常用CSS属性

## CSS背景属性(Background)

```

background:在一个声明中设置所有的背景插件1

background-attachment:设置背景图像是否固定或者随着页面的其余部分滚动1

background-color:设置元素的背景颜色1

background-image:设置元素的背景图像1

background-position:设置背景图像的开始位置1

background-repeat:设置是否及如何重复背景图像
```

## CSS边框属性(BorderfflOutline)

```

border:在一个声明中设置所有的边框属性1

border-color:设置四条边框的颜色1

border-style:设置四条边框的样式1

border-width:设置四条边框的宽度1

***************************

border-bottom:在一个声明中设置所有的下边框属性1

border-bottom-color:设置下边框的颜色2

border-bottom-style:设置下边框的样式2

border-bottom-width:下边框的宽度1

*****************************

border-left:在一个声明中设置所有的左边框属性1

border-left-color:设置左边框的颜色2

border-left-style:设置左边框的样式2

border-left-width:设置左边框的宽度1

******************************

border-right:在一个声明中设置所有右边框的属性1

border-right-color:右边框的颜色2

border-right-style:设置右边框的样式2

border-right-width:设置右边框的宽度1

*******************************

border-top:在一个声明中设置所有上边框的属性1

border-top-color:上边框的颜色2

border-top-style:设置上边框的样式2

border-top-width:设置上边框的宽度1

**************************************

outline:在一个声明中设置所有的轮廓属性2

outline-color:设置轮廓的颜色2

outline-style:设置轮廓的样式2

outline-width:设置轮廓的宽度

```


## CSS文本属性 text

```
color:文本的颜色1

direction:规定文本的方向/书写方向2

letter-spacing:宇符间距1

line-height:设置行高1

    若 line-heigh === height, 文字垂直居中

text-align:规定文本的水平对齐方式1

    center 表示 水平居中

text-decoration:规定添加到文本的装饰效果1

    underline 下划线

text-indent:规定文本块首行的缩进1

text-shadow:规定添加到文本的阴影效果2

text-transform:控制文本的大小写1

unicode-bidi:设置文本方向2

white-space:规定如何处理元素中的空白1

word-spacing:设置单词间距1

```

## CSS字体属性 font

```

font:在一个声明中设置所有宇体属性1

font-family:规定文本的宇体系列1

font-size:规定文本的宇体尺寸1

font-size-adjust:为元素规定aspect值2

font-stretch:收缩或拉伸当前的宇体系列2

font-style:规定文本的宇体样式1

font-variant:规定文本的宇体样式1

font-weight:规定宇体的粗细

```

## CSS外边距属性(Margin)

margin:在一个声明中设置所有的外边距属性1(1个值: 上下左右; 2个值: 上下, 左右; 3个值: 上, 左右, 下; 4个值: 上, 右, 下, 左;)

margin-bottom:设置元素的下外边距1

margin-left:设置元素的左外边距1

margin-right:设置元素的右外边距1

margin-top:设置元素的上外边距

## CSS内边距属性(Padding)

padding:在一个声明中设置所有的内边距属性1

padding-bottom:设置元素的下内边距1

padding-left:设置元素的左内边距1

padding-right:设置元素的右内边距1

padding-top:设置元素的上内边距

## CSS列表属性(List)

list-style:在一声明中设置所有的列表属性1

list-style-image:将图像设置为列表项标记1

list-position:设置列表项标记的放置位置1

list-style-type:设置列表项标记的类型

## CSS尺寸属性(Dimension)

height:设置元素高度1

width:设置元素的宽度1

max-height:设置元素的最大高度2

max-width:设置元素的最大宽度2

min-height:设置元素的最小高度2

min-width:设置元素的最小宽度2

## CSS定位属性(Positioning)

bottom:设置位元素下外边界与其包含块下边界之
间的偏移2

clear:规定元素的哪一侧不允许其他浮动元素1

clip:剪裁绝对定位元素2

cursor:规定要显示的光标类型(形状)2

display:规定元素应该生成的框的类型1

float:规定框是否应该浮动1

left:设置定位元素左外边距边界与其包含块左边界之间的偏移2

overflow:规定当内容溢出元素框时发生的事情2

position:规定元素定位类型2

right:设置定位元素右外边距边界与其包含块右边之间的偏移2

top:设置定位元素上外边距边界与其包含块上边之间的偏移2

vertical-align:设置元素的垂直对齐方式1

visibility:元素是否可见2

z-index:设置元素的堆叠顺序

## CSS表格属性(Table)

border-collapse:规定是否合并表格边框2

border-spacing:规定相邻单元格边框之间的距离2

caption-side:规定表格标题的位置2

empty-cells:规定是否显示表格中的空单元格上的边框和背景2

table-layout:设置用于表格的布局算法


# 选择器

css优先级：!important > 行间样式 > ID选择器 > 属性选择器 == 伪类选择器 > 类选择器 > 标签/元素选择器

        优先级划分
        a:行内样式：优先级1000
        b:ID选择器：优先级100
        c:类选择器：优先级10 (伪类属性优先级10 div:first-child == 11)
        d:元素选择器：优先级1

        !important:优先级最大10000（用了后就不能修改 慎用）
        span 优先级1
        div span 优先级 1+1
        .divC span 优先级 10+1
        #divI span 优先级 100+1

## 元素选择器 类选择器 id选择器

选择器设定的样式优先级: id选择器 > 类选择器 > 标签/元素选择器

```html
<head>
<style type="text/css">

选择器： 选择器的作用就是找到对应标签的标签体内的数据进行样式化（添加属性）。以下选择器优先级一次提高

    1.标签选择器： 就是找到所有指定的标签进行样式化。
        格式：    
            标签名{
                样式1；样式2....    
            }
        例子：
            div{
                color:#F00;
                font-size:24px;
            }
            
    2. 类选择器: 使用类选择器首先要给html标签指定对应的class属性值。
                  只对class属性值相同的标签起作用；
        格式：
            .class的属性值{
                样式1；样式2...    
            }    
        例子：
            .two{
                background-color:#0F0;
                color:#F00;
                font-size:24px;
            }
            
        类选择器要注意的事项：
            1. html元素的class属性值一定不能以数字开头.
            2. 类选择器的样式是要优先于标签选择器的样式。
        
    3. ID选择器：     只对有ID属性的那一个标签起作用；
                使用ID选择器首先要给html元素添加一个id的属性值。
            
        ID选择器的格式：
            #id属性值{
                样式1；样式2...    
            }
        id选择器要注意的事项：
            1. ID选择器的样式优先级是最高的，优先于类选择器与标签选择器。
            2. ID的属性值也是不能以数字开头的。
            3. ID的属性值在一个html页面中只能出现一次。
            
    4. 交集/后代选择器： 就是对选择器1中的所有选择器2里面的数据进行样式化。
        格式：
            选择器1 选择器2{
                样式1，样式2....    
            }
        
        例子：
            .two span{
                background-color:#999;
                font-size:24px;
            }

    5. 并集选择器： 对指定的多种选择器进行统一的样式化。
        格式：    
            选择器1,选择器2，等等{
                样式1；样式2...
            }
        例子：    
            span,a{
                border-style:solid;
                border-color:#F00;
            }
    6. 相邻兄弟选择器: 选择 B , 且满足条件A的兄弟元素是B
        写法 :
            A+B
        例子:    
            #p1+p{color:purple;}

    7. 通用选择器:  对所有 的选择器有用
        格式：
            *{
                样式1；样式2...
            }    
    
        例子：
            *{
                text-decoration:line-through;
                background-color:#CCC;
            }
</style>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>无标题文档</title>
</head> 
<body>
    <div id="one" class="two">这个是<span>第一个div标签</span>...</div>
    <div id="one" class="two">这个是<span>第二个div标签</span>...</div>
    <span>这个是一个span标签</span><br/>
    <a class="two" href="#">新闻标题</a>
</body>
</html>

```

## 属性选择器

属性选择器优先级和伪类优先级相同，代码后面覆盖前面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>属性选择器</title>
    <style>
        /*属性选择器：
            语法 E[attribute]
            找到具有attribute属性的元素
            属性可以自定义
        */
        a[href]{
            color: red;
        }
        a[custom]{
            color:yellow;
        }
        /*E[attribute]
            找到属性值为attribute元素
         */
        a[href=https]{
            color:pink;
        }
        /*E[att~=value]
            适用于多个属性值，只要包含其中一个
         */
        p[col~=cl1]{
            color:red;
        }

        /*
            E[attr|=value]
            
            选择有指定开头的元素    , 和下一个类似
            value:必须是个完整的单词;
         */
        p[lang|=zh]{
            color: red;
        }

        /*E[att^=value]
            选择属性值是指定开头的元素
         */
        p[class^=aaa]{
            color:red;
        }

        /*E[attr$=value]
            匹配属性值以指定字符结尾的所有元素
         */
        p[class$=cd]{
            color:red;
        }

        /*E[attr*=value]
        匹配属性值包含指定字符的所有元素
         */
        p[class*=bb]{color:blue;}
    </style>
</head>
<body>
    <a href="###">W我是一个标签aaaaa</a>
    <a custom>我是另一个aaaaaa</a>
    <br>
    <a href="https">a2333333</a>

    <p col="cl1 cl2">ppppppp1p1p1pp1</p>

    <p lang="zh-cn">pppppppppppppppppp2</p>

    <p class="aaaabbb">ppppppppp333333333333333</p>
    <p class="aaaabb22bb">ppppppppp444444444444444444</p>

    <p class="aaaacccd">p5555555555555555555</p>
    <p class="bbbbbbbbcd">ppppppppppp66666666666666</p>

    <p class="aabbssddee">pppppppppppppppppp7777777777777</p>
</body>
</html>
```

## 伪类选择器

选中元素的某个状态

```html
<head>
<style type="text/css">
 伪类选择器：伪类选择器就是针对于元素处于某种状态下进行样式化。
 注意： 
     1. a:hover 必须被置于 a:link 和 a:visited 之后
    2. a:active 必须被置于 a:hover 之后
即：
以下四个相对位置不能改变：
    a:link{color:#F00} /* 没有被点击过---红色 */
    a:visited{color:#0F0} /*  已经被访问过的样式---绿色 */ 
    a:hover{color:#00F;} /* 鼠标经过的状态---蓝 */
    a:active{color:#FF0;}   /*鼠标选定按住不放*/
</style>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<title>无标题文档</title>
</head>
<body>
    <a href="#">百度</a>
</body>
</html>

```

## 条件选择器(first-child,focus,checked)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>first-child伪类</title>
    <style>
        /*
            CSS2  :first-child伪类;
            错误的理解：div下的第一个子元素
            ：前是确定元素,  ：后是加条件
         */
        div:first-child{ color:red; }
        p:first-child{ color: blue; }


        /*
        匹配所有<p>下的第一个<i>元素
         */
        p>i:first-child{ color:red; }
        /* 最后一个i元素 */
        p>i:last-child{ color:blue; }

        /*匹配所有作为第一个子元素<h3>元素中的所有<i>元素*/
        h3:first-child i{ color:red; }


        /*
            ：focus:选择具有焦点的元素
            :disabled 禁用;
            :enabled  启用；
            :checked  选中状态;
         */
        
        input:focus{ background-color:red; }
        input:disabled{ background-color:red; }
        input:enabled{ background-color:red; }
        input:checked+span{ /*width: 100px;*/ background-color:red; }

        /*:lang 向带有指定lang属性的元素添加样式;
            每个<p>元素lang属性等于"zh"
         */
        p:lang(zh){ background-color: red; font-size:20px; }
        p:lang(en){ background-color: blue; font-size:30px; }

    </style>
</head>
<body>
    <div>div11111111</div>
    <div>div22222222</div>
    <div>
        <div>
            <p>pppppppppp</p>
        </div>
        <p>pppppp2222222222</p>
    </div>
    <p>今天<i>天气</i>不太好，因为不<i>考试</i></p>
    <p>今天<i>天气</i>不太好，因为不<i>考试</i></p>

    <div>
        <h3>今天<i>天气</i>不太好，因为不<i>考试</i></h3>
        <h3>今天<i>天气</i>不太好，因为不<i>考试</i></h3>
    </div>

    <input type="text" name="user">
    <input type="text" name="in2" disabled="">
    男：<input type="radio" name="ra" checked="">
    女：<input type="radio" name="ra"><span>1111</span>
    
    <p lang="zh">中文</p>
    <p lang="en">英文</p>

</body>
</html>
```

## 伪元素选择器(first-letter,first-line,before,after)

选中某个元素的不同部分(":"和"::"的对比https://stackoverflow.com/questions/29754474/css-vs-pseudo-element-vs-pseudo-selector)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>伪元素</title>
    <style>
        /*
        伪元素：向特定的选择器添加样式
        ::或:
        :first-letter 向文本中的第一个字母添加样式
         */
        #p1::first-letter{
            font-size: 20px;
            color: red;
            background-color: blue;
            border:1px solid red;
        }
        /*
        ::first-line:
            向文本的首行添加特殊样式
         */
        #p2::first-line{ background-color: red; }
        /*混合添加样式*/
        #p2::first-letter{ 
            font-size: 20px;
            color: red;
            background-color: blue;
            border:1px solid red; 
        }

        /*
            :before:在元素内容前面添加新的内容;

         */
        #p3:before{ 
            /*content: "新的内容";*/
            content: url(img/1.jpg);
            /*width: 50px;
            height: 50px;
            display: block;*/
            }

            /*
                :after;元素后面添加内容
             */
            #p3:after{
                content: "after-------";
                background-color: red;
                /*
                    content:默认类型是行内元素;内容撑开宽高，宽高不允许修改，可以同行显示
                 */
                
                /*把行内标签修改成块标签

                display设置元素显示的方式, block(块状元素方式显示), inline(行内显示), none(不显示)
                给<a>标签用了 block 后，整个按钮都可以承载 a 的link操作了


                */
                display: block;
                width: 100px;
            }
    </style>
</head>
<body>
    <p id="p1">ppppppppppppppppp111111111111</p>
    <p id="p2">pppppppppppppppp22222222222222向文本的首行添加特殊样式向文本的首行添加特殊样式向文本的首行添加特殊样式向文本的首行添加特殊样式向文本的首行添加特殊样式向文本的首行添加特殊样式向文本的首行添加特殊样式向文本的首行添加特殊样式向文本的首行添加特殊样式向文本的首行添加特殊样式</p>
    <p id="p3">pppppppppppp333333333</p>
</body>
</html>
```

## css3新增选择器

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>css3属性选择器</title>
        <style>
            p[attr]{
                color: red;
            }
            p[attr="val1"]{
                color: blue;
            }
            /*多个属性*/
            p[attr~="val2"]{
                color: green;
            }
            /*css3新增*/
            /*^=:选择器以a开头属性*/
            p[property^="ab"]{
                color: red;
            }
            /*$=:选择以c结尾*/
            p[property$="c"]{
                color: blue;
            }
            /* *=：选择属性名包含b字符*/
            p[property*="b"]{
                color: darkgreen;
            }
        </style>
    </head>
    <body>
        <p attr="val1 val2">今天是个下雨天</p>
        <p property="abc">雨下一整天</p>
    </body>
</html>

```

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style>
            /*子元素选择器*/
            .div1>p{
                color: red;
            }
            
            /*相邻兄弟选择器(必须相邻)*/
            .div1+p{
                color: blue;
            }
            
            /*css3:后兄弟选择器（同级的当前元素后面元素）*/
            .div1~p{
                color: burlywood;
            }
        </style>
    </head>
    <body>
        <div class="div1">
            <p>子代</p>
            <div>
                <p>后代</p>
            </div>
        </div>
        <p>这是p3</p>
        <p>这是p4</p>
        <div>
            <p>这是p5</p>
        </div>
        <p>这是p6</p>
    </body>
</html>
```

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
        <style>
            /*css2*/
            .testP:after{
                content: "22";
                display: block;
                clear: both;
                overflow: hidden;
            }
            .testP:before{
                content: "before";
                /*display: inline-block;*/
                height: 100%;
                vertical-align: middle;
            }
            .p2::first-letter{
                color: red;
                font-size: 20px;
            }
            .p2:first-line{
                color: blue;
            }
            
            /*目标伪类选择器，p标签被跳转后起作用*/
            #top:target{
                background: red;
            }
            .p2{
                /*padding-top: 1000px;*/
            }
            
            /*选择禁用状态输入框*/
            input:disabled{
                background-color: red;
            }
            /*选择正常编辑的输入框，默认enabled*/
            input:enabled{
                /*background-color: blanchedalmond;*/
            }
            /*处于焦点状态*/
            input:focus{
                background-color: blue;
            }
            
            /*css3选中被选中radio*/
            input:checked:after{
                content: "被选中";
                display: inline-block;
                width: 100px;
                margin-left: 20px;;
            }
            /*数值在范围内*/
            input:in-range{
                /*background-color: red;*/
            }
            /*数值超出范围*/
            input: out-of-range{
                /*background-color: blue;*/
            }
            
            /*选中只读input*/
            input:read-only{
                background-color: red;
            }
            /*选中读写（默认）*/
            input:read-write{
                /*background-color: blue;*/
            }
            
            /*选择必须填写值，否则不能提交*/
            input:required{
                background: red;
            }
            /*默认，与required相对*/
            input:optional{
                /*background: blue;*/
            }
            
            /*合法*/
            input:valid{
                background-color: red;
            }
            span{
                display: none;
            }
            /*不合法*/
            input:invalid+span{
                background: blue;
                display: inline;
            }
            
            /*两个:: 双击选中状态*/
            div::selection{
                background-color: red;
                /*font-size: 30px;*/
                color: blue;
                /*height: 80px;*/
            }
            /*选择没有子元素并且没有内容*/
            div:empty{
                height: 50px;
                background-color: red;
            }
        </style>
    </head>
    <body>
        <p id="top">11</p>
        <p class="testP">ppp11111</p>
        <p class="p2">ppppp<br />ppp</p>
        <a href="#top">回到锚点</a>
        
        <form action="">
            正常：<input type="text" />
            不可以编辑：<input type="text" disabled />
            默认选中：<input type="text" autofocus="" />
            <hr />
            男：<input type="radio" name="11" checked="" />
            <br />
            女：<input type="radio" name="11" />
            <hr />
            10~100<input type="number" min="10" max="100" value="20" />
            <hr />
            只读：<input type="text" readonly />
            <br />
            读写：<input type="text" />
            <hr />
            必须有值：<input type="text" required="" />
            <input type="submit" />
            <hr />
            email: <input type="email" /><span>内容不合法</span>
        </form>
        <div>
            <p>22</p>
        </div>
        <div>111
            <p>222</p>
        </div>
        <div>222</div>        
        <div></div>
    </body>
</html>

```

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>结构选择器</title>
        <style>
            /*css2*/
            .div1 p:first-child{
                /*color: red;*/
            }
            
            /*css3*/
            .div1 p:last-child{
                color: red;
            }
            /*div下的p，（父元素下的所有p中的第一个p）
             * 对应，first-child：不会被其他非p元素的干扰
             * */
            .div2 p:first-of-type{
                /*color: red;*/
            }
            .div2 p:last-of-type{
                color: blue;
            }
            
            /*选中div下p，满足是父元素唯一的子元素*/
            .div3 p:only-child{
                /*color: red;*/
            }
            /*选中div下p，满足是父元素唯一的子元素
             * (所有的p，其他元素不会干扰，只要满足p只有一个就可以)*/
            .div3 p:only-of-type{
                color: blue;
            }
            
            /*选择div下p,满足是父元素下的第n个*/
            .div4 p:nth-child(2){
                /*color: red;*/
            }
            /*同上，从后选*/
            .div4 p:nth-last-child(1){
                color: red;
            }
            
            /*对比: :nth-child (其他非p元素不会干扰)*/
            .div5 p:nth-of-type(1){
                color: yellow;
            }
            /*同上*/
            .div5 p:nth-last-of-type(1){
                color: green;
            }
            
            /*odd:奇数
             * even：偶数
             * */
            .div6 div:nth-child(odd){
                /*color: red;*/
            }
            .div6 div:nth-child(even){
                /*color: blue;*/
            }
            /*只对子元素是div的元素比较，其他元素不会干扰*/
            .div6 div:nth-of-type(odd){
                /*color: red;*/
            }
            
            /*从后计算*/
            .div6 div:nth-last-child(odd){
                /*color: yellow;*/
            }
            .div6 div:nth-last-of-type(odd){
                /*color: blue;*/
            }
            
            /*n从0递增，+1...*/
            .div7 div:nth-child(n){
                color: red;
            }
            
            /*代表偶数*/
            .div7 div:nth-child(2n){
                color: blue;
            }
            .div7 div:nth-child(2n+1){
                color: yellow;
            }
            
            /*不选前3（num-1）个*/
            .div7 div:nth-child(n+3){
                color: red;
            }
            
            /*选择前num个*/
            .div7 div:nth-child(-n+4){
                color: saddlebrown;
            }
            
            /*最常用
             * 三列，选择每行的第二个元素
             * 3n+2
             * */
            
            
            
            ul{
                padding: 0;
                margin: 0;
                border: 1px solid red;
                width: 500px;
                overflow: hidden;
                padding-bottom: 100px;
            }
            li{
                float: left;
                list-style: none;
                width: 20%;
                height: 80%;
                color: blue;
            }
            ul li:nth-child(5n+2){
                background-color: green;
            }
        </style>
    </head>
    <body>
        <div class="div1">
            <a href="#">aa</a>
            <p>111</p>
            <p>222</p>
            <div>
                <p>3333</p>
            </div>
        </div>
        <div class="div2">
            <h4>h4444444</h4>
            <p>p11111111</p>
            <p>p22222222</p>
            <div>
                <a href="">111</a>
                <p>pppp2.5</p>
                <p>p333333</p>
            </div>
        </div>
        <hr />
        
        <div class="div3">
            <p>p11111111111</p>
            <a href="">aaaaaa</a>
            <div>
                <p>p222</p>
                <p>p333</p>
            </div>
        </div>
        <div class="div4">
            <h4>h444</h4>
            <a href="#">qqq</a>
            <p>ppppppppp111111111</p>
            <p>ppppppppp222222222</p>
            <div>
                <p>3333333</p>
                <p>ppp444444444</p>
            </div>
        </div>
        
        <hr />
        
        <div class="div5">
            <h3>h333333333</h3>
            <p>pp1111111111</p>
            <p>p22222222222</p>
            <div></div>
            <h6>6666</h6>
        </div>
        
        <hr />
        <div class="div6">
            <a href="">111</a>
            <div>1</div>
            <div>2</div>
            <div>3</div>
            <h5>555</h5>
            <div>4</div>
            <div>5</div>
            <div>6</div>
        </div>
        <hr />
        <div class="div7">
            <div>1</div>
            <div>2</div>
            <div>3</div>
            <div>4</div>
            <div>5</div>
            <div>6</div>
        </div>
        
        <ul>
            <li>1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
            <li>5</li>
            <li>6</li>
            <li>7</li>
            <li>8</li>
            <li>9</li>
            <li>10</li>
            <li>11</li>
            <li>12</li>
            <li>13</li>
            <li>14</li>
            <li>15</li>
            <li>16</li>
        </ul>
    </body>
</html>


```

# CSS-tips

## 选择器总结

http://www.ruanyifeng.com/blog/2009/03/css_selectors.html

### 星号选择器[*]

```css
/* 
许多开发者会使用这个技巧来把margin和padding都设为0。在快速开发测试中这种设置固然是好的，但我建议绝对不要在最终的产品代码中使用。因为会给浏览器增加大量不必要的负荷。
 */
* {
 margin: 0;
 padding: 0;
}

/**
这段代码会定义#container div所有子元素的样式。跟上面一样，如果可以尽量避免使用这个方法
*/
#container * {
 border: 1px solid black;
}
```

### id选择器[#xxx]

```css
/**
id选择器比较局限，不能重用。如果可以的话，先尝试使用标签名称，HTML5的其中一个新元素，或使用伪类。
*/
#container {
 width: 960px;
 margin: auto;
}
```

### 类选择器[.xxx]

* id和class类选择器的区别是，类选择器可以定义多个元素。当你想定义一组元素的样式时可以使用class选择器。

* 另外，可以使用id选择器来定义某一个特定的元素


### 后代选择器[x y][x>y]

```css
/**
当你需要更精确地定位时，可以使用后代选择器。例如，假如说你只想选择无序列表里的链接，而不是所有的链接？这种情况下你就应该使用后代选择器
*/
li a {
text-decoration: none;
}
```
区别:

x y 会选中所有后代(包括子代和孙子代)
x>y 只会选中直接后代, 不会选中孙子元素

### 元素选择器[x]

```css
/**
假如你想定义页面里所有type标签类型一样的元素，而不使用id或者class呢？可以简单地使用元素选择器。比如选择所有的无序列表，可以用ul {}
*/
a { color: red; }
ul { margin-left: 0; }
```

### 伪类选择器[a:link]

```css
/**
:link 还没点击
:visited 点击过了
X:hover
X:checked 选中的, 比如 radio, checkbox
X:after 伪类before和after属于高级用法。这两个伪类可以在元素前面和后面添加内容。清除浮动的技巧(当overflow: hidden不管用的时候用)
X:not(selector) 选中所有,除了xxx. 如div:not(#container) 选中所有div, 除了id为container的那个
p::first-line (确实有两个冒号) 选中第一行

*/
a:link { color: red; }
a:visited { color: purple; }

.clearfix:after {
  content: "";
  display: block;
  clear: both;
  visibility: hidden;
  font-size: 0;
  height: 0;
}
```

### 邻近元素选择器[x+y][x~y]

```css
/**
选择 p 元素, 且 ul 后面紧挨着 p元素 
*/
ul + p {// 只有每个ul后面的第一个段落是红色的。
 color: red;
}
```

x+y 只会选择一个 y 元素, 即 x 后面紧挨着唯一的一个 y 元素 
x~y 选择更广泛, 选择多个 y (如果有多个的话)

### 属性选择器(x[attrName])

选中具有 attrName 的元素, attrName 可以是自定义的

此外还有 :

x[attrName="attrValue"]
x[attrName*="keyword"]
x[attrName^="startValue"]
x[attrName$="endValue"]
x[attrName~="xxx"], x[attrName~="yyy"] 可以匹配带有空格的取值 ,如 `<x attrName="xxx yyy">`
E[att|=val] 匹配所有att属性具有多个连字号分隔（hyphen-separated）的值、其中一个值以"val"开头的E元素，主要用于lang属性，比如"en"、"en-us"、"en-gb"等等


## display

每个元素都有一个默认的 display , 取值:

* block 块级元素(一个块级元素会新开始一行并且尽可能撑满容器)

* inline 行内元素

    * 把 li 元素修改成 inline，制作成水平菜单。



* none 不删除元素的情况下隐藏元素

    * display:none 不会占据它本来应该显示的空间

    * visibility: hidden 还会占据空间

* inline-block 使得元素同时具有块元素和行内元素特性

    * 使用 inline-block 来布局。有一些事情需要你牢记

        * ertical-align 属性会影响到 inline-block 元素，你可能会把它的值设置为 top

        * 你需要设置每一列的宽度

        * 如果HTML源代码中元素之间有空格，那么列与列之间会产生空隙

    * 例子:用格子铺满页面(可以用float或inline-block实现)

    使用 float:left 实现

    ```css
    .box {
        float: left;
        width: 200px;
        height: 100px;
        margin: 1em;
    }
    .after-box {
        clear: left;
    }
    ```

    <img src="Snipaste_2018-05-23_23-47-11.png" width="50%">

    使用 display: inline-block 实现

    ```css
    .box2 {
        display: inline-block;
        width: 200px;
        height: 100px;
        margin: 1em;
    }
    ```

    <img src="Snipaste_2018-05-23_23-47-45.png" width="50%">


* flex 

## margin: 0 auto

水平居中

## max-width

使用 max-width 替代 width 可以使浏览器`更好地处理小窗口的情况`

如果坚持用width, 当浏览器窗口比元素的宽度还要窄时，浏览器会显示一个水平滚动条来容纳页面, 不美观。


## position

* static 默认, 不会被特殊的定位

* relative 相对于自身正常位置定位

    * 表现的和 static 一样，除非你添加了一些额外的属性

    * 设置 top 、 right 、 bottom 和 left 属性会使其偏离其正常位置。其他的元素的位置则不会受该元素的影响发生位置改变来弥补它偏离后剩下的空隙。

    ```css
    .relative1 {
        position: relative;
    }
    .relative2 {
        position: relative;
        top: -20px;
        left: 20px;
        background-color: white;
        width: 500px;
    }
    ```

    <img src="Snipaste_2018-05-23_22-43-57.png" >

    
* fixed 相对于视窗定位, 也就是即便页面滚动，它还是会停留在相同的位置。

    * top 、 right 、 bottom 和 left 属性都可用。

    * 一个固定定位元素不会保留它原本在页面应有的空隙（脱离文档流）


* absolute 相对于最近的“positioned”祖先元素(即除了默认的static定位类型之外的元素)定位

    * 如果没有“positioned”祖先元素，那么它是相对于文档的 body 元素定位
    
    * 并且它会随着页面滚动而移动

## float

脱离文档流 浮动

Float 可用于实现文字环绕图片

### clear:left|right|both

clear 属性被用于控制浮动(用 clear 我们就可以将这个段落移动到浮动元素 div 下面)

### 浮动元素溢出

```css
img {
  float: right;
}
```

上面的代码有问题:

<img src="Snipaste_2018-05-23_23-17-44.png" width="50%">

加入一条新样式就好多了

```css
img {
  float: right;
  overflow: auto;
}
```

<img src="Snipaste_2018-05-23_23-19-45.png" width="50%">



## column,文字多列布局

```css
.three-column {
  padding: 1em;
  /* SS columns是很新的标准，所以你需要使用前缀 */
  -moz-column-count: 3;
  -moz-column-gap: 1em;
  -webkit-column-count: 3;
  -webkit-column-gap: 1em;
  column-count: 3;
  column-gap: 1em;
}
```

<img src="Snipaste_2018-05-23_23-54-50.png" width="50%  ">

## 创建小圆形

```html
<div class="circle"></div>
```

```css
.circle {
  border-radius: 50%;/* 最小为50%, 再小就不是圆了 */
  width: 2rem;/* 宽高必须等同 */
  height: 2rem;
  background: #333;
}
```

## clearfix(清除浮动)

<img src="Snipaste_2018-05-26_17-20-36.png">

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title></title>
    <style>
        .container {
            width: 500px;
            background-color: black;
        }

        .left {
            width: 200px;
            height: 300px;
            background-color: red;
            float: left;
        }

        .right {
            width: 200px;
            height: 200px;
            background-color: green;
            float: right;
        }

        .footer {
            width: 400px;
            height: 50px;
            background-color: blue;
            /* clear: left; */
        }

        .clearfix::after {
            content: "";
            clear: left;
            display: block;
        }
    </style>
</head>

<body>
    <!-- black -->
    <div class="container clearfix">
        <!-- red -->
        <div class="left">left</div>
        <!-- green -->
        <div class="right">right</div>
    </div>
    <!-- blue -->
    <div class="footer">footer</div>
</body>

</html>
```

## 响应式方块

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title></title>
    <style>
        .constant-width-to-height-ratio {
            background: #333;
            width: 50%;
        }

        .constant-width-to-height-ratio::before {
            content: '';
            padding-top: 100%;/* 方块的高度==宽度的100% */
            float: left;
        }

        .constant-width-to-height-ratio::after {
            content: '';
            display: block;
            clear: both;
        }
    </style>
</head>

<body>
    <div class="constant-width-to-height-ratio"></div>
</body>

</html>
```

## 给无序列表添加数字

<img src="Snipaste_2018-05-26_17-37-46.png" width="50%">

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title></title>
    <style>
        ul {
            counter-reset: counter;/* 初始化一个计数器，该值是计数器的名称, 默认0开始 */
        }

        li::before {
            counter-increment: counter;/* 在可数的元素中使用 */
            content: counters(counter, '.') ' ';
        }
    </style>
</head>

<body>
    <ul>
        <li>List item</li>
        <li>List item</li>
        <li>List item
            <ul>
                <li>xxx</li>
                <li>yyy</li>
                <li>zzz</li>
            </ul>
        </li>
    </ul>
</body>

</html>
```

## 自定义文本选择颜色

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title></title>
    <style>
        ::selection {
            background: aquamarine;
            color: black;
        }
        
        // 这个也可以
        /* .custom-text-selection::selection {
            background: deeppink;
            color: white;
        } */
    </style>
</head>

<body>
    <p class="custom-text-selection">Select some of this text.</p>
</body>

</html>
```

## 在css中使用自定义变量

```css
/**
变量是在:rootCSS伪类中全局定义的，它与表示文档的树的根元素相匹配
*/
:root {
  --some-color: #da7800;
  --some-keyword: italic;
  --some-size: 1.25em;
  --some-complex-value: 1px 1px 2px whitesmoke, 0 0 1em slategray, 0 0 0.2em slategray;
}
.custom-variables {
  color: var(--some-color);
  font-size: var(--some-size);
  font-style: var(--some-keyword);
  text-shadow: var(--some-complex-value);
}
```

## 禁用文本选择

```css
.unselectable {
  user-select: none;
}
```


# 字体

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>字体</title>
    <style>
        /*字体家族:宋体，楷体，微软雅黑，黑体，仿宋。。;必须电脑有字体
            
         */
        p{
            font-family: "黑体";
            /*样式：
            font-style:
            normal:常规;
            italic:斜体;
            oblique:斜体;
            区别：
            italic:字体的斜体样式;
            oblique:强行倾斜;

             */
            font-style: normal;
            font-style: italic;
            font-style: oblique;
            /*font-size: 50px;*/
            /*大写的形式+小写的高度*/
            font-variant: small-caps;
            
            /*加粗*/
            font-weight: bold;
            /*更粗*/
            font-weight: bolder;
            /*正常*/
            font-weight: normal;
            /*加粗的数字的表现形式 100-900
            400==normal
            700==bold
            
            */
            font-weight: 900;
            /*简写，字体必须给*/
            font: italic small-caps bold 50px "宋体";
        }
    </style>
</head>
<body>
    <p>今天电脑很卡aaaaaaaaaaaabbbbccc</p>
    
</body>
</html>
```

# 引入媒体元素

## 背景图片

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>背景</title>
    <style>
        div{
            width:1000px;
            /* 若采用 百分比, 表示相对于父容器的百分比, 需要把 父容器也设置为 100% 才能撑开 */
            height:1000px;
            /*背景色*/
            background-color: red;
            /*背景图
            背景图片默认平铺
            */
            background-image: url( img/1.jpg);
            /*
                no-repeat:不平铺;
                repeat:平铺;（默认）
                repeat-x:水平平铺;
                repeat-y:竖向平铺;
             */
            background-repeat: no-repeat;

            /*背景是否滚动
            scroll:跟着页面滚动;（默认）
            fixed:固定;
            */
            /*background-attachment: fixed;*/

            /*有x y两项
                x:left center right;(水平方向)
                y:top center bottom;(垂直方向)

                可以只写一个：
                right == right center
                center == center center
             */
            /*background-position: center center;*/
            background-position: top;
            
            /*
            图片左上角到浏览器左上角的距离
             */
            background-position: -50px  -200px;
            /*
            0% 0% 元素左上角
            100% 100% 元素右下角
            可以为负数
             */
            background-position: 50% 50%;
            /*简写
            1.颜色
            2.路径
            3.平铺
            4.固定
            5.位置
            */
            background: red url(img/1.jpg) no-repeat fixed left center;

            /*background: url(img/html.jpg)*/
            /*background-color: yellow;*/
        }
    </style>
</head>
<body>
    
    <div>
        11111111 <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        <br>
        111111
    </div>

</body>
</html>
```

# 隐藏

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>隐藏效果</title>
    <style>
        .div1{ 
            width: 100px; 
            height: 100px; 
            background: red;
            /*隐藏，*/
            /* 
            visibility: hidden;保留原来位置
            display: none;不保留原来位置，下面div占据了他的位置
            */
            /*visibility: hidden;*/
            display: none;
        }
        

        .div2{
            width: 100px;
            height: 100px;
            background: blue;
        }
    </style>
</head>
<body>
    <div class="div1"></div>
    <div class="div2"></div>
</body>
</html>
```

## 伪元素解决浮动造成父元素高度为0的问题

如果父盒子中有一个子盒子，父盒子没有设置高度，子盒子浮动，那么父盒子不能被子盒子撑开，即父盒子高度为0，这时下面的盒子会占位

可一用伪类解决

```html
<style>
    .clearfix:after，.clearfix:before {
        content:"";
        height:0;
        line-height:0;
        display:block;
        clear:both;
        visibility:hidden;
    }
    .clearfix {
        zoom:1; /* 兼容ie6*/
    }
</style>

<div class="father clearfix">
    <div class="son"></div>
</div>
```


# outline(轮廓)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>轮廓</title>
    <style>
    /*
    outline(轮廓)
     */
        div{
            width: 100px;
            height: 100px;
            background: red;
            /*外框颜色*/
            outline-color: green;
            /*外框宽度*/
            outline-width: 10px;
            /*外框样式*/
            /*
                solid:实线;
                dashed:虚线;
                dotted:点;
                double:双边框;
                outset:3d凸;
                inset:3d凹;
                ridge:3d凸槽;
             */
            outline-style: ridge;
            /*简写*/
            outline: 10px solid blue;
        }
    </style>
</head>
<body>
    
    <div></div>
</body>
</html>
```

# 修改列表的样式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>列表样式</title>
    <style>
        li{
            /*
            none：取消样式；(最常用)
            circle:空心圆;
            disc:实心圆;
            square:方块;
            decimal:数字;
             */
            list-style-type: decimal;
            /*图片样式*/
            list-style-image: url(img/11.jpg);/* 每一行前的圆点变为图片 */
            /*
            样式位置：
            outside:默认(li的外部);
            inside:；li的内部;

             */
            list-style-position: inside;
        }
    </style>
</head>
<body>
    <ul>
        <li>西瓜</li>
        <li>苹果</li>
        <li>橘子</li>
    </ul>
</body>
</html>
```

# 文本设置

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>文本设置</title>
    <style>
        /*
            1.文本颜色
            {
                1.英文单词：17种标准色
                aqua, black, blue, fuchsia, gray, green, lime, maroon, navy, olive, orange, purple, red, silver, teal, white, yellow
                浅绿色，黑色，蓝色，紫红色，灰色，绿色，石灰，栗色，海军，橄榄，橙，紫，红，银，蓝绿色，白色和黄色

                2.rgb()取值范围0~255，也可以用百分比rgb(0,0,255) == rgb(0%,0%,100%)
                组合:(256*256*256)种颜色

                3.rgba() a:透明的alpha;范围0~1，0：透明 1：不透明；

                4.用16进制数字表示
                #RRGGBB（红绿蓝），值在0~ff之间
                #0000ff: 蓝色;
                #aabbcc==#abc
                #ffffff==#fff
            }
                2.
                text-decoration:
                underline:下划线;
                overline: 上划线;
                line-through: 删除线;
                none: 什么也没了;
                text-decoration-color firefox支持

                3.对齐方式
                text-align:left right center
                justify: 两端对齐，多行显示，最后一行没有效果;
                text-align-last: 对齐文本最后一行;
                
                4.text-indent:文本缩进;
                text-indent: 20px;
                text-indent: 2em; 根据文本大小缩进两个字符

                5.文本方向：
                direction:
                ltr: left to right;

                6.文字转换
                text-transform: uppercase;大写
                text-transform: lowercase;小写
                text-transform: capitalize;首字母大写

                7.字母间距
                letter-spacing:正数负数都可以;
                word-spacing:单词间距;

                8.
                white-space: nowrap;文本不换行
                white-space: pre;原文解析类似<pre>
                white-space: pre-wrap;空白字符会保留（和pre区别：pre保留原始样式，再长也不换行，pre-wrap:超出换行）
                white-space: pre-line;
                空白字符被合并，但是保留换行；

                9.line-height:上一行下边距到本行下边距;
                文本居中：height == line-height;

         */
        p{
            /*background-color: blue;*/
            /*color: blue;*/
            /*color: rgb(0,250,135);*/
            /*color: rgb(100%,0%,0%);*/
            /*color: rgba( 255,0,0,0.5 );*/
            color: #0f0;
            text-decoration: underline;
            text-decoration: overline;
            text-decoration: line-through;
            /*text-decoration: none;*/
            text-decoration-color: red;
            /*text-align: right;*/
            text-align: justify;
            /*text-align-last: right;*/
            /*font-size: 10px;*/
            /*text-indent: 20px;*/
            text-indent: 2em;
            /*direction: rtl;*/
            text-transform: uppercase;
            text-transform: lowercase;
            text-transform: capitalize;

            /*text-transform: 10px;  */  /*？？？？？？？？*/
            /*letter-spacing: 50px;*/
            height: 20px;  /*????????*/

            white-space: nowrap;
            white-space: pre-wrap;
            white-space: pre;
            white-space: pre-line;

            /*line-height:50px;*/
        }
        .p2{
            background-color: red;
            height: 100px;
            line-height:100px;
        }
    </style>
</head>
<body>
    <p>
        he---------取值范围0~255取值范围
        0~255取值范围
        0~255取值范围0~255取值范围0~255取值范围0~255取值范
        围0~255取值范围0~255取值范围0~255取值范围0~255he---------取值范围0~255取值范围0~255取值范围0~255取值范围0~255取33333333333333333333333333333333333333333333333333333
        值范围0~255取值范围0~255取值范围0~255取值范围0~255取值范围0~255取值范围0~255he---------取值范围0~255取值范围0~255取值范围0~255取值范围0~255取值
        范围0~255取值范围0~255取值范围0~255取值范围0~255取值范围0~255取值范围0~255
        dsdfdfdffafaSSDSDSDSDD
    </p>
    <p class="p2">你好啊？</p>
</body>
</html>
```

# 垂直居中

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>垂直居中</title>
    <style>
        /*vertical-align:
            middle:垂直居中;
            sub:文本下沉为下标;
            supper:文本上浮为上标;
            text-top:把元素的顶部和父元素字体的顶部对齐;
            text-bottom(默认):把元素的底部和父元素的文本的底部对齐;

            top:把元素的顶部和行中最高元素的顶部对齐;
            bottom:把元素的底部和行中最底元素的底部对齐;
         */
        div{ width:300px; height: 100px; background-color: red; }
        #div1{ 
            text-align: center;
            /*行高==元素高度：可以实现 单行文本或元素垂直居中*/
            line-height: 100px;
        }
/*        #p1{
            width: 300px;
            height: 100px;
            background-color: green;
            /*如果有多行文本  无法垂直居中*/
            /*line-height: 100px;
        }
*/
        #p1{
            width: 300px;
            height: 100px;
            background-color: green;
            /*vertical-align:middle 只适用于table标签*/
            /* 这里没用 */
            vertical-align: middle;
        }

        /*兼容性最好！！！*/
        table{
            width: 300px;
            height: 150px;
            background-color: green;
            /*vertical-align:middle;只适用于table标签，垂直方向居中*/
            vertical-align: middle;
        }
        
        #div2{
            /*dispaly: none; 使div不显示*/
            /*display: none;*/
            /* display: table; 使得内容垂直居中 */
            display: table;
        }

        #div2 p{
            /*p转化为cell（单元格td）*/
            /* 转化为td后, 可以使用vertical-align了 */
            display: table-cell;
            vertical-align:  middle;
        }

        #p2 span{
            /* 文本下沉 */
            vertical-align: sub;
        }
        #p2 img{
            /* 图片的顶部和父元素的文本顶部对齐 */
            /* vertical-align: text-top; */
        }
        #p2 i{
            /* 元素的顶部对齐于行最高元素顶部 */
            vertical-align: top;
        }
        /*
        子元素垂直居中
        1.1 line-height==元素高
        vertical-align:middle;
         */
        #div3{/* div子元素垂直居中 */
            line-height: 100px;
        }
        #div3 img{/* img元素垂直居中 */
            vertical-align: middle;
        }
        #div3 em{
            font-size: 30px;
            vertical-align: middle;
        }

        /*
        子元素垂直居中
        1.2
         */
        #div4 span,#div4 img,#div4 em{
            vertical-align: middle;
        }
        #div4 span{
            font-size: 20px;
        }
        #div4 em{
            font-size: 25px;
        }
        /*
        前提: 高度 == 父元素高度
        img: 既可以同行显示，也可以设置宽高;
        作用： 作为一个参考线

         */
        #base_line{
            height: 100px;
        }

        /*子元素垂直居中  1.5*/
        #div5 span,#div5 img,#div5 em{
            vertical-align: middle;
        }
        #div5 span{
            font-size: 20px;
        }
        #div5 em{
            font-size: 25px;
        }
        /*
            高度== 父元素高度；
            img:既可以同行显示，也可以设置宽高;
            作用：作为一个参考线

            解决火狐下用img的bug
        */
        #base_line_div{
            /*
                inline-block:把一个块元素或者行元素修改为一个既是行也是块;
            */
            height: 100%;
            width: 0px;
            background: blue;
            display: inline-block;
            vertical-align: middle;
        }
    </style>
</head>
<body>
    <div id="div1">你好啊</div>
    <p id="p1">
        你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊
    </p>

    <hr>

    <table>
        <tr>
            <td>你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊</td>
        </tr>
    </table>

    <hr>

    <div id="div2">
        <p>
            你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊~~~你好啊
        </p>
    </div>
    <p id="p2">
        明天考<img src="img/2.jpg" alt="方块">计算机<span>网络~！！！！！！！！！！！！！！</span>
        <i>考试~~~~~~</i>
    </p>

    <div id="div3">你好啊~~~<span>你好啊！！！</span><img src="img/2.jpg" alt="方块"><em>恩，不错0,0</em></div>
    <br>
    <div id="div4"><img id="base_line">
        你好啊~~~<span>你好啊！！！</span><img src="img/2.jpg" alt="方块"><em>恩，不错0,0</em>
    </div>
    <div id="div5">
        <div id="base_line_div"></div> 
        你好啊~~~<span>你好啊！！！</span><img src="img/2.jpg" alt="方块"><em>恩，不错0,0</em>
    </div>
</body>
</html>
```

# 修复图片间隙问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>图片间隙问题</title>
    <style>
        div{
            background-color: red;
        }
        /*解决bug*/
        /* 如果不加这段, img底部和div接触的面会有间隙 */
        img{
            vertical-align: top;
            width: 100px;
        }
    </style>
</head>
<body>
    <div>
        <img src="img/1.jpg" alt="">
    </div>
</body>
</html>
```

# 修复元素间隙问题

<img src="Snipaste_2018-05-23_14-58-06.png" width="30%"><img src="Snipaste_2018-05-23_14-55-49.png" width="30%">

使用 浮动

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>浮动</title>
    <style>
        /* div{
            background-color: red;
            width: 100px;
            height: 100px;
            display: inline-block;
        }
        
        span{
            background-color: blue;
            height: 100px;
            width: 100px;
            display: inline-block;
        } */
    /*inline-block
            使得内嵌元素支持宽高设置
            块元素可以同排显示
            默认内容撑开宽高

            不足：
            换行被解析
            ie 6 7 不支持
        */
    

        /*浮动*/
        div{
            background-color: red;
            width: 100px;
            height: 100px;
            float: left;
        }
        
        span{
            background-color: blue;
            height: 100px;
            width: 100px;
            float: left;
        }
        /*浮动
            使用支持宽高设置
            块元素可以同排显示
            默认内容撑开宽高
            换行不被解析
        **    元素间没有间隙
         */
    </style>
</head>
<body>
    <div>div1</div>
    <div>div2</div>

    <span>span1</span>
    <span>span2</span>
</body>
</html>
```

# 块元素, 内联元素, inline-block 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>inline-block</title>
    <style>
        div{
            width: 500px;
            height: 100px;
            background-color: red;
        }
        /*块元素修改为内联元，同排显示。。。=宽高无效*/
        #div1{
            /*行内 内联*/
            display: inline;
        }
        #div2{
            background-color: blue;
            display: inline;
        }
        a{
            background-color: gray;
            text-decoration: none;
        }
        #lo{
            /*内联元素转换为块元素*/
            display: block;
            width: 100px;
        }
        .div5{
            display: inline-block;
            width: 50px;
        }
        .div6{
            display: inline-block;
            width: 50px;
        }
    </style>
</head>
<body>
<!-- 块元素：
    独占一行
    支持宽高设置
    高度自适应
    支持所有css样式

 -->
    <div id="div1">1111</div>
    <div id="div2">2222</div>
    <!-- 
    内联
        不支持宽高
        同排显示
        换行被解析（1像素）
     -->
    <div>
        <a href="#">百度</a>
        <a href="#" id="lo">蓝欧科技</a>
        <a href="#">中北大学</a>
    </div>
    <!--inline-block 
        1.让块元素同排显示
        2.内联支持宽高
        3.默认内容撑开宽高
        4.换行被解析
        5.ie6 7不支持inline-block属性
     -->
    <div class="div5">55</div>
    <div class="div6">66</div>
</body>
</html>
```

# table样式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>表格样式</title>
    <style>
        table{
            /*合并单元格框*/
            border-collapse:collapse;
            /*td（边框）距离*/
            /* border-spacing: 10px; */
            /*标题位置*/
            caption-side: bottom;
            width: 400px;
            height: 200px;
            /*隐藏空cell*/
            /*empty-cells: hide;*/

            /*平分单元格
                table-layout: 
                fixed:内容过长是不拉伸，平分单元格;
                auto：内容过长拉伸表格；
            */
            table-layout: fixed;
        }
        table th,table td{
            /*border: 1px solid black;*/
        }
        tr{
            /* dotted 小点点 */
            border: 1px dotted red;
            text-align: center;
        }
        #shiTou{
            /*border: 2px solid blue;*/
            border-top: 2px solid blue;
            border-bottom: 2px solid blue;
            height: 40px;
        }
        caption{
            border:1px solid red;
            height: 80px;
            text-align: left;
        }
        #col2{
            /*border: 1px dashed gray;*/
            border-left: 1px dashed gray;
            /* dashed 虚线 */
            border-right: 2px dashed gray;
            width: 30px;
        }
    </style>
</head>
<body>
    <table -border="">
        <caption>表格信息</caption>
        <colgroup>
            <col span="1">
            <col id="col2">
            <col>
        </colgroup>
        <tr>
            <th>姓名</th>
            <th>性别</th>
            <th>成绩</th>
        </tr>
        <tr>
            <td>小敏</td>
            <td>男</td>
            <td></td>
        </tr>
        <tr id="shiTou">
            <td>石头</td>
            <td>男</td>
            <td>据介绍德雷克斯勒看到你说的道理开始sah hfisafk hfhfdssshf dhkhk hkh</td>
        </tr>
        <tr>
            <td>小李</td>
            <td>女</td>
            <td>50</td>
        </tr>
    </table>
</body>
</html>
```

# 响应式布局

## media type

```css
/* 
媒体类型(media type): screen, handheld, all, print...
    通过media type我们可以对不同的设备指定特定的样式，从而实现更丰富的界面

使用方法:

    <link href="style.css" media="screen print" ...
    或者
    @media screen{
        selector{rules}
    }

媒体查询media query: 是CSS 3对media type的增强，事实上我们可以将media query看成是media type+css属性判断

使用:

    <link rel="stylesheet" media="screen and (animation)” herf=“…”
or
    如果设备的浏览器的最大宽度是240px，则页面将使用中号字体
    @media screen and (max-width:240px){
    body{font-size:medium;}
}

更多的例子:

@media screen and (min-width:1024px) and (max-width:1280px){
    body{font-size:medium;}
}

@media handheld and (min-width:360px),screen and (min-width:480px){
body{font-size:large;}
}

检测apple中的safari
<link media="only screen and (max-device-width: 480px)" href="style.css">

android手机的多分辨率问题:
 for 240 px width screen 
@media only screen and (max-device-width:240px){
    selector{
    }
}
 
 for 360px width screen 
@media only screen and (min-device-width:241px) and (max-device-width:360px){
    selector{
    }
}
 
/for 480 px width screen 
@media only screen (min-device-width:361px)and (max-device-width:480px){
    selector{
    }
}

*/
```

看一个例子

<img src="Snipaste_2018-05-23_17-52-36.png" width="50%"><img src="Snipaste_2018-05-23_17-53-02.png" width="50%">

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>media</title>
        <style>
            *{ padding: 0; margin: 0; }
            li{ list-style: none; }
            p{ padding: 3% 0; text-indent: 2em }
            .box{ width: 80%; margin: 30px auto; border: 1px solid red; }
            .box:after{ content: ""; height: 0; clear: both; visibility: hidden; display: block; }
            nav{ border: 1px solid blue; float: left; text-align: center; }
            nav a{ color: black; text-decoration: none; }
            nav ul li{ padding: 1% 2%; }
            article{ float: left; width: 79%; border-left: 1px solid black; }
            article p:first-child{ border-bottom: 1px dashed blue; }
            @media only screen and (min-width: 520px) {
                nav{ width: 20%; }
                article{ width: 75%; }
            }
            @media only screen and (max-width: 520px) {
                nav{ width: 100%; }
                li{ float: left; }
                article{ width: 100%; }
            }
        </style>
    </head>
    <body>
        <div class="box">
            <nav>
                <ul>
                    <li><a href="#">导航一</a></li>
                    <li><a href="#">导航二</a></li>
                    <li><a href="#">导航三</a></li>
                    <li><a href="#">导航四</a></li>
                </ul>
            </nav>
            <article>
                <p>这是一个响应式布局的小列子，通过拖放浏览器窗口大小来观察实际大小</p>
                <p>
                    这是一段酱油文字。。。。。。。
                    这是一段酱油文字。。。。。。。
                    这是一段酱油文字。。。。。。。
                    这是一段酱油文字。。。。。。。
                    这是一段酱油文字。。。。。。。
                    这是一段酱油文字。。。。。。。
                    这是一段酱油文字。。。。。。。
                    这是一段酱油文字。。。。。。。
                    这是一段酱油文字。。。。。。。
                </p>
            </article>
        </div>
    </body>
</html>

```

## media query 媒体查询

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>响应式布局</title>
        <!--
            媒体查询：通过不同的媒介类型，和不同的条件定义css样式
        -->
        <link rel="stylesheet" href="css/a.css" />
        <!--当桌面小于700px的时候的样式：
            and后面空格不能省略
        -->
        <link rel="stylesheet" href="css/b.css" 
            media="only screen and (max-width:700px)" />
        <!--宽度小于500样式-->
        <link rel="stylesheet" href="css/c.css" 
            media="screen and (max-width:500px)" />
        <!--大于800px的样式-->
        <link rel="stylesheet" href="css/c.css" 
            media="screen and (min-width:800px)" />
    </head>
    <body>
        <!--
            媒体查询语法：
            @media only|not 设备类型（screen） and (条件) and (条件)...{
            css样式                
            }
            
            only screen: 意思仅仅支持电脑，手机，pad
            其他设备（打印机，阅读器...）（备注：only可以省略）
            not print: 除了打印机
            all: 所有设备
            
            条件：
            比如：max-width: 500px; 小于500满足
            min-width: 500px; 大于500满足
        -->
    </body>
</html>

```

## viewport虚拟窗口

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>viewport</title>
        <!--
        设置宽度一定，控件（标签）保持宽度不变，
        不会随设备分辨率改变而改变
        -->
        <!--<meta name="viewport" 
                content="width=device-width, 
                initial-scale=1.0, 
                maximum-scale=1.0, 
                user-scalable=0">-->
        <!--
            viewport:手机浏览器页面窗口，他是一个虚拟窗口
            width:device-width,设置viewport宽度，
            不同分辨率的手机宽度不同，
            保证在任何机型上看到的尺寸都是我们设置的尺寸
            initial-scale:初始缩放比例
            maximum-scale：允许缩放的最大比例
            user-scalable：是否允许用户缩放1（yes）0(no)
            设置0（no）：maximum-scale无效
        -->
        <!--<meta name="viewport" 
                content="width=320, 
                initial-scale=1.0, 
                maximum-scale=2.0, 
                user-scalable=1">-->
        <!--<meta name="viewport" 
                content="width=320, 
                initial-scale=0.5">-->
                
        <!--标准写法：-->
        <meta name="viewport" 
                content="width=device-width, 
                initial-scale=1.0, 
                maximum-scale=1.0, 
                user-scalable=0">
                
        <style>
            div{
                width: 150px;
                height: 150px;
                background-color: red;
            }
        </style>
    </head>
    <body>
        <form action="">
            <input type="text" name="li" placeholder="输入内容" />
        </form>
        <div></div>
    </body>
</html>

```



# 超出隐藏or滚动,css雪碧图

超出隐藏or滚动

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>overflow</title>
    <style>
        div{
            height: 150px;
            width: 250px;
            background: red;
            /*超出隐藏 */
            overflow: hidden;
            /*超出滚动*/
            overflow: scroll;

            overflow-x: hidden;/* y方向超出隐藏 */
            overflow-y: scroll;/* x方向超出滚动 */
        }
    </style>
</head>
<body>
    <div>
        yyuioiabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzbr    <br>
        wxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzbr    <br>
        wxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzbr    <br>
        wxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzbr    <br>
        wxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzbr    <br>wxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzbr    <br>

        wxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzbr    <br>
        wxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzbr    <br>
        wxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzbr    <br>
        wxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzbr    <br>
        abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz
    </div>
</body>
</html>
```

雪碧图

CSS精灵:（雪碧图）把小图标合成一张大图，通过给元素的公共css设置background-image为该合成图，这样每个元素都会以该合成图为背景，而且页面也只加载一张合成图，然后再给每个元素单独微调其background-position。把多个请求合并成一个

1.减小http请求次数，提高网页性能
2.减小图片大小

如果不使用雪碧图:

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>雪碧图实例</title>
    <link rel="stylesheet" type="text/css" href="css/index.css">
</head>

<body>
    <ul class="function">
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
    </ul>
</body>

</html>
```

css

```css
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

body {
  font-size: 18px;
}

.function {
  list-style: none;
  display: inline-block;
}
.function li {
  width: 40px;
  height: 40px;
  margin-bottom: 5px;
  background: #E0E0E0;
  background-repeat: no-repeat;
}
.function li:nth-child(1) {
  background-image: url(../img/all.png);
}
.function li:nth-child(2) {
  background-image: url(../img/back.png);
}
.function li:nth-child(3) {
  background-image: url(../img/cart.png);
}
.function li:nth-child(4) {
  background-image: url(../img/Category.png);
}
.function li:nth-child(5) {
  background-image: url(../img/close.png);
}
```

但是这样网页响应有点慢, 看network:

<img src="Snipaste_2018-05-23_10-14-31.png" width="50%">

可以看到这里有几张不到1k的小图片就这点大小还占用了多个请求

看看使用雪碧图的效果:

1. 首先要合并图片, 推荐http://csssprites.com/, 可以根据自己的需要添加边距等等，还会生成对应的参考css

```css
* {
    box-sizing: border-box;
    padding: 0;
    margin: 0;
}

body {
    font-size: 18px;
}

.function {
    list-style: none;
    display: inline-block;
}

.function li {
    width: 33px;
    height: 33px;
    margin-bottom: 5px;
    background: #E0E0E0;
    /* 此时的network就只有一个图片请求了 */
    background-image: url(../img/result.png); /*result.png为合并后的图片*/
    background-repeat: no-repeat;
}

.function li:nth-child(1) {
    background-position: 0 0;
}

.function li:nth-child(2) {
    background-position: 0 -35px;
}

.function li:nth-child(3) {
    background-position: 0 -72px;
}

.function li:nth-child(4) {
    background-position: 0 -110px;
}

.function li:nth-child(5) {
    background-position: 0 -145px;
}
```

再看一个简单的例子:

我们用到的图片是这样的

<img src="Snipaste_2018-05-23_11-32-40.png" width="50%">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>雪碧图</title>
    <style>
        div{
            background: url('img/xuebi.png') no-repeat 0 -456px;
            width: 52px;
            height: 52px;
        }
        div:hover{
            background: url('img/xuebi.png') no-repeat -144px -456px;
            width: 52px;
            height: 52px;
        }
    </style>
</head>
<body>
    <div></div>
</body>
</html>
```

最后出来的样子就是这样:(:hover 之后, 会变样子)

<img src="Snipaste_2018-05-23_11-37-37.png" width="20%">

## 雪碧图-demo

<img src="Snipaste_2018-05-23_15-07-45.png" width="50%">

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>外媒评论精选</title>
    <style>
        body {
            font-size: 100%;
            margin: 0;
            padding: 0;
            background-color: silver;
        }

        .main {
            width: 368px;
            height: 289px;
            margin: 100px auto 0;
            background-color: #fff;
        }

        .title {
            width: 100%;
            height: 23px;
            background: url(img/titlebg.png);
        }

        .title span {
            height: 13px;
            width: 14px;
            background: url(img/1.jpg) no-repeat -6px -4px;
            vertical-align: middle;
            display: inline-block;
            margin-left: 5px;
        }

        .title h1 {
            font-size: .8em;
            display: inline-block;
            margin: 0;
            vertical-align: middle;
            color: rgb(50, 67, 75);
        }

        .content {
            width: 100%;
        }

        .img {
            width: 101px;
            display: inline-block;
            vertical-align: middle;
            margin-left: 5px;
        }

        .font {
            width: 250px;
            font-size: .8em;
            display: inline-block;
            vertical-align: middle;
        }

        .p1 {
            font-weight: bold;
            margin-bottom: 6px;
        }

        .p2 {
            line-height: 1.6em;
            margin-top: 0px;
            color: #676B6E;
            height: 42px;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: pre-wrap;
        }

        .p2 a {
            text-decoration: none;
            color: #676B6E;
        }

        .p2 a:hover {
            color: red;
        }
    </style>
</head>

<body>
    <div class="main">
        <div class="title">
            <span></span>
            <h1>外媒评论精选</h1>
        </div>
        <div class="content">
            <div class="img">
                <img src="img/img1.png" alt="">
            </div>
            <div class="font">
                <p class="p1">《加勒比海盗4》---商业味浓郁</p>
                <p class="p2">本集《加勒比海盗》讲述了杰克船长受英王所托寻找“不老泉”，与他斤...
                    <a href="#">[详细]</a>
                </p>
            </div>
        </div>
        <div class="content">
            <div class="img">
                <img src="img/img1.png" alt="">
            </div>
            <div class="font">
                <p class="p1">《加勒比海盗4》---商业味浓郁</p>
                <p class="p2">本集《加勒比海盗》讲述了杰克船长受英王所托寻找“不老泉”，与他...
                    <a href="#">[详细]</a>
                </p>
            </div>
        </div>
        <div class="content">
            <div class="img">
                <img src="img/img1.png" alt="">
            </div>
            <div class="font">
                <p class="p1">《加勒比海盗4》---商业味浓郁</p>
                <p class="p2">本集《加勒比海盗》讲述了杰克船长受英王所托寻找“不老泉”，与他...
                    <a href="#">[详细]</a>
                </p>
            </div>
        </div>
    </div>
</body>

</html>
```

# 浮动

浮动：使元素脱离文档流，按照指定方向浮动，`遇到父元素边界或者相邻元素边界停止`

*    使支持宽高设置
*    块元素可以同排显示
*    默认内容撑开宽高
*    换行不被解析
*    `元素间没有间隙`
*  `脱离文档流(但还是会遵循div嵌套关系)`
*   浮起来的div会挡住后面的div


## 块元素同行显示,消除元素间隙

例子1:

<img src="Snipaste_2018-05-23_11-52-42.png" width="40%">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>浮动</title>
    <style>
        /* div{
            background-color: red;
            width: 100px;
            height: 100px;
            display: inline-block;
        }
        
        span{
            background-color: blue;
            height: 100px;
            width: 100px;
            display: inline-block;
        } */
    /*inline-block
            使得内嵌元素支持宽高设置
            块元素可以同排显示
            默认内容撑开宽高

            不足：
            换行被解析
            ie 6 7 不支持
        */
    

        /*浮动*/
        div{
            background-color: red;
            width: 100px;
            height: 100px;
            float: left;
        }
        
        span{
            background-color: blue;
            height: 100px;
            width: 100px;
            float: left;
        }
        /*浮动
            使用支持宽高设置
            块元素可以同排显示
            默认内容撑开宽高
            换行不被解析
        **    元素间没有间隙
         */
    </style>
</head>
<body>
    <div>div1</div>
    <div>div2</div>

    <span>span1</span>
    <span>span2</span>
</body>
</html>
```

## 脱离文档流

例子2:

<img src="Snipaste_2018-05-23_11-59-26.png" width="40%">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>浮动2-脱离文档流</title>
    <style>
        .box{
            border: 2px solid gray;
        }
        .box div{
            width: 100px;
            height: 100px;
            background-color: red;
            /*撑起父元素高度*/
            /* display: inline-block; */

            /* 只能是left 或right 才能脱离文档流 */
            float: left;
        }
        /*
            使用支持宽高设置
            块元素可以同排显示
            默认内容撑开宽高

            换行不被解析
            脱离文档流（文档流：文档中显示对象排列时占用的位置）

         */
    </style>
</head>
<body>
    <div class="box">
        <div>div1</div>
        <div>div2</div>
    </div>
</body>
</html>
```

例子3:

<img src="Snipaste_2018-05-23_12-05-19.png" width="30%">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>浮动3-验证脱离文档流</title>
    <style>
        .box1{
            width: 300px;
            height: 300px;
            border: 1px solid red;
        }
        .top1{/* 这块脱离文档流了 */
            width: 100px;
            height: 100px;
            background-color: gray;
            float: left;
        }
        .bottom1{/* 这块会被挡住 */
            width: 120px;
            height: 120px;
            background-color: blue;
            /* 如果加上, 就会顺序排列, 不会挡住 */
            /*float: left;*/
        }
    </style>
</head>
<body>
    <div class="box1">
        <div class="top1"></div>
        <div class="bottom1"></div>
    </div>
</body>
</html>
```

## 左右浮动, 清除浮动影响

clear: none|left|right|both, 指定哪边不能有浮动元素, `会发生运动的只是clear所在的元素`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>清浮动</title>
    <style>
        .box{
            border: 1px solid red;
        }
        .box span{
            height: 100px;
            width: 100px;
        }
        .box div{
            height: 100px;
            width: 100px;
        }
        .span1{
            background-color: gray;
            float: left;
            float: right;
        }
        .div2{
            background-color: blue;
            /*清除浮动
                相当于div另提一行
            */
            /*clear: right;*/
            clear: both;
        }

        /*
        clear:
        left: 左边不能有浮动元素
        right：右边不能有浮动元素
        both:左右都不能有浮动元素
         */
        .box2{
            border: 1px solid red;
        }
        .box2 div{
            height: 100px;
            width: 100px;
        }
        .box2_div1{
            background-color: blue;
            float: left;
        }
        .box2_div2{
            background-color: green;
            float: right;
        }
        .box2_div3{
            background-color: yellow;
            clear: both;
        }
    </style>
</head>
<body>
    <div class="box" >
        <span class="span1"></span>
        <div class="div2"></div>
    </div>
    

    <div class="box2">
        <div class="box2_div1"></div>
        <div class="box2_div2"></div>
        <div class="box2_div3"></div>
    </div>
</body>
</html>
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>验证清除前面所有的浮动元素</title>
    <style>
        .box{
            border: 1px solid red;
        }
        .box div{
            width: 100px;
            height: 100px;
        }
        div.div1{
            background-color: blue;
            float: left;
            height: 150px;
        }
        div.div2{
            height: 120px;
            background-color: gray;
            float: left;
        }
        .div3{
            background-color: green;
            /* 清除浮动元素的影响, 把float元素当成普通元素, 排在后面 */
            /* 如果注释, div-div3会从左上角开始排列, div-box的边框线只会围绕div3 */
            clear: left;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="div1"></div>
        <div class="div2"></div>
        <div class="div3"></div>
    </div>
</body>
</html>
```

## 浮动造成父元素高度为0的问题

<img src="Snipaste_2018-05-23_14-45-57.png" width="50%"><img src="Snipaste_2018-05-23_14-47-26.png" width="50%">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>解决浮动造成父元素高度为0的问题</title>
    <style>
        /*
        第一：父元素不浮动，子元素也不浮动，子元素内容可以撑起父元素高度
        第二：父元素不浮动，子元素浮动，不可以撑起父元素高度-------只有它没法撑起parent的高度

        第三：父元素浮动，子元素不浮动，可以撑起父元素高度
        第四：父元素浮动，子元素也浮动，也可以撑起父元素高度
         */
解决办法:
    /*
    1.加高：（无法自适应，扩展性差）
    2.父元素浮动：其他元素布局造成影响，margin auto失效
    3.inline-block: margin auto 失效

    4.overflow:hidden:
    越界都被隐藏，zoom：1兼容ie67

    5.6 空标签：不符合 行为 样式 结构 分离思想

    7.伪元素：目前用的最多，大型网站一般都是用的这个方法
    zoom：1 解决ie67兼容
     */
        .box{
            border: 2px solid black;
            /*1.父元素加高度*/
            /*height: 100px;*/
            /*2.自身加浮动*/
            /*float: left;*/

            /*3.inline-block*/
            /*display: inline-block;*/
            /*margin: 10px auto;*/
            /*width: 200px;*/

            /*4.overflow: hidden*/
            /*overflow: hidden;*/
            /*width: 200px;
            margin: 10px auto;*/
            /*zoom: 4;*/

            /*5.加br空标签 clear="all"*/


        }
        /*6。加空标签*/
        /* .box .clear{
            clear: both;    
        } */
        

        .box .div1{
            height: 100px;
            width: 100px;
            background-color: red;
            float: left;
        }
        /*7.通过伪元素 ie 6 7不支持*/
        /* 
        会插入.box元素内部的最后, clear:both表示清除内部所有元素的浮动float
         */
        .box::after{
            /*内容*/
            content: "";
            /*标签类型*/
            display: block;
            /*清除浮动*/
            clear: both;
            /*height: 8px;*/
            /*background-color: blue;*/
            height: 0;
            visibility: hidden;
        }
        /*解决IE67问题*/
        .box{ zoom: 1; }
    </style>
</head>
<body>
    <div class="box">
        <div class="div1"></div>
        <!-- <br clear="all"> -->
        <!-- <div class="clear"></div> -->
    </div>


</body>
</html>
```

## 浮动-demo

### 九宫格

<img src="Snipaste_2018-05-23_14-52-07.png" width="50%">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>demo</title>
    <style>
        .box{
            height: 400px;
            width: 400px;
            border: 1px solid red;
            font-size: 3em;
            font-weight: bold;
        }
        .box div{
            height: 100px;
            width: 100px;
            float: right;
            text-align: center;
            line-height: 100px;
        }
        div.div1{
            height: 300px;
            width: 300px;
            float: left;
            background-color: pink;
            line-height: 300px;
        }
        .div2,.div4,.div6,.div8{
            background-color: green;
        }
        .div3,.div5,.div7{
            background-color: yellow;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="div1">1</div>
        <div class="div2">2</div>
        <div class="div3">3</div>
        <div class="div4">4</div>
        <div class="div5">5</div>
        <div class="div6">6</div>
        <div class="div7">7</div>
        <div class="div8">8</div>
    </div>
</body>
</html>
```

li元素的例子:

<img src="Snipaste_2018-05-23_17-46-31.png" width="50%">

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>流式布局</title>
        <!--
            不再用px单位，（rem设置文字或者宽高）
            用百分比计算宽高，margin或padding也用百分比
            
            目标元素宽高 / 上下文元素宽高（保留5位小数）
        -->
        <style>
            *{
                padding: 0;
                margin: 0;
            }
            /* 取消小点点 */
            li{list-style: none}
            /**/
            section{
                width: 800px;
                height: 800px;
                border: 2px solid red;
                margin: 20px auto;
            }
            
            ul{
                width: 100%;
                height: 100%;
            }
            li{
                background: blue;
                /*22%*/
                width: 22%;
                height: 22%;
                border: 5px solid black;
                /*怪异模式 width+padding+border+内容*/
                box-sizing: border-box;
                /* 呈九宫格排列 */
                float: left;
                color: red;
                font-size: 40px;
                
                /*12%/5*/
                margin-left: 2.4%;
                /*是根据width宽度计算*/
                margin-top: 2.4%;
            }
        </style>
    </head>
    <body>
        <section>
            <ul>
                <li>1</li>
                <li>2</li>
                <li>3</li>
                <li>4</li>
                <li>5</li>
                <li>6</li>
                <li>7</li>
                <li>8</li>
                <li>9</li>
                <li>10</li>
                <li>11</li>
                <li>12</li>
                <li>13</li>
                <li>14</li>
                <li>15</li>
                <li>16</li>
            </ul>
        </section>
    </body>
</html>

```

### float,div布局经典用法

<img src="Snipaste_2018-05-23_15-21-23.png" width="50%">

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>float</title>
    <style>
        body {
            background-color: #F8F8F8;
        }

        .box {
            /* 居中布局 */
            margin: 50px auto;
            height: 393px;
            width: 439px;
            border: 1px solid red;
            text-align: center;
            font-size: 3em;
            font-weight: border-left;
        }

        .top {
            /* 占满parent元素width */
            width: 100%;
            height: 70px;
        }

        .box1,
        .box2,
        .box3,
        .box4,
        .box5 {
            background-color: green;
            height: 70px;
            width: 87.8px;
            float: left;
            line-height: 70px;
        }

        .middle {
            width: 100%;
            height: 250px;
        }

        .middle_left {
            width: 264px;
            height: 100%;
            /* 如果不加, middle_left会占到一行, middle_right就会另提一行 */
            float: left;
        }

        .ml_top {
            width: 100%;
            height: 70px;
        }

        .box6 {
            height: 100%;
            width: 100%;
            background-color: yellow;
            line-height: 70px;
        }

        .ml_middle {
            width: 100%;
            height: 105px;
        }

        .box8 {
            width: 50%;
            height: 100%;
            background-color: silver;
            float: left;
            line-height: 105px;
        }

        .box9 {
            width: 50%;
            height: 100%;
            background-color: black;
            float: left;
            line-height: 105px;
            color: white;
        }

        .ml_bottom {
            width: 100%;
            height: 75px;
        }

        .box11 {
            height: 100%;
            width: 100%;
            background-color: red;
            line-height: 75px;
        }

        .middle_right {
            width: 175px;
            height: 100%;
            float: left;
        }

        .box7 {
            width: 100%;
            height: 142px;
            background-color: pink;
            line-height: 142px;
        }

        .box10 {
            width: 100%;
            height: 108px;
            background-color: #88CDEA;
            line-height: 108px;
        }

        .bottom {
            width: 100%;
            height: 75px;
        }

        .box12 {
            height: 100%;
            width: 100%;
            background-color: blue;
            line-height: 75px;
        }
    </style>
</head>

<body>
    <div class="box">
        <div class="top">
            <div class="box1">1</div>
            <div class="box2">2</div>
            <div class="box3">3</div>
            <div class="box4">4</div>
            <div class="box5">5</div>
        </div>
        <div class="middle">
            <div class="middle_left">
                <div class="ml_top">
                    <div class="box6">6</div>
                </div>
                <div class="ml_middle">
                    <div class="box8">8</div>
                    <div class="box9">9</div>
                </div>
                <div class="ml_bottom">
                    <div class="box11">11</div>
                </div>
            </div>
            <div class="middle_right">
                <div class="box7">7</div>
                <div class="box10">10</div>
            </div>
        </div>
        <div class="bottom">
            <div class="box12">12</div>
        </div>
    </div>
</body>

</html>
```

# 定位

* position 定位
    * 相对定位：relative
    * 绝对定位：absolute
    * 相对窗口定位： fixed
    * 默认定位：static

## z-index层级

<img src="Snipaste_2018-05-23_16-30-54.png" width="50%">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>z-index</title>
    <style>
        body{ position: relative; }
        div{ width: 100px; height: 100px; }
        .div1{ background-color: red; }
        .div2{ background-color: yellow; }
        .div3{ background-color: blue; }

        .div1{
            position: absolute;
            left: 0;
            top: 0;
            /*定位元素显示优先级，越大，优先级越高*/
            z-index: 1;
        }
        .div2{
            position: relative;
            left: 20px;
            top: 20px;
            z-index: 2;
        }
        .div3{
            position: absolute;
            left: 40px;
            top: 40px;
            z-index: 100;
        }
    </style>
</head>
<body>
    <div class="div1">div1</div>
    <div class="div2">div2</div>
    <div class="div3">div3</div>
</body>
</html>
```

## relative相对定位

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>相对定位</title>
    <style>
        div{ width: 100px; height: 100px; }
        .div1{ background-color: red; }
        .div2{ background-color: yellow; }
        .div3{ background-color: green; }
        /**/
        /*.div2{ 
            margin-left: 100px;
            margin-top: 100px;
        }
        .div3{
            margin-top: -100px;
        }*/

        /*相对定位移动div2*/
        .div2{ 
            /*设置相对定位
            (相对于自身位置偏移)
            */
            position: relative;
            /*和左边距离, == 右移10px*/
            left: 10px;
            /*可以正负数*/
            /*top: -100px;*/
            /*底部位置*/
            /*bottom: 100px;*/
            bottom: 30px;
        }
        .div3{
            position: relative;
        }
    </style>
</head>
<body>
    <div class="div1">div1</div>
    <div class="div2">div2</div>
    <div class="div3">div3</div>
</body>
<!-- 
相对定位：
a.不影响元素本身特性
b.不会使元素脱离文档流（元素后移后位置会被保留）
c.定位后，如果没有位置偏移，没有任何影响
d.元素相对自身位置定位
e.提升层级
 -->
</html>
```

### relative相对定位的优先级问题

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>相对定位问题</title>
    <style>
        div{ 
            width: 100px; 
            height: 100px;
            background-color: red;
            position: relative;
            /*left和right同时出现，right失效*/
            /*left: 50px;*/
            /*right: 50px;*/

            /*top和bottom同时出现，bottom失效*/
            top: 50px;
            bottom: 100px;

            /*
            优先级：
            left>right
            top>bottom
             */
        }
    </style>
</head>
<body>
    <div>div1</div>
</body>
</html>
```

## absolute绝对定位

绝对定位的元素的位置相对于最近的已定位祖先元素，如果元素没有已定位的祖先元素，那么它的位置相对于最初的包含块。

普通流中其它元素的布局就像绝对定位的元素不存在一样

<img src="Snipaste_2018-05-23_16-16-55.png" width="50%">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>绝对定位</title>
    <style>
        div{ width: 100px; height: 100px; }
        .div1{ background-color: red; }
        .div2{ background-color: yellow; }
        .div3{ background-color: green; }


        .div2{
            position: absolute;
            /* 透明度 */
            opacity: .5;
            /* 100px+8px(8px是div的外边距) */
            left: 108px;
        }

        a{
            background-color: gray;
            width: 100px;
            height: 100px;
            position: absolute;
            bottom: 350px;
        }
    </style>
</head>
<body>
    <div class="div1">div1</div>
    <div class="div2">div2</div>
    <div class="div3">div3</div>
    <a href="#">百度</a>
</body>
<!-- 
绝对定位：
a.使元素脱离了文档流
b.改变元素性质，块元素内容撑开宽高，内联元素支持宽高设置
c.提升层级
d.
 -->
</html>
```

### div在div内部居中

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>居中问题</title>
    <style>
        .box{
            height: 300px;
            width: 300px;
            background-color: red;
            position: relative;
            /*解决子元素margin-top作用父级*/
            overflow: hidden;
        }
        .center{
            width: 100px;
            height: 100px;
            background-color: blue;
            /*常用：子元素绝对定位，父元素相对定位*/
            position: absolute;
            /*position: relative;*/
            /*必须知道自身的宽高*/
            left: 50%;
            margin-left: -50px;
            top: 50%;
            margin-top: -50px;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="center"></div>
    </div>
</body>
</html>
```

另一种方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>居中2</title>
    <style>
        body{
            position: relative;
        }
        .wrap{
            width: 300px;
            height: 300px;
            background-color: red;

            /* position: absolute; */
            /*横向居中*/
            /* left: 0;
            right: 0; */
            margin: 0 auto;

            position: relative;
        }
        .center{
            background-color: blue;
            /*宽高适应父元素宽高（不给宽度）*/
            position: absolute;
            /*left: 0;
            right: 10px;
            top: 0;
            bottom: 10px;*/

            /*横向纵向居中（必须设定宽度高度）*/
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            margin: auto;
            
            height: 100px;
            width: 100px;

        }
    </style>
</head>
<body>
    <div class="wrap">
        <div class="center"></div>
    </div>
</body>
</html>
```

### 图片上的灰色蒙板,居中

<img src="Snipaste_2018-05-23_17-14-18.png" width="50%">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>图片隐藏</title>
    <style>
        p{/* 先给所有p标签的默认盒子模型清空 */
            margin: 0;
            padding: 0;
            text-align: center;
        }
        section{
            width: 250px;
            /*居中设置，必须有宽度*/
            /* 这块删掉了貌似也没问题 */
            /* position: relative;
            left: 0;
            right: 0; */
            margin: 20px auto;
            /*外围阴影*/
            box-shadow: 2px 2px 10px black;
            /*圆角*/
            border-radius: 5px;
            /*越出隐藏*/
            overflow: hidden;
            /*鼠标图像样式*/
            /* 加上了, 即使在普通文本上, 也显现手形标志 */
            cursor: pointer;
        }
        a{
            width: 100%;
            display: inline-block;
            text-decoration: none;
            color: white;
    
            /*让隐藏的div相对最近的定位属性的元素定位*/
            position: relative;
        }
        img{
            width: 100%;
            vertical-align: top;
        }
        .bottom-title{
            padding: 7px 0;
            color: gray;
        }
        .hidden{
            /*background-color: black;
            opacity: .5;*/
            background: rgba(0,0,0,.5);
            /*绝对定位*/
            position: absolute;
            left: 0;
            top: 0;
            right: 0;
            bottom: 0;
            opacity: 0;
            /*display: none;*/
            /* opacity: 1; */
            /* 1s 渐变
                渐变的属性为opacity, duration为1s
            */
            transition: opacity 1s;
            border-radius: 6px;
        }
        .middle{
            position: absolute;
            left: 0;
            right: 0;
            top: 0;
            bottom: 0;
            /*给宽高*/
            height: 52px;
            margin: auto;
        }
        /*找到middle下的第一个p元素*/
        .middle p:first-child{
            font-size: 30px;
        }
        section:hover .bottom-title{
            color: black;
        }
        section:hover .hidden{
            opacity: 1;
            /*display: block;*/
        }
    </style>
</head>
<body>
    <section>
        <a href="#">
            <img src="img/img.jpg" alt="">
            <div class="hidden">
                <div class="middle">
                    <!-- ctrl+shift+方向键，上下移动 -->
                    <p>腾讯</p>
                    <p>今天就看今日头条</p>
                </div>
            </div>
        </a>
        <p class="bottom-title">腾讯网络媒体峰会</p>
    </section>
</body>
</html>
```



## 窗口定位

窗口定位：和绝对定位性质基本一致，区别他是始终相对于窗口定位


# 过场动画

[css动画回调](https://www.cnblogs.com/afei-qwerty/p/6869023.html)
CSS3有animation
 普通css属性有 transition

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8" />
        <title>过渡动画</title>
        <style>
            div{
                width: 100px;
                height: 100px;
                background: red;
                /*动画设置*/
                
                /*-webkit-transition-property: all;*/
                /*动画属性，让哪一个属性有动画(多个属性用逗号隔开)*/
                /*transition-property: background,width,height;*/
                /*transition-property: all;*/
                /*动画持续时间（必要条件）*/
                /*transition-duration: 1s;*/
                
                /*简写: 动画属性省略：默认的所有（all）*/
                transition: 1s;
            }
            div:hover{
                width: 200px;
                height: 200px;
                background-color: blue;
            }
        </style>
    </head>
    <body>
        <div></div>
    </body>
</html>

```

# seo

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <!-- 整个网页不去收录 -->
    <meta name="robots" content="nofollow">
    <meta name="robots" content="noindex,nofollow">
    <title>seo</title>
</head>
<body>
    seo:搜索引擎优化（不花钱）
    sem:搜索引擎营销（花钱）
    白帽，黑帽

    pr:网页等级，用来表示一个网页等级的标准，级别 0-10，一般级别在6

    <!-- <meta name="keywords" content="iOS培训,iOS培训班,北京iOS培训,HTML5培训,北京HTML5培训,iOS培训机构,iOS开发培训,iOS培训学校,北京安卓培训,Android培训,北京Android培训,Android开发培训,Web安全培训,App开发培训" />
    <meta name="description" content="腾讯网十年最具创新力IT教育品牌！国内最大的App开发培训基地，专注iOS培训、HTML5培训、Android开发培训、Unity 3d培训、Web安全攻防培训,选择蓝鸥培训机构,三大创始人亲自授课，学员平均就业薪资行业最高。" /> -->
    <!-- <meta name="keywords" content=""> -->
    <!-- <meta name="description" content=""> -->
    <!-- <meta name="author" content=""> -->
    

    对seo优化有用的标签：

    title: 权重最大：100

    meta 属性：name=keywords，content=description
    h1-h6:权重次于title (h1只能用一次) h2（10次左右）

    strong,em: 语气加重，重要的文本 

    img:属性alt(替换文本)

    <img src=".." alt="快快快">

    a：title:说明，

    <a href="#" title="文本提示"></a>
    <div title="提示">急急急</div>

    <!-- a: rel="nofollow" 
        告诉百度蜘蛛：这个链接跟我没有关系，不用去收录
    -->
    <a href="" rel="nofollow"></a>
</body>
</html>
```

# demo

## 典型注册表单

<img src="Snipaste_2018-05-22_23-29-43.png" width="50%">



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册邮箱账号</title>
    <style>
        body{ margin: 0; font-size: 100%; background-color: #DCE1E7; }
        .main{ width: 800px; background-color: #fff; margin: 20px auto; }
        .top{ width: 100%; background: url(img/email-bg.jpg); background-repeat: no-repeat; height: 52px; padding-left: 1em; vertical-align: middle; display: table-cell; }
        .form{ padding-left: 3em; line-height: 3em; }
        .font{ text-align: right; width: 150px; display: inline-block; vertical-align: middle; }
        #email,#username,#password,#repassword{ height: 30px; width: 300px; }
        /* display: inline-table 元素间距去除, 针对chrome */
        .inputoth{ display: inline-table; vertical-align: top; font-size:.5em; width: 150px; line-height: 1.1em; padding-top: 1.2em;}
        #reg,#yz{ width: 300px; }
        #yz,#down{ display: inline-block; vertical-align: middle; }
        #sex{ width: 50px; }
    </style>
</head>
<body>
    <div class="main">
        <div class="top">注册邮箱账号</div>
        <hr>
        <div class="form">
            <form action="#" method="post" >
                <label for="email">
                    <span class="font">邮箱账号</span>
                    <input type="email" name="email" id="email">
                </label>
                <div class="inputoth">
                    请输入您常用的电子邮箱<br>
                    <a href="#">创建邮箱</a>
                    或
                    <a href="#">注册普通QQ号</a>
                </div>
                <br>
                <label for="username">
                    <span class="font">昵称</span>
                    <input type="text" name="username" id="username" >
                </label>
                <br>
                <label for="password">
                    <span class="font">密码</span>
                    <input type="password" name="password" id="password">
                </label>
                <br>
                <label for="repassword">
                    <span class="font">确认密码</span>
                    <input type="password" name="password"  id="repassword">
                </label>
                <br>
                <span class="font">性别</span>                
                <input type="radio" name="sex" value="0" id="sex">男
                <input type="radio" name="sex" value="1" id="sex">女
                <br>
                <span class="font">生日</span>
                <select name="type" id="type">
                    <option value="0">公历</option>
                    <option value="1">农历</option>
                </select>
                <input type="date" name="date">
                <br>
                <span class="font">所在地</span>
                <select name="con" id="con">
                    <option value="1">中国</option>
                    <option value="2">毛里求斯共和国</option>
                </select>
                <select name="qu" id="qu">
                    <option value="1">上海</option>
                    <option value="2">呼和浩特</option>
                </select>
                <select name="add" id="add">
                    <option value="1">黄埔</option>
                    <option value="2">十三里堡</option>
                </select>
                <br>
                <span class="font">验证码</span>
                <a href="#"><img src="img/yz.png" alt="yz" id="yz"></a>
                请先进行安全验证
                <br>
                <span class="font"></span>
                <label for="yes">
                    <input type="checkbox" name="yes" id="yes">
                    我已阅读并同意相关服务条款和隐私政策
                    <a href="#"><img src="img/down.jpg" alt="down" title="查看详情" id="down"></a>
                </label>
                <br>
                <span class="font"></span>
                <input type="image" src="img/register.jpg" value="register" id="reg">

            </form>
        </div>
    </div>
</body>
</html>
```

## 典型的图片文字排版

<img src="Snipaste_2018-05-22_23-35-40.png" width="50%">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>加入我们</title>
    <style>
        body{ background-color: silver; font-size:100%;}
        .main{ max-width:800px; 
            background-color: #F2F2F2; margin:0px auto; }
        .caption{ max-width: 70%; margin:0 auto; padding-bottom:2.5em;}
        .img{ width:100%; padding-top:1.2em;}
        .img img{ width:100%; }
        h1{ font-size: 1.7em; font-weight:900; }
        .sum{ width:100%; font-family:"黑体"; }
        .sum p{ font-weight:bold; text-indent:5em; margin-top:3em; line-height:1.3em; text-align: justify; color:#0E0E0E; }
        .session{ width:100%; font-family:"宋体"; }
        .session img{ max-width:150px; margin-top:30px; }
        .session p{ font-size:1em; line-height:1.3em; color:#595959; margin-bottom:1.5em; }
    </style>
</head>
<body>
    <div class="main">
        <div class="caption">
            <div class="img">
                <img src="img/joinus.jpg" alt="加入我们" title="加入我们">
            </div>
            <div class="sum">
                <h1>加入我们</h1>
                <p>作为一群年轻激情的创作团队，我们拥有共同的追求和热爱，创作更具创造力的作品。这里不会出现固步自封，及流水线式的动画“加工”，拥有灵魂的作品才不会被时代遗忘。我们相信创意、设计、动画、音频的基因融合，聚集最优秀的创作人，一起协同才能变得更加专业、完美和优秀。我们不相信“追求热爱”与“苦逼”一定会挂上等号，在提供更优厚的待遇和包容的同时，你只需一心投入更好地创作。</p>
            </div>
            <div class="session">
                <img src="img/joinus4.jpg" alt="文案创意" title="文案创意" >
                <p>
                    从事职务：<br>
                    深入理解客户品牌定位和产品市场；<br>
                    构想动画广告的创意方案及文案撰写；<br>
                    协同其他环节人员共同执行创意方案。
                </p>
                <p>
                    期望条件：<br>
                    创造力第一；<br>
                    有广告行业的文案、创意经验；<br>
                    脑洞大开，创意思维活跃；<br>
                    良好的逻辑思维，懂得换位思考；<br>
                    精准洞察产品痛点orG点；<br>
                    出色的文字功底，有感染力；<br>
                    欢迎诗人、小说家、剧作家或者各界异能人士。
                </p>
                <p>
                    优先条件：<br>
                    审美好，懂美术，热衷视觉创意；<br>
                    对产品、市场和商业领域充满兴趣；<br>
                    知识全面，涉猎广泛。
                </p>
                <img src="img/joinus9.jpg" alt="客户经理" title="客户经理" >
                <p>
                    从事职务：<br>
                    负责与客户沟通跟进项目，准确把握客户需求；<br>
                    参与制定创意方案策划、提案和执行；<br>
                    协调各环节人员，解决项目进程问题；<br>
                    整理项目相关文件，能够及时有效调用。
                </p>
                <p>
                    期望条件：<br>
                    熟悉广告行业，具备管理经验和客户沟通经验；<br>
                    性格开朗，善于沟通；<br>
                    喜爱视觉设计领域，审美好；<br>
                    具备较强的责任感、积极的心态以及团队合作意识。
                </p>
                <p>
                    优先条件：<br>
                    有从事设计或动画相关工作经验；<br>
                    良好的逻辑思维，敏捷的应变能力；<br>
                    知识全面，涉猎广泛。
                </p>
            </div>
        </div>        
    </div>
</body>
</html>
```

## 选中则变换颜色

<img src="Snipaste_2018-05-22_23-33-36.png" width="50%">

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>练习</title>
    <style>
        #box{ width: 500px; margin: 0 auto; }
        /*input:hover+span{ color:yellow; }*/
        .c1:hover+span{ color: yellow; }
        .cl1:hover+span{ color: orange; }
        #b1:checked+span{ background-color: blue; }
        #b1:checked+span::after{ content:"蓝色被选中了"; }
        #re:checked+span{ background-color: red; }
        #re:checked+span::after{ content:"红色被选中了"; }
        #gr:checked+span{ background-color: green; }
        #gr:checked+span::after{ content:"绿色被选中了"; }
    </style>
</head>
<body>
    <div id="box">
        <form action="###">
            <!-- 控件组 -->
            <fieldset>
                <!-- 说明 -->
                <legend>单选</legend>
                <div>
                    <label for="b1">
                        <input type="radio" name="name" value="0" id="b1" class="c1"><span>蓝色</span>
                    </label>
                </div>
                <div>
                    <label for="re">
                        <input type="radio" name="name" value="1" id="re" class="c1"><span>红色</span>
                    </label>
                </div>
                <label for="gr">
                    <input type="radio" name="name" value="2" id="gr" class="c1"><span>绿色</span>
                </label>
            </fieldset>
            <!-- 控件组 -->
            <fieldset>
                <legend>多选</legend>
                <ul>
                    <li>
                        <div>
                            <input type="checkbox" name="name" value="0" id="b1" class="cl1"><span>蓝色</span>
                        </div>
                    </li>
                    <li>
                        <div>
                            <input type="checkbox" name="name" value="1" id="re" class="cl1"><span>红色</span>
                        </div>
                    </li>
                    <li>
                        <div>
                            <input type="checkbox" name="name" value="1" id="gr" class="cl1"><span>绿色</span>
                        </div>
                    </li>
                </ul>
            </fieldset>
        </form>
    </div>
</body>
</html>
```

## 小方块信息展示

```html

```