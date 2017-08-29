---
title: iOS一些控件细节设置
date: 2017-08-23 09:51:52
tags:
---
## iOS 控件的一些小设置
```sh
textFiled.autocorrectionType = UITextAutocorrectionTypeNo;//取消自动联想输入
textFiled.autocapitalizationType = UITextAutocapitalizationTypeNone;//取消首字母大写
```

NSString 转换成NSData 对象 
```sh
NSData* xmlData = [@"testdata" dataUsingEncoding:NSUTF8StringEncoding]; 
```
NSData 转换成NSString对象 

```sh
NSData * data; 
NSString *result = [[NSString alloc] initWithData:data  encoding:NSUTF8StringEncoding]; 
```
NSData 转换成char* 

```sh
NSData *data; 
char *test=[data bytes];
``` 
char* 转换成NSData对象 

```sh
byte* tempData = malloc(sizeof(byte)*16); 
NSData *content=[NSData dataWithBytes:tempData length:16];
```

[iOS绘制图表三方库](https://github.com/danielgindi/Charts)
