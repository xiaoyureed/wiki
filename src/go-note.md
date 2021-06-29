---
title: Golang
tags:
  - golang
date: 2017-12-14 17:06:33
categories: computer science
---

https://github.com/hackstoic/golang-open-source-projects  中文版awesome-go

official doc: http://docs.studygolang.com/doc/
中文 http://docscn.studygolang.com/

dev web app by golang: https://slides.com/akshaydeo/writing-web-apps-in-golang

https://github.com/unknwon/the-way-to-go_ZH_CN

实现一个简单的 video server : https://github.com/xiaoyureed/video_server

tips: https://zhuanlan.zhihu.com/p/27518650, https://blog.gaobinzhan.com/category/22/

https://github.com/astaxie/build-web-application-with-golang

https://github.com/wyh267/FalconEngine 简单搜索引擎


https://github.com/TimothyYe 小工具

https://github.com/geektutu/7days-golang 手写中间件

https://awesome-go.com/

https://github.com/geektutu/high-performance-go性能分析

<!--more-->

<!-- TOC -->

- [特性](#特性)
- [环境配置](#环境配置)
  - [install](#install)
  - [IDE](#ide)
- [quickstart](#quickstart)
- [basic](#basic)
- [标准库](#标准库)
  - [http](#http)
- [工程管理](#工程管理)
  - [go mod 自定义包](#go-mod-自定义包)
    - [使用方法 相关命令](#使用方法-相关命令)
    - [GO111MODULE](#go111module)
    - [如何导入自定义包](#如何导入自定义包)
- [golang 命令工具使用](#golang-命令工具使用)
  - [有哪些命令](#有哪些命令)
  - [交叉编译](#交叉编译)
  - [测试](#测试)
- [开发命令行程序](#开发命令行程序)
- [goroutine原理](#goroutine原理)
  - [协程怎么理解](#协程怎么理解)
  - [协程在内存层面的实现](#协程在内存层面的实现)
- [开发微服务](#开发微服务)

<!-- /TOC -->


# 特性

- 完整的开发工具链 （tools， test，benchmark，builtin。。。）
- 部署简单（直接编译生成二进制文件， 丢到服务器就可以了）
- 自带的http库and模板引擎
- 优秀的并发模型；

# 环境配置

## install

https://studygolang.com/dl

对于 Linux:

```sh
# 下载压缩包
curl -L https://studygolang.com/dl/golang/go1.14.6.linux-amd64.tar.gz --output xxx.tar.gz
# 解压到 /usr/local
tar -C /usr/local -zxvf go1.14.6.tar.gz 
# 添加到 path, 
# export GOPATH=/opt/gopath
# export GOROOT=/usr/lib/golang
# export GOBIN=$GOROOT/bin
# export PATH=$PATH:$GOBIN

# vim /etc/profile
# centos 要加到这个文件里, 才能所有用户登录都执行 go verison  有效
vi /etc/bashrc

source /etc/profile

# 验证
go version

# 国内代理 参考 https://goproxy.io/zh/
# https://goproxy.cn
# or https://athens.azurefd.net/

```

对于 macOS , 最简单的是直接下载安装包, 无需任何设置, 当然, 如果要配合 vscode , 需要设置 goroot, gopath

## IDE

vscode + go 插件


安装 go 插件. 然后会有提示, 安装一些工具到 gopath/bin 下. 所以需要 设置 GOPATH (单纯 go 使用无需 gopath)

编辑 /etc/profile

```sh
GOPATH=/root/gopath
GOROOT=/usr/local/go
export PATH=$PATH:$GOROOT/bin

go env -w GO111MODULE=on
go env -w GOPROXY=https://goproxy.io,direct

# 设置不走 proxy 的私有仓库，多个用逗号相隔（可选）
#go env -w GOPRIVATE=*.corp.example.com

# 设置不走 proxy 的私有组织（可选）
#go env -w GOPRIVATE=example.com/org_name

```

# quickstart


编写代码

```go
package main

import (
  "flag" // 专门处理命令行参数的工具包
  "fmt"
  "os"
)

func main() {
  cmd := os.Args[0]            //命令行第一个值, 即 “当前命令”
  fmt.Println("cmd: ", cmd)    //D:\gopath\src\cli-example\cli-example.exe
  argCount := len(os.Args[1:]) // 命令行参数个数
  fmt.Println("arg count: ", argCount)
  //遍历打印参数
  for i, v := range os.Args[1:] {
    fmt.Printf("参数%d: %s\n", i+1, v)
  }

  var port int
  flag.IntVar(&port, "port", 8080, "端口, 默认 8080") // 为名称为 port 的flag赋值, 设置默认值8080
  flag.Parse()                                    // 开始解析
  fmt.Println("端口号: ", port)
  /*
    $ ./cli-example.exe -port=9000 // 如果指定了 port , 则为指定值
    端口号:  9000
  */
}

```

运行: 三种方法

- 通过 IDE 的启动按钮

- `go run app.go`

- 先编译, 再运行  `go build xxx.go` + `./xxx.exe` (可执行文件名 为 go 文件名)

	go build 若不加任何参数, 则是编译 main 包, 可执行文件名 为 包名






# basic

```go
package main //Programs start running in package main.
// main 包 和 main 函数 告诉 go编译器: 这是一个可执行程序, 需要编译为二进制文件

// 远程包: import "github.com/xiaoyureed/xxx" ; 会先搜索 gopath, 如果没有, 会使用 go get github.com/xiaoyureed/xxx 下载到本地
// 包重命名: import myfmt "mylib/fmt"
// 特殊的重命名: import _ "mylib/fmt" 不使用这个包, 但是执行包的init()
//
// import "xxx_package" --------- 单个包导入
import (
	"bytes"
	"fmt"
	"io"
	"math"
	"math/cmplx" // 代码中用到的只是 cmplx, 而不是 math/cmplx, by convention, package name == last element of "import path"
	"runtime"
	"strings"
	"sync"
	"time"

	"xiaoyureed.github.io/hello/xx" //go代码文件名没有出现, 直接通过包名调用导出的函数
)

// 每个package 中的 init() 总是最先执行
func init() {
	fmt.Println("init() executed")
}

// 编译生成的二进制文件名: main 函数 所在 的go文件 的名字
func main() {
	fmt.Println("hello", "world")
	xx.Hello() // 使用自定义包

	// beego.Run()

	basicType()

	funcDemo() // 变量or方法都必须大写才能在包外访问

	closureDemo()

	stringDemo()

	pointerDemo()

	structDemo()

	structMethodReceiver()

	arrayAndSlice()

	interfaceDemo()

	typeAssert()

	typeSwitch()

	mapDemo()
	mapInitDemo()

	rangeDemo()

	//////////////////////////////

	conditionControl()

	ioDemo()

	goroutines()

	channelDemo()

	syncMutexDemo()
}

//Basic types
/* golang 的基本类型
bool

string

有符号 int (32 or 64 bit, os 决定)  int8  int16  int32  int64
无符号 uint (32 or 64 bit, os 决定) uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8, 不支持中文

rune // alias for int32
	 // represents a Unicode code point , 所以支持中文

uintptr //无符号整型，用于存放一个指针

float32 float64

complex64 complex128 实数+虚数
*/
func basicType() {
	// 定义变量, 赋值
	// var c, python, java bool
	// var x, y int = 1, 2

	// 字符串
	var s string = "hello"
	s1 := "world" // 省略的写法, 只能在函数内部使用, 这样声明的变量可以重复赋值, 常用来接收 err
	fmt.Println("s + s1 = ", s, s1)
	// 类型推断
	// i := 42           // int
	// f := 3.142        // float64
	// g := 0.867 + 0.5i // complex128

	// 声明常量
	const World = "世界" // 注意 Constants cannot be declared using the := syntax.

	//  多个变量声明也可以这样
	var (
		ToBe   bool       = false
		MaxInt uint64     = 1<<64 - 1
		z      complex128 = cmplx.Sqrt(-5 + 12i)
	)
	println(ToBe, MaxInt, z)

	var boo1 bool = false
	fmt.Println("boo1 = ", boo1)

	//
	// 数字类型
	//
	// int、uint 和 uintptr 位数有系统决定
	//
	var a int8 = 1
	var b, c int16 = 2, 3
	println("a = ", a)
	println("b = ", b, ", c = ", c)
	var d, e int32 // 先声明
	d, e = 5, 6    // 再使用
	println("d = ", d, ", e = ", e)

	//Zero values 零值
	var i int
	var f float64
	var x bool
	var y string
	// %q 存疑?
	fmt.Printf("%v %v %v %q\n", i, f, x, y) //0 0 false ""

	//Type conversions 类型转换
	//The expression T(v) converts the value v to the type T.
	var zz int = 42
	var w float64 = float64(i)
	var u uint = uint(f)
	// 在func 里可以更简单
	// i := 42
	// f := float64(i)
	// u := uint(f)

	fmt.Println(zz, w, u) //42 42 42

}

func stringDemo() {
	var s1 = "hello world \n"
	s2 := `hello world \n`
	fmt.Println(s1) // 输出换行, "\n"被解释成换行
	fmt.Println(s2) // 不换行, "\n" 原样输出
	// 长度
	fmt.Println(len(s1)) // 13
	// 第一个字符, 用字节类型接收, 所以打印对应的 ascii 码
	fmt.Println(s1[0]) // 104
	// 原样打印字符
	fmt.Printf("%c\n", s1[0]) //h

	// 修改
	// s1[0] = "X" // 报错, string 类型不可修改
	// 先转为字节数组再修改
	strArr := []byte(s1)
	// strArr[0] = "X" // 报错 : cannot use "X" (type string) as type byte in assignment
	strArr[0] = 'X'             // 字符, 不是 字符串
	fmt.Println(string(strArr)) //Xello world

	// 遍历
	str := "hello 世界" // 1个中文占3byte
	for i, l := 0, len(str); i < l; i++ {
		var c byte = str[i]
		// 对应index, ascii, char
		fmt.Printf("%d - %v - %c\n", i, c, c) // 有乱码
	}
	// 正确的遍历
	for i, c := range str {
		fmt.Printf("%d - %v - %c\n", i, c, c)
	}

	// byte.Buffer 类似 Java 中的 Stringbuffer
	var buffer bytes.Buffer
	buffer.WriteString("hello")
	fmt.Println(buffer.String())
}

// Pointers 指针
//A pointer holds the memory address of a value.保存着一个value的内存地址
// &变量 - 根据变量获得指针
// *指针   - 根据指针获取变量
// var t *int  - 声明一个指针类型变量 , "*Type" 表示某个类型的指针
// 零值为 nil.
func pointerDemo() {
	i, j := 42, 2701

	p := &i         // point to i
	fmt.Println(*p) // read i through the pointer
	*p = 21         // set i through the pointer
	fmt.Println(i)  // see the new value of i

	p = &j         // point to j
	*p = *p / 37   // divide j through the pointer
	fmt.Println(j) // see the new value of j

}

//// Struct
// A struct is a collection of fields.
func structDemo() {
	// 可在 func 中, 也可在func外定义
	type MyStruct struct {
		X int
		Y int
	}

	// struct init
	myStruct := MyStruct{1, 2}
	fmt.Println(myStruct) //{1 2}
	// access field by variable
	fmt.Println(myStruct.X) // 1
	// access field by pointer
	p := &myStruct
	fmt.Println(p.Y) // 2 ; "(*p).X" 中的 "*" 被省略

	//Struct Literals : 通过列出指定field来为 struct 中的某个相应 field 重新赋值
	//语法
	// var v = MyStruct{FieldName: fieldValue ...}
	type Vertex struct {
		X, Y int
	}
	var (
		v1 = Vertex{1, 2}  // has type Vertex
		v2 = Vertex{X: 1}  // Y:0 隐式的, y被赋值为零值
		v3 = Vertex{}      // X:0 and Y:0
		p1 = &Vertex{1, 2} // has type *Vertex, p 为指针类型
	)

	fmt.Println(v1, p1, v2, v3) //{1 2} &{1 2} {1 0} {0 0}

}

////////////////////////////////
//
// Methods & 接收器 receiver
//
//指定函数的接收器 - -  只是在 "func" 关键字 和 "func name"之间 添加了 "接收器"
// 接收器语法: 类似方法参数
// 这个接收器所在的方法必须和 type 定义在同一个包下
type vertex1 struct {
	X, Y float64
}

// 接收器可以是任意类型, 这里指定为 Vertex1, 相当于给Vertex1添加方法
func (v vertex1) abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
func structMethodReceiver() {
	v := vertex1{3, 4}
	fmt.Println("structMethod", v.abs()) //5

	methodContinue()

	pointerReceiverDemo()

	paramAndReceiverPointerDemo()

}

///////////////////////

//MyFloat : Methods continued
// 接收器可以是任意类型, 比如 float64, 下面的 func中 的接收器就是这个类型
type MyFloat float64

// Abs 到处必须大写
func (f MyFloat) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

func methodContinue() {
	var f MyFloat = -2.2
	println(f.Abs())     //+2.200000e+000
	fmt.Println(f.Abs()) //2.2
}

////////////////////////////////////////

// Pointer receivers 指针接收器
//相比于 value receiver 使用更加广泛
//对比 pointer receiver vs. value receiver:
//pointer receiver指针指向原始值(可以修改原始值); 而 value receiver 只是原始值的 "一份复制",不可能修改原始值(和function参数性质一样)
// 指针接收器 使用场景:
//1. 希望通过接收器修改指向的变量的值; 2. 希望避免在每次方法调用时复制值, 尤其当receiver是一个big struct 的时候
type PointerVertex struct {
	X, Y float64
}

// 使用普通接收器
func (v PointerVertex) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

// 使用指针接收器
// 可以直接修改 pointer 指向的原始值;
//这个function作用: 放大 f 倍
func (v *PointerVertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
func pointerReceiverDemo() {
	v := PointerVertex{3, 4}
	v.Scale(10)
	fmt.Println(v.Abs()) //50
}

/////////////////////////////////////

//
// pointer 作为func的 "参数" and "接收器"
// 区别: 定义时指针作为 "参数" - 则函数调用时, 必须给 pointer
//       定义时指针作为 "接收器" - 函数调用时, 给 pointer 或 普通变量 均可
//
// 相应的: 普通变量 作为func的 "参数" and "接收器" - 区别和上面类似 https://tour.golang.org/methods/7
type ParamAndReceiverPointerVertex struct {
	X, Y float64
}

func (v *ParamAndReceiverPointerVertex) Scale(f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
func ScaleFunc(v *ParamAndReceiverPointerVertex, f float64) {
	v.X = v.X * f
	v.Y = v.Y * f
}
func paramAndReceiverPointerDemo() {
	v := ParamAndReceiverPointerVertex{3, 4}
	v.Scale(2)        //虽然定义时接收器是指针, 但是这里接收器可以是普通变量
	ScaleFunc(&v, 10) // 定义时参数是指针, 这里必须是传指针
	//ScaleFunc(v, 10)  // Compile error! 如果是变量则报错

	p := &ParamAndReceiverPointerVertex{4, 3}
	p.Scale(3)      // 接收器可以是pointer
	ScaleFunc(p, 8) // 参数必须是指针

	fmt.Println(v, p) //{60 80} &{96 72}
}

//////////////////////////////////

func arrayAndSlice() {

	// Arrays
	// An array's length is part of its type, so arrays cannot be resized 容量无法更改
	// 语法
	// var a [10]int
	// a := [3]int{2, 1, 3} // 初始化/ array literal
	// q := []int{2, 3, 5, 7, 11, 13} //Slice literals 是初始化数组更好的方式(不必指定长度),底层是 先 正常创建一个数组, 然后创建一个数组的 slice
	var a [2]string
	a[0] = "Hello"
	a[1] = "World"
	fmt.Println(a[0], a[1]) //Hello World
	fmt.Println(a)          //[Hello World]

	primes := [6]int{2, 3, 5, 7, 11, 13} // 定义&初始化 /array literal
	fmt.Println(primes)                  //[2 3 5 7 11 13]

	//
	//
	// Slices
	// array 的 切片/视图, 比array好用多了;
	// 语法
	// var slice []T
	// mySlice := myArray[beginIndex:endIndex]  // 左闭右开
	// 通过内建函数 make()
	// a := make([]int, 5)  // len(a)=5
	// b := make([]int, 0, 5) // len(b)=0, cap(b)=5
	//添加元素, 通过内建函数 func append(s []T, vs ...T) []T
	// slice 被修改, 则原始 array 也会相应改变 (slice就像数组的部分的引用)
	// Nil slices  - zero value == nil
	var s []int = primes[1:4]
	fmt.Println(s) //[3 5 7]

	// slice defaults  - slice的默认index
	/* 对于数组 var a [10]int , 以下是等同的
	   a[0:10]
	   a[:10]
	   a[0:]
	   a[:]
	*/

	//
	//struct slice
	ss := []struct {
		i int
		b bool
	}{
		{2, true},
		{3, false},
		{5, true},
		{7, true},
		{11, false},
		{13, true},
	}
	fmt.Println(ss) //[{2 true} {3 false} {5 true} {7 true} {11 false} {13 true}]

	// 二维 slice 初始化
	board := [][]string{
		{"_", "_", "_"}, // []string 可省略
		[]string{"_", "_", "_"},
		[]string{"_", "_", "_"},
	}
	fmt.Println(board) //[[_ _ _] [_ _ _] [_ _ _]]

	sliceLengthAndCapacity()

	makeSliceDemo()

	sliceAppendingDemo()
}

//Slice length and capacity
// length - 切片包含的 element 个数, 切片改变的就是这个变量
// capacity - 切片的容量: 即基础数组中元素的数量，从切片中的第一个元素开始计算, 到 原始数组的末尾
// 通过 len(s) and cap(s) 获得
// 这两者关系: 只要 capacity 足够, 可以通过 re-slice 来延长切片的 length
func sliceLengthAndCapacity() {
	s := []int{2, 3, 5, 7, 11, 13}
	printSlice(s) //len=6 cap=6 [2 3 5 7 11 13]

	// Slice the slice to give it zero length.
	zeroSlice := s[:0]
	printSlice(zeroSlice) //len=0 cap=6 []

	// Extend its length.
	s = zeroSlice[:4] // s 被重新赋值
	printSlice(s)     //len=4 cap=6 [2 3 5 7]

	// Drop its first two values.
	// 注意 s 已经被重新赋值了
	s = s[2:]
	printSlice(s) //len=2 cap=4 [5 7]

	// nil slice
	var sli []int
	fmt.Println(sli, len(sli), cap(sli)) //[] 0 0
	fmt.Println(sli == nil)              // true
}
func printSlice(s []int) {
	fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
}

// Creating a slice with make
// make([]int, xxLenCap)
// make([]int, xxLen, xxCap)
func makeSliceDemo() {
	a := make([]int, 5) // 创建的是 zeroed array 的一个slice;
	printSlice2("a", a) //a len=5 cap=5 [0 0 0 0 0]

	b := make([]int, 0, 5)
	printSlice2("b", b) //b len=0 cap=5 []

	c := b[:2]
	printSlice2("c", c) //c len=2 cap=5 [0 0]

	d := c[2:5]
	printSlice2("d", d) //d len=3 cap=3 [0 0 0]
}
func printSlice2(s string, x []int) {
	fmt.Printf("%s len=%d cap=%d %v\n",
		s, len(x), cap(x), x)
}

// Appending to a slice 向 slice 添加元素
func sliceAppendingDemo() {
	var s []int
	printSlice(s) //len=0 cap=0 []

	// append works on nil slices.新的容量更大的 array 被创建, 返回它的 slice
	s = append(s, 0)
	printSlice(s) //len=1 cap=1 [0]

	// The slice grows as needed.
	s = append(s, 1)
	printSlice(s) //len=2 cap=2 [0 1]

	// We can add more than one element at a time.
	s = append(s, 2, 3, 4)
	printSlice(s) //len=5 cap=6 [0 1 2 3 4]
}

/////////////////////////////////////////////

// map - 键值对, 无序
//Map literals
//Map literals continued
// 声明: var m map[keyType]valueType
// 插入: m[key] = elem
// retrieve检索: elem = m[key]
// 是否存在: elem, ok = m[key] 如果key存在, ok==true, 否则false, 此时elem==zero value
// 删除: delete(m, key)
// zero value == nil
func mapDemo() {
	type Vertex struct {
		Lat, Long float64
	}
	var m map[string]Vertex

	m = make(map[string]Vertex)
	m["key"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["key"]) //{40.68433 -74.39967}
}

// Map literals - map的初始化
func mapInitDemo() {
	type Vertex struct {
		Lat, Long float64
	}

	var m = map[string]Vertex{
		"Bell Labs": { // Vertex 可以省略
			40.68433, -74.39967,
		},
		"Google": Vertex{
			37.42202, -122.08408,
		},
	}
	// Map literals continued - 更简单的初始化写法
	m2 := map[string]Vertex{
		"Bell Labs": {40.68433, -74.39967},
		"Google":    {37.42202, -122.08408},
	}

	fmt.Println(m) //map[Bell Labs:{40.68433 -74.39967} Google:{37.42202 -122.08408}]
	fmt.Println(m2)
}

//////////////////////////////////////////

// range
//Range continued
//迭代 slice 或 map, 返回 index 和 value(value的复制, 不是引用)
// 语法
// for index, value := range sliceName {}
func rangeDemo() {
	var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}
	for i, v := range pow {
		fmt.Printf("index=%d, value=%d\n", i, v)
	}
	// Range continued
	// 只想使用 index
	for i := range pow {
		pow[i] = 1 << uint(i) // == 2**i
	}
	// 只想使用 value
	for _, value := range pow {
		fmt.Printf("%d\n", value)
	}
}

//////////////////////////////////////
func funcDemo() {

	//Functions continued
	//// 多个参数类型相同, 可共享类型
	add1 := func(x, y int) int {
		return x + y
	}
	println(add1) // 0x4ce678

	//Multiple results
	//多个返回值
	swap := func(x, y string) (string, string) {
		return y, x
	}
	println(swap) //0x4ce680

	//Named return values
	// 被命名的 "返回值" - 直接 "return"
	// 只适合在短函数中使用, 因为 They can harm readability in longer functions.
	split := func(sum int) (x, y int) {
		x = sum * 4 / 9
		y = sum - x
		return
	}
	println(split)

	// 函数作为参数传递
	// 这个函数接受一个 函数, 用来处理 参数 "3" "4"
	compute := func(fn func(float64, float64) float64) float64 {
		return fn(3, 4)
	}
	hypot := func(x, y float64) float64 { // 求直角三角形第三边
		return math.Sqrt(x*x + y*y)
	}
	fmt.Println(hypot(5, 12))      //13
	fmt.Println(compute(hypot))    //5
	fmt.Println(compute(math.Pow)) //81

	//
	// 值传递 vs 引用传递
	// 大都是值传递, 如基本类型 byte,int,bool,string, 复合类型: 数组,数组切片,结构体,map,channnel
	//对于 "指针" 是 "引用传递"
	//
	array := [3]int{0, 1, 2}
	// 数组是值传递
	var array2 = array
	array2[2] = 5
	fmt.Println(array, array2) // [0 1 2] [0 1 5]

	// 指针是引用传递
	var arrPointer = &array
	arrPointer[2] = 5
	fmt.Println(array, *arrPointer) // 打印指针变量无法使用 println(..)

}

func closureDemo() {

	// Function closures 闭包
	// 闭包是一个 function value，它引用来自其体外的变量。
	// 闭包构成了一个独立的context, 有了 "状态"
	adder := func() func(int) int {
		sum := 0 // 一直维持在内存中, 会造成累加效果
		// 返回一个闭包
		return func(x int) int {
			sum += x // 访问体外的变量sum
			return sum
		}
	}

	pos, neg := adder(), adder()
	for i := 0; i < 5; i++ {
		fmt.Println(
			pos(i),
			neg(-2*i),
		)
		/*
			0 0
			1 -2
			3 -6
			6 -12
			10 -20
		*/
	}

}

//
////////////////////////////////
//
//
// interface - 一组方法签名
// 一个接口变量可以接受这样的值: 这些值必须通过"接收器"实现接口中的方法
//通过接口调用具体方法: https://tour.golang.org/methods/11
// 接口 == nil  https://tour.golang.org/methods/12 所以并不会报 null pointer exception
type Abser interface {
	Abs() float64
}
type MyFloatInterfaceDemo float64

//
//为MyFloatInterfaceDemo类型添加 Abs(), 表示 MyFloatInterfaceDemo 实现了 Abser 接口
// 类似 Python 中的 duck type
func (f MyFloatInterfaceDemo) Abs() float64 {
	if f < 0 {
		return float64(-f)
	}
	return float64(f)
}

//
type VertexInterfaceDemo struct {
	X, Y float64
}

//
// 为 VertexInterfaceDemo 指针类型 添加 Abs();
// 注意 不是变量类型, 单纯的VertexInterfaceDemo类型还是不能赋值给 interface变量
func (v *VertexInterfaceDemo) Abs() float64 {
	return math.Sqrt(v.X*v.X + v.Y*v.Y)
}

func interfaceDemo() {
	var a Abser // 定义 interface 类型变量
	var b Abser
	v := VertexInterfaceDemo{3, 4}         // 定义 VertexInterfaceDemo 类型的 struct
	f := MyFloatInterfaceDemo(-math.Sqrt2) // 定义 MyFloatInterfaceDemo 类型变量

	// &v 是指针, 作为指针, 实现了 abser interface, 所以可以接收
	a = &v // a *Vertex implements Abser
	// f 作为普通变量, 实现了 abser interface
	b = f // a MyFloat implements Abser

	// In the following line, v is a Vertex (not *Vertex) and does NOT implement Abser.
	// 错误, 因为 v 是普通变量, 作为普通变量 v 并没有实现 Abser interface
	//a = v

	// 正确
	// ?
	b = &f

	fmt.Println(a.Abs()) //5
	fmt.Println(b.Abs()) //1.4142135623730951

	//
	//
	//empty interface 空接口 - 空接口可以接受任意类型变量
	//例如 fmt.Print方法可以接受任意类型变量, 定义时就是使用的interface{}类型
	//
	describe := func(i interface{}) {
		fmt.Printf("(%v, %T)\n", i, i) // 打印值, 类型
	}

	var i interface{}
	describe(i) //(<nil>, <nil>)

	i = 42
	describe(i) //(42, int)

	i = "hello"
	describe(i) //(hello, string)

	//
	//
	stringerInterface()

	//
	//
	errorInterface()
}

//
// Stringer interface - 类似 Java 中的 toString() - 定义在 fmt 包, 只有一个方法: String() string
type Person struct {
	Name string
	Age  int
}

// 实现 Stringer接口
func (p Person) String() string {
	return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}
func stringerInterface() {
	a := Person{"Arthur Dent", 42}
	z := Person{"Zaphod Beeblebrox", 9001}
	fmt.Println(a, z) //Arthur Dent (42 years) Zaphod Beeblebrox (9001 years)
}

//
// error interface - 类似 Stringer 接口, 只有一个方法: Error() string
type MyError struct {
	When time.Time
	What string
}

// 实现 error 接口
// 之后 就可以 被 error 接收
func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s",
		e.When, e.What)
}
func errorInterface() {
	run := func() error {
		return &MyError{
			time.Now(),
			"it didn't work",
		}
	}

	if err := run(); err != nil {
		fmt.Println(err) //at 2018-10-16 12:56:02.8149885 +0800 CST m=+0.092996101, it didn't work
	}
}

// Type assertions 类型断言 - 提供对 接口变量 具体类型的访问
// 语法: t := i.(T)
func typeAssert() {
	var i interface{} = "hello"

	s := i.(string)
	fmt.Println(s) //hello

	s, ok := i.(string)
	fmt.Println(s, ok) ////hello true

	f, ok := i.(float64)
	fmt.Println(f, ok) //0 false

	// f = i.(float64) // 触发 panic
	// fmt.Println(f)
}

// Type switches  - 类型断言的升级版: 多个类型断言写在一起
func typeSwitch() {
	do := func(i interface{}) {
		switch v := i.(type) { //type 是关键字(固定写法)
		case int:
			fmt.Printf("Twice %v is %v\n", v, v*2)
		case string:
			fmt.Printf("%q is %v bytes long\n", v, len(v))
		default:
			fmt.Printf("I don't know about type %T!\n", v)
		}
	}

	do(21)      //Twice 21 is 42
	do("hello") //"hello" is 5 bytes long
	do(true)    //I don't know about type bool!

}

func conditionControl() {

	// if 条件判断
	//条件判断语句里面允许声明一个变量，这个变量的作用域只能在该条件逻辑块内
	// 开根号
	// func sqrt(x float64) string { //这里只是演示 if 用法
	// 	if x < 0 {
	// 		return sqrt(-x) + "i"
	// 	}
	// 	// 输出到字符串
	// 	return fmt.Sprint(math.Sqrt(x)) //1.4142135623730951 2i
	// }

	//if 后面可以跟短句
	// if v := math.Pow(x, n); v < lim { // 如果小于 lim 返回结果
	// 	return v
	//   }

	// for loop 循环
	sum := 0
	for i := 0; i < 10; i++ { // The init and post statements are optional.首尾可选
		sum += i
	}
	fmt.Println(sum) // 45

	// 没有 while, 只有 for
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)

	//Forever
	// 死循环
	// for {
	// }

	// 另一种死循环
	// ch := make(chan int)
	// do sth
	// go goroutine
	// <-ch // 这里会阻塞住

	//
	//goto
	//用goto跳转 必须跳转到 在当前函数内定义的标签
	//标签名是大小写敏感的
	i := 0
Here: //这行的第一个词，以冒号结束作为标签
	println("goto ->", i)
	i++
	if i < 3 {
		goto Here //跳转到Here去
	}

	//
	// switch 就是 if else 的简便写法
	//Switch evaluation order: Switch cases evaluate cases from top to bottom, stopping when a case succeeds.
	fmt.Print("Go runs on ")
	switch os := runtime.GOOS; os { // 跟 if 一样, 条件前可跟 一条语句
	case "darwin":
		fmt.Println("OS X.")
		// 这里无需跟break
	case "linux":
		fmt.Println("Linux.")
	// case f() // 可以是函数
	default:
		// freebsd, openbsd,
		// plan9, windows...
		fmt.Printf("%s.", os)
	}

	// switch 不带条件, 相当于 switch true
	// 可以用来代替 long if-then-else chains
	t := time.Now()
	switch {
	case t.Hour() < 12:
		fmt.Println("Good morning!")
	case t.Hour() < 17:
		fmt.Println("Good afternoon.")
	default:
		fmt.Println("Good evening.")
	}

	//
	// defer
	// 语法
	// defer xxxStatement
	// defer xxxfunc(arguments)
	//推迟执行, arguments 会被立即正常执行, 但是 这个func会被推迟执行(until the surrounding function returns.)
	// Stacking defers: 推迟执行的func都存储到哪里了?
	//  都被存储到一个 stack 中 , 遵循 last-in-first-out order
	deferParam := func() string {
		fmt.Println("参数被执行")
		return "世界"
	}
	defer fmt.Println("world")      // 先进后出
	defer fmt.Println(deferParam()) // 参数立即执行, 即deferParam() 立即运行

	fmt.Println("hello")
	//参数被执行
	// hello
	// 世界
	// world

}

func ioDemo() {

	// reader - The io package specifies the io.Reader interface
	//有一个方法: func (T) Read(b []byte) (n int, err error)
	r := strings.NewReader("Hello, Reader!")

	b := make([]byte, 8)
	for {
		// n 表示读取了多少长度
		n, err := r.Read(b)
		fmt.Printf("n = %v err = %v b = %v\n", n, err, b)
		fmt.Printf("b[:n] = %q\n", b[:n])
		if err == io.EOF {
			break
		}
	}
	/*
		n = 8 err = <nil> b = [72 101 108 108 111 44 32 82]
		b[:n] = "Hello, R"
		n = 6 err = <nil> b = [101 97 100 101 114 33 32 82]
		b[:n] = "eader!"
		n = 0 err = EOF b = [101 97 100 101 114 33 32 82]
		b[:n] = ""
	*/

	// image -  Package image defines the Image interface
	// https://tour.golang.org/methods/24
	/*
	   有三个方法:
	   ColorModel() color.Model
	   Bounds() Rectangle
	   At(x, y int) color.Color
	   **/
}

//Goroutines
//go routine 轻量级的thread, 十几个goroutine可能体现在底层就是五六个线程
//Go语言内部帮你实现了这些goroutine之间的内存共享 // 也就是说 goroutine运行在相同的地址空间
// 语法: go f(x, y, z)
func goroutines() {
	say := func(s string) {
		for i := 0; i < 5; i++ {
			time.Sleep(100 * time.Millisecond)
			fmt.Println(s)
		}
	}

	go say("world")
	say("hello")

	/* runtime包中有几个处理goroutine的函数:
	   Goexit

	   退出当前执行的goroutine，但是defer函数还会继续调用

	   Gosched

	   让出当前goroutine的执行权限，调度器安排其他等待的任务运行，并在下次某个时候从该位置恢复执行。

	   NumCPU

	   返回 CPU 核数量

	   NumGoroutine

	   返回正在执行和排队的任务总数

	   GOMAXPROCS

	   用来设置可以并行计算的CPU核数的最大值，并返回之前的值。

	*/
}

// channel
//无缓冲 channel 是在多个 goroutine之间同步的好工具
//goroutine运行在相同的地址空间，因此访问共享内存必须做好同步
//
/**语法
 ch := make(chan int) // 声明&创建
 c chan int						// 声明
ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
close(ch) // 关闭 channel
*/
// 默认情况下, channel接收和发送数据都是阻塞的
// 比如, 任何发送动作（如 ch<-5）将会被阻塞，直到前一个数据被读出; 任何读出动作 (如 value := <-ch) 将会被阻塞, 直到下一个数据被发送
//发送和接收块会在另一侧准备好的情况下进行(如果对方还没准备好, 就 block)。这样就允许goroutine在没有显式锁或条件变量的情况下进行同步
//
//Buffered Channels 缓冲 channel
// 语法: ch := make(chan int, 100)
//
//sender 可以关闭 channel 通过 close(xxxChannel), 表示传输终止
// receiver 可以检测 channel是否被关闭通过 v, ok := <-ch
// 只有 sender 可以 close channel, 而不是 receiver
// channel不是 文件, 一般无需关闭, 只有 receiver 需要被告知 "没有更多的 value 会被传过来了" 才需要关闭(比如: 终结 range channel)
func channelDemo() {
	sum := func(s []int, c chan int) {
		sum := 0
		for _, v := range s {
			sum += v
		}
		c <- sum // send sum to c
	}

	////对切片中的数字求和，在两个goroutine之间分配工作。两个goroutine完成计算后，它会计算最终结果
	s := []int{7, 2, 8, -9, 4, 0}

	c := make(chan int)
	go sum(s[:len(s)/2], c) // 用一个 goroutine 计算前半部分
	go sum(s[len(s)/2:], c) // 计算后半部分
	// receive from c ; x 为后半段和, y 为前半段和
	x, y := <-c, <-c // 阻塞在这里, 直到新开的 goroutine 将计算结构塞入 chan

	fmt.Println(x, y, x+y) //-5 17 12

	// 缓冲队列
	ch := make(chan int, 2)
	ch <- 1
	ch <- 2
	fmt.Println(<-ch) //1
	fmt.Println(<-ch) //2

	//
	// range 循环读取 channel
	//
	// 被 select 替代了, 因为 range channel不是异步的, 是顺序的会阻塞,
	//而且 如果channel中没有数据流动了, 会一直阻塞, 而 select 提供了 default 选项
	//
	fibonacci := func(n int, c chan int) {
		x, y := 0, 1
		for i := 0; i < n; i++ {
			c <- x
			x, y = y, x+y
		}
		close(c) // 关闭 channel
	}

	c1 := make(chan int, 10)
	go fibonacci(cap(c1), c1)
	for i := range c1 { //// range xxxChan 会循环接收 channel中的元素, 直到 sender 关闭 channel
		fmt.Print(i, " ") //0 1 1 2 3 5 8 13 21 34
	}
	println()

	//
	//
	// select -------- 循环读取 channel 更好的选择
	//
	//select 用于选择不同类型的通讯
	// 通过select可以监听多个channel上的数据流动
	// select默认是阻塞的，只有当监听的channel中有发送或接收可以进行时才会运行
	//当多个channel都准备好的时候，select是随机的选择一个执行的
	// default就是当监听的channel都没有准备好的时候，默认执行的（select不再阻塞等待channel）
	fibonacci1 := func(c, quit chan int) {
		x, y := 1, 1
		for {
			select {
			case c <- x: // 如果成功将 x 塞入 c 中
				x, y = y, x+y
			case <-quit: // 如果接受到结束标志
				fmt.Println("quit")
				return
			default: // 当所有 channel block时执行
				fmt.Println(">>> all channel block")
			}

		}
	}
	cha := make(chan int)
	quit := make(chan int)
	go func() {
		for i := 0; i < 10; i++ {
			fmt.Println(<-cha)
		}
		quit <- 0 // 发出结束标志
	}()
	fibonacci1(cha, quit)

	//
	//// 使用 select  处理 goroutine 超时
	//
	ch1 := make(chan int)
	ch2 := make(chan bool)
	go func() {
		for {
			select {
			case v := <-ch1:
				println(v)

			case <-time.After(2 * time.Second): // 等待 2s后执行
				//time.After(2 * time.Second) 返回一个channel, 等待2s 后塞入数据
				println("timeout")
				ch2 <- true
				break
			}
		}
	}()
	<-ch2 // block until 2s later
}

//
///////////////////////////////////////////
//
// sync.Mutex ---------- 加锁
//
// 实现 goroutine 互斥, 有两个方法: Lock(), Unlock()
//sync.Mutex一旦被锁住，其它的Lock()操作就无法再获取它的锁，只有通过Unlock()释放锁之后才能通过Lock()继续获取锁。
// SafeCounter is safe to use concurrently.
type SafeCounter struct {
	v   map[string]int
	mux sync.Mutex
}

// Inc increments the counter for the given key.
func (c *SafeCounter) Inc(key string) {
	c.mux.Lock() // 开始锁定
	// Lock so only one goroutine at a time can access the map c.v.
	c.v[key]++
	c.mux.Unlock() // 释放锁
}

// Value returns the current value of the counter for the given key.
func (c *SafeCounter) Value(key string) int {
	c.mux.Lock() // 开始锁定
	// Lock so only one goroutine at a time can access the map c.v.
	defer c.mux.Unlock() // 释放锁
	return c.v[key]
}

func syncMutexDemo() {
	c := SafeCounter{v: make(map[string]int)} // mux 无需初始化
	for i := 0; i < 1000; i++ {
		go c.Inc("somekey")
	}
	time.Sleep(time.Second)
	fmt.Println(c.Value("somekey"))
}

///////////////////////////////////////////
//读写锁
//
//RWMutex是基于Mutex的，在Mutex的基础之上增加了读、写的信号量，并使用了类似引用计数的读锁数量
// 可以同时申请多个读锁
// 有读锁时申请写锁将阻塞，有写锁时申请读锁将阻塞
// 只要有写锁，后续申请读锁和写锁都将阻塞
//
//func (rw *RWMutex) Lock()
// func (rw *RWMutex) Unlock() //Lock()和Unlock()用于申请和释放写锁, 如果不存在写锁，则Unlock()引发panic
//
// func (rw *RWMutex) RLock()
// func (rw *RWMutex) RUnlock() // RLock()和RUnlock()用于申请和释放读锁  // 一次RUnlock()操作只是对读锁数量减1，即减少一次读锁的引用计数, 如果不存在读锁，则RUnlock()引发panic
//
// func (rw *RWMutex) RLocker() Locker  // RLocker()用于返回一个实现了Lock()和Unlock()方法的Locker接口

////////////////////////////////////
//
//sync.Once 执行一次
//
// var once sync.Once
// once.Do(fn1)// fn1执行一次
// once.Do(fn2) // fn2不会执行
// 实现单例模式

```

# 标准库

## http

原理:

```go
// 路由如何实现的?
// 默认路由器中有个 map[string]muxEntry, muxEntry {pattern, Handler}, Handler 接口 就是处理业务逻辑的
// Handler包含一个 ServeHTTP(ResponseWriter, *Request) 方法, 只有实现这个方法, 才是 Handler

// 最关键的点:
type HandlerFunc func(ResponseWriter, *Request) 
// ServeHTTP calls f(w, r).
func (f HandlerFunc) ServeHTTP(w ResponseWriter, r *Request) {
	f(w, r)
}
// 这样, 我们实现自己的 Handler 时就无需手动实现 ServeHTTP 方法了
```

# 工程管理

## go mod 自定义包

包管理: Golang的包管理经过了多种工具的演变，从go vendor，到 godep，再到dep. 从go v1.11开始支持的go Modules。

推荐 go mod, 告别 GOPATH


go modules 好处:

- 不必须将项目目录放在GOPATH中
- 项目内会生成一个go.mod文件，列出包依赖, 自动下载依赖包
- 不使用vendor目录，而是统一安装到$GOPATH/pkg/mod/cache
- 所有引入进来的第三方包会准确的指定版本号
- 对于已经转移的包，可以用replace 申明替换，不需要改代码


### 使用方法 相关命令

1. 在 gopath 外部新建一个 folder 作为模块目录(名称不限, 一般指定为 模块名称 如 hello)

1. 进入后 `go mod init xiaoyureed.github.io/hello` (xiaoyureed.github.io 表示模块发布的路径, hello 表示模块名), 生成 go.mod 文件, 内容包含: module name (即xiaoyureed.github.io/hello), go version, requires, 类比 package.json

1. 新建 main 文件, 引入 `import "github.com/astaxie/beego"`, 然后 main 函数中 `beego.Run()`, 直接运行 `go run main.go` 

    或者先编译 `go build`, 生成 main 文件, ./main 执行 (同时, go build 后产生一个名为go.sum的文件, 类比 package.lock)

    依赖包会下载到 `$GOPATH/pkg/mod` 下


```sh
# 生成模块, 就是在当前目录下生成一个 go.mod
# go.mod 文件的出现定义了它所在的目录为一个模块。一个项目中，不同文件夹都可以有go.mod 
# 包括 mod name, go version
go mod init <mod name>

# 下载modules到本地$GOPATH/pkg/mod和 ​$GOPATH/pkg/sum
go mod download

# 下载指定 mod, 没有 version 就是下载最新版
go get github.com/gogf/gf@version
# 指定分支
go get github.com/gogf/gf@master



# 编辑 go.mod
go mod edit
# 查看帮助
go help mod edit

# 验证依赖是否正确
go mod verify
# 以文本模式打印模块需求图
go mod graph
# 删除错误或者不使用的modules
go mod tidy
# 生成vendor目录
go mod vendor
# 查找依赖
go mod why
# 清理moudle 缓存
go clean -modcache
# 查看可下载版本
go list -m -versions github.com/gogf/gf
```


### GO111MODULE

可以把项目放在$GOPATH/src下吗? 可以, go会根据GO111MODULE的值而采取不同的处理方式
- auto 自动模式下 (默认)，项目在$GOPATH/src里会使用$GOPATH/src的依赖包，在$GOPATH/src外，就使用go.mod 里 require的包
- on 开启模式，1.12后，无论在$GOPATH/src里还是在外面，都会使用go.mod 里 require的包
- off 关闭模式，就是老规矩。

### 如何导入自定义包

这就要说到模块名称的作用: 用来引用当前项目内的其他包, 如: 在项目下新建目录 utils，创建一个tools.go文件, package utils. 那么 在 main.go 中就能使用 `import "<mod name>/utils"` 来引用 utils 包


三方库版本号规则?  就是包发布到 github 标记的 tag，格式为 vn.n.n (n代表数字), 在 github 仓库的release 可以看到. 如果包的作者还没有标记版本，默认为 v0.0.0, 在 go.mod 中指定, 若没有指定, 默认 为 latest

依赖地址失效怎么办: 在 go.mod 中 `replace golang.org/x/text => github.com/golang/text latest` (前者表示要替换的地址, 后者表示新的有效地址). 原理就是下载http://github.com/golang/text 的最新版本到 $GOPATH/pkg/mod/golang.org/x/text下



# golang 命令工具使用
https://hyper0x.github.io/go_command_tutorial/#/0.13

## 有哪些命令

`go <cmd> <args>`

```sh
# 编译
# 编译整个项目
go build
# 编译当前目录
go build .

go build hello.go
go build github.com/xiaoyureed/stringutil
go build github.com/xiaoyureed/stringutil/...

go env # 环境变量 GOARCH=amd64 GOOS=windows ...

# 跨平台编译需要先设定 env 值
env GOOS=linux GOARCH=amd64 go build

go clean # 清除编译文件... 用法类似 go build

go run main.go  # 运行, 必须是 package main, 有 main 函数

go install  # 编译同时安装到 gopath 下的 bin 或 pkg

# 下载github第三方包到 gopath, 末尾不带版本号则使用最新, 默认 master 分支上的代码, -u 更新 -v 显示进度
go get -u github.com/xiaoyureed/xxx 

go fmt  # 格式化 用法类似 go build
go vet  # 错误检查 用法类似 go build
go test # 单元测试 用法类似 go build； -v 同时打印信息

```



## 交叉编译

 (ref: https://studygolang.com/articles/13760)

```
GOOS：目标平台的操作系统（darwin、freebsd、linux、windows） 
GOARCH：目标平台的体系架构（386、amd64、arm） 
交叉编译不支持 CGO 所以要禁用它

# Windows 下编译 Mac 和 Linux 64位可执行程序
SET CGO_ENABLED=0
SET GOOS=darwin
SET GOARCH=amd64
go build main.go
 
SET CGO_ENABLED=0
SET GOOS=linux
SET GOARCH=amd64
go build main.go


# Linux 下编译 Mac 和 Windows 64位可执行程序
CGO_ENABLED=0 GOOS=darwin GOARCH=amd64 go build main.go
CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build main.go

# Mac 下编译 Linux 和 Windows 64位可执行程序
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build main.go
CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build main.go

```

## 测试

对于普通test, `go test .`

1. 文件名以 "_test.go"结尾 （规范，否则 `go test` 出错）
1. 文件必须包含测试函数
1. 测试函数名以 "Test"开头, 接受一个 "*testing.T" 类型的参数 (必须,否则不执行这个 test case)

对于 benchmark test 是特殊的test, 用于测试性能, `go test -bench=.`, 更多细节:https://www.cnblogs.com/davygeek/p/7741616.html

1. 函数 以 "Benchmark" 开头
1. benchmark函数一般会跑 b.N 次(不是确定的), 以每次花费的时间代表性能

```go
package main

import (
  "fmt"
  "testing" // 测试包
)

// 方法名必须大写, 以 Test开头
func TestXxx(t *testing.T) {

  // t.SkipNow// 跳过当前 方法, 必须在第一行

  // 可以保证 test 的顺序
  t.Run("sub test1", func(t *testing.T) {
    fmt.Println("sub test1")
  })
  t.Run("sub test2", func(t *testing.T) {
    fmt.Println("sub test2")
  })

  if false {
    t.Errorf("stm goes wrong")
  }
}

// 小写不会执行
// 小写的test一般作为 sub test
func testXxx(t *testing.T) {
  t.Errorf("testXxx 没执行")
}

// 待 性能测试的函数
func btest() int {
  xx := 20 + 30
  return xx
}

// benchmark 性能测试
func BenchmarkXxx(b *testing.B) {
  for n := 0; n < b.N; n++ { // b.N 为默认提供的数值 不是定值, 会根据测试代码的执行时间动态调整直到执行时间达到稳态, 最后打印出的实践是每次执行的平均时间
    btest() // 测试代码的执行时间必须能够达到稳态, 否则benchmark会永远执行不完
  }
}

// 可以通过 TestMain做一些初始化工作, 数据库连接, 文件打开, restful登陆...
// TestMain 不是必须的
func TestMain(m *testing.M) {

  fmt.Println("TestMain 最先执行")
  m.Run() // 必须调用, 否则其他test不会执行
}


```

# 开发命令行程序

https://www.cnblogs.com/tianlongtc/articles/8841530.html

# goroutine原理

[相关面试题](https://yushuangqi.com/blog/2017/golang-mian-shi-ti-da-an-yujie-xi.html?from=groupmessage&isappinstalled=0)

https://www.cnblogs.com/wdliu/p/9272220.html

goroutine的本质是协程, 对应到Java， 则是 线程

## 协程怎么理解

Coroutine 就是一个可以手动调度的, 可以控制暂停返回多次, 并能恢复执行的函数 (所以普通函数是协程的一个特例, 没有暂停,只能返回一次)

其调度过程类似 cpu 对于系统线程的调度, 只不过调度人从 CPU 换为了程序员, 所以协程也称为用户态线程

和线程的异同：

- 一个线程可以多个协程，一个进程也可以单独拥有多个协程

- 线程进程都是同步机制，而协程则是异步

- 协程能保留上一次调用时的状态，每次过程重入时，就相当于进入上一次调用的状态， 所以上下文的切换非常快

- 多个线程相对独立，有自己的上下文，`切换受系统控制`；而协程也相对独立，有自己的上下文，但是`其切换由程序员手动控制`，如: 若从当前协程切换到其他协程则由当前协程来控制。因此，没有线程切换的开销

- 协程不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了



因为协程是一个线程执行，那怎么利用多核CPU呢？最简单的方法是多进程+协程，既充分利用多核，又充分发挥协程的高效率，可获得极高的性能

## 协程在内存层面的实现

协程是可以被暂停/恢复的函数, 所以暂停时需要将函数上下文(在函数栈区)复制一份保存到一个安全的地方 (堆)

或者进一步, 省掉复制的步骤, 为协程分配空间时, 直接在堆中分配

# 开发微服务

```
从一线回老家五线城市，因为钱少（5k左右），IT部门走的只剩我一个人。本来职责是随便维护一下，但以前996养成的工作习惯根本闲不住。

想用什么语言框架都可以，爽的飞起。

我接手的时候是java ssh老项目，还好文档齐全，后面用go重写，加入了gitlab ci持续化集成，目前微服务基于docker k8s gomicro grpc，etcd做服务发现，最近在调研istio。保留了部分java代码是因为netty性能比较好，还有就是wso2的物联网部分。

数据这一块cdh全家桶，spark hbase kafka hive flume ，主要语言是scala，最近准备把实时计算的spark streaming 切到flink。其实一开始不准备用go的，当时觉得scala一种语言就够了，个人很喜欢akka，函数式的库scalaz cats也很有意思，奈何公司网络不好，sbt有时候resolve一上午都没动静

```