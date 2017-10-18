---

layout: post
title: "Tableview的headerView使用xib加载的注意事项"
description: 
headline: 
modified: 2016-07-18
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

如果TableView的headerView有多个控件，那么加载xib的时候会出现高度错位的问题。


我们一般加载headerview是这样的：

	AddCustomer_NewRoleHeaderView *headerView = [[[NSBundle mainBundle] loadNibNamed:@"AddCustomer_NewRoleHeaderView" owner:self options:nil] lastObject];
	headerView.frame = CGRectMake(0, 0, kDeviceWidth, 262);
	_tableView.tableHeaderView = headerView;

希望得到的结果是这样的：

![](/images/tableview-headerview-xib/1.png)

但是实际上却是这样的：

![](/images/tableview-headerview-xib/2.png)

我们通过视图工具可以看到：

![](/images/tableview-headerview-xib/3.png)

这里的headerview和cell错位了！

我们这时候看右侧属性：

![](/images/tableview-headerview-xib/4.png)

显示高度是366，这里就奇怪了，明明我设置的是262怎么是366了呢？

![](/images/tableview-headerview-xib/5.png)

我们在视图工具中直接修改到262，发现呈现的效果是我们期待的正确的效果：

![](/images/tableview-headerview-xib/1.png)

这是什么问题导致的呢，百思不得其解。我回想以前也出现过这个问题，最后是在将xib的view放到一个临时的UIView中，然后tableview加载的是UIView，这样就可以避免这个问题了。试了一下，真的又可以了。这里做下笔记。

	AddCustomer_NewRoleHeaderView *headerView = [[[NSBundle mainBundle] loadNibNamed:@"AddCustomer_NewRoleHeaderView" owner:self options:nil] lastObject];
    headerView.frame = CGRectMake(0, 0, kDeviceWidth, 262);
    UIView *headViewBgView = [[UIView alloc] initWithFrame:headerView.frame];
    [headViewBgView addSubview:headerView];
    _tableView.tableHeaderView = headViewBgView;
    
 效果：
 
 ![](/images/tableview-headerview-xib/1.png)
 

原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
