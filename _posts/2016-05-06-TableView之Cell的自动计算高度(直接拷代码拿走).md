---

layout: post
title: "TableView之Cell的自动计算高度(直接拷代码拿走)"
description: 
headline: 
modified: 2016-05-06
category: iOS之路
tags: []
image: 
  feature: 
comments: true
mathjax:
 
---

关于TableView在Autolayout下的动态cell高度

![](/images/tableViewCellHeight/1.png)

贴上代码，下次直接拷贝就可以拿去用了！
原理是采用dispatch_once_t生成一个cell来计算！(这种方法创建的cell只能是同样样式的，如果有不同样式的要用if-else区分创建！)

注意：例子中的cellHeightCacheArray是一个用于缓存cell高度的数组。别忘记在下拉列表刷新全部数据的时候清空这个数组。


	- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
	{
    
    if (cellHeightCacheArray.count > indexPath.row) {
        return [cellHeightCacheArray[indexPath.row] floatValue];
    }
    
    //只创建一个cell用作测量高度
    static XFJ_Rexources_Cell *cell = nil;
    
    static dispatch_once_t onceToken;
    //只会走一次
    dispatch_once(&onceToken, ^{
        cell = [[[NSBundle mainBundle] loadNibNamed:@"XFJ_Rexources_Cell" owner:self options:nil] lastObject];
        cell.delegate = self;
    });
    
    
    cell.resources = [self.paginator.results objectAtIndex:indexPath.row];
    
    CGFloat h = [self getCellHeight:cell];
    
    [cellHeightCacheArray addObject:@(h)];
    
    
    return h;
	}

	- (CGFloat)getCellHeight:(UITableViewCell*)cell
	{
    [cell setNeedsUpdateConstraints];
    [cell updateConstraintsIfNeeded];
    
    CGFloat height = [cell.contentView systemLayoutSizeFittingSize:UILayoutFittingCompressedSize].height;
	//    NSLog(@"\n:height%f",height);
    return height +1;
	}
	
	
更多方法：

![](/images/tableViewCellHeight/2.jpg)

原创作品，欢迎转载，转载请声明出处：http://www.真无聊.com

友情链接联系:5914018@qq.com

交流QQ群：271568188