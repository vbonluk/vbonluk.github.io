---

layout: post
title: "xib中加载UIView之奥秘"
description: 
headline: 
modified: 2016-07-22
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

### 前言

对于UIView：

	(.h和.m) 一对多 (xib-View)
	(xib-View) 一对多 (.h和.m)

有的时候我们会有特殊的需求：一个VC中动态调用多个不同样式的自定义View，例如各种不同样式的弹出窗，背景半透明的弹出窗等，在不想用纯代码来构建View的时候有的人会用xib来放置View，这时候通常是一个View，OC有3个文件(.h、.m和xib)，Swift有2个文件(.swift和xib)，那么对于业务逻辑差不多的View要建立起来，需要有多个文件，这对有文件洁癖的人来说是一个挑战。so，我们下面来尝试一下一个xib里面放多个View来调用。虽然这样子似乎违背了MVC和代码可读性。但是我们还是可以做一下实验~

### 正文

#### 首先：

	(.h和.m) 一对多 (xib-View)

首先我们建立一个空白项目。新建好TestView所需文件，并在ViewA顶部放入一直“奥巴马”身份证的UIImageView。

![](/images/xib-uiview/1.png)

我们再拖出一个ViewB

![](/images/xib-uiview/2.png)

我们往ViewB底部放奥巴马身份证并设置好约束

![](/images/xib-uiview/3.png)

设置好相应的属性：Custom Class，这里我们设置对应的TestView

![](/images/xib-uiview/4.png)

设置好相应的属性：View tag，这里我们设置ViewA为对应的10001，ViewB为10002

![](/images/xib-uiview/5.png)

在ViewControler中加入代码调用

![](/images/xib-uiview/6.png)

接下来我们调用A和B，运行模拟器。

![](/images/xib-uiview/7.png)![](/images/xib-uiview/8.png)![](/images/xib-uiview/9.png)

可以明显的从代码知道我们是通过遍历

	NSArray *viewArray = [[NSBundle mainBundle] loadNibNamed:@"TestView" owner:self options:nil];

来获取view的。重要的依据是tag值

	if (view.tag == 10002) {
            viewB = (TestView*)view;
        }

#### 接下来
	
	(xib-View) 一对多 (.h和.m)

首先我们新建TestView2的.h和.m文件，并设置ViewB的Custom Class为TestView2，这样一来我们一个xib里面的两个view就对应2个controller了。

![](/images/xib-uiview/10.png)

接下来我们修改原来VC中的代码：

![](/images/xib-uiview/11.png)

运行模拟器，效果：

![](/images/xib-uiview/7.png)![](/images/xib-uiview/8.png)![](/images/xib-uiview/9.png)

发现还是可以调用的哦！

我们这次再看代码：

	if ([view isKindOfClass:[TestView2 class]]) {
            viewB = (TestView2*)view;
        }
        
唯一的区别在于判断条件不同了。注意：这里我们可以不用管tag值有没有设置。

#### Last

从上面两个得出我们可以通过遍历来得到对应的View，解决了一个疑问：每次再也不是硬生生的调用
	
	[[[NSBundle mainBundle] loadNibNamed:@"TestView" owner:self options:nil] lastObject];

了！我们这下可以灵活运用了。

其实我们不单可以在View里面的xib调用多个view控件。我们还可以在VC的Xib里面调用不同的控制器呢。

原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
