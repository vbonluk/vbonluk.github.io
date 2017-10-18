---

layout: post
title: "UISearchDisplayController + UISearchBar 笔记"
description: 
headline: 
modified: 2016-08-26
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

搜索栏，现在也是每个APP必备的东西了。这里将遇到的问题做一个笔记。

布局：

![](http://oapglm9vz.bkt.clouddn.com/1472177062.png )


问题：

![](http://oapglm9vz.bkt.clouddn.com/1472177103.png )

解决方案：

![](http://oapglm9vz.bkt.clouddn.com/1472177148.png )

	- (void)viewDidLoad {
    	[super viewDidLoad];
    
    	self.edgesForExtendedLayout = UIRectEdgeNone;
	}

效果：

![](http://oapglm9vz.bkt.clouddn.com/1472177172.png )

原因分析：

	self.edgesForExtendedLayout = UIRectEdgeNone;

这句代码使UISearchDisplayController在点击UISearchBar扩展VC视图布局的时候恢复到正确的位置。

原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
