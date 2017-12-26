---
layout: post
title: Jenkins+SVN实现Android项目持续集成
category: Android
tags: Android, Jenkins, 持续集成, CI
description:
---
> 最近刚弄完了Jenkins对iOS项目的持续继承，效果挺好。就想着把Android的持续集成页完成。于是有了这篇文章。

### Jenkins基础
Jenkins基础及安装的内容参考我另一篇[Jenkins + SVN实现iOS项目持续集成](http://www.jianshu.com/p/104fd563e0f8)

### 插件
构建Android工程需要用到[Gradle插件](https://wiki.jenkins.io/display/JENKINS/Gradle+Plugin)，在现在的版本中这个插件被整合进了Global Tool Configuration工具中。如下图所示。
![Global Tool Configuration](http://upload-images.jianshu.io/upload_images/3096441-8b93a946a2e58612.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Global Tool Configuration
* JDK设置
进入Global Tool Configuration，点击**JDK安装**。
![JDK安装](http://upload-images.jianshu.io/upload_images/3096441-dd3eeb3bcfac9932.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 填写上你的JDK名称和路径
![JDK名称和路径](http://upload-images.jianshu.io/upload_images/3096441-8e14c0ee283b336c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Gradle设置
接着在同一页，找到Gradle安装。
![Gradle安装](http://upload-images.jianshu.io/upload_images/3096441-fbc145edfc5530ac.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 填写上你Gradle名称和路径
![填写Gradle路径](http://upload-images.jianshu.io/upload_images/3096441-cf05441afd3f3010.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 保存设置

### 创建Android项目
* Jenkins->新建->自由风格软件
![新建Android构建项目](http://upload-images.jianshu.io/upload_images/3096441-0381756ee821a70c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 选择合适你项目的JDK
![选择JDK](http://upload-images.jianshu.io/upload_images/3096441-f4fc22863a9ef549.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 设置合适你项目的代码管理
![设置代码管理](http://upload-images.jianshu.io/upload_images/3096441-fb4f1f09d88dc362.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 选择构建类型
![选择构建类型](http://upload-images.jianshu.io/upload_images/3096441-fe7de4af9f7800e7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 设置构建项目Gradle
![构建项目Gradle](http://upload-images.jianshu.io/upload_images/3096441-dac77398ae530164.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Android Studio中Gradle构建项目位置
![AndroidStudio中Gradle构建项目](http://upload-images.jianshu.io/upload_images/3096441-659cafc04714b7b5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 开始构建
保存后返回构建项目列表，进入Androidg构建项目，选择立即构建。

![立即构建](http://upload-images.jianshu.io/upload_images/3096441-9aa96d90edcd9e07.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 构建成功
![构建成功](http://upload-images.jianshu.io/upload_images/3096441-2c95668e682e2ee0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 后续
* 可以选择把构建好的包上传到自己的服务器页或者[第三方下载平台](https://www.pgyer.com/doc/view/jenkins)。


### 最后

感谢阅读，如果对大家有帮助，请在[github上follow和star](https://github.com/yuxinyang0325)，本文发布在[逆流的简书博客](http://www.jianshu.com/p/c391f3aa6717)，转载请注明出处
