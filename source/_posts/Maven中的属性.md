---
title: Maven中的属性
date: 2016-12-26 22:37:14
category:
    - maven
tags:
    - maven
    - properties
---
属性这个东西吧，其实就是各种参数。在maven中，我们最常见的形式就是以properties的形式出现，然而，maven中的属性并不仅限于此。

## maven中的属性
在maven中属性有6种
- 内置属性：Maven内部定义的，可以直接使用。一般只会用到前2个
    ```
    ${basedir} : 表示项目根目录,即包含pom.xml文件的目录;
    ${version} : 表示项目版本;
    ${project.basedir} : 同${basedir};
    ${project.baseUri} : 表示项目文件地址;
    ${maven.build.timestamp} : 表示项目构件开始时间;
    ${maven.build.timestamp.format} : 表示属性${maven.build.timestamp}的展示格式,默认值为yyyyMMdd-HHmm,可自定义其格式
    ```
- POM属性：pom文件中定义过的属性，可以直接引用到对应元素
    ```
    ${project.build.directory} ： 表示主源码路径;
    ${project.build.sourceEncoding} ： 表示主源码的编码格式;
    ${project.build.sourceDirectory} : 表示主源码路径;
    ${project.build.finalName} : 表示输出文件名称;
    ${project.version} : 表示项目版本,与${version}相同;
    ```
- 自定义属性:在pom.xml文件的<properties>标签下定义的属性，也可以通过插件将外部的properties文件引入pom文件
- settings.xml文件属性:与pom属性同理,通过settings.开头的属性引用settings.xml文件中的XML元素值
    ```
    ${settings.localRepository} : 表示本地仓库的地址;
    ```
- Java系统属性: 可以在maven中使用java系统属性
    ```
    ${user.home} ： 表示用户目录;
    ```
- 环境变量属性：可以通过 env. 开头来使用环境变量。环境变量jav系统属性都可以通过 mvn help:system 来查看。
    ```
    ${env.JAVA_HOME} ：表示JAVA_HOME环境变量的值;
    ```

## 用户自定义属性
用户自定义的属性可以定义在project标签下的properties、profile下的properties。
```
<project>
    ...
    <properties>
        <my.pro>ppp</my.pro>
    </properties>
    ...
</project>
```
属性定义超简单
```
pom.xml
|-project
     |- ...
     |-properties
     |      |- ...
     |      |-property
     |             
     |- ...
```

## 小结
maven属性就是提供方便，常和其他特性一起使用。
