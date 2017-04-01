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

## Spring Boot(参数)配置
0. 测试优先
    1. Spring的devtools的全局配置(~/.spring-boot-devtools.properties文件)(当使用了devtools时)
    2. Test类上通过@TestPropertySource声明的属性文件
    3. Test类上通过@SpringBootTest#properties声明的属性
<!-- more -->
1. 文本命令行参数
  > Java -jar app.jar --name="Spring" --server.port=9090

  参数用 --xxx=xxx 的形式传递。SpringApplication 会把这种 -- 开头的参数转化到 Spring 的 Environment 中。
  如果不想让命令行参数添加到Environment中, 可通过SpringApplication.setAddCommandLineProperties(false)设置。

2. SPRING_APPLICATION_JSON 参数
  几种方式：
  > SPRING_APPLICATION_JSON='{"foo":{"bar":"spam"}}' java -jar myapp.jar  // 环境变量形式。相当于在Spring的Environment中添加了foo.bar=spam
  > java -Dspring.application.json='{"foo":"bar"}' -jar myapp.jar   // 系统变量
  > java -jar myapp.jar --spring.application.json='{"foo":"bar"}'   // 命令行参数
  > java:comp/env/spring.application.json //JNDI方式提供

3. ServletConfig初始化参数

4. ServletContext初始化参数

5. 来自于java:comp/env的JNDI属性

6. Java系统属性
System.getProperties()

7. 操作系统环境变量

8. 通过 RandomValuePropertySource 生成的 random.* 属性

9. jar包外的profile配置文件(application-{profile}.properties和YAML配置)

10. jar包内的profile配置文件(application-{profile}.properties和YAML配置)

11. jar包外的应用程序配置文件(application.properties和YAML配置)

12. jar包内的应用程序配置文件(application.properties和YAML配置)
  打包后的配置文件，可以被包外的文件替代

13. 配置类(@Configuration类)上的通过@PropertySource注解声明的属性文件

14. 通过 SpringApplication.setDefaultProperties 声明的默认属性

### application.properties application.yml
classpath:/config/application.properties > classpath:/application.properties;即在classpath:/config/下的配置文件优先级比较高。

该配置文件名一般是固定的，可以通过 命令行或者系统属性或环境变量 中指定spring.config.name和spring.config.location环境属性。
> java -jar myproject.jar --spring.config.name=myproject
> java -jar myproject.jar --spring.config.location=classpath:/default.properties,classpath:/override.properties

若spring.config.location指定的是一个目录, 则应该以/结尾, 并且使用该目录下spring.config.name指定的配置文件

#### 随机变量
RandomValuePropertySource可以注入一些随机变量, 可产生integer, long, string, uuid等类型的随机值

> my.secret=${random.value}
> my.number=${random.int}
> my.bignumber=${random.long}
> my.number.less.than.ten=${random.int(10)}
> my.number.in.range=${random.int[1024,65536]}

#### 变量引用
> app.name=MyApp
> app.description=${app.name} is a Spring Boot application

### 多环境配置
- application-{profile}.properties的优先级要高于application.properties
- application-{profile}.properties 一般会设置 dev test prod 等环境，没有就默认会使用application-default.properties配置
- 在 application.properties 中通过属性 spring.profiles.active=profile 来指定
- 也可已通过 SpringApplication.setAdditionalProfiles(…) 的方式来指定

### 自定义配置
application.properties 的属性一般配置系统级的配置，所以一般自定义的配置不推荐在这里。

#### 自定义配置文件
db.properties, 通过@PropertySource加载配置文件, 然后通过@Value("${key:defaultVlaue}")的形式进行配置
```
db.driver=MySQL
db.username=username
db.password=123456
db.arr[0]=arr1
db.arr[1]=arr2
```
```
@Component
@PropertySource("db.properties")
public class DBConfig {
    @Value("${db.driver}")
    private String driver;

    @Value("${db.username}")
    private String username;

    @Value("${db.password}")
    private String password;

    @Value("${db.arr[0]}")
    private String table1;

    @Value("${db.arr[1]}")
    private String table2;
    ...
}
```

#### 类型安全的配置加载
> @ConfigurationProperties(prefix="xxx", locations = "classpath:aa/yy.properties")

1. 被注解的类需要是 Spring 的bean
2. 或者， 在 Application 类上注解 @EnableConfigurationProperties({XXConfig.class})
3. 支持 JSR-303 注解做数据校验

注：.yml 文件只支持该注解方式，不支持 @PropertySource

## 整合Mybatis
虽然Spring鼓励使用 Mybatis Annotation，但明显 xml 更好用，也更方便维护。

### 多数据源
