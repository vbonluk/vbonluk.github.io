---

layout: post
title: "让Xcode就地加速起飞🚀"
description: 
headline: 
modified: 2017-05-24
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

参考链接： <http://ios.jobbole.com/93171>

大家有没有发现每次修改一点东西要编译运行才能看到效果(特别是纯代码党)的时候就神烦，项目大了，文件多了，每次就是要等一段时间。虽然说苹果爸爸一直在优化Xcode，但是项目大，文件多，编译时间还是要等的。

![](http://oapglm9vz.bkt.clouddn.com/1495615311.png )

我们知道Xcode编译一直以来都是很吃CPU的。

![](http://oapglm9vz.bkt.clouddn.com/1495615263.png )

我们知道CPU一般很难再提升性能，那么有没有办法让Xcode再提速呢？

国外神人Jeff告诉我们答案是：有的。多年的计算机研究让他对整个计算机架构非常熟悉。下图是他展示的计算机结构简图。

![](http://oapglm9vz.bkt.clouddn.com/1495614921.png )

除了CPU我们还可以通过将项目编译的文件以及索引放到内存中，这样我们可以大大提速。这时候使用SSD的同学会问了，我的硬盘已经是SSD固态了，还用得着吗？其实一般电脑的SSD性能由于可能你是机械升级SSD(接口为STAT3),或者是M.2接口的。速度理论上还是比不上内存速度的。即使是最先进的 SSD，其速度也比 RAM 慢了400倍。也就是无论你怎么在软件层优化，其速度也无法突破 SSD 的瓶颈；

我们可以通过在运行Xcode项目前执行一段shell代码，在内存中开辟一个空间。空间大小根据你电脑实际内存大小和项目文件大小来决定。

	#!/bin/bash
	RAMDISK=”ramdisk”
	SIZE=2048        #size in MB for ramdisk.
	diskutil erasevolume HFS+ $RAMDISK \
     `hdiutil attach -nomount ram://$[SIZE*2048]`
     
将上面的代码保存为test.sh文件。(我们可以通过修改SIZE值来修改直接想要的大小，我这里为2G内存)然后在iTerm或者系统终端运行:
	
	./test.sh

你会看到桌面上多了一个虚拟盘

![](http://oapglm9vz.bkt.clouddn.com/1495614571.png )

打开这个虚拟盘，里面什么都没有，那是因为我们还没有在Xcode设置。

打开Xcode，Xcode -> Preferences -> Locations -> Locations Tab，配置 DerivedData。

![](http://oapglm9vz.bkt.clouddn.com/1495614651.png )

配置好了，运行一下Xcode。就会产生编译文件以及索引文件。

![](http://oapglm9vz.bkt.clouddn.com/1495614797.png )

这时候我们也可以感受一下编译的速度是否有提升。

是不是感觉以前是做火车，现在是坐🚀？

#### 需要注意的是：

	1、我们每次重启电脑后都需要执行一下脚本开辟内存空间给Xcode，不然Xcode点击run后会没有反应或者报错。
	
	2、如果Xcode报错，可以去看看开辟的虚拟盘是否已经满了。如果已经满了，有可能是你分配的空间不足，Xcode需要更大的空间。


原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
