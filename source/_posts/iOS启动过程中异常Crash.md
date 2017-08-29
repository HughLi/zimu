---
title: iOS启动过程中异常Crash
date: 2017-08-24 11:43:36
tags:
---

#### 接上设备后打开崩溃日志记录对应的APP发现崩溃信息如下

![](http://ostglltzu.bkt.clouddn.com/17-8-24/35726988.jpg)

原来是在设备启动的时候耗时太长,系统主动终结了该APP的运行,结合程序代码分析可能原因是由于在启动的时候有向后台注册一些信息,当信息返回的时候才开始进入界面.所以问题应该是这里出现的

### 使用charts框架技巧
![](http://ostglltzu.bkt.clouddn.com/17-8-24/89231287.jpg)
[柱状图]
(http://www.cnblogs.com/wanghuaijun/p/5587746.html)
![](http://ostglltzu.bkt.clouddn.com/17-8-24/55779980.jpg)
[折线图]
(http://www.jianshu.com/p/039d6d9ff3f7)
![](http://ostglltzu.bkt.clouddn.com/17-8-24/89672071.jpg)
[饼状图]
(http://www.jianshu.com/p/45194d861b21)