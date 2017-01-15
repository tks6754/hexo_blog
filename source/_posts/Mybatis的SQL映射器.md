---
title: Mybatis的SQL映射器
date: 2017-01-12 23:42:58
category:
    - Mybatis
tags:
    - Mybatis
---
个人认为Mybatis之所以好用，就是因为它的映射语句，通过映射语句，可以简洁很多代码，而且可以是使用SQL的方式取操作数据库。

映射文件中常用的几个有：
- resultMap 数据库结果集对象到Java对象的映射
- sql 可被其他语句引用的可重用语句块。
- insert 映射插入语句
- update 映射更新语句
- delete 映射删除语句
- select 映射查询语句

## 输入映射、输出映射
Mybatis最新版本中支持
- 输入映射 parameterType
- 输出映射 resultType、resultMap

### parameterType
输入映射支持 简单类型、hashmap、pojo包装类

### resultType、resultMap
resultType支持 简单类型、hashmap、pojo包装类、pojo包装类列表

- 对于hashmap输出映射，会将属性名作为key，属性值作为value
- pojo包装类和pojo包装类列表，只要当查出结果的列名与pojo属性名有一个相同，那么就会创建pojo对象。单个pojo和列表在resultType中指定都是一样，在映射对应的接口返回不同

## insert
首先来复习下见表sql
```sql
CREATE TABLE `user` (
  `id` bigint(20) NOT NULL,
  `name` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8

CREATE TABLE IF NOT EXISTS `user_account` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `id_no` varchar(32) NOT NULL,
  `account_no` varchar(64) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=14 DEFAULT CHARSET=utf8;
```
这两个建表sql主要区别在于 user表 主键没有自增长，user_account表 主键设置了自增长。
对user表插入数据，必须要为主键传值；而user_account表则不需要。

上几个栗子先
```xml
<!-- 手动传主键 -->
<insert id="insert01">
  INSERT INTO user (id,name)
  VALUES (#{id},#{name})
</insert>

<!-- MySQL支持uuid -->
<insert id="insert02" parameterType="com.demo.User">
    <selectKey keyProperty="id" order="BEFORE" resultType="java.lang.String">
        SELECT uuid()
    </selectKey>
    insert into user(id,name) value(#{id},#{name});
</insert>

<insert id="insert03" parameterType="User">
  INSERT INTO user(name)
  VALUES (#{name})
</insert>

<!-- 使用数据库生成的主键id -->
<insert id="insert04" parameterType="com.demo.po.User">
    <selectKey keyProperty="id" order="AFTER" resultType="java.lang.Integer">
        SELECT LAST_INSERT_ID()
    </selectKey>
    INSERT INTO user_account (id_no, account_no)values (#{idNO},#{accountNO});
</insert>
```
