---
title: apple 开启ftp服务等乱七八糟的事儿
date: 2017-08-30 15:26:39
tags:
---
```sh
开启服务
➜  ~ sudo -s launchctl load -w /System/Library/LaunchDaemons/ftp.plist
关闭服务
➜  ~ sudo -s launchctl unload -w /System/Library/LaunchDaemons/ftp.plist
```


### Linux 下面解压.XZ .tar 等,并将SD卡格式化后写入

1. 解压

```sh
xz -d **.tar.xz
tar -xv -f **.tar
tar -Jxv -f **.tar.xz
```
2. 查看盘并做分区删除等操作

```sh
1.输入命令“fdisk -l”查看设备挂载的位置，因为这个在设备挂载的时候有可能会发生变化。

  假设设备挂载到了 /dev/sdc 需要对该设备进行分区

2.输入命令“fdisk /dev/sdc”进入分区功能：

  a.在此状态下输入“m”可以查看帮助

  b.在此状态下输入“d” 删除已经存在的分区，第一次可跳过，如果要重新分区就需要。

  c.在此状态下输入“p” 查看已经存在的分区

  d.在此状态下输入“n” 新增分区，根据提示需要输入分区号，之后还需要输入分区大小

  e.在此状态下输入“t” 改变文件系统格式，命令“l”显示对应文件系统格式的id（提示上也有这个说明）

  f.在此状态下输入“w” 保存分区兵退出

其它的一些命令就输入“m”查看
```
3. 格式化

```sh
1. 卸载盘
为了防止在写入镜像的时候有其他读取或写入，我们需要卸载设备。如果有两个分区就都要卸载。
umount /dev/sdb1
umount /dev/sdb2

2. 格式化
➜  ~ mkfs.[文件格式] [设备路径]

mkfs         mkfs.cramfs  mkfs.ext3    mkfs.fat     mkfs.msdos
mkfs.bfs     mkfs.ext2    mkfs.ext4    mkfs.minix   mkfs.vfat

3. 写入 

sudo dd bs=4M if=2013-09-25-wheezy-raspbian.img of=/dev/sdb

bs  一次写入多大的块，(blocksize的缩写),
4M  一般都没问题，如果不行，试试改成1M，
if  下载的镜像的路径（input file缩写），
of  设备地址（output file的缩写)

如果你非常想看到此时的拷贝进度,打开另一个命令窗口
sudo pkill -USR1 -n -x dd
```