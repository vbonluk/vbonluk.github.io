---

layout: post
title: "Xcode自定义模板-代码规范统一化"
description: 
headline: 
modified: 2017-03-13
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

先上效果图：

没有使用自定义模板

![](http://oapglm9vz.bkt.clouddn.com/xc2.gif )

使用了自定义模板

![](http://oapglm9vz.bkt.clouddn.com/xc1.gif )

首先，我们来简单说明一下什么是Xcode模板。Xcode模板就是每次你新建类文件(或者其他文件,这里以类文件作为例子)，系统默认帮你添加进去类文件里面的内容。如下图：

![](http://oapglm9vz.bkt.clouddn.com/1489379743.png )

这些方法都是系统默认模板给你写好的。

那么有没有办法弄一套我们自己想要的，结合项目的模板呢？

### 看了上面的我们来谈一下需求：

#### 目标：一线码农。

结合实际开发来说，大家会不会遇到每次新建类文件都需要将一些固定的方法从别的的放复制然后粘贴到这个类文件，虽然说这个过程并不痛苦，可是就是会浪费那么一点时间，要知道我们程序猿的时间可是宝贵的。然后有的时候我们会把一些方法遗漏了，没有复制过去，那么我们这时候又要去找。这个轮回是不是很繁琐？

#### 目标：团队Leader，小组Leader

每次code review的时候老是发现一些队友没有按照规定好的路线走。每个成员的代码风格各不一致，导致看起来有些混乱。明明有相同的方法却重复造轮。(做SDK开发的朋友这一点是刚需)

结合上面两点，我觉得Xcode自定义模板势在必行，应该说是一个团队里面必备的东西了。对团队成员也好，Leader也好。起码看起来赏心悦目吧。

#### 模板知识延伸 -> UIView的xib创建。

不知道大家有没有经常使用xib来创建界面(纯代码党可以略过)。大伙会发现苹果早已在创建UIView的时候那个创建xib的选项默认是灰色不能选择的。那么有没有办法利用模板来使他可以勾选呢？答案是有的。

![](http://oapglm9vz.bkt.clouddn.com/1489381393.png )

### 开始制作Xcode自定义模板

#### 第0步

我已经上传自定义模板到这里：<https://github.com/vbonluk/XcodeTemplate>

不想一步步配置的朋友可以直接下载上面链接的项目，修改其中的
	
	CustomVCObjective-C 	文件夹
	CustomVCXIBObjective-C 	文件夹
	
这两个文件夹里面的 

	___FILEBASENAME___.h 
	___FILEBASENAME___.m 

文件的内容就可以了。内容为相同的内容。

![](http://oapglm9vz.bkt.clouddn.com/1489381533.png )

使用第0步配置的同学可以完全忽略下面的内容了。此文完结。

#### 第1步

找到Xcode的模板文件xctemplate。

在Xcode8.1的路径是：

	/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates/File Templates/Source

大家可以根据自己xcode版本来查找，至于使用了xcode8Gr(为了使用插件而定制的Xcode)路径则是相应的将前面的Xcode.app改成你的Xcode8Gr的名字就可以了。下面做的修改也要放到对应的Xcode8Gr内。

#### 第2步

为了方便和快速，我们复制已经写好的默认模板，在默认模板的基础上进行修改就可以了。

![](http://oapglm9vz.bkt.clouddn.com/1489382151.png )

我们复制Cocoa Touch Class.xctemplate文件夹，并粘贴到当前路径，改名为：CustomVC.xctemplate(注意，我们是复制，而不是直接修改，如果直接修改那么会影响原来的模板，例如你删除了ViewController，那么你将无法正常创建原来的ViewController，需要自己全部手动敲进去)

![](http://oapglm9vz.bkt.clouddn.com/1489382235.png )

删掉自己不用的东西。例如我这里只用到了ViewController这个类进行自定义，而且我项目主要采用Objective-C进行开发，那么连Swift的也可以一并删去。(后缀是Objective-C的是Objective-C版本，后缀带Swift的是Swift版本)。

最后我们的文件夹可以简洁成这样：

![](http://oapglm9vz.bkt.clouddn.com/1489382446.png )

我们将 UIViewControllerObjective-C 和 UIViewControllerXIBObjective-C 这两个文件夹改名为：CustomVCObjective-C 以及 CustomVCXIBObjective-C 。

这里需要特别说明一下：这个文件夹名字中带XIB的表示是你创建类文件时候勾选了xib选项就会使用带XIB的文件夹模板。还有一点就是这里的命名是很重要的。“CustomVC”这个前缀是我们定义的，这是一定要修改的，不能使用原来的UIViewController，会和原来的默认模板冲突导致无法创建类文件。

### 第3步

修改文件内容：根据自己实际需求来进行。这里以我的为例子。

	___FILEBASENAME___.h 文件
	
![](http://oapglm9vz.bkt.clouddn.com/1489383021.png )

	___FILEBASENAME___.m 文件

![](http://oapglm9vz.bkt.clouddn.com/1489383092.png )

编辑好类的内容后，记得将带XIB的CustomVCXIBObjective-C文件夹里的.h和.m也覆盖进去，达到同样的效果。

### 第4步

编辑TemplateInfo.plist文件。

找到当前自定义模板下的TemplateInfo.plist文件。

![](http://oapglm9vz.bkt.clouddn.com/1489383417.png )

我们只需要编辑Options下面的Item1和Item2就可以了。

![](http://oapglm9vz.bkt.clouddn.com/1489383456.png )

找到Item1中的Values和Suffixes并删除里面所有的内容，写入我们自定义模板的名字。

![](http://oapglm9vz.bkt.clouddn.com/1489383517.png )

对了Item1中的FallbackHeader

![](http://oapglm9vz.bkt.clouddn.com/1489383610.png )

表示的是.h文件上面#import的内容。这里我修改为我需要的。

接下来是Item2，找到RequiredOptions选项并展开，继续展开cocoaTouchSubclass选项，删除所有内容，写入CustomVC就可以了。至于为什么我这里会多了一个UIView，下面我会说明。

![](http://oapglm9vz.bkt.clouddn.com/1489383697.png )

做完上面我们切换到Xcode试试效果(修改无需重启Xcode直接尝试就可以了)。

下面列出修改过程中可能用到的宏定义的说明：

	DATE：标识当前时间；
	FILENAME：带文件后缀的全名；
	FILEBASENAME：不带文件后缀的名字；
	FULLUSERNAME：当前的用户名；
	PROJECTNAME：工程名字；
	FILEBASENAMEASIDENTIFIER： VC 类名称；
	IMPORTHEADER_cocoaSubclass： 导入的头文件。

关于TemplateInfo.plist文件的说明

	SortOrder：模版在界面中的位置；
	Options：对应 图2 四行；
	FallbackHeader：.h 导入的头文件；
	RequiredOptions -> cocoaSubclass：是否支持选择 xib；	Default 默认 true 自动勾选；
	Values：自定义模版的名称(一定要保持一致);
	Suffixes：模版默认类名。

### 第5步

这一步其实是关于UIView的Xib文件创建的设置。(UIView的Xib关联)

按照上面第4部说的，我们需要在TemplateInfo.plist文件的Options -> Item1 -> Values 新增一项 CustomView。

然后

在TemplateInfo.plist文件的Options -> Item1 -> Suffixes 新增一项 View。

然后

在TemplateInfo.plist文件的Options -> Item2 -> RequiredOptions -> cocoaTouchSubclass 新增一项 UIView。

![](http://oapglm9vz.bkt.clouddn.com/1489636784.png )

修改对应的CustomViewObjective-C和CustomViewXIBObjective-C两个文件夹内的___FILEBASENAME___.h文件。

![](http://oapglm9vz.bkt.clouddn.com/1489644860.png )

![](http://oapglm9vz.bkt.clouddn.com/1489644986.png )

最后尝试创建。可以了。

![](http://oapglm9vz.bkt.clouddn.com/1489636816.png )

### 写在最后

希望大家配置成功，走上可以偷懒(避免重复劳作)以及规范的道路。

原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
