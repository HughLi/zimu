---
title: 鹅厂的Sonic
date: 2017-08-10 08:56:00
tags:
---
听说能够大幅度提升首页H5的用户体验,不过要求要前后端的配合,目前只有PHP和Node.js,持续关注中,引用一段官方的FAQ:
>目前Sonic使用的是UIWebView，暂时没有支持WKWebView，原因如下：

>1.WKWebView没法做NSURLProtocol拦截，而且WKWebView的回调都是IPC跨进程的，渲染也比UIWebView慢；

>2.WKWebView目前不支持NSURLProtocol拦截是最大的问题，网上流传的私有接口拦截有一个大问题就是Post请求会丢失掉body；

>3.iOS11虽然又提供了拦截注册，但是不允许注册http和https这种系统默认的schema。
>

[项目地址](https://github.com/Tencent/VasSonic)