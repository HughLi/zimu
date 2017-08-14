---
title: React-Native 备忘
date: 2017-08-04 09:41:39
tags:
---
1. 在react-native的标签内使用
```sh
`...${...}`
```
模板没有效果

2. 报错
![img](http://ostglltzu.bkt.clouddn.com/17-8-4/52097411.jpg)

解决办法:在FlatList 标签内添加

```sh
<FlistList
keyExtractor={this._keyExtractor}
/>
function   _keyExtractor = (item,index) => item.id;

```
