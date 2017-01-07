---
title: Tomcat中最重要的配置文件——server.xml
date: 2017-01-05 20:57:37
category:
    - Tomcat
tags:
    - Tomcat
    - server.xml
---
Tomcat作为最常用的Java web服务器，它免费、使用广泛。一直使用，却没有好好取研究它，有必要好好学习下。

Tomcat是一个基于组件的web服务器，自然的，这些组件都是可以配置的。其配置文件 ./conf/server.xml 就是Tomcat组件的配置文件。

## 先来一张图
这个图是根据tomcat 7.x 和 8.x 的server.xml文件画出来的
![server.xml](https://github.com/tks6754/Photos01/raw/master/tomcatserver.png)

图中每个组件，可以对应到 server.xml 文件中。大部分可以重复配置，少数几个只能出现配置一个。

<!-- more -->

## 最外层的server
server标识的是Catalina servlet组件，是tomcat最外层组件。

每一个server组件会起服务，专门监听指定端口，等待“关闭”指令来停止服务。

常见属性
| 属性 | 作用     |
| :------------- | :------------- |
| port       | 默认为 8005，是停止tomcat运行的监控端口       |
| shutdown  | 默认值为“SHUTDOWN”，指定关闭tomcat的识别命令  |

## Listener
就如名字，是监听器组件，可以有多个，通过指定calssName来使用不同的监听组件。

Listener可以嵌在多个地方，比如 Server、Engine、Host、Context

常用属性
| 属性 | 作用     |
| :------------- | :------------- |
| calssName    | 指定启用的监听组件  |

## GlobalNamingResources
用于JNDI，不怎么使用，后续再看

## Service
Service是Tomcat最重要的组件，这个组件组成一个完整服务。

常用属性
| 属性 | 作用     |
| :------------- | :------------- |
| name   | 默认为“Catalina”，Service的名字   |

一个Service一般由多个连接器（Connector）和一个引擎（Engine）组成，每个Service中有且只能有一个Engine。哈哈，都用上数学用词了。

### Connector
Connector的作用是用来获得请求，然后将请求送往Engine

根据请求来源的不同一般分为两种
- 来自客户端（浏览器）的请求，支持的协议是 HTTP/1.1 。即 HTTP Connector
- 来自其他服务器（IIS、Nginx）的请求，一般支持的协议是 AJP/1.3 。 即 AJP Connector

常用属性
| 属性 | 作用     |
| :------------- | :------------- |
| executor   | 指定线程池   |
| port  | 连接器接收请求的端口  |
| protocol  | 使用的协议  |
| connectionTimeout    | 默认为“20000”，单位是ms。一个请求的最长连接时间   |
| redirectPort  | 默认为“8433”，在处理http请求时，转发收到的SSL传输请求  |   
| enableLookups | “true”或“false”，通过request.getRemoteHost()方法，根据DNS查询来获取客户端主机名  |
| maxThreads  | 最大并发连接数，默认200个  |


### Engine
每个Service中只能包含一个Engine，Engine是Servlet引擎，来处理所有Connector接收的请求，并把不同的请求发往不同的Host。由Host处理完请求，再经由Engine返回给Connector。

常用属性
| 属性 | 作用     |
| :------------- | :------------- |
| name | Engine的名字，一般和Service同名    |
| defaultHost  |  指定默认Host   |

#### Host
Host代表一个 Virtual Host，即虚拟主机。每个Host与某个网络域名对应，相当于tomcat目录下webapps目录。而每个Host中可以部署多个web应用。

常用属性
| 属性 | 作用     |
| :------------- | :------------- |
| name  | 名字，一般为“localhost”  |
| appBase  | 默认为“webapps”，存放web项目的目录路径  |
| unpackWAR  | “true”或“false”。true时，将WAR项目解压成目录结构形式部署；false时，直接从WAR文件运行web项目  |
| autoDeploy |“true”或“false”。设为ture，检测到appBase目录下有新项目，会自动部署  |

##### Context
一个Host中可以有多个Context，一个Context相当于一个web项目。

在实际web项目运行中，Context会根据配置文件 $CATALINA_HOME/conf/web.xml 和 $WEBAPP_HOME/WEB-INF/web.xml 载入Servlet类

常用属性
| 属性 | 作用     |
| :------------- | :------------- |
| path  | web项目的url  |
| docBase | web项目的存放目录，可以是绝对路径，可以是相对与Host的appBase的相对路径  |
| reloadable | web项目重载。为true时，Tomcat会实时根据 /WEB-INF/lib 和 /WEB-INF/classes 目录的变化，自动装载新的应用程序  |

```
到这里可以总结一些在浏览器中访问web项目的url问题。
一般，在本地运行一个web项目的uri地址为：http://localhost:8080/web。
未完～
```

#### Value
Valve类似于过滤器，它可以工作于Engine和Host/Context之间、Host和Context之间以及Context和Web应用程序的某资源之间。一个容器内可以建立多个Valve，而且Valve定义的次序也决定了它们生效的次序。

#### Realm
Realm可以理解为包含用户、密码、角色的”数据库”。

Tomcat定义了多种Realm实现：JDBC Database Realm、DataSource Database Realm、JNDI Directory Realm、UserDatabase Realm等

#### Logger
记录日志组件，Tomcat运行日志，Catalina类日志


## 小结
了解Tomcat的基本组件，特别是Service组件内部的关系，Tomcat是怎样接收请求，处理请求的基本流程。然后，关于Realm组件和Value组件，后续还需要专门学习下，看看在实际项目中是怎么使用的。
