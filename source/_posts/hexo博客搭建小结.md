---
title: hexo博客搭建小结
date: 2016-12-11 19:23:52
category: hexo
tags:
  - hexo
  - next
---
惨！惨！惨！前段时间把之前用的台式机出售了，结果发动了程序猿的特殊技能，忘记吃药了，清盘时忘记把hexo源文件保存下来，只能对着屏幕，“靠！”，想砸屏幕。。。

这次重新搭建Hexo博客，使用经典组合Hexo+Next，简单记录一下过程。并且，并且，最重要的是我研究了一下Hexo源码保存的方法，以后再也不用担心源文件丢失了。哭～

## 准备工作

Hexo是基于Node.js写出来的的，通过解析Markdown文件，利用主题生成静态文件，最终得到一个静态网站。

所以呢，搭建一个Hexo博客，需要一下工具：
  - Node.js，就不解释了
  - Git，前期主要是down东西，之后作博客发布用
  - GitHub账户，推荐使用，国内有coding也很好，速度还快，github只是个人取向
  - Atom，推荐使用，GitHub良心出品，写Markdown很方便

有了这些东西，就可以愉快的搭建Hexo博客了。

<!-- more -->

## 在github新建项目

这个项目用来存放Hexo生成的静态博客文件。由于github有Git Pages，它可以将html文件显示为网页，我们由特殊网址进入，就可以查看到我们写的博文了。

建立项目为 github用户名.github.io ，就这点，没有其他要求了。

## 安装Hexo

> $ npm install -g hexo-cli

## 建站

> $ hexo init <folder>
> $ cd <folder>
> $ npm install

## 更换主题

hexo默认使用Landspace主题，不是很好用，也不够好看，还是支持国产Next吧！

> $ cd your-hexo-site
> $ git clone https://github.com/iissnan/hexo-theme-next themes/next

这个路径下载Next最新版本，Next在Github维护，有兴趣可以看看。

下载完成后，将站根目录下的 _config.yml 文件中的 theme 字段值改为 “next”。

## Hexo使用和Next配置

这个。。。这个。。还是看官方文档吧。真没啥好说的！

[Hexo使用指南](https://hexo.io/zh-cn/docs/)

[Next使用指南](http://theme-next.iissnan.com/)

看代码，页面宽度很重要，哈哈。[调整桌面显示页面宽度](https://github.com/iissnan/hexo-theme-next/issues/759#issuecomment-202242848)

## Hexo站源码保存方案

关于站源码保存的解决方案有很多种，这里介绍最保险的做法，就是将站源码也保存到github上。

为了节省操作，利用node.js的shelljs模块，将hexo发布操作作为出发机制，发布时完成站源码push到github上去。

#### 将站源码加入Git仓库

> $ git remote add origin git@github.com:XXX/XXX.git
> $ git pull origin master

为了加快push操作，修改.gitignore文件（如果没有请手动创建一个），在里面加入*.log 和 public/ 以及.deploy*/。因为每次执行hexo generate命令时，上述目录都会被重写更新，可忽略这两个目录下的文件。

#### 安装shelljs模块

> $ npm install --save shelljs

#### 脚本文件

在站源码目录下，新建一个scripts目录，在scripts目录下新建任意名称js文件。js文件内容如下：

```JavaScript
require('shelljs/global');
try {
	hexo.on('deployAfter', function() {//当deploy完成后执行备份
		run();
	});
} catch (e) {
	console.log("产生了一个错误<(￣3￣)> !，错误详情为：" + e.toString());
}
function run() {
	if (!which('git')) {
		echo('Sorry, this script requires git');
		exit(1);
	} else {
		echo("======================Auto Backup Begin===========================");
		cd('/home/miaomu/hexo_blog');    //此处修改为Hexo根目录路径
		if (exec('git add --all').code !== 0) {
			echo('Error: Git add failed');
			exit(1);
		}
		if (exec('git commit -am "Form auto backup script\'s commit"').code !== 0) {
			echo('Error: Git commit failed');
			exit(1);
		}
		if (exec('git push origin master').code !== 0) {
			echo('Error: Git push failed');
			exit(1);
		}
		echo("==================Auto Backup Complete============================")
	}
}
```
注意修改到自己的站源码目录，以及注意git分支名（这个一般都不用改）。

这些工作完成后，以后deploy的时候，站源码文件也上传到github仓库了。不过，我没有使用ssh，还需要手输入用户名和密码 =_=。并非原创，[解决方案链接](http://zhujiegao.com/2015/12/06/automatic-backup/)。


## 一些想说的话

写博客的工具很多，但每个人总想有自己的博客空间，Hexo提供了一个很不错的选择。然后，既然使用了人家免费的资源，就要好好写点东西。天朝难得能上去的网站，别浪费了！写博客贵在坚持，好好加油吧！
