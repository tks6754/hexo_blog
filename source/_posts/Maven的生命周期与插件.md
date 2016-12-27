---
title: Maven的生命周期与插件
date: 2016-12-26 22:37:52
category:
    - maven
tags:
    - maven
    - plugin
    - 生命周期
---
Maven使用过程中常常用到这样一些指令 mvn install、mvn clean等，这些install或clean都是Maven生命周期（Build Lifecycle）的阶段（phase）。Maven的生命周期是一个抽象的概念，类似于网络协议的7层网络协议，真正实现功能的是TCP/IP。在maven中，生命周期分为很多阶段，但都是抽象出来的概念，具体的实现通过maven插件来完成的。

## maven中的生命周期
完整的项目build过程需要很多步骤，基本的包括clean、compile、package、deploy。maven把这些步骤总结归纳为3个生命周期：clean生命周期、default生命周期、site生命周期

### clean生命周期
| 顺序（步骤）    | 阶段（phase）  | 具体内容    |默认绑定插件    |
| :------------- | :------------- |:------------- |:------------- |
| 步骤 一      |  pre-clean  | 执行clean前需要完成的工作 |     |
| 步骤 二      |  clean      | 清理上一次构建的所有文件（一般就是清空target） | maven-clean-plugin:clean  |
| 步骤 三      |  post-clean | 执行清理后需要执行的工作  |   .  |  

<!-- more -->

### default生命周期
| 顺序（步骤）    | 阶段（phase）  | 具体内容    | 默认绑定插件    |
| :------------- | :------------- |:------------- |:------------- |
| 步骤 一    | validate   |  验证     |  |
| 步骤 二    | initialize   |  初始化配置   |  |
| 步骤 三    | generate-sources   |  生成源代码编译目录  |  |
| 步骤 四    | process-sources   | 处理项目主资源文件，复制资源文件到outputclasspath  | maven-resources-plugin:resources  |
| 步骤 五    | generate-resources   |  生成资源目录     |   |
| 步骤 六    | process-resources    |  处理资源文件     ||
| 步骤 七    | complie    |  编译源代码     | maven-compiler-pugin:compile  |
| 步骤 八    | process-classes    |  处理编译后文件   |  |
| 步骤 九    | generate-test-sources    | 生成测试目录  |    |
| 步骤 十    | process-test-sources    |  处理项目测试资源文件，复制测试资源文件到outputclasspath  |       |
| 步骤 十一    | generate-test-resources    | 生成测试资源文件      |   |
| 步骤 十二    | process-test-resources    | 处理测试资源文件    | maven-resources-plugin:testResources  |
| 步骤 十三    | test-compile    | 编译测试代码      |  maven-compiler-plugin:testCompile  |
| 步骤 十四    | process-test-classes    |  处理测试代码     |    |
| 步骤 十五    | test    | 单元测试运行测试代码      | maven-surefire-plugin:testCompile  |
| 步骤 十六    | prepare-package    |  打包前的准备    |  |
| 步骤 十七    | package    | 将编译好的代码打包成为jar或者war或者ear等等    |  maven-jar-plugin:jar  |
| 步骤 十八    | pre-integration-test    |  准备整体测试    |  |
| 步骤 十九    | integration-test    |  整体测试     |   |
| 步骤 二十    | post-integration-test    | 为整体测试收尾   |   |  
| 步骤 二十一    | verify    |  验证     |  |
| 步骤 二十二    | install    |  安装到本地Maven库     |  maven-install-plugin:install  |
| 步骤 二十三    | deploy    | 将最终包部署到远程Maven仓库    |  maven-deploy-plugin:deploy  |

### site生命周期
| 顺序（步骤）    | 阶段（phase）  | 具体内容    ||
| :------------- | :------------- |:------------- |
| 步骤 一      |  pre-site  | 站点生成准备     |  |
| 步骤 二      |  site      | 生成站点和文档  | maven-site-plugin:site  |
| 步骤 三      |  post-site | 站点生成收尾  |   |
| 步骤 三      |  site-deploy | 发布站点  | maven-site-plugin:deploy  |

### 生命周期与插件
- 每个生命周期内部阶段是存在依赖关系的，后面的步骤依赖前面的步骤
- 对于3个生命周期而言，三者是相互独立的。即，指定运行某个生命周期的阶段，不会影响到其他周期内的阶段。
- 插件的目标：每一个插件都不是只实现一个功能，一个常见可能会支持多个功能，这样的单独一个功能称为插件的目标。
- 生命周期的阶段绑定到插件，通过插件来实现具体功能,具体是绑定到插件的目标。
- Maven的生命周期和插件是在项目中绑定的，当然，maven已经默认绑定了一些基础的插件（上表）。注意，并非所有生命周期阶段都有绑定到插件。另外，还可以通过插件配置扩展功能（build的extensions）。

## Maven指令

















































## 插件配置
```

```
