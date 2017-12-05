---
layout: post
title: Mac环境 Python安装Paramiko模块
category: Python
tags: Python, Mac, paramiko, easy_install, pip, gcc, 
description:
---


> 最近在研究Python的时候需要用到paramiko模块，希望这篇博客能让大家少走弯路。

### Paramiko简介
[Paramiko](http://www.paramiko.org)是用python语言写的一个模块，实现了SSH2协议，支持以加密和认证的方式，进行远程服务器的连接。它依赖另一个Python模块PyCrypto。

### PyCrypto简介
[PyCrypto](https://www.dlitz.net/software/pycrypto/)是一个Python模块，它提供了很多加密方法[
MD2 128 bits
MD4 128 bits
MD5 128 bits
RIPEMD 160 bits
SHA1 160 bits
SHA256 256 bits
AES 16, 24, or 32 bytes/16 bytes
ARC2 Variable/8 bytes
Blowfish Variable/8 bytes
CAST Variable/8 bytes
DES 8 bytes/8 bytes
DES3 (Triple DES) 16 bytes/8 bytes
IDEA 16 bytes/8 bytes
RC5 Variable/8 bytes](https://www.dlitz.net/software/pycrypto/doc/)

它依赖gcc库，所以首先我们要先安装GCC库

### 安装GCC

* 方法1 
完整安装[GCC库](https://gcc.gnu.org)。

* 方法2
安装Xcode的**Command Line Tools**，里面有[Clang](https://baike.baidu.com/item/clang/3698345?fr=aladdin)，它是一个C语言、C++、Objective-C、Objective-C++语言的轻量级编译器。
在环境变量里添加

* 添加环境变量
$ cd ~
$ touch .bash_profile
$ open .bash_profile

* 使用方法1的同学请添加
```shall
export CC=llvm-gcc-4.2
export CXX=llvm-g++-4.2
```
* 使用方法2的同学请添加
```shall
export CC=clang
export CXX=llvm-g++-4.2
```

* 重新载入配置
$ source .bash_profile 

### 安装 easy_install 和 pip
easy_install 和pip都是Python包管理器，目前官方更推荐用pip，我们后续的模块安装都依赖于pip。
* 如果没有安装的话请打开terminal，输入$  **sudo easy_install pip**，输入管理员密码即可完成安装。
**注意:** 如果你用的是**Mac OS X**自带的Python的话，我建议重新安装一个Python，不要折腾系统的。重新安装的Python可以自带easy_install和pip。[参考这里](http://pythonguidecn.readthedocs.io/zh/latest/starting/install/osx.html)

### 安装PyCrypto
安装pycrypto有两种方式：
* 第一种直接通过pip install方式，前提是已经安装了easy install工具，终端执行 $ **pip install pycrypto**
* 第二种直接下载pycrypto包，解压后进入setup.py文件目录，终端执行$ **sudo python setup.py install**

### 安装Paramiko
* 方法1 终端执行 $ **pip install paramiko** 
* 方法2 下载[paramiko](https://github.com/paramiko/paramiko)包，终端执行 $**python setup.py install**安装

## 最后

感谢阅读，如果对大家有帮助，请在[github上follow和star](https://github.com/yuxinyang0325)，本文发布在[逆流的简书博客](http://www.jianshu.com/p/3b572cdab266)，转载请注明出处
