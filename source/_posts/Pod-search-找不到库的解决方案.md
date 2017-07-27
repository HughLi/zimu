---
title: Pod search 找不到库的解决方案
date: 2017-07-21 09:12:02
tags:
---
报错如下
```
[!] Unable to find a pod with name, author, summary, or description matching `AFNetworking
```
删除本地目录后修复
```
 rm ~/Library/Caches/CocoaPods/search_index.json
 ```