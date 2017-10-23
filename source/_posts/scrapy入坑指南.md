---
title: scrapy入坑指南
date: 2017-09-20 15:13:00
tags:
---
### linux7z的使用命令

linux上安装7z命令有两种方式：在线安装和安装包安装，下面分别介绍。

1.1 在线安装

如果你的宿主机Linux可以连接外网，推荐用这种方式，方便简单，执行命令：

sudo apt-get install p7zip
即可在线安装7z命令。

1.2 安装包安装

7z（准确点说是7-Zip）提供了线下的程序安装包，也可自己编译安装。这里讲的是用7z提供的bin包来安装。

宿主机linux一般是X86的，而7z提供编译好了的bin包，可以很方便的安装。步骤如下：

1） 去网站http://sourceforge.net/projects/p7zip/files/或http://sourceforge.net/projects/p7zip/files/p7zip/上下载p7zip的包，当前最新版本是9.20.1；

2） 找到对应版本号进去，页面会提供两个供你下载，一个是bin包，另一个是源码包，这里下的是bin包，以9.20.1为例，下载的包名称是：p7zip_9.20.1_x86_linux_bin.tar.bz2；

3） 在Linux上执行下面命令（解压和安装）：

tar xjvf p7zip_9.20.1_x86_linux_bin.tar.bz2

cd p7zip_9.20.1

sh install.sh
注意上面的命令权限，需要root权限，因此最好在tar和sh命令前加上sudo。

到此，就安装完成了。

2. 7z命令的使用

2.1 解压缩7z文件

7za x phpMyAdmin-3.3.8.1-all-languages.7z -r -o./
参数含义：

x  代表解压缩文件，并且是按原始目录树解压（还有个参数 e 也是解压缩文件，但其会将所有文件都解压到根下，而不是自己原有的文件夹下）

phpMyAdmin-3.3.8.1-all-languages.7z  是压缩文件，这里我用phpadmin做测试。这里默认使用当前目录下的phpMyAdmin-3.3.8.1-all-languages.7z

-r 表示递归解压缩所有的子文件夹

-o 是指定解压到的目录，-o后是没有空格的，直接接目录。这一点需要注意。

2.2 压缩文件／文件夹

7za a -t7z -r Mytest.7z /opt/phpMyAdmin-3.3.8.1-all-languages/*
参数含义：
a  代表添加文件／文件夹到压缩包

-t 是指定压缩类型，这里定为7z，可不指定，因为7za默认压缩类型就是7z。

-r 表示递归所有的子文件夹

Mytest.7z 是压缩好后的压缩包名

/opt/phpMyAdmin-3.3.8.1-all-languages/*：是压缩目标。

注意：7za不仅仅支持.7z压缩格式，还支持.tar.bz2等压缩类型的。如上所述，用-t指定即可。
***
>使用scrapy

### 1. 新建

````sh
scripy startproject spiderName
````

### 2. 自动类

````sh
cd spider
scrapy genspider spiderName url
````

### 3. Re 正则判断在