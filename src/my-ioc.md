---
title: 造个小轮子 My IOC
tags:
  - ioc
  - side project
date: 2018-05-10 10:22:22
categories: java web
---

<div align="center">
动手实现一个ioc容器
(https://github.com/xiaoyureed/toy-ioc)
</div>

<!--more-->

# 代码

定义容器接口规范, 有注册, 获取等几个方法

```java
/**
 * container 接口(添加/移除/获取)
 *
 * @version 0.1
 * @author xy
 * @date 2018年6月5日 下午5:00:34
 */
public interface Container {

    /**
     * 初始化
     * @param clazzSet 
     */
    void register(String packageName);
    
    /**
     * 貌似暂时用不到
     */
//    void remove();
    
    /**
     * 根据 class 获取 bean
     * @param clazz
     * @return
     */
    <T> T getByType(Class<T> clazz);
    
    /**
     * 
     * @param name
     * @return
     */
    <T> T getByName(String name);
}

```

提供一个默认实现

```java
/**
 * default container
 *
 * @version 0.1
 * @author xy
 * @date 2018年6月5日 下午11:20:02
 */
public class DefaultContainer implements Container {
    
    private static final Logger log = LoggerFactory.getLogger(DefaultContainer.class);
    
    /**
     * store beans
     * 
     * key -- value
     * beanSimpleName -- bean
     */
    Map<String, Object> beanMap = new ConcurrentHashMap<String, Object>();
    
    /**
     * store dependencies, key: 注入关系, value: bean's name(beansMap的key)
     * 
     * key--value
     * beanSimpleName.fieldName--beanSimpleName
     */
    Map<String, String> relationMap = new ConcurrentHashMap<String, String>();
    
    /////////////// 单例 start //////////////////////////
        
    private DefaultContainer() {
    }
    private static class DefaultContainerHolder {
    private static final DefaultContainer instance = new DefaultContainer();
    }
    public static DefaultContainer me() {
    return DefaultContainer.DefaultContainerHolder.instance;
    }
    //////////////////end//////////////////////
    

    @SuppressWarnings("unchecked")
    public <T> T getByType(Class<T> clazz) {
        Bean beanAnno = clazz.getAnnotation(Bean.class);
        if (beanAnno != null
                && StringUtil.isValid(beanAnno.value())) {// use specified beanName
            String specifiedName = beanAnno.value();
            log.debug("class [{}]指定了有效beanName[{}]", clazz.getName(), specifiedName);
            return (T) this.beanMap.get(specifiedName);
        }
        else {// use beanSimpleName
            String simpleName = clazz.getSimpleName();
            log.debug("class [{}]没指定有效的beanName, 使用默认值(simple bean name)", clazz.getName());
            return (T) this.beanMap.get(simpleName);
        }
    }
    
    @SuppressWarnings("unchecked")
    public <T> T getByName(String name) {
        return (T) this.beanMap.get(name);
    }

    public void register(String packageName) {
        
        Set<Class<?>> clazzSet = scanPackage(packageName);  
        
        init(clazzSet);
        
        inject();
    }

    /**
     * 注入
     */
    private void inject() {
        Iterator<Entry<String, String>> ite = relationMap.entrySet().iterator();
        while(ite.hasNext()) {
            Entry<String, String> next = ite.next();
            String value = next.getValue();
            String key = next.getKey();
            String[] split = key.split("\\.");
            try {
                PropertyUtils.setProperty(this.beanMap.get(split[0]), split[1], this.beanMap.get(value));
            } catch (IllegalAccessException e) {
                e.printStackTrace();
            } catch (InvocationTargetException e) {
                e.printStackTrace();
            } catch (NoSuchMethodException e) {
                e.printStackTrace();
            }
        }
    }


    /**
     * 创建bean实例, 创建注入关系
     * (也就是填充两个map)
     * @param clazzSet
     */
    private void init(Set<Class<?>> clazzSet) {
        
        Iterator<Class<?>> ite = clazzSet.iterator();
        while(ite.hasNext()) {
            Class<?> clazz = ite.next();
            Object bean = null;
            try {
                bean = ReflectUtil.createInstance(clazz);
            } catch (Exception e) {
                log.error("创建bean[{}]出现问题", clazz.getName(), e);
            }
            
            String specName = clazz.getAnnotation(Bean.class).value();
            if (StringUtil.isValid(specName)) {
                this.beanMap.put(specName, bean);
            }
            // 没有指定beanname, 使用bean 的 qualifiedName 注册
            else {
                this.beanMap.put(clazz.getName(), bean);
            }
            
            // 初始化 relationMap
            Field[] fields = clazz.getDeclaredFields();
            for (Field f: fields) {
                Inject injectAnno = f.getAnnotation(Inject.class);
                if (injectAnno != null) {
                    // 指定了注入的name
                    if (StringUtil.isValid(injectAnno.value())) {
//                        this.relationMap.put(clazz.getName().concat(".").concat(f.getName()), injectAnno.value());
                        this.relationMap.put(specName.concat(".").concat(f.getName()), injectAnno.value());
                    }
                    // 没有指定injection name
                    else {
                        
//                        this.relationMap.put(clazz.getName() + "." + f.getName(), f.getName());
                        this.relationMap.put(specName + "." + f.getName(), f.getName());
                    }
                }
            }
            
        }
        
        
    }
    /**
     * 扫描所有class
     * @param packageName
     * @return
     */
    private Set<Class<?>> scanPackage(String packageName) {
        Set<Class<?>> result = new LinkedHashSet<Class<?>>();
        
        ClassLoader classLoader = Thread.currentThread().getContextClassLoader();
        // classpath 下所有的资源文件
        Enumeration<URL> urls = null;
        try {
            urls = classLoader.getResources(packageName.replace(".", "/"));
        } catch (IOException e) {
            e.printStackTrace();
        }
        
        while (urls.hasMoreElements()) {
            URL url = urls.nextElement();
            String protocol = url.getProtocol();
            
            // 文本中的 class
            if ("file".equals(protocol)) {
                try {
                    File packageFile = new File(URLDecoder.decode(url.getFile(), "UTF-8"));
                    
                    if (!packageFile.exists()
                            || !packageFile.isDirectory()) {
                        log.error("package: [{}]无效", url.getFile());
                    }
                    
                    File[] files = packageFile.listFiles();
                    
                    handleFiles(files, result, classLoader, packageName);
                    
                } catch (UnsupportedEncodingException e) {
                    e.printStackTrace();
                }
            }
            // jar 中的 class
            else if ("jar".equals(protocol)) {
                JarURLConnection jarConn;
                try {
                    jarConn = (JarURLConnection) url.openConnection();
                    JarFile jarFile = jarConn.getJarFile();
                    Enumeration<JarEntry> jarEntry = jarFile.entries();
                    // TODO
//                    handleJarFile();
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                
            }
            else {
                log.error("获取package下资源的protocal出错.");
            }
        }
     
        
        return result;
    }
    
    private void handleFiles(File[] files, Set<Class<?>> result, ClassLoader classLoader, String packageName) {
        for (File f: files) {
            // it's a folder, 递归
            if (f.isDirectory()) {
                handleFiles(f.listFiles(), result, classLoader, packageName);
            }
            // 是文件
            else {
                try {
                    Class<?> clazz = classLoader.loadClass(packageName + "." + f.getName().substring(0, f.getName().lastIndexOf(".")));
                    // 被 @Bean 注解才扫描
                    Bean beanAnno = clazz.getAnnotation(Bean.class);
                    if (beanAnno != null) {
                        /*// specified name
                        if (StringUtil.isValid(beanAnno.value()) {
                            
                        }
                        // 默认, 使用 bean 的 SimpleName 
                        else {
                            
                        }*/
                        result.add(clazz);
                    }
                } catch (ClassNotFoundException e) {
                    log.error("扫描文件[{}]发生错误", f.getPath(), e);
//                    e.printStackTrace();
                }
            }
        }
        
    }
    
}
```

当然, 还需要最少两个注解. 首先是@bean, 标识纳入container

```java
/**
 * put into the Ioc container
 *
 * @version 0.1
 * @author xy
 * @date 2018年6月5日 下午7:48:17
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE})
public @interface Bean {

    String value() default "";
}
```

然后是@inject, 依赖注入时候用:

```java

/**
 * inject resources into 
 *
 * @version 0.1
 * @author xy
 * @date 2018年6月5日 下午7:53:21
 */
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.FIELD})
public @interface Inject {

    String value();
}

```

# 测试

先定义bean

```java
@Bean("son")
public class Son {

}

@Bean("mama")
@Data// 需要引入lombok
public class Mama {

    @Inject("son")
    private Son son;
    
    // lombok 提供 getter
}
```

使用:

```java
public class DefaultContainerTest {

    @Test
    public void testDefaultContainer() {
        DefaultContainer container = DefaultContainer.me();
        container.register("com.xiaoyu.ioc.bean");
        Object oMama = container.getByName("mama");
        System.out.println(oMama);
    }
}
```