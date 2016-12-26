---
title: Maven中的过滤参数
date: 2016-12-26 21:39:03
category:
    - maven
tags:
    - maven
    - filter
---
Maven中默认提供一种很简单，但是很有用的机制：过滤参数。

严格的说，过滤“级别”有两个层次：pom文件内的自动参数过滤；resource资源文件的参数过滤。

## pom文件内的自动参数过滤
这种实在是太常见了，常见到大家都没有意思到这也是参数过滤。pom文件中经常 ${param.key}的参数，这样形式会被自动找到对应的值，并填充该位置。

## resource资源文件的参数过滤
其实这是maven中默认的resource插件maven-resources-plugin支持的过滤功能。所谓过滤，就是在build时，将外部的properties文件的键值对或pom文件中配置的properties键值对，来替换资源文件中的 ${param.key}，起到统一管理配置的效果。

常见过滤使用
```
<project>
...
    <build>
        ...
        <filters> <!-- 过滤器，用于过滤resource中的各个文件 -->
            <filter>filters/filter1.properties</filter>
        </filters>
        <resources>
            <resource>
                <targetPath>META-INF/plexus</targetPath>
                <filtering>false</filtering> <!-- 是否使用过滤器 -->
                <directory>${basedir}/src/main/plexus</directory>
                <includes>
                    <include>configuration.xml</include>
                </includes>
                <excludes>
                    <exclude>**/*.properties</exclude>
                </excludes>
            </resource>
        </resources>
        ...  
    </build>
...
</project>
```
从例子中看出，在filter中配置过滤的参数文件，在resource中决定该资源文件启用过滤。
```
pom.xml
|-project
     |- ...
     |-build
     |   |- ...
     |   |-filters
     |   |    |-filter
     |   |- ...
     |   |-resources
     |   |    |-resource  
     |   |        |-targetPath
     |   |        |-filtering
     |   |        |-directory
     |   |        |-includes
     |   |        |     |-include
     |   |        |-excludes
     |   |              |-exclude
     |   |- ...
     |- ...
```
配置文件说明

| 配置项          | 是否必配        |     含义        |
| :------------- | :------------- | :------------- |
| filter  |  否 | 指定properties文件，用于过滤  |
| resource  | 否  | 资源文件配置   |
| targetPath   | 否  | build后resource文件的存放目录，默认为${basedir}  |
| filtering   | 否  | 是否启用filter，true/flase  |
| directory   | 否  | resource文件所在目录，默认为${basedir}/src/main/resources  |
| include   | 否  | 指定作为resource文件  |
| exclude   | 否  | 排除某些文件，在include也配置了，则exclude中起作用  |

## 结
需要注意的是filter和resource一般一同使用，当然也可以单独使用。但filter单独使用并不能起作用，resouce单独使用作为指定resource文件位置。
