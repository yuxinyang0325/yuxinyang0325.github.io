---
layout: post
title: 利用Jenkins + Maven + SVN + Nexus仓库，自动构建并上传资源
category: 技术
tags: Nexus, Maven, 私有仓库, Java, Jenkins
description:
---


> 目前大多数的Jar包管理都利用Maven, Jenkins作为一个统一构建工具当然也可以集成Maven，所以我们利用Jenkins + Maven + SVN自动构建Jar包，并上传资源到Nexus私有仓库。

### 前置条件

* [Maven && Nexus](https://www.jianshu.com/p/89cf662ee561)
* [Jenkins](http://www.jianshu.com/p/104fd563e0f8)

### 安装插件
* **Maven Integration plugin** 
* **Email Extension plugin**
安装后请重启Jenkins使插件生效。


### 配置Maven

在**Jenkins**->**系统管理**->**Global Tool Configuration**
中找到Maven,设置路径

![Maven设置](http://upload-images.jianshu.io/upload_images/3096441-0a4d8c30c519a36e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Jenkins配置JDK
**Jenkins**->**系统管理**->**Global Tool Configuration**
中找到JDK，设置你的JDK路径
![JDK路径设置](http://upload-images.jianshu.io/upload_images/3096441-d34829b001cbf390.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Jenkins Location
Jenkins->系统管理->系统设置->Jenkins Location

填写Jenkins URL
填写系统管理员邮件地址

![Jenkins Location](http://upload-images.jianshu.io/upload_images/3096441-21f9f2b15c377a7f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


填写**邮件通知**
![邮件通知](http://upload-images.jianshu.io/upload_images/3096441-d50224a19195ae6b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 创建Maven构建项目
* **Jenkins**->**新建**->**构建一个Maven项目**
![创建Maven项目](http://upload-images.jianshu.io/upload_images/3096441-b0040389525d6a75.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 设置Maven构建项目
添加一个版本号，在构建的时候改变版本
![设置Maven构建项目](http://upload-images.jianshu.io/upload_images/3096441-5ef1e191d5dad7e4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* 设置代码数据源
![ 设置代码数据源](http://upload-images.jianshu.io/upload_images/3096441-05f66781a6c64006.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 构建触发器
![构建触发器](http://upload-images.jianshu.io/upload_images/3096441-011f956014757f1a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 增加构建前置步骤
更换版本号
**env
mvn versions:set -DnewVersion=$MAVEN_PROJECT_VERSION-SNAPSHOT**

![增加构建前置步骤](http://upload-images.jianshu.io/upload_images/3096441-3f2c8b9dbd656257.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 设置Build内容
**其中pom.xml是Maven的配置文件**，是手动拷贝到此Maven构建项目中的。
![设置Build内容](http://upload-images.jianshu.io/upload_images/3096441-4f44d28b433ab99b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 设置构建后上传Jar包到Maven仓库
![上传Jar包到Maven仓库](http://upload-images.jianshu.io/upload_images/3096441-fd431f00d8e99864.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


* 设置构建后邮件通知
1. 填写邮件信息
![设置邮件通知](http://upload-images.jianshu.io/upload_images/3096441-71be86d8b9d19d48.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.  新增邮件Triggers
![设置邮件Triggers](http://upload-images.jianshu.io/upload_images/3096441-6fb4bcdc5a029326.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 添加Success Triggers
![QQ20171226-154906@2x.jpg](http://upload-images.jianshu.io/upload_images/3096441-d30552d409aedffe.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 

### 构建Maven项目
* 返回Maven构建项目，选择**Build with Parameters**，填写版本号开始构建项目。
![构建Maven项目](http://upload-images.jianshu.io/upload_images/3096441-c4400ac861dd1e67.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 后续
至此，Jenkins + SVN + Maven 构建项目已经完成。这其中Jenkins做了一个中间操作，帮助用户完成Maven项目构建，并利用Maven项目中的pom.xml完成上传Jar包操作。


### 最后

感谢阅读，如果对大家有帮助，请在[github上follow和star](https://github.com/yuxinyang0325)，本文发布在[逆流的简书博客](https://www.jianshu.com/p/89cf662ee561)，转载请注明出处
