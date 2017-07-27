---
title: Python 在Mac 上的多版本管理及pip安装
date: 2017-07-20 09:29:42
tags:
---
### 1. 安装homebrew

官网 http://brew.sh/index_zh-cn.html

打开终端，在终端中粘贴如下脚本
````
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
````
测试是否安装成功

在终端中输入 
```
brew -v
```
出现类似提示，即代表安装成功
```
Homebrew 0.9.5 (Git revision 1021; last commit 2016-03-30)
```
 

### 2.安装pyenv

在终端中输入
```
brew install pyenv
```
验证是否安装成功 
```
pyenv -v
```
 出现类似结果，即代表安装成功
```
pyenv 20150310
```

 

### 3.查看可安装的Python版本

在终端中输入  
```
pyenv install --list
```
 会列出可安装的python版本号

 

### 4.安装特定版本的Python

在终端中输入 
```
pyenv install <version> 
```
安装对应的Python版本，如: 
```
pyenv install 2.7.11
 ```

### 5.异常处理

如出现如下异常：


```
Installing Python-2.7.11...
ERROR: The Python zlib extension was not compiled. Missing the zlib?

Please consult to the Wiki page to fix the problem.
https://github.com/yyuu/pyenv/wiki/Common-build-problems


BUILD FAILED (OS X 10.11.5 using python-build 20160130)

Inspect or clean up the working tree at /var/folders/fb/7406jr3s60z_tdpxxqm2s9hh0000gn/T/python-build.20160616162746.48644
Results logged to /var/folders/fb/7406jr3s60z_tdpxxqm2s9hh0000gn/T/python-build.20160616162746.48644.log

Last 10 log lines:
rm -f /Users/Matrix/.pyenv/versions/2.7.11/share/man/man1/python.1
(cd /Users/Matrix/.pyenv/versions/2.7.11/share/man/man1; ln -s python2.1 python.1)
if test "xno" != "xno"  ; then \
        case no in \
            upgrade) ensurepip="--upgrade" ;; \
            install|*) ensurepip="" ;; \
        esac; \
         ./python.exe -E -m ensurepip \
            $ensurepip --root=/ ; \
    fi
yujingyao:2.7.11 Matrix$ CFLAGS="-I$(brew --prefix openssl)/include" LDFLAGS="-L$(brew --prefix openssl)/lib" pyenv install 3.6-Dev
Cloning https://hg.python.org/cpython...
error: please install `mercurial` and try again
```
 则在终端中输入，注意替换如下代码的版本号
```
CFLAGS="-I$(brew --prefix openssl)/include -I$(xcrun --show-sdk-path)/usr/include" \
LDFLAGS="-L$(brew --prefix openssl)/lib" \
pyenv install -v 2.7.11
```
 在EI Capitan实测有效

资料来源：
>https://github.com/yyuu/pyenv/issues/448

如有其他异常可以参考
> https://github.com/yyuu/pyenv/wiki/Common-build-problems 

### 6.查看pyenv已安装的Python版本
```
pyenv versions
```
 在终端中会列出已安装的Python版本，如
 
```
2.7.11

3.5.1
```
 

### 7.编辑.bash_profile文件

在终端中输入如下命令，进入当前用户的Home目录

```
cd ~
```
输入如下命令，打开.bash_profile文件

```
open .bash_profile
```
如不存在，则输入如下命令，创建文件

```
touch .bash_profile
```

编辑文件

````
open -e .bash_profile
````
在弹出的.bash_profile文件中新增

````
if which pyenv > /dev/null; then eval "$(pyenv init -)"; fi
````

command + s 保存文件

在终端中输入如下命令，使用更新之后的.bash_profile内容

```
source .bash_profile
```
### 8.指定目录切换指定版本的Python

在终端中cd到特定目录，路径名称自行修改

```
cd /Users/Matrix/Documents/Projects/Python/3.5.1 
```
输入：

```
pyenv local <version>
```
如

```
pyenv local 3.5.1
```
 

### 9.设定全局的Python版本

在终端中输入

```
pyenv global <version>
```
如

```
pyenv global 3.5.11
```
不建议如此操作，可能会导致部分系统程序无法正常工作

 

### 10.检查是否切换成功

在终端中cd到特定目录，路径名称自行修改

```
cd /Users/Matrix/Documents/Projects/Python/3.5.1 
```
 在终端中输入：
 
```
python
```
会列出当前目录使用的python版本，和设置的版本一样则代表切换成功
### 11. 使用python3 安装pip
    python3  setup.py install