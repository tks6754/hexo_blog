---
title: Maven的仓库
date: 2016-12-25 02:06:17
category:
    - maven
tags:
    - maven
    - maven仓库
---
## 仓库
Maven中通过配置依赖来向模块中加入项目依赖，那Maven真正获取依赖插件和构建（通常就是指jar包）的地方就是仓库，就如名字一样，仓库就是存放插件和构建的地方。

## 仓库的分类
maven的仓库可以分为：本地仓库和远程仓库。

### 本地仓库
本地仓库就是个人计算机存放从远程仓库下载下来构建的地方，也就是一个目录，默认位置是 ${user.home}/.m2/repository。

本地仓库目录是可以修改的：
1. 类似于默认目录，一般新建目录../.m2/repository
2. 修改maven安装目录下的./conf/setting.xml 文件
  ```
    <settings>
        ...
        <localRepository>path/to/repository</localRepository>
        ...
    </settings>
  ```
3. 将maven安装目录下的./conf/settings.xml复制一份到修改的./.m2目录下，作为用户的配置文件

<!-- more -->

### 远程仓库
远程仓库也就是远端网络存放构件的地方，一般有公开的仓库和私服仓库的分法。公开的仓库就是大家都可以用的，比如中央仓库和国内使用较多的ali仓库；私服仓库一般是指公司内部使用的仓库，一般会使用开源项目nexus来搭建公司仓库。

同样，远程仓库的地址也是可以修改的，根据配置地方（修改的配置文件）不同，起到的作用范围也不同：
- 项目pom.xml文件：只对当前项目起作用
- ./.m2/settings.xml：只对当前系统用户的所有maven项目起作用
- 安装目录./conf/settings.xml：对所有maven项目起作用，切换系统用户后仍起作用

#### 在pom文件中配置远程仓库地址
网络上一个oschina仓库配置例子
```
<repositories>
    <repository>
        <id>maven.oschia.net</id>
        <name>oschina maven repository</name>
        <url>http://maven.oschina.net/content/groups/public/</url>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>daily</updatePolicy>
            <checksumPolicy>ignore</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>true</enabled>
            <updatePolicy>daily</updatePolicy>
            <checksumPolicy>ignore</checksumPolicy>
        </snapshots>
        <layout>default</layout>
    </repository>
</repositories>
<pluginRepositories>
    <pluginRepository>
        <id>maven.oschina.net</id>
        <name>oschina maven repository</name>
        <url>http://maven.oschina.net/content/groups/public/</url>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>daily</updatePolicy>
            <checksumPolicy>ignore</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
        <layout>default</layout>
    </pluginRepository>
</pluginRepositories>
```
可以看出，涉及普通构件和插件构件的地址配置
```
pom.xml
|-project
     |- ...
     |-repositories
     |      |- ...
     |      |-repository
     |             |-id
     |             |-name
     |             |-url
     |             |-releases
     |             |     |-enabled
     |             |     |-updatePolicy    
     |             |     |-checksumPolicy
     |             |-snapshots
     |             |     |-enabled
     |             |     |-updatePolicy
     |             |     |-checksumPolicy
     |             |-layout
     |- ...
     |-pluginRepositories
     |      |- ...
     |      |-pluginRepository
     |             |-id
     |             |-name
     |             |-url
     |             |-releases
     |             |     |-enabled
     |             |     |-updatePolicy    
     |             |     |-checksumPolicy
     |             |-snapshots
     |             |     |-enabled
     |             |     |-updatePolicy
     |             |     |-checksumPolicy
     |             |-layout
     |
     |- ...
```
配置项说明

| 配置项          | 是否必配        |     含义        |
| :------------- | :------------- | :------------- |
| id | 是 | 每个repository或pluginRepository需要指定唯一id，id相同时，后面的配置会覆盖前面配置 |
| name | 否 | 名字，描述作用 |
| url | 否 | 远程仓库地址 |
| releases | 是 | release版本构件配置 |
| enabled | 是 | 是否启用配置。true或flase |
| updatePolicy | 否 | 更新频率。never-从不，always-每次构建都检查，daily-每天（默认），inerval：x-每隔x分钟 |
| checksumPolicy | 否 | 配置文件校验和策略，远程仓库的每一个构件都会有一个校验和文件，在下载时需要检查校验和。warn-校验失败输出警告（默认），fail-校验失败构建失败，ignore-忽略 |
| snapshots | 是 | snapshot版本构件配置。具体配置项同releases |
| layout | 是 | 布局配置。默认为default |

#### 在settings.xml文件配置远程仓库
设置远程仓库地址的方法有两种：
- 由于settings.xml不能直接支持repositories和pluginRepositories，可以使用profile来辅助配置，并设置activeProfile永久激活。这里就不做细述。
- 通过mirror来配置远程仓库地址

```
<settings>
...
    <profiles>
        <profile>
            <id>profile-default</id>
            <repositories>
                <repository>
                    <id>central</id>
                    <url>http://central</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
                <repository>
                    <id>repo-osc-thirdparty</id>
                    <url>http://maven.oschina.net/content/repositories/thirdparty/</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>central</id>
                    <url>http://central</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
    </profiles>

    <activeProfiles>
        <activeProfile>profile-default</activeProfile>
        <!--<activeProfile>profile-iss</activeProfile>-->
    </activeProfiles>
...
</settings>
```

