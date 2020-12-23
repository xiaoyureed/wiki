---
title: Regular Expression regex
date: 2014-03-25 22:13:07
tags: [regex]
categories: tool
---

<div align="center">
正则表达式, js
Java中的正则：https://www.cnblogs.com/chanshuyi/p/9197164.html
</div>

<!-- more -->

<!-- TOC -->

- [匹配字符](#匹配字符)
  - [横向匹配 量词](#横向匹配-量词)
  - [纵向匹配 字符组](#纵向匹配-字符组)
  - [贪婪/惰性匹配](#贪婪惰性匹配)
  - [多选](#多选)
- [匹配位置](#匹配位置)
  - [位置的理解](#位置的理解)
  - [锚字符](#锚字符)
  - [案例](#案例)
- [括号的作用](#括号的作用)
  - [分组 分支结构](#分组-分支结构)
  - [通过API引用分组](#通过api引用分组)
    - [提取数据](#提取数据)
    - [替换](#替换)
  - [正则内引用分组](#正则内引用分组)
  - [非捕获分组](#非捕获分组)
  - [先行(否定)断言](#先行否定断言)
  - [案例](#案例-1)
- [匹配原理--回溯](#匹配原理--回溯)
- [拆分](#拆分)
- [实践](#实践)
  - [四种操作](#四种操作)
    - [验证](#验证)
    - [切分](#切分)
    - [提取](#提取)
    - [替换](#替换-1)
  - [相关API注意](#相关api注意)
    - [search和match的参数问题](#search和match的参数问题)
    - [match返回结果的格式问题](#match返回结果的格式问题)
    - [exec比match更强大](#exec比match更强大)
    - [修饰符g，对exex和test的影响](#修饰符g对exex和test的影响)
    - [split相关注意事项](#split相关注意事项)
    - [replace 功能强大](#replace-功能强大)
    - [regex.source属性](#regexsource属性)
  - [案例](#案例-2)
- [Java 中的正则](#java-中的正则)

<!-- /TOC -->

# 匹配字符

## 横向匹配 量词

一个正则可匹配的字符串的长度不是固定的  

```js
var regex = /ab{2,5}c/g;
var string = "abc abbc abbbc abbbbc abbbbbc abbbbbbc";
console.log( string.match(regex) ); 
// => ["abbc", "abbbc", "abbbbc", "abbbbbc"]
```

**{m,}** : 表示至少出现m次。  
**{m}** 等价于{m,m}，表示出现m次。  
**?** 等价于{0,1}，表示出现或者不出现。记忆方式：问号的意思表示，有吗？  若跟在两次之后, 表示懒惰匹配
+: 等价于{1,}，表示出现至少一次。记忆方式：加号是追加的意思，得先有一个，然后才考虑追加。  
**\*** 等价于{0,}，表示出现任意次，有可能不出现。记忆方式：看看天上的星星，可能一颗没有，可能零散有几颗，可能数也数不过来。  


## 纵向匹配 字符组

一个正则匹配的字符串，具体到某一位字符时，它可以不是某个确定的字符，可以有多种可能

```js
var regex = /a[123]b/g;
var string = "a0b a1b a2b a3b a4b";
console.log( string.match(regex) ); 
// => ["a1b", "a2b", "a3b"]
```

[123456abcdefGHIJKLM]，可以写成[1-6a-fG-M]  

匹配“a”、“-”、“z”这三者中任意一个字符：[-az]或[az-]或[a\-z]。即要么放在开头，要么放在结尾，要么转义。总之不会让引擎认为是范围表示法就行了  


```
x|y , x 或 y

[^abc]，反向匹配表示是一个除”a”、”b”、”c”之外的任意一个字符  

\d 数字字符匹配。等效于 [0-9]
\D 非数字字符匹配。等效于 [^0-9]

\s
匹配任何空白字符，包括空格、制表符、换页符等。与 [ \f\n\r\t\v] 等效
\S
匹配任何非空白字符。与 [^ \f\n\r\t\v] 等效

\w
匹配任何字类字符，包括下划线。与"[A-Za-z0-9_]"等效
\W
与任何非单词字符匹配。与"[^A-Za-z0-9_]"等效

\num
匹配 num，此处的 num 是一个正整数。到捕获匹配的反向引用。例如，"(.)\1"匹配两个连续的相同字符。

```


## 贪婪/惰性匹配

通过在量词后面加个问号就能实现惰性匹配  
默认是贪婪匹配，它会尽可能多的匹配。你提供给我我6个满足条件的字符，我就要匹配6个。你能给我3个字符，我就3。反正只要在能力范围内，越多越好

```js
var s = 'aaa';
s.match(/a+/) // ["aaa"]

// =========================

var regex = /\d{2,5}/g;
var string = "123 1234 12345 123456";
console.log( string.match(regex) ); 
// => ["123", "1234", "12345", "12345"]
```

而惰性匹配，就是尽可能少的匹配, 一旦条件满足，就不再往下匹配。

如果想将贪婪模式改为非贪婪模式，可以在量词符后面加一个问号


*?：表示某个模式出现0次或多次，匹配时采用非贪婪模式。
+?：表示某个模式出现1次或多次，匹配时采用非贪婪模式。

```js
var s = 'aaa';
s.match(/a+?/) // ["a"]

// =========================

var regex = /\d{2,5}?/g;
var string = "123 1234 12345 123456";
console.log( string.match(regex) ); 
// => ["12", "12", "34", "12", "34", "12", "34", "56"]
```
其中`/\d{2,5}?/`表示，虽然2到5次都行，当2个就够的时候，就不在往下尝试了。

## 多选

(p1|p2|p3)，其中p1、p2和p3是子模式，用|（管道符）分隔，表示其中任何之一。  

分支结构也是惰性的，即当前面的匹配上了，后面的就不再尝试了  

```js
var regex = /good|nice/g;
var string = "good idea, nice try.";
console.log( string.match(regex) ); 
// => ["good", "nice"]
```

# 匹配位置

## 位置的理解

位置是相邻字符之间的位置，相当于一个空字符串`""`  

## 锚字符

* **`^`**
* **`$`**
* **`\b`**
* **`\B`** (包括单词中间的位置)
* **`(?=p)`**: 其中p是一个子模式，即p前面的位置, 或者这么理解--本位置后的字符需复合p模式
* **`(?!p)`**：与上面相反

```js
var result = "hello".replace(/(?=l)/g, '#');
console.log(result); 
// => "he#l#lo"

var result = "hello".replace(/(?!l)/g, '#');
console.log(result); 
// => "#h#ell#o#"
```

## 案例

1. 不匹配任何东西

/.^/

2. 数字的千位分隔符表示法

```js
var string = "12345678 123456789",
reg = /(?!\b)(?=(\d{3})+\b)/g;
var result = string.replace(reg, ',')
console.log(result); 
// => "12,345,678 123,456,789"
```

其中(?!\b)怎么理解呢？  
要求当前是一个位置，但不是\b前面的位置，其实(?!\b)说的就是\B。  
因此最终正则变成了：`/\B(?=(\d{3})+\b)/g。  `

3. 验证密码问题

密码长度6-12位，由数字、小写字符和大写字母组成，但必须至少包括2种字符  

```js
var reg = /((?=.*[0-9])(?=.*[a-z])|(?=.*[0-9])(?=.*[A-Z])|(?=.*[a-z])(?=.*[A-Z]))^[0-9A-Za-z]{6,12}$/;
console.log( reg.test("1234567") ); // false 全是数字
console.log( reg.test("abcdef") ); // false 全是小写字母
console.log( reg.test("ABCDEFGH") ); // false 全是大写字母
console.log( reg.test("ab23C") ); // false 不足6位
console.log( reg.test("ABCDEF234") ); // true 大写字母和数字
console.log( reg.test("abcdEF234") ); // true 三者都有

//或者
var reg = /(?!^[0-9]{6,12}$)(?!^[a-z]{6,12}$)(?!^[A-Z]{6,12}$)^[0-9A-Za-z]{6,12}$/;
console.log( reg.test("1234567") ); // false 全是数字
console.log( reg.test("abcdef") ); // false 全是小写字母
console.log( reg.test("ABCDEFGH") ); // false 全是大写字母
console.log( reg.test("ab23C") ); // false 不足6位
console.log( reg.test("ABCDEF234") ); // true 大写字母和数字
console.log( reg.test("abcdEF234") ); // true 三者都有
```

# 括号的作用

括号提供了分组，便于我们引用它  

## 分组 分支结构

```js
var regex = /(ab)+/g;
var string = "ababa abbb ababab";
console.log( string.match(regex) ); 
// => ["abab", "ab", "ababab"]
// 此时分组内容没有被捕获
// 没有返回使用组匹配时，不宜同时使用g修饰符，否则match方法不会捕获分组的内容

var regex = /^I love (JavaScript|Regular Expression)$/;
console.log( regex.test("I love JavaScript") );
console.log( regex.test("I love Regular Expression") );
// => true
// => true
```

## 通过API引用分组

### 提取数据

提取年月日：

```js
var regex = /(\d{4})-(\d{2})-(\d{2})/;
var string = "2017-06-12";
var arr = string.match(regex);//regex.exec(string)效果相同
console.log( arr ); 
// => ["2017-06-12", "2017", "06", "12", index: 0, input: "2017-06-12"]
console.log(arr[4]);//=> undefined

```

也可以使用构造函数的全局属性$1至$9来获取：

```js
var regex = /(\d{4})-(\d{2})-(\d{2})/;
var string = "2017-06-12";
regex.test(string); // 正则操作即可，例如
//regex.exec(string);
//string.match(regex);
console.log(RegExp.$1); // "2017"
console.log(RegExp.$2); // "06"
console.log(RegExp.$3); // "12"
```

### 替换

想把yyyy-mm-dd格式，替换成mm/dd/yyyy怎么做？

```js
var regex = /(\d{4})-(\d{2})-(\d{2})/;
var string = "2017-06-12";
var result = string.replace(regex, "$2/$3/$1");
//等价于如下表达：
//var result = string.replace(regex, function() {
//	return RegExp.$2 + "/" + RegExp.$3 + "/" + RegExp.$1;
//});
//也等价于
//var result = string.replace(regex, function(match, year, month, day) {
//	return month + "/" + day + "/" + year;
//});
console.log(result); 
// => "06/12/2017"
```

## 正则内引用分组

除了使用相应API来引用分组，也可以在正则本身里引用分组。但只能引用之前出现的分组，如：

    需要满足三种格式的日期：

    2016-06-12
    2016/06/12
    2016.06.12

`var regex = /\d{4}(-|\/|\.)\d{2}\1\d{2}/`  

`\1`即第一个分组，表示的引用之前的那个分组`(-|\/|\.)`。不管它匹配到什么（比如-），`\1`都匹配那个同样的具体某个字符。  

1. 括号嵌套的情况

以左括号（开括号）为准。比如: `/^((\d)(\d(\d)))\1\2\3\4$/`  

```js
var regex = /^((\d)(\d(\d)))\1\2\3\4$/;
var string = "1231231233";
console.log( regex.test(string) ); // true
console.log( RegExp.$1 ); // 123
console.log( RegExp.$2 ); // 1
console.log( RegExp.$3 ); // 23
console.log( RegExp.$4 ); // 3
```

第一个字符是数字，比如说1，  
第二个字符是数字，比如说2，  
第三个字符是数字，比如说3，  
接下来的是\1，是第一个分组内容，那么看第一个开括号对应的分组是什么，是123，  
接下来的是\2，找到第2个开括号，对应的分组，匹配的内容是1，  
接下来的是\3，找到第3个开括号，对应的分组，匹配的内容是23，  
最后的是\4，找到第3个开括号，对应的分组，匹配的内容是3。 

2. `\10`表示第十个分组，不是表示第一个分组后跟零字符


## 非捕获分组

如果只想要括号最原始的功能，但不会引用它，即，既不在API里引用，也不在正则里反向引用。此时可以使用非捕获分组`(?:p)`, p为表达式

非捕获组的作用请考虑这样一个场景，假定需要匹配foo或者foofoo，正则表达式就应该写成/(foo){1, 2}/，但是这样会占用一个组匹配。这时，就可以使用非捕获组，将正则表达式改为/(?:foo){1, 2}/，它的作用与前一个正则是一样的，但是不会单独输出括号内部的内容。

```js
var regex = /(?:ab)+/g;
var string = "ababa abbb ababab";
console.log( string.match(regex) ); 
// => ["abab", "ab", "ababab"]

// =====================

var m = 'abc'.match(/(?:.)b(.)/);
m // ["abc", "c"]
// 上面代码中的模式，一共使用了两个括号。其中第一个括号是非捕获组，所以最后返回的结果中没有第一个括号，只有第二个括号匹配的内容。

//======================

// 正常匹配
var url = /(http|ftp):\/\/([^/\r\n]+)(\/[^\r\n]*)?/;

url.exec('http://google.com/');
// ["http://google.com/", "http", "google.com", "/"]

// 非捕获组匹配
var url = /(?:http|ftp):\/\/([^/\r\n]+)(\/[^\r\n]*)?/;

url.exec('http://google.com/');
// ["http://google.com/", "google.com", "/"]
// 上面的代码中，前一个正则表达式是正常匹配，第一个括号返回网络协议；后一个正则表达式是非捕获匹配，返回结果中不包括网络协议。
```

## 先行(否定)断言

x(?=y)称为先行断言（Positive look-ahead），x只有在y前面才匹配，y不会被计入返回结果。比如，要匹配后面跟着百分号的数字，可以写成/\d+(?=%)/。

“先行断言”中，括号里的部分是不会返回的。

x(?!y)称为先行否定断言（Negative look-ahead），x只有不在y前面才匹配，y不会被计入返回结果。比如，要匹配后面跟的不是百分号的数字，就要写成/\d+(?!%)/。

“先行否定断言”中，括号里的部分是不会返回的。

```js
var m = 'abc'.match(/b(?=c)/);
m // ["b"]

//=====================

/\d+(?!\.)/.exec('3.14')
// ["14"]

```

## 案例

1. 字符串trim方法模拟

第一种，匹配到开头和结尾的空白符，然后替换成空字符,效率较第二种高

```js
function trim(str) {
	return str.replace(/^\s+|\s+$/g, '');
}
console.log( trim("  foobar   ") ); 
// => "foobar"
```

第二种，匹配整个字符串，然后用引用来提取出相应的数据

```js
function trim(str) {
    //这里使用了惰性匹配*?，不然也会匹配最后一个空格之前的所有空格的。
	return str.replace(/^\s*(.*?)\s*$/g, "$1");
}
console.log( trim("  foobar   ") ); 
// => "foobar"
```

2. 每个单词的首字母转大写

```js
function titleize(str) {
	return str.toLowerCase().replace(/(?:^|\s)\w/g, function(c) {
		return c.toUpperCase();
	});
}
console.log( titleize('my name is epeli') ); 
// => "My Name Is Epeli"
```

3. 驼峰化

```js
function camelize(str) {
	return str.replace(/[-_\s]+(.)?/g, function(match, c) {
		return c ? c.toUpperCase() : '';
	});
}
console.log( camelize('-moz-transform') ); 
// => "MozTransform"
```

其中分组(.)表示首字母。单词的界定是，前面的字符可以是多个连字符、下划线以及空白符。正则后面的?的目的，是为了应对str尾部的字符可能不是单词字符，比如str是’-moz-transform  ‘

4. 中划线化---驼峰化逆过程

```js
function dasherize(str) {
	return str.replace(/([A-Z])/g, '-$1').replace(/[-_\s]+/g, '-').toLowerCase();
}
console.log( dasherize('MozTransform') ); 
// => "-moz-transform"
```

# 匹配原理--回溯

整个匹配过程：  

1. 编译
2. 设定起始位置
3. 尝试匹配
4. 匹配失败的话，从下一位开始继续第3步
5. 最终结果：匹配成功或失败

简单总结就是，正因为有多种可能，所以要一个一个试。直到，要么到某一步时，整体匹配成功了；要么最后都试完后，发现整体匹配不成功。  

贪婪量词“试”的策略是：买衣服砍价。价钱太高了，便宜点，不行，再便宜点。  
惰性量词“试”的策略是：卖东西加价。给少了，再多给点行不，还有点少啊，再给点。  
分支结构“试”的策略是：货比三家。这家不行，换一家吧，还不行，再换。  

# 拆分

操作符优先级：（由高到低）  

1. 转义符 \
2. 括号和方括号 (...)、(?:...)、(?=...)、(?!...)、[...]
3. 量词限定符 {m}、{m,n}、{m,}、?、*、+
4. 位置和序列 ^ 、$、 \元字符、 一般字符
5. 管道符（竖杠）|


# 实践

## 四种操作

### 验证

常用test，

```js
var regex = /\d/;
var string = "abc123";
console.log( regex.test(string) );
// => true
```

### 切分

```js
var regex = /\D/;
console.log( "2017/06/26".split(regex) );
console.log( "2017.06.26".split(regex) );
console.log( "2017-06-26".split(regex) );
// => ["2017", "06", "26"]
// => ["2017", "06", "26"]
// => ["2017", "06", "26"]
```

### 提取

虽然整体匹配上了，但有时需要提取部分匹配的数据。  

```js
//match--常用
var regex = /^(\d{4})\D(\d{2})\D(\d{2})$/;
var string = "2017-06-26";
console.log( string.match(regex) );
// =>["2017-06-26", "2017", "06", "26", index: 0, input: "2017-06-26"]

//exec
var regex = /^(\d{4})\D(\d{2})\D(\d{2})$/;
var string = "2017-06-26";
console.log( regex.exec(string) );
// =>["2017-06-26", "2017", "06", "26", index: 0, input: "2017-06-26"]

//test
var regex = /^(\d{4})\D(\d{2})\D(\d{2})$/;
var string = "2017-06-26";
regex.test(string);
console.log( RegExp.$1, RegExp.$2, RegExp.$3 );
// => "2017" "06" "26"

//search
var regex = /^(\d{4})\D(\d{2})\D(\d{2})$/;
var string = "2017-06-26";
string.search(regex);
console.log( RegExp.$1, RegExp.$2, RegExp.$3 );
// => "2017" "06" "26"

//replace
var regex = /^(\d{4})\D(\d{2})\D(\d{2})$/;
var string = "2017-06-26";
var date = [];
string.replace(regex, function(match, year, month, day) {
	date.push(year, month, day);
});
console.log(date);
// => ["2017", "06", "26"]
```

### 替换

比如把日期格式，从yyyy-mm-dd替换成yyyy/mm/dd  

```js
var string = "2017-06-26";
var today = new Date( string.replace(/-/g, "/") );
console.log( today );
// => Mon Jun 26 2017 00:00:00 GMT+0800 (中国标准时间)
```

## 相关API注意

### search和match的参数问题

search和match，会把字符串转换为正则   

```js
var string = "2017.06.27";
console.log( string.search(".") );
// => 0
//需要修改成下列形式之一
console.log( string.search("\\.") );
console.log( string.search(/\./) );
// => 4
// => 4
console.log( string.match(".") );
// => ["2", index: 0, input: "2017.06.27"]
//需要修改成下列形式之一
console.log( string.match("\\.") );
console.log( string.match(/\./) );
// => [".", index: 4, input: "2017.06.27"]
// => [".", index: 4, input: "2017.06.27"]
console.log( string.split(".") );
// => ["2017", "06", "27"]
console.log( string.replace(".", "/") );
// => "2017/06.27"
```

### match返回结果的格式问题

match返回结果的格式，与正则对象是否有修饰符g有关  

```js
var string = "2017.06.27";
var regex1 = /\b(\d+)\b/;
var regex2 = /\b(\d+)\b/g;
console.log( string.match(regex1) );
console.log( string.match(regex2) );
// => ["2017", "2017", index: 0, input: "2017.06.27"]
// => ["2017", "06", "27"]
```

没有g，返回的是标准匹配格式，即，数组的第一个元素是整体匹配的内容，接下来是分组捕获的内容，然后是整体匹配的第一个下标，最后是输入的目标字符串。  

有g，返回的是所有匹配的内容。  

当没有匹配时，不管有无g，都返回null。  

### exec比match更强大

当正则没有g时，使用match返回的信息比较多。但是有g后，就没有关键的信息index了。  

而exec方法就能解决这个问题，它能接着上一次匹配后继续匹配  

```js
var string = "2017.06.27";
var regex2 = /\b(\d+)\b/g;
var result;
while ( result = regex2.exec(string) ) {
	console.log( result, regex2.lastIndex );//lastIndex属性，表示下一次匹配开始的位置
}
// => ["2017", "2017", index: 0, input: "2017.06.27"] 4
// => ["06", "06", index: 5, input: "2017.06.27"] 7
// => ["27", "27", index: 8, input: "2017.06.27"] 10
```

### 修饰符g，对exex和test的影响

上面提到了正则实例的lastIndex属性，表示尝试匹配时，从字符串的lastIndex位开始去匹配。  

字符串的四个方法，每次匹配时，都是从0开始的，即lastIndex属性始终不变。  

而正则实例的两个方法exec、test，当正则是全局匹配时，每一次匹配完成后，都会修改lastIndex  

```js
var regex = /a/g;
console.log( regex.test("a"), regex.lastIndex );
console.log( regex.test("aba"), regex.lastIndex );
console.log( regex.test("ababc"), regex.lastIndex );
// => true 1
// => true 3
// => false 0

var regex = /a/;
console.log( regex.test("a"), regex.lastIndex );
console.log( regex.test("aba"), regex.lastIndex );
console.log( regex.test("ababc"), regex.lastIndex );
// => true 0
// => true 0
// => true 0
```

### split相关注意事项

它可以有第二个参数，表示结果数组的最大长度    

正则使用分组时，结果数组中是包含分隔符的  

```js
var string = "html,css,javascript";
console.log( string.split(/,/, 2) );
// =>["html", "css"]

var string = "html,css,javascript";
console.log( string.split(/(,)/) );
// =>["html", ",", "css", ",", "javascript"]
```

### replace 功能强大

```js
"1234 2345 3456".replace(/(\d)\d{2}(\d)/g, function(match, $1, $2, index, input) {
	console.log([match, $1, $2, index, input]);
});
// => ["1234", "1", "4", 0, "1234 2345 3456"]
// => ["2345", "2", "5", 5, "1234 2345 3456"]
// => ["3456", "3", "6", 10, "1234 2345 3456"]
```

### regex.source属性

在构建动态的正则表达式时，可以通过查看该属性，来确认构建出的正则到底是什么：

```js
var className = "high";
var regex = new RegExp("(^|\\s)" + className + "(\\s|$)");
console.log( regex.source )
// => (^|\s)high(\s|$) 即字符串"(^|\\s)high(\\s|$)"
```

## 案例

http://blog.didispace.com/regular-expression-6/

# Java 中的正则
