---
title: Mybatis简单使用
date: 2017-01-11 23:18:41
category:
    - Mybatis
tags:
    - Mybatis
---
Mybatis是一个Java实现的数据持久层开源框架，其内部封装了JDBC操作，简化了很多操作。Mybatis用映射的方式，使程序猿将注意力集中在SQL，而且实现了动态SQL使用，具有很好的可用性。

## 简单原理
Mybatis有2个配置文件，mapper.xml 和 mybatisConfig.xml(mybatis配置文件)。在mapper.xml中编写的SQL映射语句会被解析为statement；在mybatisConfig.xml配置文件中配置数据源和事物类型，并配置映射配置文件。Mybatis通过 SqlSession 操作数据库，SqlSession内部通过执行器 Exector 来执行mapper文件中映射语句来操作数据库。

<!-- more -->

```
                Mybatis全局配置文件 (配置 数据源、事物 等运行环境，配置映射文件)
                      |
                      |
                SqlSessionFactory (由配置文件创建工厂)
                      |
                      |
                  SqlSession (会话，数据操作接口)
                      |
                      |
                   Exector (执行器，实际操作执行接口，具有缓存功能)
                      |
                      |
 参数输入 ———> mapped statement (底层封装对象，等同于JDBC中的statement) ———> 输出参数
                      |
                      |
                    MySQL
```

## 一个超简单的例子
mapper.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.demo">
    <select id="findUserById" parameterType="int" resultType="com.iot.mybatis.po.User">
        SELECT * FROM  user  WHERE id=#{value}
    </select>
</mapper>

```

mybatisConfig.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC" />
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver" />
                <property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatisdb?characterEncoding=utf-8" />
                <property name="username" value="root" />
                <property name="password" value="123" />
            </dataSource>
        </environment>
    </environments>

    <!-- 加载映射文件-->
    <mappers>
        <mapper resource="sqlmap/User.xml"/>
    </mappers>
</configuration>
```

Test.java
```Java
public class Test {

    @Test
    public void findUserByIdTest() throws IOException{
        String resource = "mybatisConfig.xml";
        InputStream inputStream =  Resources.getResourceAsStream(resource);
        //由mybatis配置文件创建会话工厂
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        // 通过工厂得到SqlSession
        SqlSession sqlSession = sqlSessionFactory.openSession();

        // sqlSession查询记录
        User user = sqlSession.selectOne("com.demo.findUserById", 1);

        System.out.println(user);

        // 释放资源
        sqlSession.close();
    }


}
```

实际使用中，一般使用mapper接口映射，简化dao层的代码。特别是结合Spring后，通过Spring管理对象，更加简单代码。

Mapper.java
```
//注意包路径必须要和mapper.xml的namespace相同
package com.demo

public interface Mapper {
    //根据id查询用户信息
    public User findUserById(int id) throws Exception;

}

```
