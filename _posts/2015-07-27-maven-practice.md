---
layout: post
title: Apache Maven 使用指南
categories: [configuration]
tags: [Maven]
---

###Apache Maven:
-----------------
Maven 是一个项目管理和构建自动化工具。但是对于我们程序员来说，我们最关心的是它的项目构建功能。所以这里我们介绍的就是怎样用 maven 来满足我们项目的日常需要。
Maven 使用惯例优于配置的原则 。它要求在没有定制之前，所有的项目都有如下的结构:

|目录|目的|
|----|----|
|${basedir}|存放 pom.xml和所有的子目录|
|${basedir}/src/main/java|项目的 java源代码|
|${basedir}/src/main/resources|项目的资源，比如说 property文件|
|${basedir}/src/test/java|项目的测试类，比如说 JUnit代码|
|${basedir}/src/test/resources|测试使用的资源|

###Maven 的安装:
[maven下载安装](http://maven.apache.org/download.html)  
`$ mvn -v`

###Maven 的使用:
1. archetype 
{% highlight sh %}
$ mvn archetype:generate 
-DgroupId=com.mycompany.helloworld 
-DartifactId=helloworld 
-Dpackage=com.mycompany.helloworld 
-Dversion=1.0-SNAPSHOT
{% endhighlight %}

2. 构建  
{% highlight sh %}
$ mvn clean compile   
$ mvn clean test   
$ mvn clean package  
$ mvn clean install
{% endhighlight %}

3. POM  
{% highlight xml %}
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
     <modelVersion>4.0.0</modelVersion> 

     <groupId>com.mycompany.helloworld</groupId> 
     <artifactId>helloworld</artifactId> 
     <version>1.0-SNAPSHOT</version> 
     <packaging>jar</packaging> 

     <name>helloworld</name> 
     <url>http://maven.apache.org</url> 

     <properties> 
       <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding> 
     </properties> 

     <dependencies>
       <dependency> 
         <groupId>junit</groupId> 
         <artifactId>junit</artifactId> 
         <version>3.8.1</version> 
         <scope>test</scope> 
       </dependency> 
     </dependencies> 
    </project>
{% endhighlight %}

在 POM 中，groupId, artifactId, packaging, version 叫作 maven 坐标，它能唯一的确定一个项目。有了 maven 坐标，我们就可以用它来指定我们的项目所依赖的其他项目，插件，或者父项目。一般 maven 坐标写成如下的格式：  
`groupId:artifactId:packaging:version`  
像我们的例子就会写成：  
`com.mycompany.helloworld: helloworld: jar: 1.0-SNAPSHOT`  

我们的 helloworld 示例很简单，但是大项目一般会分成几个子项目。在这种情况下，每个子项目就会有自己的 POM 文件，然后它们会有一个共同的父项目。这样只要构建父项目就能够构建所有的子项目了。子项目的 POM 会继承父项目的 POM。

4. scope  
* compile (默认)
* test
* provided (servlet-api)
* runtime (JDBC Driver)
* system
* import

5. 聚合和继承

6. 反应堆  

7. 常用命令  

