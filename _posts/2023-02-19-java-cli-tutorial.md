---
title: java cli tutorial
date: 2023-02-19
---



# Java 命令行教程

编译

```sh
javac SynchronizedDemo2.java
```

反编译

```sh
javap -verbose SynchronizedDemo2.class
```



## Others

最近使用 idea 发现诸多问题与不便。

首先是我使用的 idea 的 EAP 版本的 spring boot devtools 的热更新不能用了，

然后是早就发现的他运行都是通过图形界面 GUI 配置的，这就导致我并不知道他底层的命令行执行的是什么。

因此我需要学习 java 的命令行。

首先使用 `man java` 查看手册，对命令行有大概的了解，然后遇到具体的使用在仔细查看。

vscode安装java插件运行main所使用的命名：

```sh
/usr/bin/env /Users/ming/Library/Java/JavaVirtualMachines/temurin-17.0.3/Contents/Home/bin/java -XX:+ShowCodeDetailsInExceptionMessages @/var/folders/lq/tbyc_2497t37tcl391rb_w240000gn/T/cp_2ntkv7twgtvmq8oqze5nfz8t4.argfile com.hihusky.whisper.Application
```

通过不断减少，最终可写为

```sh
java @/var/folders/lq/tbyc_2497t37tcl391rb_w240000gn/T/cp_2ntkv7twgtvmq8oqze5nfz8t4.argfile com.hihusky.whisper.Application
```

关于 @(at sign) 是 java 命令的特殊语法，@ 后跟文本文件，表示将文本文件的信息作为参数。打开上述的 `.argfile` 文件，内容是 `-cp` 参数信息。

-cp 选项 是 --class-path 的缩写，里面全是编译后的 class 或者 jar 包，通过 semicolon(:) 扩展。

