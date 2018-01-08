---
layout: post
title: Mac 搭建私有Git Server
category: 技术
tags: Git, Mac, Git Server, Server
description:
---

> 大家对[Github](https://baike.baidu.com/item/github/10145341?fr=aladdin)一定不陌生，虽然我们可以在Github上申请私有仓库，但是在某些情况下，我们还是需要一个可以自己管理仓库的私有Git Server。
此篇文章旨在介绍如何简单的在Mac上搭建Git Server，有需要企业级管理的同学可以搜索Gitosis、Gitolite、Gitlab之类的管理软件。


### 一，创建Git用户
1，在用作服务器的机器上创建 git 账户。我们可以通过 **系统偏好设置**->**用户与群组** 来添加。账户权限给的是**管理员**权限为了方便操作。
![创建Git用户](http://upload-images.jianshu.io/upload_images/3096441-fd78d821c0ac2c4c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


2，设置远程访问
在**系统偏好设置**->**共享** 中，勾选**仅这些用户**允许访问。
![共享设置](http://upload-images.jianshu.io/upload_images/3096441-e64725fe29406e9c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 二，Git Server设置
1，验证Git用户
$ ssh git@yourComputerName.local
按提示输入git用户的密码，如图出现**~ git$ **提示则说明登陆成功。

![验证输入](http://upload-images.jianshu.io/upload_images/3096441-14096d5ebb5aefda.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


2， 生成 ssh rsa 公钥
* 如果client端已经创建ssh rsa 过公钥，则略过此步骤：
``` shell
$ cd ~
$ ssh-keygen -t rsa  
# 公钥默认生成路径 ~/.ssh/id_rsa.pub
# 可以拷贝到其他路径改名$ cp ~/.ssh/id_rsa.pub /你的路径/新名称.pub
```

* 把ssh rsa 公钥拷贝到Git Server端。
``` shell
# 使用git用户登录，并创建.ssh 文件夹
$ ssh git@yourComputerName.local mkdir .ssh
$ scp ~/.ssh/id_rsa.pub git@yourComputerName.local:.ssh/authorized_keys
# 之后会出现文件传输信息
```

* 修改Git Server端的sshd_config文件
```shell
$ ssh git@yourComputerName.local
$ cd /etc
$ sudo chmod 666 sshd_config
```
注意：这里有个需要注意的地方，/etc文件夹可能没有sshd_config文件，只有有一个**sshd_config~previous**文件，那我们操作的文件就换成**sshd_config~previous**。

* 修改sshd_config中的内容
```shell
$ vi sshd_config 
#PermitRootLogin yes      改为 PermitRootLogin no

#下面的去掉 # 注释
#RSAAuthentication yes
#PubkeyAuthentication yes
#AuthorizedKeysFile     .ssh/authorized_keys
#PasswordAuthentication no
#PermitEmptyPasswords no

#UsePAM yes　　　　　　　　　　改为 UsePAM no

#保存文件并退出
```
![sshd_config文件修改_1](http://upload-images.jianshu.io/upload_images/3096441-ad7f71304af88644.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![sshd_config文件修改_2](http://upload-images.jianshu.io/upload_images/3096441-d50b49cb621c0241.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


* 在Git Server端创建空的 repository

```shell
$ cd /Desktop/gitrepos/
#这里的路径是自定义的，创建你需要路径

$ mkdir newgitrepo.git # 同上 自定义名称
$ cd newrepo.git
$ git init --bare # --bare 参数表明只是用来存储 pushes，不会当做本地 repository 来使用。

```

* 回到在Git Client端，创建本地仓库并提交。
注意：先在终端中使用exit命令退出git用户。
```shell
$ cd /Desktop/gitrepos/ #这里我使用和服务器相同路径
$ mkdir newrepo
$ git init
$ touch README
$ git add .
$ git commit -m "initial commit"
$ git remote add origin git@yourComputerName:Desktop/gitrepos/newrepo.git 
# 如果直接之前在 server 端创建的 newrepo.git 是在 home 目录($ cd ~)，则这里地址为：git@yourComputerName:newrepo.git
$ git push origin master
# 之后可以看到提交信息
```

### 总结
这样，我们就完成了在Mac中搭建私有Git Server的操作。
如果你的应用场景是企业级也可以参考这篇来设置服务器上的Git Server，或者搜索Gitosis、Gitolite、Gitlab之类的管理软件。


### 最后

感谢阅读，如果对大家有帮助，请在[github上follow和star](https://github.com/yuxinyang0325)，本文发布在[逆流的简书博客](https://www.jianshu.com/p/df6c3f14f7f7)，转载请注明出处
