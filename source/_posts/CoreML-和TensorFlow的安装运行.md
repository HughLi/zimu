---
title: CoreML 和TensorFlow的安装运行
date: 2017-08-07 16:41:28
tags:
---
### 1. CoreML Apple训练模型测试
1. 先下载下来.mlmodel文件并拖入工程中
<p>[下载链接](https://developer.apple.com/machine-learning/)</p>
<p>![](http://ostglltzu.bkt.clouddn.com/17-8-7/2674477.jpg)</p>
<mark>此处要注意 ,如果没有出现上图Model Class的箭头需要勾选下图内容<mark><p>![](http://ostglltzu.bkt.clouddn.com/17-8-7/71022914.jpg)</p>
2. 导入CoreML库文件
![](http://ostglltzu.bkt.clouddn.com/17-8-7/51996763.jpg)
3. 编写代码
![](http://ostglltzu.bkt.clouddn.com/17-8-7/64366435.jpg)

### 2. TensorFlow 安装
我使用的是容器安装的方式

```sh
$ sudo easy_install pip  # 如果还没有安装 pip
$ sudo pip install --upgrade virtualenv

$ virtualenv --system-site-packages ~/tensorflow
$ cd ~/tensorflow

$ source bin/activate  # 如果使用 bash zsh 等

$ pip install --upgrade tensorflow       ＃for Python 2.7
 $ pip3 install --upgrade tensorflow      ＃for Python 3.n
 
```

安装完成

