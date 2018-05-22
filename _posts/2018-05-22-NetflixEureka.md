---
layout: post
title: "Netflix eureka"
date: 2018-05-22 14:13:57
image: 'https://adongs.github.io/assets/img/resources/netflix-eureka.jpg'
description: 学习Netflix eureka
category: 'Netflix eureka'
tags:
- Spring boot
- Spring
- Spring Cloud
- Netflix eureka
introduction: Netflix eureka(注册中心)搭建和理解
---

### 理解


### 疑问

> 启动时候如果遇到如下错误(依赖包版本冲突)

![placeholder](https://adongs.github.io/assets/img/blog/springcloud/eureka/7.jpg "idea创建项目")

- 修改父项目的pom.xml的配置如下(原来的2.0.2.RELEASE修改为1.5.4.RELEASE)

```xml
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.4.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
```

### 架构图

- 如下图eureka 服务,就是这个eureka 只是spring cloud成员的一位
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/config/14.jpg "idea创建项目")


### 创建项目

>前提环境 IDEA,JDK1.8

- 打开idea创建项目,点击Next(由于之前创建的是微服务项目,这个项目是在父项目中创建的)
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/eureka/1.jpg "idea创建项目")

- 设置好group和artifact名称,点击Next(创建config项目),点击Next
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/eureka/2.jpg "idea创建项目")

- 选择 Core-><a href="https://www.zhihu.com/question/42348457">Lombok</a>,点击Next
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/eureka/3.jpg "idea创建项目")

- 设置项目名称,点击Finish (项目名为eureka,你也可以自定义)
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/eureka/4.jpg "idea创建项目")

- 创建好后结构如下图
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/eureka/5.jpg "idea创建项目")

- 打开eureka中的pom.xml文件,修改parent标签如下groupId和artifactId,version都需要指向父项目groupId和artifactId,version,其他修改如下

```xml
  <?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.springcloud</groupId>
    <artifactId>eureka</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>eureka</name>
    <description>Demo project for Spring Boot</description>

    <parent>
        <groupId>com.springcloud</groupId>
        <artifactId>cloud</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-eureka-server</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>


</project>
```

- 打开父项目的pom.xml文件,在modules标签里面加入如下

```xml
        <module>eureka</module>
```

- 在eureka项目EurekaApplication启动类中添加一个注解@EnableEurekaServer

![placeholder](https://adongs.github.io/assets/img/blog/springcloud/eureka/6.jpg "idea创建项目")

- 你会发现无法导入那个注解,没有指定的包,你需要打开父项目的pom.xml文件,
在标签dependencies标签下面加入如下标签

```xml
 <dependencyManagement>
        <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>Dalston.SR1</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        </dependencies>
    </dependencyManagement>
```

- eureka项目默认的配置文件是.properties格式的,为方便好的阅读修改为.yml格式


- 在application.yml 文件中添加如下配置文件

```yml
spring:
    application:
        name: eureka
server:
    port: 8761 #启动端口

eureka:
    client:
        registerWithEureka: false  #false:不作为一个客户端注册到注册中心
        fetchRegistry: false      #为true时，可以启动，但报异常：Cannot execute request on any known server
```

- 启动成功如下

![placeholder](https://adongs.github.io/assets/img/blog/springcloud/eureka/8.jpg "idea创建项目")


- 打开http://localhost:8761/,即可看到注册中心
![placeholder](https://adongs.github.io/assets/img/blog/springcloud/eureka/9.jpg "idea创建项目")