```
-cp "/Users/ming/projects/whisper/bin/main:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework.boot/spring-boot-starter-web/2.7.1/29f47f503f9955b1a9746870aeaebdba448416d/spring-boot-starter-web-2.7.1.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework.boot/spring-boot-starter-jdbc/2.7.1/bba8a72a8042cec0b7961f11b0b3332724f5845e/spring-boot-starter-jdbc-2.7.1.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework.boot/spring-boot-starter-json/2.7.1/711889df8474d7f0271b1e25cd75a9249e0a4621/spring-boot-starter-json-2.7.1.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework.boot/spring-boot-starter/2.7.1/48f7e04459ccc16d3532bfc486c1b6d629e6e0fc/spring-boot-starter-2.7.1.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/com.auth0/java-jwt/4.0.0/b73d56c5efa1b51c7e5f99a4f724f98717c02689/java-jwt-4.0.0.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.postgresql/postgresql/42.4.0/21ff952426bbfe4a041c175407333d4a07c70931/postgresql-42.4.0.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework.boot/spring-boot-autoconfigure/2.7.1/923ad789b004e8cc17d67853b1e4d3db11946f0/spring-boot-autoconfigure-2.7.1.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework.boot/spring-boot/2.7.1/8e49b8e7e9ea470a7772f489532264732ab206a2/spring-boot-2.7.1.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework.boot/spring-boot-starter-logging/2.7.1/461cf82dc10505f47d3ce2146bd01721177cde4a/spring-boot-starter-logging-2.7.1.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework.boot/spring-boot-starter-tomcat/2.7.1/c99fe94b685f1707907afb84ecb998ac13271ead/spring-boot-starter-tomcat-2.7.1.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/jakarta.annotation/jakarta.annotation-api/1.3.5/59eb84ee0d616332ff44aba065f3888cf002cd2d/jakarta.annotation-api-1.3.5.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework/spring-webmvc/5.3.21/a62db425cc29c48e138846e706ca37acb138ca13/spring-webmvc-5.3.21.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework/spring-web/5.3.21/317aadd37f70ba34ff93d068343e3110b5dcf2f/spring-web-5.3.21.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework/spring-jdbc/5.3.21/68f9b9a9b1541bdb40a7187f5eb96e3af6c27b40/spring-jdbc-5.3.21.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework/spring-context/5.3.21/fe371c85f02b8c6690fc3b3d0950ef4f965db0cd/spring-context-5.3.21.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework/spring-aop/5.3.21/58ec4ff7a0ce30a1e2612f04ad0fb13ea806705/spring-aop-5.3.21.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework/spring-tx/5.3.21/13f4f564024d2f85502c151942307c3ca851a4f7/spring-tx-5.3.21.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework/spring-beans/5.3.21/e3eae7e6d211381642a0b7507a5215e3ac1b32e1/spring-beans-5.3.21.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework/spring-expression/5.3.21/ca8c5822fc528066ec717f1e74160a1575c43192/spring-expression-5.3.21.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework/spring-core/5.3.21/1b0c9be6b972e4c615f175c70fc32e80557e68e8/spring-core-5.3.21.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.yaml/snakeyaml/1.30/8fde7fe2586328ac3c68db92045e1c8759125000/snakeyaml-1.30.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/com.zaxxer/HikariCP/4.0.3/107cbdf0db6780a065f895ae9d8fbf3bb0e1c21f/HikariCP-4.0.3.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/ch.qos.logback/logback-classic/1.2.11/4741689214e9d1e8408b206506cbe76d1c6a7d60/logback-classic-1.2.11.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.apache.logging.log4j/log4j-to-slf4j/2.17.2/17dd0fae2747d9a28c67bc9534108823d2376b46/log4j-to-slf4j-2.17.2.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.slf4j/jul-to-slf4j/1.7.36/ed46d81cef9c412a88caef405b58f93a678ff2ca/jul-to-slf4j-1.7.36.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework/spring-jcl/5.3.21/b41a2888c0e708f9fd12cf9cc0c29cebbcab2e5e/spring-jcl-5.3.21.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/com.fasterxml.jackson.datatype/jackson-datatype-jsr310/2.13.3/ad2f4c61aeb9e2a8bb5e4a3ed782cfddec52d972/jackson-datatype-jsr310-2.13.3.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/com.fasterxml.jackson.module/jackson-module-parameter-names/2.13.3/f71c4ecc1a403787c963f68bc619b78ce1d2687b/jackson-module-parameter-names-2.13.3.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/com.fasterxml.jackson.core/jackson-annotations/2.13.3/7198b3aac15285a49e218e08441c5f70af00fc51/jackson-annotations-2.13.3.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/com.fasterxml.jackson.core/jackson-core/2.13.3/a27014716e4421684416e5fa83d896ddb87002da/jackson-core-2.13.3.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/com.fasterxml.jackson.datatype/jackson-datatype-jdk8/2.13.3/d4884595d5aab5babdb00ddbd693b8fd36b5ec3c/jackson-datatype-jdk8-2.13.3.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/com.fasterxml.jackson.core/jackson-databind/2.13.3/56deb9ea2c93a7a556b3afbedd616d342963464e/jackson-databind-2.13.3.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.apache.tomcat.embed/tomcat-embed-websocket/9.0.64/2a5e4f1f04830f2bfd01108ddc59a451c4baef34/tomcat-embed-websocket-9.0.64.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.apache.tomcat.embed/tomcat-embed-core/9.0.64/2d91a06d1b93ba13a2cca9e9ea7c143a64037351/tomcat-embed-core-9.0.64.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.apache.tomcat.embed/tomcat-embed-el/9.0.64/227363669235feab54519102af723a54d1a7850e/tomcat-embed-el-9.0.64.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.slf4j/slf4j-api/1.7.36/6c62681a2f655b49963a5983b8b0950a6120ae14/slf4j-api-1.7.36.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/ch.qos.logback/logback-core/1.2.11/a01230df5ca5c34540cdaa3ad5efb012f1f1f792/logback-core-1.2.11.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.apache.logging.log4j/log4j-api/2.17.2/f42d6afa111b4dec5d2aea0fe2197240749a4ea6/log4j-api-2.17.2.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.springframework.boot/spring-boot-devtools/2.7.1/cb154a2b04532d9cb7c0b4a04132c57bf8efae31/spring-boot-devtools-2.7.1.jar:/Users/ming/.gradle/caches/modules-2/files-2.1/org.checkerframework/checker-qual/3.5.0/2f50520c8abea66fbd8d26e481d3aef5c673b510/checker-qual-3.5.0.jar"
```

然后 com.hihusky.whisper.Application 全限定名就是指定程序执行入口。