##### 镜像
镜像只能在settings.xml文件中配置，一个常用的ali镜像
```
<settings>  
...  
    <mirrors>
      <mirror>
        <id>alimaven</id>
        <name>aliyun maven</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
        <mirrorOf>central</mirrorOf>        
      </mirror>
    </mirrors>
...  
</settings>  
```
镜像原理：所谓镜像，其实类似与远程仓库访问的拦截器。当请求远程仓库的某个依赖，首先需要经过镜像，如果镜像地址的仓库中也存在请求的构件，那么就直接从镜像地址仓库下载，镜像地址仓库没有的，才进入正常的远程仓库。
```
settings.xml
|-settings
    |- ...
    |-mirrors
    |    |- ...
    |    |-mirror
    |        |-id
    |        |-name
    |        |-url
    |        |-mirrorOf
    |- ...
```
配置项说明

| 配置项          | 是否必配        |     含义        |
| :------------- | :------------- | :------------- |
| mirror  | 是  | 一个镜像  |
| id  | 是  |  镜像id  |
| name  | 是  | 名字，一个简单镜像描述  |
| url  | 是  | 镜像地址  |
| mirrorOf | 是  | 需要拦截的远程仓库id  |

mirrorOf的配置方式：
```
 <mirrorOf>*</mirrorOf> ： 匹配所有远程仓库
 <mirrorOf>repo1,repo2</mirrorOf> ： 匹配仓库repo1和repo2，使用逗号分隔多个远程仓库
 <mirrorOf>*,!repo1</miiroOf> ： 匹配所有远程仓库，repo1除外，使用感叹号将仓库从匹配中排除。
```

## 私服仓库
私服仓库作为组织内部使用的远程仓库，通常会架设在局域网内的仓库服务，私服一般被配置为互联网远程仓库的镜像，供局域网内的Maven用户使用。

- 下载：当Maven需要下载构件的时候，先向私服请求，如果私服上不存在该构件，则从外部的远程仓库下载，同时缓存在私服之上，然后为Maven下载请求提供下载服务。
- 部署：对于自定义或第三方的jar可以从本地上传到私服，供局域网内其他maven用户使用。

### 仓库认证
对于一般的公共远程仓库是不需要认证的，而私服很可能会设置身份认证。管理员会给maven仓库设置用户名和密码，本地使用需要在settings.xml文件中配置认证信息
```
<settings>
...
    <servers>
        <server>
            <id>mapping repository id</id>
            <username></username>
            <password></password>
        </server>
    </servers>
...
</settings>
```
配置对应的仓库id的访问用户名和密码。
```
settings.xml
|-settings
    |- ...
    |-servers
    |    |- ...
    |    |-server
    |        |-id
    |        |-username
    |        |-password
    |- ...

```
配置项目说明

| 配置项          | 是否必配        |     含义        |
| :------------- | :------------- | :------------- |
| id  | 是  | 对应仓库id  |
| username | 是 | 访问远程仓库的用户名|
| password| 是 | 访问远程仓库的密码|

### 部署构件
用户可以从远程仓库下载构件，当然反过来也可将自己制作的构件分享出去，部署到远程仓库，一般只能部署到私服上。可以在pom.xml中配置。
```
<project>    
  ...    
  <distributionManagement>    
    <repository>    
      <id>nexus-releases</id>    
      <name>Nexus Release Repository</name>    
      <url>http://127.0.0.1:8080/nexus/content/repositories/releases/</url>    
    </repository>    
    <snapshotRepository>    
      <id>nexus-snapshots</id>    
      <name>Nexus Snapshot Repository</name>    
      <url>http://127.0.0.1:8080/nexus/content/repositories/snapshots/</url>    
    </snapshotRepository>    
  </distributionManagement>    
  ...    
</project>  
```
mvn install 会将项目生成的构件安装到本地Maven仓库，mvn deploy 用来将项目生成的构件分发到远程Maven仓库。注意了，发布构件一般是需要认证的！

Maven区别对待release版本的构件和snapshot版本的构件，Maven会根据你项目的版本来判断将构件分发到哪个仓库。
- snapshot为开发过程中的版本，实时，但不稳定
- release版本则比较稳定
```
pom.xml
|-project
     |- ...
     |-distributionManagement
     |      |- ...
     |      |-repository
     |      |      |-id
     |      |      |-name
     |      |      |-url
     |      |- ...
     |      |-snapshotRepository
     |             |-id
     |             |-name
     |             |-url
     |- ...
```

配置项说明

| 配置项          | 是否必配        |     含义        |
| :------------- | :------------- | :------------- |
| repository  | 是  | release版本仓库  |
| id  | 是  |  仓库id  |
| name  | 是  | 名字，一个简单的仓库描述  |
| url| 是 | 要部署构件的目标仓库地址 |
| snapshotRepository | 是 | snapshot版本仓库 |

## 小结
哈！结！我去，中二了。。

讨论了很多，仓库分本地仓库和远程仓库。主要还是一些常见仓库配置，以及了解部署构件的配置。然后，然后私服的搭建。。。恩。。。以后在研究吧。
