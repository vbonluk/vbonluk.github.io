---

layout: post
title: "iOS runtime实现应用内多语言切换，无需重启APP，xib，storyboard实时更新"
description: 
headline: 
modified: 2017-02-05
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

参考链接：

<http://blog.csdn.net/u010884518/article/details/50482241>

<http://www.2cto.com/kf/201510/444933.html>

<http://www.jianshu.com/p/627da5a9992c>

<http://blog.csdn.net/jackjia2015/article/details/51554375>

<http://www.jianshu.com/p/843a24a5508b>

<http://stackoverflow.com/questions/1669645/how-to-force-nslocalizedstring-to-use-a-specific-language/20257557#20257557>

### <http://devma.cn/blog/2016/05/27/ios-duo-yu-yan-ben-di-hua/>

正文：

一个APP的多语言支持设置的方法我这里就不多说了，网上一大把教程。这里特别要做到的是应用内切换，且无需重启APP。

网上也有不少关于应用内切换多语言的，而且也是无需重启APP，

<http://blog.csdn.net/jackjia2015/article/details/51554375>

<http://www.jianshu.com/p/627da5a9992c>

<http://blog.csdn.net/u010884518/article/details/50482241>

但是有一个缺点：就是无法在不重启APP的情况下切换xib以及storyboard的语言，必须重启APP才能奏效。

下面我找到了一个可行的方法：(感谢@Gilad)

通过run-time来实现

新增一个名为Language的NSBundle类目文件

![](http://oapglm9vz.bkt.clouddn.com/1486263030.png )

![](http://oapglm9vz.bkt.clouddn.com/1486266316.png )

	
使用方法：

可以设置个点击按钮用于切换语言，这里我只列出按钮事件的方法

![](http://oapglm9vz.bkt.clouddn.com/1486266343.png )

别忘记 #import "NSBundle+Language.h"

最后一步：

将appdelegate里面设置window的rootViewController的方法封装出来独立一个，用于切换语言后重新赋值rootViewController。

![](http://oapglm9vz.bkt.clouddn.com/1486266368.png )

现在已经完成了所有步骤了。赶紧运行试试吧。

上面说的如果不了解，可以运行下面我提供的Demo。

Demo链接：

原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
