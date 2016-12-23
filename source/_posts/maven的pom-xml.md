---
title: maven的pom.xml
date: 2016-12-23 21:13:29
category:
    - maven
tags:
    - maven
    - pom
---
Maven是Java开发中最最常用的项目构建工具了，然而。。然而，自己还没有能好好的使用它，所以，必须要抽点时间好好的研究一下。

Maven中最重要的，也是maven的根本，就是pom文件了。pom文件是maven构建项目的配置文件，根据pom文件，可以实现很多功能。例如，项目唯一指定，依赖管理，项目编译，项目的发布管理，还能实现项目自动化管理。

<!-- more -->

## 一个比较完整的pom文件
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.miao</groupId>
	<artifactId>project</artifactId>
	<packaging>pom</packaging>
	<version>1.0-SNAPSHOT</version>
	<modules>
		<module>web</module>
		<module>biz</module>
		<module>dal</module>
		<module>client</module>
	</modules>
  <properties>
		<veloctiy.version>1.7</veloctiy.version>
		<velocity.tools.version>2.0</velocity.tools.version>
		<!-- module -->
		<biz.version>1.0-SNAPSHOT</biz.version>
		<web.version>1.0-SNAPSHOT</web.version>
		<!-- spring -->
		<spring.version>4.1.6.RELEASE</spring.version>
		<!-- quartz -->
		<quartz.version>2.2.2</quartz.version>
		<!-- pageHelper -->
		<pageHelper.version>3.4.1</pageHelper.version>
		<!-- dao -->
		<mapper.version>3.2.2</mapper.version>
		<!-- cglib -->
		<cglib.version>2.2.2</cglib.version>
		<!-- persistence-api -->
		<persistence-api.version>1.0</persistence-api.version>
		<!-- shiro -->
		<shiro.version>1.2.4</shiro.version>
		<!-- mysql -->
		<mysql.connection.version>5.1.35</mysql.connection.version>
		<!-- druid -->
		<druid.version>1.0.14</druid.version>
		<!-- servlet 3.1.0 -->
		<servlet.version>2.5</servlet.version>
		<jstl.version>1.1.2</jstl.version>
		<!-- jsp -->
		<jsp.version>2.2</jsp.version>
		<!-- velocity -->
		<veloctiy.version>1.7</veloctiy.version>
		<velocity.tools.version>2.0</velocity.tools.version>
		<!-- log -->
		<log4j.version>1.2.17</log4j.version>
		<slf4j.version>1.7.12</slf4j.version>
		<!-- fastjson -->
		<fastjson.version>1.2.6</fastjson.version>
		<net.json.version>2.4</net.json.version>
		<!-- jackson -->
		<jackson.version>2.6.0</jackson.version>
		<!-- jodaTime -->
		<jodaTime.version>2.3</jodaTime.version>
		<!-- test -->
		<junit.version>4.12</junit.version>
		<mockito.version>1.10.19</mockito.version>
		<!-- maven -->
		<maven.compiler.version>3.3</maven.compiler.version>
		<!-- JDK -->
		<java.version>1.7</java.version>
		<!-- server -->
		<maven.tomcat7.version>2.2</maven.tomcat7.version>
		<maven.jetty.version>8.1.16.v20140903</maven.jetty.version>
		<!-- aliyun start -->
		<aliyun.sdk.version>2.0.0-rc1</aliyun.sdk.version>
		<jedis.version>2.7.3</jedis.version>
		<!-- <jedis.version>2.1.0</jedis.version> -->
		<aliyun.api.version>1.0.0</aliyun.api.version>
		<!-- aliyun end -->
		<!-- aspectj -->
		<aopalliance.version>1.0</aopalliance.version>
		<aspectjrt.version>1.8.7</aspectjrt.version>
		<!-- aspectj -->
		<commons.lang.version>2.5</commons.lang.version>
		<httpclient.version>4.4.1</httpclient.version>
		<poi.version>3.12</poi.version>
		<commons.pool.version>1.6</commons.pool.version>
		<!-- ueditor -->
		<codec.version>1.9</codec.version>
		<fileupload.version>1.3.1</fileupload.version>
		<io.version>2.4</io.version>
		<json.version>20140107</json.version>
		<ueditor.version>1.1.1</ueditor.version>
		<velocity.version>1.4</velocity.version>
		<velocity.tools.version>2.0</velocity.tools.version>
		<client.version>1.0.0-SNAPSHOT</client.version>
	</properties>
	<dependencyManagement>
		<dependencies>
			<!-- Spring start -->
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-core</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context</artifactId>
				<version>${spring.version}</version>
				<exclusions>
					<exclusion>
						<artifactId>commons-logging</artifactId>
						<groupId>commons-logging</groupId>
					</exclusion>
				</exclusions>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-context-support</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-beans</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-web</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-webmvc</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-tx</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-aop</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-aspects</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-expression</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework</groupId>
				<artifactId>spring-jdbc</artifactId>
				<version>${spring.version}</version>
			</dependency>
			<dependency>
				<groupId>org.springframework.ws</groupId>
				<artifactId>spring-ws-core</artifactId>
				<version>2.0.0.RELEASE</version>
			</dependency>
			<!-- Spring end -->
			<dependency>
				<groupId>org.apache.cxf</groupId>
				<artifactId>cxf-bundle</artifactId>
				<version>2.7.18</version>
				<exclusions>
					<exclusion>
						<groupId>org.apache.geronimo.specs</groupId>
						<artifactId>geronimo-servlet_3.0_spec</artifactId>
					</exclusion>
					<exclusion>
						<groupId>org.springframework</groupId>
						<artifactId>spring-asm</artifactId>
					</exclusion>
				</exclusions>
			</dependency>
		</dependencies>
	</dependencyManagement>
	<!-- maven插件配置 -->
	<build>
		<finalName>frontgateway</finalName>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>2.3.1</version>
				<dependencies>
					<dependency>
						<groupId>org.codehaus.plexus</groupId>
						<artifactId>plexus-compiler-javac</artifactId>
						<version>1.8.1</version>
					</dependency>
				</dependencies>
				<configuration>
					<source>1.7</source>
					<target>1.7</target>
					<meminitial>128m</meminitial>
					<maxmem>512m</maxmem>
					<encoding>UTF-8</encoding>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat7-maven-plugin</artifactId>
				<version>2.1</version>
				<configuration>
					<port>8081</port>
					<url>http://121.41.45.54:8080/manager/text</url>
					<server>tomcat</server>
					<update>true</update>
					<username>tomcat</username>
					<password>tomcat123#</password>
				</configuration>
			</plugin>
		</plugins>
	</build>
	<distributionManagement>
		<repository>
			<id>snapshots</id>
			<name>Snapshots</name>
			<url>http://maven.yunatutech.com:8081/nexus/content/repositories/snapshots/</url>
		</repository>
	</distributionManagement>
