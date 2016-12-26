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
其实这是maven中默认的resource插件maven-resources-plugin支持的过滤功能。所谓过滤，就是在build时，将外部的properties文件的键值对或pom文件中配置的properties键值对来替换资源文件中的 ${param.key}，起到统一管理配置的效果。

常见过滤使用
```

```
