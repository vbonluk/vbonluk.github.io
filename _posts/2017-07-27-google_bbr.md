---

layout: post
title: "SS(SSR) + Google BBR 加速,翻墙速度原地起飞！"
description: 
headline: 
modified: 2017-07-27
category: iOS之路
tags: [ss,ssr,Google BBR,bbr,加速,翻墙,科学上网,ss加速,ssr加速]
imagefeature: 
comments: true
mathjax: 

---

在之前的这一篇[博客](http://xn--rgvu79ah1g.com/ios%E4%B9%8B%E8%B7%AF/kcptun_ssr)介绍了KcpTun的ss加速方案。那么还有没有其他方案呢，其实网上比较出名是有很多的例如：锐速，FinalSpeed等。但是最近似乎大家更加关注到一个新的加速方案：google BBR。这个加速是Google出的。他的加速效果如何呢？下面我们从部署到测速一步一步慢慢看。

想要了解更多ss，ssr，shadowsocks shadowsocksR的请浏览篇文章。本文适合已经了解并熟悉ss，ssr的同学阅读。<http://xn--rgvu79ah1g.com/ios%E4%B9%8B%E8%B7%AF/kcptun_ssr>

### 部署

#### 服务器端

##### 第一步：

	ssh远程连接你的VPS。这里我用帮瓦工做例子。
	
值得注意的是帮瓦工VPS默认的系统是Centos6，而且有的是32位(x86)，有的是64位(x64)。大家一定要留意自己的系统是多少的。google BBR只支持64位系统！！！

如果你的是帮瓦工，可以在 KiwiVM 的 Main controls 看到系统信息。

![](http://oapglm9vz.bkt.clouddn.com/1501121903.png )

不知道怎么进入KiwiVM的看下面：

![](http://oapglm9vz.bkt.clouddn.com/1501121997.png )

登录帮瓦工后，点击 Services 点击 My Service。

![](http://oapglm9vz.bkt.clouddn.com/1501122118.png )

找到 KiwiVM Control Panel 点击。

如果你的系统是Centos6 x86的，也就是32位的，那么如果要使用BBR的话请重装系统。我就是默认帮瓦工给我32位的，最后直接重装到了centos7。需要重装的看下图：

![](http://oapglm9vz.bkt.clouddn.com/1501122474.png )

首先点击 Stop 停止运行。需要的时间，等待停止成功后再操作下一步。

![](http://oapglm9vz.bkt.clouddn.com/1501122552.png )

点击 Reload 重装。重装需要时间，请等待。

重装完成后首先重新安装ssr。安装ssr的可以参考[这里](http://xn--rgvu79ah1g.com/ios%E4%B9%8B%E8%B7%AF/kcptun_ssr)

安装完ssr，并且测试了ssr是可用之后，我们进行下一步，安装BBR。

如果没有使用帮瓦工的，并且系统是64位的，可以直接忽略上面说的了，下面开始部署BBR。

##### 第二步：

我们使用网上最近很过的“魔改版”BBR来部署。并且使用扩软的一键脚本来安装。

ssh远程到VPS后执行下面命令下载脚本：

	wget https://raw.githubusercontent.com/kuoruan/shell-scripts/master/ovz-bbr/ovz-bbr-installer.sh
	
	chmod +x ovz-bbr-installer.sh

下面我们运行脚本：
	
	./ovz-bbr-installer.sh
	
![](http://oapglm9vz.bkt.clouddn.com/1501123171.png )

这里需要输入加速的端口，也就是你ss或者ssr的端口。这里我使用了ssr安装默认的端口8388。

![](http://oapglm9vz.bkt.clouddn.com/1501123227.png )

部署成功后：

![](http://oapglm9vz.bkt.clouddn.com/1501123305.png )

没错就是这么简单。我们只需这么几步就可以部署完成了，而且无需客户端，要知道KcpTun是需要配合客户端来运行的，也就是每次都要开启ssr和Kcptun客户端两个软件，那么BBR是在服务器端的，无需客户端配合，是不是方便了很多呢。

### 测速

下面放出使用Kcptun和google BBR的加速效果图。

Kcptun的加速效果：

![](http://oapglm9vz.bkt.clouddn.com/1500957091.png )

BBR的加速效果：

![](http://oapglm9vz.bkt.clouddn.com/1501123531.png )

得出的效果是BBR明显优于Kcptun，这里的结果也是不一定，因为我测速的时间段不一致，因为我在安装BBR的时候已经卸载了Kcptun，所以测速的结果可能会有时间上的误差，宽度的误差。具体效果还是需要你们具体去尝试。但是最起码有一点好的，就是无需客户端了。这一点十分适合我Mac端。再也无需开机执行shell脚本了。2.5M每秒的速度也是秒了4K视频。是在是

太爽了~~~



原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
