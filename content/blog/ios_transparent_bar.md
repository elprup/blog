+++
date = "2015-01-23T07:01:46Z"
title = "iOS 8开发：半透明的状态栏 (StatusBar)"
slug = "ios_transparent_statusbar"
+++

# iOS 8开发：半透明的状态栏 (StatusBar)
## 问题  

状态栏是app最顶端的包括运营商图标的那一栏。一般app的状态栏颜色和导航栏（navigation bar）一致。如果没有导航栏，在scroll view下（比如table view）会出现文字重叠的BUG（如下图）

[![](http://s3.sinaimg.cn/mw690/0024ZInHzy6PoVp48Nke2)](http://photo.blog.sina.com.cn/showpic.html#blogid=&url=http://album.sina.com.cn/pic/0024ZInHzy6PoVp48Nke2)

目标得到一个半透明的状态栏，如下图

[![](http://s8.sinaimg.cn/mw690/0024ZInHzy6PoVpRBcze7)](http://photo.blog.sina.com.cn/showpic.html#blogid=&url=http://album.sina.com.cn/pic/0024ZInHzy6PoVpRBcze7)

## 分析

在iOS 8之前，系统会提供默认的全局状态栏style，这是一个全局变量，在pinfo.list中设定，基本的风格区别在于字体颜色是黑色还是白色。这样统一设置的缺点是灵活性不够，因为可能在某个页面下需要黑色字体的状态栏，而某些地方需要白色。iOS 8中修改了这种设定，状态栏默认值设置为透明，并提供了基于view controller的状态栏风格设定。
但iOS 8取消了对半透明状态栏的风格的默认支持，所以，直接设定这种半透明状态栏风格的想法不可行，需要尝试点不一样的。
苹果官方的交互指南
（HumanInterface Guideline）中对于状态栏的指南中提到了这个问题，这是解决方案的最大线索：
```
防止滑动内容穿过状态栏显示：
1. 使用导航栏显示内容
2. 新建一个自定义图像显示在状态栏下
3. 定位内容以避开状态栏
```
其实苹果的状态栏风格可以认为是状态栏**字体颜色风格**，对于背景的设置需要通过程序设置。我们采用的做法是方法2，**新建一个半透明自定义图像在状态栏下**。

## 解决方案

1. 把Table View 放入一个 navigation bar 中
```
FeedViewController \*vc1 = [[FeedViewController alloc]init];
UINavigationController\*navController = [[UINavigationController alloc]initWithRootViewController:vc1];
```
2. 将navigation bar 设置为默认不可见
```
[[self navigationController] setNavigationBarHidden:YESanimated:YES];
```
3. 消除不可见navigation bar以后navigation bar和第一个cell之间的空白：
```
self.automaticallyAdjustsScrollViewInsets= NO;
```
4. 默认table view会从状态栏开始，所以需要留给状态栏位置
```
//keep space for status bar
self.tableView.contentInset = UIEdgeInsetsMake(20, 0, 0, 0);
self.tableView.scrollIndicatorInsets = UIEdgeInsetsMake(20, 0, 0, 0);
```
5. 将一个半透明的view插入到导航栏顶部
```
UIView *topGradientView = [[**UIView** alloc]initWithFrame:**CGRectMake**(0, 0, [**Util**getScreenWidth], 20)];
topGradientView.backgroundColor= [**UIColor** colorWithWhite:1.0 alpha:0.9];
[navController.viewaddSubview:topGradientView];
```
## 参考资料
https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/Bars.html
http://stackoverflow.com/questions/19111451/ios-7-uitableview-how-to-remove-space-between-navigation-bar-and-first-cell  

http://stackoverflow.com/questions/2926914/navigation-bar-show-hide
