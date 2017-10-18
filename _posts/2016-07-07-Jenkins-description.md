---

layout: post
title: "[最新Jenkins教程]持续化集成(自动打包发布)方案:\nJenkins + SVN + Fir"
description: 
headline: 
modified: 2016-07-07
category: iOS之路
tags: []
imagefeature: Jenkins.png
comments: true
mathjax: 

---

## 前言

想必大家都知道每天测试都跑来要你打包测试一下功能，不同版本打包一次，不同网络环境打包一次，不同设备打包一次。这个过程是有多痛苦和耗时间。

在我还没开始用Jenkins之前，一开始我是这样的：

	不同版本打包一次，不同网络环境打包一次，不同设备打包一次。

	然后进化了一下，开始在APP内部增加一个调试功能，方便测试人员切换网络环境。这个功能大大的缩减了我厌烦的打包工作！

	再后来我开始集成fir的发布ad-Hoc内测版。再也不用每次给每台手机打包了。

到现在我使用了Jenkins自动打包和发布，真的是解放了双手！而且越来越发现Jenkins的功能十分的强大！可以执行shell，真的很爽啊！对比以前，仿佛是从原始时代进化到了现代一样！废话不多说了，网上部署Jenkins的环境教程有很多！大家可以去看看，网上一些教程和解决方案都是老旧的，或者是根本没有说到解决方法的。我这里也列出整理过的资料和自己遇到的最恶心的Email通知问题以及一些部署问题。

## 参考文献

<http://www.jianshu.com/p/1be7281cffbc>

<http://www.jianshu.com/p/a17167274463>

<https://github.com/FIRHQ/fir-cli/blob/master/doc/publish.md>

<http://aigo.iteye.com/blog/2055610>

<http://blog.csdn.net/littlechang/article/details/8642149>

<http://zhidao.baidu.com/link?url=EUEvqH793mbU-2oXk2BfKbiqJtF5Os1TRSdSOKD28x9fQuBPz3AoAA0gtU3EQwkpwpQL91-uUR2PSYGYEzYJJTM8FVM_8n2EjFhX8o6-reC>

<http://www.cnblogs.com/zz0412/p/jenkins_jj_01.html>

<http://www.2cto.com/os/201409/334323.html>

<http://www.cnblogs.com/GGHHLL/p/jenkins.html>

## 正文

首先按照上面的文献前几个部署Jenkins环境和fir-cli。

这里说明一下Jenkins和fir的区别。Jenkins是用来自动打包程序的，fir是用来发布到内测网站fir.im上面的。原理就是Jenkins每次构建程序(打包)的时候先update一下SVN上面的文件，然后通过Xcode的xcodebuild命令来编译ipa，Jenkins再通过执行的Shell来运行Fir-cli来上传文件到fir.im。

成功部署和构建应该是这样的：

![](/images/jenkins/1.png)

下面列出一些部署过程中会遇到的问题和解决方案。

### 一、The svn command failed. Command output: svn: E155036: Please see the 'svn upgrade' command

出现这个问题主要是SVN版本的问题。因为Jenkins每次构建的时候会先update一下，但是有的SVN版本并不支持update命令。所以提示使用upgrade命令。这里我们先看看项目的配置

![](/images/jenkins/2.png)

找到“源码管理”->“Subversion”->“Check-out Strategy”

![](/images/jenkins/3.png)

一开始我以为设置这里为upgrade就可以了，狗血的是这里只有3个选项，而且都是update。后来发现其实可以在Jenkins的系统设置里面设置Jenkins的SVN版本

![](/images/jenkins/4.png)

![](/images/jenkins/5.png)

将里面默认的1.4改成最新的1.8就可以了。

![](/images/jenkins/6.png)

### 二、“-workspace”与“-target”的区别

如果你是使用了cocoaPods等插件来管理项目的，肯定会有****.xcworkspace的文件来打开项目。这时候你的Jenkins的Shell命令应该使用上面参考文献的“-workspace”命令，但是如果你没有使用cocoaPods等插件来管理项目，那么你就需要使用“-target”命令来编译程序！

![](/images/jenkins/7.png)

