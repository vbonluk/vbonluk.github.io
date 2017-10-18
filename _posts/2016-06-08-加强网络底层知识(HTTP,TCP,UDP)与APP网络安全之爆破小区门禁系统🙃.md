---

layout: post
title: "加强网络底层知识(HTTP,TCP,UDP)与APP网络安全之爆破小区门禁系统🙃"
description: 
headline: 
modified: 2016-06-08
category: 乱七八糟
tags: []
imagefeature: 
comments: true
mathjax: 

---
 
### 前言

最近租的小区里面新装了一套门禁系统，支持手机解锁，感觉是不错的功能。但是每次走到小区楼下都要打开APP，登录APP，操作APP，选择打开的楼层，个人感觉略有不爽，我就是只要一个解锁的就行了！于是乎打算破解这个app拿到里面的接口来调用，做一个一键开锁的功能在手机上(直接用自己的账号自动登录请求接口)，无需过多的点击。心想了挺久的一直没去实现，昨天开始弄这个了。

#### 参考文献

<http://my.oschina.net/u/1585857/blog/479306>

<http://zhidao.baidu.com/link?url=qw82nAPlsdDqLYpzA_MQTXvDNar__VT9KkhlHN6uKisvVvkIu2uFzOHfXOqUWqsW8FG8-9SdwKzRYYP0AvgTeJQlBeQabXrPLIfO2Huhtqq>

<http://blog.csdn.net/xluren/article/details/38535565>

<http://www.cnblogs.com/wangkangluo1/archive/2011/12/19/2293750.html>

<http://blog.csdn.net/cumirror/article/details/7054496>

第一步当然是打算用Charles来拦截接口啦，(其实我也想过直接破解app，但是目前的技术通常都是通过class-dump和iOSOpenDev来拿到头文件而已，并不能拿到.m文件。其实就算拿到了.m文件，也是请求接口的。所以我这里优先开始通过第三方拦截的方式来看看接口到底是什么鬼)

连上WiFi，打开Charles，打开APP。

我们点开Safari浏览百度看看有没有成功代理，出现下图这样应该就是可以的了。

![](/images/pojiexiaoqu/1.png)

通过Charles我们可以看到一些接口的详细。确认可以后，我们点开需要破解的APP(先清除掉Charles其他请求或者添加过滤)，

![](/images/pojiexiaoqu/4.png)

这里我们暂时还不知道域名，所以不做Charles的过滤，先清一下请求再进app就可以知道域名了。

![](/images/pojiexiaoqu/2.png)

我找到了tcc.taichuan.com这个域名，点开它，发现登录app的账号和密码居然是明文传输的！被吓到了有木有！然而接下来的更可怕！我们继续点开
Charles拿到的数据，发现居然连用户的详细信息都全部泄露了！！吓得我都想赶紧取消这个服务了！个人隐私全无呀。

![](/images/pojiexiaoqu/3.png)

先不吐槽APP的安全性问题，我们继续要进行的突破，我们到操作APP到解锁的界面尝试获取APP的解锁接口。

![](/images/pojiexiaoqu/5.png)

用Charles的过滤直接只拿tcc的域名相关，

![](/images/pojiexiaoqu/6.png)

可以看到Charles拿到的接口就是APP上面的显示的数据！当然还包括了各种重要的用户信息，一览无遗。接下来我们点选项中的随便一个来开锁。同时看Charles里面有没有进行请求。

![](/images/pojiexiaoqu/6.png)

咦！发现我每次点解锁都是进行了数据的拉取，而解锁的请求并没有进行！Charles一点反应都没有哦！多次尝试均无请求，百思不得其解。这时候我就想到了以前想抓取QQ和微信的聊天内容的尝试，发现QQ的倒是可以拿到加密后的数据，但是微信的也是拿不到的。我立即就醒起了，Charles这个软件只能抓取HTTP协议的请求，对于其他协议是不支持的！我觉得既然他是解锁的，必然不能那么简单不安全的进行不加密的http请求吧。会不会走的是TCP或者UDP协议呢？

我觉得十分有可能！因为我多次尝试了，Charles在我点击解锁的时候均无拦截！于是打开之前用过的神奇WireShark！这个神奇可以监听到绝大部分协议的请求！

![](/images/pojiexiaoqu/7.png)

这里我们选择WiFi，因为我是通过WiFi来是电脑和手机出于同一个网络的。原理和Charles一样。

![](/images/pojiexiaoqu/8.png)

点开就可以看到源源不断的请求在进行着！包含了各种协议！

这些请求来自自己电脑和手机的。我们在这里可以看到Charles看不到的请求，例如来自QQ的请求。

