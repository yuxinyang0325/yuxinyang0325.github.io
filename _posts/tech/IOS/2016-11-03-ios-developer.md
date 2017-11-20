---
layout: post
title: iOS真机调试(Xcode8)，创建证书Certificates,Identifiers & Profiles
category: iOS
tags: iOS,开发者计划
description:
---
>最近项目里证书又过期了，这次准备重新申请一个证书，趁这个机会整理一下。

在进入证书处理步骤前，让我们先在Mac上创建CSR文件，这是申请证书的必要条件，如果你已经创建过那么可以直接跳到**申请证书**部分😉

###创建CSR文件
* 打开钥匙串，选择证书助理**->**从证书颁发机构请求证书
![创建CSR文件_图1.jpg](http://upload-images.jianshu.io/upload_images/3096441-1b19c0c06960b818.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*  在证书信息信息这填写正常的邮件地址、名称后选择**存储到磁盘**

![创建CSR文件_图2.jpg](http://upload-images.jianshu.io/upload_images/3096441-3568d4bd4f900363.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样会生成一个默认名称是**CertificateSigningRequest.certSigningRequest**的文件，这就是一会我们申请证书要用的CSR文件。

---

###申请证书

* 首先进入苹果开发者网站点击[这里](https://developer.apple.com)
* 接下来进入证书设置**Certificates, Identifiers & Profiles**

![图1.jpg](http://upload-images.jianshu.io/upload_images/3096441-9a9e906e9a3a5879.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 因为是发布证书过期所以只需要配置发布版证书就可以了，这里点击图1中右上角➕号添加证书

![图2.jpg](http://upload-images.jianshu.io/upload_images/3096441-b5d7bba50bbaaf77.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 选择图2中 Production里面 App Store and Ad Hoc 选项，点击页面下方**Continue**继续下一步。
*PS(这里我已经申请了2个发布证书和2个开发证书，所以iOS App Development选项和App Store and Ad Hoc已经不能选了)*


![图3.jpg](http://upload-images.jianshu.io/upload_images/3096441-3ae1f657482a244f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 这里介绍怎么创建CSR文件，我们继续下一步。


![图4.jpg](http://upload-images.jianshu.io/upload_images/3096441-b40c1722217ee817.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 此处就是上传我们创建的CSR文件的地方，点击**Choose File**选择刚刚生成的CSR文件，点击**Continue**。


![图5.jpg](http://upload-images.jianshu.io/upload_images/3096441-46bdfe510fccec24.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 此时我们发布证书就申请成功啦👏。
接着在对应的PP文件**(Provisioning Profiles)**配置新证书就大功告成了。
什么？你说还没有PP文件🤔！没关系，接着往下看吧😉
PS(如果已经有APP ID那么可以直接看**创建Provisioning Profiles**)

---

###创建iOS App IDs

*  点击Identifiers中的App IDs 选项，在右侧iOS App IDs点击➕号

![图8.jpg](http://upload-images.jianshu.io/upload_images/3096441-c2391aaf97d7a6b1.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 填写**App ID Description**

![图9.jpg](http://upload-images.jianshu.io/upload_images/3096441-d3b24286bf3d6436.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 填写 **Bundle ID** 对应工程里的Bundle ID

![图10.jpg](http://upload-images.jianshu.io/upload_images/3096441-5b7c669830157492.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 选择**App Services** 

![图11.jpg](http://upload-images.jianshu.io/upload_images/3096441-6d70694bd9378bf1.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选择完点击**Continue**，注册成功后可以在**iOS App IDs**处找到新注册的ID

---

###创建Provisioning Profiles

* 选择**Provisioning Profiles**

![图12.jpg](http://upload-images.jianshu.io/upload_images/3096441-4d18422725e6d2b4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 点击➕新建PP文件

![图13.jpg](http://upload-images.jianshu.io/upload_images/3096441-396999b118602d9d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 选择**Distribution**中的**App Store**（创建**Developmen**t的PP就选择上面的两项）

![图14.jpg](http://upload-images.jianshu.io/upload_images/3096441-daf9ad5f7e6add6d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 选择刚刚新建的App ID，点击**Continue**

![图15.jpg](http://upload-images.jianshu.io/upload_images/3096441-d1e54414cb58ce9f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 选择刚刚新建的发布证书，点击**Continue**

![图16.jpg](http://upload-images.jianshu.io/upload_images/3096441-ab57ccc65da94cdd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 最后给PP文件命名，点击**Continue**

![图17.jpg](http://upload-images.jianshu.io/upload_images/3096441-3ad75698baf4f83c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* **Provisioning Profiles**创建完毕

![图18.jpg](http://upload-images.jianshu.io/upload_images/3096441-98bb276f8e702546.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 先别着急下载安装PP文件及证书还有一个步骤没有完成

###添加调试机器的Device ID
在真机调试前还需要一个步骤就是把准备用于调试用机的Device ID添加到开发PP文件里。
**(如果你只创建了发布证书及发布PP文件，那么你还需要再创建开发证书及开发PP文件)**
* 可用同一个CSR文件创建开发证书，区别就是所有选择发布**Distribution**的地方选择开发**Development**就可以了。

* 如何查看机器的UDID点[这里](http://jingyan.baidu.com/article/d7130635040a2013fdf47594.html)

* 点击Derives ID ，添加将要用于真机调试的设备
设备名（中英文都行）
UDID不对时会有提示
填写完后注册该设备

![图19.jpg](http://upload-images.jianshu.io/upload_images/3096441-aeeca09ede539651.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 添加完成后，我们返回**Provisioning Profiles**中找到创建的**Developer PP**文件**TestAPP_PP_Developer**，并且编辑它

![图20.jpg](http://upload-images.jianshu.io/upload_images/3096441-9960f049f274db5f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 在编辑界面的**Devices**框中选中刚才添加机器，或者选中**Select All**，创建**Generate**

![图21.jpg](http://upload-images.jianshu.io/upload_images/3096441-51d5f3aff53d14c9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

到此证书准备工作完成，我们庆祝一下👏

##### 把刚刚创建的文件都下载到Mac并双击安装。

1. **发布证书** (上传AppStore需要)
2. **发布PP文件** (上传AppStore需要)
3. **开发证书** (真机调试需要) 
4. **开发PP文件**(真机调试需要)

#####或者在Xcode下载证书，操作路径如下
**Xcode->Preferences->Accounts->View details**

![图22.jpg](http://upload-images.jianshu.io/upload_images/3096441-f913e1d643049983.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在红色标记框中的PP文件列表中找到**开发者网站里创建的PP文件**并下载，找不到的话可以都删除掉（任意PP文件右键**Show in Finder**），点击**Download All Profiles**重新下载全部PP文件

![图23.jpg](http://upload-images.jianshu.io/upload_images/3096441-e81c98de69791de8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---




###Xcode8真机调试
真机调试需要开发证书，如果还有创建的话请参考前面**添加调试机器的Device ID**部分

* 用Xcode8新建一个iOS工程
打开工程的**TARGETS**->**General**

![图24.jpg](http://upload-images.jianshu.io/upload_images/3096441-f6ed89c754f9b637.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*  填写信息:**Bundle Identifier、Signing(Debug)、Signing(Release)**

![图25.jpg](http://upload-images.jianshu.io/upload_images/3096441-353f4da11e489728.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

另外Xcode8 可以直接选中**Automatically manage signing**，让Xcode管理证书，这样可以省去创建证书的步骤。（但是我还是喜欢手动控制☺️）

---

**好了有关iOS真机调试，以及证书的创建整理到这，希望对大家有帮助**😄

**补充：没有购买每年99$的开发者也可以真机调试**
* 用个人的Apple ID登录开发苹果开发者网站[这里](https://developer.apple.com)，注册成开发者
* 在Xcode 8中添加个人Apple ID账户
* 新建工程并选择自动管理证书，就可以真机调试了
* 第一次真机调试时Xcode会提示在设备上信任证书（其实就是自己AppleID生成的开发者证书）路径是**设置->通用->描述文件与设备管理**，信任与AppleID同名的证书文件。

## 最后

感谢收看，如果对大家有帮助，请[github上follow和star](https://github.com/yuxinyang0325)，本文发布在[逆流的简书博客](http://www.jianshu.com/u/30300da6b66b)，转载请注明出处