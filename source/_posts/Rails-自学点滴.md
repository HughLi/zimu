---
title: Rails 自学点滴
date: 2018-01-22 13:57:55
tags:
---

### 从零开始构建我的Rails
1. 新建项目
```sh
$ rails new projectName
```
2. 设置服务端口
```sh
$ rails server -b $IP -p $PORT
```
3. rails 5的render 后面接 html *
4. rails 撤销操作

> rails generate controller StaticPages home help  
 rails destroy controller StaticPages home help
 
> rails generate model User name:string email:string      
 rails destroy model User

迁移通过下面的命令改变数据库的状态：

```sh
$ rails db:migrate
```
我们可以使用下面的命令撤销前一个迁移操作：

````sh
$ rails db:rollback
````
如果要回到最开始的状态，可以使用：

````sh
$ rails db:migrate VERSION=0
````
你可能猜到了，把数字 0 换成其他数字就会回到相应的版本，

####  Heroku部署
1. Git push heroku master

rails 部署

rails server -b $IP -p $PORT

 
![](http://ostglltzu.bkt.clouddn.com/18-1-11/9325858.jpg)