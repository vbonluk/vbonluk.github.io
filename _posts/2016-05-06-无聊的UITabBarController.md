---

layout: post
title: "无聊的UITabBarController"
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

## Objective-C：

在AppDelegate.m中

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
		

加入

	UITabBarController *tabBarController = (UITabBarController *)self.window.rootViewController;
	
	for (UITabBarItem *item in tabBarController.tabBar.items) {
        item.image = [item.image imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
        item.selectedImage = [item.selectedImage imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
    }
    
    [[UITabBarItem appearance] setTitleTextAttributes:@{ NSForegroundColorAttributeName : [UIColor colorWithRed:147.0/255.0 green:147.0/255.0 blue:147.0/255.0 alpha:1.0 ],NSFontAttributeName:[UIFont systemFontOfSize:12.0f]} forState:UIControlStateNormal];
    
    [[UITabBarItem appearance] setTitleTextAttributes:@{ NSForegroundColorAttributeName : [UIColor colorWithRed:255/255.0 green:80/255.0 blue:0/255.0 alpha:1.0],NSFontAttributeName:[UIFont systemFontOfSize:12.0f]} forState:UIControlStateSelected];

## Swift：

在AppDelegate.swift中

	func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
	
	}

加入

	let tabBarVC = self.window!.rootViewController as? UITabBarController;
        for vc in tabBarVC!.viewControllers! {
            vc.tabBarItem.selectedImage = vc.tabBarItem.selectedImage?.imageWithRenderingMode(UIImageRenderingMode.AlwaysOriginal)
        }
        let titleDicNormal = [NSForegroundColorAttributeName: UIColor.init(colorLiteralRed: 147.0/255.0, green: 147.0/255.0, blue: 147.0/255.0, alpha: 1.0) , NSFontAttributeName : UIFont.systemFontOfSize(12.0)]
        UITabBarItem.appearance().setTitleTextAttributes(titleDicNormal, forState: UIControlState.Normal)
        
        let titleDicSelected = [NSForegroundColorAttributeName: UIColor.init(colorLiteralRed: 255/255.0, green: 80/255.0, blue: 0/255.0, alpha: 1.0) , NSFontAttributeName : UIFont.systemFontOfSize(12.0)]
        UITabBarItem.appearance().setTitleTextAttributes(titleDicSelected, forState: UIControlState.Selected)

显示原图以及字体颜色有相应的变化

# 下次直接拷贝使用或者新建一个UITabBarController的类目来使用。。。

使用前：

![](/images/UITabBarController/1.png)

使用后：

![](/images/UITabBarController/2.png)

原创作品，欢迎转载，转载请声明出处：http://www.真无聊.com

友情链接联系:5914018@qq.com

交流QQ群：271568188