---
title: iOS 设置布局起始
date: 2017-08-08 11:58:35
tags:
---
* 

1. ```self.automaticallyAdjustsScrollViewInsets = NO；``` 禁用掉自动设置的内边距，自行控制controller上index为0的控件以及scrollview控件的位置

2. ```self.edgesForExtendedLayout = UIExtendedEdgeNone;```这种方式设置，不需要再重新设置index为0的控件的位置以及scrollview的位置，（0，0）默认的依然是从导航栏下面开始算起

[相关说明链接](http://www.vinqon.com/codeblog/?detail/11109)

3. masonry 布局新方法

```sh
make.width.mas_lessThanOrEqualTo().offset()
```
