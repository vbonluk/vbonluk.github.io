---

layout: post
title: "翻墙 科学上网 防查水表 SS SSR 加速 加密 安全 可靠 YouTube 1080p一步到位"
description: 
headline: 
modified: 2017-07-25
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

本文所讨论的科学上网技巧只用于获取国外资讯、技术文章、技术博客等。无意冲撞国家政策。如有违反或者带来不适请[邮箱](5914018@qq.com)联系本人。

有关于ss以及ss加速的请看我之前的博客。[点我](http://xn--rgvu79ah1g.com/ios%E4%B9%8B%E8%B7%AF/speed-up)

这里所讨论的是最新的，更为安全可靠的科学上网技术。

首先我们来看一下最近发生的事情：

![](http://oapglm9vz.bkt.clouddn.com/1500949632.png )

![](http://oapglm9vz.bkt.clouddn.com/1500950113.png )

![](http://oapglm9vz.bkt.clouddn.com/1500950149.png )

![](http://oapglm9vz.bkt.clouddn.com/1500950228.png )

![](http://oapglm9vz.bkt.clouddn.com/1500950305.png )

![](http://oapglm9vz.bkt.clouddn.com/1500950326.png )

看到这么多有的没的，先不管事情真假，在我看来现在以及未来科学上网的环境将会越来越艰难。为了规避嗅探，我们很有必要使用ssr的混淆策略了。因为目前只有ssr为最佳策略，别无他选。

因为搬瓦工用户量剧增，导致其速度慢了很多，上YouTube默认只能带动320p了。实在无法忍受。故，下面介绍的是：KcpTun + SSR 的科学上网策略。使用后的效果，上YouTube默认HD 1080p看片。

### KcpTun

#### 服务器配置

部署方法参考：<http://xn--rgvu79ah1g.com/ios%E4%B9%8B%E8%B7%AF/speed-up>

需要注意的是文章里面的KcpTun版本是旧版的，现在已经更新到最新版。如果已经安装过KcpTun的请ssh到服务器运行Kcptun脚本，更新一下就行。还有一点就是客户端连接的时候记得使用匹配的“client_darwin_amd64”文件。注意：Kcptun版本对应client_darwin_amd64文件，弄错了是无法连接上的。

下面是我的Kcptun服务器配置图，给大家参考一下：

![](http://oapglm9vz.bkt.clouddn.com/1500951222.png )

注意：

Mac下没有运行kcptun的客户端，Windows下有客户端。

#### Mac

利用终端来运行客户端。

![](http://oapglm9vz.bkt.clouddn.com/1500951716.png )

![](http://oapglm9vz.bkt.clouddn.com/1500951830.png )

#### Windows

![](http://oapglm9vz.bkt.clouddn.com/1500951875.png )

Windows直接下载kcptun_gclient文件，填好参数就可以运行了。kcptun_gclient文件在上面的部署参考链接可以找到。

具体配置：

![](http://oapglm9vz.bkt.clouddn.com/1500952128.png )


### SSR

#### 服务器配置

安装参考链接：<https://github.com/breakwa11/shadowsocks-rss/wiki/Server-Setup>

需要注意的是：

![](http://oapglm9vz.bkt.clouddn.com/1500952411.png )

这一步十分重要，我们需要修改config.json文件来配置好混淆策略以及连接协议。不建议使用帮瓦工自带的一键安装ssr，使用之后无法编辑config.json。这里按照作者的推荐我们使用如下配置：

协议：

protocol：auth_chain_a

![](http://oapglm9vz.bkt.clouddn.com/1500952822.png )

混淆策略：

obfs: tls1.2_ticket_auth_compatible

![](http://oapglm9vz.bkt.clouddn.com/1500952809.png )

注意：

![](http://oapglm9vz.bkt.clouddn.com/1500952950.png )

这也就是为什么上面我们定义的加密为none的原因。

kcptun里面定义加密的字段是：crypt

ssr的则是：method

这两点需要注意一下。

下面给出我们的服务器ssr配置图(也就是服务器上面的ssr文件夹里面的config.json文件)给大家参考一下：

![](http://oapglm9vz.bkt.clouddn.com/1500953153.png )


关于协议以及混淆策略更详细的参考链接：<https://github.com/breakwa11/shadowsocks-rss/blob/master/ssr.md>

#### Mac

![](http://oapglm9vz.bkt.clouddn.com/1500952187.png )

我们使用ssr软件。注意使用ssr软件建议关闭ss软件。

![](http://oapglm9vz.bkt.clouddn.com/1500952264.png )

配置如下：

![](http://oapglm9vz.bkt.clouddn.com/1500952312.png )

#### Windows

我们使用ssr软件。注意使用ssr软件建议关闭ss软件。

![](http://oapglm9vz.bkt.clouddn.com/1500953280.png )

配置如下：

![](http://oapglm9vz.bkt.clouddn.com/1500953252.png )

### 写在最后

一般来说看完第一次后，肯定也是连不上的，因为看完上面的，大多数人也还没配置成功，原因一般都是某个步骤出错了。解决的办法还是自己多琢磨一下服务器端与客户端需要匹配的原理。慢工出细活。希望大家成功加速以及SSR部署成功。下面放一张加速以及混淆后的YouTube1080p测速图。

![](http://oapglm9vz.bkt.clouddn.com/1500957091.png )





原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