这里我放出我自己的Shell

	export LANG="en_US.UTF-8"

		#变量
		TARGET_NAME="xfj-adHoc"
		APPDISPLAY_NAME="xfj-adHoc"
		CODE_SIGN="iPhone Developer: tan jiehui (57EPS29NYN)"
		BUILD_DIR="${WORKSPACE}/build"
		IPA_DIR="${WORKSPACE}/ipa"
		
		#环境变量的更改
		if ${host_distribution}; then
		BUILD_CONFIG="Release"
		else
		BUILD_CONFIG="Release"
		fi
		
		#从svn更新
		svn update
		
		#首先，清除build记录：
		xcodebuild clean -target $TARGET_NAME
		#设置build号
		# xcrun agvtool new-version -all ${BUILD_NUMBER}
		#其次，执行build：
		xcodebuild build -target $TARGET_NAME BUILD_DIR=$BUILD_DIR BUILD_ROOT="${WORKSPACE}/buildRoot" CODE_SIGN_IDENTITY="$CODE_SIGN"
		
		#创建输出目录
		mkdir -p $IPA_DIR
		cp -f -r $BUILD_DIR/$BUILD_CONFIG-iphoneos/$TARGET_NAME.app.dSYM $IPA_DIR
		#最后，将app打包为ipa：
		/usr/bin/xcrun -sdk iphoneos PackageApplication -v $BUILD_DIR/$BUILD_CONFIG-iphoneos/$APPDISPLAY_NAME.app -o ${WORKSPACE}/ipa/$APPDISPLAY_NAME${BUILD_NUMBER}.ipa
		
		#更新到svn
		# svn commit -m "jenkins auto packaging and chang Bundle version to ${BUILD_NUMBER}"
		
		export LANG="en_US.UTF-8"
		#环境变量的更改
		if ${host_distribution}; then
		DESCRIPTION="网络环境:测试环境
		${fir_description}"
		else
		DESCRIPTION="网络环境:测试环境
		${fir_description}"
		fi
		
		#重命名fir包
		mv ${WORKSPACE}/ipa/$APPDISPLAY_NAME${BUILD_NUMBER}.ipa ${WORKSPACE}/ipa/$fir_app_name${BUILD_NUMBER}.ipa
		
		#上传到fir
		/usr/local/bin/fir publish ${WORKSPACE}/ipa/$fir_app_name${BUILD_NUMBER}.ipa -c "${DESCRIPTION}"
		
仅供参考，大家可以在我的基础上修改。如果是workspace的修改参考文献的Shell就可以了。

### update in 2016.12.02

新的shell方法简化构建过程
参考：

	export LANG="en_US.UTF-8"
	
	#从svn更新
	svn update
    
    #使用fir buid_ipa 进行build项目，并上传到fir
    fir build_ipa ${WORKSPACE} -o ${WORKSPACE}/ipa -p -c ${changelog这个是构建参数，要添加} -Q -T 你的fir token

### 三：恶心的E-mail邮件通知问题！

在我按照教程来设置构建程序的邮件通知时，我发现死活不行！我使用的是QQ邮箱，已经开通了POP3/SMTP服务和IMAP/SMTP服务，无论是使用Jenkins的自带邮件通知还是第三方插件：Extended E-mail Notification还是不行！最后发现原来是即使你全部设置正确了，但是默认还是不会给你分配构建成功的通知。只分配默认的构建失败的通知！下面给出正确的设置图

QQ邮箱的：

![](/images/jenkins/8.png)

Jenkins的系统设置(全局设置)

![](/images/jenkins/9.png)

![](/images/jenkins/10.png)

![](/images/jenkins/11.png)

然后是对应的项目里面的“配置”，我们找到最下面的，增加构建步骤，我们新增就可以了。

![](/images/jenkins/12.png)

关键：即使你全部设置正确了，但是默认还是不会给你分配构建成功的通知。只分配默认的构建失败的通知！那么在哪里配置这个成功或者失败的邮件通知呢？为了找到这个问题我花了几个小时。。。。为了解决这个问题又花了几个小时。。。想想也是醉了！

现在网上的资料都是这样的图片

![](/images/jenkins/13.png)

我找了几个小时都找不到！搜索也是没有什么结果。。。。

然而，我发现原来是这个插件更新了版本，没有那样子的显示的。而是变成了：

![](/images/jenkins/14.png)

我也是醉了！吐了一口血。。。。我们往里面新增一个SUCCESS的就可以在每次构建程序成功的时候收到E-mail通知了！当然你还可以新增其他的通知。

下面来一张成功的图片：

![](/images/jenkins/15.png)

### 四、对于定时构建程序，你可以设置“构建触发器”来定时构建程序

![](/images/jenkins/16.png)

我这里使用SCM来构建，参数是：“H * * * *”意思是每天的每个小时的第0秒如果我的Jenkins还打开着那就构建一次程序并发布。

这里有点同学会很有疑问，里面的参数到底这么设置呢？我这里给你找来了答案：

	第一个参数代表的是分钟 minute，取值 0~59；
	第二个参数代表的是小时 hour，取值 0~23；
	第三个参数代表的是天 day，取值 1~31；
	第四个参数代表的是月 month，取值 1~12；
	最后一个参数代表的是星期 week，取值 0~7，0 和 7 都是表示星期天。
	如0 * * * * 表示的就是每个小时的第 0 分钟执行一次构建。



至此，你就可以解放双手了！再也不要烦恼于打包给测试了！我的天！

原创作品，欢迎转载，转载请声明出处：http://www.真无聊.com
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
