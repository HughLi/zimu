---
title: 'pi出的蠢问题,蠢哭了'
date: 2017-09-06 13:43:59
tags:
---

### 安装完npm后出的错

昨天发现备胎系统里面有node 但是没有npm(真是奇葩镜像),于是手动安装了npm
安装完成后

```sh
	sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}
```

再后面发现sudo不能使用

```sh
中文版: sudo：/usr/bin/sudo 务必属于用户 ID 0(的用户)并且设置 setuid 位
英文版:sudo: /usr/bin/sudo must be owned by uid 0 and have the setuid bit set
```

由于自己在之前激活了raspberry的root,

```sh
 sudo passwd root
 sudo passwd --unlock root
```
所以以为是root的问题于是把root 重新lock

```sh
sudo passwd --lock root
```

发现还是不行
上网一查 尼玛需要使用root权限

>1. Log out as the current user, then log back in as root.
>2. Execute chown root:root /usr/bin/sudo && chmod 4755 /usr/bin/sudo
>3. Log out as root, then log back in as the current user.

这个系统算是费了,真替自己捉急啊


