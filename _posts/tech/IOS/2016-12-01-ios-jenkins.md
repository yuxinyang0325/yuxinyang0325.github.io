---
layout: post
title: Mac环境配置Jenkins + SVN实现iOS项目持续集成(Continuous Integration）
category: 技术
tags: iOS, Jenkins, 持续集成, CI
description:
---


>随着项目的不断推进，参与的人员越来越多，内容越来越复杂，构建项目本身可能就变成了复杂又耗时的工作。持续集成(Continuous Integration,简称CI)，作为一种团队开发实践方法，很好的解决这些问题。它可以让开发团队专注于业务需求，让测试团队更快的构建项目检测问题，加快项目进度。


[Jenkins](https://baike.baidu.com/item/Jenkins?fr=aladdin)作为一种[持续集成](https://baike.baidu.com/item/持续集成)的方案，由于其丰富的插件和较高的可控性，备受大家喜爱。

### 安装Jenkins
* 首先到[官方网站](https://jenkins.io)下载Jenkins安装包，由于它是Java项目所以依赖[JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

 [下载JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
![下载JDK](http://upload-images.jianshu.io/upload_images/3096441-54ab59a2b6c37da5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

[下载Jenkins](https://jenkins.io/download/)
![下载Jenkins](http://upload-images.jianshu.io/upload_images/3096441-b3e14eb7a2fe5cc8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 找到下载的jenkins.pkg安装包，安装时注意在选择 **安装类型** 阶段选择自定义安装。
![Jenkins自定义安装](http://upload-images.jianshu.io/upload_images/3096441-4b418d0e453bbf3d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 取消 **Start at boot as "jenkins"**
![取消勾选](http://upload-images.jianshu.io/upload_images/3096441-ac08e38676aa99d0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 安装完成后在Terminal中输入
$ **open /Applications/Jenkins/jenkins.war**
或
$ **java -jar /Applications/Jenkins/jenkins.war** 
启动Jenkins

### 配置Jenkins
1. 打开http://localhost:8080会出现重设初始密码的界面。根据界面中的路径提示去相应文件中取出密码。
2. 创建一个用户
3. 安装插件[Xcode integration](https://wiki.jenkins.io/display/JENKINS/Xcode+Plugin)和[Keychains and Provisioning Profiles Management]()
打开Jenkins->系统管理->插件管理->可选插件，安装这两个插件。
![安装插件](http://upload-images.jianshu.io/upload_images/3096441-954cd9f332f26274.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4.配置Keychains and Provisioning Profiles Management 

Jenkins->系统管理->Keychains and Provisioning Profiles Management。

![配置证书相关](http://upload-images.jianshu.io/upload_images/3096441-28ce623438170ccd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* Upload Keychain or Provisioning Profile File这里上传的是 **/Users/你的用户/Library/Keychains/login.keychain** 这个文件，如果你的系统中有login.keychain-db这个文件，请把它拷贝出来，重命名成**login.keychain**再上传。

* **Code Signing Identity**这里填写 证书名称
* **Provision Profiles Directory Path**这里填写与证书相对应的描述文件的路径，这个路径可以是自定义路径。

5. 配置Xcode integration

Jenkins->系统管理->系统设置->Xcode Builder


![Xcode Builder设置](http://upload-images.jianshu.io/upload_images/3096441-943a6b312f599763.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


6. 新建项目
* Jenkins->新建

![新建工程](http://upload-images.jianshu.io/upload_images/3096441-561bff147b865051.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 选择->构建一个自由风格的软件项目
![创建自由风格项目](http://upload-images.jianshu.io/upload_images/3096441-4dd4ba03bc77c493.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 填写项目信息
![填写项目信息](http://upload-images.jianshu.io/upload_images/3096441-3801b16505f90907.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 填写SVN配置
Repository URL 填写SVN项目路径。
Credentials填写SVN用户。

![填写SVN配置](http://upload-images.jianshu.io/upload_images/3096441-c144d8b9c8dd6489.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 增加SVN用户信息
![增加SVN用户信息](http://upload-images.jianshu.io/upload_images/3096441-ca86b91eedc5d762.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 填写构建信息
这里需要注意一点，如果第一次点开**Keychain and Code Signing Identites**后发现，**Code Sign Identity** 如果不能选择，那先点击保存，再打开项目来继续设置。
![填写构建信息](http://upload-images.jianshu.io/upload_images/3096441-243e4145e2361e43.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 填写**Mobile Provisioning Profiles**
![填写Mobile Provisioning Profiles](http://upload-images.jianshu.io/upload_images/3096441-026d8cd3d571a2bd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 增加  **构建**
**注意**:这里如果Xcode的版本低于**9.0**，就可以正常输出ipa包。如果Xcode版本不低于**9.0**，就会由于**Xcode 9.0**不在允许你访问钥匙串里的内容(具体原因看[这里](https://github.com/fastlane/fastlane/issues/9589))，而输出ipa失败。具体的解决方法请参照后面"**遇到的问题**"中具体的解决方法。

如果你的Xcode版本低于**9.0**那么请继续向下看😄
增加构建步骤->Xcode

![增加构建步骤](http://upload-images.jianshu.io/upload_images/3096441-342e575b85134b8e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 填写iOS项目Target名称
填写后点击 **Setting** 设置其他参数
![填写iOS项目Target名称](http://upload-images.jianshu.io/upload_images/3096441-ac702f71201af3ea.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 填写Setting信息
其中注意 **Output Directory** 是生成iPA包的路径 **${WORKSPACE}**是项目在Jenkins中的工作目录。
![填写Setting信息](http://upload-images.jianshu.io/upload_images/3096441-36648551c9bfbdfb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 设置**Code signing & OS X keychain options**
![设置 Code Signing & OS X keychain options](http://upload-images.jianshu.io/upload_images/3096441-c4c1965b46aaaaf6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**Development Team ID**填写开发团队的ID,它可以在钥匙串中的证书详情里查看(右键证书->查看详情)。
勾选Unlock Keychain，选择上传的login.keychain文件。

* 如果项目里使用了workspace ，那还需要配置**Advanced Xcode build options** 

![Advanced Xcode build options](http://upload-images.jianshu.io/upload_images/3096441-c981d65d721c1ec0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**Xcode Schema File**填写iOS项目中工程名
**Xcode Workspace File**填写iOS workspace文件的绝对路径

* 开始构建项目
点击保存，返回到Test_Project_1构建下，选择立即构建
![立即构建](http://upload-images.jianshu.io/upload_images/3096441-95e56ce8e6bef1e2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这时会有进度条显示，构建进度，点击进入之后可以查看Log
![构建进度](http://upload-images.jianshu.io/upload_images/3096441-2539681353891117.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 构建后的操作
构建完成后，可以选择将ipa包上传到自己的服务器，也可以用fir或者蒲公英等第三方服务。
* 参考http://blog.fir.im/jenkins/使用官方工具[fir-plugin-1.9.5.hpi](http://7xju1s.com1.z0.glb.clouddn.com/fir-plugin-1.9.5.hpi)插件上传ipa包到fir。

### 遇到的问题
* Xcode版本高于9.0 ，使用Xcode Builder构想项目的时候会出现导出ipa失败。

![导出ipa失败](http://upload-images.jianshu.io/upload_images/3096441-b4b511e1c26916a5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### error: exportArchive: "Your.app" requires a provisioning profile with the Push Notifications feature.

###### Error Domain=IDEProvisioningErrorDomain Code=9 ""Your.app" requires a provisioning profile with the Push Notifications feature." UserInfo={NSLocalizedDescription="Your.app" requires a provisioning profile with the Push Notifications feature., NSLocalizedRecoverySuggestion=Add a profile to the "provisioningProfiles" dictionary in your Export Options property list.}
###### ** EXPORT FAILED **

最后错误提示，EXPORT FAILED，推断导出ipa包时出的错。根据最后提示
**Add a profile to the "provisioningProfiles" dictionary in your Export Options property list** 
google一下，发现**Xcode 9.0**不允许访问钥匙串里的内容。

解决方案: 在**构建**中添加**Execute Shell**替代**Xcode Builder**。
![选择Execute Shell](http://upload-images.jianshu.io/upload_images/3096441-87b02bc7e604b0af.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用脚本替代插件(**插件本质是通过我们的配置生成打包脚本并执行**)。
![脚本内容](http://upload-images.jianshu.io/upload_images/3096441-0c91d58d7f0f4026.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 脚本内容
```shell
xcodebuild -archivePath "/Users/你的用户/.jenkins/workspace/你的Jenkin构建项目名/output/debug/name.xcarchive" -workspace name.xcworkspace -sdk iphoneos -scheme "schemename" -configuration "Release" archive
xcodebuild -exportArchive -archivePath "/Users/你的用户/.jenkins/workspace/你的Jenkin构建项目名/output/debug/name.xcarchive" -exportPath "/Users/你的用户/.jenkins/workspace/你的Jenkin构建项目名/ipa/" -exportOptionsPlist '/Users/chaos/.jenkins/workspace/你的Jenkin构建项目名/ipa/ExportOptions.plist' -allowProvisioningUpdates
```
按照你的实际项目情况替换脚本中的内容
**name.xcarchive** =>TargetName.xcarchive
**name.xcworkspace** => iOS 项目 workspace 名字
**schemename** => scheme manage中的名字

其中**ExportOptions.plist** ，直接使用Xcode导出iPA同文件夹中的同名文件就行。

![ExportOptions.plist文件位置](http://upload-images.jianshu.io/upload_images/3096441-4ddf058837e42b55.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ExportOptions.plist文件请放在Jinkens的workspace，构建项目文件夹下。
脚本中ExportOptions.plist路径可以自定义。
![ExprotOptions.plist位置](http://upload-images.jianshu.io/upload_images/3096441-8f3cfc6433b27d2d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 扩展
* 其他问题可以参考[Jenkins自动打包遇到的问题](http://www.jianshu.com/p/d727d08bb274)

* Jenkins配置Android构建，请[点击这里](https://www.pgyer.com/doc/view/jenkins)


## 最后

感谢阅读，如果对大家有帮助，请在[github上follow和star](https://github.com/yuxinyang0325)，本文发布在[逆流的简书博客](http://www.jianshu.com/p/104fd563e0f8)，转载请注明出处
