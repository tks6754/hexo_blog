---
title: JDBC简单使用
date: 2017-01-03 20:17:45
category:
    - JDBC
tags:
    - JDBC
---
Java中，所有对数据库进行的操作都是建立在JDBC的基础上的。记得在学JDBC的时候，有一套标准流程。这篇文只是想对JDBC做一些简单总结，然后补充一些不常用的API。

## JDBC标准流程
1. 加载驱动程序
2. 创建并获取数据库链接
3. 由链接得到statement对象
4. 通过statement执行sql语句，获得结果
5. 对结果进行解析
6. 依次释放资源

<!-- more -->

## 一个代码模板
```Java
import java.sql.*;

public class JDBCExample {
     static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";  
     static final String DB_URL = "jdbc:mysql://localhost:3306/test";

     static final String USER = "root";
     static final String PASS = "root123";

     public static void main(String[] args) {
         Connection conn = null;
         Statement stmt = null;
         try{
            Class.forName("com.mysql.jdbc.Driver");

            conn = DriverManager.getConnection(DB_URL,USER,PASS);

            stmt = conn.createStatement();
            String = "SELECT id, name, age FROM Employees";
            ResultSet rs = stmt.executeQuery(sql);

            while(rs.next()){
               int id  = rs.getInt("id");
               String first = rs.getString("name");
               int last = rs.getInt("age");

               System.out.print("ID: " + id);
               System.out.print(", Name: " + name);
               System.out.print(", Age: " + age);
              }
            rs.close();
            stmt.close();
            conn.close();
         }catch(SQLException se){
            se.printStackTrace();
         }catch(Exception e){
            e.printStackTrace();
         }finally{
            try{
               if(stmt!=null)
                  stmt.close();
            }catch(SQLException se){
              se.printStackTrace();
            }
            try{
               if(conn!=null)
                  conn.close();
            }catch(SQLException se){
               se.printStackTrace();
            }
        }
    }
}
```

## 数据库驱动的类型
JDBC驱动有四种类型，JDBC-ODBC桥、本地API驱动、网络协议驱动和本地协议驱动。平时一般只会用到网络协议驱动。

### JDBC-ODBC桥
ODBC（Open Database Connectivity，开放数据库互连）是一种标准的数据库访问方式，和程序无关，只和数据库有关。由数据库厂商自己实现。
JDBC-ODBC桥是Java实现的标准API，它的作用是通过调用ODBC来操作数据库。
这种做法虽然也能实现Java对数据库进行操作，但是存在许多问题：
- 操作相当与中转了一次，大量数据操作时效率低
- 只能访问本地数据库，且要求本地数据库有安装ODBC驱动；不支持网络数据源

### 本地API驱动
这种方式类似于JDBC-ODBC桥的实现方式，只不过这次Java是直接调用数据库原生驱动，这些原生驱动也是由数据库厂商开发的。
- 不用通过ODBC做中介，效率提高
- 依旧不支持网络数据源，且必须本地数据库有对应驱动可用

### 网络协议驱动
网络协议驱动的出现解决操作网络数据源的问题。通过中间件的方式，Java程序将数据库访问请求发给中间件服务器，有服务器通过ODBC或原生数据驱动来对数据库进行操作。
- 解决操作网络数据源的问题
- 显而易见，效率低

### 本地协议驱动
本地驱动协议支持将Java对数据库操作直接转化为对数据库的操作，而且是通过socket连接直接与数据库进行通信。这类驱动是有数据库厂商通过Java语言开发的。
- 高效率
- 支持网络数据源

## 获得链接的几个API
```Java
getConnection(String url)
getConnection(String url, Properties prop)
getConnection(String url, String user, String password)

eg: getConnection("jdbc:oracle:thin:username/password@host:port:DBname");
```

各种数据库的驱动和链接url写法

| 数据库    | JDBC驱动     |url样式     |
| :------------- | :------------- |:------------- |
| MySQL       | com.mysql.jdbc.Driver  |jdbc:mysql://hostname:port/databaseName      |
|ORACLE       | oracle.jdbc.driver.OracleDriver  |jdbc:oracle:thin:@hostname:port:databaseName      |
| DB2       | COM.ibm.db2.jdbc.net.DB2Driver      |jdbc:db2:hostname:port/databaseName     |

## Statement、PrepareStatement、CallableStatement
3个都是接口，是依次继承的关系，CallableStatement extends PrepareStatement extends Statement。通过这3个接口对象的方法，可以发送 SQL 命令或 PL/SQL 命令到数据库，并从你的数据库接收数据。

三者在使用上是有些许区别的，使用最多的　PrepareStatement 。

| 接口 | 特性     |
| :------------- | :------------- |
| Statement   |  不能接收参数，SQL是被写死的      |
| PrepareStatement   | 可以接收参数，写SQL是使用问号 ？ 来代替参数，SQL可被复用       |
| CallableStatement  | 比较特殊，执行存储过程的statement对象，也可以接收参数，参数类型有多种，后单独讨论   |

常见的三种执行操作

|  方法 | 特性     |
| :------------- | :------------- |
| boolean execute(String SQL)      | 一般执行DDL语句，返回true or false       |
| int executeUpdate(String SQL)       | 一般执行INSERT，UPDATE 或 DELETE 语句，返回受影响的行数       |
| ResultSet executeQuery(String SQL)     |  一般执行SELECT 语句，返回查询到的结果集      |


### PrepareStatement赋值
该statement对象的SQL中使用 ？ 代替参数，可以通过 setXXX() 来注入参数，XXX表示的参数的Java类型，并不是sql数据类型，要注意。而且，注入顺序是从1开始的。

## 小结
多写写吧，自然就会了
