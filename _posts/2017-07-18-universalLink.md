---

layout: post
title: "iOS9+ Universal Link 右上角 “UIStatusBarOpenInSafariItemView” 禁用技巧"
description: 
headline: 
modified: 2017-07-18
category: iOS之路
tags: [Universal Link 右上角]
imagefeature: 
comments: true
mathjax: 

---

## 参考链接：

## 正文：

本文不讨论 Universal Link 的基础部署步骤。需要的同学请点击链接：

<https://developer.apple.com/library/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html>

注意：页面域名和 Universal Link 域名必须不一致，也就是所谓的“跨域”，才能成功跳转APP。

下面我们来讨论一下一个特殊的需求，那就是跳转到APP后，将右上角的跳转Safari按钮取消跳转事件，也就是下图中右上角的"ykb100.com"。

![](http://oapglm9vz.bkt.clouddn.com/1500369698.png )

如果我们不处理，那么按照Apple的逻辑，点击了之后将会跳转到Safari打开网页，然后你所有定义在web的 Universal Link 都将使用Safari来打开，而不会提示你使用APP来打开。

这就是麻烦所在，当用户点击了右上角后我们再也无法通过APP跳转了。都是直接调取web了。更糟糕的情况是，假如你的 Universal Link 没有与之对应的web网页，那么这时候用户将会跳转到一个空白页面或者404页面。显然这不是我们所需要的。

为了防止这种情况的发生，我们有2种解决方法。

### 第一种方法：

其实按照Apple的逻辑，也是没有问题的。虽然用户点击了右上角后就再也无法通过web跳转APP了，而是直接跳转web了，但是我们这时候还是有办法设置成跳转APP的。

![](http://oapglm9vz.bkt.clouddn.com/1500370960.png )

如上图，只要我们在含有 Universal Link 的web网页中长按了 Universal Link 那么就会在actionSheet中多出一个选项：在“。。。”中打开。这时候，我们点击它就可以跳转到APP了。用户如果通过这种形式来跳转到APP，那么系统将会默认使用APP来跳转我们的 Universal Link 。也就是恢复正常了。

但是这种方法需要引导用户操作，成本大。

### 第二种方法：

![](http://oapglm9vz.bkt.clouddn.com/1500371384.png )

如上图所示，我们在Safari中打开含有 Universal Link 的网页，如果用户取消了APP跳转的方式，那么这时候，如果我们下拉网页就会显示出来这种形式。这是系统自动解析的，无需我们操心。但是有一点需要注意的，那么还是需要引导用户下拉网页，因为默认这个是隐藏的。

还有一种方法无需提示用户下拉，那么就是：接入Apple的js，Apple有个smart banner，可以提示。具体这里不再详谈。

### 第三种方法：

较为极端的方法：禁用跳转到Safari中打开，只能跳转到APP中打开。

![](http://oapglm9vz.bkt.clouddn.com/1500369698.png )

简单来讲，就是用户点击右上角的时候，不会跳转到Safari，那么系统就不会将我们的跳转方式设置成Safari打开，我们就可以实现一直通过APP打开了。

这个方法的思考来源是：网易新闻APP客户端。想体验的朋友可以分享一篇文章到微信，然后微信跳转到APP打开，点击右上角，你会发现是无法响应的。这样就可以没有Safari之忧了。

![](http://oapglm9vz.bkt.clouddn.com/1500372565.png111 )

### 分析

	上面三种都是可行的解决方法。第三种是较为极端的做法，完全屏蔽了web跳转。方法的取舍就是要看运营怎么决策了吧。按照Apple的意思是要么web跳转，要么APP跳转。
	
### 实现方法

第一二种方法没有什么技巧可言，都是直接操作。这里我们来研究一下第三种的方法。

1、我们在appdelegte.m文件找到如下applicationDidBecomeActive方法：

	- (void)applicationDidBecomeActive:(UIApplication *)application{
	
	 }

我们加入如下代码：

	- (void)applicationDidBecomeActive:(UIApplication *)application{
    
    	NSArray *statusBarSubViews = [[[[UIApplication sharedApplication] valueForKeyPath:@"statusBar"] valueForKeyPath:@"foregroundView"] subviews];
    	for (int i = 0; i < [statusBarSubViews count]; i ++) {
        	UIView *view = statusBarSubViews[i];
        
        	if ([view isKindOfClass:[NSClassFromString(@"UIStatusBarOpenInSafariItemView") class]]) {
            	view.userInteractionEnabled = NO;
            
            	id destinationText = [view valueForKeyPath:@"_destinationText"];
            	NSLog(@"destinationText is %@", destinationText);
        	}
        
    	}
    
	}

断电并运行APP，运行后我们需要执行一次跳转动作，例如：微信web点击我们的 Universal Link 跳转到APP才能断点成功，我们可以得到如下信息：
	
![](http://oapglm9vz.bkt.clouddn.com/1500456004.png )

为了更详细的了解，我们再打印一下view的数组：

![](http://oapglm9vz.bkt.clouddn.com/1500456114.png )

这时候我们可以清晰的看到：“UIStatusBarOpenInSafariItemView”这个view，从字面上我们就可以大概猜到是我们所需要的了。

我们可以尝试作出修改来达到我们的无法点击需求。

![](http://oapglm9vz.bkt.clouddn.com/1500456181.png )

从上面的图片可以看到，我们尝试了通过设置透明度alpha,移除view，设置frame,的方式来解决，但是最终都被注释了，而采用的是设置view的事件传输userInteractionEnabled属性。

原因还是有的，首先如果我们通过前面的方法，设置透明度alpha，和移除view，我们会发现还是无法实现的，原因就是状态栏会刷新，但是我们没有获取刷新的方法，设置了之后，系统自动刷新了状态栏，那么view的属性将会被重置。

经过尝试，我们通过设置userInteractionEnabled可以达到禁用点击事件。从而来达到我们的需求。我们实现的原理是基于KVO来拿到状态栏的view。

其实跟进一步来说还是有办法实现，修改右上角文字，或者通过runtime来实现替换点击方法，甚至隐藏掉右上角的。这里就暂时先不再深入探讨了。

希望完成了更多自定义的同学也把方法分享出来吧，我会链接到本文的。谢谢。

原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
