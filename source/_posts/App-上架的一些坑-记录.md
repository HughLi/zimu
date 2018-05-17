---
title: App 上架的一些坑 记录
date: 2018-01-16 15:00:24
tags:
---
### 1. 提交了build版本后很久都没有在connect中出现

该问题可能原因是由于在plist文件中关于用户设备权限的调用的描述没有填写完成

### 2. IDFA检测

可以在项目的根目录下使用 ```grep -r advertisingIdentifier .```检测当前项目的IDFA使用情况

```sh
grep -r advertisingIdentifier .
Binary file ./CarInsureHybird/Vender(第三方)/SDK/TalkingData/libTalkingData1.3.4-IDFA.a matches
```

### 3. SDK导出模拟器和真机都可以使用的.a文件

```sh
➜  ~ lipo -create 模拟器.a真机 -output 两者兼可.a

```

### 4. SDK打包成为release版本
设置run为执行release版本