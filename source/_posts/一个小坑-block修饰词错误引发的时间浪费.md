---
title: '一个小坑--block修饰词错误引发的时间浪费 '
date: 2017-08-14 21:29:56
tags:
---
报错如图:

![](http://ostglltzu.bkt.clouddn.com/17-8-14/10927584.jpg)

![](http://ostglltzu.bkt.clouddn.com/17-8-14/3479218.jpg)

由于在之前的项目中直接把property给拖了一份到代码块,导致这次的这个错误,前后排查了block的循环引用,类文件头部包含等各种问题,最后实在没辙,经过亲友团的帮助发现这个坑,黄油手啊,🐽