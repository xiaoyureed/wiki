---
title: mvn-dependency-tree
tags: [maven, shell]
categories: tool
---
<div align="center">
springboot中使用log4j2需要排除到所有组件的logback依赖, 排除掉 `spring-boot-starter-web` 中的logback后, 在 maven dependencies中还是查看到logback的存在, `mvn dependency:tree`查看依赖树, 翻的老眼昏花, 终于找到了. 由此想到是不是可以通过一条脚本简化这个过程呢
</div>

<!--more-->

看看依赖树的输出结果:

```shell
$ mvn dependency:tree
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building rest-api-boot 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:3.0.2:tree (default-cli) @ rest-api-boot ---
[INFO] com.xiaoyu.obj:rest-api-boot:jar:0.0.1-SNAPSHOT
[INFO] +- org.springframework.boot:spring-boot-starter-freemarker:jar:2.0.2.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter:jar:2.0.2.RELEASE:compile
[INFO] |  |  +- javax.annotation:javax.annotation-api:jar:1.3.2:compile
[INFO] |  |  \- org.yaml:snakeyaml:jar:1.19:runtime
[INFO] |  +- org.freemarker:freemarker:jar:2.3.28:compile
[INFO] |  \- org.springframework:spring-context-support:jar:5.0.6.RELEASE:compile
[INFO] |     +- org.springframework:spring-beans:jar:5.0.6.RELEASE:compile
[INFO] |     \- org.springframework:spring-context:jar:5.0.6.RELEASE:compile

```

扫描每一行, 发现目标则向上(往回)扫描每一行, 如果发现该行没有"|"符号,则返回该行