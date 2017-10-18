---

layout: post
title: "Xcode插件重生"
description: 
headline: 
modified: 2017-07-20
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

### 前言

按照一贯的做法，我们每次更新Xcode最蛋疼的莫过于需要重新配置Xcode插件了。破解Xcode不说，还需要去每个插件里面加入UUID，这琐碎程度真的大了。有没有办法一步到位，有没有工具一次性全部弄好呢。答案是有的。

### 正文

我们参考github上面的大神放出来的工具：<https://github.com/inket/update_xcode_plugins>

第一步：

我们在终端中运行以下命令：安装工具

	gem install update_xcode_plugins

![](http://oapglm9vz.bkt.clouddn.com/1500540520.png )

运行完成如上图。

第二步：

我们在终端中运行以下命令：安装Alcatraz插件管理

	curl -fsSL https://raw.github.com/alcatraz/Alcatraz/master/Scripts/install.sh | sh
	
![](http://oapglm9vz.bkt.clouddn.com/1500540614.png )

运行完成如上图。

第三步：

我们在终端中运行以下命令：运行工具

	update_xcode_plugins
	
![](http://oapglm9vz.bkt.clouddn.com/1500540737.png )

运行完成如上图。

证明我们是Xcode8以上的，需要去掉Xcode签名。

第四步：

我们在终端中运行以下命令：去签名

	update_xcode_plugins --unsign
	
![](http://oapglm9vz.bkt.clouddn.com/1500540838.png )

运行完成如上图。

有一点需要注意，这一步在中间会停顿下来提示你选择Xcode，我们按回车确定，并输入y

![](http://oapglm9vz.bkt.clouddn.com/1500541388.png )

到这一步已经安装完毕了，并且插件也已经自动加入UUID了。再也无需一个一个添加了。以后每次只需要运行一下update_xcode_plugins --unsign就可以了。

我们重启Xcode。

![](http://oapglm9vz.bkt.clouddn.com/1500541028.png )

惯例点击：Load Bundles 按钮来加载插件。

至此，重新享受插件带来的愉悦吧。

原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
