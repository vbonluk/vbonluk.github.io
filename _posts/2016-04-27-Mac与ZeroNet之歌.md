---

layout: post
title: "Mac与ZeroNet之歌"
description: "尝鲜"
category: 乱七八糟
tags: [sample post, readability]
modified: 2016-04-27
imagefeature: ZeroNet.png
comments: true
share: true

---

# 前言

最近一同学无聊发了自己的网站在群里，我点进去发现是一个叫ZeroNet的网站，只是域名是同学的。询问得知他将自己的域名指向了自己的本地ZeroNet。查了一下ZeroNet，好像还有点意思。自己也去尝鲜了一把，下面放出部署中遇到的坑。

按照惯例：先上弯路

# 走过的弯路

参考文献:

<https://zeronet.io/>

<http://www.wanbizu.com/news/201603086775.html>

<http://blog.csdn.net/yuleslie/article/details/8170086>

<http://python-guide-zh.readthedocs.org/zh_CN/latest/starting/install/osx.html>

<https://www.reddit.com/r/zeronet/comments/48wlhb/zeronet_installation_problem/>

<http://bbs.kafan.cn/thread-2035792-1-1.html>

工具:

python

pip

# 正文
由于ZeroNet是开源的

首先去ZeroNet的官方github:<https://github.com/hellozeronet/zeronet>下载整个项目下来。

使用终端(Terminal 或者iterm)cd到项目目录。

然后输入python start.py

由于Mac在10.8就已经集成了Python，我们不需要另外下载。

这里没问什么问题的话就会看到

![](/images/ZeroNet/1.png)

但是！在我的iMac，系统10.11.4 由于之前没用Python进行开发，缺少一些包。

出现了这个
Issue: ImportError: No module named gevent 

有的同学可能会遇到连Python运行都出错的问题。这里我建议使用homebrew重新安装一个完全版的Python而不是mac自带的Python。
参考<http://python-guide-zh.readthedocs.org/zh_CN/latest/starting/install/osx.html>

在配置好homebrew环境后
运行

		brew install python --framework

安装完整的Python。

好了，说回来上面的ImportError错误。无非就是缺少gevent包。

这里我们用Python的发布包Pip来快速安装这个包。

现在终端安装Pip工具

		$ easy_install pip
		
(更新：最新版的Python默认安装了Pip)如下图：

![](/images/ZeroNet/2.png)

接着我们安装gevent

		pip install gevent

![](/images/ZeroNet/3.png)


然后开始ZeroNet的安装，cd到项目目录。执行

		python start.py

发现打印出来了一堆东西，我就知道没那么通顺！

![](/images/ZeroNet/4.png)

我们往下拉，慢慢找错误！

![](/images/ZeroNet/5.png)

找到这一句！ImportError: No module named msgpack

又是缺少包的错误！我们继续用Pip安装msgpack，发现报错！！
按照错误提示，我以为是pip版本太低。就运行了推荐的命令pip install --upgrade pip来升级pip到最新版本。再次运行pip install msgpack还是报错！！只是这次没有了版本低的错误。。。原来这是两个错误！
无奈之下只有上网找了。

爬文许久，找到一个方法<https://www.reddit.com/r/zeronet/comments/48wlhb/zeronet_installation_problem/>

原来msgpack有一个专门开发的Python版本！！

运行

		python -m pip install msgpack-python gevent

![](/images/ZeroNet/7.png)

安装完了msgpack。再次cd到ZeroNet的项目，用Python运行start.py

		python start.py

![](/images/ZeroNet/8.png)

当看到了Starting servers....就表示成功了！

这时候等他安装和请求完一些必要的东西，浏览器自动弹出ZeroNet首页。

<http://127.0.0.1:43110/>

注册id在这个网址<http://127.0.0.1:43110/zeroid.bit>

![](/images/ZeroNet/9.jpg)

这里说一下ZeroNet。

如果你不关闭刚刚的终端去流量ZeroNet网站的话，你会发现你每次点进去一个新的网站，他会将网站整个资源都请求下来(你可以点击一下GIF TIME试试)，而且是不断刷新和请求你已经连接的网站(在CONNETCTD SITESL列表可以看到你连接的网站)

![](/images/ZeroNet/10.png)

其实你每进入一个新的网站就等于连接该网站。而且会源源不断的获取该网站的内容(可能是想动态更新到网站最新的状态)，所以这里我建议如果不是经常去的网站，可以点Delete删除掉，或者其他的Pause暂停也行。

![](/images/ZeroNet/11.png)

这里有的人想问：如果我不想ZeroNet一直占用我的带宽怎么办？人性化的是在ZeroNet你可以点击页面上的Shut down ZeroNet来关掉ZeroNet。

![](/images/ZeroNet/12.png)

好了，开始你的去中心化的匿名流量网站之旅吧~

(不要进入暗网)

下面列出一些网站导航：

中文论坛
<http://127.0.0.1:43110/1NzWeweqJ32aRVdM5UzFnYCszuvG5xV3vS>

中文论坛2：
<http://127.0.0.1:43110/1NzWeweqJ32aRVdM5UzFnYCszuvG5xV3vS>

海盗湾全球种子站：
<http://127.0.0.1:43110/1PLAYgDQboKojowD3kwdb3CtWmWaokXvfp/>

ZeroNet站点集合 & 搜索引擎
站点集合
<http://127.0.0.1:43110/138R53t3ZW7KDfSfxVpWUsMXgwUnsDNXLP/?Page:list-of-zites>

<http://127.0.0.1:43110/0list.bit/>
<http://127.0.0.1:43110/1Dt7FR5aNLkqAjmosWh8cMWzJu633GYN6u/>

搜索引擎
<http://127.0.0.1:43110/kaffiene.bit>
<http://127.0.0.1:43110/zerosearch.bit/>
<http://127.0.0.1:43110/zerofind.bit>

原创作品，欢迎转载，转载请声明出处：http://www.真无聊.com

友情链接联系:5914018@qq.com

交流QQ群：271568188

