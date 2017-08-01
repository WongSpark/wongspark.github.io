---
layout: post
title: Andorid 获取发布版本及debug版本SHA1码
categories: Tools
description: Andorid 获取发布版本及debug版本SHA1码
keywords: Andorid SHA1
---

# 背景
近期参与安卓项目开发，在app打包发布的过程中遇到了一些小问题：`高德地图定位功能在真机测试时提示‘鉴权失败’，经排查，发现时app release版本与debug版本的SHA1码不同，导致高德密钥在真机上失效。`于是便简单探究了有关app SHA1码的相关问题，记录如下。



# Android Studio使用获取app release及debug版本的SHA1
## 1·在Android studio中查看项目的keystore
在Android Studio的`build`弹出菜单中，选择`Generate Signed APK`，可以看到该项目keystore的位置。
![](/images/posts/tools/android-get-sha1/1.png)

## 2·cmd（命令行）中获取SHA1
打开cmd，cd到keystore所在目录下，或者先打开keystore所在的文件夹，按住`shift+鼠标右键`，选择`在此处打开命令行`，输入
```shell
keytool -list -v -keystore keyStore.jks
```
再输入密码，此处输入的密码就是你的keystore的密码。就可以看到release 版本的的SHA1码。
![](/images/posts/tools/android-get-sha1/2.png)


> 注：最初，按照高德官方给出的SHA1获取教程一步步尝试，始终报错。
最终发现是笔者弄错了命令行参数，误将-keystore的参数写成了
-key.store，应改为keyStore.Jks。下图为当时所报错误。

![](/images/posts/tools/android-get-sha1/4.png)

![](/images/posts/tools/android-get-sha1/3.png)

## 3·修改高德开发控制台的应用密钥
按照如图所示修改，即可获得在release版本下也可以使用的应用密钥。
![](/images/posts/tools/android-get-sha1/5.png)

![](/images/posts/tools/android-get-sha1/6.png)


# 参考资料
[高德地图鉴权失败原因及解决方案](http://www.cnblogs.com/feijian/p/4549524.html)

[Android Studio 中高德地图申请key和获取sha1及配置的几点方法](http://blog.csdn.net/makeyourchance/article/details/51542273)

[Android Studio创建key-store的方法](http://blog.csdn.net/ljss321/article/details/53590083)

# 结束语
权作记录
