---
title: 在OC项目里面引入Swift文件
date: 2017-08-25 19:55:34
tags:
---

### OC项目里面引入Swift
最近要使用Carts 框架绘制图表,使用了cocoapods集成,所以用起来还是十分方便,但是在图标上面要显示点击后的气泡(ballom),自己想使用作者的Demo里面的,所以需要在OC中引入Swift文件.

新建swift的时候,系统会自动提醒李是否需要桥接文件,这个是Swift引用OC的时候使用的,
系统隐式的为我们生成了一个<mark>项目名-Swift.h</mark>的头文件,当我们每次新建/引入Swift的时候,系统会帮我们在该文件内自动将类和方法接口写好,我们在需要使用这个Swift文件的时候只需<mark>#import "项目-Swift.h"</mark>就可以直接调用所有的可以使用的swift类,而不需单独的引入头文件.

![](http://ostglltzu.bkt.clouddn.com/17-8-25/92484889.jpg)

#### 系统自动生成的swift引用文件
![](http://ostglltzu.bkt.clouddn.com/17-8-25/80982659.jpg)

<mark>如果文件不是新建的而是引入的,那么需要在```Build Phases```里面将文件添加到里面</mark>

![](http://ostglltzu.bkt.clouddn.com/17-8-25/40633551.jpg)