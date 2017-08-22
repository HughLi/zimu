---
title: Pod search 找不到库的解决方案
date: 2017-07-21 09:12:02
tags:
---
报错如下
```sh
[!] Unable to find a pod with name, author, summary, or description matching `AFNetworking
```
删除本地目录后修复
```sh
 rm ~/Library/Caches/CocoaPods/search_index.json
 ```
 
 使用cocoapods的时候，当cocoapods更新到新的版本的时候，pod install 会报此cocoapods没有满足的版本，要求更新cocoapods，应该是本地缓存的问题，在这里记录一下解决办法，方便以后查找。
 
 *  移除本地master

```sh
sudo rm -fr ~/.cocoapods/repos/master
```

* 移除本地缓存

```sh
sudo rm -fr ~/Library/Caches/CocoaPods/
```

* 重新setup,如果网速较慢,可以在后面加上 --verbose

```sh
pod setup
```