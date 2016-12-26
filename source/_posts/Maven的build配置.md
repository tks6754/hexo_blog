---
title: Maven的build配置
date: 2016-12-26 20:38:46
category:
    - maven
tags:
    - maven
    - build
---
Maven的build配置用来控制项目构建过程，可以通过build配置来使项目构建达到我们想要的效果。build里支持丰富的配置，常用的有资源文件配置、过滤配置、以及各种插件配置。

build配置在pom.xml中，配置的位置一般作为project的子元素，然后还可以在profile中配置。需要注意，作为project的子元素的build有两个特殊的配置项：xxxDirectory和extensions，这两个配置项不能出现在profile下的build中。

<!-- more -->

## 出现的形式
```
<project>
...
    <build>
          <!-- build基础配置 -->
          <finalName>finalName</finalName>
          <defaultGoal>install</defaultGoal>
          <directory>target</directory>
          <sourceDirectory>src/main/java</sourceDirectory>

          <!-- 过滤配置 -->
          <filters>
              <filter>src/main/resources/filters/${path}/${env}.properties</filter>
          </filters>

          <!-- 指定资源文件 -->
          <resources>  
              <resource>  
                <targetPath>META-INF/plexus</targetPath>  
                <filtering>false</filtering>  
                <directory>${basedir}/src/main/plexus</directory>  
                <includes>  
                  <include>configuration.xml</include>  
                </includes>  
                <excludes>  
                  <exclude>**/*.properties</exclude>  
                </excludes>  
              </resource>  
          </resources>  
          <testResources>  
            ...  
          </testResources>  

          <!-- maven 插件 -->
          <plugins>
              <plugin>
                  <groupId>org.apache.maven.plugins</groupId>
                  <artifactId>maven-resources-plugin</artifactId>
                  <version>2.7</version>
                  <configuration>
                    <encoding>UTF-8</encoding>
                  </configuration>
              </plugin>
              <plugin>
                  <groupId>org.apache.maven.plugins</groupId>
                  <artifactId>maven-compiler-plugin</artifactId>
                  <version>2.5.1</version>
                  <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                  </configuration>
              </plugin>
              <plugin>
                  <groupId>org.apache.tomcat.maven</groupId>
                  <artifactId>tomcat7-maven-plugin</artifactId>
                  <version>2.2</version>
                  <configuration>
                    <path>/${project.artifactId}</path>
                    <port>8080</port>
                  </configuration>
              </plugin>
          </plugins>

          <extensions>  
              <extension>  
                <groupId>org.apache.maven.wagon</groupId>  
                <artifactId>wagon-ftp</artifactId>  
                <version>1.0-alpha-3</version>  
              </extension>  
          </extensions>  
    </build>
...
</project>
```
可以看出，主要的配置项有：
- build基础配置：finalName、defaultGoal、directory、xxxDirectory
- 过滤配置：filters
- 资源文件配置：resources
- 插件配置和插件扩展配置：plugins、extensions

## 基本配置项说明
```
pom.xml
|-project
     |- ...
     |-build
     |   |-finalName
     |   |-defaultGoal
     |   |-directory
     |   |-xxxDirectory
     |   |- ...
     |   |-filters
     |   |    |- ...
     |   |- ...
     |   |-resources
     |   |    |- ...    
     |   |- ...
     |   |-plugins
     |   |    |- ...
     |   |- ...
     |   |-extensions
     |   |    |- ...
     |   |    
     |- ...
     |-profile
     |      |- ...
     |      |-build
     |           |- ...
     |
     |- ...
```
| 配置项          | 是否必配        |     含义        |
| :------------- | :------------- | :------------- |
| finalName  |  否  | build后生成文件的文件名，默认为${artifactId}-${version}   |
| defaultGoal  | 否   | buid的目标，可配置为一个goal或一个phase，例如jar:jar,install，默认为install   |
| directory |  否  | build后生成文件的目标目录，默认为${basedir}/target   |
| xxxDirectory  |  否  | 不常用，多出现在父模块pom中，供子模块使用，可以是绝对路径，也可以是相对路径   |
| filters  |  否  |  过滤配置，不细述  |
| resources  | 否   | 资源文件配置，不细述   |
| plugins  |  否  | 插件配置，不细述   |
| extensions  |  否  | 插件扩展配置，不细述   |


## 小结
这里对于build只是一个初步的了解，熟悉下build下的基本配置，其它分开在细说吧。
