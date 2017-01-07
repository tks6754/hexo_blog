---
title: Tomcat目录文件简单介绍
date: 2017-01-04 21:11:43
category:
    - Tomcat
tags:
    - Tomcat
---
Tomcat是组件服务器，它的目录结构和大多数工具或是说软件一样，每个目录都自己的作用，“各司其职”吧。

## 目录结构
- bin：存放Tomcat启动、停止服务程序以及一些其他脚本程序
- conf：存放Tomcat全局配置文件。常见的有 server.xml 、 web.xml等
- lib：存放Tomcat运行需要的jar包，这个目录中的文件将被附加到Tomcat的classpath中。
- logs： 存放Tomcat运行的相关日志
- temp：存放Tomcat运行的临时文件
- webapps： Web应用的发布目录，默认情况下把Web应用文件放于此目录
- work：Tomcat的工作目录，默认情况下把编译JSP文件生成的servlet类文件和字节码文件放在该目录
- 还有一些协议、介绍和使用手册文件不用在意了

<!-- more -->

## ./conf 目录下的配置文件
- server.xml：组件配置文件，包含Service,Connector, Engine, Realm, Valve, Hosts主组件的相关配置信息；
- web.xml：遵循Servlet规范标准的配置文件，用于配置servlet，并为所有的Web应用程序提供包括MIME映射等默认配置信息；
- tomcat-user.xml：Realm组件认证时用到的相关角色、用户和密码等信息；manager和hostManager会使用到这个文件
- catalina.policy：Java相关的安全策略配置文件，在系统资源级别上提供访问控制的能力；
- catalina.properties：Tomcat内部package的定义及访问相关的控制，也包括对通过类装载器装载的内容的控制；Tomcat在启动时会事先读取此文件的相关设置；
- logging.properties：Tomcat的日志配合文件
- context.xml：所有host的默认配置信息；
