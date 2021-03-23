---
title: How to Test Java App
tags:
  - mock
  - test
date: 2019-08-14 15:23:16
categories: java web
---


相关工具和类库

- Junit、TestNG, Spring Test
- 断言库：AssertJ (推荐), Hamcrest（不更新了，Matcher分散在多个类中，编写困难，JUnit仅依赖了Hamcrest核心包，只附带了最基本的断言功能，如果我们希望断言数字大小之类的话，还需要自己引入Hamcrest完整包，比较麻烦）
- MOCK 框架，例如 Jmock、Easymock、PowerMock (mock静态方法, 私有方法...) ， Mockito(推荐, springboot-test 默认提供);
- rest api 自动化测试：REST Assured， postman
- Selenium: ui测试
- JSONassert：JSON 断言库
- JsonPath：JSON XPath

<!--more-->

<!-- TOC -->

- [spring-boot-starter-test](#spring-boot-starter-test)
- [AssertJ](#assertj)
- [Mockito](#mockito)
  - [和 springboot 配合使用](#和-springboot-配合使用)
- [powerMock](#powermock)
- [web层测试](#web层测试)
- [压测](#压测)
  - [性能指标](#性能指标)
  - [siege](#siege)
  - [Gatling](#gatling)
  - [jmeter](#jmeter)
  - [ab](#ab)
- [Junit](#junit)
- [ab test](#ab-test)
- [集成测试 Testcontainers](#集成测试-testcontainers)
- [优化测试编写体验](#优化测试编写体验)
- [talend api tester 浏览器插件](#talend-api-tester-浏览器插件)

<!-- /TOC -->


## spring-boot-starter-test

用于 spring boot 单元测试

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.junit.vintage</groupId> // 最新版可以不排除了
            <artifactId>junit-vintage-engine</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

需要排除掉 `junit-vintage-engine` (若不排除, 则测试类需要用 `@RunWith(SpringRunner.class)` 标注才能正常注入 bean)

最新版现在使用只需要添加类注解 `@SpringBootTest` 即可


## AssertJ

流式断言库, spring-boot-starter-test 提供了依赖

Hamcrest 提供了更多断言, springboot 维护了其版本

https://assertj.github.io/doc/#assertj-core
http://joel-costigliola.github.io/assertj/

```java
import static org.assertj.core.api.Assertions.assertThat;

// 常用断言:

// 集合
// 至少有一个符合
Assertions.assertThat(tree).anyMatch(cate -> !CollectionUtils.isEmpty(cate.getChildren()));
// 全都要符合 allMatch
// 全都不符合 noMatch

assertThat(Arrays.asList("world", "hello"))
				.as("列表断言描述")
				.isNotEmpty() 
				.isNotNull()
				.isInstanceOf(List.class)
				.isSubsetOf("hello", "world")
				.contains("hello")
				.containsOnlyOnce("world")
				.startsWith("world")
				.endsWith("hello");


// 单个对象
// matches 符合多个条件

assertThat(user1)
				.as("对象断言描述")
				.isEqualToComparingFieldByField(user2) //user1的每个字段是否与user2相同
				.isExactlyInstanceOf(User.class) //user1是否是User类的对象
				.isSameAs(user3) //是否是同一个对象
				.isNotNull() //是否非空
				.hasFieldOrProperty("name") //是否含有name字段
				.hasFieldOrPropertyWithValue("age", 12); //是否含有age字段，且值为12



// 字符串
assertThat("test").isNotBlank() // 是否为" "字符串
				.as("字符串断言描述").isSubstringOf("test1") // 是否为test1的一部分
				.isSameAs("test") // 对象内元素是否相等
				.isNotEmpty() // 是否为空字符串
				.isEqualTo("test") // 是否相等
				.isEqualToIgnoringCase("Test") // 是否相等（忽略大小写）
				.isExactlyInstanceOf(String.class) // 是否是实例
				.isIn(Arrays.asList("test", "hello")) // 是否在列表中
				.isIn("test", "hello") // 是否在参数列表中
				.isInstanceOfAny(String.class, Integer.class) // 是否是实例中任何一个
				.isNotNull() // 是否不为空
				.contains("es") // 是否包含es子串
				.startsWith("te") // te开始
				.endsWith("st") // st结束
				.matches(".e.t"); // 是否匹配 .e.t 格式
		assertThat("").isNullOrEmpty();


// 数字断言
		assertThat(new Integer(100))
				.as("数字断言描述").isEqualTo(100) // 是否相等
				.isBetween(0, 300) // 是否在0，300之间
				.isNotNull() // 是否非空
				.isNotZero() // 是否不等于0
				.isGreaterThanOrEqualTo(80) // 是否大约等于80
				.isLessThan(200) // 是否小于200
				.isPositive() // 是否是正数
				.isNotNegative() // 是否是非负数
				.isIn(Arrays.asList(100, 200)) // 是否在列表中
				.isInstanceOf(Integer.class); // 是否是Integer类型

// 日期断言
		assertThat(new Date())
				.as("日期断言描述")
				.isAfter("2018-08-01")
				.isAfterYear(2017)
				.isBetween("2018-01-01", "2018-08-31")
				.isEqualToIgnoringHours(new Date().toLocaleString())
				.isExactlyInstanceOf(Date.class)
				.isInSameHourAs(new Date())
				.isInThePast()
				.isToday();



// 字典断言
Map foo = Maps.newHashMap();
foo.put("A", 1);
foo.put("B", 2);
foo.put("C", 3);
assertThat(foo)
        .as("字典断言描述")
        .isNotNull() // 是否不为空
        .isNotEmpty() // 是否size为0
        .hasSize(3) // size是否为3
        .contains(entry("A", 1)) // 是否包含entry
        .containsKeys("A") // 是否包含key
        .containsValue(1); // 是否包含value

// 异常断言
//https://www.baeldung.com/assertj-exception-assertion#:~:text=AssertJ%20Exception%20Assertions%201%20Overview.%20In%20this%20quick,expressions.%204%20Conclusion.%20And%20there%20we%20are.%20
assertThatThrownBy(() -> {
    List<String> list = Arrays.asList("String one", "String two");
    list.get(2);
}).isInstanceOf(IndexOutOfBoundsException.class)
  .hasMessageContaining("Index: 2, Size: 2");
			//.hasMessage("Index: %s, Size: %s", 2, 2)
		// .hasMessageStartingWith("Index: 2")
		// .hasMessageContaining("2")
		// .hasMessageEndingWith("Size: 2")
		// .hasMessageMatching("Index: \\d+, Size: \\d+")
		// .hasCauseInstanceOf(IOException.class)
		// .hasStackTraceContaining("java.io.IOException");
//or
assertThatExceptionOfType(IndexOutOfBoundsException.class)
  .isThrownBy(() -> {
      // ...
}).hasMessageMatching("Index: \\d+, Size: \\d+");

```


## Mockito

测试用 mocking framework, 比如 web app 测试, spring boot-starter-test提供了她的依赖

demo: https://github.com/xiaoyureed/springboot-demos/tree/master/mockito-mybatis-plus-demo

http://wiki.jikexueyuan.com/project/spring-boot-cookbook-zh/test-mockito.html

```java
import static org.mockito.ArgumentMatchers.any;
import static org.mockito.Mockito.when;
```

### 和 springboot 配合使用

scenario1: 需要 mock 的对象成员如 RedisService 使用 @mock, 不需要 mock 的成员使用 @Spy @Autowired;  填充到目标测试对象 使用 @InjectMocks 标注目标对象, 每次测试前需要初始化 `@BeforeEach void xxx() {MockitoAnnotations.initMocks(this);}`

scenario2: @MockBean 标注需要 mock 的成员, @Autowired 标注目标对象, 无需初始化, 但是这种方式造成 appContext 重启, 性能低


## powerMock 

用于解决 mockito 无法覆盖的 case, 

## web层测试

- MockMVC 单元测试
- [TestRestTemplate](https://www.baeldung.com/spring-boot-testresttemplate), RestAssured 集成测试

[区别]](https://stackoverflow.com/questions/52051570/whats-the-difference-between-mockmvc-restassured-and-testresttemplate)

[web层unit test demo](https://github.com/xiaoyureed/springboot-demos/blob/master/mockito-mybatis-plus-demo/src/test/java/io/github/xiaoyureed/mockitomybatisplusdemo/controller/UserControllerTest.java)

https://github.com/rest-assured/rest-assured - rest api 测试库

```java
@SpringBootTest(classes = GeneratorApp.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)

/**
     * @LocalServerPort 提供了 @Value("${local.server.port}") 的代替
     */
    @LocalServerPort
    private int port;




@AutoConfigureMockMvc
@SpringBootTest
public class ExceptionTest {
    @Autowired
    MockMvc mockMvc;

    @Test
    void should_return_400_if_param_not_valid() throws Exception {
        mockMvc.perform(get("/api/illegalArgumentException"))
                .andExpect(status().is(400))
                .andExpect(jsonPath("$.message").value("参数错误!"));
    }

    @Test
    void should_return_404_if_resourse_not_found() throws Exception {
        mockMvc.perform(get("/api/resourceNotFoundException"))
                .andExpect(status().is(404))
                .andExpect(jsonPath("$.message").value("Sorry, the resourse not found!"));
    }
}
```


## 压测

https://www.ibm.com/developerworks/cn/java/j-lo-performance-analysissy-tools/index.html
https://www.ibm.com/developerworks/cn/java/j-lo-performance-analysissy-tools2/index.html
https://www.ibm.com/developerworks/cn/java/j-lo-performance-analysissy-tools3/


测试 内存泄漏 , 并发, 同步, ...问题

### 性能指标

RS: response time 

QPS：Queries Per Second意思是“每秒查询数”，是一台服务器每秒能够的查询次数, 次/秒 , qps 衡量接口性能

TPS：是TransactionsPerSecond的缩写, 每秒交易数, 笔/秒, 一个交易/事务可能包含多个请求, tps 用来衡量整个业务流程性能; 金融系统 1000tps-5000tps, 保险系统 100-100000tps, 制造行业 100-5000tps, 电商 10000-1000000tps, 中小型网站类似金融系统

吞吐量: 表示应用系统每秒钟最大能接受的用户访问量 , 反应系统的承压能力; 单个reqeust 对CPU消耗越高，外部系统接口、IO影响速度越慢，系统吞吐能力越低，反之越高, 类似 tps, qps

### siege

http 压测工具

https://www.joedog.org/siege-manual/

TODO


### Gatling

类似 jmeter, 使用 Scala


### jmeter

一般查看 吞吐量, 90% 请求响应时间, 99%响应时间


### ab

apache benchmark并发测试工具



## Junit

这里主要介绍 Junit5 (https://junit.org/junit5/docs/current/user-guide/#overview-getting-started)

```xml
    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>${maven.compiler.source}</maven.compiler.target>
        <junit.jupiter.version>5.6.2</junit.jupiter.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>${junit.jupiter.version}</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage


## ab test

https://mp.weixin.qq.com/s/tmbGaWHp8k2MjByIa6z7MA

TODO



## 集成测试 Testcontainers

https://blog.csdn.net/mail_liuxing/article/details/99075606
https://github.com/testcontainers/testcontainers-java

## 优化测试编写体验

groovy + spock


## talend api tester 浏览器插件

类似 postman, 更轻量

https://www.bilibili.com/video/av90263035/ 