![](/images/pojiexiaoqu/9.png)

WireShark真的十分之强大，还收录了腾讯的协议，看我圈出的地方就可以知道。OICQ IM software ，popular in china。

![](/images/pojiexiaoqu/10.png)

我们可以看到QQ的数据包是加密的。但是本次QQ不在我们研究的范围之内，我们通过WireShark的过滤来筛选出来自手机的请求吧。有意思的是WireShark的过滤是有语法要求的。一开始我是这样的：

![](/images/pojiexiaoqu/11.png)

发现并不能进行查询！查了一下语法，我们可以这样写：

	ip.dst==192.168.9.100

这样就可以过滤出手机的请求了(192.168.9.100是我手机的IP地址)。

![](/images/pojiexiaoqu/12.png)

具体的过滤语法可以看看[这里](http://www.cnblogs.com/wangkangluo1/archive/2011/12/19/2293750.html)

这时我们清除掉已经拦截的请求，操作手机APP到解锁的步骤，我们来看看它到底进行了什么请求！

![](/images/pojiexiaoqu/13.png)

终于看到了！原来他是走了TCP协议！最后那一步才走的HTTP协议哦！这里一目了然！我们先来看最后一个HTTP请求，选择最后一个请求，点击下面的'eXtenSible Markup Language'可以看到明文数据。

![](/images/pojiexiaoqu/14.png)

为了好看些，我们点开接口来看

![](/images/pojiexiaoqu/15.png)

你会发现是和Charles拦截到的是一样的！所以我们这里是正确拿到了请求！

好了既然我们现在知道了它在开锁的时候不仅仅进行HTTP请求，而且还有TCP连接！那么我大胆得猜测他是通过TCP保持连接，用户点击APP上面的解锁，就会通过TCP发送给服务端一个解锁的数据包来进行解锁！那么好了，我们来研究一下APP进行过的几个TCP连接。

这里我们来科普一下TCP，具体的大家自己去查吧，我这里就贴一张图。

![](/images/pojiexiaoqu/16.png)

大伙查阅过资料都应该知道了，TCP的三次握手和4次挥手应该是比较容易理解的！

选择其中一个TCP连接，我们会看到底下Wirshark已经帮我们将数据归好类了，我们会看到几个层级:Frame->Ethernet II->Internet Protocol Version 4->Transmfssion control protocol。一开始我也不懂为何这样分布，但是通过查阅资料得知，这其实就是底层网络的对应的OSI七层模型！下面借用网络上的一张图片来说明：

![](/images/pojiexiaoqu/18.png)

我们点开TCP里面的Transmission Control Protocol(有的同学会好奇这个是什么，其实这个就是TCP的全称)，找到Flags标志位，

![](/images/pojiexiaoqu/17.png)

我们看到APP进行的TCP连接所有的Flags标志位都是ACK状态！也就是说我们在进行这个解锁操作之前已经建立了TCP连接！应该是登录成功后就开始建立TCP连接来传输数据！所以我们这里不会看到SYN这个握手标志。

我们来看最底下的TCP传输的数据包内容，你会发现并未有加密！

![](/images/pojiexiaoqu/19.png)

我们可以清楚地看到里面的数据。我们可以从几个TCP连接里面分别找到数据包来拿数据。进行破解。由于在主面板下面显示的数据内容不好直接观察，我们右键某一个TCP连接，选择跟踪流，再选择TCP流。来打开可读性高一点的tcp流信息。

![](/images/pojiexiaoqu/20.png)


![](/images/pojiexiaoqu/21.png)

可以看到文件中有点数据中间有“........”或者“12..3.....”这些我们可以忽略。那么里面的信息就可以完全整合出来了！

更新(2016.06.12)

我们可以在下面选择分组显示数据。可以将发送的数据分为两组：

	手机——>服务器
	服务器——>手机

如图：

![](/images/pojiexiaoqu/22.png)

通过ip指向(192.168.9.101:8888 -> 192.168.9.100:50559)可以知道这是手机向服务器发送的数据。

至此，我们可以着手开始APP的破解了！由于破解过程会涉及APP安全性问题，我这里暂不和大家探讨了。大伙自己研究吧。本文只给予一种研究的思路以及过程。大伙以后可以对自己APP进行第三方拦截的方式来看看安全性是否做到位了！其实接口可以使用HTTPS来进行连接，返回来的数据进行加密，就可防范大多数的第三方拦截攻击了！

更多功能欢迎大家继续研究讨论。

原创作品，欢迎转载，转载请声明出处：http://www.真无聊.com
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
