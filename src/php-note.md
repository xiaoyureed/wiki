---
title: PHP
date: 2014-03-14 13:40:42
tags: [PHP]
categories: computer science
---
<div align="center">
php（PHP: Hypertext Processor）是一种服务器上运行的脚本语言;
php文件包括 文本、html、js、php代码这些元素;
执行后， 返回给browser的结果是html形式的， 默认拓展名".php"
</div>
<!--more-->

<!-- TOC -->

- [环境搭建](#环境搭建)
- [语法](#语法)
- [trait，interface和abstract的异同](#traitinterface和abstract的异同)
- [表单处理](#表单处理)
- [swoole2.0 和协程](#swoole20-和协程)

<!-- /TOC -->

# 环境搭建

reference: [1](https://blog.csdn.net/tojohnonly/article/details/79820471)

也可以使用php内置的server, `php -S localhost:8000 -t D:\_learn\php`, D:/_learn/php即为被代理的文件夹; ([ref](https://www.cnblogs.com/HouJiao/p/4985597.html)); 访问: http://localhost:8000/demo1.php

vscode的debug环境搭建: [ref](https://blog.csdn.net/x356982611/article/details/52664334), debug时候需要server也开启

# 语法

PHP 脚本以 `<?php` 开始，以 `?>` 结束

```php
<!DOCTYPE html> 
<html> 
<body> 

<h1>My first PHP page</h1> 

<?php 

///////////////////////
//
//        echo和print
//
//////////////////////////

echo "Hello world!<br>";
echo "这是一个", "字符串，", "使用了", "多个", "参数。";
$txt1="学习 PHP";
$txt2="RUNOOB.COM";
$cars=array("Volvo","BMW","Toyota");// 数组
echo $txt1;
echo "<br>";
echo "在 $txt2 学习 PHP ";
echo "<br>";
echo "我车的品牌是 {$cars[0]}";

print "xxx";
print "Hello world!<br>";
// echo和print区别: echo - 可以输出一个或多个字符串; print - 只允许输出一个字符串，返回值总为 1
// echo是语言结构(language construct)，而并不是真正的函数，因此不能作为表达式的一部分使用
// echo 输出的速度比 print 快；函数print()打印一个值（它的参数），如果字符串成功显示则返回true，否则返回false

//////////////////////
//
//        EOF 原样打印
//
//////////////////////

// 如果有很多html标签需要打印, 可以使用PHP EOF(heredoc): 按照原样，包括换行格式什么的，输出在其内部的东西
echo <<<EOF
    <h1>我的第一个标题</h1>
    <p>我的第一个段落。</p>
EOF;
// 结束需要独立一行且前后不能空格

// 位于开始标记和结束标记之间的变量可以被正常解析，但是函数则不可以。在heredoc中，变量不需要用连接符.或,来拼接
$name="runoob";
$a= <<<EOF
    "abc"$name
    "123"
EOF;
echo $a; // 结果为 "abc"runoob "123"

//       单行注释
/*
多行
注释
*/

////////////////////////
//
//      变量作用域 四种
//
/////////////////////////
/*
local
global
static
parameter
*/
//------------全局变量-----------

// 变量, 这里都是全局变量， 在函数内部无法访问， 需要通过global访问 
$text="xxyyzz"
$x=5;
$y=6;
$z=$x+$y;
echo $z;

// ------------局部变量----------

function myTest() 
{ 
    $y=10; // 局部变量 
    echo "<p>测试函数内变量:<p>"; 
    echo "变量 x 为: $x"; //没有值, 函数内部无法使用
    echo "<br>"; 
    echo "变量 y 为: $y"; // 有值, 可以直接使用
}  
myTest(); 

echo "<p>测试函数外变量:<p>"; 
echo "变量 x 为: $x"; // 有值
echo "<br>"; 
echo "变量 y 为: $y"; // 没有值

//--------在方法体内部使用全局变量需要先通过global声明------------

$x=5;
$y=10;
function myTest1()
{
    global $x,$y;// 全局变量可以在函数内部如此声明使用
    $y=$x+$y;
    // -----------
    // 效果等同下面, 局变量存储在一个名为 $GLOBALS[index] 的数组中。 index 保存变量的名称
    $GLOBALS['y']=$GLOBALS['x']+$GLOBALS['y'];
}
 
myTest1();
echo $y; // 输出 15

// -----------Static变量, 任然是局部变量------------

// 当一个函数完成时，它的所有变量通常都会被删除。然而，有时候您希望某个局部变量不要被删除。
function myTest()
{
    static $x=0; // 该变量将会保留着函数前一次被调用时的值
    echo $x;
    $x++;
}
 
myTest();
myTest();
myTest();
// 结果是:012

//////////////////
//
//      数据类型
//
/////////////////
// String（字符串）, Integer（整型）, Float（浮点型）, Boolean（布尔型）, Array（数组）, Object（对象）, NULL（空值）

// --------String-----------
$txt1="Hello world!"; 
$txt2="What a nice day!"; 
echo $txt1 . " " . $txt2; // 连接
$text1 .= "xxx" // 等于 $text1 = $text1 . "xxx"
echo strlen("Hello world!"); // 返回长度
echo strpos("Hello world!","world");// 搜寻, 返回6


// ---------integer---------
$x = 5985;
var_dump($x);// 返回变量的数据类型和值
echo "<br>"; 
$x = -345; // 负数 
var_dump($x);
echo "<br>"; 
$x = 0x8C; // 十六进制数
var_dump($x);
echo "<br>";
$x = 047; // 八进制数
var_dump($x);
var_dump(intdiv(10, 3));// 整除, 输出3
// -------- float--------
$x = 10.365;
var_dump($x);
echo "<br>"; 
$x = 2.4e3;
var_dump($x);
echo "<br>"; 
$x = 8E-5;
var_dump($x);
//------------布尔值------
$x=true;
$y=false;
// ----------数组----------
$cars=array("Volvo","BMW","Toyota");
var_dump($cars);

//------object------
class Car
{
  var $color;
  function __construct($color="green") {
    $this->color = $color;
  }
  function what_color() {
    return $this->color;
  }
}

// -------null-----
$x="Hello world!";
$x=null;// 清空数据
var_dump($x);

//////////////
//
//      常量
//
/////////////

// 设置常量，使用 define() 函数, 语法: bool define ( string $name , mixed $value [, bool $case_insensitive = false ] )
// 常量在定义后，默认是全局变量，可以在整个运行的脚本的任何地方使用
// 默认区分大小写的常量名
define("GREETING", "hello!");
echo GREETING;    // 输出 "hello!"
echo '<br>';
echo greeting;   // 输出 "greeting", 如果希望不区分大小写, define("GREETING", "hello!", true);定义即可

// 另种方式
const CONNECT_OK = 1;

/////////////////////
//
//       比较运算符
//
///////////////////

/*
x == y	等于	如果 x 等于 y，则返回 true	5==8 返回 false
x === y	绝对等于	如果 x 等于 y，且它们类型相同，则返回 true	5==="5" 返回 false
x != y	不等于	如果 x 不等于 y，则返回 true	5!=8 返回 true
x <> y	不等于	如果 x 不等于 y，则返回 true	5<>8 返回 true
x !== y	绝对不等于	如果 x 不等于 y，或它们类型不相同，则返回 true	5!=="5" 返回 true
x > y	大于	如果 x 大于 y，则返回 true	5>8 返回 false
x < y	小于	如果 x 小于 y，则返回 true	5<8 返回 true
x >= y	大于等于	如果 x 大于或者等于 y，则返回 true	5>=8 返回 false
x <= y	小于等于	如果 x 小于或者等于 y，则返回 true	5<=8 返回 true
 */

///////////////////////////////
//
//          逻辑运算符 and, or, xor, &&, ||, !
//
///////////////////////////////
/*
x and y	与	如果 x 和 y 都为 true，则返回 true	x=6
y=3 
(x < 10 and y > 1) 返回 true

x or y	或	如果 x 和 y 至少有一个为 true，则返回 true	x=6
y=3 
(x==6 or y==5) 返回 true

x xor y	异或	如果 x 和 y 有且仅有一个为 true，则返回 true	x=6
y=3 
(x==6 xor y==3) 返回 false

x && y	与	如果 x 和 y 都为 true，则返回 true	x=6
y=3
(x < 10 && y > 1) 返回 true

x || y	或	如果 x 和 y 至少有一个为 true，则返回 true	x=6
y=3
(x==5 || y==5) 返回 false

! x	非	如果 x 不为 true，则返回 true	x=6
y=3
!(x==y) 返回 true
*/

//////////////////////
//
//      数组运算符
//
/////////////////////
/*
x + y	集合	x 和 y 的集合
x == y	相等	如果 x 和 y 具有相同的键/值对，则返回 true
x === y	恒等	如果 x 和 y 具有相同的键/值对，且顺序相同类型相同，则返回 true
x != y	不相等	如果 x 不等于 y，则返回 true
x <> y	不相等	如果 x 不等于 y，则返回 true
x !== y	不恒等	如果 x 不等于 y，则返回 true
*/
$x = array("a" => "red", "b" => "green"); 
$y = array("c" => "blue", "d" => "yellow"); 
$z = $x + $y; // union of $x and $y
var_dump($z);
echo "<br>";
var_dump($x == $y);
echo "<br>";
var_dump($x === $y);
echo "<br>";
var_dump($x != $y);
echo "<br>";
var_dump($x <> $y);
echo "<br>";
var_dump($x !== $y);
// 结果:
/*
array(4) { ["a"]=> string(3) "red" ["b"]=> string(5) "green" ["c"]=> string(4) "blue" ["d"]=> string(6) "yellow" } 
bool(false) 
bool(false) 
bool(true) 
bool(true) 
bool(true)
*/

////////////////////
//
//      三元运算符
//
/////////////////////
$test = 'xiaoyu';
// 普通写法
$username = isset($test) ? $test : 'nobody';
echo $username, PHP_EOL; // php_eol 表示换行
 
// PHP 5.3+ 版本写法
$username = $test ?: 'nobody';// 中间的那项省略了, 如果第一项有值返回第一项, 没有则返回nobody
echo $username, PHP_EOL;

// php7+版本进一步简化了
// 判断 $_GET 请求中含有 user 值，如果有返回 $_GET['user']，否则返回 nobody
$username = $_GET['user'] ?? 'nobody';

//////////////////////
//
//      组合比较符, 返回-1 0 1
//
//////////////////////
/*
语法：$a<=>$b
如果$a > $b, 返回的值为1 
如果$a == $b, 返回的值为0 
如果$a < $b, 返回的值为-1
*/
// 整型
echo 1 <=> 1; // 0
echo 1 <=> 2; // -1
echo 2 <=> 1; // 1

// 浮点型
echo 1.5 <=> 1.5; // 0
echo 1.5 <=> 2.5; // -1
echo 2.5 <=> 1.5; // 1
 
// 字符串
echo "a" <=> "a"; // 0
echo "a" <=> "b"; // -1
echo "b" <=> "a"; // 1

/////////////////////
//
//      条件控制    循环
//
///////////////////
/*
支持if(){}, if(){}else{}, if(){}else if(){}else{} 
while(){}, do{}while();, for(){}, foreach($xx as $yy){$yy ...}
*/
$t=date("H");
if ($t<"20")// 如果当前时间小于 20，
{
    echo "Have a good day!";
}



//////////////////////
//
//      数组
//
//////////////////////
$cars=array("Volvo","BMW","Toyota");
echo "I like " . $cars[0] . ", " . $cars[1] . " and " . $cars[2] . ".";
echo count($cars);// 数组长度
/*
有三种类型的数组: 
    * 数值数组 - 带有数字 ID 键的数组
    * 关联数组 - 带有指定的键的数组，每个键关联一个值
    * 多维数组 - 包含一个或多个数组的数组
*/

// ---------数值数组------------------

$cars=array("Volvo","BMW","Toyota");// 自动分配 ID 键（ID 键总是从 0 开始）
// 或者, 人工分配 ID 键：
$cars[0]="Volvo";
$cars[1]="BMW";
$cars[2]="Toyota";

// 遍历数值数组

$cars=array("Volvo","BMW","Toyota");
$arrlength=count($cars);

for($x=0;$x<$arrlength;$x++)
{
echo $cars[$x];
echo "<br>";
}

// -----------关联数组(键值对)---------------

$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
// or
$age['Peter']="35";
$age['Ben']="37";
$age['Joe']="43";

// 遍历关联数组

$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
foreach($age as $x=>$x_value)
{
echo "Key=" . $x . ", Value=" . $x_value;
echo "<br>";
}

////////////////////////////
//
//      数组排序(借助一些内建函数)
//
//////////////////////////
/*
sort() - 对数组进行升序排列

rsort() - 对数组进行降序排列

asort() - 根据关联数组的值，对数组进行升序排列

ksort() - 根据关联数组的键，对数组进行升序排列

arsort() - 根据关联数组的值，对数组进行降序排列

krsort() - 根据关联数组的键，对数组进行降序排列
*/

$cars=array("Volvo","BMW","Toyota");
sort($cars);
print_r($cars);// Array ( [0] => BMW [1] => Toyota [2] => Volvo )

/////////////////////////
//
//      超级全局变量  superglobals
//
////////////////////////
// 在一个脚本的全部作用域中都可用
/*
$GLOBALS: 一个包含了全部变量的全局组合数组。变量的名字就是数组的键

$_SERVER: 是一个包含了诸如头信息(header)、路径(path)、以及脚本位置(script locations)等等信息的数组。这个数组中的项目由 Web 服务器创建。

$_REQUEST 用于收集HTML表单提交的数据。

$_POST  被广泛应用于收集表单数据，在HTML form标签的指定该属性："method="post"

$_GET  同样被广泛应用于收集表单数据，在HTML form标签的指定该属性："method="get"。
        也可以收集URL中发送的数据。

$_FILES

$_ENV

$_COOKIE

$_SESSION
*/

$x = 75; 
$y = 25;
function addition() 
{ 
   $GLOBALS['z'] = $GLOBALS['x'] + $GLOBALS['y']; 
}
addition(); 
echo $z; // 函数外部照样可以访问

/////////////////////////////////////////////////////////////////////

// 当前访问地址: http://localhost:8000/demo1.php
echo $_SERVER['PHP_SELF']; // 当前脚本名称, /demo1.php
echo "<br>";
echo $_SERVER['SERVER_NAME']; // server主机名称, localhost
echo "<br>";
echo $_SERVER['HTTP_HOST'];// 当前请求头中 Host: 项的内容, localhost:8000, 如果header不存在将报错
echo "<br>";
echo $_SERVER['HTTP_REFERER'];// 来源地址, 测试时因为没有这个值, 报错了
echo "<br>";
echo $_SERVER['HTTP_USER_AGENT'];// user agent, Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/66.0.3359.139 Safari/537.36
echo "<br>";
echo $_SERVER['SCRIPT_NAME'];// 脚本名称, /demo1.php


// 对比
echo $_SERVER['SCRIPT_NAME']."<br>";
echo $_SERVER['SCRIPT_FILENAME']."<br>";
echo $_SERVER['PHP_SELF']."<br>";
echo $_SERVER['SERVER_NAME']."<br>";
echo $_SERVER['HTTP_HOST']."<br>";
// 结果:
/*
/welcome/index.php
D:\_learn\php\welcome\index.php
/welcome/index.php
localhost
localhost:8000
*/

/*

一些重要的 $_SERVER 变量

$_SERVER['PHP_SELF']	当前执行脚本的文件名，与 document root 有关。例如，在地址为 http://example.com/test.php/foo.bar 的脚本中使用 $_SERVER['PHP_SELF'] 将得到 /test.php/foo.bar。
$_SERVER['GATEWAY_INTERFACE']	服务器使用的 CGI 规范的版本；例如，"CGI/1.1"。
$_SERVER['SERVER_ADDR']	当前运行脚本所在的服务器的 IP 地址。
$_SERVER['SERVER_NAME']	当前运行脚本所在的服务器的主机名。如果脚本运行于虚拟主机中，该名称是由那个虚拟主机所设置的值决定。(如: www.php.cn)
$_SERVER['SERVER_SOFTWARE']	服务器标识字符串，在响应请求时的头信息中给出。 (如：Apache/2.2.24)
$_SERVER['SERVER_PROTOCOL']	请求页面时通信协议的名称和版本。例如，"HTTP/1.0"。
$_SERVER['REQUEST_METHOD']	访问页面使用的请求方法；例如，"GET", "HEAD"，"POST"，"PUT"。
$_SERVER['REQUEST_TIME']	请求开始时的时间戳。从 PHP 5.1.0 起可用。 (如：1377687496)
$_SERVER['QUERY_STRING']	query string（查询字符串），如果有的话，通过它进行页面访问。
$_SERVER['HTTP_ACCEPT']	当前请求头中 Accept: 项的内容，如果存在的话。
$_SERVER['HTTP_ACCEPT_CHARSET']	当前请求头中 Accept-Charset: 项的内容，如果存在的话。例如："iso-8859-1,*,utf-8"。
$_SERVER['HTTP_HOST']	当前请求头中 Host: 项的内容，如果存在的话。
$_SERVER['HTTP_REFERER']	引导用户代理到当前页的前一页的地址（如果存在）。由 user agent 设置决定。并不是所有的用户代理都会设置该项，有的还提供了修改 HTTP_REFERER 的功能。简言之，该值并不可信。)
$_SERVER['HTTPS']	如果脚本是通过 HTTPS 协议被访问，则被设为一个非空的值。
$_SERVER['REMOTE_ADDR']	浏览当前页面的用户的 IP 地址。
$_SERVER['REMOTE_HOST']	浏览当前页面的用户的主机名。DNS 反向解析不依赖于用户的 REMOTE_ADDR。
$_SERVER['REMOTE_PORT']	用户机器上连接到 Web 服务器所使用的端口号。
$_SERVER['SCRIPT_FILENAME']	当前执行脚本的绝对路径。
$_SERVER['SERVER_ADMIN']	该值指明了 Apache 服务器配置文件中的 SERVER_ADMIN 参数。如果脚本运行在一个虚拟主机上，则该值是那个虚拟主机的值。(如：someone@php.cn)
$_SERVER['SERVER_PORT']	Web 服务器使用的端口。默认值为 "80"。如果使用 SSL 安全连接，则这个值为用户设置的 HTTP 端口。
$_SERVER['SERVER_SIGNATURE']	包含了服务器版本和虚拟主机名的字符串。
$_SERVER['PATH_TRANSLATED']	当前脚本所在文件系统（非文档根目录）的基本路径。这是在服务器进行虚拟到真实路径的映像后的结果。
$_SERVER['SCRIPT_NAME']	包含当前脚本的路径。这在页面需要指向自己时非常有用。__FILE__ 常量包含当前脚本(例如包含文件)的完整路径和文件名。
$_SERVER['SCRIPT_URI']	URI 用来指定要访问的页面。例如 "/"。
*/

///////////////////////////////////////////////////////////////////////

<!-- 当用户通过点击 "Submit" 按钮提交表单数据时, 表单数据将发送至<form>标签中 action 属性中指定的脚本文件
    这里, 就是自身
 -->
<form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
Name: <input type="text" name="fname">
<input type="submit">
</form>

<?php 
$name = $_REQUEST['fname']; 
echo $name; 

///////////////////////////////////////////////////////////////////
<form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
Name: <input type="text" name="fname">
<input type="submit">
</form>

<?php 
$name = $_POST['fname']; 
echo $name; 
?>

//////////////////////////////////////////////////////////////

<a href="test_get.php?subject=PHP&web=php.cn">Test $GET</a>

// test_get.php代码
echo "Study " . $_GET['subject'] . " at " . $_GET['web'];

//////////////////////////
//
//      内建函数    自定义函数
//
////////////////////////////

function add($x,$y)
{
$total=$x+$y;
return $total;// 如果不需要返回值, 去掉这行即可
}
echo "1 + 16 = " . add(1,16);


//////////////////////////////////
//
//          魔术变量
//
///////////////////////////////////
/*
PHP 向它运行的任何脚本提供了大量的预定义常量。

不过很多常量都是由不同的扩展库定义的，只有在加载了这些扩展库时才会出现，或者动态加载后，或者在编译时已经包括进去了。

有八个魔术常量它们的值随着它们在代码中的位置改变而改变。

例如 __LINE__ 的值就依赖于它在脚本中所处的行来决定。这些特殊的常量不区分大小写
*/

echo '这是第 “ '  . __LINE__ . ' ” 行';// 当前行数

echo '该文件位于 “ '  . __FILE__ . ' ” ';// 文件的完整路径和文件名(绝对路径)。如果用在被包含文件中，则返回被包含的文件名; 获取相对路径 $_SERVER['PHP_SELF']

echo '该文件位于 “ '  . __DIR__ . ' ” 文件夹中';// 文件所在的目录。如果用在被包括文件中，则返回被包括的文件所在的目录

echo  '函数名为：' . __FUNCTION__ ;// 返回当前函数名, 如果不再函数内, 则返回空;
echo '类名为：'  . __CLASS__ . "<br>";// 类名称

class test {
    function _print() {
        echo '类名为：'  . __CLASS__ . "<br>";
        echo  '函数名为：' . __FUNCTION__ ;
    }
}
$t = new test();
$t->_print();
/*输出结果:
类名为：test
函数名为：_print
*/

echo  '函数名为：' . __METHOD__ ;// 类的方法名（PHP 5.0.0 新加）。返回该方法被定义时的名字

namespace MyProject;
echo '命名空间为："', __NAMESPACE__, '"'; // 当前命名空间的名称, 输出 "MyProject", 如果没有namespace, 输出""

///////////////////////////////////
//
//          trait 代码复用
//
/////////////////////////////////
class Base {// 基类
    public function sayHello() {
        echo 'Hello ';
    }
}
trait SayWorld {// 复用类
    public function sayHello() {
        parent::sayHello();
        echo 'World!';
    }
}
// 优先级: MyHelloWorld中的方法会覆盖trait中的方法, trait中的会覆盖父类中的方法
class MyHelloWorld extends Base {
    use SayWorld;
}
$o = new MyHelloWorld();
$o->sayHello();

////////////////////////////////////////
//
//          命名空间
//
/////////////////////////////////////////

declare(encoding='UTF-8'); //定义多个命名空间和不包含在命名空间中的代码
namespace MyProject1;  
// MyProject1 命名空间中的PHP代码  
 
namespace MyProject2;  // 可以定义多个namespace
// MyProject2 命名空间中的PHP代码    
 
// 另一种语法
namespace MyProject3 {  
 // MyProject3 命名空间中的PHP代码    
}

namespace { // 全局代码
session_start();
$a = MyProject\connect();
echo MyProject\Connection::start();
}

// ---------------------分层次的namespace---------------------------

/*
下面例子创建了常量 MyProject\Sub\Level\CONNECT_OK，类 MyProject\Sub\Level\Connection 和函数 MyProject\Sub\Level\Connect。
*/
namespace MyProject\Sub\Level;  //声明分层次的单个命名空间
const CONNECT_OK = 1;
class Connection { /* ... */ }
function Connect() { /* ... */  }

// --------------------怎么使用---------------------------

// file1的代码
namespace Foo\Bar\subnamespace; 

const FOO = 1;
function foo() {}
class foo
{
    static function staticmethod() {}
}

// file2代码
namespace Foo\Bar;
include 'file1.php';// 引入file1.php

const FOO = 2;
function foo() {}
class foo
{
    static function staticmethod() {}
}

/* 非限定名称, 在当前namespace使用 */
foo(); // 解析为 Foo\Bar\foo 
foo::staticmethod(); // 解析为类 Foo\Bar\foo的静态方法staticmethod。
echo FOO; // resolves to constant Foo\Bar\FOO

/* 限定名称 */
subnamespace\foo(); // 解析为函数 Foo\Bar\subnamespace\foo
subnamespace\foo::staticmethod(); // 解析为类 Foo\Bar\subnamespace\foo,
                                  // 以及类的方法 staticmethod
echo subnamespace\FOO; // 解析为常量 Foo\Bar\subnamespace\FOO
                                  
/* 完全限定名称, 在任何namespace都可以使用 */
\Foo\Bar\foo(); // 解析为函数 Foo\Bar\foo
\Foo\Bar\foo::staticmethod(); // 解析为类 Foo\Bar\foo, 以及类的方法 staticmethod
echo \Foo\Bar\FOO; // 解析为常量 Foo\Bar\FOO

/* 在命名空间内部访问全局类、函数和常量： */
$a = \strlen('hi'); // 调用全局函数strlen
$b = \INI_ALL; // 访问全局常量 INI_ALL
$c = new \Exception('error'); // 实例化全局类 Exception

// --------------------namespace的解析优先级---------------------------

namespace A;
use B\D, C\E as F;

// 函数调用

foo();      // 首先尝试调用定义在命名空间"A"中的函数foo()
            // 再尝试调用全局函数 "foo"

\foo();     // 调用全局空间函数 "foo" 

my\foo();   // 调用定义在命名空间"A\my"中函数 "foo" 

F();        // 首先尝试调用定义在命名空间"A"中的函数 "F" 
            // 再尝试调用全局函数 "F"

// 类引用

new B();    // 创建命名空间 "A" 中定义的类 "B" 的一个对象
            // 如果未找到，则尝试自动装载类 "A\B"

new D();    // 使用导入规则，创建命名空间 "B" 中定义的类 "D" 的一个对象
            // 如果未找到，则尝试自动装载类 "B\D"

new F();    // 使用导入规则，创建命名空间 "C" 中定义的类 "E" 的一个对象
            // 如果未找到，则尝试自动装载类 "C\E"

new \B();   // 创建定义在全局空间中的类 "B" 的一个对象
            // 如果未发现，则尝试自动装载类 "B"

new \D();   // 创建定义在全局空间中的类 "D" 的一个对象
            // 如果未发现，则尝试自动装载类 "D"

new \F();   // 创建定义在全局空间中的类 "F" 的一个对象
            // 如果未发现，则尝试自动装载类 "F"

// 调用另一个命名空间中的静态方法或命名空间函数

B\foo();    // 调用命名空间 "A\B" 中函数 "foo"

B::foo();   // 调用命名空间 "A" 中定义的类 "B" 的 "foo" 方法
            // 如果未找到类 "A\B" ，则尝试自动装载类 "A\B"

D::foo();   // 使用导入规则，调用命名空间 "B" 中定义的类 "D" 的 "foo" 方法
            // 如果类 "B\D" 未找到，则尝试自动装载类 "B\D"

\B\foo();   // 调用命名空间 "B" 中的函数 "foo" 

\B::foo();  // 调用全局空间中的类 "B" 的 "foo" 方法
            // 如果类 "B" 未找到，则尝试自动装载类 "B"

// 当前命名空间中的静态方法或函数

A\B::foo();   // 调用命名空间 "A\A" 中定义的类 "B" 的 "foo" 方法
              // 如果类 "A\A\B" 未找到，则尝试自动装载类 "A\A\B"

\A\B::foo();  // 调用命名空间 "A\B" 中定义的类 "B" 的 "foo" 方法
              // 如果类 "A\B" 未找到，则尝试自动装载类 "A\B"

/*

名称解析遵循这些规则:

* 对完全限定名称的函数，类和常量的调用在编译时解析。例如      new \A\B 解析为类 A\B。

* 所有的非限定名称和限定名称（非完全限定名称）根据当前的导入规则在编译时进行转换。例如，如果命名空间     A\B\C 被导入为 C，那么对 C\D\e()     的调用就会被转换为 A\B\C\D\e()。

* 在命名空间内部，所有的没有根据导入规则转换的限定名称均会在其前面加上当前的命名空间名称。例如，在命名空间     A\B 内部调用 C\D\e()，则 C\D\e()     会被转换为 A\B\C\D\e() 。

* 非限定类名根据当前的导入规则在编译时转换（用全名代替短的导入名称）。例如，如果命名空间     A\B\C 导入为C，则 new C()     被转换为 new A\B\C() 。

* 在命名空间内部（例如A\B），对非限定名称的函数调用是在运行时解析的。例如对函数      foo() 的调用是这样解析的：

    * 在当前命名空间中查找名为 A\B\foo() 的函数

    * 尝试查找并调用 全局(global) 空间中的函数 foo()。

* 在命名空间（例如A\B）内部对非限定名称或限定名称类（非完全限定名称）的调用是在运行时解析的。下面是调用      new C() 及 new D\E() 的解析过程：       new C()的解析: new D\E()的解析:为了引用全局命名空间中的全局类，必须使用完全限定名称 new \C()。

    * 在类名称前面加上当前命名空间名称变成：A\B\D\E，然后查找该类。

    * 尝试自动装载类 A\B\D\E。

    * 在当前命名空间中查找A\B\C类。

    * 尝试自动装载类A\B\C。
*/


//////////////////////////////////////////
//
//          oo的一些内容, 和java的异同
//
////////////////////////////////////////
/*
构造函数
析构函数: 与构造函数相反，当对象结束其生命周期时（例如对象所在的函数已调用完毕），系统自动执行析构函数。析构函数往往用来做"清理善后" 的工作（例如在建立对象时用new开辟了一片内存空间，应在退出前在析构函数中用delete释放）。
*/

// 一个最简单的pojo类
class Site {
  /* 成员变量 
    类属性必须定义为公有(public)，受保护(protected)，私有(private)之一。如果用 var 定义，则被视为公有
  */
  var $url;
  var $title;
  /* 构造函数
    默认为public
  */
  function __construct( $par1, $par2 ) {
   $this->url = $par1;
   $this->title = $par2;
  }
  /* 析构函数 */
  function __destruct() {
       print "销毁 " . $this->title . "\n";
  }
  /* 成员函数 */
  function setUrl($par){
     $this->url = $par;
  }
  function getUrl(){
     echo $this->url . PHP_EOL;
  }
  function setTitle($par){
     $this->title = $par;
  }
  function getTitle(){
     echo $this->title . PHP_EOL;
  }
}

$taobao = new Site;
// or
$taobao = new Site('www.taobao.com', '淘宝'); 
$taobao->setTitle( "淘宝" );
?> 

</body> 
</html>
```

# trait，interface和abstract的异同

* trait看上去更像是为了代码的复用而写的一个小插件，它类似于include，可以用use放在类中间，让trait里面定义的方法作为class的一部分，本身不能直接实例化

* 当abstract作用于某个类时，此类被限定为抽象类，只能用于继承，而无法实例化对象; 抽象方法是必须被子类继承实现的，所以不可以用private修饰符去限定abstract抽象方法，可以用public和protected去修饰

* interface里面的方法都是虚拟的，需要在继承的时候对这些方法进行重定义


# 表单处理

```php
<html>
<head>
<meta charset="utf-8">
<title>php中文网(php.cn)</title>
</head>
<body>

<form action="/welcome/index.php" method="post">
名字: <input type="text" name="fname">
年龄: <input type="text" name="age">
<input type="submit" value="提交">
</form>

<?php
echo $_SERVER['HTTP_HOST'];
?>

</body>
</html>
```

welcome.php 文件如下所示

```php
欢迎 <?php echo $_POST["fname"]; ?>!<br>
你的年龄是 <?php echo $_POST["age"]; ?>  岁。
```

# swoole2.0 和协程

Swoole 2.x 让 PHP 这门 20多年历史的老牌后台编程语言也能有协程的能力













