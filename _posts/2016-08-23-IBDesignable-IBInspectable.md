---

layout: post
title: "iOS开发效率之IBDesignable与IBInspectable神器!"
description: 
headline: 
modified: 2016-08-23
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

### 前言：

在iOS开发过程中，其实经常需要给一些view或者imageView，button等空间设置圆角，边框等基本需求。那么我们经常需要在代码上调用下面这几句代码：

	view.layer.cornerRadius = 8.0f;
    view.layer.masksToBounds = YES;
    view.layer.borderWidth = 1.0f;
    view.layer.borderColor = [UIColor yellowColor].CGColor;
 
其实说起来麻烦也不麻烦，但是就是耗费时间去写。写一两个view的还好。但是一直下来随着每次功能的开发都需要设置，我们量化一下所耗费的时间就会知道越来越多。那么有没有办法改善呢？

有的同学会说：可以设置成代码块呀。每次只需要修改相应的变量名就可以了呀。如下图：

![](http://oapglm9vz.bkt.clouddn.com/1471940419.png )

如果这样做，我觉得倒是可以的。但是还是多了需要修改变量名和每次都要硬编码到.m文件中。那么还有没有更好的办法呢？

答案是：有的。

![](http://oapglm9vz.bkt.clouddn.com/1471939696.png )

### 正文：

参考资料：

<http://blog.csdn.net/u010873087/article/details/48025197>

[https://mp.weixin.qq.com](https://mp.weixin.qq.com/s?__biz=MzAxMzE2Mjc2Ng==&mid=2652155135&idx=2&sn=aa368a47d56761d42114357e43287354&scene=1&srcid=0801vyJLoHVWqxPGGaQdKYBp&key=8dcebf9e179c9f3a63e1f6bebbde7c517820cd36e53caff62549f941b8cdb637366776026b146998d55fae722d21c51d&ascene=0&uin=Mjc5ODQzMDkyMQ%3D%3D&devicetype=iMac12%2C1+OSX+OSX+10.11.5+build(15F34)&version=11020201&pass_ticket=Ai0xTicATNFGC%2FkJF7cWinDxAUCVNo3BSNNL9cZupFCNYd%2BcpJRCLsf6%2Fy7noRa0)

<https://segmentfault.com/a/1190000003703119>

<http://www.cocoachina.com/ios/20150227/11202.html>

新建一个View类继承于UIView

![](http://oapglm9vz.bkt.clouddn.com/1471941858.png )

在类中新增3个属性，分别是：圆角大小、边框大小，边框颜色

#### OC：

.h文件

使用IBInspectable来修饰变量。

![](http://oapglm9vz.bkt.clouddn.com/1471941993.png )

.m文件

使用IB_DESIGNABLE标识

![](http://oapglm9vz.bkt.clouddn.com/1471942096.png )

#### SWIFT:

使用@IBInspectable来修饰变量。

![](http://oapglm9vz.bkt.clouddn.com/1471942021.png )

这时，我们就可以在xib或者storyboard中新建一个view测试一下。这里我们使用storyboard实验。

![](http://oapglm9vz.bkt.clouddn.com/1471942186.png )

我们打开第3项设置父类。

![](http://oapglm9vz.bkt.clouddn.com/1471942219.png )

等待xcode进度条走完

![](http://oapglm9vz.bkt.clouddn.com/1471942294.png )

这时候发现下放多了3个属性选项。

![](http://oapglm9vz.bkt.clouddn.com/1471942328.png )

但是我们想更直观一点来设置这些属性，我们打开第4个属性：

![](http://oapglm9vz.bkt.clouddn.com/1471942387.png )

神奇的一幕出现了有木有？

终于可以实时设置圆角以及边框和边框颜色啦！对于使用xib以及storyboard的同学来说必然是一大爽快！

再也不用编写重复代码了！而且关键是可以实时看到效果！很爽有木有？

![](http://oapglm9vz.bkt.clouddn.com/222.gif)

觉得有用的同学可以直接去下面拿工具来使用。

【【工具源码】】在这里，欢迎star此项目：<https://github.com/vbonluk/IBDesignable-Help>

原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
