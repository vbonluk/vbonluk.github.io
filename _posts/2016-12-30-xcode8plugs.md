---

layout: post
title: "xcode8plugs"
description: 
headline: 
modified: 2016-12-30
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

### 参考链接

<https://github.com/fpg1503/MakeXcodeGr8Again>

<http://www.cocoachina.com/bbs/read.php?tid-1705417.html>

<http://blog.csdn.net/hdfqq188816190/article/details/50668243>

### 方法

下载Xcode Gr8项目。然后build项目

<https://github.com/fpg1503/MakeXcodeGr8Again>

![](http://oapglm9vz.bkt.clouddn.com/1483092408.png )

将生成的东西放桌面，双击运行。

![](http://oapglm9vz.bkt.clouddn.com/1483407767.png )

打开Finder将Xcode放进去。等十来分钟。就完成了对Xcode的unsigned以及复制一个独立的Xcode出来。

![](http://oapglm9vz.bkt.clouddn.com/1483407786.png )

这时候我们会发现多出来的Xcode叫：XcodeGr8。

![](http://oapglm9vz.bkt.clouddn.com/1483407811.png )

注意：XcodeGr8，我们只用来编码项目，并不用于发布APP。发布APP请使用原版。原因是XcodeGr8进行了破解，会导致上传出问题。

接下来我们安装Xcode插件管理工具：Alcatraz [点我](https://github.com/alcatraz/Alcatraz) 下载项目build。重启XcodeGr8.

如果这时候没有弹出：Load Bundle窗口，那么我们需要在Alcatraz项目中添加Xcode8的UUID，以我这里的Xcode8.2.1为例。使用终端获取Xcode的UUID，命令如下：

	defaults read /Applications/Xcode.app/Contents/Info DVTPlugInCompatibilityUUID
	
得到UUID(E0A62D1F-3C18-4D74-BFE5-A4167D643966)后将UUID放进项目中的Info.plist,重新build项目，这时候我们重启XcodeGr8就可以发现Load Bundle弹窗出来了，我们点击Load Bundle加载插件。

![](http://oapglm9vz.bkt.clouddn.com/1483408118.png )

运行了Alcatraz并装好了需要的插件发现并不是每个插件都可以运行的，这时候大部分原因是UUID的问题了。我们打开终端使用命令为每个插件添加UUID：

	find ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins -name Info.plist -maxdepth 3 | xargs -I{} defaults write {} DVTPlugInCompatibilityUUIDs -array-add `defaults read /Applications/XcodeGr8.app/Contents/Info.plist DVTPlugInCompatibilityUUID`
	
再重启XcodeGr8，发现弹出框再次出现，我们依旧选择Load Bundle加载插件。发现想要的插件都可以使用了。

以上~
-------------------------------
关于Xcode8.1
经测试8.1可以用没问题

如过resign后出现闪退的问题,可能是旧插件导致
可以尝试清空这两个文件夹的全部内容
~/Library/Application Support/Developer/Shared/Xcode/Plug-ins
/Users/你的用户名/Application\ Support/Developer/Shared/Xcode/Plug-ins

如果出现不能调用命令行resign的情况
1.检查钥匙串中填写的信息是否一致
2.检查终端命令是否打错(直接复制不会出错)
3.检查xcode是否纯净没被修改过

---关于KSImageNamed图片名代码提示插件在Xcode8.1上不可用
https://github.com/ksuther/KSImageNamed-Xcode

由于KSImageNamed这个插件的存放位置比较特殊,需要手动添加uuid

下载后用xcode打开项目 然后在 plist里面添加xcode8.1的uuid 之后bulid项目即可安装成功

分享一下我自己的插件列表吧：

[Backlight](https://github.com/limejelly/Backlight-for-XCode) Xcode主题

[CleanHeaders](https://github.com/insanoid/CleanHeaders-Xcode) 清除重复import

[ColorSenseRainbow](https://github.com/NorthernRealities/ColorSenseRainbow) 实时显示UIColor颜色以及支持取色。

[DefaultMarginDisabler](https://github.com/mshibanami/DefaultMarginDisabler)默认关闭AutoLayout的Margin的8像素

[DerivedData Exterminator](https://github.com/kattrali/deriveddata-exterminator) 给Xcode增加一个清除缓存的按钮

[DXXcodeConsoleUnicodePlugin](https://github.com/dhcdht/DXXcodeConsoleUnicodePlugin)给控制栏添加编码支持，解决中文乱码问题。

[FuzzyAutocomplete](https://github.com/FuzzyAutocomplete/FuzzyAutocompletePlugin)增强Xcode的代码提示插件。

[HighlightSelectedString](https://github.com/keepyounger/HighlightSelectedString)给选中的字符高亮

[Injection Plugin](https://github.com/johnno1962/injectionforxcode)一个实时在模拟器显示代码效果的插件！不用每次重新编译。

[KSImageNamed-Xcode](https://github.com/ksuther/KSImageNamed-Xcode)为Xcode增加UIImage图片提示。

[KZLinkedConsole](https://github.com/krzysztofzablocki/KZLinkedConsole)将控制栏中的log的frame转换成可以显示的样式。

[Miku](https://github.com/poboke/Miku)一个装逼插件，在Xcode显示一个动画。

[Peckham](https://github.com/markohlebar/Peckham)在随意地方import头文件，不用在最上面import。

[SCXcodeMiniMap](https://github.com/stefanceriu/SCXcodeMiniMap)模仿SubLimeText中的代码小地图。

[VVDocumenter](https://github.com/onevcat/VVDocumenter-Xcode)代码注释一键生成

[XActivatePowerMode](https://github.com/qfish/XActivatePowerMode)装逼利器，敲代码时候的特效。

[XAlign](https://github.com/qfish/XAlign)一键自动对齐真个文件的代码。

[XcodeColors](https://github.com/robbiehanson/XcodeColors)控制台不同颜色区分不同信息。
	

原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
