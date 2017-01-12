---
title: Mybatis配置文件中的配置项
date: 2017-01-12 20:49:57
category:
    - Mybatis
tags:
    - Mybatis
---
Mybatis通过配置文件配置项获得SqlSessionFactory。在Mybatis中可以配置很多项目，最常见的 数据源、事务类型、缓存配置、映射文件配置。

## 属性 Properties
在很多开源框架中都支持属性配置，属性配置方式可以是外部引入的properties文件，也可以是直接设置。一般多为数据源属性配置。
```xml
<configuration>
...
    <properties resource="org/mybatis/example/config.properties">
      <property name="username" value="dev_user"/>
      <property name="password" value="F2Fa3!33TYyg"/>
    </properties>
...
</configuration>
```
<!-- more -->

如果属性在多个地方进行了配置，那么 MyBatis 将按照下面的顺序来加载，且后面属性覆盖前面的加载属性：
1. 在 properties 元素体内指定的属性首先被读取。
2. 然后根据 properties 元素中的 resource 属性读取类路径下属性文件或根据 url 属性指定的路径读取属性文件，并覆盖已读取的同名属性。
3. 最后读取作为方法参数传递的属性，并覆盖已读取的同名属性。

建议将属性都写在.properties 的外部文件。

## 环境 environments
environments中可以配置多个environment，而每个environment中需要配置 数据源dataSource和事务管理器transactionManager。
```xml
<configuration>
...
    <environments default="development">
      <environment id="development">
        <transactionManager type="JDBC">
          <property name="..." value="..."/>
        </transactionManager>
        <dataSource type="POOLED">
          <property name="driver" value="${driver}"/>
          <property name="url" value="${url}"/>
          <property name="username" value="${username}"/>
          <property name="password" value="${password}"/>
        </dataSource>
      </environment>
    </environments>
...
</configuration>
```
每个environment需要指定属性id，environments中可以通过该id指定默认default环境。
创建SqlSessionFactory可以指定environment来创建，也可以不指定，使用默认
```java
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment);
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment,properties);

SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader);
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader,properties);
```

### 事务类型
事务管理器中type可配置为JDBC|MANAGED
- JDBC：直接使用了 JDBC 的提交和回滚设置，它依赖于从数据源得到的连接来管理事务作用域。
- MANAGED：从来不提交或回滚一个连接，而是让容器（JBoss， WebLogic，GlassFish）来管理事务的整个生命周期。默认情况下它会关闭连接，如果不希望这样，可将 closeConnection 属性设置为 false 来阻止它默认的关闭行为。

当使用 Spring + MyBatis，则没有必要配置事务管理器， 因为 Spring 模块会使用自带的管理器来覆盖前面的配置。

### 数据源
数据源配置并非必须，只是为了方便使用延迟加载，数据源才是必须的。

有三种内建数据源类型： UNPOOLED|POOLED|JNDI
- UNPOOLED：每次被请求时打开和关闭连接
- POOLED：数据库连接池，数据库操作从池中取连接，操作完成后将此连接返回给连接池。
- JNDI：从在应用服务器向配置好的 JNDI 数据源 dataSource  

## settings
顾名思义，关于Mybatis的功能配置，如常用的缓存启用设置。

[更多的看这里](http://www.mybatis.org/mybatis-3/zh/configuration.html#settings)

## typeAliases 别名
方便在sqlmapper文件中使用，作为  resultType 和 parameterType 的属性值。

几种配置方式
1. Mybatis中默认配置好的别名，基本数据类型，对大小写不敏感
2. 自定义别名
  - 配置文件中配置：配置单个别名|批量配置
  - 注解方式配置（只能单个配置） @Alias

```
<typeAliases>  
    <!-- 针对单个别名定义  type:类型的路径，alias:别名-->  
    <typeAlias type="c.mybatis.PO.User" alias="user"/>  

    <!-- 批量定义别名
    指定包名，mybatis自动扫描包中的PO类，自动定义别名，别名就是类名(首字母可大写可小写) -->  
   <package name="c.mybatis.PO"/> 
</typeAliases>
```
```java
@Alias("StudentAlias")
public class Student{ }
```

## typeHandlers 类型处理器
解决的PreparedStatement中数据类型与Java数据类型转换的问题。与typeAliases类似，有Mybatis内部实现的，也可以自己来定义。

[具体看这里](http://www.mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)

## 对象工厂 objectFactory
额～基本没用过，之后在看看吧

## 插件 plugins
在已映射语句执行过程中的某一点进行拦截，这就使用到插件。

其实，我也没用到过～

## databaseIdProvider
更没用过了，一般只用MySQL

## 映射器 mappers
将SQl映射配置文件加载到配置文件中。

有多种定位方式
1. mapper
  - resource 相对路径，定位xml文件
  - url 绝对路径，定位xml文件
  - class 相对路径，定位mapper接口
2. package
  - name 相对路径，定位mapper接口所在的包，自动加载所有mapper接口

```xml
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>

  <mapper url="file:///var/mappers/AuthorMapper.xml"/>

  <mapper class="org.mybatis.builder.AuthorMapper"/>

  <package name="org.mybatis.builder"/>
</mappers>
```

## 总体结构
```
|-configuration
    |-properties（一般用于引入外部配置文件）
    |-settings（设置MyBatis的相关特性，如缓存机制、延迟加载）
    |-typeAliases（定义别名）
    |-typeHandlers（类处理器，JDBC类型与Java类型转换器）
    |-objectFactory
    |-plugins
    |-environments（数据库连接的相关配置，与spring整合后可在spring.xml中配置）
    |   |- ...
    |   |- environment（单个数据源的配置）
    |         |-transactionManager（失误处理机制）
    |         |-dataSource（数据源连接信息）
    |-mappers（加载映射文件）
```

## 小结
熟悉下就好，数据类型处理器要写几个例子。在与Spring结合后，配置会简化很多。
