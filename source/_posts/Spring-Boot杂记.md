---
title: Spring Boot杂记
date: 2017-03-22 22:02:16
category:
    - Spring Boot
tags:
    - Spring Boot
---

## spring-boot-maven-plugin插件
1. man package : 打包生成一个可执行的 jar 包，jar 包中包含所有依赖，可以直接运行。（java -jar xxx.jar）
2. mvn spring-boot:run : 在 tomcat(默认容器) 中直接启动项目。
3. Spring-Loaded 热部署支持依赖，需要在该插件下添加该依赖，代码改动后，需要 IDE 编译(开启自动编译功能)，然后生效
