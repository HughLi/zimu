---
title: linux 扩容(raspberry)
date: 2017-09-02 10:03:34
tags:
---

1.查看当前使用情况

```sh
 $ sudo df -h
```
2.使用fdisk删除原来的linux分区

````sh
 $ sudo fdisk /dev/mmcblk0
命令：按P（打印）
你应该会看到三个分区，现在把分区2的信息写下来（/dev/mmcblk0p2）。
命令：按d（删除分区2）
命令：按p（打印）
现在应该会看到2个分区
命令：按n（加分区）
选择P (主要)
於分区2选择2
第一空格输入原来分区2的开始位置
最后的空格输入默认值
命令：按p（打印）
你应该會看到分区2填满所有空间
命令：按w（保存）
````

3.重新启动

```sh
 $sudo reboot
```
4.重新启动后，使用resize2fs来修复分区2

```sh
$ sudo resize2fs /dev/mmcblk0p2
等待约2-3分钟
$ sudo df -h
 重新查看分区使用情况
```


### 一个小错误的修复
使用
```sh
apt-get install kali-linux-full
```
的时候,终端打印错误

````sh
dpkg: error processing python-pip(--remove):
 Package is in a very bad inconsistent state - you should
 reinstall it before attempting a removal.
Errors were encountered while processing:
python-pip 
````

### 一个大错误的修复
由于没有正常的断开电源,linux系统启动后发现没法进入xwindow ,而且df -h 发现系统全部成为了read-only 
经过google 发现使用fsck -y 修复有效,之后系统顺利进入