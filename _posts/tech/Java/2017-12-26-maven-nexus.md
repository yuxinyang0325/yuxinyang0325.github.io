---
layout: post
title: Mac利用Nexus构建Maven私有仓库
category: 技术
tags: Nexus, Maven, 私有仓库, Java
description:
---


> Maven即[Apache Maven](https://zh.wikipedia.org/wiki/Apache_Maven)是一个项目（尤其是Java项目）管理及自动构建工具。而Nexus作为Maven项目的代码仓库，也有着广泛的应用。而我在搭建过程中页遇到的很多坑，现在整理出来，让大家尽可能少走弯路。

### 前置条件 
JDK安装

### Maven
* Maven安装
如图[下载Maven](http://maven.apache.org/download.cgi)，下载后放在已知的路径下。
![Maven下载](http://upload-images.jianshu.io/upload_images/3096441-c05910c2ff4c132d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 设置环境变量
终端执行 $ **open ~/.bash_profile**
在文件中添加，环境变量（Maven包位置，比如我的就放在桌面上）
**export M2_HOME=/Users/用户/Desktop/apache-maven-3.5.2**
**export PATH=$PATH:$M2_HOME/bin**

![设置环境变量](http://upload-images.jianshu.io/upload_images/3096441-cb01abd5ccb3208b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

保存后文件后，使环境变量生效。
终端执行 $ **source ~/.bash_profile**

* 检查Maven是否可用
终端执行 $ **mvn -v**
正常的话会出现Maven的版本号。
![检查Maven](http://upload-images.jianshu.io/upload_images/3096441-69fc43b90bff371a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### Nexus
* Nexus安装
如图[下载Nexus](https://www.sonatype.com/download-oss-sonatype)
![下载Nexus](http://upload-images.jianshu.io/upload_images/3096441-26c716ce32af2f25.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 启动Nexus
终端进入下载的Nexus文件夹中找到bin目录，使用命令 $**./nexus start**
![启动Nexus](http://upload-images.jianshu.io/upload_images/3096441-62fbc373c36c68b0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 构建仓库
1. 访问Nexus
Nexus默认访问路径：**http://localhost:8081/nexus/**
用户：**admin**
密码：**admin123**

2. 添加新仓库
2.1  选择图中左侧Views/Repository中的 Repository选项
2.2  选择图中Add Repository 
![添加新仓库](http://upload-images.jianshu.io/upload_images/3096441-ab8f91ad2eff1d59.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2.3 选择添加Hosted Repository

![Hosted Repository](http://upload-images.jianshu.io/upload_images/3096441-eecc254dc4013fdc.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.4 填写仓库信息
重点信息：
Repository ID:**Snapshot_Repository**
Repository Name:**Snapshot_Repository**
Provider:**Maven2**
Repositroy Policy:**Snapshot**
Deployment Policy:**Allow Redeploy**
![填写仓库信息](http://upload-images.jianshu.io/upload_images/3096441-f0531f7cea1cda54.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 配置Maven

* 配置Maven Setting.xml
文件位置：**$ M2_HOME/conf/settings.xml**
1. 在servers标签中，新添加一个用户信息。
    这里填写的是Nexus默认开发账户信息。
```xml
  <servers>
  ···  
  ···
    <server>
        <id>Snapshot_Repository</id>
        <username>admin</username>
        <password>admin123</password>
    </server>
  </servers>
```
2. 在profiles标签下，新加Nexus仓库信息。
```xml
  <profiles>
    ···
    ···
    <profile>
        <id>dev</id>
        <repositories>
            <repository>
                <id>public</id>
                <name>Maven Mirror</name>
                <url>http://127.0.0.1:8081/nexus/content/groups/public/</url>
                <releases>
                    <enabled>true</enabled>
                </releases>
                <snapshots>
                    <enabled>true</enabled>
                    <updatePolicy>always</updatePolicy>
                </snapshots>
            </repository>
        </repositories>
    </profile>
 </profiles>
```

3. 使配置信息生效
```xml
<setting>  
    ···
    ···
    <activeProfiles>
        <activeProfile>dev</activeProfile>
    </activeProfiles>
</settings>
```

### 总结
至此，Maven构建私有仓库的必备步骤就结束了，后续还可以利用Jenkins配置Maven，一键构建并上传Nexus仓库。
可以参考我的[利用Jenkins + Maven + SVN + Nexus仓库，自动构建并上传资源](https://www.jianshu.com/p/89cf662ee561)


### 最后

感谢阅读，如果对大家有帮助，请在[github上follow和star](https://github.com/yuxinyang0325)，本文发布在[逆流的简书博客](https://www.jianshu.com/p/415ddc150f4e)，转载请注明出处
