---

layout: post
title: "iOS: TableView HeaderView + AutoLayout"
description: 
headline: 
modified: 2016-12-29
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---


### 参考链接
	
<http://stackoverflow.com/questions/16471846/is-it-possible-to-use-autolayout-with-uitableviews-tableheaderview>
	
<http://stackoverflow.com/questions/20609206/setneedslayout-vs-setneedsupdateconstraints-and-layoutifneeded-vs-updateconstra>
	
### 正文

首先我们根据两张张图片来分析布局

图一：

![](http://oapglm9vz.bkt.clouddn.com/1483000539.png )

图二：

![](http://oapglm9vz.bkt.clouddn.com/1483000454.png )

我们可以根据图二来开始制作页面的布局，我们对图二进行布局思考：

![](http://oapglm9vz.bkt.clouddn.com/1483000662.png )

显然是TableView + HeaderView的结合。


下面开始实现布局：（xib模式）

![](http://oapglm9vz.bkt.clouddn.com/1483000849.png )

编码：

![](http://oapglm9vz.bkt.clouddn.com/1483001128.png )

一般来说这样做虽然就没有用到AutoLayout了(高度用frame指定)，但是也是是没有问题的。

这时候，我们将图二对比图一，发现区别在于Header中的

![](http://oapglm9vz.bkt.clouddn.com/1483001317.png )

这一块是不一定有的。

下面我来说一下需求：当用户分配了秘书则显示秘书，如果无则不显示。

这样的需求其实也就是HeaderView的高度是动态的。在不使用AutoLayout来获取真实高度的情况下我们也可以这样通过frame的改变来复制给TableView：

首先给定秘书view一个固定高度，并将高度链接到类文件。

![](http://oapglm9vz.bkt.clouddn.com/1483001646.png )

![](http://oapglm9vz.bkt.clouddn.com/1483001712.png )

因为我们秘书view的高度是已知的(125)，这时候我们定义两个高度：

	1.没有秘书的HeaderView高度
	2.有分配秘书的HeaderView的高度

![](http://oapglm9vz.bkt.clouddn.com/1483001893.png )

这时候我们在HeaderView定义一个方法，用来动态改变高度。

![](http://oapglm9vz.bkt.clouddn.com/1483002043.png )

最后我们在请求服务器返回数据的回调中改变HeaderView的高度并赋值给TableView。

![](http://oapglm9vz.bkt.clouddn.com/1483002001.png )

通过这个方法我们就完成了需求。

但是有没有更好的解决方案呢？我不想每次都用frame这套古老的方法布局！我不想写死HeaderView高度！而且假如有新的需求：
	
	HeaderView里面有一个Label，而这个Label的长度是动态的，
	也就是行数是不确定的，行数的不确定意味着高度的动态！

在这种需求下面虽然也可以使用上面的方法，但是要实现里面的Label动态高度却是很麻烦，你要用代码计算一次Lable高度，再相加其他控件的高度得到确定的高度，才能赋值到TableView的tableHeaderView。虽然实现代码不多，但是过程也肯定不会愉快！(我就做过)

### 下面我们通过AutoLayout来简化这个过程吧！

既然是动态的高度，那么我首先去掉定义的两个高度。

![](http://oapglm9vz.bkt.clouddn.com/1483001893.png )

删除掉上面的固定高度。

我们新增一个Category,并定义方法：setAndLayoutTableHeaderView

.h

![](http://oapglm9vz.bkt.clouddn.com/1483002696.png )

.m

![](http://oapglm9vz.bkt.clouddn.com/1483003424.png )


此文件已经上传到这里：<https://github.com/vbonluk/TableViewHeader-Autolayout>

我们最好将此文件在基类import

![](http://oapglm9vz.bkt.clouddn.com/1483002865.png )

这样以后我们所有的ViewController都可以调用了！

回到我们刚刚的页面，我们修改tableview的HeaderView的赋值方式：

![](http://oapglm9vz.bkt.clouddn.com/1483003006.png )

上面是初始化的时候调用，初始化默认没有分配秘书，所以初始化的时候调用：
	
	[headerView isShowSecretary:NO];
	
接下来我们修改数据回调中的方法：

![](http://oapglm9vz.bkt.clouddn.com/1483003175.png )

这时候运行：

无秘书：

![](http://oapglm9vz.bkt.clouddn.com/1483000539.png )

有秘书：

![](http://oapglm9vz.bkt.clouddn.com/1483000454.png )

发现出来的效果是一致的！Oh yeah！

我们来分析一下那个Category：

![](http://oapglm9vz.bkt.clouddn.com/1483003424.png )

原理：

	1.调用setNeedsLayout告诉view布局改变了。
	2.调用layoutIfNeeded强制布局。
	3.调用systemLayoutSizeFittingSize获得布局后的准确高度。
	4.调用self.tableHeaderView = header重新赋值HeaderView。
	
### 总结

上面的文件已经开源到这里：<https://github.com/vbonluk/TableViewHeader-Autolayout>

通过上面的方法我们可以在布局HeaderView的时候放下frame的计算了。其实上面的方法可能还可以再次优化！只需一次赋值HeaderView就可以动态高度。我觉得可以通过run-time + KVO + layoutSubView的方法来实现。暂时没时间去研究，先放下。
	
下面给大家一张调用布局方法的流程图：

![](http://oapglm9vz.bkt.clouddn.com/1483003691.png )

# Update 更新

通过上面的方法发现还有一点问题，就是在多行Label动态高度的时候systemLayoutSizeFittingSize方法算出的高度并不是准确的。

![](http://oapglm9vz.bkt.clouddn.com/1483016354.png )

这一点我们可以通过设置UILabel的preferredMaxLayoutWidth属性来进行修正。

通常情况我们可以通过简单的遍历来给所有subViews的UILabel设置属性。

	for (UIView *view in header.subviews) {
        if ([view isKindOfClass:[UILabel class]]) {
            UILabel *label = (UILabel *)view;
            //不设置这个属性会导致算出来的高度不准。
            label.preferredMaxLayoutWidth = label.frame.size.width;
        }
    }
    
但是，假如我们的HeaderView是比较复杂的多层view嵌套呢？不同层级下有UILabel呢？这时候，其实我们可以将HeaderView的SubViews看做是一个树(tree)型结构，也就是多叉树。我们可以通过多叉树的递归遍历来给每层View的Label设置属性。

![](http://oapglm9vz.bkt.clouddn.com/1483016206.png )



原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
