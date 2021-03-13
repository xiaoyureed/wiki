---
title: Mybatis Note
tags:
  - orm
  - mybatis
date: 2014-02-01 16:21:24
categories: java web
---

<div align="center">
mybatis就是一个封装了jdbc的持久层框架，同类是 hibernate，但是hibernate是一个标准的orm框架，而mybatis是一个不完全的orm框架， 需要手写sql;
Mybatis让我们只关注sql本身，而不需要去关注创建连接、创建statement、关闭连接等操作; Mybatis会对输入参数、输出结果进行映射
MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 POJOs映射成数据库中的记录

</div>
<!--more-->

<!-- TOC -->

- [mybatis一些概念理解](#mybatis一些概念理解)
  - [Mybatis和hibernate区别, 应用场景](#mybatis和hibernate区别-应用场景)
  - [原生的jdbc](#原生的jdbc)
- [原理-quickstart](#原理-quickstart)
  - [核心组件](#核心组件)
  - [quickstart](#quickstart)
- [使用mapper代理的方式开发dao](#使用mapper代理的方式开发dao)
- [mybatis的配置文件](#mybatis的配置文件)
  - [配置文件的各部分有顺序](#配置文件的各部分有顺序)
  - [properties](#properties)
  - [settings](#settings)
  - [typeAliases](#typealiases)
  - [typeHandlers](#typehandlers)
    - [JdbcType](#jdbctype)
    - [默认类型处理器](#默认类型处理器)
    - [自定义类型处理器](#自定义类型处理器)
      - [怎么开发自定义类型处理器](#怎么开发自定义类型处理器)
      - [怎么启用呢](#怎么启用呢)
    - [自动检索类型处理器](#自动检索类型处理器)
    - [泛型类型处理器](#泛型类型处理器)
    - [处理枚举类型](#处理枚举类型)
  - [对象工厂（objectFactory）-自定义查询结果的实例化](#对象工厂objectfactory-自定义查询结果的实例化)
  - [插件（plugins）拦截器](#插件plugins拦截器)
  - [配置环境（environments）-数据源-事务管理](#配置环境environments-数据源-事务管理)
    - [怎么配置environment](#怎么配置environment)
    - [自定义mybatis事务管理](#自定义mybatis事务管理)
    - [自定义数据源-第三方数据源](#自定义数据源-第三方数据源)
  - [对不同数据库的支持（databaseIdProvider）](#对不同数据库的支持databaseidprovider)
  - [指定映射器（mappers）位置](#指定映射器mappers位置)
- [mapper映射配置xml文件](#mapper映射配置xml文件)
  - [mapper.xml支持哪些元素](#mapperxml支持哪些元素)
  - [查询select元素](#查询select元素)
  - [insert, update 和 delete](#insert-update-和-delete)
  - [sql元素-重用sql](#sql元素-重用sql)
  - [动态sql](#动态sql)
    - [if](#if)
    - [choose,when,otherwise-多个when中择其一](#choosewhenotherwise-多个when中择其一)
    - [where,set,trim-除去sql中的关键字](#wheresettrim-除去sql中的关键字)
    - [多数据库支持](#多数据库支持)
    - [动态 SQL 中的可插拔脚本语言(如Apache Velocity )](#动态-sql-中的可插拔脚本语言如apache-velocity-)
  - [参数（Parameters）](#参数parameters)
    - [常规用法](#常规用法)
    - [在占位符里设置参数类型和特殊的类型处理器类](#在占位符里设置参数类型和特殊的类型处理器类)
    - [两种字符串替换方式-#$区别](#两种字符串替换方式-区别)
  - [Result Map,属性字段映射](#result-map属性字段映射)
    - [简单结果映射](#简单结果映射)
    - [resultMap高级结果映射](#resultmap高级结果映射)
      - [resultMap总共有哪些元素](#resultmap总共有哪些元素)
      - [constructor标签, 构造方法](#constructor标签-构造方法)
      - [association关联](#association关联)
        - [关联的嵌套查询](#关联的嵌套查询)
        - [关联的嵌套结果](#关联的嵌套结果)
      - [集合collection关联](#集合collection关联)
        - [集合的嵌套查询](#集合的嵌套查询)
        - [集合的嵌套结果](#集合的嵌套结果)
      - [discriminator 鉴别器](#discriminator-鉴别器)
    - [自动映射](#自动映射)
- [缓存](#缓存)
  - [一级缓存](#一级缓存)
    - [怎么开启一级缓存](#怎么开启一级缓存)
    - [一级缓存分析](#一级缓存分析)
  - [二级缓存](#二级缓存)
    - [怎么开启二级缓存](#怎么开启二级缓存)
    - [二级缓存分析](#二级缓存分析)
  - [使用自定义缓存](#使用自定义缓存)
    - [自定义缓存](#自定义缓存)
    - [第三方缓存-整合ehcache](#第三方缓存-整合ehcache)
  - [应用场景](#应用场景)
  - [局限性](#局限性)
- [延迟加载](#延迟加载)
- [整合到spring](#整合到spring)
- [逆向工程](#逆向工程)
- [拾遗](#拾遗)
  - [#和$区别](#和区别)
  - [parameterType和resultType](#parametertype和resulttype)
  - [resultMap和resultType](#resultmap和resulttype)

<!-- /TOC -->

# mybatis一些概念理解

## Mybatis和hibernate区别, 应用场景

Mybatis：
1、 通过直接编写SQL语句，可以直接对SQL进行性能的优化；
2、 由于直接编写SQL语句，所以灵活多变，代码维护性更好。
3、 不能支持数据库无关性，移植性不好。
4、 需要编写结果映射。

Hibernate：
1、 标准的orm框架，不需要编写SQL。
2、 具有良好的数据库无关性。
3、 不能自主的去进行SQL性能优化。因为sql由框架生成

## 原生的jdbc

```java
public static void main(String[] args) {
	Connection connection = null;
	PreparedStatement preparedStatement = null;
	ResultSet resultSet = null;
	
	try {
		//1、加载数据库驱动
		Class.forName("com.mysql.jdbc.Driver");
		//2、通过驱动管理类获取数据库链接
		connection =  DriverManager.getConnection("jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8", "root", "root");
		//3、定义sql语句 ?表示占位符
		String sql = "select * from user where username = ?";
		//4、获取预处理statement
		preparedStatement = connection.prepareStatement(sql);
		//5、设置参数，第一个参数为sql语句中参数?的序号（从1开始），第二个参数为设置的参数值
		preparedStatement.setString(1, "王五");
		//6、向数据库发出sql执行查询，查询出结果集
		resultSet =  preparedStatement.executeQuery();
		//7、遍历查询结果集
		while(resultSet.next()){
			System.out.println(resultSet.getString("id")+"  "+resultSet.getString("username"));
		}
	} catch (Exception e) {
		e.printStackTrace();
	}finally{
		//8、释放资源
		if(resultSet!=null){
			try {
				resultSet.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
		if(preparedStatement!=null){
			try {
				preparedStatement.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
		if(connection!=null){
			try {
				connection.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
	}
}
```

问题分析:

1、 创建连接时存在硬编码
		可通过【配置文件】解决

2、 执行statement时存在硬编码
		可通过【配置文件】解决

3、 频繁关闭数据库连接会影响数据库的性能
		可通过【数据库连接池】解决



# 原理-quickstart

## 核心组件

- SqlSessionFactory  

  - 全局唯一， 和 app 共存亡

  - SqlSessionFactory 本身是由 SqlSessionFactoryBuilder 创建的，它可以从 XML、注解或手动配置 Java 代码来创建 SqlSessionFactory

- SqlSession 

  - 每次请求会新开一个线程，每个线程都应该有它自己的 SqlSession 实例。SqlSession 的实例不是线程安全的，因此是不能被共享的， 返回一个响应后， 就关闭 sqlsession; 

  - 通过这个接口来执行命令，获取映射器和管理事务

  - SqlSessions 是由 SqlSessionFactory 实例创建, 和spring整合时, SqlSessions 将被依赖注入框架所创建

- Mapper interface - 映射器接口的实例是从 SqlSession 中获得的

```java

// SqlSessionFactoryBuilder api

      // 从 xml 创建

SqlSessionFactory build(InputStream inputStream)
SqlSessionFactory build(InputStream inputStream, String environment)
SqlSessionFactory build(InputStream inputStream, Properties properties)
SqlSessionFactory build(InputStream inputStream, String env, Properties props)

      // 纯 代码 创建
SqlSessionFactory build(Configuration config)


      // 示例
DataSource dataSource = BaseDataTest.createBlogDataSource();
TransactionFactory transactionFactory = new JdbcTransactionFactory();

Environment environment = new Environment("development", transactionFactory, dataSource);

Configuration configuration = new Configuration(environment);
configuration.setLazyLoadingEnabled(true);
configuration.setEnhancementEnabled(true);
configuration.getTypeAliasRegistry().registerAlias(Blog.class);
configuration.getTypeAliasRegistry().registerAlias(Post.class);
configuration.getTypeAliasRegistry().registerAlias(Author.class);
configuration.addMapper(BoundBlogMapper.class);
configuration.addMapper(BoundAuthorMapper.class);

SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
SqlSessionFactory factory = builder.build(configuration);

////////////////////////////////////////////

// SqlSessionFactory api

SqlSession openSession() // 开启一个事务（也就是不自动提交）。
SqlSession openSession(boolean autoCommit)
SqlSession openSession(Connection connection)
SqlSession openSession(TransactionIsolationLevel level)
SqlSession openSession(ExecutorType execType,TransactionIsolationLevel level)
SqlSession openSession(ExecutorType execType)
SqlSession openSession(ExecutorType execType, boolean autoCommit)
SqlSession openSession(ExecutorType execType, Connection connection)
Configuration getConfiguration();

void clearCache() // 清空本地缓存
void close()//确保 SqlSession 被关闭

/////////////////////////////////////////////////////

// SqlSession api

<T> T selectOne(String statement, Object parameter)//必须返回一个对象或 null 值
<E> List<E> selectList(String statement, Object parameter) // 如果不知道返回对象的数量，请使用 selectList
<K,V> Map<K,V> selectMap(String statement, Object parameter, String mapKey)//将返回的对象的其中一个属性作为 key 值，将对象作为 value 值
int insert(String statement, Object parameter)
int update(String statement, Object parameter)
int delete(String statement, Object parameter)

//限制返回行数的范围，或者提供自定义结果控制逻辑，这通常在数据集合庞大的情形下使用
<E> List<E> selectList (String statement, Object parameter, RowBounds rowBounds)
<K,V> Map<K,V> selectMap(String statement, Object parameter, String mapKey, RowBounds rowbounds)
void select (String statement, Object parameter, ResultHandler<T> handler)
void select (String statement, Object parameter, RowBounds rowBounds, ResultHandler<T> handler)

```

## quickstart

依赖: mybatis, mybatis-connector-java


```java
String resource = "org/mybatis/example/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
//或者可以这样, MyBatis 包含一个名叫 Resources 的工具类，它包含一些实用方法，可使从 classpath 或其他位置加载资源文件更加容易。
InputStream is = Tests.class.getClassLoader().getResourceAsStream(resource);
            // Thread.currentThread().getContextClassLoader().getResourceAsStream
// SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在, SqlSessionFactory 的最佳作用域是应用作用域。有很多方法可以做到，最简单的就是使用单例模式或者静态单例模式。
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
// 每个线程都应该有它自己的 SqlSession 实例。SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。
SqlSession session = sqlSessionFactory.openSession([TransactionIsolationLevel.READ_COMMITTED]);// 可选指定事务的隔离级别

// 通过xml定义的sql
try {
  Blog blog = (Blog) session.selectOne("org.mybatis.example.BlogMapper.selectBlog", 101);
} finally {
  session.close();// 重要, 每次返回一个响应, 都要关闭session
}

// 通过mapper接口定义, 注解
try {
  BlogMapper mapper = session.getMapper(BlogMapper.class);
  Blog blog = mapper.selectBlog(101);
} finally {
  session.close();
}
```

mybatis-config.xml:

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development"><!-- environment 元素体中包含了事务管理和连接池的配置 -->
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers><!-- mappers 元素则是包含一组 mapper 映射器 -->
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

基于xml的sql(mapper文件):

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <!-- 在命名空间“org.mybatis.example.BlogMapper”中定义了一个名为“selectBlog”的映射语句，这样它就允许你使用指定的完全限定名“org.mybatis.example.BlogMapper.selectBlog”来调用映射语句 -->
<mapper namespace="org.mybatis.example.BlogMapper">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```

基于注解定义sql:

```java
package org.mybatis.example;
public interface BlogMapper {
  @Select("SELECT * FROM blog WHERE id = #{id}")/* 如果太复杂, 可以写到sql配置文件中 */
  Blog selectBlog(int id);
}
```

# 使用mapper代理的方式开发dao

如果使用原来的方法开发dao, 是先定义dao接口, 然后实现类: 

```java
public class UserDaoImpl implements UserDao {
 
	// SqlSessionFactory应该是单例存在，不应该在具体的实例中去创建，所以要外部依赖注入
	private SqlSessionFactory sqlSessionFactory;
 
	public UserDaoImpl(SqlSessionFactory sqlSessionFactory) {
			this.sqlSessionFactory = sqlSessionFactory;
	}
 
	@Override
	public User findUserById(int id) {
			// 创建SqlSession
			SqlSession sqlSession = sqlSessionFactory.openSession();
            // test.findUserById, 格式为 namespace.sqlId
			User user = sqlSession.selectOne("test.findUserById", id);
			// 关闭SqlSession
			sqlSession.close();
			return user;
	}
 
	@Override
	public List<User> findUsersByName(String name) {
			// 创建SqlSession
			SqlSession sqlSession = sqlSessionFactory.openSession();
			List<User> list = sqlSession.selectList("test.findUsersByName", name);
			// 关闭SqlSession
			sqlSession.close();
			return list;
	}
 
}

```

这样存在大量模版编码, 新的方式: Mapper代理方式（mapper接口代替dao接口, mybatis自动生成接口的代理实现类）

1、mapper接口的全限定名要和mapper映射文件的namespace值一致。
2、mapper接口的方法名称要和mapper映射文件的statement的id一致。
3、mapper接口的方法参数类型要和mapper映射文件的statement的parameterType值一致。
4、mapper接口的方法返回值类型要和mapper映射文件的statement的resultType值一致。


```java
public interface UserMapper {

	User findUserById(int id);
	List<User> findUserByName(String username);
	void insertUser(User user);
	User findUserRmap(int id);
	List<User> findUser(UserQueryVO vo);
	List<OrdersExt> findUserAndOrder();
	List<OrdersExt> findOrdersAndDetailRmap();
}
////////////////////////////////////////
// 测试
@Test
	public void test4() {
		UserMapper userMapper = session.getMapper(UserMapper.class);
		List<User> userList = userMapper.findUserByName("小明");
		System.out.println(userList);
	}
	
	@Test
	public void test5() {
		UserMapper userMapper = session.getMapper(UserMapper.class);
		User user = userMapper.findUserById(1);
		System.out.println(user);
	}


```

UserMapper.xml(默认, 不是固定的): 

```xml
<!-- 属性namespace：mapper接口的权限定名 -->
<mapper namespace="cn.xy.mapper.UserMapper">
    <select id="findUserById" parameterType="int" resultType="cn.xy.po.User" >
        select * from user where id = #{id}
    </select>
    <select id="findUserByName" parameterType="java.lang.String" resultType="cn.xy.po.User">
        select * from user where username like '%${value}%'
    </select>
    <insert id="insertUser" parameterType="cn.xy.po.User">
      <selectKey keyProperty="id" resultType="int" order="AFTER">select LAST_INSERT_ID()</selectKey>
        insert into user 
        (username, sex, birthday, address)
        values
        (#{username}, #{sex}, #{birthday}, #{address})
</insert>
</ mapper>

```


# mybatis的配置文件

## 配置文件的各部分有顺序

```
properties 属性
settings 设置
typeAliases 类型别名
typeHandlers 类型处理器
objectFactory 对象工厂
plugins 插件
environments 环境
    environment 环境变量
        transactionManager 事务管理器
        dataSource 数据源
databaseIdProvider 数据库厂商标识
mappers 映射器
```

## properties

有三个地方可以配置properties

1. 首先读取在 properties 元素体中指定的属性；
 
1. 其次，读取从 properties 元素的类路径 resource 或 url 指定的属性，且会覆盖已经指定了的重复属性；
 
1. 最后，读取作为方法参数传递的属性，且会覆盖已经从 properties 元素体和 resource 或 url 属性中加载了的重复属性。

第一和第二个:

```xml
<properties resource="org/mybatis/example/config.properties"><!-- 首先被读取 -->
  <property name="username" value="dev_user"/><!-- 次之被读取, 可以在这里设置覆盖值 -->
  <property name="password" value="F2Fa3!33TYyg"/>
  <property name="org.apache.ibatis.parsing.PropertyParser.enable-default-value" value="true"/> <!-- 开启占位符默认值 -->
</properties>
```

然后其中的属性就可以在整个配置文件中被用来替换需要动态配置的属性值

```xml
<dataSource type="POOLED">
    <!-- driver 和 url 属性将会由 config.properties 文件中对应的值来替换 -->
  <property name="driver" value="${driver}"/>
  <property name="url" value="${url}"/>
  <!--  username 和 password 将会由 properties 元素中设置的相应值来替换 -->
  <property name="username" value="${username:xiaoyu}"/><!-- 从MyBatis 3.4.2开始，你可以为占位符指定一个默认值 -->
  <property name="password" value="${password}"/>
</dataSource>
```

(第三个)
属性也可以通过 SqlSessionFactoryBuilder.build()方法传递:

```java

// 创建 properties, environment...

SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, props);// 最后被读取, 以这里设置的properties为准
// ... or ...
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment, props);
```

## settings

```xml
<settings>
    <!-- 打印查询语句, 实际就是指定日志实现, 这里指定的是"标准输出", 即日志打印到console -->
    <setting name="logImpl" value="STDOUT_LOGGING" />

  <!-- def:true 开发可能需要关闭 -->
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <!-- 允许 JDBC 支持自动生成主键 def:false -->
  <setting name="useGeneratedKeys" value="false"/>
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  <setting name="defaultExecutorType" value="SIMPLE"/>
  <setting name="defaultStatementTimeout" value="25"/>
  <setting name="defaultFetchSize" value="100"/>
  <setting name="safeRowBoundsEnabled" value="false"/>
  <!-- 数据库字段名和java属性命名规范映射, 一般打开 def:false -->
  <setting name="mapUnderscoreToCamelCase" value="true"/>
  <!-- 默认值为 SESSION，这种情况下会缓存一个会话中执行的所有查询。 若设置值为 STATEMENT，本地会话仅用在语句执行上，对相同 SqlSession 的不同调用将不会共享数据 def:session -->
  <setting name="localCacheScope" value="SESSION"/>
  <!-- 当没有为参数提供特定的 JDBC 类型时，为空值指定 JDBC 类型 -->
  <setting name="jdbcTypeForNull" value="OTHER"/>
  <!-- 指定触发 lazyload 的对象方法 def: equals,clone,hashCode,toString-->
  <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
  <!-- 指定当结果集中值为 null 的时候是否调用映射对象的 setter（map 对象时为 put）方法, 可用来做null 值初始化，def:false -->
  <setting name="callSettersOnNulls" value="false"/>
  <!-- 当返回行的所有列都是空时，MyBatis默认返回null。 当开启这个设置时，MyBatis会返回一个空实例 def:false -->
  <setting name="returnInstanceForEmptyRow" value="false"/>
</settings>
```

<img src="Snipaste_2018-05-19_15-57-42.png"><img src="Snipaste_2018-05-19_15-59-12.png"><img src="Snipaste_2018-05-19_15-59-44.png">

## typeAliases

类型别名是为 Java 类型设置一个短的名字。它只和 XML 配置有关，存在的意义仅在于用来减少类完全限定名的冗余

```xml
<typeAliases>
  <typeAlias alias="Author" type="domain.blog.Author"/>
  <typeAlias alias="Blog" type="domain.blog.Blog"/><!-- Blog可以用在任何使用domain.blog.Blog的地方 -->
  <typeAlias alias="Comment" type="domain.blog.Comment"/>
  <typeAlias alias="Post" type="domain.blog.Post"/>
  <typeAlias alias="Section" type="domain.blog.Section"/>
  <typeAlias alias="Tag" type="domain.blog.Tag"/>
</typeAliases>
```

也可以指定一个包名，MyBatis 会在包名下面搜索需要的 Java Bean

```xml
<typeAliases>
  <!-- 每一个在包 domain.blog 中的 Java Bean，在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名。 
        比如 domain.blog.Author 的别名为 author；若有注解，则别名为其注解值 -->
  <package name="domain.blog"/>
</typeAliases>
```

使用注解指定别名:

```java
@Alias("author")
public class Author {
    ...
}
```

常见的 Java 类型内建的相应的类型别名。它们都是大小写不敏感的

<img src="Snipaste_2018-05-19_16-09-21.png"><img src="Snipaste_2018-05-19_16-09-53.png">

## typeHandlers

类型处理器将获取的值以合适的方式转换成 Java 类型

### JdbcType

<img src="Snipaste_2018-05-20_10-34-36.png">


### 默认类型处理器

下面是一些默认的类型处理器

<img src="Snipaste_2018-05-19_16-12-41.png"><img src="Snipaste_2018-05-19_16-13-18.png"><img src="Snipaste_2018-05-19_16-13-55.png">

### 自定义类型处理器

#### 怎么开发自定义类型处理器

实现 org.apache.ibatis.type.TypeHandler 接口， 或继承一个很便利的类 `org.apache.ibatis.type.BaseTypeHandler`， 然后可以选择性地将它映射到一个 JDBC 类型

```java
// ExampleTypeHandler.java
/**
可以通过两种方式来指定被关联的 JDBC 类型：
1. 在类型处理器的xml配置元素上增加一个 jdbcType 属性（比如：jdbcType="VARCHAR"）；
2. 在类型处理器的类上（TypeHandler class）增加一个 @MappedJdbcTypes 注解来指定与其关联的 JDBC 类型列表。 如果在 jdbcType 属性中也同时指定，则注解方式将被忽略。
*/
@MappedJdbcTypes(JdbcType.VARCHAR)
//@MappedTypes(String.class)
public class ExampleTypeHandler extends BaseTypeHandler<String> {

  // 泛型: 通过类型处理器的泛型，MyBatis 可以得知该类型处理器处理的 Java 类型，不过这种行为可以通过两种方法改变
  /**
  1. 在类型处理器的配置元素（typeHandler element）上增加一个 javaType 属性（比如：javaType="String"）；
  2. 在类型处理器的类上（TypeHandler class）增加一个 @MappedTypes 注解来指定与其关联的 Java 类型列表。 如果在 javaType 属性中也同时指定，则注解方式将被忽略
  */
  
  /**
  // 重新定义要发往数据库的数据, 另外三个方法中将从数据库读出的数据类型进行转换
  * 为db 中不能为 null 的字段设置值
  */
  @Override 
  public void setNonNullParameter(PreparedStatement ps, int i, String parameter, JdbcType jdbcType) throws SQLException {

    ps.setString(i, parameter);// 这里没有做任何处理, 就存入数据库了

    // 比如要做一下处理: 将Date类型的时间转为varchar类型， 当然，是为 BaseTypeHandler<Date> 开发的
    // preparedStatement.setString(i, String.valueOf(date.getTime()));
  }

  /**
  * 编辑从 db 中查出的数据， 转为 Java 类型
  */
  @Override
  public String getNullableResult(ResultSet rs, String columnName) throws SQLException {
    return rs.getString(columnName);
    // return new Date(resultSet.getLong(s));
  }

  @Override
  public String getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
    return rs.getString(columnIndex);
    //  return new Date(resultSet.getLong(i));
  }

  @Override
  public String getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
    return cs.getString(columnIndex);
    //  return callableStatement.getDate(i);
  }
}
```

#### 怎么启用呢

启用了我们自定义的这个TypeHandler之后，数据的读写都会被这个类所过滤 

同时使用这个的类型处理器将会覆盖已经存在的处理 Java 的 String 类型属性和 VARCHAR 参数及结果的类型处理器

注册这个typeHandler有2种方式

第一种: (这种方式只能解决读取时的数据转换问题)

```xml
<!-- mybatis-config.xml -->
<typeHandlers>
  <typeHandler handler="org.mybatis.example.ExampleTypeHandler"/>
</typeHandlers>
<!-- or -->
<typeHandlers>
    <package name="org.sang.db"/><!-- Handler所在的包， 这会自动检测包下所有的 type handlers -->
</typeHandlers>
```

第二种: 在 insert 和 select 元素中分别配置, 对应读和写 - 这种控制粒度更细

```xml
<resultMap id="userResultMap" type="org.sang.bean.User">
    <result typeHandler="org.sang.db.MyDateTypeHandler" column="regTime" javaType="java.util.Date"
            jdbcType="VARCHAR"
            property="regTime"/>
</resultMap>
<select id="getUser" resultMap="userResultMap">
    select * from user4
</select>
<insert id="insertUser" parameterType="org.sang.bean.User">
    INSERT INTO user4(username,password,regTime) VALUES (#{username},#{password},#{regTime,javaType=Date,jdbcType=VARCHAR,typeHandler=org.sang.db.MyDateTypeHandler})
</insert>
```

### 自动检索类型处理器

```xml
<!-- mybatis-config.xml -->
<typeHandlers>
    <!-- 此时, 只能通过注解方式来指定 JDBC 的类型 -->
  <package name="org.mybatis.example"/>
</typeHandlers>
```

### 泛型类型处理器

创建一个能够处理多个类的泛型类型处理器, 像 EnumTypeHandler 和 EnumOrdinalTypeHandler 都是泛型类型处理器

```java
//GenericTypeHandler.java
public class GenericTypeHandler<E extends MyObject> extends BaseTypeHandler<E> {

  private Class<E> type;

  public GenericTypeHandler(Class<E> type) {
    if (type == null) throw new IllegalArgumentException("Type argument cannot be null");
    this.type = type;
  }

//   ...

```

### 处理枚举类型

若想映射枚举类型 Enum，则需要从 EnumTypeHandler 或者 EnumOrdinalTypeHandler 中选一个来使用。

```xml
<!-- mybatis-config.xml -->
<typeHandlers>
  <typeHandler handler="org.apache.ibatis.type.EnumOrdinalTypeHandler" javaType="java.math.RoundingMode"/>
</typeHandlers>
```

## 对象工厂（objectFactory）-自定义查询结果的实例化

MyBatis 每次创建结果对象的新实例时，它都会使用一个对象工厂（ObjectFactory）实例来完成。

默认的对象工厂需要做的仅仅是实例化目标类，要么通过默认构造方法，要么在参数映射存在的时候通过参数构造方法来实例化。 如果想覆盖对象工厂的默认行为，则可以通过创建自己的对象工厂来实现

```java
// ExampleObjectFactory.java
public class ExampleObjectFactory extends DefaultObjectFactory {

  /*
  * 处理默认构造方法
  */
  public Object create(Class type) {
    return super.create(type);
  }

  // 处理带参数的构造方法
  //
  public Object create(Class type, List<Class> constructorArgTypes, List<Object> constructorArgs) {
    return super.create(type, constructorArgTypes, constructorArgs);
  }

  /*此方法可以被用来配置 当前这个ObjectFactory，
  * 在初始化 ObjectFactory 实例后，xml配置中 objectFactory 元素体中定义的属性会被传递给 setProperties 方法。
  * 方法参数值来自: 下面 xml 中的配置
  */
  public void setProperties(Properties properties) {
    super.setProperties(properties);
  }
  public <T> boolean isCollection(Class<T> type) {
    return Collection.class.isAssignableFrom(type);
  }}
```

```xml
<!-- mybatis-config.xml -->
<objectFactory type="org.mybatis.example.ExampleObjectFactory">
  <property name="someProperty" value="100"/><!-- 属性会被传递给 setProperties 方法 -->
</objectFactory>
```

## 插件（plugins）拦截器

Mybatis 为我们提供了一个 Interceptor 接口，通过实现该接口就可以定义我们自己的拦截器。

> 除了用插件来修改 MyBatis 核心行为之外，还可以通过完全覆盖配置类来达到目的。只需继承后覆盖其中的每个方法，再把它传递到 SqlSessionFactoryBuilder.build(myConfig) 方法即可。

* Executor (update, query, flushStatements, commit, rollback, getTransaction, close, isClosed)
* ParameterHandler (getParameterObject, setParameters)
* ResultSetHandler (handleResultSets, handleOutputParameters)
* StatementHandler (prepare, parameterize, batch, update, query)

```java
/**
  * 打印 SQL
  *
  * 注解 @Intercepts 用于表明当前的对象是一个 Interceptor
  * 而 @Signature则表明要拦截的接口、方法以及对应的参数类型。
  */
@Intercepts({@Signature(
        type = Executor.class,
        method = "update",
        args = {MappedStatement.class, Object.class}
), @Signature(type = Executor.class, method = "query",
        args = {MappedStatement.class, Object.class, RowBounds.class, ResultHandler.class})})
private static class SqlInterceptor implements Interceptor {
    @Override
    public Object intercept(Invocation invocation) throws Throwable {
        MappedStatement mappedStatement = (MappedStatement) invocation.getArgs()[0];
        Object parameter = null;
        if (invocation.getArgs().length > 1) {
            parameter = invocation.getArgs()[1];
        }

        BoundSql boundSql = mappedStatement.getBoundSql(parameter);
        Configuration configuration = mappedStatement.getConfiguration();
        Object returnVal = invocation.proceed();

        //获取sql语句
        String sql = getSql(configuration, boundSql);
        log.info("Mybatis 拦截器获取SQL:{}",sql);
        return returnVal;
    }

    /**
      * 在 plugin 方法中我们可以决定是否要进行拦截, 如果拦截, 就返回封装后的对象
      * 如果不拦截, 则返回原始对象
      *
      * Mybatis 中有一个叫做Plugin 的类，里面有一个静态方法
      * wrap(Object target,Interceptor interceptor)，通过该方法可以决定要返回的对象是目标对象还是对应的代理。
      */
    @Override
    public Object plugin(Object o) {
        // 表示始终拦截
        return Plugin.wrap(o, this);
    }

    /**
      * 用于在 Mybatis 配置文件中指定一些属性的。
      */
    @Override
    public void setProperties(Properties properties) {
    }

    /**
      * 获取SQL
      * @param configuration
      * @param boundSql
      * @return
      */
    private String getSql(Configuration configuration, BoundSql boundSql) {
        Object parameterObject = boundSql.getParameterObject();
        List<ParameterMapping> parameterMappings = boundSql.getParameterMappings();
        String sql = boundSql.getSql().replaceAll("[\\s]+", " ");
        if (parameterObject == null || parameterMappings.size() == 0) {
            return sql;
        }
        TypeHandlerRegistry typeHandlerRegistry = configuration.getTypeHandlerRegistry();
        if (typeHandlerRegistry.hasTypeHandler(parameterObject.getClass())) {
            sql = sql.replaceFirst("\\?", getParameterValue(parameterObject));
        } else {
            MetaObject metaObject = configuration.newMetaObject(parameterObject);
            for (ParameterMapping parameterMapping : parameterMappings) {
                String propertyName = parameterMapping.getProperty();
                if (metaObject.hasGetter(propertyName)) {
                    Object obj = metaObject.getValue(propertyName);
                    sql = sql.replaceFirst("\\?", getParameterValue(obj));
                } else if (boundSql.hasAdditionalParameter(propertyName)) {
                    Object obj = boundSql.getAdditionalParameter(propertyName);
                    sql = sql.replaceFirst("\\?", getParameterValue(obj));
                }
            }
        }
        return sql;
    }

    private String getParameterValue(Object obj) {
        String value = null;
        if (obj instanceof String) {
            value = "'" + obj.toString() + "'";
        } else if (obj instanceof Date) {
            DateFormat formatter = DateFormat.getDateTimeInstance(DateFormat.DEFAULT, DateFormat.DEFAULT, Locale.CHINA);
            value = "'" + formatter.format(obj) + "'";
        } else {
            if (obj != null) {
                value = obj.toString();
            } else {
                value = "";
            }
        }
        return value;
    }
}
```

然后添加 interceptor: 

```java
@Bean
    public SqlSessionFactoryBean sqlSessionFactoryBean(DataSource dataSource) throws IOException {
        SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
        factoryBean.setDataSource(dataSource);
        if (StringUtils.isInvalid(mapperLocation)) {
            throw new RuntimeException(">>> mapper directory is invalid -> " + mapperLocation);
        }
        PathMatchingResourcePatternResolver resolver = new PathMatchingResourcePatternResolver();
        factoryBean.setMapperLocations(resolver.getResources(mapperLocation));
        factoryBean.setPlugins(new Interceptor[]{this.pageHelper(), sql()});
        return factoryBean;
    }
```

## 配置环境（environments）-数据源-事务管理

### 怎么配置environment

开发、测试和生产环境需要有不同的配置

尽管可以配置多个环境，每个 SqlSessionFactory 实例只能选择其一

果你想连接两个数据库，就需要创建两个 SqlSessionFactory 实例，每个数据库对应一个

```java

// environment不传则是默认 env， 可以在 xml 中配置 默认 env

SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment);
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment, properties);

```

```xml
<environments default="development"><!-- 默认的环境 ID -->
  <!-- 这个 env 会被设为 默认 -->
  <environment id="development">

  <!-- 如果你正在使用 Spring + MyBatis，则没有必要配置事务管理器， 因为 Spring 模块会使用自带的管理器来覆盖前面的配置 -->

    <!-- 这个配置就是直接使用了 JDBC 的提交和回滚设置
    - type=”[JDBC|MANAGED]
     -->
    <transactionManager type="JDBC">
      <property name="..." value="..."/>
    </transactionManager>
      <!-- 或者 -->
      <!-- 这个配置几乎没做什么。它从来不提交或回滚一个连接，而是让容器来管理事务的整个生命周期（比如 JEE 应用服务器的上下文）。 默认情况下它会关闭连接，然而一些容器并不希望这样，因此需要将 closeConnection 属性设置为 false 来阻止它默认的关闭行为 -->
    <transactionManager type="MANAGED">
        <property name="closeConnection" value="false"/>
    </transactionManager>
    
    <!-- 有三种内建的数据源类型（也就是 type=”[UNPOOLED|POOLED|JNDI]”） -->
    <dataSource type="POOLED">
      <property name="driver" value="${driver}"/>
      <property name="url" value="${url}"/>
      <property name="username" value="${username}"/>
      <property name="password" value="${password}"/>
    </dataSource>
  </environment>
</environments>
```

### 自定义mybatis事务管理

前面这两种事务管理器类型不过是类型别名，可以使用 TransactionFactory 接口的实现类的完全限定名或类型别名代替它们

```java
public interface TransactionFactory {
  /*
  - xml中的配置会传入
  */
  void setProperties(Properties props);  
  
  Transaction newTransaction(Connection conn);
  Transaction newTransaction(DataSource dataSource, TransactionIsolationLevel level, boolean autoCommit);  
}
```

也需要创建一个 Transaction 接口的实现类

```java
public interface Transaction {
  Connection getConnection() throws SQLException;
  void commit() throws SQLException;
  void rollback() throws SQLException;
  void close() throws SQLException;
  Integer getTimeout() throws SQLException;
}
```

使用这两个接口，可以完全自定义 MyBatis 对事务的处理

### 自定义数据源-第三方数据源

可以通过实现接口 org.apache.ibatis.datasource.DataSourceFactory 来使用第三方数据源：
也可以继承 org.apache.ibatis.datasource.unpooled.UnpooledDataSourceFactory

```java
public interface DataSourceFactory {
  void setProperties(Properties props);
  DataSource getDataSource();
}

///////////////////////////////////

public class C3P0DataSourceFactory extends UnpooledDataSourceFactory {

  public C3P0DataSourceFactory() {
    this.dataSource = new ComboPooledDataSource();
  }
}

```

然后需要在 xml 中配置使其工作

```xml
<dataSource type="org.myproject.C3P0DataSourceFactory">
  <property name="driver" value="org.postgresql.Driver"/>
  <property name="url" value="jdbc:postgresql:mydb"/>
  <property name="username" value="postgres"/>
  <property name="password" value="root"/>
</dataSource>

```

## 对不同数据库的支持（databaseIdProvider）

MyBatis 可以根据不同的数据库厂商执行不同的语句

这一特性是通过映射语句中的 databaseId 属性， 如果在Mapper.xml中同时找到带有 databaseId 和不带 databaseId 的相同语句，优先使用带databaseId

```xml

<!-- DB_VENDOR databaseIdProvider 也可以自定义成别的 Provider， 通过接口 DatabaseIdProvider ，然后配置 type=实现类 -->

<databaseIdProvider type="DB_VENDOR">
  <property name="SQL Server" value="sqlserver"/>
  <property name="DB2" value="db2"/>        
  <property name="Oracle" value="oracle" />
</databaseIdProvider>

```



## 指定映射器（mappers）位置

四种方法配置mybatis到哪里去找mapper的xml文件

```xml
<!-- classpath -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
<!-- 资源定位符（URL） -->
<mappers>
  <mapper url="file:///var/mappers/AuthorMapper.xml"/>
  <mapper url="file:///var/mappers/BlogMapper.xml"/>
  <mapper url="file:///var/mappers/PostMapper.xml"/>
</mappers>
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
  <mapper class="org.mybatis.builder.BlogMapper"/>
  <mapper class="org.mybatis.builder.PostMapper"/>
</mappers>
<!-- 将包内的映射器接口实现全部注册为映射器 -->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

# mapper映射配置xml文件

## mapper.xml支持哪些元素

```
SQL 映射文件有很少的几个顶级元素（按照它们应该被定义的顺序）：

cache – 给定命名空间的缓存配置。
cache-ref – 其他命名空间缓存配置的引用。
resultMap – 是最复杂也是最强大的元素，用来描述如何从数据库结果集中来加载对象。
parameterMap – 已废弃！老式风格的参数映射。内联参数是首选,这个元素可能在将来被移除，这里不会记录。
sql – 可被其他语句引用的可重用语句块。
insert – 映射插入语句
update – 映射更新语句
delete – 映射删除语句
select – 映射查询语句

```

## 查询select元素

```xml
<!-- 接受一个 int（或 Integer）类型的参数，并返回一个 HashMap 类型的对象，其中的键是列名，值便是结果行中的对应值 -->
<select id="selectPerson" parameterType="int" resultType="hashmap">
  SELECT * FROM PERSON WHERE ID = #{id}<!-- 这样的一个参数 #{id} <=> 在 SQL 中会由一个“?”来标识，并被传递到一个新的预处理语句中 -->
</select>
```

```xml
<select
  id="selectPerson"
  parameterType="int" // 可选的， 因为 MyBatis 可以通过 TypeHandler 推断出具体传入语句的参数
  resultType="hashmap"  // 注意如果是集合情形，那应该是集合可以包含的类型，而不能是集合本身
  resultMap="personResultMap" // 外部 resultMap 的命名引用
  flushCache="false"// 将其设置为 true，任何时候只要语句被调用，都会导致本地缓存和二级缓存都会被清空，默认值：false， 对于 更新操作 默认true
  useCache="true"// 将其设置为 true，将会导致本条语句的结果被二级缓存，默认值：对 select 元素为 true
  timeout="10000"// 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为 unset（依赖驱动）
  fetchSize="256"// 这是尝试影响驱动程序每次批量返回的结果行数和这个设置值相等。默认值为 unset（依赖驱动）
  statementType="PREPARED"// STATEMENT，PREPARED 或 CALLABLE 的一个。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。
  databaseId=""
  resultSetType="FORWARD_ONLY",>// upset 驱动决定
```

## insert, update 和 delete

```xml
<!-- attention: 最后一个字段末尾没有 "," -->
<insert id="userF01" parameterType="io.github.xiaoyureed.sql.dto.UserF01InM01">
        INSERT INTO user (
          id,
          name,
          pwd
        )
        VALUES (
          #{id},
          #{name},
          #{password}
        )
</insert>

<insert
  id="insertAuthor"
  parameterType="domain.blog.Author"// 可选, 因为 MyBatis 可以通过 TypeHandler 推断出具体传入语句的参数，默认值为 unset。
  flushCache="true"// 将其设置为 true，任何时候只要语句被调用，都会导致本地缓存和二级缓存都会被清空，默认值：true（对应插入、更新和删除语句）
  statementType="PREPARED"
  useGeneratedKeys=""// （仅对 insert 和 update 有用）这会令 MyBatis 使用 JDBC 的 getGeneratedKeys 方法来取出由数据库内部生成的主键（比如：像 MySQL 和 SQL Server 这样的关系数据库管理系统的自动递增字段），默认值：false
  keyProperty=""// （仅对 insert 和 update 有用）指定属性，MyBatis 会自动填充数据到指定的属性上. 默认：unset。如果希望得到多个生成的列，也可以是逗号分隔的属性名称列表。
  keyColumn=""// （仅对 insert 和 update 有用）通过生成的键值设置表中的列名，这个设置仅在某些数据库（像 PostgreSQL）是必须的，当主键列不是表中的第一列的时候需要设置。如果希望得到多个生成的列，也可以是逗号分隔的属性名称列表
  databaseId=""
  timeout="20">

<update
  id="updateAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  timeout="20">

<delete
  id="deleteAuthor"
  parameterType="domain.blog.Author"
  flushCache="true"
  statementType="PREPARED"
  timeout="20">
```

```xml
<!-- 主键自动生成， 见下面的demo -->
<selectKey
  keyProperty="id"// selectKey 语句结果应该被设置的目标属性
  keyColumn=""// 匹配属性的返回结果集中的列名称
  resultType="int"// 结果的类型, 可选
  order="BEFORE"// 这可以被设置为 BEFORE 或 AFTER。如果设置为 BEFORE，那么它会首先选择主键，设置 keyProperty 然后执行插入语句。如果设置为 AFTER，那么先执行插入语句，然后是 selectKey 元素
  statementType="PREPARED">
```

看看具体demo

```xml
<insert id="insertAuthor">
  insert into Author (id,username,password,email,bio)
  values (#{id},#{username},#{password},#{email},#{bio})
</insert>
<!-- 使用自动生成主键 -->
<insert id="insertAuthor" useGeneratedKeys="true"
    keyProperty="id"> // id ：java对象的属性 
  insert into Author (username,password,email,bio)
  values (#{username},#{password},#{email},#{bio})
</insert>
<!-- 如果你的数据库还支持多行插入, 你也可以传入一个Authors数组或集合， -->
<insert id="insertAuthor" useGeneratedKeys="true"
    keyProperty="id">
  insert into Author (username, password, email, bio) values
  <foreach item="item" collection="list" separator=",">
    (#{item.username}, #{item.password}, #{item.email}, #{item.bio})
  </foreach>
</insert>

<insert id="insertAuthor">
  <!-- 另一种方式 自动 id ， 不推荐-->
  <selectKey keyProperty="id" resultType="int" order="BEFORE">
    select CAST(RANDOM()*1000000 as INTEGER) a from SYSIBM.SYSDUMMY1
  </selectKey>
  insert into Author
    (id, username, password, email,bio, favourite_section)
  values
    (#{id}, #{username}, #{password}, #{email}, #{bio}, #{favouriteSection,jdbcType=VARCHAR})
</insert>

<update id="updateAuthor">
  update Author set
    username = #{username},
    password = #{password},
    email = #{email},
    bio = #{bio}
  where id = #{id}
</update>

<delete id="deleteAuthor">
  delete from Author where id = #{id}
</delete>
```

## sql元素-重用sql

定义可重用的 SQL 代码段，可以包含在其他语句中

```xml
<sql id="userColumns"> ${alias}.id,${alias}.username,${alias}.password </sql>

<select id="selectUsers" resultType="map">
  select
    <include refid="userColumns"><property name="alias" value="t1"/></include> ,
    <include refid="userColumns"><property name="alias" value="t2"/></include>
  from some_table t1
    cross join some_table t2
</select>

<!-- 属性值也可以被传入 include 元素的 refid 属性里 -->
<include refid="${include_target}"/>

<sql id="sometable">
  ${prefix}Table
</sql>

<sql id="someinclude">
  from
    <include refid="${include_target}"/>
</sql>

<select id="select" resultType="map">
  select
    field1, field2, field3
  <include refid="someinclude">
    <property name="prefix" value="Some"/>
    <property name="include_target" value="sometable"/>
  </include>
</select>
```

## 动态sql

### if

```xml
<!-- if -->
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG WHERE state = ‘ACTIVE’
  <if test="title != null">
    AND title like #{title}
  </if>
  <if test="author != null and author.name != null">
    AND author_name like #{author.name}
  </if>
</select>

```

### choose,when,otherwise-多个when中择其一

```xml

<!-- choose, when, otherwise 不想应用到所有的条件语句，而只想从中择其一项 -->
<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG WHERE state = ‘ACTIVE’
  <choose>
    <when test="title != null">
      AND title like #{title}
    </when>
    <when test="author != null and author.name != null">
      AND author_name like #{author.name}
    </when>
    <otherwise>
      AND featured = 1
    </otherwise>
  </choose>
</select>

```

### where,set,trim-除去sql中的关键字

```xml

<!-- trim, where, set -->

<select id="findActiveBlogLike"
     resultType="Blog">
  SELECT * FROM BLOG 
  <!-- where特别有用, 将 where关键字提取出来, sql 适用性更强 (避免最终sql是这样 "SELECT * FROM BLOG WHERE") -->
  <!-- where 元素只会在至少有一个子元素的条件返回 SQL 子句的情况下才去插入“WHERE”子句。而且，若语句的开头为“AND”或“OR”，where 元素也会将它们去除。 -->
  <where> 
    <if test="state != null">
         state = #{state}
    </if> 
    <if test="title != null">
        AND title like #{title}
    </if>
    <if test="author != null and author.name != null">
        AND author_name like #{author.name}
    </if>
  </where>
</select>
<!-- trim是更普适的元素, 这里的trim元素作用和where元素等价 -->
<!-- 它的作用是移除所有指定在 prefixOverrides 属性中的内容，并且插入 prefix 属性中指定的内容。 -->
<trim prefix="WHERE" prefixOverrides="AND |OR ">
  ... 
</trim>

<!-- 类似的用于动态更新语句的解决方案叫做 set。set 元素可以用于动态包含需要更新的列，而舍去其它的 -->
<update id="updateAuthorIfNecessary">
  update Author
<!-- set 元素会动态前置 SET 关键字，同时也会删掉无关的逗号 -->
    <set>
      <if test="username != null">username=#{username},</if>
      <if test="password != null">password=#{password},</if>
      <if test="email != null">email=#{email},</if>
      <if test="bio != null">bio=#{bio}</if>
    </set>
  where id=#{id}
</update>
<!-- set元素等价于 -->
<trim prefix="SET" suffixOverrides=",">
  ...
</trim>

<!-- foreach -->
<select id="selectPostIn" resultType="domain.blog.Post">
  SELECT *
  FROM POST P
  WHERE ID in
  <!-- index 是当前迭代的次数，item 的值是本次迭代获取的元素。当使用 Map 对象（或者 Map.Entry 对象的集合）时，index 是键，item 是值。 -->
  <!-- collection="list" 接受 Java 中传过来的可迭代值 -->
  <foreach item="item" index="index" collection="list"
      open="(" separator="," close=")">
        #{item}
  </foreach>
</select>

<!-- bind 元素可以从 OGNL 表达式中创建一个变量并将其绑定到上下文。 -->
<select id="selectBlogsLike" resultType="Blog">
  <bind name="pattern" value="'%' + _parameter.getTitle() + '%'" />
  SELECT * FROM BLOG
  WHERE title LIKE #{pattern}
</select>

```

### 多数据库支持

```xml

<!-- 多数据库支持 -->
<!-- 一个配置了“_databaseId”变量的 databaseIdProvider 可用于动态代码中，这样就可以根据不同的数据库厂商构建特定的语句 -->
<insert id="insert">
  <selectKey keyProperty="id" resultType="int" order="BEFORE">
    <if test="_databaseId == 'oracle'">
      select seq_users.nextval from dual
    </if>
    <if test="_databaseId == 'db2'">
      select nextval for seq_users from sysibm.sysdummy1"
    </if>
  </selectKey>
  insert into users values (#{id}, #{name})
</insert>

```

### 动态 SQL 中的可插拔脚本语言(如Apache Velocity )

可以通过实现以下接口来插入一种语言：

```java
public interface LanguageDriver {
  ParameterHandler createParameterHandler(MappedStatement mappedStatement, Object parameterObject, BoundSql boundSql);
  SqlSource createSqlSource(Configuration configuration, XNode script, Class<?> parameterType);
  SqlSource createSqlSource(Configuration configuration, String script, Class<?> parameterType);
}
```

一旦设定了自定义语言驱动，你就可以在 mybatis-config.xml 文件中将它设置为默认语言

```xml
<typeAliases>
  <typeAlias type="org.sample.MyLanguageDriver" alias="myLanguage"/>
</typeAliases>
<settings>
  <setting name="defaultScriptingLanguage" value="myLanguage"/>
</settings>
```

针对特殊的语句指定特定语言，可以通过如下的 lang 属性来完成

```xml
<select id="selectBlog" lang="myLanguage">
  SELECT * FROM BLOG
</select>
```

或者基于注解配置脚本语言

```java
public interface Mapper {
  @Lang(MyLanguageDriver.class)
  @Select("SELECT * FROM BLOG")
  List<Blog> selectBlog();
}
```

## 参数（Parameters）

### 常规用法

```xml
<select id="selectUsers" resultType="User">
  select id, username, password
  from users
  where id = #{id}
</select>

<!-- 如果传入一个复杂的对象，行为就会有一点不同了 -->
<!-- User的 id、username 和 password 属性将会被查找，然后将它们的值传入预处理语句的参数中 -->
<insert id="insertUser" parameterType="User">
  insert into users (id, username, password)
  values (#{id}, #{username}, #{password})
</insert>

<!-- 类似的， 传递HashMap<String, Object>综合查询用户信息 -->
<select id="findUserByHashmap" parameterType="hashmap" resultType="user">
    select * from user where id=#{id} and username like '%${username}%'
</select>


```

### 在占位符里设置参数类型和特殊的类型处理器类

在占位符里设置参数类型和特殊的类型处理器类

```xml

#{property,javaType=int,jdbcType=NUMERIC,typeHandler=MyTypeHandler}

<!-- 比如： -->
#{age,javaType=int,jdbcType=NUMERIC,typeHandler=MyTypeHandler}
```

对于数值类型，还有一个小数保留位数的设置，来确定小数点后保留的位数。

```xml
#{height,javaType=double,jdbcType=NUMERIC,numericScale=2}
```

### 两种字符串替换方式-#$区别

默认情况下,使用 #{} 格式的语法会导致 MyBatis 创建 PreparedStatement 参数并安全地设置参数（就像使用 ? 一样）。这样做更安全，更迅速，通常也是首选做法。

不过有时就是想直接在 SQL 语句中插入一个不转义的字符串 ${xxx}



## Result Map,属性字段映射

延迟加载 - [cheeck here](#延迟加载)

### 简单结果映射

```xml
<!-- 简单的映射 -->
<select id="selectUsers" resultType="map"><!-- 简单地将所有的列名映射到 HashMap 的键上, 值映射到hashmap的值上 -->
  select id, username, hashedPassword
  from some_table
  where id = #{id}
</select>

<!-- 进一步进化, 定义一个pojo, 将数据映射到它里面 -->
<!-- MyBatis 会在幕后自动创建一个 ResultMap，再基于属性名来映射列到 JavaBean 的属性上 -->

<select id="selectUsers" resultType="User"><!-- 这里可以使用类型别名进行简化 -->
  select
    user_id             as "id",
    user_name           as "userName",
    hashed_password     as "hashedPassword"
  from some_table
  where id = #{id}
</select>

<!-- 等价配置 - 使用resultMap解决属性和字段不一致， 实际开发用上面简单的方式就可以了 -->

<resultMap id="userResultMap" type="User">

  <!-- id 和 result 都将一个列的值映射到一个简单数据类型(字符串,整型,双精度浮点数,日期等)的属性或字段。对于反则数据类型：javaType， jdbcType，typeHandler 需要指定 -->

  <id property="id" column="user_id" />
  <result property="username" column="user_name"/>
  <result property="password" column="hashed_password"/>

</resultMap>

<select id="selectUsers" resultMap="userResultMap">
  select user_id, user_name, hashed_password
  from some_table
  where id = #{id}
</select>
```

### resultMap高级结果映射

#### resultMap总共有哪些元素

```xml
<!-- 超复杂的 Result Map -->
<resultMap id="detailedBlogResultMap" type="Blog"><!-- autoMapping: 如果设置这个属性，MyBatis将会为这个ResultMap开启或者关闭自动映射。这个属性会覆盖全局的属性 autoMappingBehavior。默认值为：unset。 -->

  <constructor><!-- 用于在实例化类时，注入结果到构造方法中 -->
    <idArg column="blog_id" javaType="int"/><!-- 标记出作为 ID 的结果可以帮助提高整体性能 -->
    <arg/><!-- 将被注入到构造方法的一个普通结果 -->
  </constructor>

  <id />
  <!-- 注入到字段或 JavaBean 属性的普通结果,  property支持复杂形式的属性导航如 address.street.number, 此外, 还支持 typeHandler -->
  <result property="title" column="blog_title"/>

  <!-- 一个复杂类型的关联 -->
  <association property="author" javaType="Author">
    <id property="id" column="author_id"/>
    <result property="username" column="author_username"/>
    <result property="password" column="author_password"/>
    <result property="email" column="author_email"/>
    <result property="bio" column="author_bio"/>
    <result property="favouriteSection" column="author_favourite_section"/>
  </association>

  <!-- 一个复杂类型的集合 -->
  <collection property="posts" ofType="Post">
    <id property="id" column="post_id"/>
    <result property="subject" column="post_subject"/>
    <association property="author" javaType="Author"/>
    <collection property="comments" ofType="Comment">
      <id property="id" column="comment_id"/>
    </collection>
    <collection property="tags" ofType="Tag" >
      <id property="id" column="tag_id"/>
    </collection>
    <discriminator javaType="int" column="draft"><!-- discriminator: 使用结果值来决定使用哪个 resultMap -->
      <case value="1" resultType="DraftPost"/>
    </discriminator>
  </collection>
  
</resultMap>
```


最好一步步地建立结果映射。单元测试可以在这个过程中起到很大帮助。如果你尝试一次创建一个像上面示例那样的巨大的结果映射， 那么很可能会出现错误而且很难去使用它来完成工作   

#### constructor标签, 构造方法

```xml
<!-- MyBatis搜索一个声明了三个形参的的构造方法，以 java.lang.Integer, java.lang.String and int 的顺序排列。 -->
<constructor>
   <idArg column="id" javaType="int" name="id" /><!-- 如果类中存在名称和类型相同的属性，那么可以省略 javaType 。 -->
   <arg column="age" javaType="_int" name="age" />
   <arg column="username" javaType="String" name="username" />
</constructor>
```

对应的构造方法:

```java
public class User {
   //被注入到这个构造方法
   // @Param 添加后可以不按照parameter顺序， 按照参数名称确定constructor
   public User(@Param Integer id, @Param String username, @Param int age) {
     //...
  }
//...
}
```

#### association关联

```xml
<!-- property支持映 射 到 一 些 复 杂 的 东 西 : “address.street.number -->
<association property="author" column="blog_author_id" javaType="Author">
  <id property="id" column="author_id"/>
  <result property="username" column="author_username"/>
</association>
```

##### 关联的嵌套查询

```xml
<!-- 博客的结果映射：描 述了“selectAuthor”语句应该被用来加载它的 author 属性 -->
<resultMap id="blogResult" type="Blog">
    <!-- column也可以这样配置: column="{user_id=id, username=username}" 这是针对符合主键的情况 -->
    <!-- select 指定一个 select-->
    <!-- fetchType 可选的。有效值为 lazy和eager。 如果使用了，它将取代全局配置参数lazyLoadingEnabled。 -->

  <association property="author" column="author_id" javaType="Author" select="selectAuthor"/>
  
</resultMap>

<!-- 一个来加载博客, -->
<select id="selectBlog" resultMap="blogResult">
  SELECT * FROM BLOG WHERE ID = #{id}
</select>

<!-- 另外一个来加载作者 -->
<select id="selectAuthor" resultType="Author">
  SELECT * FROM AUTHOR WHERE ID = #{id}
</select>
```

但是对于大型数据集合和列表这种方式将不会表现很好, 会导致成百上千的 SQL 语句被执行 （即 `N+1 查询问题`：你执行了一个单独的 SQL 语句来获取结果列表(就是“+1”)，对返回的每条记录,你执行了一个查询语句来为每个加载细节(就是“N”)）, 那么引入了:

关联的嵌套结果

##### 关联的嵌套结果

```xml
<resultMap id="blogResult" type="Blog">
  <!-- id 必须指定， 提高mybatis性能 -->
  <id property="id" column="blog_id" />
  <result property="title" column="blog_title"/>
  <association property="author" column="blog_author_id" javaType="Author" resultMap="authorResult"/>
  <!-- resultMap 重用 -->
  <association property="coAuthor"
    resultMap="authorResult"
    columnPrefix="co_" /> <!--效果等效于：select CA.id  as co_author_id 。。。 -->
</resultMap>

<!-- 这里将这个author映射独立出来, 可以重用, 如果只是使用一次, 可以不独立出来, 见下面 -->
<resultMap id="authorResult" type="Author">
  <id property="id" column="author_id"/>
  <result property="username" column="author_username"/>
  <result property="password" column="author_password"/>
  <result property="email" column="author_email"/>
  <result property="bio" column="author_bio"/>
</resultMap>

<!-- //////////////////////////////////////////////////////////////// -->
<!-- 不重用的写法 -->
<resultMap id="blogResult" type="Blog">
  <id property="id" column="blog_id" />
  <result property="title" column="blog_title"/>
  <association property="author" javaType="Author">
    <id property="id" column="author_id"/>
    <result property="username" column="author_username"/>
    <result property="password" column="author_password"/>
    <result property="email" column="author_email"/>
    <result property="bio" column="author_bio"/>
  </association>
</resultMap>

<!-- //////////////////////////////////// -->

<!-- select -->
<select id="selectBlog" resultMap="blogResult">
  select
    B.id            as blog_id,
    B.title         as blog_title,
    B.author_id     as blog_author_id,
    A.id            as author_id,
    A.username      as author_username,
    A.password      as author_password,
    A.email         as author_email,
    A.bio           as author_bio
  from Blog B left outer join Author A on B.author_id = A.id
  where B.id = #{id}
</select>
```

因为结果中的列名与resultMap中的列名不同, 怎么重用resultMap呢

可以指定columnPrefix去重用映射co-author结果的resultMap

```xml
<resultMap id="blogResult" type="Blog">
  <id property="id" column="blog_id" />
  <result property="title" column="blog_title"/>
  <association property="author"
    resultMap="authorResult" />
  <association property="coAuthor"
    resultMap="authorResult"
    columnPrefix="co_" />
</resultMap>
```

#### 集合collection关联

相当于 1对n

```java
// Blog类
private List<Post> posts;
```

```xml
<collection property="posts" ofType="domain.blog.Post"><!-- ofType 标识集合的元素类型 -->
  <id property="id" column="post_id"/>
  <result property="subject" column="post_subject"/>
  <result property="body" column="post_body"/>
</collection>
```

##### 集合的嵌套查询

```xml
<resultMap id="blogResult" type="Blog">
<!-- javaType可以省略 -->
  <collection property="posts" javaType="ArrayList" column="id" ofType="Post" select="selectPostsForBlog"/>
</resultMap>

<select id="selectBlog" resultMap="blogResult">
  SELECT * FROM BLOG WHERE ID = #{id}
</select>

<select id="selectPostsForBlog" resultType="Post">
  SELECT * FROM POST WHERE BLOG_ID = #{id}
</select>
```

##### 集合的嵌套结果

```xml
<resultMap id="blogResult" type="Blog">
  <id property="id" column="blog_id" />
  <result property="title" column="blog_title"/>
  <collection property="posts" ofType="Post">
    <id property="id" column="post_id"/>
    <result property="subject" column="post_subject"/>
    <result property="body" column="post_body"/>
  </collection>
</resultMap>
<!-- //////////////////////////////////////// -->
<!-- 如果希望重用resultMap -->
<resultMap id="blogResult" type="Blog">
  <id property="id" column="blog_id" />
  <result property="title" column="blog_title"/>
  <collection property="posts" ofType="Post" resultMap="blogPostResult" columnPrefix="post_"/>
</resultMap>

<resultMap id="blogPostResult" type="Post">
  <id property="id" column="id"/>
  <result property="subject" column="subject"/>
  <result property="body" column="body"/>
</resultMap>

<!-- //////////////////////////////// -->

<!-- select -->
<select id="selectBlog" resultMap="blogResult">
  select
  B.id as blog_id,
  B.title as blog_title,
  B.author_id as blog_author_id,
  P.id as post_id,
  P.subject as post_subject,
  P.body as post_body,
  from Blog B
  left outer join Post P on B.id = P.blog_id
  where B.id = #{id}
</select>
```

#### discriminator 鉴别器

像 Java 语言中的 switch 语句， 以demo说明

有一个机动车父类，它有两个子类： Car 和 Bus

ORM 映射有一个原则：对象模型细粒度，关系模型粗粒度。所以我们将所有的车都存储一张表里（t_vehicle），通过鉴别字段 vType 来区分车的类型（ "c" 代表 Car ， "b" 代表 Bus ）

```java
public class Vehicle {
	//主键id
	private String id;
	//车的名字
	private String name;
	
}

public class Car extends Vehicle {
	//车门的数量
	private int carDoor;
	//车的牌子
	private String band;
	
}

public class Bus extends Vehicle {
	//公共汽车的容量
	private int capacity;
	
}

// mapper接口
public interface VehicleMapper {
	//根据id查询机动车
	Vehicle getVechicleById(String id);
	//根据名字查询小汽车
	Car getCarByName(String name);
}
```

映射配置

```xml
<?xml version="1.0" encoding="UTF-8" ?>  
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd"> 
<mapper namespace="com.tgb.mybatis.data.VehicleMapper">
	<select id="getVechicleById" resultMap="vehicleMap">
		SELECT * FROM TB_VEHICLE WHERE VID = #{id}
	</select>
	<select id="getCarByName" resultMap="vehicleMap">
		SELECT * FROM TB_VEHICLE WHERE VTYPE='c' AND VName = #{id}
	</select>
   <resultMap type="vehicle" id="vehicleMap">
	   <id property="id" column="vId"/>
	   <result property="name" column="vName"/>
       <!-- 根据鉴别字段的值自动映射成对应的子类 -->
	   <discriminator javaType="string" column="vType"><!-- vType是表的某个字段 -->
		   <case value="c" resultType="car">
			   <result property="carDoor" column="cardoor"/>
			   <result property="band" column="band"/>
		   </case>
		   <case value="b" resultType="bus">
			   <result property="capacity" column="capacity"/>
		   </case>
	   </discriminator>
   </resultMap>
</mapper>
```

### 自动映射

```xml
<!--  id 和 userName列将被自动映射， hashed_password 列将根据配置映射 -->
<select id="selectUsers" resultMap="userResultMap">
  select
    user_id             as "id",
    user_name           as "userName",
    hashed_password
  from some_table
  where id = #{id}
</select>

<!-- autoMapping="false" 打开/关闭自动映射 -->
<resultMap id="userResultMap" type="User" autoMapping="false">
  <result property="password" column="hashed_password"/>
</resultMap>

```

# 缓存

references: http://www.mybatis.org/mybatis-3/zh/sqlmap-xml.html#cache， https://tech.meituan.com/mybatis_cache.html

查询顺序: 二级缓存 ———> 一级缓存——> 数据库

<img src="Snipaste_2018-05-20_14-30-20.png" width="70%">

一级缓存指的是sqlsession级别。一级缓存内部含有一个数据模型（HashMap），map的key是通过sql语句、statement的id以及条件组成的。Value就是查询的结果集。

二级缓存指的是mapper级别，即同一个namespace下的mapper文件(二级缓存是多个SqlSession共享的，其作用域是mapper的同一个namespace)，强调不同session之间缓存数据共享。二级缓存内部含有一个数据模型（HashMap），map的key是通过sql语句、statement的id以及条件组成的。Value就是查询的结果集。


二级缓存中会有多个一级缓存，不同的一级缓存互相不影响。

与Spring整合后，一级缓存会失效; 一级缓存默认是开启的, 二级缓存默认是关闭的

## 一级缓存

### 怎么开启一级缓存

怎么开启一级缓存

只需在MyBatis的配置文件中，添加如下语句

```xml
<!-- 共有两个选项，SESSION或者STATEMENT，默认是SESSION级别，即在一个MyBatis会话中执行的所有语句，都会共享这一个缓存。一种是STATEMENT级别，可以理解为缓存只对当前执行的这一个Statement有效 -->

<setting name="localCacheScope" value="SESSION"/>

```

### 一级缓存分析

又叫 local stage , 本地缓存会被增删改、提交事务、关闭事务以及关闭 session 所清空

组件结构：

![alt](Snipaste_2018-11-03_11-43-36.png)

同一个 sqlSession 效果示意图：

<img src="Snipaste_2018-05-20_14-35-19.png" width="70%">

- 第一次真正查询了数据库，后续的查询使用了一级缓存
- 通过同一个 sqlSession 修改操作后执行相同查询，直接查询了数据库，此时一级缓存失效

开启两个 sqlSession 示意图：

![alt](Snipaste_2018-11-03_12-05-22.png)

- 查询和修改操作在不同的 sqlSession 会读取到脏数据。这证明 一级缓存只在数据库会话（sqlSession）内部共享

总结：

1. MyBatis一级缓存的生命周期和SqlSession一致。
1. MyBatis一级缓存内部设计简单，只是一个没有容量限定的HashMap，在缓存的功能性上有所欠缺。
1. MyBatis的一级缓存最大范围是SqlSession内部，有多个SqlSession或者分布式的环境下，数据库写操作会引起脏数据，建议设定缓存级别为`Statement`。


## 二级缓存

### 怎么开启二级缓存

一级缓存默认开启， spring整合后一级失效， 生效的只有二级缓存

要开启二级缓存,先开启总开关

```xml
<setting name="cacheEnabled" value="true"/>
```

然后开启mapper级别的开关:

```xml
<mapper>
    <!-- 声明这个namespace使用二级缓存 -->
    <cache/>

    <!-- 如果需要自定义缓存配置 -->
    <!-- 
    缓存策略:
        LRU – 最近最少使用的:移除最长时间不被使用的对象。(默认)
        FIFO – 先进先出:按对象进入缓存的顺序来移除它们。
        SOFT – 软引用:移除基于垃圾回收器状态和软引用规则的对象。
        WEAK – 弱引用:更积极地移除基于垃圾收集器状态和弱引用规则的对象。
    -->
    <cache type=""     // cache使用的类型，默认是PerpetualCache, 可自定义
    eviction="FIFO"    //回收的策略
    flushInterval="60000" // 配置一定时间自动刷新缓存，单位是毫秒; 默认情况是unset,也就是没有刷新间隔,缓存仅仅调用更新语句时刷新。
    size="512"        //最多缓存对象的个数。
    readOnly="true"/> //是否只读，若配置可读写，则需要对应的实体类能够序列化; 默认false(可读写), 是一份拷贝 这会慢一些,但是安全 -->

    <!-- cache-ref代表引用别的命名空间的Cache配置，两个命名空间的操作使用的是同一个Cache -->
    <cache-ref namespace="mapper.StudentMapper"/>

</mapper>
```

最后, 二级缓存需要查询结果映射的pojo对象实现`java.io.Serializable`接口实现序列化和反序列化操作

如果希望临时禁用二级缓存

```xml
<select userCache="false"></select><!-- 默认是true -->
```

### 二级缓存分析

一级缓存中，其最大的共享范围就是一个SqlSession内部，如果多个SqlSession之间需要共享缓存，则需要使用到二级缓存

组件示意图:

![alt](Snipaste_2018-11-03_12-27-08.png)

- 开启二级缓存后，会使用CachingExecutor装饰Executor
- 进入一级缓存的查询流程前，先在CachingExecutor进行二级缓存的查询
- 同一个namespace下的所有操作语句，都影响着同一个Cache，即二级缓存被多个SqlSession共享
- 查询执行的流程就是 二级缓存 -> 一级缓存 -> 数据库

二级缓存生效示意图: [check here](https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=%E6%9C%AA%E5%91%BD%E5%90%8D%E8%A1%A8%E5%8D%95.xml#R3VjbjuMoEP0apJmHadnG10fn0rv7MNJImdXOPBKbOKixyWLSSebrF2x8wSQz2Wx6W91RS40LqqBOUYcCAOfl8TeOdtvPLMcUeE5%2BBHABPM91Qk%2F%2BU5JTKwncsBUUnOR60CBYkR%2B409TSPclxbQwUjFFBdqYwY1WFM2HIEOfsYA7bMGrOukMFtgSrDFFb%2BhfJxbaVxl40yH%2FHpNh2M7th0vasUfZUcLav9HzAg5vm13aXqLOlHa23KGeHkQguAZxzxkTbKo9zTBW2HWyt3uOF3n7dHFfiGgUdp2dE99r1%2Bm%2B6wnVNWOWCZQRmc5BAsAxAugBJrBrxHMxS1Uge1V%2Frhjh10DXOY2XeBXB22BKBVzuUqd6D3CxSthUl1d16dswFPl70wO1xkfsNsxILfpJDtELY7ZlTFwv9fRgC53Zwb8dBg1qI9GYpetsDYLKhMTuPH%2FwJft6bxA%2FCa%2FHz7oBft94RgCXa7TBvsEtBGjWQxSCdKUmSgtgHS191SRyHMRbQrw2r75uwJlei2uv9J1TttLYAyVhJMu1vLTh76plOejXDVZ4qHpWfa8qyp69bUpkYbVglHlFJqHJw3hjznBWqavnv80oPWOn5hknmjDLerAA6zU%2F2ZHv%2B3IemXSjOLYKeIC9PBMQLLEY8ZgdjhHZwBuxOxjFFgjybM54LgJ7hCyNyLX2svSkFxZMY1mzPM6y1xmw8MeQ7E0PRxFDrsmVIhgqdRsN2akB9ecEwNOfxHOOUkI3W4rDZekyv239naFHgnQxvuiFV%2FmeN%2Bez0R%2F7B%2FWhtS5lxwtxoHNfkB1o3A9Ru0c7J0cEMBAspQXvB6raGUAqIkkJu1gXFG2VKpTGRR3uqxYKpbK9l8pOq%2BKo%2BFp%2F8%2ByR9n%2BVd%2FIIzWQ%2FPbMS7UKn%2F66SX5cZONTcUH3V%2Bj1M9o0ieXNkk0Qmlo6zV5YzSkwv8pgY9ODLgWvBdRenB9%2FxO8AVzIn3BXIcPH4n41gxywkh%2Ff2%2BtuIH%2Bnupczwltphm1jUETF%2BL3%2F%2FCEddS6N%2FJEOM3f8DqeuCWVwzO7KqSiTaFKtoumLdNbup2qEAwF5IM8ZEoiPnzsVORsY633lf2hE5hRCXw7%2B6OXyv7o7R75PZN8kqTgmkwSxsklJrmxUnhVCnAnJwS8tVQI4omh5IVKhUkd69%2B7VIjPlgqw5ZL3XS0E0Ayhf4YvXqxaSCzc87VdL1gsMYK6KybKY6FegR42lB2yLeLiIUcCrVGNL9y07oBdlEQGdrFdaEXQRs4qqG%2B689t31rdHtY5Js8GdSTZ5TZKN4C%2BuUdeSbOJNDF1ZZ%2F1bko0mZN5dIO9FstB%2BDyhPawlxfdVbinp48dVb1TIESfMaIyVpoyglsQ%2Fix%2BY9awZmy%2FdF0VFoxKW%2F2L5ARSc%2Fh2ffNsbD2zpc%2FgM%3D)

![alt](Snipaste_2018-11-03_12-46-56.png)

update操做会更新二级缓存 [check here](https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=%E6%9C%AA%E5%91%BD%E5%90%8D%E8%A1%A8%E5%8D%95.xml#R5Vptb6M4EP41SN0PWwE2bx%2BTNL076VZaKXu6vY8OOAlXgrPgNMn%2B%2BhuDCWCTlqak3fSqSrUHv%2BBnZp4ZDzXQZL3%2FLSOb1RcW0cSwzWhvoDvDti3TteGPkBxKiWtZpWCZxZEcVAtm8U9azZTSbRzRvDWQM5bweNMWhixNachbMpJlbNcetmBJe9cNWVJNMAtJokv%2FjiO%2BKqW%2B7dXy32m8XFU7W25QPpmT8GGZsW0q9zNstCh%2BysdrUq0lD5qvSMR2DRGaGmiSMcbL1no%2FoYnAtoKtnHd%2F4unxvTOa8j4TpJ4eSbKVR89%2FJDOa5zFLLWPqGeOJESBj6hijOyPwRcOfGOORaAT34rc8Bj9U0BWHp2J5y0Dj3SrmdLYhoXi6A2MB2YqvE%2FlY7k4zTvcnT2AdcQF7o2xNeXaAIfvKtCSUh0oXsr%2BrFWdVcK%2BaSkNSSKSxLI9r14BBQ2LWjR82NQBoBKYkuyzjK7ZkKUmmtXRcQ2S24aD7mH9vtP8RQ249R3RTeLPvRd8MgkpQDgi8qv%2BVZjGcgWZy7X8p5wfpXmTLGYjqV%2FqTsU1LD%2BLNn9YCHJRts1COQtIzSbakcpQddCsrownh8WN7%2BdcAj54wXPsqDReZfQ3XHsBwq%2FdtALgmmw1YjsBuZIy8AjLfGI2FJBgZPjamWDwCHOsxGtDvDSvGbViDnqhibwhUdT7VAAnZOg7leXOesYdjiLELN45GIoBBd56w8OHbKk7bGC1Yyu%2FJOk7EASfFYrY5I2kOf77M5ICKgOpNJixhWfEGYGjiB56E2%2BzxqJr%2BHNBy%2BG5lNNB2OsCuZL1pQe7wlcXwLkdd2yr3%2B4oOS7aSs5phUFkIm8pCqjGUR9YWAlWRQ2PYRgzIT78wctv72GYrPEOjXLE2tiOm%2FeyvgxY53YB6R4s4jf7KaTY%2B%2FBHdWJ80swSP421Dy2ge%2FyTzYoCwFnk4GO2MDecOJCKk5GV0ERNIEi%2FBWO8SuhBLCTeOIacaSTEX4Wacg%2FPH6fJbEXs%2B42Gc%2Fujllf6cDq9HHYY4CJXi550e8ryNaC4Supf%2B3XT1MCEQuULF0eMkaXitzCMbqYB1a4LCW6kAtvGpXKBKLiCBcL1GggGrWI7sq3POywvsDpo4ob%2B34Qkt1Fpn8oSr%2Bq%2FbjyfOcWW3w6rchJculEJ7WbTBveHYI6GCOnO%2FhSCzjvnNp2oK7Nac9bG83zWdtlYcrHu%2Fdynv96435B%2BZ5DOQgtVmEtcPTjFJX1Z4L3e3Aqvt7upNr3daoOSSCKPLpAVYeWFn4LTA70wLUMkbHzszcE9Q%2F5tkBoGGezTXcwONERpQV4nDer8UpbbbRcJ24Ypk%2FDYinMxJTk%2FcqgbAzlfCpqvTqoc6kFOj61n3e%2F1%2Ben20arYp1XktofartbwNyXromStTX5INbGWhnjnVS0nW85V9%2FGFJFul3%2F%2FVhDhDnveomosiCRV1q6hpBUXkByaiYCBIfG%2F59UbsaG%2BPpx6Joz23p5XiJfYvsDT1VR0RXWUfEnqfjd7E6Iupx%2Bb1Gnj6Pkktz%2BlXKYY55bt6rlMNwcBlKVsthjj0wJTu6d0Pei8u8d7uBFIqKzPcGoIbFYHP3x1Z8egP6jOrOR0uJsf88YbiXItyussa18EXn5zn7ZXTR%2BoD2rnShZnDaF5C%2BdOEr12RbLcMPRBe%2BksGhoTO4HqWcSxVyTaWQ65lew%2BJM3eJeeZHQzfDXKti2K3pYpZ6%2BpukoEQa7Xi%2FTPMd6uoss%2BFiczWVW2SjOfqSw4ijfYDAO3jCs6JWW6wkrzSqsrVRhPX%2FYosG7Rhwr6CbwF7s1UiPBpb7XKpEND%2Fy9FutVLsEZ7v%2BhMFtNqVTYUV0cqjAL3fpf6ko11f%2B3iKb%2FAQ%3D%3D)

![alt](Snipaste_2018-11-03_13-02-02.png)

验证MyBatis的二级缓存不适应用于映射文件中存在多表查询的情况:

```java
@Test
public void testCacheWithDiffererntNamespace() throws Exception {
        SqlSession sqlSession1 = factory.openSession(true); 
        SqlSession sqlSession2 = factory.openSession(true); 
        SqlSession sqlSession3 = factory.openSession(true); 

        StudentMapper studentMapper = sqlSession1.getMapper(StudentMapper.class);
        StudentMapper studentMapper2 = sqlSession2.getMapper(StudentMapper.class);
        ClassMapper classMapper = sqlSession3.getMapper(ClassMapper.class);

        System.out.println("studentMapper读取数据: " + studentMapper.getStudentByIdWithClassInfo(1));
        // 带 class 信息的 User 被缓存
        //
        sqlSession1.close();

        System.out.println("studentMapper2读取数据: " + studentMapper2.getStudentByIdWithClassInfo(1));// 二级缓存命中

        //修改 class 信息,
        classMapper.updateClassName("特色一班",1);
        // 不同的 mapper, namespace 也不同, 所以commit()后, StudentMapper 下的 cache感应不到 classMapper 下的变动, 没有更新缓存
        //
        sqlSession3.commit();

        System.out.println("studentMapper2读取数据: " + studentMapper2.getStudentByIdWithClassInfo(1));// 读取到的user 中的class 信息还是旧的, 造成脏读
}

```

怎么解决多表查询中的脏读呢? - 可以使用Cache ref，让ClassMapper引用StudenMapper命名空间，这样两个映射文件对应的Sql操作都使用的是同一块缓存了

不过这样做的后果是，缓存的粒度变粗了，多个Mapper namespace下的所有操作都会对缓存使用造成影响。

总结:

1. MyBatis的二级缓存相对于一级缓存来说，实现了SqlSession之间缓存数据的共享，同时粒度更加的细，能够到namespace级别，通过Cache接口实现类不同的组合，对Cache的可控性也更强。
1. MyBatis在多表查询时，极大可能会出现脏数据，有设计上的缺陷，安全使用二级缓存的条件比较苛刻。
1. 在分布式环境下，由于默认的MyBatis Cache实现都是基于本地的，分布式环境下必然会出现读取到脏数据，需要使用集中式缓存将MyBatis的Cache接口实现，有一定的开发成本，直接使用Redis,Memcached等分布式缓存可能成本更低，安全性也更高。

## 使用自定义缓存

### 自定义缓存

Mybatis的二级缓存，不支持分布式。Mybatis本身是持久层框架，它不擅长缓存

Mybatis中有一个cache接口，实现二级缓存的话，需要实现该接口

```xml
<cache type="com.domain.something.MyCustomCache"/><!-- 这个类必须实现org.mybatis.cache.Cache接口 -->

<!-- 而且是通过 cache 元素来传递属性, 比如, 下面代码会在你的缓存实现中调用一个称为 “setCacheFile(String file)” 的方法 -->
<!-- 支持钩子函数 MyCustomCache 实现 org.apache.ibatis.builder.InitializingObject 接口, 所有属性设置完毕以后可以调用接口内的初始化方法 -->
<cache type="com.domain.something.MyCustomCache">
  <property name="cacheFile" value="/tmp/my-custom-cache.tmp"/>
</cache>
```

### 第三方缓存-整合ehcache

添加依赖: ehcache-core, mybatis-ehcache

配置mapper标签

```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache" flashInterval="120000"/><!-- 刷新间隔120s --> 
```

在config包下，创建ehcache.xml文件

## 应用场景

对于访问响应速度要求高，但是实时性不高的查询，可以采用二级缓存技术。

## 局限性

Mybatis二级缓存对细粒度的数据级别的缓存实现不好。

场景：对商品信息进行缓存，由于商品信息查询访问量巨大，但是要求用户每次查询都是最新的商品信息，此时如果使用二级缓存，就无法实现当一个商品发生变化只刷新该商品的缓存信息而不刷新其他商品缓存信息，因为二级缓存是mapper级别的，当一个商品的信息发送更新，所有的商品信息缓存数据都会清空。

解决此类问题，需要在业务层根据需要对数据有针对性的缓存。比如可以`对经常变化的 数据操作单独放到另一个namespace的mapper中`。


# 延迟加载

在mybatis中通过resultMap中的association和collection标签，提供类延迟加载的功能

延迟加载即是先加载主信息，需要的时候，再去加载从信息

在mybatis中延迟加载默认是不启用的

```xml
<settings>
	<!-- 开启全局性设置懒加载 -->
	<setting name="lazyLoadingEnabled" value="true"/>
	<!-- 开启按需加载 -->
	<setting name="aggressiveLazyLoading" value="false"/>
</settings>
```

# 整合到spring

引入 mybatis-spring

1、	将数据源信息及事务交由spring进行管理
2、	将SqlSessionFactory交由spring进行单例管理。
3、	将原始dao的实现类交由spring管理
4、	将mapper代理对象交由spring来代理生成。

# 逆向工程

https://github.com/mybatis/generator

常见问题:
Mapper.xml文件已经存在时，如果进行重新生成则mapper.xml文件时，内容不被覆盖而是进行内容追加，结果导致mybatis解析失败。
解决方法：删除原来已经生成的mapper xml文件再进行生成。
Mybatis自动生成的po及mapper.java文件不是内容而是直接覆盖没有此问题


```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
	<context id="testTables" targetRuntime="MyBatis3">
		<commentGenerator>
			<!-- 是否去除自动生成的注释 true：是 ： false:否 -->
			<property name="suppressAllComments" value="true" />
		</commentGenerator>
		<!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
		<jdbcConnection driverClass="com.mysql.jdbc.Driver"
			connectionURL="jdbc:mysql://localhost:3306/mybatis" userId="root"
			password="root">
		</jdbcConnection>

		<!-- <jdbcConnection driverClass="oracle.jdbc.OracleDriver" connectionURL="jdbc:oracle:thin:@127.0.0.1:1521:yycg" 
			userId="yycg" password="yycg"> </jdbcConnection> -->

		<!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 
			和 NUMERIC 类型解析为java.math.BigDecimal -->
		<javaTypeResolver>
			<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>

		<!-- targetProject:生成PO类的位置 -->
		<javaModelGenerator targetPackage="cn.itcast.ms.po"
			targetProject=".\src">
			<!-- enableSubPackages:是否让schema作为包的后缀 -->
			<property name="enableSubPackages" value="false" />
			<!-- 从数据库返回的值被清理前后的空格 -->
			<property name="trimStrings" value="true" />
		</javaModelGenerator>
		<!-- targetProject:mapper映射文件生成的位置 -->
		<sqlMapGenerator targetPackage="cn.itcast.ms.mapper"
			targetProject=".\src">
			<!-- enableSubPackages:是否让schema作为包的后缀 -->
			<property name="enableSubPackages" value="false" />
		</sqlMapGenerator>
		<!-- targetPackage：mapper接口生成的位置 -->
		<javaClientGenerator type="XMLMAPPER"
			targetPackage="cn.itcast.ms.mapper" targetProject=".\src">
			<!-- enableSubPackages:是否让schema作为包的后缀 -->
			<property name="enableSubPackages" value="false" />
		</javaClientGenerator>
		<!-- 指定数据库表 -->
		<table tableName="user"></table>
		<table tableName="orders"></table>
		<table tableName="orderdetail"></table>
		<table tableName="items"></table>
	</context>
</generatorConfiguration>


```


# 拾遗

## #和$区别

`#{}`和`${}`区别

* `#{ }` 解析为一个 JDBC 预编译语句（prepared statement）的参数标记符 (一个 #{ } 被解析为一个参数占位符 ?)

* `${ }` 仅仅为一个纯碎的 string 替换，在动态 SQL 解析阶段将会进行变量替换, 有注入风险

* `${ }` 的变量的替换阶段是在动态 SQL 解析阶段，而 #{ }的变量的替换是在 DBMS 中

* tips

    * 能使用 #{ } 的地方就用 #{ } (首先这是为了性能考虑的，相同的预编译 sql 可以重复利用, 其次，${ } 在预编译之前已经被变量替换了，这会存在 sql 注入问题)
    
    * 表名作为变量时, 或者字段名作为变量名时，必须使用 ${ }, (表名是字符串, 字段名是字符串，使用 sql 占位符替换字符串时会带上单引号 ''，这会导致 sql 语法错误)

```xml
<!-- 此时, 下面两者效果相同 -->
<!-- 但是在在动态 SQL 解析阶段， #{ } 和 ${ } 会有不同的表现 -->
select * from user where name = #{name} 会被解析为 select * from user where name = ?
select * from user where name = '${name}' 当我们传递的参数为 "ruhua" 时，sql 的解析为 select * from user where name = "ruhua"
```


## parameterType和resultType

* parameterType是指定输入参数的java类型
* resultType是指定输出结果的java类型（单条结果）


## resultMap和resultType

* 使用resultType映射，数据库中sql查询出多少条，就会映射成多少个对象，而且数据库中查询出的字段多少个，对象中就要有多少个属性对应。(resultType映射是一种平铺式的映射)

* 使用resultMap进行映射，它是可以对象嵌套对象这种方式进行映射

* resultMap的association和collection标签具有延迟加载的功能，而resultType不具有

* tips:

    * 如果完成一对一映射，使用resultMap和resultType都 可以

    * 使用resultType映射相对来说更简单，一般如果是一对一映射，则推荐使用resultType进行映射


