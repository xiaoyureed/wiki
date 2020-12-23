---
title: CPP and C
tags: cpp
categories: language
date: 2020-06-15 21:57:31
---



https://github.com/jobbole/awesome-c-cn 资源集合

https://github.com/changkun/modern-cpp-tutorial

https://www.learncpp.com/

https://zh.cppreference.com/w/cpp 参考文档

《Essential C++ 中文版》
《C++ Primer Plus 第6版中文版》
《C++ Primer中文版（第5版）》

[cpp 常见问题](http://www.sunistudio.com/cppfaq/)

[cpp 类库框架搜索](https://www.thefreecountry.com/sourcecode/cpp.shtml)

[C 和 C++ 的用户组](http://www.hal9k.com/cug/), 提供了免费的涵盖各种编程领域 C++ 项目的源代码，包括 AI、动画、编译器、数据库、调试、加密、游戏、图形、GUI、语言工具、系统编程

[现代 cpp](https://www.bookstack.cn/books/modern-cpp-tutorial-zh)

[算法珠玑 cpp 版本](https://www.bookstack.cn/books/algorithm-essentials-cpp), [java 版本](https://www.bookstack.cn/read/algorithm-essentials-java/3.md), [算法总结](https://www.bookstack.cn/books/fucking-algorithm)

https://github.com/luohaha/Chinese-uvbook libuv 教程


<!--more-->

<!-- TOC -->

- [编译器](#编译器)
  - [GCC](#gcc)
  - [Clang](#clang)
  - [链接库 动态编译 静态编译](#链接库-动态编译-静态编译)
- [make 和 makefile](#make-和-makefile)
  - [make Makefile](#make-makefile)
- [存储类型](#存储类型)
- [内存划分](#内存划分)
  - [c 中划分](#c-中划分)
  - [cpp 中的划分](#cpp-中的划分)
- [c 语法](#c-语法)
  - [预处理器指令](#预处理器指令)
  - [标准输入输出](#标准输入输出)
  - [变量-常量-数据类型](#变量-常量-数据类型)
  - [变量存储类型](#变量存储类型)
  - [函数](#函数)
  - [指针](#指针)
  - [文件读写-io](#文件读写-io)
  - [错误处理](#错误处理)
  - [内存管理函数](#内存管理函数)
  - [使用命令行参数](#使用命令行参数)
  - [工程结构](#工程结构)
- [cpp 语法](#cpp-语法)
  - [数据类型-数学计算-cpp](#数据类型-数学计算-cpp)
  - [存储类型-cpp](#存储类型-cpp)
  - [流程控制](#流程控制)
  - [函数-cpp](#函数-cpp)
  - [指针-cpp](#指针-cpp)
  - [引用](#引用)
  - [动态分配内存](#动态分配内存)
  - [命名空间](#命名空间)
  - [模板](#模板)
  - [标准模板库 stl](#标准模板库-stl)
  - [日期时间](#日期时间)
  - [io-输入输出](#io-输入输出)
  - [面向对象](#面向对象)
    - [类-对象](#类-对象)
    - [友元](#友元)
    - [内联函数](#内联函数)
    - [函数重载-运算符重载](#函数重载-运算符重载)
    - [继承](#继承)
    - [多态-虚函数](#多态-虚函数)
  - [io-输入输出-cpp](#io-输入输出-cpp)
  - [异常处理-cpp](#异常处理-cpp)
  - [信号处理](#信号处理)
  - [多线程](#多线程)
  - [web 开发](#web-开发)
- [qt 框架](#qt-框架)
- [开发工具](#开发工具)
- [debug 工具](#debug-工具)
- [拾遗](#拾遗)
  - [指针的指针和二维数组的区别](#指针的指针和二维数组的区别)

<!-- /TOC -->


# 编译器

## GCC

gcc 是来自GNU套装 的 C/C++ 编译器, 原本只能处理 C语言。GCC 很快地扩展，变得可处理 C++。之后也变得可处理 Fortran、Pascal、Objective-C、Java, 以及 Ada与其他语言

GCC 5.1以下的所有版本，都默认对C语言使用C89, ~~建议 用 C99 标准~~， 通过为 compiler 设置参数 “-std=c99” 开启 (当前最新的C语言标准为 C11 ，用这个)

GCC将编译：`*C/*cpp`文件，分别作为C和C++。

G++ 是 gnu 专门用来编译 cpp 的, 默认使用 c++ 语法标准 (Gcc 不会, 但是可以给 gcc 指定语法标准), 将编译：`*.c/*.cpp`文件，但它们都将被视为C++文件。

GCC编译C文件的预定义宏较少。

```sh
# 查看版本
gcc -v

# build
# 生成 a.out 文件
gcc hello.c
# 指定输出文件名， 编译多个文件
gcc test1.c test2.c -o main

# run
./a.out
./main


# g++ 用法类似

# g++ 有些系统默认是使用 C++98，我们可以指定使用 C++11 来编译 
g++ -g -Wall -std=c++11 main.cpp


-ansi	只支持 ANSI 标准的 C 语法。这一选项将禁止 GNU C 的某些特色， 例如 asm 或 typeof 关键词。
-c	只编译并生成目标文件。
-DMACRO	以字符串"1"定义 MACRO 宏。
-DMACRO=DEFN	以字符串"DEFN"定义 MACRO 宏。
-E	只运行 C 预编译器。
-g	生成调试信息。GNU 调试器可利用该信息。
-IDIRECTORY	指定额外的头文件搜索路径DIRECTORY。
-LDIRECTORY	指定额外的函数库搜索路径DIRECTORY。
-lLIBRARY	连接时搜索指定的函数库LIBRARY。
-m486	针对 486 进行代码优化。
-o	FILE 生成指定的输出文件。用在生成可执行文件时。
-O0	不进行优化处理。
-O	或 -O1 优化生成代码。
-O2	进一步优化。
-O3	比 -O2 更进一步优化，包括 inline 函数。
-shared	生成共享目标文件。通常用在建立共享库时。
-static	禁止使用共享连接。
-UMACRO	取消对 MACRO 宏的定义。
-w	不生成任何警告信息。
-Wall	生成所有警告信息
```


## Clang

Clang 是一个 C++ 编写、基于 LLVM、发布于 LLVM BSD 许可证下的 C/C++/Objective C/Objective C++ 编译器，其目标（之一）就是超越 GCC

mac 自带, clang比gcc编译速度更快一些，而且错误提示更人性化。

分为 clang， clang++

clang++ 默认按照 c++ 处理代码

编译参数最好使用 “-std=c++2a”

编译过程: ( 以编译单文件 test.cc)

1. 预处理阶段 - 处理源码文件, 比如包含头文件到文件中，替换宏定义。生成预处理后的文件test.i `clang++ -E test.cc -o test.i`

1. 编译阶段 - 将预处理文件翻译成汇编程序 test.s `clang++ -S test.i`

1. 汇编阶段 - 将汇编程序翻译为 机器指令, 打包成 目标文件 test.o (windows 下为 test.obj, 即 object file ) `clang++ -c test.s`
   
   进一步可生成静态链接库 xxx.a `ar -rcs xxx.a test.o` (一般将独立的文件如一个方法编译为静态链接库)

1. 链接阶段 - 将多个 object file 合并成可执行文件. 比如: 若调用了系统函数 cout, 链接器将该函数对应的目标文件 cout.o 合并到 test.o, 生成可执行二进制文件 

   (可以加入静态链接文件) `clang++ -o main main.cc [xxx.a]`; 
   
   或者 在系统目录下查找指定静态链接库中的方法 add `clang++ -o main main.cc -l add`, 
   
   或者不在系统目录查找, 自己指定目录 `clang++ -o main main.cc -L ./ -l add`


编译多文件:

`clang++ -o main main.cc add.cc` 生成可执行文件, `/main` 执行

```cpp
// main.cc
#include <iostream>

extern int add(int, int);
using namespace std;

int main()
{
    int a, b;
    cin >> a >> b;
    cout << "a + b = " << add(a, b) << endl;
}

// add.cc
int add(int a, int b) 
{
    return a + b;
}

```

## 链接库 动态编译 静态编译

- 静态链接库: 即 obj 文件 (xxx.o) 的集合, 形如 xxx.a, `ar -rcs xxx.a xxx.o`, 在生成二进制文件可直接使用, 无需编译 (静态库的另一个优势是开发者可以提供库文件给使用人员不用开放源代码。理论上静态库比动态库速度快1%-5%.)

- 动态链接库: 动态库是程序运行时加载的库, 是 目标文件的集合; 动态库的名称有别名(soname),真名(realname)和链接名(linker name)。
    
    别名由一个前缀lib，然后是库的名字加上.so构成。
    
    真名是动态库的真实名称，一般总是在别名的基础上加上一个小的版本号

    链接名，即程序链接时使用的库的名字。在动态链接库安装的时候总是复制库文件到某个目录，然后用软连接生成别名，在库文件进行更新的时候仅仅更新软连接即可

```sh
# shared:生成动态链接库
# -soname:生成动态链接库的别名libadd.so
# 实际的动态链接库libstr.so.1
clang++ -shared -Wl,-soname,libadd.so -o libstr.so.1 add.cc


```


- 动态编译

- 静态编译

# make 和 makefile

## make Makefile

构建工具, 类似 java 的 maven, 大型工程必备

make 通过 调用 通过调用makefile文件中用户指定的命令来进行编译和链接

cmake 和 make 类似, 用来和 clang 搭配使用

```sh
# 编译当前目录下的 c 文件, 生成 main 
# 默认替我们做了这些事:
# 1. 找到 main.c, 
# 2. 执行 cc main.c -o main (cc 是就是编译器的别名, 在 mac 中就是 clang)
make main

# 编译时, 传递参数项
CFLAGS="-Wall" make ex1 # 显示警告, CFLAGS 也可以在环境变量中设置
```


# 存储类型

存储类|时期|作用域|连接|声明方式
|:--:|:--:|:--:|:--:|:--:|
自动|自动|代码块|空|代码块内 or 加 auto
寄存器|自动|代码块|空|代码块内，使用关键字register
具有外部连接的静态|静态|文件|外部|所有函数之外
具有内部连接的静态|静态|文件|内部|所有函数之外，使用关键字static
空连接的静态|静态|代码块|空|代码块内，使用关键字static

# 内存划分

https://blog.csdn.net/derkampf/article/details/51000466

## c 中划分

1. 内存栈区： 存放局部变量名， 函数的形参/局部变量，由编译器自动分配释放；
2. 内存堆区： 存放new或者malloc出来的对象，需要手动分配释放；
3. 常量区： 常量字符串就是放在这里的。 程序结束后由系统释放；
4. 静态区/全局区： 用于存放全局变量或者静态局部变量, 程序结束后由系统释放；
5. 代码区：存放函数体的二进制代码

使用free和delete销毁new和malloc申请的堆内存，而栈内存是动态释放。

了减少内存碎片的产生，编译器可能会将堆区又分为block和heap区。block由一系列大小相等的内存块组成。分配内存时先在block中分配，如果block占满则从heap区中分配。同时block的大小和个数可以通过配置文件进行配置，使之达到一个合适的数量

在静态全局区中， 初始化的全局变量和静态变量在一块区域（RW）， 未初始化的全局变量和未初始化的静态变量在相邻的另一块区域（ZI）

## cpp 中的划分

- 栈，就是那些由编译器在需要的时候分配，在不需要的时候自动清除的变量的存储区。里面的变量通常是局部变量、函数参数等。

- 堆(C里面的概念)，就是那些由malloc分配的内存块 (用于动态分配的内存)，一般一个malloc就要对应一个free.

- 自由存储区（C++引入的概念），就是那些由new等分配的内存块，他和堆是十分相似的

- 常量存储区，这是一块比较特殊的存储区，他们里面存放的是常量，不允许修改(当然，你要通过非正当手段也可以修改)

- 全局/静态存储区，全局变量和静态变量被分配到同一块内存中，在以前的C语言中，全局变量又分为初始化的和未初始化的，在C++里面没有这个区分了，他们共同占用同一块内存区。



# c 语法

hello.c 文件拓展名 

(clang 默认 将 *.c 视为 c++ 文件, 但是已经 过时了 deprecated)

C11（也被称为C1X）指ISO标准ISO/IEC 9899:2011，是当前最新的C语言标准。在它之前的C语言标准为C99

C 是区分大小写的编程语言

## 预处理器指令



```cpp
// 预处理器命令
//C 预处理器不是编译器的组成部分，但是它是编译过程中一个单独的步骤。简言之，是一个文本替换工具，在实际编译之前完成所需的预处理。 C 预处理器（C Preprocessor）简写为 CPP
// 常见预处理器:
// #define	定义宏
// #include	包含一个源代码文件
// #undef	取消已定义的宏
// #ifdef	如果宏已经定义，则返回真
// #ifndef	如果宏没有定义，则返回真
// #if	如果给定条件为真，则编译下面代码
// #else	#if 的替代方案
// #elif	如果前面的 #if 给定条件不为真，当前条件为真，则编译下面代码
// #endif	结束一个 #if……#else 条件编译块
// #error	当遇到标准错误时，输出错误消息
// #pragma	使用标准化方法，向编译器发布特殊的命令到编译器中

#define MAX_ARRAY_LENGTH 20 // 告诉 CPP 把所有的 MAX_ARRAY_LENGTH 替换为 20。使用 #define 定义常量来增强可读性
#undef  FILE_SIZE  // 告诉 CPP 取消已定义的 FILE_SIZE，并重新定义它为 42
#define FILE_SIZE 42
// 引入头文件
#include <stdio.h> // 告诉 CPP 从系统库中获取 stdio.h，并添加文本到当前的源文件中
#include "myheader.h" //告诉 CPP 从本地目录中获取 myheader.h
// 告诉 CPP 只有当 MESSAGE 未定义时，才定义 MESSAGE
// 推荐引入头文件时这样做, 先验证是否引入过
#ifndef MESSAGE
   #define MESSAGE "You wish!"
#endif
// 告诉 CPP 如果定义了 DEBUG，则执行处理语句。在编译时，如果向 gcc 编译器传递了 -DDEBUG 开关量，可以在编译期间随时开启或关闭调试
#ifdef DEBUG
   /* Your debugging statements here */
#endif
// 有条件引用
// SYSTEM_H 可通过 -D 选项被 Makefile 定义
#if SYSTEM_1
   # include "system_1.h"
#elif SYSTEM_2
   # include "system_2.h"
#elif SYSTEM_3
   ...
#endif
// 如果头文件较多
#define SYSTEM_H "system_1.h"
 ...
 #include SYSTEM_H

// 保证只引用一次头文件 (因为多次引用头文件, 编译器会报错)
//当再次引用头文件时，条件为假，因为 HEADER_FILE 已定义。此时，预处理器会跳过文件的整个内容，编译器会忽略它。
#ifndef HEADER_FILE
#define HEADER_FILE
the entire header file file
#endif

// 预定义宏
// ANSI C 已经定义了许多宏
// __DATE__	当前日期，一个以 "MMM DD YYYY" 格式表示的字符常量。
// __TIME__	当前时间，一个以 "HH:MM:SS" 格式表示的字符常量。
// __FILE__	这会包含当前文件名，一个字符串常量。
// __LINE__	这会包含当前行号，一个十进制常量。
// __STDC__	当编译器以 ANSI 标准编译时，则定义为 1。

// 预处理器运算符
// 宏延续运算符（\）: 一个宏通常写在一个单行上。但是如果宏太长，一个单行容纳不下, 可以折行
// 字符串常量化运算符（#）: 把一个宏的参数转换为字符串常量
#define  message_for(a, b)  \
    printf(#a " and " #b ": We love you!\n")
int main(void)
{
   message_for(Carole, Debra);
   return 0;
}

// 标记粘贴运算符（##）: 在宏定义中将两个独立的标记/令牌 被合并为一个标记/令牌
#define tokenpaster(n) printf ("token" #n " = %d", token##n)
int main(void)
{
   int token34 = 40;
   
   tokenpaster(34); // 输出 token34 = 40
   return 0;
}

// defined() 运算符: 确定一个标识符是否已经使用 #define 定义过
#if !defined (MESSAGE)
   #define MESSAGE "You wish!"
#endif
int main(void)
{
   printf("Here is the message: %s\n", MESSAGE);  
   return 0;
}

// 参数化的宏, 使用宏定义取代函数
// 宏名称和左圆括号之间不允许有空格
#define MAX(x,y) ((x) > (y) ? (x) : (y))
int main(void)
{
   printf("Max between 20 and 10 is %d\n", MAX(10, 20));  
   return 0;
}

```

## 标准输入输出

```cpp

//引入标准输入输出头文件
// 编译器遇到 printf() 函数时，如果没有找到 stdio.h 头文件，会发生编译错误。
// 使用 #include <iostream> 亦可
#include <stdio.h>
 
int main()
{
    char str[100];
   int i;
   printf( "Enter a value :");
   scanf("%s %d", str, &i);
   printf( "\nYou entered: %s %d ", str, i);
   printf("\n");

    int c;
    printf( "Enter a value :");
    // 只读取一个字符, 以数字返回
    c = getchar( );
    printf( "\nYou entered: ");
    // 只打印一个字符
    putchar( c );
    printf( "\n");

    char str[100];
   printf( "Enter a value :");
   // 读取一行到 s 所指向的缓冲区，直到一个终止符或 EOF
   gets( str );
   printf( "\nYou entered: ");
   // 把字符串 s 和一个尾随的换行符写入到 stdout。
   puts( str );

    return 0;
}

```

## 变量-常量-数据类型

```cpp

// 变量
//左值（lvalue）：指向内存位置的表达式被称为左值（lvalue）表达式。左值可以出现在赋值号的左边或右边。
//右值（rvalue）：术语右值（rvalue）指的是存储在内存中某些地址的数值。右值是不能对其进行赋值的表达式，也就是说，右值可以出现在赋值号的右边，但不能出现在赋值号的左边

// 局部变量
// 也叫 auto自动变量(auto可写可不写)，栈区存储，代码块{}内部定义的变量都是自动变量， 
// 只在函数/代码块内部有效
// 局部变量未初始化，值为乱码

// 全局变量
// 在函数外定义，可被本文件及其它文件中的函数所共用，若其它文件中的函数调用此变量,须用extern声明
// 全局变量的生命周期和程序运行周期一样
// 全局变量未初始化、值为0
//不同文件的全局变量不可重名

// 静态局部变量
// 作用域也是在定义的函数内有效
//生命周期和程序运行周期一样
//只会初始化一次，可以多次赋值
//若未赋以初值，则由系统自动赋值：数值型变量自动赋初值0，字符型变量赋空字符
// 变量只被引用而不改变其值，则这时用静态局部变量比较方便，如不必要，不要多用静态局部变量（用静态存储要多占内存，而且降低了程序的可读性）

//静态(static)全局变量
//在函数外定义,作用范围被限制在所定义的文件中
//static全局变量的生命周期和程序运行周期一样，同时staitc全局变量的值只初始化一次
//不同文件静态全局变量可以重名,但作用域不冲突

// 变量修饰符
//const	const 类型的对象在程序执行期间不能被修改改变。
// volatile	修饰符 volatile 告诉编译器不需要优化volatile声明的变量，让程序可以直接从内存中读取变量。对于一般的变量编译器会对变量进行优化，将内存中的变量值放在寄存器中以加快读写效率。
// restrict	由 restrict 修饰的指针是唯一一种访问它所指向的对象的方式。只有 C99 增加了新的类型限定符 restrict。

int main()
{
    // 数据类型 
    // 定义，会分配空间 
    // 初始化, 局部变量需要手动初始化
    float f,x=3.6,y=5.2; // 4 byte
    // 4 byte
    int i=4,a,b;
    
    a=x+y;
    b=(int)(x+y);
    f=10/i;
    printf("a=%d,b=%d,f=%f,x=%f\n",a,b,f,x);

    // 定义
    char   c, ch; // 1 byte
    short sh; // 2 byte
    long lo; // 4 byte  
    double d; // 8 byte
    byte z = 22;

    // 变量声明
    //向编译器保证变量以指定的类型和名称存在，这样编译器在不需要知道
    //变量完整细节的情况下也能继续进一步的编译。变量声明只在编译时有它的意义

    int j; // 声明， 也是定义，分配了空间
    extern int i; //声明，不是定义， 不分配空间， 变量 i 可以在别处定义， 如在 函数外部， 甚至另外的文件

    // 运算符
    // 移位运算 https://www.runoob.com/cprogramming/c-operators.html
}

// 获取 int 类型的大小
// sizeof是用来求类型的字节数的。如int a;那么无论sizeof(int)或者是sizeof(a)都是等于4，因为sizeof(a)其实就是sizeof(type of a)
#include <stdio.h>
#include <limits.h>
 
int main()
{
   printf("int 存储大小 : %lu \n", sizeof(int));
   
   return 0;
}


// 常量
//前缀指定基数：0x 或 0X 表示十六进制，0 表示八进制，不带前缀则默认表示十进制。
//整数常量也可以带一个后缀，后缀是 U 和 L 的组合，U 表示无符号整数（unsigned），L 表示长整数（long）。后缀可以是大写，也可以是小写，U 和 L 的顺序任意。

// 字符串常量
// 三者等价
"hello, dear"

"hello, \
dear"

"hello, " "d" "ear"

// 定义常量
#define LENGTH 10   
#define WIDTH  5
#define NEWLINE '\n'

//等价
const int  LENGTH = 10;
//Const  将数据定为不变的
// 以后不可改变其值。在指针中使用时，const的位置决定是指针本身不变还是指针指向的数据不变



// 数组
// 声明
double balance[10];
// 初始化
double balance[5] = {1000.0, 2.0, 3.4, 7.0, 50.0};
double balance[] = {1000.0, 2.0, 3.4, 7.0, 50.0};

// 数组和指针
//balance 是一个指向 &balance[0] 的常量指针 (常量指针就是指针无法加减, 只能通过这个指针获取值)，即数组 balance 的第一个元素的地址， 可以通过 *balance 访问这个元素
// balance+4 是指向 &balance[4] 的指针，可以通过 *(balance+4)访问元素

// 指针数组
int  var[] = {10, 100, 200};
int i, *ptr[MAX];

for ( i = 0; i < MAX; i++)
{
    ptr[i] = &var[i]; /* 赋值为整数的地址 */
}
for ( i = 0; i < MAX; i++)
{
    printf("Value of var[%d] = %d\n", i, *ptr[i] );
}

//// 字符指针可以指向一个字符串
char *str = "This is a string."; // 字符指针指向的是一个字符串常量的首地址
// 用一个指向字符的指针数组来存储一个字符串列表
const char *names[] = {
                "Zara Ali",
                "Hina Ali",
                "Nuha Ali",
                "Sara Ali",
};
int i = 0;

for ( i = 0; i < MAX; i++)
{
    printf("Value of names[%d] = %s\n", i, names[i] );
}




/**
  * 字符串 字符数组 
  *  
  * （字符串实际上是使用 null 字符 '\0' 终止的一维字符数组）
  * 
 以字符串形式出现的，编译器都会为该字符串自动添加一个0作为结束符，如在代码中写："abc",那么编译器帮你存储的是"abc\0"
 字符串常量的类型可以理解为相应字符常量数组的类型，如：
    "abcdef"的类型就可以看成是const char[7]
*/
// "abc"是常量吗？答案是有时是，有时不是
 char str[] = "abc"; // 此时不是常量，存放于栈
 // 等价于
char str[4] = {'a','b','c','/0'};

  // 此时是常量，存放在常量区
  //ptr[0] = 'x';能编译通过，但是执行ptr[0] = 'x';就会发生运行时异常，因为这个语句试图去修改程序常量区中的东西
char* ptr = "abc";
// 推荐的写法
const char* ptr = "abc"

char string[]="This is a string.";
// 字符数组
//字符指针
// 字符指针可以指向一个字符串，指向的是一个字符串常量的首地址
char *str, *str1="This is another string.";
char string[100]="This is a string.";
str++; /* 指针str加1 */
str = "This is a NEW string."; /* 使指针指向新的字符串常量 */
str = str1; /* 改变指针str的指向，回复原来的值 */
strcpy( string, "This is a NEW string.") /* 改变字符串的的内容 */
strcat( string, str) /* 进行串连接操作 */

string++; /* 不能对数组名进行++运算 */
string = "This is a NEW string."; /* 错误的串操作，字符数组无法整体赋值，只能单个元素赋值 */
string = str1; /* 对数组名不能进行赋值 */
strcat(str, "This is a NEW string.") /* 不能在指针的后面进行串连接 */
strcpy(str, string) /* 不能向指针进行串复制 */
// 区别：
// (1)字符数组由若干个元素组成，每个元素中存放字符串的一个字符，而字符指针变量中存放的是字符串的首地址。
// (2)初始化方式不同。对字符数组初始化要用static存储类别，在编译时进行。而对字符指针变量初始化不必加static，在实际执行时进行。
// (3)赋值方式不同。对字符数组不能整体赋值，只能转化成份量，对单个元素进行。而字符指针变量赋值可整体进行
// (4)在定义一个字符数组时，编译时即已分配内存单元，有确定的地址。而定义一个字符指针变量时，给指针变量分配内存单元，但该指针变量具体指向哪个字符串，并不知道，即指针变量存放的地址不确定

// 字符串操作函数u
strcpy(s1, s2); // 复制字符串 s2 到字符串 s1。
strcat(s1, s2); //连接字符串 s2 到字符串 s1 的末尾。
strlen(s1); //返回字符串 s1 的长度。
strcmp(s1, s2); // 如果 s1 和 s2 是相同的，则返回 0；如果 s1<s2 则返回小于 0；如果 s1>s2 则返回大于 0。
strchr(s1, ch); // 返回一个指针，指向字符串 s1 中字符 ch 的第一次出现的位置。
strstr(s1, s2); // 返回一个指针，指向字符串 s1 中字符串 s2 的第一次出现的位置   


// 枚举
//在C 语言中，枚举类型是被当做 int 或者 unsigned int 类型来处理的， 因此可以将整数转换为枚举
// 定义类型
enum DAY
{//第一个枚举成员的默认值为整型的 0，后续枚举成员的值在前一个成员上加 1。我们在这个实例中把第一个枚举成员的值定义为 1，第二个就为 2，以此类推。
      MON=1, TUE, WED, THU, FRI, SAT, SUN
};
//没有指定值的枚举元素，其值为前一元素加 1。也就说 spring 的值为 0，summer 的值为 3，autumn 的值为 4，winter 的值为 5
enum season {spring, summer=3, autumn, winter};

// 定义枚举变量
enum DAY day;
// 可以同时进行
enum DAY
{
      MON=1, TUE, WED, THU, FRI, SAT, SUN
} day;
// 甚至 枚举名称可省略
enum
{
      MON=1, TUE, WED, THU, FRI, SAT, SUN
} day;

// 整型转换
enum day
{
    saturday,
    sunday,
    monday,
    tuesday,
    wednesday,
    thursday,
    friday
} workday;

int a = 1;
enum day weekend;
weekend = ( enum day ) a;  //类型转换
//weekend = a; //错误


/**
  * 结构体
  * 
  *
 **/
// 完整声明
struct Books
{
   char  title[50];
   char  author[50];
   char  subject[100];
   int   book_id;
} book;
// 没有标签
struct 
{
    int a;
    char b;
    double c;
} s1;
// 没有声明变量
struct SIMPLE
{
    int a;
    char b;
    double c;
};
//用SIMPLE标签的结构体，另外声明了变量t1、t2、t3
struct SIMPLE t1, t2[20], *t3;

//也可以用typedef创建新类型
typedef struct
{
    int a;
    char b;
    double c; 
} Simple2;
//现在可以用Simple2作为类型声明新的结构体变量
Simple2 u1, u2[20], *u3;


/** typeof 
为类型取一个新的名字

typedef vs #define
- typedef 仅限于为类型定义符号名称，#define 不仅可以为类型定义别名，也能为数值定义别名，比如您可以定义 1 为 ONE。
- typedef 是由编译器执行解释的，#define 语句是由预编译器进行处理的。
*/
typedef unsigned char BYTE;// 为单字节数字定义了一个术语 BYTE
BYTE  b1, b2; // BYTE 可作为类型 unsigned char 的缩写




// 结构体可嵌套
//此结构体的声明包含了其他的结构体
struct COMPLEX
{
    char string[100];
    struct SIMPLE a;
};
 
//此结构体的声明包含了指向自己类型的指针
struct NODE
{
    char string[100];
    struct NODE *next_node;
};

// 如果两个结构体互相包含，则需要对其中一个结构体进行不完整声明，如下所示：
struct B;    //对结构体B进行不完整声明
//结构体A中包含指向结构体B的指针
struct A
{
    struct B *partner;
    //other members;
};
//结构体B中包含指向结构体A的指针，在A声明完后，B也随之进行声明
struct B
{
    struct A *partner;
    //other members;
};

// 结构体定义 & 初始化
struct Books
{
   char  title[50];
   char  author[50];
   char  subject[100];
   int   book_id;
} book = {"C 语言", "RUNOOB", "编程语言", 123456};

// 也可以先定义再初始化
struct Books Book1;        /* 声明 Book1，类型为 Books */
/* Book1 详述 */
strcpy( Book1.title, "C Programming");
strcpy( Book1.author, "Nuha Ali"); 
strcpy( Book1.subject, "C Programming Tutorial");
Book1.book_id = 6495407;
/* 输出 Book1 信息 */
printf( "Book 1 title : %s\n", Book1.title);
printf( "Book 1 author : %s\n", Book1.author);
printf( "Book 1 subject : %s\n", Book1.subject);
printf( "Book 1 book_id : %d\n", Book1.book_id);

/* 通过传 Book1 的地址来输出 Book1 信息 */
printBook( &Book1 );

// 作为函数参数
void printBook( struct Books *book ) {
    // 通过指针访问结构体
   printf( "Book title : %s\n", book->title);
   printf( "Book author : %s\n", book->author);
   printf( "Book subject : %s\n", book->subject);
   printf( "Book book_id : %d\n", book->book_id);
}

/**
 位域/位段
 把一个字节中的二进位划分为几个不同的区域，并说明每个区域的位数。每个域有一个域名，允许在程序中按域名进行操作。这样就可以把几个不同的对象用一个字节的二进制位域来表示
 如: 用 1 位二进位存放一个开关量时，只有 0 和 1 两种状态。
-  一个位域不能跨字节, 如一个字节所剩空间不够存放另一位域时，则会从下一单元起存放该位域。也可以有意使某位域从下一单元开始
*/
// 定义
// data 为 bs 变量，共占两个字节。其中位域 a 占 8 位，位域 b 占 2 位，位域 c 占 6 位
struct bs{
    int a:8; // 位域a
    int  :4;    /* 空域/无名位域 , 会在对应位填 0 表示不用*/
    int b:2; // 位域b, 从下一单元(字节)开始存放  // 最大能够表示 3
    int c:6; // 位域c
} data;
struct bs{
    unsigned a:1;
    unsigned b:3; // 最大可以表示 7
    unsigned c:4;
} bit,*pbit;
bit.a=1;    /* 给位域赋值（应注意赋值不能超过该位域的允许范围） */
bit.b=7;    /* 给位域赋值（应注意赋值不能超过该位域的允许范围） */
bit.c=15;    /* 给位域赋值（应注意赋值不能超过该位域的允许范围） */
printf("%d,%d,%d\n",bit.a,bit.b,bit.c);    /* 以整型量格式输出三个域的内容 */
pbit=&bit;    /* 把位域变量 bit 的地址送给指针变量 pbit */
pbit->a=0;    /* 用指针方式给位域 a 重新赋值，赋为 0 */
pbit->b&=3;    /* 使用了复合的位运算符 "&="，相当于：pbit->b=pbit->b&3，位域 b 中原有值为 7，与 3 作按位与运算的结果为 3（111&011=011，十进制值为 3） */
pbit->c|=1;    /* 使用了复合位运算符"|="，相当于：pbit->c=pbit->c|1，其结果为 15 */
printf("%d,%d,%d\n",pbit->a,pbit->b,pbit->c);    /* 用指针方式输出了这三个域的值 */


/** 共用体
在相同的内存位置存储不同的数据类型, 但是任何时候只能有一个成员带有值
共用体占用的内存应足够存储共用体中最大的成员
*/
// Data 类型的变量可以存储一个整数、一个浮点数，或者一个字符串。这意味着一个变量（相同的内存位置）可以存储多个多种类型的数据
//Data 将占用 20 个字节的内存空间，因为在各个成员中，字符串所占用的空间是最大的
union Data
{
   int i;
   float f;
   char  str[20];
} data;
int main( )
{
   union Data data;        
   printf( "Memory size occupied by data : %d\n", sizeof(data));// 20

   data.i = 10;
   data.f = 220.5;
   strcpy( data.str, "C Programming");// 会覆盖前面赋给的值
 
   printf( "data.i : %d\n", data.i);// 无法正确显示
   printf( "data.f : %f\n", data.f);// 无法正确显示
   printf( "data.str : %s\n", data.str);// 显示正常

   // 正确的用法: (同一时间只用到一个成员)
   data.i = 10;
   printf( "data.i : %d\n", data.i);
   
   data.f = 220.5;
   printf( "data.f : %f\n", data.f);
   
   strcpy( data.str, "C Programming");
   printf( "data.str : %s\n", data.str);

   return 0;
}

```

## 变量存储类型

```cpp

/**
存储类型 storage type
用来定义变量/函数的作用域/生命（存储）周期/连接属性
分为：动态存储、静态存储(static)、自动变量(auto)、寄存器变量 (register)、外部变量(extern)

auto char c; //字符型自动变量，在函数内定义  
register int d; //整型寄存器变量，在函数内定义  
static int a; //静态局部整型变量或静态外部整型变量  
extern int b; //声明一个已定义的外部整型变量  

作用域：也就是变量的可见范围， 有三个等级：
      - 代码块作用域 (在函数内部代码块中定义的变量、形参，称为局部变量)
      - 函数原型作用域（从变量定义处一直到原型声明的末尾）
      - 文件作用域 (在函数之外定义的变量，整个文件都可见，称为全局变量)
      - 函数作用域 (只适用于goto语句使用的标签, 意味着一个特定函数中的goto标签对该函数中任何地方的代码都是可见的，无论该标签出现在哪一个代码块中。)
存储周期/生命周期：定义变量在内存中的存储方式，分2类
      - 静态存储周期 （变量在静态存储区存储，系统对变量分配固定的存储空间，在程序执行期间一直存在， 例如全局变量）
      - 自动/动态存储周期 （变量动态存储区存储，系统对变量在运行期间动态地分配存储空间，  在程序进入定义的变量代码块中时在栈中分配内存，退出时释放内存,例如局部变量）
连接属性：
      - 外部连接 （具有外部连接的变量可以在多文件程序的任何地方使用，多个文件可有共享）
      - 内部连接 （具有内部连接的变量可以在一个文件的任何地方使用，归该文件私有）如 用static在所有函数的外部进行定义来创建
      - 空连接 （具有代码块作用域和函数原型作用域的变量是空连接，由定义所在的代码块和函数原型所私有）

自动变量 （auto）
- 用auto修饰或者不修饰就默认属于自动存储类的变量，具有自动存储周期、代码块作用域、空连接。
- 在默认下，在代码快或者函数开头定义的任意变量都属于自动存储类。也只能用于代码块内/函数内部的变量
- 对于自动变量，除非显式的初始化，否则不会自动初始化, 在运行时程序才赋初值， 每运行一次， 赋初值一次

寄存器变量（register ）
- 用于将局部变量的值放在CPU中的寄存器中，需要用时直接从寄存器取出参加运算，不必再到内存中去存取 (所以无法获得其地址，变量的最大尺寸等于寄存器的大小 （通常是一个词），且不能对它应用一元的 '&' 运算符（因为它没有内存位置）)
- register i;等价于register int i;,register默认是int类型.
- 只能用在具有代码块作用域的变量
- 只用于需要快速访问的变量，比如计数器
- 仅仅意味着变量可能存储在寄存器中，这取决于硬件和实现的限制。

静态存储变量（static）
- 在程序的生命周期内始终保持变量不销毁 
- 赋初值是在编译时进行的，即只赋初值一次，在程序运行时它已有初值，若不指定，编译时自动初始化为0
- 使用 static 修饰局部变量可以在函数调用之间保持局部变量的值 （也就是使该变量具有静态存储周期，变量仍保留代码块作用域和空连接， 在其他函数中它是“不可见”的）
- 当 static 修饰全局变量时，会使变量的作用域限制在声明它的文件内 （也就是 使该变量具有内部连接）

外部变量(extern)
- 用于提供一个已定义的全局变量的引用 （这个全局变量可能定义在另外的文件），声明一个在其他地方定义了的变量
- 定义在函数外部的变量的存储类型都是extern,即使不使用关键字extern.


*/



```

## 函数

```cpp


/** 函数
 * 存储在 代码区

函数声明可以不要参数名 如： int max(int, int);

匿名内部函数
- 执行完后立即销毁， 用 {} 包裹

函数作用域
- 所有的函数默认都是全局的，意味着所有的函数都不能重名，但如果是staitc函数，那么作用域是文件级的，在其他文件中不能调用，所以不同的文件static函数名是可以相同的。 
- 同一源文件中,允许全局变量和局部变量同名，在局部变量的作用域内，全局变量暂时被覆盖

值传递：c 默认采用，传递的是 复制拷贝
引用传递：传递指针

数组作为参数：
void myFunction(int *param) {}
void myFunction(int param[10]){}
void myFunction(int param[]){} // 就函数而言，数组的长度是无关紧要的，因为 C++ 不会对形式参数执行边界检查
如果形参为数组，则编译器把其解释为普通的指针类型，如
   对于void func(char sa[100],int ia[20],char *p)
  则sa的类型为char*，ia的类型为int*，p的类型为char*


数组作为返回值 (返回值类型为指针, 因为 c / c++ 不支持返回一个数组, 只能返回执行数组的指针)
int * myFunction(){
    C 不支持函数返回局部变量的地址，除非定义局部变量为 static 变量
    static int result[10];
    ...
    return result;
}
*/


/**
可变参数
省略号之前的那个参数是 int，代表了要传递的可变参数的总数。为了使用这个功能，您需要使用 stdarg.h 头文件，该文件提供了实现可变参数功能的函数和宏
int func(int, ... ) {}
int main()
{
   func(2, 2, 3);
   func(3, 2, 3, 4);
}
*/
#include <stdio.h>
#include <stdarg.h>
 
double average(int num,...)
{
 
    // 创建一个 va_list 类型变量，该类型是在 stdarg.h 头文件中定义的
    // 代表参数列表
    va_list valist;
    double sum = 0.0;
    int i;
 
    // 初始化参数列表
    // 宏 va_start 是在 stdarg.h 头文件中定义的
    va_start(valist, num);
 
    /* 访问所有赋给 valist 的参数 */
    for (i = 0; i < num; i++)
    {
        // 访问参数列表中的每个元素
       sum += va_arg(valist, int);
    }
    /* 清理为 valist 保留的内存 */
    va_end(valist);
 
    // 返回平均值
    return sum/num;
}
 
int main()
{
   printf("Average of 2, 3, 4, 5 = %f\n", average(4, 2,3,4,5));
   printf("Average of 5, 10, 15 = %f\n", average(3, 5,10,15));
}

```

## 指针

```cpp


/**

    指针


 * 指针是一个变量，其值为另一个变量的地址，即，内存位置的直接地址
 * 4/8 byte
*/
// 指针声明
int    *ip;    /* 一个整型的指针 */
double *dp;    /* 一个 double 型的指针 */
float  *fp;    /* 一个浮点型的指针 */
char   *ch;     /* 一个字符型的指针 */

// 对变量取地址
int  var1;
char var2[10];
printf("var1 变量的地址： %p\n", &var1  );
printf("var2 变量的地址： %p\n", &var2  );

int  var = 20;   
//推荐初始化为 null，赋为 NULL 值的指针被称为空指针，NULL 指针是一个定义在标准库中的值为零的常量，
// 检查空指针可以这样做: if(!ip)  如果为空。。。
int  *ip = NULL;        /* 指针变量的声明 */

ip = &var;  /* 在指针变量中存储 var 的地址 */

printf("Address of var variable: %p\n", &var  );
/* 在指针变量中存储的地址 */
printf("Address stored in ip variable: %p\n", ip );
/* 使用指针访问值 */
printf("Value of *ip variable: %d\n", *ip );


// 指针运算 ++、--、+、-
//指针的每一次递增，它其实会指向下一个元素的存储单元 （跳跃的字节数取决于指针所指向变量数据类型长度，比如 int 就是 4 个字节）
//v指针的每一次递减，它都会指向前一个元素的存储单元。

// 指针递增
// 使用指针代替数组，因为变量指针可以递增，而数组不能递增，数组可以看成一个指针常量
int  var[] = {10, 100, 200};
int  i, *ptr;
// 数组名可以直接赋值给指针, 指向数组第一个元素
ptr = var;
for ( i = 0; i < MAX; i++)
{

    printf("存储地址：var[%d] = %x\n", i, ptr );
    printf("存储值：var[%d] = %d\n", i, *ptr );

    /* 移动到下一个位置 */
    ptr++;
}

// 指针比较
int  var[] = {10, 100, 200};
int  i, *ptr;
ptr = var;
i = 0;
while ( ptr <= &var[MAX - 1] )
{

    printf("Address of var[%d] = %p\n", i, ptr );
    printf("Value of var[%d] = %d\n", i, *ptr );

    /* 指向上一个位置 */
    ptr++;
    i++;
}





// 函数指针
// 指向函数的指针变量
// typedef int (*fun_ptr)(int,int); // 声明一个指向同样参数、返回值的函数指针类型
int max(int x, int y)
{
    return x > y ? x : y;
}
int main(void)
{
    /* p 是函数指针 */
    int (* p)(int, int) = & max; // &可以省略
    int a, b, c, d;
 
    printf("请输入三个数字:");
    scanf("%d %d %d", & a, & b, & c);
 
    /* 与直接调用函数等价，d = max(max(a, b), c) */
    d = p(p(a, b), c); 
 
    printf("最大的数字是: %d\n", d);
 
    return 0;
}

// 函数指针作为参数
// 回调函数
// size_t 就是 unsigned int ，为了跨平台
void populate_array(int *array, size_t arraySize, int (*getNextValue)(void))
{
    for (size_t i=0; i<arraySize; i++)
        array[i] = getNextValue();
}
// 获取随机值
int getNextRandomValue(void)
{
    return rand();
}
int main(void)
{
    int myarray[10];
    populate_array(myarray, 10, getNextRandomValue);
    for(int i = 0; i < 10; i++) {
        printf("%d ", myarray[i]);
    }
    printf("\n");
    return 0;
}

```

## 文件读写-io


```cpp

/** 文件读写
打开文件
FILE *fopen( const char * filename, const char * mode );
mode 打开模式:
r	打开一个已有的文本文件，允许读取文件。
w	打开一个文本文件，允许写入文件。如果文件不存在，则会创建一个新文件。在这里，您的程序会从文件的开头写入内容。如果文件存在，则该会被截断为零长度，重新写入。
a	打开一个文本文件，以追加模式写入文件。如果文件不存在，则会创建一个新文件。在这里，您的程序会在已有的文件内容中追加内容。
r+	打开一个文本文件，允许读写文件。
w+	打开一个文本文件，允许读写文件。如果文件已存在，则文件会被截断为零长度，如果文件不存在，则会创建一个新文件。
a+	打开一个文本文件，允许读写文件。如果文件不存在，则会创建一个新文件。读取会从文件的开头开始，写入则只能是追加模式。
如果处理二进制文件, 需要加 "b"

关闭文件
int fclose( FILE *fp ); 会清空缓冲区中的数据, 返回 0

写入文件
int fputc( int c, FILE *fp ); 写入一个字符
int fputs( const char *s, FILE *fp ); 写入字符串, 写入成功，它会返回一个非负值，如果发生错误，则会返回 EOF
int fprintf(FILE *fp,const char *format, ...) 

读取文件
int fgetc( FILE * fp ); 读取一个字符
char *fgets( char *buf, int n, FILE *fp ); 读取字符串
int fscanf(FILE *fp, const char *format, ...) 在遇到第一个空格和换行符时，它会停止读取

读写二进制流
用于存储块的读写 - 通常是数组或结构体
size_t fread(void *ptr, size_t size_of_elements, 
             size_t number_of_elements, FILE *a_file);
size_t fwrite(const void *ptr, size_t size_of_elements, 
             size_t number_of_elements, FILE *a_file);
*/
#include <stdio.h>
 
int main()
{
   FILE *fp = NULL;
 
   fp = fopen("/tmp/test.txt", "w+");
   fprintf(fp, "This is testing for fprintf...\n");
   fputs("This is testing for fputs...\n", fp);
   fclose(fp);

   char buff[255];
 
   fp = fopen("/tmp/test.txt", "r");
   fscanf(fp, "%s", buff);
   printf("1: %s\n", buff );
 
   fgets(buff, 255, (FILE*)fp);
   printf("2: %s\n", buff );
   
   fgets(buff, 255, (FILE*)fp);
   printf("3: %s\n", buff );
   fclose(fp);
}

```

## 错误处理

```cpp

/** 错误处理
发生错误时，大多数的 C 或 UNIX 函数调用返回 1 或 NULL，同时会设置一个错误代码 errno，该错误代码是全局变量
可以在errno.h 头文件中找到各种各样的错误代码

perror() 函数显示您传给它的字符串，后跟一个冒号、一个空格和当前 errno 值的文本表示形式。
strerror() 函数，返回一个指针，指针指向当前 errno 值的文本表示形式。

*/
#include <stdio.h>
#include <errno.h>
#include <string.h>
 
extern int errno ;
 
int main ()
{
   FILE * pf;
   int errnum;
   pf = fopen ("unexist.txt", "rb");
   if (pf == NULL)
   {
      errnum = errno;
      fprintf(stderr, "错误号: %d\n", errno);
      perror("通过 perror 输出错误");
      fprintf(stderr, "打开文件错误: %s\n", strerror( errnum ));
      // 程序出错, 宏 EXIT_FAILURE，被定义为 -1
    //   exit(EXIT_FAILURE);
   }
   else
   {
      fclose (pf);
   }
   return 0;
   // 顺利结束, 宏 EXIT_SUCCESS 被定义为 0 
   // exit(EXIT_SUCCESS);
}
// 错误号: 2
// 通过 perror 输出错误: No such file or directory
// 打开文件错误: No such file or directory

```

## 内存管理函数

https://blog.csdn.net/u014427196/article/details/52568029
https://blog.csdn.net/weixin_40548136/article/details/101351135
TODO

```cpp

/**

内存管理函数

C 语言为内存的分配和管理提供了几个函数。这些函数可以在 <stdlib.h> 头文件中找到
void *calloc(int num, int size); 在内存中动态地分配 num 个长度为 size 字节 的连续空间，并将每一个字节都初始化为 0
void free(void *address); 释放 address 所指向的内存块,释放的是动态分配的内存空间 (void * 类型表示未确定类型的指针)
void *malloc(int num); 在堆区分配一块指定大小的内存空间，用来存放数据。这块内存空间在函数执行完成后不会被初始化
void *realloc(void *address, int newsize); 该函数重新分配内存，把内存扩展到 newsize。

*/
// 动态分配内存
//重新调整内存的大小和释放内存
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
int main()
{
   char name[100]; // 固定容量的字符串
   char *description; // 字符指针, 大小未确定
 
   strcpy(name, "Zara Ali");
 
   /* 动态分配内存 200 个字符大小 */
   description = (char *)malloc( 200 * sizeof(char) );
   // 等价
   // calloc(200, sizeof(char));

   if( description == NULL )
   {
       // 输出错误信息
      fprintf(stderr, "Error - unable to allocate required memory\n");
   }
   else
   {
       // 初始化
      strcpy( description, "Zara ali a DPS student in class 10th");
   }
   printf("Name = %s\n", name );
   printf("Description: %s\n", description );


    // 重新分配内存大小
   description = (char *) realloc( description, 100 * sizeof(char) );
   if( description == NULL )
   {
      fprintf(stderr, "Error - unable to allocate required memory\n");
   }
   else
   {
      strcat( description, "She is in class 10th");
   }
   
   printf("Name = %s\n", name );
   printf("Description: %s\n", description );
 
   /* 使用 free() 函数释放内存 */
   free(description);
}


```

## 使用命令行参数


```cpp

// 命令行参数

#include <stdio.h>
//argc 是指传入参数的个数 (参数个数 包括命令执行本身, 如 ./main.out)，argv[] 是一个指针数组，指向传递给程序的每个参数
int main( int argc, char *argv[] )  
{
   if( argc == 2 )
   {
      printf("The argument supplied is %s\n", argv[1]);
   }
   else if( argc > 2 )
   {
      printf("Too many arguments supplied.\n");
   }
   else
   {
      printf("One argument expected.\n");
   }
}

```

## 工程结构

如何引入其他 c 代码中的方法?

需要一个中间桥梁 `xxx.h`, 在其中声明方法, add.c 中具体实现这个方法(需要引入 xxx.h), , main.c 中引入 xxx.h 后, 即可使用 add.c 中定义的方法了 ([ref](https://www.cnblogs.com/ifantastic/p/3914756.html))


# cpp 语法

文件拓展名 ".cpp .cc .C .cxx"

多个标准:

- c++98 传统 c++

- c++11，c++14/17， c++20 现代 c++

C++ 是 C 的一个超集，事实上，任何合法的 C 程序都是合法的 C++ 程序。

C++ 通常用于编写设备驱动程序和其他要求实时性的直接操作硬件的软件。

标准的 C++ 由三个重要部分组成：

- 核心语言，提供了所有构件块，包括变量、数据类型和常量，等等。
- C++ 标准库，提供了大量的函数，用于操作文件、字符串等。
- 标准模板库（STL），提供了大量的方法，用于操作数据结构等


## 数据类型-数学计算-cpp

```cpp
#include <iostream>
#include <stdio.h> //可以继续使用，但C++建议使用 #include <cstdio>
using namespace std;
int main()
{
    cout << "Hello, world!" << endl; // 可以用 "\n" 代替以上代码里的 endl
    return 0;
}

/**
数据类型
 
变量

bool:         所占字节数：1    最大值：1        最小值：0
char:         所占字节数：1    最大值：        最小值：?
signed char:     所占字节数：1    最大值：        最小值：?
unsigned char:     所占字节数：1    最大值：?        最小值：
short:         所占字节数：2    最大值：32767        最小值：-32768
int:         所占字节数：4    最大值：2147483647    最小值：-2147483648
unsigned:     所占字节数：4    最大值：4294967295    最小值：0
long:         所占字节数：8    最大值：9223372036854775807    最小值：-9223372036854775808
unsigned long:     所占字节数：8    最大值：18446744073709551615    最小值：0
double:     所占字节数：8    最大值：1.79769e+308    最小值：2.22507e-308
long double:     所占字节数：16    最大值：1.18973e+4932    最小值：3.3621e-4932
float:         所占字节数：4    最大值：3.40282e+38    最小值：1.17549e-38
size_t:     所占字节数：8    最大值：18446744073709551615    最小值：0
wchar_t:     所占字节数：4    最大值：2147483647        最小值：-2147483648
string:     所占字节数：24

*/
extern int d = 3, f = 5;    // d 和 f 的声明 
int d = 3, f = 5;           // 定义并初始化 d 和 f
byte z = 22;                // 定义并初始化 z
char x = 'x';               // 变量 x 的值为 'x'

/**
数字
c++ 对数学运算加强了
打印数字更加方便
数学头文件 <cmath> 提供了多种数学运算方法
*/
// 数字定义
short  s;
int    i;
long   l;
float  f;
double d;
// 数字赋值
s = 10;      
i = 1000;    
l = 1000000; 
f = 230.47;  
d = 30949.374;
// 数字输出
cout << "short  s :" << s << endl;
cout << "int    i :" << i << endl;
cout << "long   l :" << l << endl;
cout << "float  f :" << f << endl;
cout << "double d :" << d << endl;

double cos(double); //该函数返回弧度角（double 型）的余弦。
double sin(double);//该函数返回弧度角（double 型）的正弦。
double tan(double);//该函数返回弧度角（double 型）的正切。
double log(double);//该函数返回参数的自然对数。
double pow(double, double);//假设第一个参数为 x，第二个参数为 y，则该函数返回 x 的 y 次方。
double hypot(double, double);//该函数返回两个参数的平方总和的平方根，也就是说，参数为一个直角三角形的两个直角边，函数会返回斜边的长度。
double sqrt(double);//该函数返回参数的平方根。
int abs(int);//该函数返回整数的绝对值。
double fabs(double);//该函数返回任意一个浮点数的绝对值。
double floor(double);//该函数返回一个小于或等于传入参数的最大整数

// 随机数
#include <iostream>
#include <ctime>
#include <cstdlib>
using namespace std;
int main ()
{
   int i,j;
   // 生成随机数之前必须先调用 srand() 函数。
   // 设置种子
   //  time() 函数来获取系统时间的秒数
   srand( (unsigned)time( NULL ) );
   /* 生成 10 个随机数 */
   for( i = 0; i < 10; i++ )
   {
      // 生成实际的随机数
      // 如果不设置种子, 将会只返回一个伪随机数
      j= rand();
      cout <<"随机数： " << j << endl;
   }
   return 0;
}

// 常量

// 字符常量
//字符常量是括在单引号中。如果常量以 L（仅当大写时）开头，则表示它是一个宽字符常量（例如 L'x'），此时它必须存储在 wchar_t 类型的变量中。否则，它就是一个窄字符常量（例如 'x'），此时它可以存储在 char 类型的简单变量中

/**

字符串 String 类


C++ 标准库提供了 string 类类型
*/
#include <iostream>
#include <string>
 
using namespace std;
 
int main ()
{
   string str1 = "Hello";
   string str2 = "World";
   string str3;
   int  len ;
 
   // 复制 str1 到 str3
   str3 = str1;
   cout << "str3 : " << str3 << endl;
 
   // 连接 str1 和 str2
   str3 = str1 + str2;
   cout << "str1 + str2 : " << str3 << endl;
 
   // 连接后，str3 的总长度
   len = str3.size();
   cout << "str3.size() :  " << len << endl;
 
   return 0;
}

```


## 存储类型-cpp

```cpp

/**

存储类型:
从 C++ 17 开始，auto 关键字不再是 C++ 存储类说明符，且 register 关键字被弃用。

auto: 用于两种情况：声明变量时根据初始化表达式自动推断该变量的类型、声明函数时函数返回值的占位符

register: 同 c

static: 同 c 
此外, 在 C++ 中，当 static 用在类数据成员上时，会导致仅有一个该成员的副本被类的所有对象共享。

extern: 同 c

mutable: 仅适用于类的对象, 允许对象的成员替代常量。也就是说，mutable 成员可以通过 const 成员函数修改。

thread_local (C++11): 每个线程都有一个该变量的副本

*/

```

## 流程控制

```cpp

/**
循环 流程控制
goto 语句允许把控制无条件转移到同一函数内的被标记的语句。
不建议使用
*/

// 局部变量声明
int a = 10;

// 定义一个 label: LOOP
LOOP:do
{
    if( a == 15)
    {
        // 跳过迭代
        a = a + 1;
        goto LOOP;
    }
    cout << "a 的值：" << a << endl;
    a = a + 1;
}while( a < 20 );

// goto 语句一个很好的作用是退出深嵌套例程
for(...) {
   for(...) {
      while(...) {
         if(...) goto stop;
         .
         .
         .
      }
   }
}
stop:
cout << "Error in program.\n";


```

## 函数-cpp

```cpp

/**

函数


传值调用	该方法把参数的实际值赋值给函数的形式参数。在这种情况下，修改函数内的形式参数对实际参数没有影响。
指针调用	该方法把参数的地址赋值给形式参数。在函数内，该地址用于访问调用中要用到的实际参数。这意味着，修改形式参数会影响实际参数。
引用调用	该方法把参数的引用赋值给形式参数。在函数内，该引用用于访问调用中要用到的实际参数。这意味着，修改形式参数会影响实际参数。

C++ 使用传值调用来传递参数
*/
// 参数默认值
int sum(int a, int b=20)
{
  int result;
  result = a + b;
  return (result);
}

//Lambda 函数与表达式
// 语法:
[capture](parameters)->return-type{body} // 有返回值的 lambda
[capture](parameters){body} // 没有返回值的 lambda

// "[]" 中括号 用来指定外部变量, 以便在表达式内部访问

// []      // 沒有定义任何变量。使用未定义变量会引发错误。
// [x, &y] // x以传值方式传入（默认），y以引用方式传入。
// [&]     // 任何被使用到的外部变量都隐式地以引用方式加以引用。
// [=]     // 任何被使用到的外部变量都隐式地以传值方式加以引用。
// [&, x]  // x显式地以传值方式加以引用。其余变量以引用方式加以引用。
// [=, &z] // z显式地以引用方式加以引用。其余变量以传值方式加以引用。

//对于[=]或[&]的形式，lambda 表达式可以直接使用 this 指针。但是，对于[]的形式，如果要使用 this 指针，必须显式传入：
[this]() { this->someFunc(); }();


[](int x, int y){ return x < y ; }
[]{ ++global_x; } 
[](int x, int y) -> int { int z = x + y; return z + x; }


```

## 指针-cpp

```cpp

/**

    指针
        
声明指针, 最好为指针变量赋一个 NULL 值 (NULL 指针是一个定义在标准库中的值为零的常量)


*/
#include <iostream>
 
using namespace std;
 
int main ()
{
   int  var = 20;   // 实际变量的声明
   int  *ip;        // 指针变量的声明
 
   ip = &var;       // 在指针变量中存储 var 的地址
 
   cout << "Value of var variable: ";
   cout << var << endl;
 
   // 输出在指针变量中存储的地址
   cout << "Address stored in ip variable: ";
   cout << ip << endl;
 
   // 访问指针中地址的值
   cout << "Value of *ip variable: ";
   cout << *ip << endl;
 
   return 0;
}

//检查空指针:
if(ptr)     /* 如果 ptr 非空，则完成 */
if(!ptr)    /* 如果 ptr 为空，则完成 */


//c++ 允许 指向指针的指针。
int  var;
int  *ptr;
int  **pptr;

var = 3000;

// 获取 var 的地址
ptr = &var;

// 使用运算符 & 获取 ptr 的地址
pptr = &ptr;

// 使用 pptr 获取值
cout << "var 值为 :" << var << endl;
cout << "*ptr 值为:" << *ptr << endl;
cout << "**pptr 值为:" << **pptr << endl;

```

## 引用

```cpp

/**

    引用 reference

类似指针，它们之间有三个主要的不同：
- 不存在空引用。引用必须连接到一块合法的内存。
- 一旦引用被初始化为一个对象，就不能被指向到另一个对象。指针可以在任何时候指向到另一个对象。
- 引用必须在创建时被初始化。指针可以在任何时间被初始化

所以, 准确的类比是: 引用相当于第二个变量名

引用通常用于函数参数列表和函数返回值, 比使用指针更方便
*/
int i = 17;
int&  r = i; // r 是一个初始化为 i 的整型引用
double& s = d;


// 引用作为函数参数
// 函数声明
void swap(int& x, int& y);
// 函数定义
void swap(int& x, int& y)
{
   int temp;
   temp = x; /* 保存地址 x 的值 */
   x = y;    /* 把 y 赋值给 x */
   y = temp; /* 把 x 赋值给 y  */
  
   return;
}

// 引用作为函数返回值
//当函数返回一个引用时，则返回一个指向返回值的隐式指针。这样，函数就可以放在赋值语句的左边
//当返回一个引用时，要注意被引用的对象不能超出作用域。所以返回一个对局部变量的引用是不合法的，但是，可以返回一个对静态变量的引用。

double vals[] = {10.1, 12.6, 33.1, 24.1, 50.0};
double& setValues( int i )
{
  return vals[i];   // 返回第 i 个元素的引用
}
int main ()
{
   cout << "改变前的值" << endl;
   for ( int i = 0; i < 5; i++ )
   {
       cout << "vals[" << i << "] = ";
       cout << vals[i] << endl;
   }
   setValues(1) = 20.23; // 改变第 2 个元素
   setValues(3) = 70.8;  // 改变第 4 个元素
   cout << "改变后的值" << endl;
   for ( int i = 0; i < 5; i++ )
   {
       cout << "vals[" << i << "] = ";
       cout << vals[i] << endl;
   }
   return 0;
}


```

## 动态分配内存

```cpp

/**
    new , 类似 malloc()

malloc() 函数在 C 语言中就出现了，在 C++ 中仍然存在，但建议尽量不要使用 malloc() 函数。
new 与 malloc() 函数相比，其主要的优点是，new 不只是分配了内存，它还创建了对象
*/

double* pvalue  = NULL;
// new 用于在堆中动态分配内存
// 如果堆区已被用完，可能无法成功分配内存, 需要检查分配是否成功
if( !(pvalue  = new double ))
{
   cout << "Error: out of memory." <<endl;
   exit(1);
 
}
*pvalue = 29494.99;     // 在分配的地址存储值
cout << "Value of pvalue : " << *pvalue << endl;
delete pvalue;        // 释放 pvalue 所指向的内存

// 数组动态内存分配
// 多维数组 https://www.runoob.com/cplusplus/cpp-dynamic-memory.html
char* pvalue  = NULL;   // 初始化为 null 的指针
pvalue  = new char[20]; // 为变量请求内存
delete [] pvalue;        // 删除 pvalue 所指向的数组


```

## 命名空间

```cpp
//作为附加信息来区分不同库中相同名称的函数、类、变量
#include <iostream>
// using 指令
// 用于指定默认使用的命名空间, 这样在使用命名空间时就可以不用在前面加上命名空间的名称。这个指令会告诉编译器，后续的代码将使用指定的命名空间中的名称。
using namespace std; 
// using 指令也可以用来指定命名空间中的特定项目
// 如: using std::cout;, 随后的代码中，在使用 cout 时就可以不用加上命名空间名称作为前缀，但是 std 命名空间中的其他项目仍然需要加上命名空间名称作为前缀

// 第一个命名空间
// 创建新的命名空间
// or
// 为已有的命名空间增加新的元素
namespace first_space{
   void func(){
      cout << "Inside first_space" << endl;
   }
}
// 第二个命名空间
namespace second_space{
   void func(){
      cout << "Inside second_space" << endl;
   }
}
int main ()
{
 
   // 调用第一个命名空间中的函数
   first_space::func();
   
   // 调用第二个命名空间中的函数
   second_space::func(); 
 
   return 0;
}



// 命名空间嵌套
#include <iostream>
using namespace std;
 
// 第一个命名空间
namespace first_space{
   void func(){
      cout << "Inside first_space" << endl;
   }
   // 第二个命名空间
   namespace second_space{
      void func(){
         cout << "Inside second_space" << endl;
      }
   }
}
using namespace first_space::second_space;
int main ()
{
 
   // 调用第二个命名空间中的函数
   func();
   
   return 0;
}
```

## 模板

```cpp
// 模板是泛型编程的基础

// 函数模板
template <typename type> 
ret-type func-name(parameter list) {.....}
// 类模板
template <class type> 
class class-name {.....} 

// 函数模板 demo
#include <iostream>
#include <string>
using namespace std;
template <typename T>
inline T const& Max (T const& a, T const& b) 
{ 
    return a < b ? b:a; 
} 
int main ()
{
    int i = 39;
    int j = 20;
    cout << "Max(i, j): " << Max(i, j) << endl; 
    double f1 = 13.5; 
    double f2 = 20.7; 
    cout << "Max(f1, f2): " << Max(f1, f2) << endl; 
    string s1 = "Hello"; 
    string s2 = "World"; 
    cout << "Max(s1, s2): " << Max(s1, s2) << endl; 
   return 0;
}

// 类模板 demo
#include <iostream>
#include <vector>
#include <cstdlib>
#include <string>
#include <stdexcept>
using namespace std;
template <class T>
class Stack { 
  private: 
    vector<T> elems;     // 元素 
 
  public: 
    void push(T const&);  // 入栈
    void pop();               // 出栈
    T top() const;            // 返回栈顶元素
    bool empty() const{       // 如果为空则返回真。
        return elems.empty(); 
    } 
}; 
 
template <class T>
void Stack<T>::push (T const& elem) 
{ 
    // 追加传入元素的副本
    elems.push_back(elem);    
} 
 
template <class T>
void Stack<T>::pop () 
{ 
    if (elems.empty()) { 
        throw out_of_range("Stack<>::pop(): empty stack"); 
    }
    // 删除最后一个元素
    elems.pop_back();         
} 
 
template <class T>
T Stack<T>::top () const 
{ 
    if (elems.empty()) { 
        throw out_of_range("Stack<>::top(): empty stack"); 
    }
    // 返回最后一个元素的副本 
    return elems.back();      
} 
 
int main() 
{ 
    try { 
        Stack<int>         intStack;  // int 类型的栈 
        Stack<string> stringStack;    // string 类型的栈 
 
        // 操作 int 类型的栈 
        intStack.push(7); 
        cout << intStack.top() <<endl; 
 
        // 操作 string 类型的栈 
        stringStack.push("hello"); 
        cout << stringStack.top() << std::endl; 
        stringStack.pop(); 
        stringStack.pop(); 
    } 
    catch (exception const& ex) { 
        cerr << "Exception: " << ex.what() <<endl; 
        return -1;
    } 
}

```

## 标准模板库 stl

C++ STL（标准模板库）是一套功能强大的 C++ 模板类，提供了通用的模板类和函数，这些模板类和函数可以实现多种流行和常用的算法和数据结构，如向量、链表、队列、栈。

容器（Containers）	容器是用来管理某一类对象的集合。C++ 提供了各种不同类型的容器，比如 deque、list、vector、map 等。 (https://www.cnblogs.com/Cnxz/p/12313780.html)

算法（Algorithms）	算法作用于容器。它们提供了执行各种操作的方式，包括对容器内容执行初始化、排序、搜索和转换等操作。

迭代器（iterators）	迭代器用于遍历对象集合的元素。这些集合可能是容器，也可能是容器的子集。


```cpp
#include <iostream>
#include <vector>
using namespace std;
 
int main()
{
   // 创建一个向量存储 int
   vector<int> vec; 
   int i;
 
   // 显示 vec 的原始大小
   cout << "vector size = " << vec.size() << endl;
 
   // 推入 5 个值到向量中
   for(i = 0; i < 5; i++){
       // 在向量的末尾插入值，如果有必要会扩展向量的大小
      vec.push_back(i);
   }
 
   // 显示 vec 扩展后的大小
   cout << "extended vector size = " << vec.size() << endl;
 
   // 访问向量中的 5 个值
   for(i = 0; i < 5; i++){
      cout << "value of vec [" << i << "] = " << vec[i] << endl;
   }
 
   // 使用迭代器 iterator 访问值
   // begin( )  返回一个指向向量开头的迭代器, end( ) 函数返回一个指向向量末尾的迭代器
   vector<int>::iterator v = vec.begin();
   while( v != vec.end()) {
      cout << "value of v = " << *v << endl;
      v++;
   }
 
   return 0;
}
```

## 日期时间

```cpp

/**

    日期时间

需要在 C++ 程序中引用 <ctime> 头文件。
有四个与时间相关的类型：clock_t、time_t、size_t 和 tm
*/
struct tm {
  int tm_sec;   // 秒，正常范围从 0 到 59，但允许至 61
  int tm_min;   // 分，范围从 0 到 59
  int tm_hour;  // 小时，范围从 0 到 23
  int tm_mday;  // 一月中的第几天，范围从 1 到 31
  int tm_mon;   // 月，范围从 0 到 11
  int tm_year;  // 自 1900 年起的年数
  int tm_wday;  // 一周中的第几天，范围从 0 到 6，从星期日算起
  int tm_yday;  // 一年中的第几天，范围从 0 到 365，从 1 月 1 日算起
  int tm_isdst; // 夏令时
}

// 关于时间日期的函数
time_t time(time_t *time); //该函数返回系统的当前日历时间，自 1970 年 1 月 1 日以来经过的秒数。如果系统没有时间，则返回 .1。
char *ctime(const time_t *time); //该返回一个表示当地时间的字符串指针，字符串形式 day month year hours:minutes:seconds year\n\0。
struct tm *localtime(const time_t *time); //该函数返回一个指向表示本地时间的 tm 结构的指针。
clock_t clock(void); //该函数返回程序执行起（一般为程序的开头），处理器时钟所使用的时间。如果时间不可用，则返回 .1。
char * asctime ( const struct tm * time ); //该函数返回一个指向字符串的指针，字符串包含了 time 所指向结构中存储的信息，返回形式为：day month date hours:minutes:seconds year\n\0。
struct tm *gmtime(const time_t *time); //该函数返回一个指向 time 的指针，time 为 tm 结构，用协调世界时（UTC）也被称为格林尼治标准时间（GMT）表示。
time_t mktime(struct tm *time); //该函数返回日历时间，相当于 time 所指向结构中存储的时间。
double difftime ( time_t time2, time_t time1 ); //该函数返回 time1 和 time2 之间相差的秒数。
size_t strftime(); //该函数可用于格式化日期和时间为指定的格式。

// 获取当前日期时间
#include <iostream>
#include <ctime>
using namespace std;
int main( )
{
   // 基于当前系统的当前日期/时间
   time_t now = time(0);
   
   // 把 now 转换为字符串形式
   char* dt = ctime(&now);
 
   cout << "本地日期和时间：" << dt << endl; // Sat Jan  8 20:07:41 2011
 
   // 把 now 转换为 tm 结构
   tm *gmtm = gmtime(&now);
   dt = asctime(gmtm);
   cout << "UTC 日期和时间："<< dt << endl; // Sat Jan  8 20:07:41 2011

    //////////////////////////////

   // 格式化时间
   
   // 基于当前系统的当前日期/时间
   time_t now = time(0);
   cout << "1970 到目前经过秒数:" << now << endl;
   tm *ltm = localtime(&now);
   // 输出 tm 结构的各个组成部分
   cout << "年: "<< 1900 + ltm->tm_year << endl;
   cout << "月: "<< 1 + ltm->tm_mon<< endl;
   cout << "日: "<<  ltm->tm_mday << endl;
   cout << "时间: "<< ltm->tm_hour << ":";
   cout << ltm->tm_min << ":";
   cout << ltm->tm_sec << endl;
   //1970 到目前时间:1503564157
    //年: 2017
    //月: 8
   // 日: 24
   // 时间: 16:42:37
}

```

## io-输入输出

```cpp


/**

    io 输入输出

io 库头文件:
<iostream>	该文件定义了 cin、cout、cerr 和 clog 对象，分别对应于标准输入流、标准输出流、非缓冲标准错误流和缓冲标准错误流。
<iomanip>	该文件通过所谓的参数化的流操纵器（比如 setw 和 setprecision），来声明对执行标准化 I/O 有用的服务。
<fstream>	该文件为用户控制的文件处理声明服务。
*/
// 标准输出/输入, 都是 iostream 中定义的实例
#include <iostream>
using namespace std; // cout 这些对象都在 std 空间下
int main( )
{
   char str[] = "Hello C++";
   // "<<" 运算符被重载来输出内置类型（整型、浮点型、double 型、字符串和指针）, 可以多次使用
   // endl 用于在行末添加一个换行符
   cout << "Value of str is : " << str << endl;

   char name[50];
   cout << "请输入您的名称： ";
   cin >> name;
   // 流提取运算符 >> 在一个语句中可以多次使用
   // 如:
   cin >> name >> age;
}

// 标准错误流（cerr）
//cerr 对象附属到标准错误设备，通常也是显示屏，但是 cerr 对象是非缓冲的，且每个流插入到 cerr 都会立即输出。
#include <iostream>
using namespace std;
int main( )
{
   char str[] = "Unable to read....";
   cerr << "Error message : " << str << endl; // Error message : Unable to read....
}

//标准日志流（clog）
//clog 对象附属到标准错误设备，通常也是显示屏，但是 clog 对象是缓冲的。这意味着每个流插入到 clog 都会先存储在缓冲区，直到缓冲填满或者缓冲区刷新时才会输出。
// 用法 和 cerr 类似
// 使用 cerr 流来显示错误消息，而其他的日志消息则使用 clog 流来输出


```

## 面向对象

### 类-对象

```cpp


/**

    类, 对象

class 和 struct 区别:
- 默认的读取权限不同。struct是public,而class是private。
- class可用于声明类模板，而struct不可以

* ---struct好的地方在于，你能够直接从栈中获取数据，而不需要再到堆中去找。

*/
#include <iostream>
 
using namespace std;
 
class Box
{
    // 类访问修饰符
    // private、在类的外部是不可访问的, 如果不指定修饰符,默认 成员都是私有的
    //protected, 类似 private, 但是 保护成员在派生类（即子类）中是可访问的
    //public、 类外部可直接访问
   public:
      static int objectCount;// 声明类的成员为静态时，这意味着成员独立于对象实例存在，无论有多少个实例, 静态成员都只有一个副本
      double length;   // 长度
      double breadth;  // 宽度
      double height;   // 高度
      double getVolume(void); // 声明成员函数, 成员函数把函数声明为内联的，即便没有使用 inline 标识符
      // 可以声明定义在一起
      double getVolume(void)
      {
         return length * breadth * height;
      }
};
// 可以在类内部声明, 外部定义, 借助 "::"
double Box::getVolume(void)
{
    return length * breadth * height;
}

int main( )
{
   Box Box1;        // 声明 Box1，类型为 Box
   Box Box2;        // 声明 Box2，类型为 Box
   double volume = 0.0;     // 用于存储体积
   // box 1 详述
   Box1.height = 5.0; 
   Box1.length = 6.0; 
   Box1.breadth = 7.0;
   // box 2 详述
   Box2.height = 10.0;
   Box2.length = 12.0;
   Box2.breadth = 13.0;
   // box 1 的体积
   volume = Box1.height * Box1.length * Box1.breadth;
   cout << "Box1 的体积：" << volume <<endl;
   // box 2 的体积
   volume = Box2.height * Box2.length * Box2.breadth;
   cout << "Box2 的体积：" << volume <<endl;
   return 0;
}


// 构造函数 : 每次定义/创建对象时运行, 用于初始化对象
            //可重载
//析构函数: 每次删除所创建的对象时执行, 用于释放资源
            // 不可重载
//拷贝构造函数: 作用有 三, (也就是在这三个场景会调用拷贝构造函数); 可重载
//      - 使用同一类型之前创建的对象来初始化新定义的对象
//      - 复制对象把它作为参数传递给函数
//      - 复制对象，并从函数返回这个对象

// - 如果在类中没有定义拷贝构造函数，编译器会自行定义一个。(默认拷贝构造函数, 执行的是浅拷贝, 一旦对象存在了动态成员 (即存在指针时)，那么浅拷贝就会出问题 必须深拷贝)
//      - 为什么会出问题: 因为默认的拷贝构造函数是按成员拷贝构造，这导致了两个不同的指针(如ptr1=ptr2)指向了相同的内存。当一个实例销毁时，调用析构函数 free(ptr1)释放了这段内存，那么剩下的一个实例的指针ptr2就无效了，在被销毁的时候free(ptr2)就会出现错误了
// -  如果类带有指针变量，并有动态内存分配，则它必须有一个拷贝构造函数
// - 拷贝构造函数必须的一个参数是本类型的一个引用变量。(构造函数 和 拷贝构造函数可以合并在一起)
// - 为什么拷贝构造函数必须是引用传递，不能是值传递？- 是为了防止递归引用 (因为如果使用值传递, 拷贝构造函数调用的时候, 实参需要传入一个拷贝, 那么有需要调动拷贝构造函数, 套娃了)

class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line();  // 这是构造函数
      Line(double len);// 带参树构造函数
      ~Line();  // 这是析构函数声明
      Line( const Line &obj);      // 拷贝构造函数, 形式通常这样: classname (const classname &obj) {}

   private:
      double length;
      int *ptr;
};
Line::Line(void)
{
    cout << "Object is being created" << endl;
}
// 也可带有参数
Line::Line( double len)
{
    cout << "Object is being created, length = " << len << endl;
    length = len;
    // 为指针分配内存
    ptr = new double;
    // 为指针赋值
    *ptr = len;
}

void display(Line obj)
{
   cout << "line 大小 : " << obj.getLength() <<endl;
}

// 程序的主函数
int main( )
{
   Line line; // 定义 Line 变量, 调用构造函数, 拷贝构造函数
   Line line1(10);
 
   Line line2 = line1; // 这里也调用了拷贝构造函数

   display(line1); // 调用完后, line1 会被销毁, 调用析构函数
   display(line2);
}

// 使用初始化列表来初始化字段
Line::Line( double len): length(len) // 等价于 代码 length = len;
{
    cout << "Object is being created, length = " << len << endl;
} 
// 若有多个字段 X、Y、Z 需要初始化
C::C( double a, double b, double c): X(a), Y(b), Z(c)
{
  ....
}

// 析构函数定义
Line::~Line(void)
{
    cout << "Object is being deleted" << endl;
    delete ptr;
}
// 拷贝构造函数定义
Line::Line(const Line &obj)
{
    cout << "调用拷贝构造函数并为指针 ptr 分配内存" << endl;
    ptr = new int;
    *ptr = *obj.ptr; // 拷贝值
}

```

### 友元

```cpp

/**

    友元函数
    类的友元函数是定义在类外部 (不属于类成员)，但有权访问类的所有私有（private）成员和保护（protected）成员

友元可以是一个函数，该函数被称为友元函数；友元也可以是一个类，该类被称为友元类，在这种情况下，整个类及其所有成员都是友元
*/
class Box
{
   double width;
public:
   double length;
   friend void printWidth( Box box ); //声明函数为一个类的友元
   void setWidth( double wid );
};


// 请注意：printWidth() 不是任何类的成员函数
void printWidth( Box box )
{
   /* 因为 printWidth() 是 Box 的友元，它可以直接访问该类的任何成员 */
   cout << "Width of box : " << box.width <<endl;
}

// 程序的主函数
int main( )
{
   Box box;
   // 使用成员函数设置宽度
   box.setWidth(10.0);
   // 使用友元函数输出宽度
   printWidth( box );
   return 0;
}

// 声明类 ClassTwo 的所有成员函数作为类 ClassOne 的友元，需要在类 ClassOne 的定义中放置
friend class ClassTwo; 
// 把一个类定义为另一个类的友元类，会暴露实现细节，从而降低了封装性


```

### 内联函数

```cpp

/**

    内联函数

编译器会把内联函数的代码副本替换到每个调用该函数的地方 (而对于其他的函数，都是在运行时候才被替代。) 是空间换时间的思想

- 类成员函数都是内联函数，即使没有使用 inline 说明符。(如 构造函数、析构函数、普通成员函数等）)
- 对于类内声明，在类外定义函数，则可以在类内声明时, 或者在类外定义函数时加上inline关键字
- 内联函数的函数体过大，一般的编译器会放弃内联方式，而采用普通的方式调用函数。一般不要内联超过 10 行的函数, 不要内联含有循环or 流程控制的函数
- 函数即使声明为内联的也不一定会被编译器内联 (使用内联函数，只不过是向编译器提出一个申请，编译器可以拒绝你的申请); 比如虚函数和递归函数就不会被正常内联

内联函数和宏定义的区别:
- 宏是由预处理器对宏进行替代，而内联函数是通过编译器控制来实现的
- 宏定义在使用时只是简单的文本替换，并没有做严格的参数检查，也就不能享受C++编译器严格类型检查的好处

*/
inline int Max(int x, int y)
{
   return (x > y)? x : y;
}

// 程序的主函数
int main( )
{
   cout << "Max (20,10): " << Max(20,10) << endl;// 会被编译器替换为  (x > y)? x : y
   cout << "Max (0,200): " << Max(0,200) << endl;
   cout << "Max (100,1010): " << Max(100,1010) << endl;
   return 0;
}

```

### 函数重载-运算符重载

```cpp

// 函数重载, 运算符重载
// 可以声明几个功能类似的同名函数，但是这些同名函数的形式参数（指参数的个数、类型或者顺序）必须不同。您不能仅通过返回类型的不同来重载函数
// 重载 + 运算符，用于把两个 Box 对象相加
Box operator+(const Box& b)
{
    Box box;
    box.length = this->length + b.length;
    box.breadth = this->breadth + b.breadth;
    box.height = this->height + b.height;
    return box;
}
// https://www.runoob.com/cplusplus/cpp-overloading.html
// 可重载的运算符:
// 双目算术运算符	+ (加)，-(减)，*(乘)，/(除)，% (取模)
// 关系运算符	==(等于)，!= (不等于)，< (小于)，> (大于>，<=(小于等于)，>=(大于等于)
// 逻辑运算符	||(逻辑或)，&&(逻辑与)，!(逻辑非)
// 单目运算符	+ (正)，-(负)，*(指针)，&(取地址)
// 自增自减运算符	++(自增)，--(自减)
// 位运算符	| (按位或)，& (按位与)，~(按位取反)，^(按位异或),，<< (左移)，>>(右移)
// 赋值运算符	=, +=, -=, *=, /= , % = , &=, |=, ^=, <<=, >>=
// 空间申请与释放	new, delete, new[ ] , delete[]
// 其他运算符	()(函数调用)，->(成员访问)，,(逗号)，[](下标)

// 不可重载的运算符:
// .：成员访问运算符
// .*, ->*：成员指针访问运算符
// ::：域运算符
// sizeof：长度运算符
// ?:：条件运算符
// #： 预处理符号

```

### 继承


```cpp

// 类的继承
// 继承时, 可指定 父类的 访问级别:
// public (在 struct 中默认 public 继承): 
// protected
// private (如果继承时不显示声明, 默认就是 private 继承)
// 总结:
继承方式	基类的public成员	基类的protected成员	基类的private成员	继承引起的访问控制关系变化
public继承	仍为public成员	仍为protected成员	不可见	基类的非私有成员在子类的访问属性不变
protected继承	变为protected成员	变为protected成员	不可见	基类的非私有成员都为子类的保护成员
private继承	变为private成员	变为private成员	不可见	基类中的非私有成员都称为子类的私有成员
class Box
{
   protected: // protect 成员, 可被子类访问到
      double width;
};
class SmallBox:Box, public OtherClass // SmallBox 是派生类
{
   public:
      void setSmallWidth( double wid );
      double getSmallWidth( void );
};
// 子类的成员函数
double SmallBox::getSmallWidth(void)
{
    return width ;
}
void SmallBox::setSmallWidth( double wid )
{
    width = wid;
}

// 一个派生类继承了所有的基类方法，但下列情况除外：
//基类的构造函数、析构函数和拷贝构造函数。
//基类的重载运算符。
//基类的友元函数。




// 多重继承
// 问题: 歧义问题
#include <iostream>
using namespace std;
class Dog
{
public:
    virtual void eat() {cout << "The Dog has eaten." << endl;};
};
class Bird
{
public:
    virtual void eat() {cout << "The Bird has eaten." << endl;};
};
class DogBird : public Dog, public Bird
{
};
int main()
{
    DogBird db;
    // 为了消除歧义，要么在 DogBird类重写 eat() 方法，要么显示的指明调用的是哪一个父类的版本。
    static_cast<Dog>(db).eat(); // Slices, calling Dog::eat()
    db.Bird::eat();             // Calls Bird::eat()
    return 0;
}
/*
Output:
The Dog has eaten.
The Bird has eaten.
*/


```

### 多态-虚函数

```cpp

/**

    多态, 虚函数, 纯虚函数(接口)

多态:调用成员函数时，会根据对象具体类型来执行不同的函数
虚函数: 通过 virtual 将父类方法定义为虚函数, 子类重写后, 可以实现多态
纯虚函数: 就是java 中的抽象方法 (如果类中至少有一个函数被声明为纯虚函数，则这个类就是抽象类)
*/
class Shape {
   protected:
      int width, height;
   public:
      Shape( int a=0, int b=0)
      {
         width = a;
         height = b;
      }
      virtual int area() // 多态需要 关键字 virtual 支持
      // 也叫做 虚函数, 在派生类中重新定义基类中定义的虚函数时，会告诉编译器不要静态链接到该函数, 而要动态链接 (后期绑定) 到具体类型对应的函数
      {
         cout << "Parent class area :" <<endl;
         return 0;
      }
      // pure virtual function 纯虚函数
      // 父类中暂时无法给出虚函数具体实现, 就可以定义为纯虚函数
      virtual int area() = 0;
};
class Rectangle: public Shape{
   public:
      Rectangle( int a=0, int b=0):Shape(a, b) { }
      int area ()
      { 
         cout << "Rectangle class area :" <<endl;
         return (width * height); 
      }
};
class Triangle: public Shape{
   public:
      Triangle( int a=0, int b=0):Shape(a, b) { }
      int area ()
      { 
         cout << "Triangle class area :" <<endl;
         return (width * height / 2); 
      }
};
// 程序的主函数
int main( )
{
   Shape *shape;
   Rectangle rec(10,7);
   Triangle  tri(10,5);
 
   // 存储矩形的地址
   shape = &rec;
   // 调用矩形的求面积函数 area
   shape->area();
 
   // 存储三角形的地址
   shape = &tri;
   // 调用三角形的求面积函数 area
   shape->area();
   
   return 0;
}
```

## io-输入输出-cpp

```cpp
//iostream 标准输入输出
//ofstream	该数据类型表示输出文件流，用于创建文件并向文件写入信息。
//ifstream	该数据类型表示输入文件流，用于从文件读取信息。
//fstream	该数据类型通常表示文件流，且同时具有 ofstream 和 ifstream 两种功能，这意味着它可以创建文件，向文件写入信息，从文件读取信息。


// 打开文件
//open() 函数是 fstream、ifstream 和 ofstream 对象的一个成员
// 打开模式:
// ios::app	追加模式。所有写入都追加到文件末尾。
// ios::ate	文件打开后定位到文件末尾。
// ios::in	打开文件用于读取。
// ios::out	打开文件用于写入。
// ios::trunc	如果该文件已经存在，其内容将在打开文件之前被截断，即把文件长度设为 0。
void open(const char *filename, ios::openmode mode);

//想要以写入模式打开文件，并希望截断文件，以防文件已存在
ofstream outfile;
outfile.open("file.dat", ios::out | ios::trunc );

//想要打开一个文件用于读写
ifstream  afile;
afile.open("file.dat", ios::out | ios::in );

// 关闭文件
// close() 函数是 fstream、ifstream 和 ofstream 对象的一个成员。
void close();


// 文件读写 demo
#include <fstream>
#include <iostream>
using namespace std;
int main ()
{
   char data[100];
 
   // 以写模式打开文件
   ofstream outfile;
   outfile.open("afile.dat");
 
   cout << "Writing to the file" << endl;
   cout << "Enter your name: "; 
   cin.getline(data, 100); // 从标准输入读取一行
 
   // 向文件写入用户输入的数据
   outfile << data << endl;
 
   cout << "Enter your age: "; 
   cin >> data;
   cin.ignore(); // 忽略掉之前读语句留下的多余字符
   
   // 再次向文件写入用户输入的数据
   outfile << data << endl;
 
   // 关闭打开的文件
   outfile.close();
 
   // 以读模式打开文件
   ifstream infile; 
   infile.open("afile.dat"); 
 
   cout << "Reading from the file" << endl; 
   infile >> data; 
 
   // 在屏幕上写入数据
   cout << data << endl;
   
   // 再次从文件读取数据，并显示它
   infile >> data; 
   cout << data << endl; 
 
   // 关闭打开的文件
   infile.close();
 
   return 0;
}

// 文件位置指针
// 是一个整数值，指定了从文件的起始位置到指针所在位置的字节数
//  istream 的 seekg（意思 为"seek get"）
//  ostream 的 seekp（"seek put"）。
// seekg 和 seekp 的参数通常是一个长整型。第二个参数可以用于指定查找方向。查找方向可以是 ios::beg（默认的，从流的开头开始定位），也可以是 ios::cur（从流的当前位置开始定位），也可以是 ios::end（从流的末尾开始定位）

// 定位到 fileObject 的第 n 个字节（假设是 ios::beg）
fileObject.seekg( n );
 
// 把文件的读指针从 fileObject 当前位置向后移 n 个字节
fileObject.seekg( n, ios::cur );
 
// 把文件的读指针从 fileObject 末尾往回移 n 个字节
fileObject.seekg( n, ios::end );
 
// 定位到 fileObject 的末尾
fileObject.seekg( 0, ios::end );
```

## 异常处理-cpp

```cpp
// 抛出异常
double division(int a, int b)
{
   if( b == 0 )
   {
      throw "Division by zero condition!";
   }
   return (a/b);
}
// 捕获异常
int main ()
{
   int x = 50;
   int y = 0;
   double z = 0;
 
   try {
     z = division(x, y);
     cout << z << endl;
   }
   //由于抛出了一个类型为 const char* 的异常，因此，当捕获该异常时，我们必须在 catch 块中使用 const char*
   // 想让 catch 块能够处理任何类型的异常，则必须在括号内使用省略号 ... (三个点)
   catch (const char* msg) {
     cerr << msg << endl;
   }
 
   return 0;
}


// 标准异常
// C++ 提供了一系列标准的异常，定义在 <exception> 中, 具有层级结构:
std::exception	该异常是所有标准 C++ 异常的父类。
    std::bad_alloc	该异常可以通过 new 抛出。
    std::bad_cast	该异常可以通过 dynamic_cast 抛出。
    std::bad_exception	这在处理 C++ 程序中无法预期的异常时非常有用。
    std::bad_typeid	该异常可以通过 typeid 抛出。
    std::logic_error	理论上可以通过读取代码来检测到的异常。
        std::domain_error	当使用了一个无效的数学域时，会抛出该异常。
        std::invalid_argument	当使用了无效的参数时，会抛出该异常。
        std::length_error	当创建了太长的 std::string 时，会抛出该异常。
        std::out_of_range	该异常可以通过方法抛出，例如 std::vector 和 std::bitset<>::operator[]()。
    std::runtime_error	理论上不可以通过读取代码来检测到的异常。
        std::overflow_error	当发生数学上溢时，会抛出该异常。
        std::range_error	当尝试存储超出范围的值时，会抛出该异常。
        std::underflow_error	当发生数学下溢时，会抛出该异常

// 自定义异常
//可以通过继承和重载 exception 类来定义新的异常
#include <iostream>
#include <exception>
using namespace std;
struct MyException : public exception
{
    // what() 是异常类提供的一个公共方法，它已被所有子异常类重载
  const char * what () const throw ()
  {
    return "C++ Exception";
  }
};
 
int main()
{
  try
  {
    throw MyException();
  }
  catch(MyException& e)
  {
    std::cout << "MyException caught" << std::endl;
    std::cout << e.what() << std::endl;
  }
  catch(std::exception& e)
  {
    //其他的错误
  }
}
```

## 信号处理

```cpp
// 信号是由操作系统传给进程的中断，会提早终止一个程序
//这些信号是定义在 C++ 头文件 <csignal> 中。
// 有哪些信号:
SIGABRT	程序的异常终止，如调用 abort。
SIGFPE	错误的算术运算，比如除以零或导致溢出的操作。
SIGILL	检测非法指令。
SIGINT	接收到交互注意信号。
SIGSEGV	非法访问内存。
SIGTERM	发送到程序的终止请求


// 捕获信号
// 这个函数接收两个参数：第一个参数是一个整数，代表了信号的编号；第二个参数是一个指向信号处理函数的指针。
void (*signal (int sig, void (*func)(int)))(int); 
// 生成信号
// SIGINT、SIGABRT、SIGFPE、SIGILL、SIGSEGV、SIGTERM、SIGHUP
int raise (signal sig);

// 捕获信号
#include <iostream>
#include <csignal>
#include <unistd.h>
using namespace std;
void signalHandler( int signum )
{
    cout << "Interrupt signal (" << signum << ") received.\n";
    // 清理并关闭
    // 终止程序  
   exit(signum);  
}
int main ()
{
    // 注册信号 SIGINT 和信号处理程序
    //  Ctrl+C 来中断程序
    signal(SIGINT, signalHandler);  
    while(1){
       cout << "Going to sleep...." << endl;
       sleep(1);
    }
    return 0;
}

// 生成信号
#include <iostream>
#include <csignal>
#include <unistd.h>
using namespace std;
void signalHandler( int signum )
{
    cout << "Interrupt signal (" << signum << ") received.\n";
    // 清理并关闭
    // 终止程序 
   exit(signum);  
}
int main ()
{
    int i = 0;
    // 注册信号 SIGINT 和信号处理程序
    signal(SIGINT, signalHandler);  
    while(++i){
       cout << "Going to sleep...." << endl;
       if( i == 3 ){
          raise( SIGINT);
       }
       sleep(1);
    }
    return 0;
}

```

## 多线程

```cpp


```

## web 开发

公共网关接口（CGI）


# qt 框架

https://www.bookstack.cn/read/qt-study-road-2/b173882cafb6bb28.md

不再需要研究 STL，不再需要 C++ 的 `<string>`，不再需要到处去找解析 XML、连接数据库、访问网络的各种第三方库，因为 Qt 自己内置了

提供 gui 库

# 开发工具

Qt Creator, CLion, vscode


# debug 工具

valgrind, 用来分析程序错误

# 拾遗

## 指针的指针和二维数组的区别

https://www.zhihu.com/question/40973588/answer/91761240

```cpp
#include<stdio.h>
  2 int main (void)
  3 {
  4     int a[2][3] = {{1,2,3},{4,5,6}};
  5     int ** array_ptr = a; // 错误, a 是一个指向 a 这个数组第一个元素的指针, 而第一个元素还是一个数组, 所以 a 是一个指向数组的指针; array_ptr 代表一个指针的地址, 即指向指针的指针, 明显不是指向数组的指针
         // 改成这样既可:
         int (*array_ptr)[3] = a;

         
  6     printf("%d\n",a[0][0]);
  8     printf("%p\n",a);
  9     printf("%p\n",*a);
 10     printf("%p\n",array_ptr);
 11     printf("%p\n",*array_ptr);
 12     return 0;
 13 }
```
