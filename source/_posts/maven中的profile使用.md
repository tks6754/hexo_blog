---
title: maven中的profile使用
date: 2016-12-25 21:25:29
category:
    - maven
tags:
    - maven
    - profile
    - pom
---
maven的配置中有两个很神奇的东西，一个是build，另一个就是profile。

profile的中文有“轮廓”的意思，maven中非常好的展现了这个“词”的意思。各种信息都可以放入profile这个“轮廓”中。如果某个profile被激活，那么在profile中配置的信息就会同原先pom中配置信息合并，并覆盖原有的相同配置信息。利用这一点，可以做很多事情，最常见的就是项目多场景启动配置。

## profile的配置项
profile可以在settings.xml和pom.xml中配置，但settings.xml中profile内可配置的信息相对较少。
- settings.xml中profile可以配置：repositories、pluginRepositories、properties
- pom中profile可以配置：repositories、pluginRepositories、properties、dependencyManagement、dependencies、distributionManagement、plugins、modules、reporting，以及build下的defaultGoal、resources、testResources、finalName

其实，还有一个地方会出现profile配置，就是在maven2的年代，可以有profile.xml文件，支持外部引入profile。到maven3的年代，这个已经被取消了，可以见此类profile配置到settings中。

```
pom.xml
|-project
     |- ...
     |-profiles
     |      |- ...
     |      |-profile
     |             |-id
     |             |-activation
     |             |     |- .激活方式配置.
     |             |- .内部其他信息嵌套配置.
     |- ...
```
配置项说明

| 配置项          | 是否必配        |     含义        |
| :------------- | :------------- | :------------- |
| id | 是 | 该profile的唯一id |
| activation | 否 | 激活方式 |
| 其他可配信息 | 是 | 根据profile位置不同，支持不同的配置 |

<!-- more -->

## profile的激活方式

### 显式激活
通过-P参数来显示激活，激活多个profile时可以用 逗号 分隔开，排除使用感叹号。
> mvn -U clean package -Ptest,local,!ignore

一般还会见到跳过默认test的profile
> -Dmaven.test.skip=true

IDEA中在MavenProject里可以直接勾选profile，也很好用

### 隐式激活
隐式激活信息配置在activation中，有多种方式可选。并且这些方式还可以组合使用，很强大。

#### 默认激活
```
<!-- pom中默认激活 -->
<profiles>
  ...
  <profile>
    <activation>
        <activeByDefault>true</activeByDefault>
    </activation>
    ...
  </profile>
</profiles>

<!-- settings中维护profile默认激活列表 -->
<activeProfiles>
    <activeProfile>artifactory</activeProfile>
</activeProfiles>
```

#### 根据操作系统类型
```
<!-- pom中根据操作系统激活 -->
<profiles>
  ...
  <profile>
    <activation>
        <os>
            <!-- 不必指定所有信息 -->
            <name>linux</name>
            <family>unix</family>
            <arch>amd64</arch>
            <version>3.19.0-30-generic</version>
        </os>
    </activation>
    ...
  </profile>
</profiles>
```

#### 根据JDK版本激活
```
<!-- pom中根据jdk版本激活，可配置多个 -->
<profiles>
  ...
  <profile>
    <activation>
        <os>
            <jdk>1.8</jdk>
        </os>
    </activation>
    ...
  </profile>
</profiles>
```
多个形式为：[1.6,1.8)

#### 根据系统属性
```
<!-- pom中根据系统属性激活，也可只指定name，不指定对应的值 -->
<profiles>
  ...
  <profile>
    <activation>
          <property>  
               <name>hello</name>  
               <value>world</value>  
          </property>
    </activation>
    ...
  </profile>
</profiles>
```
> mvn package –Dhello=world

#### 根据文件是否存在
```
<!-- pom中根据系统属性激活，也可只指定name，不指定对应的值 -->
<profiles>
  ...
  <profile>
    <activation>
        <file>  
             <exists>target</exists>  
        </file>
    </activation>
    ...
  </profile>
  <profile>
    <activation>
        <file>  
             <missing>target</missing>
        </file>
    </activation>
    ...
  </profile>
</profiles>
```

### 查看profile激活状态
> mvn help:active-profiles
