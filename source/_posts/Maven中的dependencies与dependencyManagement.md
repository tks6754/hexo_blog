---
title: Maven中的dependencies与dependencyManagement
date: 2016-12-25 01:24:24
category:
    - maven
tags:
    - maven
    - pom
---
起初我只知道Maven中的依赖是使用dependencies来配置的,后来有看到项目的pom中有dependencyManagement。就字面意思来说，这是一个“依赖管理”。。。好吧，还是来看看这两者到底是怎么用的吧。

## 出现的基本形式
```
<!-- dependencies栗子 -->
<dependencies>  
    <dependency>  
        <groupId>org.eclipse.persistence</groupId>  
        <artifactId>org.eclipse.persistence.jpa</artifactId>  
        <version>${org.eclipse.persistence.jpa.version}</version>  
        <scope>provided</scope>  
    </dependency>  
    <dependency>  
        <groupId>javax</groupId>  
        <artifactId>javaee-api</artifactId>  
        <version>${javaee-api.version}</version>  
    </dependency>  
</dependencies>


<!-- dependencyManagement栗子 -->
<dependencyManagement>        
    <dependencies>  
        <dependency>  
            <groupId>org.eclipse.persistence</groupId>  
            <artifactId>org.eclipse.persistence.jpa</artifactId>  
            <version>${org.eclipse.persistence.jpa.version}</version>  
            <scope>provided</scope>  
        </dependency>      
        <dependency>  
            <groupId>javax</groupId>  
            <artifactId>javaee-api</artifactId>  
            <version>${javaee-api.version}</version>  
        </dependency>  
    </dependencies>  
</dependencyManagement>  
```
可以看出，dependencyManagement内部就是一个完整的dependencies。

```
pom.xml
|-project
    |- ...
    |-dependencyManagement
    |         |-dependencies
    |               |- ...
    |               |-dependency
    |                     |-groupId
    |                     |-artifactId  
    |                     |-version
    |                     |-scope
    |- ...
```
```
pom.xml
|-project
    |- ...
    |-dependencies
    |      |- ...
    |      |-dependency
    |            |-groupId
    |            |-artifactId  
    |            |-version
    |            |-scope
    |- ...
```

## 配置项说明
| 配置项          | 是否必配        |     含义        |
| :------------- | :------------- | :------------- |
|  groupId  |    是      | 一般表项目名称       |
|  artifactId |   是    |  一般表模块名称    |
|  version  |  是     |   模块版本    |
|  scope  |   否     |   依赖范围，默认为     |
