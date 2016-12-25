---
title: Maven中的dependencies与dependencyManagement
date: 2016-12-25 01:24:24
category:
    - maven
tags:
    - maven
    - dependency
    - pom
---
起初我只知道Maven中的依赖是使用dependencies来配置的,后来有看到项目的pom中有dependencyManagement。就字面意思来说，这是一个“依赖管理”。。。好吧，还是来看看这两者到底是怎么用的吧。

## 坐标（GAV）
坐标是maven中的一个基础概念，maven是怎么找到定位一个依赖（构件）的？就是通过这个坐标方式，Maven中使用三个属性来定位一个构件。
- groupId（G）：组织名或项目名
- artifactId（A）：构件id，所谓构件一般就是依赖jar包
- version（V）：版本号，随着时间和项目推进，每个构件出现很多版本，通过版本号来确定对应版本

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
|  groupId  |    是      | 一般表组织或项目名称       |
|  artifactId |   是    |  一般表模块名称    |
|  version  |  否     |   模块版本，子模块依赖继承父模块pom的dependencyManagement中的依赖可省略   |
|  scope  |   否     |   依赖范围    |

## 二者的使用场景及区别

### dependencies
dependencies是模块管理依赖的配置项。通过坐标来自动添加模块依赖，并且这些依赖在会在子模块中直接被继承引入。

### dependencyManagement
dependencyManagement一般只会出现在多个模块的项目中，而且是出现在父模块的pom文件中。

常见做法
在项目顶层模块的pom中配置dependencyManagement,并指定完整的依赖坐标,在项目的子模块pom中如果要使用依赖,只需要在dependencies配置groupId和artifactId,而version会自动使用父模块pom中对应依赖指定的版本号。这样做的好处是可以统一控制项目中的依赖版本，保证项目完好运行。

### 二者区别和一些注意点
1. dependencies在父模块pom中添加的依赖，会在父模块中加入，而且，不管子模块pom中是否配置，都会自动被加载到子模块的依赖中；dependencyManagentment则不同，在父模块中不会作为依赖加入，只是作为依赖“声明”，在子模块pom中配置dependencies的依赖才会作为依赖引入；
2. dependencyManagentment的继承依赖是有条件的，只有在子模块pom中配置groupId和artifactId且没有配置version，这种情况会继承父模块中依赖配置，使用父模块pom中指定的版本号和依赖范围；但子模块pom依赖配置了完整的坐标gav，则会使用子模块pom中配置的版本号对应依赖。