</project>
```

## pom文件解读

### 声明规范
- xml头，指定xml文档的版本和编码方式；
- project元素，是所有pom.xml的根元素，声明了pom相关命名空间及xsd元素，并非必须，用于方便IDE编辑pom；
- modelVersion，指定pom模型版本，固定为4.0.0；

### groupId、artifactId、version定义一个项目的基本坐标，Maven通过这三个元素可以找到任何项目
- groupId：项目包名——com.mycom.myapp，一般公司逆序加项目名，与包的命名一样
- artifactId：模块名，当前项目组中唯一ID，一个groupId可以有多个子项目（模块）
- version：版本
- packaging：所有带有子模块的项目的packaging都为pom，为固定值；一般还有jar、war、ear；默认值jar

### modules、module模块配置
- modules：子模块列表
- module：模块（有时称作子项目）被构建成项目的一部分。列出的每个模块元素是指向该模块的目录的相对路径

### dependencyManagement、dependencies、dependency项目依赖管理
- dependencyManagement：依赖管理
- dependencies：依赖列表，声明用到的第三方包
- dependency：依赖，每个dependency中是依赖项目的GAV属性值
- properties：键对值，在整个pom中起作用，相当于properties配置文件中配置

### build、finalName、plugins、plugin、dependencies、configuration编译设置和插件管理
- build：项目构建信息
- defaultGoal：预定义执行的目标或者阶段，必须和命令行的参数相同。例如：defaultGoal配置clean install ，在命令行输入mvn时会自动拼接成mvn clean install。
- finalName：build目标文件的文件名，默认情况下为${artifactId}-${version}；生成文件的样式
- directory：编译输出的目录；默认值${basedir}/target(不建议修改)
- filters：过滤列表
- filter：定义*.properties文件，包含一个properties列表，该列表会应用到支持filter的resources中。也就是说，定义在filter的文件中的”name=value”值对会在build时代，替${name}值应用到resources中。Maven的默认filter文件夹是${basedir}/src/main/filters/。
- sourceDirectory：
- resources：资源路径列表
- resource：资源路径

#### plugins:插件列表
- plugin:插件，包含基本的GAV标准坐标信息
- extensions：true/false，是否加载plugin的extensions，默认为false
- inherited：true/false，当前plugin是否应用到该POM的孩子POM，默认true
- dependencies:作为plugin的依赖
- configuration:一些配置参数信息

### distributionManagement发布管理
- distributionManagement:发布管理配置
- repository:项目
- id:id
- name：项目名
- url：地址

### parent父模块
- parent：父模块
- groupId：
- artifactId：
- version：
- relativePath：关联父模块pom路径

### profiles、profile
- profiles：
- profile：
- id：
- properties：
- env：
- activation：
- activeByDefault：

### 剩余问题
- filter不是特别清楚
- profile问题
