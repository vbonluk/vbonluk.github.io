---

layout: post
title: "Mac效率之iTerm与Finder这对好基友"
description: 
headline: 
modified: 2016-07-23
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

### 需求：

1. iTerm中打开到对应路径的Finder
2. Finder中打开到对应路径的iTerm

#### 一：iTerm中打开到对应路径的Finder

##### 以前的做法:

![](http://oapglm9vz.bkt.clouddn.com/1469241252.png )

cd到对应路径，并且pwd拿到当前路径，复制路径，再打开Finder，command+G，粘贴路径，Finder跳转到对应路径。共7步。

##### 现在的做法：

![](http://oapglm9vz.bkt.clouddn.com/1469241429.png )

cd到对应路径，直接输入

	open .

就可以在Finder中打开到对应路径了。共2步。

#### 二：Finder中打开到对应路径的iTerm

##### 以前的做法：

![](http://oapglm9vz.bkt.clouddn.com/1469241668.png )

一步步cd到对应的路径这样太慢了，我以前采取的是拖动文件夹到iTerm得到路径

![](http://oapglm9vz.bkt.clouddn.com/1469241668.png )![](http://oapglm9vz.bkt.clouddn.com/1469241747.png )

打开iTerm，拖动文件夹iTerm，共2步。

##### 现在的做法：

先配置好环境，使用Go2Shell这个工具。

下载：<http://zipzapmac.com/Go2Shell>

使用按住Command方式将Go2Shell拖拽到Finder工具栏 或者 在Alfed 直接输入Go2Shell即可。

![](http://oapglm9vz.bkt.clouddn.com/1469241897.png )

选择iTerm2打开，这样就配置好了。接着按下面的install Go2Shell from Finder就可以了，我这里的图片是已经安装好的，所以显示是unInstall。。。

我们打开Finder，发现有个小按钮：

![](http://oapglm9vz.bkt.clouddn.com/1469242019.png )

没错，这个就是一键打开到iTerm。共1步。

![](http://oapglm9vz.bkt.clouddn.com/1111.gif)

### Last

上面这两点虽然是小事情，但是却可以大大提高我们使用的效率。

原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
