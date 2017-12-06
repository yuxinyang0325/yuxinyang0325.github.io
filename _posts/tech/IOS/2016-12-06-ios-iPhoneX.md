---
layout: post
title: iPhoneX适配
category: 技术
tags: iOS, iPhoneX, 适配
description:
---


> iPhoneX来了，作为一名iOS Developer🙄，又一次踏上了适配之路...

### 适配启动图
如果在iPhoneX启动你的应用发现上下都有黑边，那你的LaunchImage启动方案肯定是Assets。
解决方法：
1  在LaunchImage对应的Assets**文件夹**里中添加一张像素**1125 * 2436**的启动图。（可以按照苹果规范命名为**Default-iOS11-812h@3x.png**）
2  在对应的Contents.json文件中增加下面的内容(**注意JSON Array元素分割符**)
```json
{
    "orientation" : "portrait",
    "extent" : "full-screen",
    "idiom" : "iphone",
    "subtype" : "2436h",
    "filename" : "启动图名字.png",
    "minimum-system-version" : "11.0",
    "scale" : "3x"
}
```
**或者放弃Assets 直接用[LaunchScreen.storyboard](http://www.jianshu.com/p/77054dccafdb)作为启动图**

### 导航栏高度
* iOS 7之前
状态栏不在屏幕高度内，所以导航栏高度：**44pt**

* iOS 7 ~ iOS 10
状态栏：**20pt**
导航高度：**44pt**
合计可用高度：**64pt**

* iOS 11 
非iPhoneX：**64pt**
iPhoneX：**44pt(状态栏) + 44pt = 88pt**
### iPhoneX 用到的宏

```objc
// iPhone X
#define iPhoneX ([UIScreen instancesRespondToSelector:@selector(currentMode)] ? CGSizeEqualToSize(CGSizeMake(1125, 2436), [[UIScreen mainScreen] currentMode].size) : NO)
// 状态栏高度
#define APP_STATUS_HEIGHT (iPhoneX ? 44 : 20)
// 导航高度
#define APP_NAVIGATION_HEIGHT ((APP_STATUS_HEIGHT) + 44)
// HOME栏高度
#define APP_HOME_INDICATOR_HEIGHT (iPhoneX ? 34 : 0)
// tabBar高度
#define APP_TAB_BAR_HEIGHT (49 + (APP_HOME_INDICATOR_HEIGHT))

```

### TabBar
iPhoneX 底部新增一个半角的矩形，使得tabBar多出来了**34p**的高度。
如果你的用是系统的TabBar会自动适配safeArea。
如果你使用的是自定义TabBar你可能会遇到它的半角区域变成**黑色**的问题，原因是你自定义的TabBar高度比系统的小34p。
解决方法 1：
```objc
controller.tabBar.backgroundColor = [UIColor whiteColor];//添加你的颜色
```
解决方法 2:
修改你自定义的tabBar的frame，重新布局内容。


### iPhoneX 在push进入 webView页面时，底部有黑边闪过

这也是由iOS11的safeArea引起的问题。检查webView的高度是否是(屏幕高度-（导航栏高度+状态栏高度))
解决方案：
设置webView.scrollView的contentInsetAdjustmentBehavior
```
if (@available(iOS 11.0, *)) {
    webView.scrollView.contentInsetAdjustmentBehavior = UIScrollViewContentInsetAdjustmentNever;
}
```

### 待续...



### 参考
[Apple iPhoneX Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/overview/iphone-x/)


## 最后

感谢阅读，如果对大家有帮助，请在[github上follow和star](https://github.com/yuxinyang0325)，本文发布在[逆流的简书博客](http://www.jianshu.com/p/17a548faec2f)，转载请注明出处
