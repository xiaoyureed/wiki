---
title: Task Schedule
tags:
  - schedule
  - task
date: 2016-05-01 22:38:07
categories: java web
---


<div align="center">
任务调度系统 总结
https://www.ibm.com/developerworks/cn/java/j-lo-taskschedule/index.html
</div>

<!--more-->

<!-- TOC -->

- [分类](#分类)
- [cronExpression](#cronexpression)
- [用法示例](#用法示例)
  - [java.util.Timer](#javautiltimer)
  - [Quartz](#quartz)
    - [准备工作](#准备工作)
    - [task继承自特定基类](#task继承自特定基类)
    - [task无需继承基类](#task无需继承基类)
  - [spring task](#spring-task)
- [Quartz](#quartz-1)
  - [是什么](#是什么)
  - [quick start](#quick-start)
  - [有用的注解](#有用的注解)
  - [trigger](#trigger)
    - [quartz Calendars](#quartz-calendars)
    - [SimpleTrigger](#simpletrigger)
    - [CronTrigger](#crontrigger)
  - [TriggerListeners and JobListeners](#triggerlisteners-and-joblisteners)
  - [SchedulerListeners](#schedulerlisteners)
  - [Job Stores](#job-stores)
- [springboot和quartz实现动态配置](#springboot和quartz实现动态配置)

<!-- /TOC -->

# 分类

为了理解方便, 做一下分类, 根据实现技术分类:

1. Java自带的java.util.Timer类，这个类允许你调度一个java.util.TimerTask任务。不能指定时间，只能指定频率。

2. 使用Quartz，这是一个功能比较强大的的调度器，可以指定时间、频率。

3. Spring3.0以后自带的task，可以将它看成一个轻量级的Quartz，而且使用简单。

根据任务类继承方式分类:

1. 任务类需要继承自特定的基类

  比如：
  Quartz中需要继承自`org.springframework.scheduling.quartz.QuartzJobBean`；java.util.Timer中需要继承自`java.util.TimerTask`。

2. 不需要继承, 任务类就是普通类

指定时间 or 指定频率 :

1. 每隔指定时间则触发一次，在Quartz中对应的触发器为：`org.springframework.scheduling.quartz.SimpleTriggerBean`

2. 每到指定时间则触发一次，在Quartz中对应的调度器为：`org.springframework.scheduling.quartz.CronTriggerBean`

> 注：
> java.util.TimerTask任务就只能使用第一种触发条件。
Quartz和spring task都可以支持这两种触发条件。

# cronExpression

https://cron.qqe2.com/ 在线生成 cron

cron 表达式

格式: `{秒数} {分钟} {小时} {日期} {月份} {星期} {年份(可为空)}`

各个占位符解释:

![alt](1.png)

- "*" 用来指所有值, 比如: "*"在"分钟"的字段里表示"没分钟"

- "-" 指定一个范围, 如: "10-12"在"小时"字段表示"10点, 11点, 12点"

- "," 分隔多个值, 如: "MON,WED,FRI" 在星期里表示"周一,三,五"

- "?" 只在 "日期", "星期"字段使用, 指"不明确的值"

- "L" 指定在"月"or"星期"的最后一天(last缩写)

- "W" 离指定日期最近的星期日, 只能在"月份"中使用

- "#" 只是用在星期字段,

- "/" 如: 1/20: 从第一单位开始, 每隔20个单位, 就触发一次; 10-45/20 : 在第10到45单位内，每隔20单位触发一次，超过第45单位，则无效了


看看几个 cron 表达式 demo:

```
CRON表达式                  含义 
"0 0 12 * * ?"          每天中午十二点触发 
"0 15 10 ? * *"         每天早上10：15触发 
"0 15 10 * * ?"         每天早上10：15触发 
"0 15 10 * * ? *"       每天早上10：15触发 
"0 15 10 * * ? 2005"    2005年的每天早上10：15触发 
"0 * 14 * * ?"          每天从下午2点开始到2点59分每分钟一次触发
"0 0/5 * * * ?"         每5min触发一次
“10 0/5 * * * ?”        每5min一次触发, 在每分钟的第 10s 开始触发
"0 0/5 14 * * ?"        每天从下午2点开始到2：55分结束每5分钟一次触发 
"0 0/5 14,18 * * ?"     每天的下午2点至2：55和6点至6点55分两个时间段内每5分钟一次触发 
"0 0-5 14 * * ?"        每天14:00至14:05每分钟一次触发 
“0 30 10-13 ? * WED,FRI”    fires at 10:30, 11:30, 12:30, and 13:30, on every Wednesday and Friday.
"0 10,44 14 ? 3 WED"    三月的每周三的14：10和14：44触发 
"0 15 10 ? * MON-FRI"   每个周一、周二、周三、周四、周五的10：15触发
“0 0/30 8-9 5,20 * ?”   每个月的 5号, 20号 的 8点-10点没30min触发一次

```

# 用法示例

## java.util.Timer

`java.util.Timer`
`java.util.TimerTask`

- Timer是一个定时器类，通过该类可以为指定的定时任务进行配置。

- TimerTask类是一个定时任务类，该类实现了Runnable接口，而且是一个抽象类，如下所示：

  public abstract class TimerTask implements Runnable 
  可以通过继承该类，来实现自己的定时任务。


Timer定时器实例有多种构造方法：

* Timer() 创建一个新计时器。

* Timer(boolean isDaemon) 创建一个新计时器，可以指定其相关的线程作为守护程序运行。

* Timer(String name) 创建一个新计时器，其相关的线程具有指定的名称。

* Timer(String name, boolean isDaemon) 创建一个新计时器，其相关的线程具有指定的名称，并且可以指定作为守护程序运行。

Timer 方法:

1. 在特定时间执行任务，只执行一次

  public void schedule(TimerTask task,Date time)

1. 在特定时间之后执行任务，只执行一次

  public void schedule(TimerTask task,long delay)

1. 指定第一次执行的时间，然后按照间隔时间，重复执行

  public void schedule(TimerTask task,Date firstTime,long period)

1. 在特定延迟之后第一次执行，然后按照间隔时间，重复执行

  public void schedule(TimerTask task,long delay,long period)

1. 第一次执行之后，特定频率执行，与3同

  public void scheduleAtFixedRate(TimerTask task,Date firstTime,long period)

1. 在delay毫秒之后第一次执行，后按照特定频率执行

  public void scheduleAtFixedRate(TimerTask task,long delay,long period)

1. Timer注销：timer.cancle();

> 注:
> schedule() & scheduleAtFixedRate()两者的区别：
> <1>schedule()方法更注重保持间隔时间的稳定：保障每隔period时间可调用一次
> <2>scheduleAtFixedRate()方法更注重保持执行频率的稳定：保障多次调用的频率趋近于period时间
如果任务执行时间大于period，会在任务执行之后马上执行下一次任务

demo:

```java
/**
 * task1
 *
 * @version 0.1
 * @author xy
 * @date 2018年7月27日 下午4:33:36
 */
public class DemoTask extends TimerTask {
    
    @Override
    public void run() {
        SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        System.out.println("任务 触发...mmmm, 时间：" + df.format(System.currentTimeMillis()));
    }

}

/**
 * task2 检测是否到饭点了
 *
 * @version 0.1
 * @author xy
 * @date 2018年7月27日 下午5:40:23
 */
public class DemoTask2 extends TimerTask{
    
    private static List<Integer> eatTimes;
    /**
     * 静态初始化
     */
    static {
        initEatTimes();
    }
    
    /**
     * 初始化吃饭时间
     */
    private static void initEatTimes(){
        eatTimes = new ArrayList<Integer>();
        eatTimes.add(8);
        eatTimes.add(12);
        eatTimes.add(18);
    }


    @Override
    public void run() {
        Calendar calendar = Calendar.getInstance();
        int hour = calendar.get(Calendar.HOUR_OF_DAY);
        if(eatTimes.contains(hour)) {
            System.out.println("饿了，吃饭...");
        }

    }

}


/**
 * scheduler
 *
 * @version 0.1
 * @author xy
 * @date 2018年7月27日 下午4:37:15
 */
public class DemoScheduler {
    
    Timer timer = new Timer();
    
    /**
     * 指定时间点执行
     * @param task
     * @param ms
     */
    public void schedule(TimerTask task, long ms) {
        timer.schedule(task, ms);
    }
    
    /**
     * 间隔一段时间执行
     * @param task
     * @param firstTime
     * @param period
     */
    public void schedule (TimerTask task, Date firstTime, long period) {
        timer.schedule(task, firstTime, period);
    }
    
    public static void main(String[] args) {
        
//        SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
//        
//        new DemoScheduler().schedule(new DemoTask(), 1000);
//        System.out.println("main thread: schedule成功调用， time：" + df.format(System.currentTimeMillis()));
        
        new DemoScheduler().schedule(new DemoTask2(), Calendar.getInstance().getTime(), 1000*60*60);
    }
}

```

## Quartz

具体参见：https://blog.csdn.net/defonds/article/details/49496895

### 准备工作

导入相应的spring的包(四个核心包，一个依赖包)与Quartz的包

- 导入Quartz的包

- 导入Spring的

  Spring-context-support包----任务类基类
  Spring-web包--------spring web 开发（发现web.xml，Listener等）
  Spring-tx包

> 注：
> spring版本为3.2  quartz版本为2.2.1  也就是说如果使用了quartz2.2.1 则spring版本需3.1以上  


### task继承自特定基类

task基类: `org.springframework.scheduling.quartz.QuartzJobBean`-----来自 context-support 包

### task无需继承基类

能够支持这一特性, 归功于 `org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean`


## spring task

Spring task ：轻量级的Quartz，而且使用起来很简单，除spring相关的包外不需要额外的包，而且支持注解和配置文件两种，任务类均为POJO

# Quartz

## 是什么

http://www.quartz-scheduler.org/

基于java平台的任务调度库, 支持事务, 集群

核心概念/接口就这几个:

- Scheduler - 调度程序 (the main API for interacting with the scheduler.)

    - Scheduler’s life-cycle: 从创建开始, 到 shutdown() method 结束

    - 提供 add, remove, list Jobs and Triggers, pause trigger 等等

    - scheduler 在使用 start() 启动前, 不会起作用

- Job - 任务类需要实现它

    - 接口只一个方法 public void execute(JobExecutionContext context), 当 trigger 被触发, scheduler’s worker threads 会执行 execute 方法

    - JobExecutionContext 可以获取 scheduler, trigger, JobDetail ...

    - scheduler 每次调用 Job 的 execute(), 都会创建一个新的 Job 实例, 调用结束, 指向这个实例的 references 就被拿掉了, 对象随之被回收, 因此无法在 Job 中设置属性信息, 只有通过 JobDetail 设置 JobDataMap

- JobDetail - 定义一个 Job 的属性, 比如 name, group...

    - 任务类添加完一些属性后成为 JobDetail, 然后和 trigger 一起交给 scheduler

    - JobDataMap - 可以在 JobDetail 中为 Job 设置一些 props 然后再在 execute() 中使用 (另外, 触发器还可以具有与之关联的JobDataMaps)

    ```java
    // define the job and tie it to our DumbJob class
    JobDetail job = newJob(DumbJob.class)
        .withIdentity("myJob", "group1") // name "myJob", group "group1"
        .usingJobData("jobSays", "Hello World!")
        .usingJobData("myFloatValue", 3.141f)
        .build();

    // use JobDataMap inside task class
    public void execute(JobExecutionContext context)
        throws JobExecutionException
    {
        JobKey key = context.getJobDetail().getKey();

        JobDataMap dataMap = context.getJobDetail().getJobDataMap();

        String jobSays = dataMap.getString("jobSays");
        float myFloatValue = dataMap.getFloat("myFloatValue");

        System.err.println("Instance " + key + " of DumbJob says: " + jobSays + ", and val is: " + myFloatValue);
    }
    ```

    或者， 可以通过 setter() 实现 自动注入 (触发器触发时，将加载与其关联的JobDetail（实例定义），并通过Scheduler上配置的JobFactory实例化其引用的作业类。默认的JobFactory只是在作业类上调用newInstance（），然后尝试在类上调用与JobDataMap中的键名匹配的setter方法)

    ```java
    public class DumbJob implements Job {

        String jobSays;
        float myFloatValue;
        ArrayList state;

        public DumbJob() {
        }

        public void execute(JobExecutionContext context)
        throws JobExecutionException
        {
        JobKey key = context.getJobDetail().getKey();

        JobDataMap dataMap = context.getMergedJobDataMap();  // Note the difference from the previous example

        state.add(new Date());

        System.err.println("Instance " + key + " of DumbJob says: " + jobSays + ", and val is: " + myFloatValue);
        }

        public void setJobSays(String jobSays) {
        this.jobSays = jobSays;
        }

        public void setMyFloatValue(float myFloatValue) {
        myFloatValue = myFloatValue;
        }

        public void setState(ArrayList state) {
        state = state;
        }

    }
    
    ```

- Trigger - 定义调度规则 ( a component that defines the schedule upon which a given Job will be executed)

    quartz 预先定义好了很多类型的 trigger, 常用的是 SimpleTrigger (特定时间执行一次 [然后重复 N 次, 每次间隔 T]) and CronTrigger (可以使用更灵活的 cron expression)

- 可以使用 Domain Specific Language (or DSL, also sometimes referred to as a “fluent interface”), 见后面的代码(需要 static import)

    - JobBuilder - used to define/build JobDetail instances, which define instances of Jobs.

    - TriggerBuilder - used to define/build Trigger instances.

## quick start

🔗: xx

有必要提一下, fluent api 的使用需要首先 static import 一些包:

```java
import static org.quartz.JobBuilder.*;
import static org.quartz.SimpleScheduleBuilder.*;
import static org.quartz.CronScheduleBuilder.*;
import static org.quartz.CalendarIntervalScheduleBuilder.*;
import static org.quartz.TriggerBuilder.*;
import static org.quartz.DateBuilder.*;
```

```java
package com.xiaoyu.learn.etc.schedule.quartz.simple.demo;

import org.quartz.JobDetail;
import org.quartz.Scheduler;
import org.quartz.Trigger;
import org.quartz.impl.StdSchedulerFactory;
import static org.quartz.JobBuilder.*;
import static org.quartz.TriggerBuilder.*;
import static org.quartz.SimpleScheduleBuilder.*;

import com.xiaoyu.learn.etc.schedule.quartz.simple.demo.job.HelloJob;

/**
 * quartz simple demo
 *
 * @version 0.1
 * @author xy
 * @date 2018年7月29日 下午10:08:08
 */
public class QuartzTest {

    public static void main(String[] args) throws Exception {
        // // Grab the Scheduler instance from the Factory
        Scheduler defaultScheduler = StdSchedulerFactory.getDefaultScheduler();
        
        // setup quartz properties
        String qzPropsPath = Thread.currentThread().getContextClassLoader()
                .getResource("quartz.properties").getPath().toString();
        System.setProperty(
                "org.quartz.properties", qzPropsPath);
        System.out.println(">>>>> quartz config file path: " + qzPropsPath);
        
        // and start it off
        defaultScheduler.start();
        // -----------------------------do sth.------------------------------
        // attention: static import needed
     // define the job and tie it to our HelloJob class
        JobDetail job = newJob(HelloJob.class)
            .withIdentity("job1", "group1")// setup jobName and jobGroup
            .build();

        // Trigger the job to run now, and then repeat every 2 seconds
        Trigger trigger = newTrigger()
            .withIdentity("trigger1", "group1")
            .startNow()
                  .withSchedule(simpleSchedule()
                    .withIntervalInSeconds(2)
                    .repeatForever())            
            .build();

        // Tell quartz to schedule the job using our trigger
        defaultScheduler.scheduleJob(job, trigger);
        // ---------------------------------------------------------------
        
        Thread.sleep(60000);
        
        /**
         * Once you obtain a scheduler using `StdSchedulerFactory.getDefaultScheduler()`,
         * your application will not terminate until you call `scheduler.shutdown()`,
         * because there will be active threads.
         */
        defaultScheduler.shutdown();
    }
}

```

## 有用的注解

可以添加到 job class 上用于控制 state 和并发

- @DisallowConcurrentExecution - 规定同一个时间段内, 只能有一个 job class在执行, 但是可以有多个 jobDetail 在执行;

    举个🌰:
    有个 job class 叫 "SalesReportJob", 然后在此基础上定义两个 jobDetail "SalesReportForJoe" 和 "SalesReportForMike" (通过传递不同的jobDateMap). 如果 SalesReportJob 被标注, 则只能在给定时间执行“SalesReportForJoe”的一个实例，但它可以与“SalesReportForMike”实例同时执行

- @PersistJobDataAfterExecution - 它告诉Quartz在execute（）方法成功完成后（不抛出异常）更新JobDetail的JobDataMap的存储副本，以便下一次执行相同的工作（ JobDetail）接收更新的值而不是最初存储的值

    一般和 @DisallowConcurrentExecution 一起使用

## trigger

触发器公共的属性：

- TriggerKey 

- jobKey

- startTime -  java.util.Date, 指定 trigger’s schedule 何时首次生效

- endTime - trigger’s schedule 何时失效

- Priority - 为 trigger 设置优先级， 当 worker thread 不够用的时候生效

### quartz Calendars

在一份计划中， 排除某些日期

```java
HolidayCalendar cal = new HolidayCalendar();
cal.addExcludedDate( someDate );
cal.addExcludedDate( someOtherDate );

sched.addCalendar("myHolidays", cal, false);


Trigger t = newTrigger()
    .withIdentity("myTrigger")
    .forJob("myJob")
    .withSchedule(dailyAtHourAndMinute(9, 30)) // execute job daily at 9:30
    .modifiedByCalendar("myHolidays") // but not on holidays
    .build();

// .. schedule job with trigger

Trigger t2 = newTrigger()
    .withIdentity("myTrigger2")
    .forJob("myJob2")
    .withSchedule(dailyAtHourAndMinute(11, 30)) // execute job daily at 11:30
    .modifiedByCalendar("myHolidays") // but not on holidays
    .build();

// .. schedule job with trigger2
```

### SimpleTrigger

- 满足这样的需求: 在特定的时刻执行一次任务[间隔一定时间重复执行该任务]

- 包含这些属性: a start-time, and end-time, a repeat count, and a repeat interval.

- 使用需要 static import:

    ```java
    import static org.quartz.TriggerBuilder.*;
    import static org.quartz.SimpleScheduleBuilder.*;
    import static org.quartz.DateBuilder.*:

    ```

    - 特定时刻触发, 不重复

        ```java
        SimpleTrigger trigger = (SimpleTrigger) newTrigger()
            .withIdentity("trigger1", "group1")
            .startAt(myStartTime) // some Date 或者 .startNow() 默认就是now
            .forJob("job1", "group1") // identify job with name, group strings
            .build();
        ```

    - 特定时刻触发, 间隔10s, 重复 10+1 次

        ```java
        trigger = newTrigger()
            .withIdentity("trigger3", "group1")
            .startAt(myTimeToStartFiring)  // if a start time is not given (if this line were omitted), "now" is implied
            .withSchedule(simpleSchedule()
                .withIntervalInSeconds(10)
                .withRepeatCount(10)) // note that 10 repeats will give a total of 11 firings
            .forJob(myJob) // identify job with handle to its JobDetail itself
            .build();
        ```

    - 5min 后触发, 不重复

        ```java
         trigger = (SimpleTrigger) newTrigger()
            .withIdentity("trigger5", "group1")
            .startAt(futureDate(5, IntervalUnit.MINUTE)) // use DateBuilder to create a date in the future
            .forJob(myJobKey) // identify job with its JobKey
            .build();
        ```

    - 立即触发, 每 5min 重复一次, 直到 22:00 点

        ```java
        trigger = newTrigger()
            .withIdentity("trigger7", "group1")
            .withSchedule(simpleSchedule()
                .withIntervalInMinutes(5)
                .repeatForever())
            .endAt(dateOf(22, 0, 0))
            .build();
        ```

    - 在下一个整点触发, 然后间隔 2h, 永远重复

        ```java
         trigger = newTrigger()
            .withIdentity("trigger8") // because group is not specified, "trigger8" will be in the default group
            .startAt(evenHourDate(null)) // get the next even-hour (minutes and seconds zero ("00:00"))
            .withSchedule(simpleSchedule()
                .withIntervalInHours(2)
                .repeatForever())
            // note that in this example, 'forJob(..)' is not called
            //  - which is valid if the trigger is passed to the scheduler along with the job  
            .build();

            scheduler.scheduleJob(trigger, job);
        ```

- 执行任务失败, 可以给 trigger 设定"失败指令" ([ref](http://www.quartz-scheduler.org/documentation/quartz-2.2.x/tutorials/tutorial-lesson-05.html))

### CronTrigger

- startTime , endTime

- cronExpression 👉 [#coron表达式](#cronExpression)

- 需要静态引入

    ```java
    import static org.quartz.TriggerBuilder.*;
    import static org.quartz.CronScheduleBuilder.*;
    import static org.quartz.DateBuilder.*:
    ```

    - 每分钟触发一次, 上午5点到下午8点, 每天

        ```java
        trigger = newTrigger()
            .withIdentity("trigger3", "group1")
            .withSchedule(cronSchedule("0 0/2 8-17 * * ?"))
            .forJob("myJob", "group1")
            .build();
        ```

    - 设置时区

        ```java
        trigger = newTrigger()
            .withIdentity("trigger3", "group1")
            .withSchedule(cronSchedule("0 42 10 ? * WED"))
            .inTimeZone(TimeZone.getTimeZone("America/Los_Angeles"))
            .forJob(myJobKey)
            .build();
        ```

## TriggerListeners and JobListeners

http://www.quartz-scheduler.org/documentation/quartz-2.2.x/tutorials/tutorial-lesson-07.html

## SchedulerListeners

http://www.quartz-scheduler.org/documentation/quartz-2.2.x/tutorials/tutorial-lesson-08.html

## Job Stores

- RAMJobStore - 存储在内存中

    使用需要这样设置: `org.quartz.jobStore.class = org.quartz.simpl.RAMJobStore`

- JDBCJobStore - 存储在数据库中

# springboot和quartz实现动态配置

ref: https://blog.csdn.net/liuchuanhong1/article/details/60873295




