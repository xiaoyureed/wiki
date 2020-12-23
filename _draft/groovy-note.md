---
title: groovy note
tags: [groovy]
categories: computer science
---

jvm上的动态语言, 语法类似Java, 过渡到 ruby

Groovy in Action》第二版

<!--more-->

<!-- TOC -->

- [why groovy](#why-groovy)
    - [场景](#场景)
    - [优缺点](#优缺点)
- [有哪些项目在使用](#有哪些项目在使用)
- [怎么使用](#怎么使用)
    - [idea 环境搭建](#idea-环境搭建)
    - [和 Java 的集成](#和-java-的集成)
        - [GroovyClassLoader](#groovyclassloader)
        - [GroovyShell](#groovyshell)
        - [GroovyScriptEngine](#groovyscriptengine)
        - [优化](#优化)
    - [maven 中 Java 和 groovy 混合开发](#maven-中-java-和-groovy-混合开发)
    - [groovy 语法](#groovy-语法)
        - [和 Java 的不同](#和-java-的不同)
        - [输入输出流io](#输入输出流io)
        - [有用的工具类](#有用的工具类)
        - [元编程](#元编程)
            - [动态增加方法](#动态增加方法)
- [底层运行原理](#底层运行原理)
    - [Groovy代码文件与class文件的对应关系](#groovy代码文件与class文件的对应关系)

<!-- /TOC -->

# why groovy

## 场景

做规则引擎、流程引擎，可以做动态脚本环境，尤其是那些不需要发布又经常变更的场合

groovy有几个很强大的类GroovyClassLoader、GroovyScriptEngine，所以很适合用来做hotfix、hotswap、hotdeploy的东西

当胶水用的，比如连接各个Java组件，或者去实现一些易变性配置，比如用Groovy去写Maven插件，或者测试Java模块的测试例

## 优缺点

优点:

- 减少代码，开发效率高，可以热加载

- 对 集合, 正则, xml 支持非常好

- 完全兼容Java语法, 能够和Java很好的结合, 语法简单多了

缺点:

- 性能问题

    可以通过Groovy的静态编译 (通过添加注解@CompileStatic启用) 或者调用Java程序解决。

    如果静态编译, 会牺牲语言的动态特性

# 有哪些项目在使用

Gradle, 使用 groovy 语法构建项目, android 项目在用

Spock，测试框架，可通过其特有的DSL编写测试案例, 简单方便;

Grails，Web开发框架，无需自行编写脚手架代码，可用来快速开发；类比 Ruby on rails

Griffon，Swing开发框架，其灵感来自于Grails

jenkins dsl plugin ...通过groovy生成所有jenkins jobs 实现CICD as code

spring boot + groovy template 进行 web 开发 (https://www.logicbig.com/tutorials/spring-framework/spring-boot/groovy-view.html, https://www.ibm.com/developerworks/cn/java/j-pg02155/)

参考 http://www.groovy-lang.org/ecosystem.html

# 怎么使用

## idea 环境搭建

idea 自带对 groovy 的编译支持, 智能提示, 但是运行还是要maven依赖的支持 (为了防止位置错误, 推荐 spring boot 集成)

通过 如下 按需引入: 

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.codehaus.groovy</groupId>
            <artifactId>groovy-all</artifactId>
            <version>3.0.2</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

<dependencies>
    <dependency>
        ... 按需引入
    </dependency>
</dependencies>
```

或者直接引入所有, 包括 json, xml 处理等等...:

```xml
<dependency>
    <groupId>org.codehaus.groovy</groupId>
    <artifactId>groovy-all</artifactId>
    <version>3.0.2</version>
</dependency>
```

脚本位置: 可以在任意位置, 但是如果希望在 Java 中调用, 推荐在 class path 的 `resources/groovy/script` 下 新建 demo.groovy 脚本, 可以直接右键运行. 

demo.groovy 中可以调用其他 groovy class (无论是否在 class path 下) , 也可以调用其他 Java class ,当然, 需要 import 对应类的包

groovy class 位置: 推荐在 `/src/main/groovy` 下 (标记位 class path), 这样有些插件默认配置无需修改. 命名一般大写, 如 GroovyPerson.groovy: (`内部不要有其他脚本内容`, 否则会类名冲突, 因为 groovy 代码若作为脚本运行, 会以脚本文件名生成一个 class, 这样就和脚本内定义的类冲突)

```groovy
package io.github.xiaoyureed.starterdemo

/**
 * @author xiaoyu* date: 2020/3/15
 */
class GroovyPerson {
    String id
    String name

    String toString() {
        return "${this.getClass().toString()}{${id}, ${name}}"
    }
}
```



## 和 Java 的集成

http://groovy-lang.org/integrating.html

https://www.cnblogs.com/softidea/p/5122188.html

https://www.baeldung.com/groovy-java-applications, https://blog.csdn.net/qq_26886929/article/details/86501051

### GroovyClassLoader

GroovyClassLoader是一个定制的类装载器，负责解释加载Java类中用到的Groovy类 (将加载与脚本名称对应的类, 其余脚本内容无效)

```java
//GroovyClassLoader与Java中的加载器一样，同一个类名的类只能加载一次，如果想再次加载，必须调 GroovyClassLoader的clearCache()方法移除所有已经加载的Groovy Class

public static GroovyObject groovyObject(String classpathSource) throws URISyntaxException, IOException, IllegalAccessException, InstantiationException {
    GroovyClassLoader groovyClassLoader = new GroovyClassLoader(
            GroovyUtil.class.getClassLoader());
    Class groovyClass = groovyClassLoader.parseClass(Paths.get(Thread.currentThread()
            .getContextClassLoader().getResource(classpathSource).toURI()).toFile());
    return (GroovyObject) groovyClass.newInstance();
}

@Test
public void t1() throws IOException, InstantiationException, IllegalAccessException, URISyntaxException {
    GroovyObject groovyObject = GroovyUtil.groovyObject("groovy/clazz/Cat.groovy");
    groovyObject.setProperty("id", UUID.randomUUID().toString());
    groovyObject.setProperty("name", "xiao");
    System.out.println(groovyObject);
    groovyObject.invokeMethod("hi", null);
}
```

脚本内容:

```groovy
package groovy.clazz

/**
 * @author xiaoyu* date: 2020/3/16
 */
class Cat {
    def name
    def id
    String toString() {
        "${this.class.name}{ ${id}, ${name} }"
    }

    def hi() {
        println "I am a cat [${name}]"
    }
}

```

当然, 也可以直接执行脚本, 不过这不是 GroovyClassLoader 主要功能


### GroovyShell 

GroovyShell允许在Java类中求任意Groovy表达式的值。您可使用Binding对象输入参数给表达式，
并最终通过GroovyShell返回Groovy表达式的计算结果。

```java
GroovyShell groovyShell = new GroovyShell();

```

优化:

```java
private static GroovyShell groovyShell = new GroovyShell();
 
private static Map<String, Script> scriptCache = new ConcurrentHashMap<>();
 
private static <T> T invoke(String scriptText, String function, Object... objects) throws Exception {
    Script script;
    String cacheKey = DigestUtils.md5Hex(scriptText);
 
 if (scriptCache.containsKey(cacheKey)) {
    script = scriptCache.get(cacheKey);
 } else {
    script = groovyShell.parse(scriptText);
    scriptCache.put(cacheKey, script);
 }
 
    return (T) InvokerHelper.invokeMethod(script, function, objects);
}
````

### GroovyScriptEngine

GroovyScriptEngine从您指定的位置（文件系统，URL，数据库，等等, 即指定一个文件夹）加载Groovy脚本，并且随着脚本变化而重新加载它们

相互关联的多个脚本，使用GroovyScriptEngine会更好

```java
@Test
    public void t2() throws IOException, ResourceException, ScriptException {
        // 若是数组, 则有多个脚本地址
        // GroovyScriptEngine 可以作为一个全局唯一的 static 成员, 不必每次都创建
        GroovyScriptEngine groovyScriptEngine = new GroovyScriptEngine(PathUtil.absoluteClasspath("groovy/script/"));
        Binding binding = new Binding();
        binding.setProperty("input", "xiao");
        groovyScriptEngine.run("demo.groovy", binding);
    }

/**
    执行脚本的某个方法
*/
@Test
    public void t2_1() throws javax.script.ScriptException, FileNotFoundException, NoSuchMethodException {
        // GroovyScriptEngineFactory 可以作为 静态成员, 不必每次都创建
        GroovyScriptEngineFactory groovyScriptEngineFactory = new GroovyScriptEngineFactory();
        ScriptEngine              scriptEngine              = groovyScriptEngineFactory.getScriptEngine();
        scriptEngine.eval(new FileReader(Paths.get(PathUtil.absoluteClasspath("groovy/script/demo1.groovy")).toFile()));
        Invocable invocable = (Invocable) scriptEngine;
        invocable.invokeFunction("func", "xiao");
    }
```

脚本:

```groovy
package groovy.script
/**
 * @author xiaoyu* date: 2020/3/16
 */
println("demo =============")

def hi(str) {
    println("hello, ${str}")
}

// input 为传入参数
hi(input)
```

### 优化

https://www.cnblogs.com/softidea/p/5122188.html


- 使用GroovyShell的parse方法导致perm区爆满. 解决办法: 通常做法是缓存Script对象, key 为 groovyScript 脚本的md5值

    Groovy引擎每次执行脚本, 都会生成一个 class 和对应的 classloader,    无法被立即回收, 运行一段时间后将perm占满，一直触发fullgc

    为什么每次执行脚本都会生成新的 class? 因为对于同一个groovy脚本，groovy 引擎 为了保证每次执行的都是新的脚本内容，会每次生成一个新名字的Class文件 , 类名和时间戳有关

    为什么 groovy class loader 加载的类无法被 gc? 为了节省编译时间, GroovyClassLoader 代码中有一个 class 对象 的缓存 map, 脚本 class 对象被这个缓存引用, 无法回收

## maven 中 Java 和 groovy 混合开发

maven 插件 及依赖:

默认会把`src/main/java`和`src/main/groovy`作为源码目录

```xml
<project>
  <build>
    <plugins>
    <!-- 增加 gmavenplus 插件 允许集成Groovy到Maven-->
      <plugin> 
        <groupId>org.codehaus.gmavenplus</groupId>  
        <artifactId>gmavenplus-plugin</artifactId>
        <version>1.7.1</version>
        <executions>
          <execution>
            <goals>
              <goal>addSources</goal>
              <goal>addTestSources</goal>
              <goal>generateStubs</goal>
              <goal>compile</goal>
              <goal>generateTestStubs</goal>
              <goal>compileTests</goal>
              <goal>removeStubs</goal>
              <goal>removeTestStubs</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
      </plugin>
    </plugins>
  </build>
  <dependencies>
    <dependency>
      <groupId>org.codehaus.groovy</groupId>
      <!-- groovy-all 包含所有groovy GDK 中的包,
       groovy 只包含基础 Groovy 编译需要的包-->
      <artifactId>groovy-all</artifactId>
      <!-- any version of Groovy \>= 1.8.2 should work here -->
      <version>2.5.7</version>
      <!--指定类型为 pom -->
      <type>pom</type>
    </dependency>
  </dependencies>
</project>
```

如果 如果要使用其他目录作为Groovy源码目录,
比如src/additionalGroovy

```xml
<plugin>
        <groupId>org.codehaus.gmavenplus</groupId>
        <artifactId>gmavenplus-plugin</artifactId>
        <version>1.7.1</version>
        <executions>
          <execution>
            <goals>
              <goal>addSources</goal>
              <goal>addTestSources</goal>
              <goal>generateStubs</goal>
              <goal>compile</goal>
              <goal>generateTestStubs</goal>
              <goal>compileTests</goal>
              <goal>removeStubs</goal>
              <goal>removeTestStubs</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <sources>
        <!-- 在此节点下配置源码目录,可配置多个 -->
            <source>
              <directory>${project.basedir}/src/main/groovy</directory>
              <includes>
                <include>**/*.groovy</include>
              </includes>
            </source>
            <source>
              <directory>${project.basedir}/src/additionalGroovy</directory>
              <includes>
                 <include>**/*.groovy</include>
              </includes>
            </source>
          </sources>
        </configuration>
      </plugin>
    </plugins>
  </build>
```

如果源码目录还是不生效, 将 `src/main` 整个作为 maven 源码目录:

```xml
<project>
    <build>
        <!-- 将${project.basedir}/src/main整个作为源码目录,你再添加什么源码都在里面了 -->
        <sourceDirectory>${project.basedir}/src/main</sourceDirectory>
    </build>
</project>
```

## groovy 语法

http://www.groovy-lang.org/documentation.html

http://docs.groovy-lang.org/next/html/documentation/


所有类都是 public 的。所有属性都是 private 的 (每个属性自动提供一组 public getter 和 setter 方法)，而所有方法都是 public

在脚本中调用 Java class 可以使用 `new JavaPerson(firstName:"John", lastName:"Doe")`, 当然, 调用 groovy class 也一样. 底层实现是 Groovy 先调用默认的无实参构造函数，然后调用每个字段的相应 setter

调用 getter 时, 无需使用更冗长的 gp.getFirstName()，只需调用 gp.firstName. 类似 的, gp.setLastName("Jones") 和 gp.lastName = "Jones" 是等效的

### 和 Java 的不同

```groovy


// 默认导入 - default imports

/////////////////////////////////////

/**
 * "==" 的不同 --- 在 Java中, 表示对象完全相等(地址, 内容全相同), groovy 中表示 equals; 如果希望取得Java中的效果, 使用 a.is(b)
 */

 ///////////////////////////////////////////////////////

/**
 * default 必须位于 swith / case 结构的结尾 --- 在 Java 中，它可以放在 swith / case 结构中的任何位置，但在 Groovy 中，它更像是一个 else 子句，而非一个默认的 case 子句
 */

 ////////////////////////////////////////////////////

int ab=1//不需要分号

String multiLine='''
1//适应osc的markdwon
2//适应osc的markdwon
3'''//多行字符串

String interpolationString="$map or ${map}"//这是插值字符串,基本现代的语言都有吧

//////////////////////////////////////////////

/**
 *
 * 范围: 例如 “0..4” 表明包含 整数 0、1、2、3、4。Groovy 还支持排除范围，“0..<4” 表示 0、1、2、3。还可以创建字符范围：“a..e” 相当于 a、b、c、d、e。“a..<e” 包括小于 e 的所有值
 *
 * for(i in 0..5) {}
 *
 * def range=1..5//这是个range
 */

////////////////////////////////////////////////////////

/**
 * 默认参数值: def repeat(val, repeat=5) {}, 那么repeat 是可选的
 */

////////////////////////////////////////////////////////


/**
 * multi methods (方法重载) ------ dynamic binding
 */

int method(String str) {
    return 1
}

int method(Object obj) {
    return 2
}

Object o = "xxx"
def result = method(o)
println result // 1

///////////////////////////////////////////////

/**
 * 在 Groovy 中可以省略 return 语句。Groovy 默认返回方法的最后一行
 */


///////////////////////////////////////////////


/**
 * boxing/unboxing vs. widening ----- 在Java中 widening 优先于 boxing
 *
 * 但是在groovy 中, all primitive references use their wrapper class (基础类型自动使用包装对象)
 */

int i
m(i) // 下面哪个方法会调用?

void m(long l) { // java 中会调用, 在Java中 widening(延展) 优先于 boxing
    println "in m(long)"
}

void m(Integer i) {// groovy 中会调用
    println "in m(Integer)"
}


/////////////////////////////////////////////


/**
 * magic method 魔法方法
 */
def numbers1 = [1,2,3,4]
assert numbers1.join(",") == "1,2,3,4"
assert [1,2,3,4,3].count(3) == 2

/////////////////////////////////////////////

/**
 * - package scope ------ @PackageScope 包私有字段
 */
class Person implements Serializable {
    @PackageScope
    String name // 使用 @PackageScope 标注, 那么这个field是 package-private field

    int age // 并不是 package-private field, 而是一个 private field, 以及 getter , setter

    Person(name, age) {
        this.name = name
        this.age = age
    }
}

////////////////////////////////////////////////

/**
 * - array init -------- 使用 `[...]`, `{...}` 留给了closure
 */

int[] array = [1, 2, 3]

////////////////////////////////////////////////


 /**
 * 集合
 */
def map=[a:1,b:2]//这是个map
def map2 = [:] //默认是LinkedHashMap
assert map2.class == LinkedHashMap
map.'a' = 1 // key 包含包含空白字符之类的可以用这种方式
map.b = 2
map << [c: 3]
// null 空集合(包含map) 0 空字符串 空数组 在boolean环境中是false
def aMap=[:]
if (aMap){//groovy的false值,上面也提过
    println('这段代码不会被执行')//aMap意味着false
}
map << ['c':3]//这是重载运算符,实现起来其实很简单

def list1=[1,2,3,4,5]//这是个list
def list2 = [] //默认是ArrayList
assert list2.class == ArrayList//可以省略.class

LinkedList list3 = []//因为默认是ArrayList,这种方式可以改变
assert list3.class == LinkedList

def numbers = [1,2,3,4]
assert numbers + 5 == [1,2,3,4,5]
assert numbers - [2,3] == [1,4] // 创建了新的集合实例

////////////////////////////////////////////////////////

/**
 * 分布操作符 (spread operator) `*` -- 遍历集合
 */
assert ["JAVA", "GROOVY"] ==
        ["Java", "Groovy"]*.toUpperCase()


////////////////////////////////////////////////////////

/**
 * 防止null 空指针异常 -- 在方法调用前面添加一个 ? 就相当于在调用前面放了一个条件，可以防止在 null 对象上调用方法。
 *
 */
sng2.artist?.toUpperCase()


////////////////////////////////////////////////////////

/**
*   类初始化
*/
/**
 * 对象的初始化: 默认提供构造函数, 默认提供 getter, setter
 */
 def sng = new Song(name:"Le Freak", artist:"Chic", genre:"Disco")
/**
 * class Pro {
 *     Integer property1 //1 没用private public protected(gorrvy里面是            @PackageScope)
 *     def property2   //2 可选的statoc final(有final就会没有set) 之类的
 *     final String property3  //3 def 或者 确定的类型
 *     static String property4 //4 property名字
 *     //满足以上四个条件就是一个property
 *
 *     //实际上等于java以下内容,一个private+getter+setter(final修饰的没有setter)
 *     private String property5
 *
 *     String getProperty5() {*     return property5
 * }
 *
 * void setProperty5(String property5) {
 *     this.property5 = property5
 *     }
 * }
 * 
 * static void main(String[] args) {*
 *     def pro = new Pro()
 *     Pro pro1 = [:]//也可以这样声明,前提是必须指定类型,并且有默认的构造器,以下等价
 *     Pro pro2 = []//等价以上
 *     Pro pro3 = [property1: 123]//等价以上
 *     def pro4 = [property1: 123] as Pro//等价以上
 *     pro.property1 = 123 //类似pro.setProperty1(123)
 *     println(pro.property1)//类似pro.getProperty1()
 * }
 */

 /////////////////////////////////////////////


 /**
 * ARM blocks (automatic resources management) ------- 类似Java中的 try () {}*/

// read file, demo1
new File("./package-info.java").eachLine("UTF-8") {
    println it
}

// read file, demo2 ---- 更像Java的版本
new File("package-info.java").withReader('UTF-8') { reader ->
    reader.eachLine {
        println it
    }
}

//////////////////////////////////////////////////


/**
 * anonymous inner class 匿名内部类
 */

def called = new CountDownLatch(1)
def timer = new Timer()
timer.schedule(new TimerTask() {
    @Override
    void run() {
        called.countDown()
    }
}, 0)
//assert called.await(2, TimeUnit.SECONDS)

///////////////////////////////////////////////////


/**
  *  闭包
  *
  * lambda expression ---- java8 中的 lambda 实际上是 anonymous inner class, groovy 中的才是真正的lambda -- closure(闭包)
 */

def acoll = ["Groovy", "Java", "Ruby"]

acoll.each{
    println it // it 为关键字, 默认, 代表每个元素,可以修改
}
// 等价
acoll.each{ value -> // 自定义项变量
    println value
}

// 怎么执行
def excite = { word ->
    return "${word}!!"
}
assert "Groovy!!" == excite("Groovy")
assert "Java!!" == excite.call("Java")// 等价, 不常用


Runnable run = {
    println 'run'
}
def list = Arrays.asList(1, 2, 3, 4)
list.each { println it }
list.each(this.&println)// 等价

////////////////////////////////////////////////////

/**
 * GString - groovy string ---- "abc${1}de", 允许 `${...}`
 */

assert 'c'.getClass() == String
assert "c".getClass() == String // 等价
assert "c${1}".getClass() in GString

char a='a'                  // 单个字符的 string 会自动转型为 char
assert Character.digit(a, 16)==10 : 'But Groovy does boxing'
assert Character.digit((char) 'a', 16)==10
println 'assert ok'

// for single char strings, both are the same
assert ((char) "c").class==Character
assert ("c" as char).class==Character // 等价

// 对于包含多个字符的字符串来说，两种模式的结果并不相同
try {
    ((char) 'cx') == 'c'
    assert false: 'will fail - not castable'
} catch(GroovyCastException e) {
    println '多个字符不能转为char'
}
assert ('cx' as char) == 'c' // Groovy 模式的转换更宽容一些，只取第一个字符
assert 'cx'.asType(char) == 'c'// 等价
println 'assert ok'

//////////////////////////////////////////////////////

/**
 * 静态编译 @CompileStatic 标注在类上; 但是每一个类都加@CompileStatic实在太麻烦,不过groovy提供了一个特性.自定义CompilerConfiguration
 * 静态检查 @TypeChecked
 */

//////////////////////////////////////////////////////





```

### 输入输出流io

```groovy
/**
 * reading file
 */

def file = new File("./package-info.java")

void func1(File file) {
    file.eachLine {line -> // 若 eachLine 中出现异常, resource 会自动关闭
        println line
    }
}

void func2(File file) {
    file.eachLine {line, ln ->
        println("Line $ln: " + line)
    }
}

List func3(File file) {
    return file.collect {it}
}

String[] func4(File file) {
    return file as String[]
}

byte[] func5(File file) {
    return file.bytes
}

void func6(File file) {
    file.withInputStream {inputStream -> // stream 会自动关闭
        new InputStreamReader(inputStream).eachLine {line ->
            println(line)
        }
    }
}

//func1(file)
//func2(file)
//func3(file).forEach(System.out.&println)
//Stream.of(func4(file)).forEach(System.out.&println)
//func6(file)


/**
 * write sth to a file
 */

void fun1(File file) {
    file.withWriter('utf-8') {writer ->
        writer.writeLine('// hello groovy')
    }
}

// 原样输出 '''xxx'''
void fun2(File file) {
    file << '''
// write sth new with `''`
'''
}

//fun1(file)
//fun2(file)

/**
 * 对象数据的序列化反序列化
 */
// 序列化
void fun3(File file) {
    def hello = 'hello'
    def b = true

    file.withDataOutputStream {out ->
        out.writeBoolean(b)
        out.writeUTF(hello)
    }
}

// 反序列化
void fun4(File file) {
    file.withDataInputStream {input ->
        println(input.readBoolean()) // true
        println(input.readUTF()) // hello
    }
}

// 序列化/反序列化一个对象
void fun5(File file) {
    def person = new Person('name', 10)

    file.withObjectOutputStream {out ->
        out.writeObject(person)
    }

    file.withObjectInputStream {input ->
        def object = input.readObject()
        println(object.name)
        println(object.age)
    }
}



//fun3(file)
//fun4(file)
//fun5(new File('d:/xiaoyu.txt'))


/**
 * 遍历文件
 */

def dir = new File(".")

// 不会 recurse
void list1(File file) {
    file.eachFile {f ->
        println(f.name)
    }
}

// 正则查找
void list2(File file) {
    file.eachFileMatch(~/^package-info.java$/) { f ->
        println(f.name)
    }
}

// 会递归
void list3(File file) {
    file.eachFileRecurse {f ->
        println(f.name)
    }
}

// 只针对 文件 执行闭包代码, 对于目录, 不执行 closure
void list4(File file) {
    file.eachFileRecurse(FileType.FILES) {f ->
        println(f.name)
    }
}

void list5(File file) {
    file.traverse {f ->
        if (file.directory && file.name == 'bin') {
            FileVisitResult.TERMINATE // 若遍历到名为 bin 的目录, 退出遍历
        } else {
            println(f.name)
            FileVisitResult.CONTINUE // 继续遍历
        }
    }
}

//list1(dir)
//list2(dir)

/**
 * 执行命令行命令
 */

void comm1() {
    def process = 'ipconfig'.execute()
    println(process.text) // 中文乱码
}

// Process 对象有  in/out/err 流, in 为标准输入流, 对应着command的输出结果; out 代表标准输出, 我们可以通过out发送数据给command
// "cmd /c dir" - Windows平台shell有些builtin command, 需要通过 cmd 调用
void comm2() {
    def process = 'ipconfig'.execute()
    process.in.eachLine('gbk') {line ->
        println(line)// 使用 windows 默认 gbk 编码
    }
}

// pipe line 管道
//todo
void comm3() {
    proc1 = 'ipconfig'.execute()
    proc2 = 'grep'
}

//comm1()
comm2()


```

### 有用的工具类

```groovy
/**
 * ConfigSlurper 用来处理 groovy script 格式的 config file
 */

void func1() {
    def config = new ConfigSlurper().parse('''
        app.date = new Date()
        app.age = 10
        app {
            name = "Test${10}"
        }
        app."a.b" = "a.b hello"
    ''')

    assert config.app.date instanceof Date
    assert config.app.age == 10
    assert config.app.name == 'Test10'
    assert config.app.xxx != null // // 要么返回配置值，要么返回一个新的 ConfigObject 实例，但永远不会返回 null 值
    assert config.app."a.b" == "a.b hello" // 单引号亦可
}

// 设置多环境, 简单起见, `environments` 不变
void func2() {

    def config = new ConfigSlurper('dev').parse('''
        environments {
            dev {
                app.port = 8080
            }
            prod {
                app.post = 80
            }
            test {
                app.port = 8081
            }
        }
    ''')
    assert config.app.port == 8080
}

//`environments` 可以修改, 但是对应的也需要注册操作
void func3() {
    def slurper = new ConfigSlurper()
    slurper.registerConditionalBlock('myProject', 'developers')

    def config = slurper.parse('''
        sendMail = true
        
        myProject {
           developers {
               sendMail = false
           }
        }
    ''')

    assert config.sendMail == false
}

// 和 Java 的 properties 文件整合
void func4() {
    def config = new ConfigSlurper().parse('''
        app.date = new Date()
        app.age  = 42
        app {
            name = "Test${42}"
        }
        haha = "haha ${app.date}"
    ''')

    def properties = config.toProperties()

    println(properties."app.date") // Sat May 25 17:49:02 CST 2019
    println(properties."haha") // haha Sat May 25 17:50:44 CST 2019

    assert properties."app.date" instanceof String
    assert properties."app.age" == '42'
    assert properties."app.name" == 'Test42'
}

//func1()
//func2()
//func3()
//func4()


/**
 * Expando - 动态可拓展对象
 */

void exp1() {
    def expando = new Expando()
    expando.name = 'name'
    expando.someMethod = {s -> "some method. parameter = ${s}"}

    println(expando.name)
    println(expando.someMethod('aa'))

    assert expando.name == 'name'
    assert expando.someMethod('aa') == 'some method. parameter = aa'
}

//exp1()

/**
 * Observable list, map and set 可观测集合, 列表
 */

```

### 元编程

```groovy
package io.github.xiaoyu.groovydemo

import groovy.time.TimeCategory

/**
 * MOP
 * @author xiaoyu* @date 2019/5/25
 */
//class MetaProgram {
//}

/**
 * 运行时元编程
 * POJO - 普通 Java对象
 * POGO - groovy 对象, 继承自 java.lang.Object 且默认实现了 groovy.lang.GroovyObject 接口。
 * Groovy interceptor - Groovy 拦截器 —— 实现了 groovy.lang.GroovyInterceptable 接口的 Groovy 对象，并具有方法拦截功能
 */

/**
 * GroovyObject 接口:
 *
 * public interface GroovyObject {
 *
 *      // 除了具有 methodMissing的功能之外, 和 GroovyInterceptable 配合, 可以拦截所有方法, 开销大
 *      // 如果希望更通用的方式: 在一个对象的 MetaClass 上实现 invokeMethod()。该方法同时适于 POGO 与 POJO 对象
 *     Object invokeMethod(String methodName, Object methodArgs);
 *
 *      // 每次读取 pogo 对象的field(无论是否存在)时, 都先调用这个方法
 *     Object getProperty(String propertyName);
 *
 *      // 每次设置properties时调用
 *     void setProperty(String propertyName, Object newValue);
 *
 *     MetaClass getMetaClass();
 *
 *     void setMetaClass(MetaClass metaClass);
 * }
 */

/**
 * methodMissing - 拦截缺失的方法调用 (https://www.cnblogs.com/maijunjin/articles/3043935.html)
 *                  使用 methodMissing，并不会产生像调用 invokeMethod 那么大的开销，第二次调用代价也并不昂贵
 *
 * propertyMissing  - 拦截缺失的属性调用
 */

class InvokeMethodDemo1 {
    // override invokeMethod()
    def invokeMethod(String method, Object args) {
        "method ${method}(${args.join(', ')}) missing"
    }
    def test() {
        'method test exist'
    }
}

void func1() {
    def demo = new InvokeMethodDemo1()
    assert demo.test() == 'method test exist'
    assert demo.xxx() == 'method xxx() missing'
    assert demo.yyy('abc') == 'method yyy(abc) missing'
}

//func1()

class InvokeMethodDemo2 implements GroovyInterceptable {
    def invokeMethod(String method, Object args) {
        'hoho'
    }
    def test1() {
        println 'test1'
    }
}

void func2() {
    def demo = new InvokeMethodDemo2()
    assert demo.test1() == 'hoho'
    assert demo.xxx() == 'hoho' // 调用不存在的方法, 也被拦截

    // 如果想要拦截所有的方法调用，但又不想实现 GroovyInterceptable 这个接口，那
    // 么可以在一个对象的 MetaClass 上实现 invokeMethod()
    // 同时适于 POGO 与 POJO 对象
    def str = 'aa'
    str.metaClass.invokeMethod = {String method, Object args ->
        "invoke method ${method}"
    }
    assert str.length() == 'invoke method length'
    assert str.xxx() == 'invoke method xxx'
}
//func2()


class GetPropertyDemo {
    def f1 = 'aa'
    def f2 = 'bb'

    def getProperty(String field) {
        if (field == 'f3') { // 拦截对 f3 的读取
            return 'f3'
        }
        // 其他的 field 放行
        return metaClass.getProperty(this, field)
    }

    def getF4() {
        'f4'
    }

    void setProperty(String name, Object value) {
        this.@"$name" = value + '-tail'
    }
}

void testGetPropertyDemo() {
    def demo = new GetPropertyDemo()

    assert demo.f1 == 'aa'
    assert demo.f2 == 'bb'

    assert demo.f3 == 'f3'
    assert demo.f4 == 'f4'
}
//testGetPropertyDemo()

class GetSetAttrDemo{
    def f1 = 'f1'
    def f2 = 'f2'

    def f3
    def f4

}
void testGetSetAttrDemo() {
    def demo = new GetSetAttrDemo()

    assert demo.metaClass.getAttribute(demo, 'f1') == 'f1'
    assert demo.metaClass.getAttribute(demo, 'f2') == 'f2'

    demo.metaClass.setAttribute(demo, 'f3', 'f3')
    demo.metaClass.setAttribute(demo, 'f4', 'f4')
    assert demo.metaClass.getAttribute(demo, 'f3') == 'f3'
    assert demo.metaClass.getAttribute(demo, 'f4') == 'f4'
}
//testGetSetAttrDemo()

class MethodMissingDemo {
    def methodMissing(String name, def args) {
        return "this is me"
    }

    // 针对静态方法调用的缺失
    static def $static_methodMissing(String name, Object args) {
        return "Missing static method name is $name"
    }
}
void testMethodMissingDemo() {
    def demo = new MethodMissingDemo()
    assert demo.someMethod(20) == 'this is me'
    assert MethodMissingDemo.xxx() == 'Missing static method name is xxx'
}
//testMethodMissingDemo()

class PropertyMissingDemo1 {
    def propertyMissing(String f) { f }

    // 针对静态field调用的缺失
    static def $static_propertyMissing(String name) {
        return "Missing static property name is $name"
    }
}
void testPropertyMissingDemo1() {
    assert new PropertyMissingDemo1().aaa == 'aaa'
    assert PropertyMissingDemo1.xx == 'Missing static property name is xx'

}
//testPropertyMissingDemo1()

// 实现动态添加 field
class PropertyMissingDemo2 {
    def storage = [:] // map

    // 针对 setter
    def propertyMissing(String name, value) { storage[name] = value }

    // 针对 getter
    def propertyMissing(String name) { storage[name] }
}

void testPropertyMissingDemo2() {
    def demo2 = new PropertyMissingDemo2()
    demo2.xxx = 'aaa'

    assert demo2.xxx == 'aaa'
}
//testPropertyMissingDemo2()


/**
 * Groovy 从 Objective-C 那里借用并实现了一个概念，叫做：类别（Categories）
 *
 * groovy 默认提供几个 categories class: TimeCategory, ServletCategory, DOMCategory
 *
 *          categories class 中的方法均为static的
 *
 * 类别类默认是不能启用的。要想使用定义在类别类中的方法，必须要使用 GDK 所提供的 use(CategoriesClass, Closure)
 */
void testCategories() {
    use(TimeCategory, {// closure 可以写到 小括号之外
        println 1.minute.from.now // 一分钟之后 // TimeCategory 为 Integer 添加了方法
        println(10.hour.ago)// 十小时之前

        def date = new Date()
        println(date - 3.months)// 三个月之前 // TimeCategory 为 Date 添加了方法
    })
}

//testCategories()

/**
 * 编译时元编程
 */


```

#### 动态增加方法

```groovy
//用闭包定义一个方法 var1为参数 ,->后面是执行语句（当然参数不是必须的）
def methodA={var1-> print "this is methodA"} 

//用闭包定义一个方法 var1为参数 ,->后面是执行语句（当然参数不是必须的）
def methodB={var1-> print "this is  methodB"}

String.metaClass.addMethodA=methodA;   //将methodA绑定为成员方法。
String.metaClass.'static'.addMethodB=methodB;   //将methodB绑定为静态方法

String s="str"; 
s.addMethodA('good');  //实例调用方法A 
String.addMethodB('hello'); //静态类调用方法B
```


# 底层运行原理


groovy.lang.GroovyObject (接口): groovy class 会默认实现这个接口

groovy.lang.Script (抽象类): groovyc 编译器将获取源文件的名称并创建了一个同名的类继承 Script, 整个脚本中 非 groovy class 代码会编译到这里

## Groovy代码文件与class文件的对应关系

Groovy 允许完全省略编译步骤，不过仍然可以 进行编译。如果想要编译代码，可以使用 Groovy 编译器 groovyc。用 groovyc 编译 Groovy 代码会产生标准的 Java 字节码

- 如果Groovy脚本文件里只有执行代码，没有定义任何类（class)

    则编译器会生成一个Script的子类，类名和脚本文件的文件名一样，而脚本的代码会被包含在一个名为run的方法中，同时还会生成一个main方法，作为整个脚本的入口。

- 如果Groovy脚本文件里仅含有一个类，而这个类的名字又和脚本文件的名字一致

    这种情况下就和Java是一样的，即生成与所定义的类一致的class文件。

- 如果Groovy脚本文件含有多个类，

    groovy编译器会为每个类生成一个对应的class文件。如果想直接执行这个脚本，则脚本里的第一个类必须有一个static的main方法。
