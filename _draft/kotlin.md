---
title: kotlin
tags: kotlin
categories: computer science
---

<!--more-->


# what is kotlin

由JetBrains 开发的静态语言, 服务端, Android (Google官方推荐)均可

可以和 Java 互操作

Kotlin可以编译成Java字节码，也可以编译成JavaScript

可以编写脚本, `*.kts`文件

# 优缺点

和 Java 对比

优点:

- 更容易使用

    更多的语法特性

    比如, data类自动生成 getter setter

    比如, kotlin特有的扩展属性, 方便的看出String类都存在哪些方法

    比如, 类型推断

    通过扩展方法给java提供了很多诸如fp之类的特性,但同时始终保持对java的兼容.

- 工程规范性更好

    @Override 关键字 强制约束

    TODO处的代码运行时会异常

- 更强大的跨平台

    kotlin一样可以既可以编译成class字节码，又可以编译成js。

缺点:

- 开发包apk会变大.一方面会增加kotlin的一些库文件,另一方面kotlin编译的类增加了一些方法,

- 其他的一些bug: https://blog.csdn.net/csdnnews/article/details/80746096

    - 名称遮蔽: 呈现包含关系的的多个作用域中, 可以存在同名变量, 小作用域内的变量会遮蔽大作用域内的变量, java 中会直接报错
    - 和 java 互操作是, 可能空异常
    - 类名称字面常量: LocalDate::class 表示 kotlinClass, LocalDate::class.java 表示javaClass, 其实这个不算 bug , 只是过于繁琐
    - 伴生对象 (companion object): 静态变量/方法需要使用 伴生对象, 繁琐
    

# 作为脚本使用

全局安装 kotlin, `kotlin -version`, `Kotlinc -script xxx.kts`

[使用Kotlin编写Gradle脚本](https://www.jianshu.com/p/d08b1305ea3f) -- ide 支持不好

# 语法特性

```java
// 没有;分号
// 没有 new 关键字
// 定义字符使用单引号 var char_1:Char= 'a'
// Kotlin中没有基本数据类型,真正的是万物皆是对象
// 方法定义 fun 方法名(参数值:参数类型) : 返回值类型{}
// Kotlin所有的类都是继承自 Any,相当于java Object , open 修饰可以继承的类
//默认final，在Spring中很容易出错，我知道可以使用all open插件来解决

// 支持 var, val
// 类型推断

// 数组定义
// ByteArray => 表示字节型数组
// IntArray => 表示整型数组
// LongArray => 表示长整型数组
var arr1 = arrayOf(1,2,3,4,5) //等价于[1,2,3,4,5
var arr3 = arrayOfNulls(3) // null null null 不赋值在默认情况下就全部为空
arr3[0] = 10
// 数组工厂
var arr4 = Array(5,{index -> (index * 2).toString() }) //表示数组有5个元素,为0 2 4 6 8,Kotlin支持并提倡使用lambada表达式
// 打印数组
if (i in array.indices) {print(array[i])}

// null 空处理
val userName = a?.b?.c?.d ?: ""
message?.let { println(it) } // it关键字允许引用let开头的对象

//使用字符串模版简化字符串拼接
val info = "name:$name,age:$age"


// 更直观的相等
val john1 =Person（“John”）
val john2 =Person（“John”）
john1 == john2 // true（结构相等）
john1 === john2 // false（引用相等

// 循环 for
for (i in 1..4) print(i) // 顺序不可颠倒
for (i in 0 until 5){} // 左闭右边开
for (i in 10 until 16 step 2)// 设置步长
for (i in "abg") {} // a b c

// 三元运算符
var numB: Int = if ( numA > 2 ) 3 else 5 // 

// 默认方法参数
fun doSomething（title：String，width：Int = 800，height：Int = 600） : String { 
}

// 快速创建List/Map集合类型
// 不可变集合
val list = listOf("a","b","c")
// 可变集合
val list = mutableListOf("a","b","c")

val map = mapOf("key1" to "a", "key2" to "b")
val map = mutableMapOf("key1" to "a", "key2" to "b")

// 属性 getter setter
class User {
    var name :String?=null
}

// 默认构造函数
// 数据类 同时有了构造函数, getter setter
class User(var name: String)
data class User(val username: String, val age: Int)

// 主构造函数
// constructor是关键字,但是一般情况下省略,
class Test constructor(num : Int){
    init {
        num = 5
        println("num = $num")
    }
}

// 次构造函数
// 这里是为了代码清晰，故而没有隐藏constructor关键字
class Test constructor(num: Int){
    init {
        println("num = $num")
    }
    constructor(num : Int, num2: Int) : this(num) {
        println(num + num2)
    }
}
fun main(args: Array<String>) {
    var test1 = Test(1)
    var test2 = Test(1,2)
}
// 当类的主构造函数都存在默认值时的情况,编译器将生成一个额外的无参数构造函数，它将使用默认值
class Test constructor(num1: Int = 10 , num2: Int = 20){
 
    init {
        println("num1 = $num1\t num2 = $num2")
    }
 
    constructor(num1 : Int = 1, num2 : Int = 2, num3 : Int = 3) : this(num1 , num2){
        println("num1 = $num1\t num2 = $num2 \t num3 = $num3")
    }
}
fun main(args: Array<String>) {
    var test = Test()
    var test1 = Test(1,2)
    var test2 = Test(4,5,6)
}


// 属性调用简化, apply/use、let...//todo
val user = User()
with(user)
{
    name = "jerry"
    age = 18
}

//解构对象(Destructuring Declarations)
data class User(val username: String, val age: Int)
fun main() {
    val user = User("Jerry", 18)
    val (username, age) = user
    println("username:$username,age:$age")
}

// 接口使用
interface Demo1Interface{
   // 定义的方法
    fun fun1()
}
class Demo1 : Demo1Interface{
    override fun fun1() {
        println("我是接口中的fun1方法")
    }
}

//简化单例对象构造
object User {
    fun test()
    {
    }
}

//使用when进行分支判断(替代Java中switch, if)
var id = 1
when (id) {
    1 -> print("x == 1")
    2 -> print("x == 2")
    2,3 -> print("x == 2或者3")
    else -> {
        print("id 不是 1 ，也不是 2")
    }
}
when {
    firstName == "Dan" -> person.team = programmers
    lastName  == "Dihiansan" -> person.team = designers
    else -> person.team = others
}

// lambda 表达式 闭包 closure
// 和 groovy 的对比: https://blog.csdn.net/Gdeer/article/details/102793948
val sum = { x: Int, y: Int -> x + y }   // type: (Int, Int) -> Int
val res = sum(4,7)                      // res == 11

// 拓展函数
// 允许将方法添加到类中，而无需修改其源代码。这意味着再也不用写utils了
if (name.contains(firstName, true)) { ... }





// 协程coroutines 还是实验特性
//协程最主要的优点还不在于减少代码,而在于使调用逻辑简化,使得我们不必使用AsyncTask或者RxJava这样复杂的方案


////////////////////////////////// 安卓 Android


//视图绑定 安卓自动绑定xml文件中控件定义
//不再需要使用findViewById或ButterKnife,
import kotlinx.android.synthetic.main.activity_main.*
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        .....
        textView.text = "hello"
    }
}

//简化Parcelable实现
import android.os.Parcelable
import kotlinx.android.parcel.Parcelize
@Parcelize
class User(var age: Int,var name: String): Parcelable



```
