---
title: FreeMarker Note
tags:
  - template
date: 2017-08-25 21:46:25
categories: java web
---

<div align="center">
references: https://freemarker.apache.org/docs/dgui_quickstart_template.html
</div>
<!--more-->

<!-- TOC -->

- [和jsp-velocity-thymeleaf对比](#和jsp-velocity-thymeleaf对比)
- [和 springboot 集成](#和-springboot-集成)
- [quick start](#quick-start)
- [基本语法](#基本语法)
  - [支持的类型](#支持的类型)
  - [if 条件指令](#if-条件指令)
  - [list 遍历指令](#list-遍历指令)
  - [include 和 import](#include-和-import)
  - [定义变量](#定义变量)
  - [命名空间](#命名空间)
  - [联合使用指令](#联合使用指令)
  - [内建函数](#内建函数)
  - [方法和函数](#方法和函数)
  - [用户自定义指令(推荐优先使用)](#用户自定义指令推荐优先使用)
    - [macro nested 宏指令 嵌套指令](#macro-nested-宏指令-嵌套指令)
    - [宏指令中的循环变量](#宏指令中的循环变量)
  - [表达式](#表达式)
- [处理不存在的变量](#处理不存在的变量)
- [常见错误](#常见错误)
  - [指令体内部变量无需${}](#指令体内部变量无需)
  - [数字千分位格式化](#数字千分位格式化)
- [程序开发](#程序开发)
  - [freemaker对象的包装](#freemaker对象的包装)
  - [Configuration 配置](#configuration-配置)
    - [共享变量](#共享变量)
    - [三层配置](#三层配置)
      - [Configuration层](#configuration层)
      - [Template](#template)
      - [Environment](#environment)
    - [模板的加载](#模板的加载)
    - [错误控制](#错误控制)
  - [通过java开发自定义方法](#通过java开发自定义方法)
  - [通过java开发自定义指令](#通过java开发自定义指令)

<!-- /TOC -->

# 和jsp-velocity-thymeleaf对比

https://blog.csdn.net/richard_vi/article/details/78849539

velocity 语法简单， freemarker 功能更强大

- FreeMarker可以对任意数量的类型执行运算和比较，包括任意精度类型，而不仅仅是整数。 
- FreeMarker可以比较和显示（格式化）日期/时间值
- FreeMarker可以基于各种各样的内置和自定义数字或者日期格式，来格式化数字区域或者日期地区时区等
- 1.5及以上版本的spring boot已经不支持velocity。

# 和 springboot 集成

```yml

```


# quick start

完成一个简单的demo, 需要准备2部分工作要做

```html
数据模型

        (root)
        |
        +- user = "Big Joe"
        |
        +- latestProduct
            |
            +- url = "products/greenmouse.html"
            |
            +- name = "green mouse"
            |
            +- animals
            |   |
            |   +- (1st)
            |   |   |
            |   |   +- name = "mouse"
            |   |   |
            |   |   +- size = "small"
            |   |   |
            |   |   +- price = 50
            |   |
            |   +- (2nd)
            |   |   |
            |   |   +- name = "elephant"
            |   |   |
            |   |   +- size = "large"
            |   |   |
            |   |   +- price = 5000
            |   |
            |   +- (3rd)
            |       |
            |       +- name = "python"
            |       |
            |       +- size = "medium"
            |       |
            |       +- price = 4999
            |
            +- misc
                |
                +- fruits
                    |
                    +- (1st) = "orange"
                    |
                    +- (2nd) = "banana"

    模型数据通常来自于java对象, 树结构中的目录类比于嵌套对象, 称为`hashes`, 叶子节点称为`scalars`, 访问语法: latestProduct.url; 另外一种很重要的变量是 `sequences `, 它们像哈希表那样存储子变量，但是子变量没有名字，它们只是列表中的项, 语法: latestProduct.animals[0].name

*   模版

    <html>
        <head>
            <title>Welcome!</title>
        </head>
        <body>
            <h1>Welcome ${user}!</h1>
            <p>Our latest product:
            <a href="${latestProduct.url}">${latestProduct.name}</a>!
        </body>
    </html>
```

# 基本语法

*   `${...}` ： FreeMarker将会输出真实的值来替换大括号内的表达式，这样的表达式被称为 interpolation(插值)
*   `FTL 标签` : (FreeMarker模板的语言标签)： FTL标签和HTML标签有一些相似之处，但是它们是FreeMarker的指令，是不会在输出中打印的。 这些标签的名字以 `# `开头。(用户自定义的FTL标签则需要使用 @ 来代替 #)

*   `注释`： 注释和HTML的注释也很相似， 但是它们使用 `<#-- and --> `来标识。 不像HTML注释那样，FTL注释不会出现在输出中(不出现在访问者的页面中)， 因为 FreeMarker会跳过它们。

## 支持的类型

*   标量(scalars)
    *   字符串
    *   数字
    *   布尔值 true/false
    *   日期/时间
*   容器
    *   哈希表: 每个子变量都可以通过一个唯一的名称来查找。 这个名称是不受限制的字符串。哈希表 并不确定其中子变量的顺序
    *   序列: 每个子变量通过一个整数来标识,而且子变量是有顺序的, 子变量的类型也并不需要完全一致
    *   集合: 集合是有限制的序列。不能获取集合的大小， 也不能通过索引取出集合中的子变量，但是它们仍然可以通过 list 指令来遍历。

## if 条件指令

```html
Welcome ${user}<#if user == "Big Joe">, our beloved leader</#if>!
```

若user == "Big Joe", 则显示", our beloved leader"

类似的, `不等于`的写法

```html
<#if animals.python.price != 0>
  Pythons are not free today!
</#if>
```

多重判断

```html
<#if animals.python.price < animals.elephant.price>
  Pythons are cheaper than elephants today.
<#elseif animals.elephant.price < animals.python.price>
  Elephants are cheaper than pythons today.
<#else>
  Elephants and pythons cost the same today.
</#if>
```

## list 遍历指令

```html
<p>We have these animals:
<table border=1>
  <#list animals as animal>
    <tr><td>${animal.name}<td>${animal.price} Euros
  </#list>
</table>

<#list ["foo", "bar", "baz"] as x>
    ${x}
</#list>
```

带分隔符的输出

```html
Fruits: <#list misc.fruits as fruit>${fruit}<#sep>, </#list>
```
也可以这么写: <#sep>, </#sep>

这样最后一项后面就不会有分隔符了

进一步修改: 处理空值

```html
Fruits: <#list misc.fruits as fruit>${fruit}<#sep>, <#else>None</#list>
```
这样当没有值的时候, 会显示"None";

另一种简写语法: `Fruits: ${fruits?join(", ", "None")}`

list 和items合用, 处理空值:

```html
<#--items指令, 只有items里的元素才会循环-->
<#list nullTest>
    <ul>
    <#items as fruit>
        <li>${fruit}
    </#items>
    </ul>
</#list>
<div>上面没有空行</div>

<#--比较: 没有使用items的情况-->
<ul>
    <#list nullTest as fruit>
        <li>${fruit}
    </#list>
</ul>
<div>有空行</div>
```

## include 和 import

```html
<body>
  <h1>Test page</h1>
  <p>Blah blah...
  <#include "/copyright_footer.html">
</body>
```

include只有开始标签, 没有结束标签, 因为没有标签嵌套内容, 结束标签可以省略

实际重用代码有更好的方式: 自定义指令(宏命令), 下文讨论

再看import, import也是用于变量生命导入声明

## 定义变量

*   简单变量： 它能从模板中的任何位置来访问，或者从使用 include 指令引入的模板访问。可以使用 `assign 指令`来创建或替换这些变量。因为宏和方法只是变量，那么 `macro 指令 和 function 指令 也可以用来设置变量`，就像 assign 那样。

*   局部变量：它们只能被设置在 宏定义体内， 而且只在宏内可见。一个局部变量的生命周期只是宏的调用过程。可以使用` local指令` 在宏定义体内创建或替换局部变量。

*   循环变量：循环变量是由如 `list 指令`自动创建的，而且它们只在指令的开始和结束标记内有效。宏 的参数是局部变量而不是循环变量。

*   全局变量：这是一个高级话题了， 并且这种变量最好别用。即便它们属于不同的命名空间， 全局变量也被所有模板共享，因为它们是被 import进来的， 不同于 include 进来的。那么它们的可见度就像数据模型那样。 全局变量通过 `global指令`来定义。

## 命名空间

使用 assign 和 macro 指令创建的变量的集合, 这样的变量集合就是命名空间; 如果想创建可以重复使用的宏，函数和其他变量的集合， 通常用术语来说就是引用 库, 为了避免变量名冲突, 引入命名空间;

首先建立一个库:

```html
<!-- 俩变量: copyright, mail; 保存在文件my_test.ftl -->
<#macro copyright date>
  <p>Copyright (C) ${date} Julia Smith. All rights reserved.</p>
</#macro>

<#assign mail = "jsmith@acme.com">

```

使用: 通过import命令引入(不要使用include命令, 因为这样会在主命名空间创建两个变量, 而import会引入新的命名空间)

```html
<#import "/lib/my_test.ftl" as my> <#-- the hash called "my" will be the "gate" -->
<@my.copyright date="1999-2002"/>
${my.mail}

<!-- 在引入的命名空间中增加变量, assign命令
    结果:
    jsmith@acme.com
jsmith@other.com
 -->
 <#import "/lib/my_test.ftl" as my>
${my.mail}
<#assign mail="jsmith@other.com" in my>
${my.mail}
```

在模板中的变量优先级高于数据模型中的变量.

库的路径一般这样: /lib/yourcompany.com/your_library.ftl, 方便他人引用;


## 联合使用指令

指令可嵌套使用, 即使是在尖括号内部; 因为FreeMarker并不解析FTL标签以外的文本、插值和注释， 所以在HTML属性中使用FTL标签也不会有问题。

```html
<#list animals as animal>
      <div<#if animal.protected> class="protected"</#if>>
        ${animal.name} for ${animal.price} Euros
      </div>
</#list>
```

## 内建函数

内建函数类似于子属性(scalars), 但是访问方法不是`.`, 而是`?`, 常用的列在这里

*   user?html 给出 user 的HTML转义版本， 比如 & 会由 `&amp;` 来代替。
*   user?upper_case 给出 user 值的大写版本 (比如 "JOHN DOE" 来替代 "John Doe")
*   animal.name?cap_first 给出 animal.name 的首字母大写版本(比如 "Mouse" 来替代 "mouse")
*   user?length 给出 user 值中 字符的数量(对于 "John Doe" 来说就是8)
*   animals?size 给出 animals 序列中 项目 的个数
*   animal.protected?string("Y", "N") 基于 animal.protected 的布尔值来返回字符串 "Y" 或 "N"。
*   `animal?item_cycle('lightRow','darkRow')`, 根据当前animal计数的奇偶性, 返回字符串 "odd" 或 "even", 在给不同行着色时使用
*   `fruits?join(", ")` 通过连接所有项，将列表转换为字符串， 在每个项之间插入参数分隔符`, `
*   `user?starts_with("J")` 根据 user 的首字母是否是 "J" 返回布尔值true或false
*   专门在<#list>指令体内使用的
    *   animal?index 给出了在 animals 中基于0开始的 animal的索引值
    *   animal?counter 也像 index， 但是给出的是基于1的索引值
    *   `animal?item_parity` 基于当前计数的奇偶性，给出字符串 "odd" 或 "even"。在给不同行着色时非常有用，比如在 `<td class="${animal?item_parity}Row">`中。功能同animal?item_cycle('lightRow','darkRow')


## 方法和函数

使用: 

```
The average of 3 and 5 is: ${avg(3, 5)}
The average of 6 and 10 and 20 is: ${avg(6, 10, 20)}
The average of the price of a python and an elephant is:
${avg(animals.python.price, animals.elephant.price)}
```

方法来自于数据模型中的定义, 函数来自于模版内的定义

## 用户自定义指令(推荐优先使用)

换句话说，就是FreeMarker的标签

假设现在有一个变量 box，它的值是用户自定义的指令， 用来打印一些特定的HTML信息，包含标题和一条信息。那么， box 变量就可以在模板中使用: 

```html
<@box title="Attention!">
  Too much copy-pasting may leads to
  maintenance headaches.
</@box>
```

### macro nested 宏指令 嵌套指令

```html
<#--user-customized-directive-->
<#--macro define-->
<#macro greet>
    <font size="+2">Greet: hello</font>
</#macro>
<#--macro 调用 -->
<@greet/>
<br>
<#--macro define 带参数, 这里名称相同会覆盖之前定义的同名宏-->
<#macro greet person>
    <font size="+2">hello, ${person}</font>
</#macro>
<@greet person="xiaoyutest"/>

<!-- 定义多个参数, 带默认值, 调用时可以覆盖 -->
<#macro greet person color="black">
  <font size="+2" color="${color}">Hello ${person}!</font>
</#macro>

<!-- 自定义指令嵌套内容, <#nested> 指令执行位于开始和结束标记指令之间的模板代码段。
    输出:
    <table border=4 cellspacing=0 cellpadding=4><tr><td>
    The bordered text
  </td></tr></table>
 -->
<#macro border>
  <table border=4 cellspacing=0 cellpadding=4><tr><td>
    <#nested>
  </tr></td></table>
</#macro>
<@border>The bordered text</@border>

<!-- <#nested>可多次调用 
    输出:
    Anything.
  Anything.
  Anything.
-->
<#macro do_thrice>
  <#nested>
  <#nested>
  <#nested>
</#macro>
<@do_thrice>
  Anything.
</@do_thrice>

<!-- 如果 不使用nested指令, 嵌套内容将不会输出 -->

```

### 宏指令中的循环变量

```html
<#macro do_thrice>
    <!-- 在这里指定变量的value, -->
  <#nested 1>
  <#nested 2>
  <#nested 3>
</#macro>
<!-- x 相当于 <#list foos as foo>...</#list> 中的 foo), 即变量的name 
    结果:
     1 Anything.
  2 Anything.
  3 Anything.
-->
<@do_thrice ; x> <#-- user-defined directive uses ";" instead of "as" -->
  ${x} Anything.
</@do_thrice>

<!-- 宏指令中可以定义多个循环变量 
    结果:
     1. 0.5
    2. 1
    3. 1.5
    4. 2 Last!
-->
<#macro repeat count>
    <!-- 宏指令定义时, 定义自己的变量 -->
  <#list 1..count as x>
    <#nested x, x/2, x==count>
  </#list>
</#macro>
<!-- 宏命令调用时候, 定义调用时候使用的变量 -->
<@repeat count=4 ; c, halfc, last>
  ${c}. ${halfc}<#if last> Last!</#if>
</@repeat>
```

## 表达式

表达式用于插值/指令参数

*   直接指定值
    *   字符串： "Foo" 或者 'Foo' 或者 "It's \"quoted\"" 或者 'It\'s "quoted"' 或者 r"C:\raw\string"
    *   数字： 123.45
    *   布尔值： true， false
    *   序列： ["foo", "bar", 123.45]； 值域： 0..9, 0..<10 (或 0..!10), 0..
    *   哈希表： {"name":"green mouse", "price":150}
*   检索变量
    *   顶层变量： user
    *   从哈希表中检索数据： user.name， user["name"]
    *   从序列中检索数据： products[5]
    *   特殊变量： .main
*   字符串操作
    *   插值(或连接)： "Hello ${user}!" (或 "Hello " + user + "!")
    *   获取一个字符： name[0]
    *   字符串切分： 包含结尾： name[0..4]，不包含结尾： name[0..<5]，基于长度(宽容处理)： name[0..*5]，去除开头： name[5..]
*   序列操作
    *   连接： users + ["guest"]
    *   序列切分：包含结尾： products[20..29]， 不包含结尾： products[20..<30]，基于长度(宽容处理)： products[20..*10]，去除开头：products[20..]
*   哈希表操作
    *   连接： passwords + { "joe": "secret42" }
*   算术运算： (x * 1.5 + 10) / 2 - y % 100
*   比较运算： x == y， x != y， x < y， x > y， x >= y， x <= y， x lt y， x lte y， x gt y， x gte y， 等等。。。。。。
*   逻辑操作： !registered && (firstVisit || fromEurope)
*   内建函数： name?upper_case, path?ensure_starts_with('/')
*   方法调用： repeat("What", 3)
*   处理不存在的值：
    *   默认值： name!"unknown" 或者 (user.name)!"unknown" 或者 name! 或者 (user.name)!
    *   检测不存在的值： name?? 或者 (user.name)??
*   赋值操作： =, +=, -=, *=, /=, %=, ++, --


# 处理不存在的变量   

这里可分为2种情况, 变量不存在or变量值为null. freemaker一视同仁

可以为变量设置默认值:

```html
<h1>Welcome ${user!"visitor"}!</h1>
```

`${user!"visitor"}`表示如果user变量值丢失, 用visitor代替; 

还可以和if指令结合使用

```html
<#if user??><h1>Welcome ${user}!</h1></#if>
```

如果碰到了多级访问的变量, 如`animals.python.price!0`, 如果 animals 或 python 不存在， 那么模板处理过程将会以"未定义的变量"错误而停止, 当且仅当 animals.python 永远存在， 而仅仅最后一个子变量 price 可能不存在时用默认值0代替最终值, 其他情况均报错程序终止;合适的写法是: `(animals.python.price)!0`, animals 或 python 不存在时， 表达式的结果是 0; 

对于`??` 多级访问变量的处理类似


# 常见错误

## 指令体内部变量无需${}

用户所犯的一个常见错误是将插值放在了不需要/不应该使用的地方。 插值 仅 在 文本 区 中有效。(比如， `<h1>Hello ${name}!</h1>`) 还有在字符串值中 (比如， `<#include "/footer/${company}.html">`)。 典型的 错误 使用是 <#if ${big}>...</#if>， 这会导致语法错误。简单写为 <#if big>...</#if>即可。 而且， <#if "${big}">...</#if> 也是 错误的， 因为它将参数值转换为字符串，但是 if 指令只接受布尔值， 那么这将导致运行时错误。

## 数字千分位格式化

 因为很多地区使用分组(千分位分隔符)，那么 "someUrl?id=" + id 就可能会是 "someUrl?id=1 234"。 要预防这种事情的发生，请使用 `?c `(对计算机来说)内建函数，那么在 `"someUrl?id=" + id?c `或 `"someUrl?id=${id?c}"`中， 就会得到如 "someUrl?id=1234" 这样的输出， 而不管本地化和格式的设置是什么。



# 程序开发

## freemaker对象的包装

数据模型中，可以使用基本的Java集合类作为变量，因为这些变量会在内部被替换为适当的 TemplateModel 类型。这种功能特性被称作是 对象包装; 对象包装功能可以透明地把 任何 类型的对象转换为实现了 TemplateModel 接口类型的实例;

编写一个简单的模版, 只有插值

test.ftl

```html
hello ${user.name}, your job is ${user.job}, your school is ${user.school}

And your addr is ${user.addr}
```

启动类:
CodeGenerator.java

```java
public class CodeGenerator {

    public static void main(String[] args) throws Exception {
        String classpathWithSeparator = new CodeGenerator().getClasspath();
        Configuration cfg = new Configuration(Configuration.VERSION_2_3_27);
        try {
            cfg.setDirectoryForTemplateLoading(new File(classpathWithSeparator + "codetpl"));
            
        } catch (IOException e) {
            throw new TplDirNotExistException("模版目录不存在", e);
        }
        cfg.setDefaultEncoding("UTF-8");
        cfg.setTemplateExceptionHandler(TemplateExceptionHandler.RETHROW_HANDLER);
        
        Template tpl = null;
        try {
            tpl = cfg.getTemplate("test.ftl");
        } catch (IOException e) {
            throw new TplFileNotExistException("模版文件不存在", e);
        }
        OutputStreamWriter writer = new OutputStreamWriter(System.out);
        
        User user = new User();
        user.setAddr("上海");
        user.setJob("程序员");
        user.setName("谢大脚");
        user.setSchool("象牙山小学");
        HashMap<String, Object> map = new HashMap<String, Object>();
        map.put("user", user);
        tpl.process(map, writer);
    }
    
    public String getClasspath() {
        return this.getClass()
                .getClassLoader()
                .getResource("")
                .getPath().toString();
    }
    
}
```

此外还有一些自定义exception, bean, 这里省略了, 结果如下:

```
20:12:53.854 [main] DEBUG freemarker.cache - Couldn't find template in cache for "test.ftl"("zh_CN", UTF-8, parsed); will try to load it.
20:12:53.854 [main] DEBUG freemarker.cache - TemplateLoader.findTemplateSource("test_zh_CN.ftl"): Not found
20:12:53.854 [main] DEBUG freemarker.cache - TemplateLoader.findTemplateSource("test_zh.ftl"): Not found
20:12:53.854 [main] DEBUG freemarker.cache - TemplateLoader.findTemplateSource("test.ftl"): Found
20:12:53.854 [main] DEBUG freemarker.cache - Loading template for "test.ftl"("zh_CN", UTF-8, parsed) from "D:\\repo\\eclipse\\ssm-demo-yml\\target\\classes\\codetpl\\test.ftl"
hello 谢大脚, your job is 程序员, your school is 象牙山小学

And your addr is 上海
``` 

但是在开发自定义方法or命令时, 需要使用到freemaker中的包装对象, 模板中可用的变量都是实现了 freemarker.template.TemplateModel 接口的Java对象 

 顶层接口`freemarker.template.TemplateModel` 的子接口对应每种基本变量类型(`TemplateHashModel` 对应哈希表，` TemplateSequenceModel` 对应序列， `TemplateNumberModel` 对应数字, `TemplateScalarModel`对应字符串等等)。
 
 比如，想为模板使用 java.sql.ResultSet 变量作为一个序列，那么就需要编写一个` TemplateSequenceModel `的实现类，这个类要能够读取 java.sql.ResultSet 中的内容。即TemplateModel 的实现类 中维护一个ResultSet私有变量; 请注意一个类可以实现多个 TemplateModel 接口；这就是为什么FTL变量可以有多种类型;

 freemaker已经实现了常用的变量类型: SimpleScalar(String), SimpleHash (Map)等等


## Configuration 配置

配置(configuration)就是 freemarker.template.Configuration 对象， 它存储了常用(全局，应用程序级)的设置，定义了想要在所有模板中可用的变量(称为共享变量)。 而且，它会处理 Template 实例的新建和缓存

### 共享变量

Shared variables (共享变量)是为所有模板定义的变量。可以使用 setSharedVariable 方法向配置中添加共享变量：

```java
Configuration cfg = new Configuration(Configuration.VERSION_2_3_22);
...
cfg.setSharedVariable("warp", new WarpDirective());
cfg.setSharedVariable("company", "Foo Inc.");
```

在所有使用这个配置的模板中，名为 wrap 的用户自定义指令和一个名为 company 的字符串将会在数据模型的根root上可见， 那就不用在根哈希表上一次又一次的添加它们。在传递给 Template.process 的 根root对象里的变量将会隐藏同名的共享变量

出于向后兼容的特性，共享变量的集合初始化时 (就是对于新的 Configuration 实例来说)不能为空。 它包含下列用户自定义指令(用户自定义指令使用时需要用 @ 来代替#)：


![](./FreeMarker-note/1.png)

### 三层配置

Settings(配置设置) 是影响FreeMarker行为的已被命名的值。配置设置有很多， 例如：locale，number_format， default_encoding， template_exception_handler。

配置信息可以被想象成3层(Configuration， Template，Environment), 加载顺序由后到前为(优先级由高到低):

![](./FreeMarker-note/2.png)

具体如何配置呢?

#### Configuration层

```java
Configuration myCfg = new Configuration(Configuration.VERSION_2_3_23);
myCfg.setTemplateExceptionHandler(TemplateExceptionHandler.RETHROW_HANDLER);
myCfg.setDefaultEncoding("UTF-8");

```

spring中配置:

```xml
<bean id="freemarkerConfig"
    class="org.springframework.web.servlet.view.freemarker.FreeMarkerConfigurer">
  <property name="freemarkerSettings">
    <props>
      <prop key="incompatible_improvements">2.3.23</prop>
      <prop key="template_exception_handler">rethrow</prop>
      <prop key="default_encoding">UTF-8</prop>
    </props>
  </property>
</bean>
```

#### Template


#### Environment

使用Java API：使用 Environment 对象的setter方法。当然想要在模板执行之前来做，然后当调用 myTemplate.process(...) 时会遇到问题， 因为在内部创建 Environment 对象后立即就执行模板了， 导致没有机会来进行设置。这个问题的解决可以用下面两个步骤进行：
```java
Environment env = myTemplate.createProcessingEnvironment(root, out);
env.setLocale(java.util.Locale.ITALY);
env.setNumberFormat("0.####");
env.process();  // process the template
```

还有一种方法, 但是不推荐:

```html
<#setting locale="it_IT">
<#setting number_format="0.####">
```

### 模板的加载

configuration提供了三个方法, 代表三种方式:

```java
public void setClassForTemplateLoading(Class clazz, String pathPrefix);// 使用: cfg.setClassForTemplateLoading(FreemarkerUtil.class, "/template");    cfg.getTemplate("Base.ftl");

public void setDirectoryForTemplateLoading(File dir) throws IOException;

public void setServletContextForTemplateLoading(Object servletContext, String path);// 基于web project, 参数path表示WebRoot下的目录路径, 如: "/ftl") 就是 /WebRoot/ftl目录
```

从多个位置加载模版: 不使用内建加载器 ,使用一个特殊的加载器---ClassTemplateLoader

```java
FileTemplateLoader ftl1 = new FileTemplateLoader(new File("/tmp/templates"));
FileTemplateLoader ftl2 = new FileTemplateLoader(new File("/usr/data/templates"));
ClassTemplateLoader ctl = new ClassTemplateLoader(getClass(), "");
TemplateLoader[] loaders = new TemplateLoader[] { ftl1, ftl2, ctl };
MultiTemplateLoader mtl = new MultiTemplateLoader(loaders);

cfg.setTemplateLoader(mtl);
```
现在，FreeMarker将会尝试从 /tmp/templates 目录加载模板，如果在这个目录下没有发现请求的模板，它就会继续尝试从 /usr/data/templates 目录下加载，如果还是没有发现请求的模板， 那么它就会使用类加载器来加载模板。


编写自己的模版加载器要注意那些呢?

如果内建的类加载器都不适合使用，那么就需要来编写自己的类加载器了， 这个类需要实现 freemarker.cache.TemplateLoader 接口， 然后将它传递给 Configuration 对象的 setTemplateLoader(TemplateLoader loader)方法。

如果模板需要通过URL访问其他模板，那么就不需要实现 TemplateLoader 接口了，可以选择子接口 freemarker.cache.URLTemplateLoader 来替代， 只需实现 URL getURL(String templateName) 方法即可

模版缓存:

FreeMarker 是会缓存模板的(假设使用 Configuration 对象的方法来创建 Template 对象)。这就是说当调用 getTemplate方法时，FreeMarker不但返回了 Template 对象，而且还会将它存储在缓存中， 当下一次再以相同(或相等)路径调用 getTemplate 方法时， 那么它只返回缓存的 Template 实例， 而不会再次加载和解析模板文件了。

那么开发阶段怎么禁用这个特性? --- 有一个 Configuration 级别的设置被称作"更新延迟", 这个时间就是从上次对某个模板检查更新后，FreeMarker再次检查模板所要间隔的时间。 其默认值是5秒; 设为0即可;


### 错误控制

关于 FreeMarker 发生的异常，可以分为3类:

*   当配置 FreeMarker 时发生异常：典型地情况，就是在应用程序初始化时， 仅仅配置了一次 FreeMarker。在这个过程中，异常就会发生
*   当加载和解析模板时发生异常：调用了 Configuration.getTemplate(...) 方法， FreeMarker就要把模板文件加载到内存中然后来解析它 (除非模板已经在 Configuration 对象中被 缓存 了)。 在这期间, 异常又可以被细分为两类
    *   因模板文件没有找到而发生的 IOException 异常
    *   根据FTL语言的规则，模板文件发生语法错误时会导致 freemarker.core.ParseException异常。当获得 Template对象 (Configuration.getTemplate(...))时， 这种错误就会发生，而不是当执行 (Template.process(...))模板的时候。 这种异常是 IOException 的一个子类。
*   当执行(处理)模板时发生的异常，也就是当调用了 Template.process(...) 方法时会发生的两种异常：
    *   当试图写入输出对象时发生错误而导致的 IOException 异常
    *   执行模板时发生的其它问题而导致的 freemarker.template.TemplatException 异常

根据不同的TemplateException来自定义处理方式:

```java
// 自定义异常处理类
class MyTemplateExceptionHandler implements TemplateExceptionHandler {
    public void handleTemplateException(TemplateException te, Environment env, java.io.Writer out)
            throws TemplateException {
        try {
            out.write("[ERROR: " + te.getMessage() + "]");
        } catch (IOException e) {
            throw new TemplateException("Failed to print error message. Cause: " + e, env);
        }
    }
}

// 设置进configuration
cfg.setTemplateExceptionHandler(new MyTemplateExceptionHandler());
```

这是如果模版文件是这样 `a${"xiaoyu" + badVar}b`, 而 badVar不存在, 输出的内容是这样:

```html
a[ERROR: Expression badVar is undefined on line 1, column 4 in test.ftl.]b
```
整个插值会被跳过.

同样, 如果错误发生在指令调用中的参数计算时, 整个指令会被跳过, 如: 模版`a<#if badVar>Foo</#if>b`, 输出是
```html
a[ERROR: Expression badVar is undefined on line 1, column 7 in test.ftl.]b
```

如果错误发生在已经开始执行的指令之后，那么指令调用将不会被跳过, 如: 

```html
a
<#if true>
  Foo
  ${badVar}
  Bar
</#if>
c
```
输出是这样的: 
```html
a
  Foo
  [ERROR: Expression badVar is undefined on line 4, column 5 in test.ftl.]
  Bar
c
```

FreeMarker 本身带有这些预先编写的错误控制器:

*   `TemplateExceptionHandler.DEBUG_HANDLER`： 打印堆栈跟踪信息(包括FTL错误信息和FTL堆栈跟踪信息)和重新抛出的异常。 这是默认的异常控制器(也就是说，在所有新的 Configuration 对象中，它是初始化好的)。

*   `TemplateExceptionHandler.HTML_DEBUG_HANDLER`： 和 DEBUG_HANDLER 相同，但是它可以格式化堆栈跟踪信息， 那么就可以在Web浏览器中来阅读错误信息。 在制作HTML页面时，建议使用它而不是 DEBUG_HANDLER。

*   `TemplateExceptionHandler.IGNORE_HANDLER`： 忽略所有异常, FreeMarker 仍然会写日志。 它对处理异常没有任何作用，也不会重新抛出异常。

*   `TemplateExceptionHandler.RETHROW_HANDLER`： 简单重新抛出所有异常而不会做其它的事情。 这个控制器对Web应用程序来说非常好， 因为它在生成的页面发生错误的情况下，给了你很多对Web应用程序的控制权 (因为FreeMarker不向输出中打印任何关于该错误的信息)。 要获得更多在Web应用程序中处理错误的信息


## 通过java开发自定义方法

开发这样一个方法: indexOf(arg1, arg2), 返回字符串arg1在arg2中的位置索引

```java
public class IndexOfMethod implements TemplateMethodModelEx {

    @Override
    public Object exec(List args) throws TemplateModelException {
        if (args.size() != 2) {
            throw new TemplateModelException("参数个数不对.");
        }
        else {
            Object obj1 = args.get(0);
            if (obj1 instanceof SimpleScalar) {
                SimpleScalar sca1 = (SimpleScalar) obj1;
                
                Object obj2 = args.get(1);
                if (obj2 instanceof SimpleScalar) {
                    SimpleScalar sca2 = (SimpleScalar) obj2;
                    return sca1.toString().indexOf(sca2.toString());
                }
                else {
                    throw new TemplateModelException("参数[2]类型不对.");
                }
            } 
            else {
                throw new TemplateModelException("参数[1]类型不对.");
            }
        }
    }

}

// 当然需要将开发的方法传到数据模型中, 如果需要访问FTL运行时环境(读/写变量，获取本地化信息等)，则可以使用 Environment.getCurrentEnvironment() 来获取。
// 启动类基础上增加:
map.put("indexOf", new IndexOfMethod());
```

在test.ftl中使用

```html
<#assign x = "something">
${indexOf("m", x)}
${indexOf("foo", x)}
```

## 通过java开发自定义指令

```java

/**
 * 转大写指令
 *
 * @version 0.1
 * @author xy
 * @date 2018年2月2日 下午10:03:59
 */
public class UpperDirective implements TemplateDirectiveModel {
    
    private Logger logger = LoggerFactory.getLogger(UpperDirective.class);

    @Override
    public void execute(Environment env, Map params, TemplateModel[] loopVars, TemplateDirectiveBody body)
            throws TemplateException, IOException {
        logger.debug("xy---------UpperDirective: execute()");
        
        if (!params.isEmpty()) {
            throw new TemplateModelException("不允许参数");
        }
        if (loopVars.length != 0) {
            throw new TemplateModelException("不允许循环变量");
        }
        if (body != null) {
            body.render(new Writer() {// handle the body content
                
                Writer out = env.getOut();// 通过Environment获取在启动类中设置的writter
                
                @Override
                public void write(char[] cbuf, int off, int len) throws IOException {
                    // transfer
                    char[] rst = new char[len];
                    for(int i=0; i<len; i++) {
                        rst[i] = Character.toUpperCase(cbuf[i + off]);
                    }
                    
                    out.write(rst);
                }
                
                @Override
                public void flush() throws IOException {
                    out.flush();
                }
                
                @Override
                public void close() throws IOException {
                    out.close();
                }
            });
        } 
        else {
            throw new RuntimeException("缺失嵌套内容");
        }
        
        
    }

}

// 启动类基础上添加:
map.put("upper", new UpperDirective());


```

这里是直接将指令添加进了数据模型中, 但更好的做法是将常用的指令作为 共享变量 放到 Configuration 中

当然也可以使用 内建函数new 将指令放到一个FTL库(宏的集合，就像在模板中， 使用 include 或 import )中：

```html
<#-- Maybe you have directives that you have implemented in FTL -->
<#macro something>
  ...
</#macro>

<#-- Now you can't use <#macro upper>, but instead you can: -->
<#assign upper = "com.example.UpperDirective"?new()>

```

模版中使用:

```html
foo
<@upper>
  bar
  <#-- All kind of FTL is allowed here -->
  <#list ["red", "green", "blue"] as color>
    ${color}
  </#list>
  baaz
</@upper>
wombat
```

结果:
```
foo
  BAR
    RED
    GREEN
    BLUE
  BAAZ
wombat

```

下面来个复杂点的例子: 创建一个指令，这个指令可以一次又一次地执行其中的嵌套内容， 这个次数由指定的数字来确定(就像 list 指令)， 可以使用<hr>将输出的重复内容分开

模版内容:
```html
<#assign x = 1>

<@repeat count=4>
  Test ${x}
  <#assign x++>
</@repeat>

<@repeat count=3 hr=true>
  Test
</@repeat>

<@repeat count=3; cnt>
  ${cnt}. Test
</@repeat>
```

预期结果:
```
  Test 1
  Test 2
  Test 3
  Test 4

  Test
<hr>  Test
<hr>  Test

  1. Test
  2. Test
  3. Test
```

开发指令:

```java
/**
 * repeat directive
 *
 * @version 0.1
 * @author xy
 * @date 2018年2月3日 下午1:49:20
 */
public class RepeatDirective implements TemplateDirectiveModel {
    
    private final Logger logger = LoggerFactory.getLogger(RepeatDirective.class);
    private static final String PARAM_NAME_COUNT = "count";
    private static final String PARAM_NAME_HR = "hr";

    @SuppressWarnings("rawtypes")
    @Override
    public void execute(Environment env, Map params, TemplateModel[] loopVars, TemplateDirectiveBody body)
            throws TemplateException, IOException {
        logger.debug("xy-----------RepeatDirective: execute()");
        
        int count = 0;
        boolean hr = false;
        
        boolean countSetup = false; // count 参数是否设置
        
        if (loopVars.length > 1) {
            throw new TemplateModelException("循环变量个数只能为[1]");
        }
        if (params.isEmpty()) {
            throw new TemplateModelException("必须有参数");
        }
        else {
            Iterator iter = params.entrySet().iterator();
            while(iter.hasNext()) {
                Entry ent = (Entry) iter.next();
                Object keyObj = ent.getKey();
                String key = (String) keyObj;
                Object valueObj = ent.getValue();
                TemplateModel value = (TemplateModel) valueObj;
                
                if (PARAM_NAME_COUNT.equals(key)) {
                    if (value instanceof TemplateNumberModel) {
                        count = ((TemplateNumberModel) value).getAsNumber().intValue();
                        countSetup = true;
                        
                        if (count < 0) {
                            throw new TemplateModelException("参数["+key+"]不可以是负数");
                        }
                    }
                    else {
                        throw new TemplateModelException("参数["+key+"]类型不是number.");
                    }
                }
                else if (PARAM_NAME_HR.equals(key)) {
                    if (value instanceof TemplateBooleanModel) {
                        hr = ((TemplateBooleanModel) value).getAsBoolean();
                    }
                }
                else {
                    throw new TemplateModelException("不支持的参数:["+key+"]");
                }
                
            }
            if (!countSetup) {
                throw new TemplateModelException("参数["+PARAM_NAME_COUNT+"]未设置");
            }
            
            if (body != null) {
                Writer out = env.getOut();
                for(int i=0; i<count; i++) {
                    if (loopVars.length == 1) {
                        loopVars[0] = new SimpleNumber(i + 1);
                    }
                    if (hr && i != 0) {
                        out.write("<hr>");
                    }
                    
                    body.render(out);
                }
            } 
            else {
                throw new RuntimeException("body 缺失.");
            }
        }
    }
}

```
