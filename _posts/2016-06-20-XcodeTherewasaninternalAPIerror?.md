---

layout: post
title: "Xcode There was an internal API error ?"
description: 
headline: 
modified: 2016-06-20
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

有的时候我们编译项目在真机的运行的时候(调试环境)Xcode会给我们显示这个：

![](/images/xcodeapierror/1.png)

无论怎么clean程序，重启Xcode，重启电脑删除APP还是这样的错误，

一脸懵逼有木有？

![](/images/xcodeapierror/6.gif)

其实这是由于编译的Product Name引起的！

我们可以通过修改Product Name来运行APP。

bulid settings  ->    packaging  -> product name

![](/images/xcodeapierror/3.png)

修改其中的product name为英文，去除中文和特殊符号！

![](/images/xcodeapierror/5.png)

另外我们还可以编译后在项目文件导航栏看到编译后的文件名

![](/images/xcodeapierror/4.png)

最后：

![](/images/xcodeapierror/2.png)

运行APP。cheer!

原创作品，欢迎转载，转载请声明出处：http://www.真无聊.com
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
