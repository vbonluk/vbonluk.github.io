---

layout: post
title: "APP本地数据库之数据库的基本操作以及数据库的表结构升级(FMDB)"
description: 
headline: 
modified: 2016-07-27
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

### 前言：
	
一般来说服务器端的同学都是先了解业务，再根据业务来设计数据库的表以及表关系，可以直接进行数据库的表操作。和服务器端的数据库设计不同，我们app端的数据库是处于一个“被动的状态”。“被动的状态”指的是我们的数据库表或者表与表之间的关联在不同的版本发生变化，需要在新版本做更新的操作。这样就得要求我们在每次版本的迭代中进行数据库表版本号的检查以及升级。这种情形是必然需要做的，因为在不同的版本下，有的时候需要给不同的表做新增字段或者减少字段的需求。例如我们在5.0的版本中查找数据库中的用户信息表customer中的一个customerType字段，而这个customerType字段是在5.0才新增的。那么如果我们不操作数据库的升级，我们知道APP的更新并不是完全的覆盖更新，而是替换更新。那么这时候的数据库customer表还是4.0的表，没有customerType字段。这下调用会查不到。所以我们很有必要进行数据库表的版本号标识和检查更新。

参考资料：

<http://blog.csdn.net/a416863220/article/details/48787723>

<http://my.oschina.net/iq19900204/blog/407328>



下面直接列出相应的操作：

打开数据库：

![](http://oapglm9vz.bkt.clouddn.com/1471934150.png )


增：

![](http://oapglm9vz.bkt.clouddn.com/1471934192.png )

删：

![](http://oapglm9vz.bkt.clouddn.com/1471934218.png )

改：

![](http://oapglm9vz.bkt.clouddn.com/1471934253.png )


查：

![](http://oapglm9vz.bkt.clouddn.com/1471934295.png )

我们可以用

![](http://oapglm9vz.bkt.clouddn.com/1471934391.png )

来打开模拟器里面的app文件路径找到数据库文件

![](http://oapglm9vz.bkt.clouddn.com/1471934480.png )

再使用

![](http://oapglm9vz.bkt.clouddn.com/1471934512.png )

来打开数据库。

![](http://oapglm9vz.bkt.clouddn.com/1471933984.png )

我们看回去代码，其关键无非就是执行sql的ALTER语句来新增字段。

![](http://oapglm9vz.bkt.clouddn.com/1471934619.png )

大家不免觉得奇怪，这里面有几个方法我并没有定义

![](http://oapglm9vz.bkt.clouddn.com/1471934688.png )

其实这里我只是提供了一个思路。不同的版本对于的不同的操作。有的版本可能删除了字段，或者新增了字段。或者修改了表关联。所以具体的还是要自己去做。

需要源码的请e-mail。




原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
