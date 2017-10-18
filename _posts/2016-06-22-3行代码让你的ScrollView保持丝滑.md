---

layout: post
title: "3行代码让你的ScrollView保持丝滑"
description: 
headline: 
modified: 2016-06-22
category: iOS之路
tags: []
imagefeature: swift2.jpg
comments: true
mathjax: 

---


一般来说我们实现ScrollView都是一些长页面或者动态高度的页面或者是拿来适配不同屏幕尺寸的(iphone4，3.5寸)。按理说如果一个页面的内容高度达不到超出屏幕的高度就不会滚动。

![](/images/scrollView_scroll/0.gif)

但是事实上现在大部分用户都习惯了“滚动”页面这个概念。为了更好的适应用户的习惯，我们可以让页面内容不超过屏幕高度也可以滚动。其实原理超级简单啦。

我们先来看一个内容不足以超出屏幕范围的页面

![](/images/scrollView_scroll/1.png)

我们来通过Xcode查看视图的工具或者其他第三方的工具来查看页面布局

![](/images/scrollView_scroll/2.png)

![](/images/scrollView_scroll/3.png)

大家可以简单的了解到看到这个的页面。

下面我们来添加代码。

## Obejctive-C：

在.m文件中的

	- (void)updateViewConstraints{
		[super updateViewConstraints];
		//这里
	}


加入以下代码

	if (_scrollView.contentSize.height <= [UIScreen mainScreen].bounds.size.height - 64) {
        	_scrollView.contentSize = CGSizeMake([UIScreen mainScreen].bounds.size.width, [UIScreen mainScreen].bounds.size.height - 64 + 1);
    	}
	
## Swift：

在.swift文件中的
	
	override func updateViewConstraints() {
        super.updateViewConstraints()
        //这里
    }

加入以下代码

	if scrollView.contentSize.height <= UIScreen.mainScreen().bounds.size.height - 64 {
            scrollView.contentSize = CGSize.init(width: UIScreen.mainScreen().bounds.width, height: UIScreen.mainScreen().bounds.height - 64 + 1)
        }

我们来看一下效果：

![](/images/scrollView_scroll/5.png)

发现是可以滚动了~！

我们来分析一下代码里面的
	
	[UIScreen mainScreen].bounds.size.height - 64
	
和
	
	[UIScreen mainScreen].bounds.size.height - 64 + 1
	
是什么意思。

前面的“[UIScreen mainScreen].bounds.size.height”无疑就是获取屏幕高度而已。后面的64是什么呢？

看图

![](/images/scrollView_scroll/4.png)

这下知道了吧，就是状态栏的20px和导航栏的44px相加。由于苹果手机的这两个参数是固定的我们可以直接设死64，有的同学担心以后会发生变化？没事，我们也可以用下面的代码来获取状态栏高度和导航栏高度。

## Objective-C:

	// 状态栏(statusbar)
	CGFloat statusHeight = [[UIApplication sharedApplication] statusBarFrame].size.height;
	// 导航栏（navigationbar）
	CGFloat navHeight = self.navigationController.navigationBar.frame.size.height;
	
## Swift:
	// 状态栏(statusbar)
	let statusHeight = UIApplication.sharedApplication().statusBarFrame.size.height
	// 导航栏（navigationbar）
	let navHeight = self.navigationController?.navigationBar.frame.size.height
	
原理就是当ScrollV	iew的contentSize少于等于屏幕高度的时候赋值给他屏幕高度+1，ScrollView的内容那就超出屏幕滚动了。

对于上面获取的64导航栏和状态栏的情况，其实总共有4种情况：

1. 无导航栏无TabBar
2. 有导航栏无TabBar
3. 无导航栏有TabBar
4. 有导航栏有TabBar

这里代码大家可以动态得去设置。我这里展示给大家的是第2种情况。

Tips：TabBar高度是49。

~


原创作品，欢迎转载，转载请声明出处：http://www.真无聊.com
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
