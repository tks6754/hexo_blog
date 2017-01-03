---
title: Tomcat自身的web管理
date: 2017-01-04 00:06:11
category:
    - tomcat
tags:
    - tomcat
    - tomcat-user.xml
---
Tomcat用的不少，不过很少有使用到Tomcat自带的配合和管理工具。刚好，近期刚好打算把Tomcat好好整理补充下，于是查了些料，做点小实验。

启动Tomcat后，进入 http://localhost:8080/ ，中间会有三个按钮 Server Status、Manager App、Host Manger。这三个按钮就是Tomcat管理界面的入口，Tomcat默认是不开启的，要进去还需要进行一些必要的配置。

## ./conf/tomcat-user.xml
看名字就知道这个配置文件的意思，Tomcat用户配置，要进入之前提的管理界面，需要在这个文件中开启并配置Tomcat用户信息。

```
为方便理解，插一句。在./webapps中，默认存放着几个项目

|-apache-tomcat
      |- ...
      |- webapps
      |     |-docs : Tomcat使用文档
      |     |-examples ： 一些自带例子
      |     |-host-manager ： manager管理，按钮 Server Status、Manager App的项目文件
      |     |-manager ： admin管理，按钮 Host Manger的项目文件
      |     |-ROOT ： 默认入口
      |- ...

默认 http://localhost:8080/ 的 / 进入的ROOT项目中，在进入管理界面时，tomcat对其进行了限制。
```

<!-- more -->

### 配置模板
在tomcat-user.xml文件中，已经给出模板。
```
<tomcat-users>
    <role rolename="tomcat"/>
    <role rolename="role1"/>
    <user username="tomcat" password="<must-be-changed>" roles="tomcat"/>
    <user username="both" password="<must-be-changed>" roles="tomcat,role1"/>
    <user username="role1" password="<must-be-changed>" roles="role1"/>
</tomcat-users>
```
配置说明

| 配置项 | 说明     |
| :------------- | :------------- |
| role      |  角色      |
| rolename      |  角色名，一般只使用Tomcat定义好的角色     |
| user       | 用户       |
| username   | 用户名，自己定义   |
| password   | 密码，自己指定       |
| roles       | 角色，必须是在role中配置过的角色，多个角色用逗号分割      |

### Tomcat定义的角色
Tomcat定义好的角色及其权限，其实在点击 按钮 Server Status、Manager App、Host Manger 后，跳出输入用户名和密码，点击 取消 或 进入错误，会进入“401 Unauthorized”未授权页，会有相应的提示。（英文很简单，不翻译了）

三个按钮 Server Status、Manager App、Host Manger 按项目路径来分，前两个都属于 /manager项目，第三个属于 /host-manager项目。也就是说，角色也对应了两个项目。

| 角色    | 权限     |
| :------------- | :------------- |
| manager-gui     | allows access to the HTML GUI and the status pages      |
| manager-script  | allows access to the text interface and the status pages       |
| manager-jmx     | allows access to the JMX proxy and the status pages       |
| manager-status  | allows access to the status pages only       |
| ---=---        |  ---=---      |
| admin-gui      | allows access to the HTML GUI      |
| admin-script   | allows access to the text interface      |
角色配置注意项：
- Users with the manager-gui role should not be granted either the manager-script or manager-jmx roles.
- If the text or jmx interfaces are accessed through a browser (e.g. for testing since these interfaces are intended for tools not humans) then the browser must be closed afterwards to terminate the session.
- Users with the admin-gui role should not be granted the admin-script role.
- If the text interface is accessed through a browser (e.g. for testing since this interface is intended for tools not humans) then the browser must be closed afterwards to terminate the session.

### 新版本的一点不同
配置好用户后，一般就可以进入管理界面了。需要注意，比较新的Tomcat版本中，在项目 manager和host-manager的 ./WEB-INF/context.xml 中有如下内容
```
<Context antiResourceLocking="false" privileged="true" >
    <Valve className="org.apache.catalina.valves.RemoteAddrValve"
          allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />
</Context>
```
在低版本中，Valve是默认被注释的，新版本是打开的。要进入管理界面，要将 Value 注释掉。

## 小结
只做一些简单的配置说明，Tomcat管理工具具体使用，后续再讨论，毕竟我自己也不怎么用。
