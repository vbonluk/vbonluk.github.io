---

layout: post
title: "Mac下SVN账号信息重置(svn客户端)"
description: 
headline: 
modified: 2016-11-30
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

### 首先

![](http://oapglm9vz.bkt.clouddn.com/1480502527.png )

![](http://oapglm9vz.bkt.clouddn.com/1480502584.png )

### 解决方法：

在终端中执行：

1、cd到根目录
	
	cd ~

2、查看文件目录

	ls -a
	
3、cd进去svn目录

	cd .subversion/auth
	
4、执行删除语句

	rm -r -f -d *
	
5、再次checkout
	
	svn chectout 你的svn url
	
接着应该会出现这个：

![](http://oapglm9vz.bkt.clouddn.com/1480502940.png )

输入

	p

然后按照提示输入账号密码就可以了。
	


原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
