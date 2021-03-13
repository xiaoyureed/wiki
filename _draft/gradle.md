---
title: gradle
tags: gradle
categories: tool
---


<!--more-->

# 简介

## why gradle

maven是使用xml进行配置的，语法不简洁

maven 自定义任务不方便, 必须在 maven 的生命周期中使用插件的方式去工作

## what is gradle

gradle并不是另起炉灶，它充分地使用了maven的现有资源。继承了maven中仓库，坐标，依赖这些核心概念

同时，它又继承了ant中target的概念，我们又可以重新定义自己的任务了。(gradle中叫做task)


# 配置国内镜像源

```

a). 配置只在当前项目生效
在 build.gradle 文件内修改/添加 repositories 配置


repositories {
    maven {
        url "http://maven.aliyun.com/nexus/content/groups/public"
    }
}


b). 配置全局生效
找到 (用户家目录)/.gradle/init.gradle 文件，如果找不到 init.gradle 文件，自己新建一个

allprojects {
  repositories {
    def ALIYUN_REPOSITORY_URL = 'http://maven.aliyun.com/nexus/content/groups/public'
    def ALIYUN_JCENTER_URL = 'http://maven.aliyun.com/nexus/content/repositories/jcenter'
    all { ArtifactRepository repo ->
        if(repo instanceof MavenArtifactRepository){
            def url = repo.url.toString()
            if (url.startsWith('https://repo1.maven.org/maven2')) {
                project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_REPOSITORY_URL."
                remove repo
            }   
            if (url.startsWith('https://jcenter.bintray.com/')) {
                project.logger.lifecycle "Repository ${repo.url} replaced by $ALIYUN_JCENTER_URL."
                remove repo
            }   
        }   
    }   
    maven {
        url ALIYUN_REPOSITORY_URL
        url ALIYUN_JCENTER_URL
    }   
  }
}




验证是否修改成功
在 build.gradle 文件内增加一个任务

task showRepos {
    doLast {
        repositories.each {
            println "repository: ${it.name} ('${it.url}')"
        }
    }
}
然后执行 gradle -q showRepos 任务，如果输出了刚刚配置的地址就说明修改成功，如下：

$ gradle -q showRepos
repository: maven ('http://maven.aliyun.com/nexus/content/groups/public')


```

# 使用

https://plugins.gradle.org/
https://gradle.org/