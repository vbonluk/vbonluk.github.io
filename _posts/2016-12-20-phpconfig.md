---

layout: post
title: "Mac下Apache + PHP 配置"
description: "开启你的第一个PHP页面"
headline: 
modified: 2016-12-20
category: iOS之路
tags: []
imagefeature: 
comments: true
mathjax: 

---

### 参考资料：

<http://www.cnblogs.com/long-gengyun/p/4080144.html>

<http://blog.csdn.net/yanzi1225627/article/details/45075265>

本文主要参考以上两篇文章进行整理，因为以上两篇文章单独尝试的话发现并不能成功部署。下面整理出正确的步骤，以备日后重新部署。

### Step 1.

	开启apache服务 sudo apachectl start 
	停止apache服务 sudo apachectl stop 
	重启服务 sudo apachectl restart 
	查看版本 httpd -v
	
手动打开apache服务后，在浏览器输入localhost，将看到如下

![](http://oapglm9vz.bkt.clouddn.com/1482214472.png )

### Step 2.

首先创建用户目录：
	
	mkdir ~/Sites 

此时会在当前用户的根目录下创建一个Sites目录,我们新建一个名为firstPHP.php的测试文本，内容如下：

	<?php
	//输出文本内容：test
	echo 'test';

![](http://oapglm9vz.bkt.clouddn.com/1482214809.png )

### Step 3.

进到cd /etc/apache2/users/目录下,看看

![](http://oapglm9vz.bkt.clouddn.com/1482214950.png )

找到"用户名.conf"这个文件，如果没有，就新建一个你的用户名的conf文件。

内容为：

	<Directory "/Users/yanzi/Sites/">
		AllowOverride All
		Options Indexes MultiViews FollowSymLinks
		Require all granted
	</Directory>


新增完毕，给文件加权限：

在终端执行：

	sudo chmod 775 /private/etc/apache2/users/用户名.conf
	
![](http://oapglm9vz.bkt.clouddn.com/1482215141.png )


注意：如果在桌面新增的,先拉进去/private/etc/apache2/users/这个路径，然后再加权限。

保存文件，重启apache ，sudo  apachectl restart

此时打开safari，访问http://localhost/~username/

此时页面提示～username服务器不存在，我们还需要进入下一步的设置

### Step 4.

修改apache的httpd.conf文件

找到/etc/apache2/httpd.conf，修改之。

找到以下信息，将其前面的＃去掉：
	
	LoadModule php5_module libexec/apache2/libphp5.so
	LoadModule authz_core_module libexec/apache2/mod_authz_core.so
	LoadModule authz_host_module libexec/apache2/mod_authz_host.so
	LoadModule userdir_module libexec/apache2/mod_userdir.so
	Include /private/etc/apache2/extra/httpd-userdir.conf
	
修改/etc/apache2/extra/httpd-userdir.conf

找到一下信息修，将其前面的＃去掉：

	Include /private/etc/apache2/users/*.conf
	
重启apache，sudo apachectl restart

此时访问：http://localhost/~username/

![](http://oapglm9vz.bkt.clouddn.com/1482215395.png )

<font color=#DC143C size=3>显示：“test”，恭喜你，Apache配置成功！</font>




原创作品，欢迎转载，转载请声明出处：<http://www.真无聊.com>
 
友情链接联系:5914018@qq.com
 
交流QQ群：271568188
