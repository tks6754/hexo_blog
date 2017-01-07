---
title: Tomcat的web配置文件web.xml
date: 2017-01-07 15:37:57
category:
    - Tomcat
tags:
    - Tomcat
    - web.xml
---
在Java web项目中总会涉及到web.xml配置，这是一个初始化项目的配置，常见的配置项涉及welcome页、servlet、filter、listener，以及启动加载级别的一些配置。

其实，在Tomcat里也有一个同名的配置文件，./conf/web.xml 。而一个Context组件对应一个web app，在初始化的时候，Context会先去载入$CATALINA_HOME/conf/web.xml中部署的servlet类，然后才会去载入web-app/WEB-INF/web.xml中配置的servlet类。所有被加载的servlet都会将 servlet名字和映射地址 为一组存入Context的映射表（mapping table）中。Context在获得请求时，会根据该表请求对应的servlet，并执行，返回回应。

<!-- more -->

## $CATALINA_HOME/conf/web.xml
这个web.xml是所有Context所共享的，也就是说其中定义的servlet，所有的web app都会载入。

该web.xml中默认配置servlet有：
```
<servlet>
    <servlet-name>default</servlet-name>
    <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
    <init-param>
        <param-name>debug</param-name>
        <param-value>0</param-value>
    </init-param>
    <init-param>
        <param-name>listings</param-name>
        <param-value>false</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>

<servlet>
    <servlet-name>jsp</servlet-name>
    <servlet-class>org.apache.jasper.servlet.JspServlet</servlet-class>
    <init-param>
        <param-name>fork</param-name>
        <param-value>false</param-value>
    </init-param>
    <init-param>
        <param-name>xpoweredBy</param-name>
        <param-value>false</param-value>
    </init-param>
    <load-on-startup>3</load-on-startup>
</servlet>

<!-- 对应mapping省略 -->
```

- DefaultServlet ：当用户的HTTP请求无法匹配任何一个servlet的时候，该servlet被执行
- JspServlet ：当请求的是一个JSP页面的时候（\*.jsp）该servlet被调用。是一个JSP编译器，将请求的JSP页面编译成为servlet再执行

除了上述2个servlet外，还有很多的 Default MIME Type Mappings ，以及几个 welcome页 配置。

## web-app/WEB-INF/web.xml
该web.xml是随项目不同而不同的，基本可配置类型在开头已经简介了。之后在介绍servlet时详细看看。

## 小结
Tomcat中最重要的配置文件server.xml和web.xml都基本看完了，其正真的实现，也就是源码，也要找时间好好读读，毕竟配置文件只是了解个大概。
