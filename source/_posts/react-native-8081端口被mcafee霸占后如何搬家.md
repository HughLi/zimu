---
title: react-native 8081端口被mcafee霸占后如何搬家
date: 2017-07-18 17:07:53
tags:
---
##### 在有些公司电脑上出于内部安全管理的原因,电脑上安装了McAfee,所以8081端口基本就算是废了,该端口会被McAfee一直霸占着,关掉进程后也没辙,关掉后会自动开启新的进程

#### react-native默认的打包端口也是8081,这个时候,这个端口冲突会导致一系列奇奇怪怪的BUG,现在,让我们來一起解决下这个问题吧

> 按照react-native 新建工程的流程,完成新建工作

````
react-native init newProj
````
> 在终端直接运行

````
cd newProj
react-native run-ios
````

> 终端上面出现报错日志如下

![](http://ostglltzu.bkt.clouddn.com/17-7-18/23859563.jpg)	
	
> 我们现在在Xcode中打开查看下报错的错误日志

![](http://ostglltzu.bkt.clouddn.com/17-7-18/43005869.jpg)
> 可以看出来 是由于端口被占用导致的打包失败
> 现在我们开始来修改端口给react-native 换个port 搬个家吧

1. 首先修改Xcode内的管理端口的代码,我们来command+shift+f 全局搜索8081
![](http://ostglltzu.bkt.clouddn.com/17-7-18/51946558.jpg)
2. 然后 我们全部将他们改为8083
3. 之后我们来看看React.xcodeproj 这个文件的脚本里面还藏着一个端口的信息呢
![](http://ostglltzu.bkt.clouddn.com/17-7-18/40410071.jpg)
4. 现在我们再来研究下包里面的文件
5. /RNProj/node_modules/react-native/local-cli/server/server.js 里面修改端口
![](http://ostglltzu.bkt.clouddn.com/17-7-18/39777334.jpg)

6. /newProj/node_modules/react-native/Libraries/Core/Devtools/getDevServer.js 里面再来修改一个

![](http://ostglltzu.bkt.clouddn.com/17-7-18/41350170.jpg)

> 现在我们再来运行看看吧


![](http://ostglltzu.bkt.clouddn.com/17-7-18/7281702.jpg)

##### 好了,已经成功运行了;

```` 
react-native-cli: 2.0.1
react-native: 0.46.4
````