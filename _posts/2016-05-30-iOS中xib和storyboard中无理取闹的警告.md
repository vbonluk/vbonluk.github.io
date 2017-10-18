---

layout: post
title: "iOS中xib和storyboard中无理取闹的警告"
description: 
headline: 
modified: 2016-05-30
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---


参考文献：

<http://www.zhimengzhe.com/IOSkaifa/2847.html>

<http://stackoverflow.com/questions/25398312/automatic-preferred-max-layout-width-is-not-available-on-ios-versions-prior-to-8>

这两天无事，看到自己项目编译的警告，终于可以闲下心来处理了！

警告一：

	Automatic Preferred Max Layout Width before iOS8.0
	
警告二：

	Justified or Natural text alignment before iOS 7.0
	
这两我们分开讲吧。

![](/images/xib-storyBoard/0.png)

## 先说第一个警告：Automatic Preferred Max Layout Width before iOS8.0

![](/images/xib-storyBoard/1.png)

看警告标题我们就可以清晰的知道错误原因。你点击这个警告你会发现xcode是不会给你调到对应的哪个控件的，只是调到当前报错的文件。这个无解，只有自己一个一个点击去看了。这一类问题都是Label引起的，所以我们可以分别点击Label来排除。

![](/images/xib-storyBoard/2.png)

我们点开倒数第二个，也就是我们设置约束的地方。找到Preferred Width这个选项，默认是自动(Automatic)的。但是这个方法只适用于iOS8.0以上。对于iOS8.0以下的话需要自己定义一下。所以这里才会有警报。

这里我们的处理可以是：

1、对于单行简单的Labe我们可以点选Explicit来自动拿到宽度设死，但是如果是对于和屏幕宽度相关动态变化的话，这里最好设0。

![](/images/xib-storyBoard/3.png)

2、多行Label设0。

![](/images/xib-storyBoard/4.png)

然后我们按command + K，快捷键clean一下工程。再run。警告消失。

5s运行

![](/images/xib-storyBoard/6.png)

## 接下来我们来说第二个警告：Justified or Natural text alignment before iOS 7.0

![](/images/xib-storyBoard/7.png)

这个也是Label的问题！这个简单，设置一下Label的对齐方式就行了。

![](/images/xib-storyBoard/8.png)


原创作品，欢迎转载，转载请声明出处：http://www.真无聊.com

友情链接联系:5914018@qq.com

交流QQ群：271568188