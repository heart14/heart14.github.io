---
title: Mac mini M2(macOS Ventura 13.3.1)安装Git
tags: [Mac, Mac mini, Apple Silicon, M2, macOS, Git]
categories:
  - macOS
date: 2023-04-18 15:51:16
image:
---







系列第一弹，新买了一台Mac mini M2，记录一下Java程序员的一些环境搭建过程



### 1.查看当前系统Git

打开终端，执行git --version命令查看git版本，检查是否安装了git以及git版本

但是此处报错了，主要信息为*xcode-select: note: No developer tools were found, requesting install.* 并且弹窗提示安装XCode。

如果按弹窗提示去安装XCode的话，需要下载好几个Gb的内容，而且我也不做苹果应用开发，所以不想安装XCode。

实际上，这里Git并非一定要安装XCode，而是需要一个Command Line Tools，可以去苹果开发者网站上单独下载这个Command Line Tools。

打开苹果开发者网站（https://developer.apple.com/download/all/），登录AppleID，登录后看到More Downloads里面，会有Command Line Tools，也可以直接搜索关键字‘Command’，然后点击xxx.dmg下载，下载完成后双击打开进行安装即可。

安装完成后，再次执行git --version命令查看git版本，成功输出git版本信息。



但此时的git是macOS自带的git，据说不好用，所以一般都是重新下载官网的git。



### 2.安装Git

通过Git官网(https://git-scm.com/download/mac)的指南，有以下几种方式来安装Git：

 - Homebrew
Install homebrew if you don't already have it, then:
$ brew install git

 - MacPorts
Install MacPorts if you don't already have it, then:
$ sudo port install git

 - Xcode
Apple ships a binary package of Git with Xcode.

 - Binary installer
Tim Harper provides an installer for Git. The latest version is 2.33.0, which was released over 1 year ago, on 2021-08-30.

 - Building from Source
If you prefer to build from source, you can find tarballs on kernel.org. The latest version is 2.40.0.

 - Installing git-gui
If you would like to install git-gui and gitk, git's commit GUI and interactive history browser, you can do so using homebrew
$ brew install git-gui

而且！网上一些推荐教程里面都是说无需卸载mac自带的git，只需安装好新的git后把环境变量配置改为指向新的git home即可，但是，我这台新的mac mini，没有~/.bash_profile文件，也没有~/.zshrc文件，我也不知道现在的环境变量配置在哪里了，所以我决定不安装新的git了，就用系统自带的这个吧。。。



### 3.配置Git

#### 3.1.基本配置

查看Git全局配置

> git config --global --list 

设置Git用户信息

> git config --global user.name "your name"
> git config --global user.email "your email"



#### 3.2.配置Github

##### 3.2.1生成ssh key

> ssh-keygen -t rsa -C "your email"

在终端中执行上述命令，回车后会有三次让你输入密码的地方，直接按回车键跳过即可

命令执行完之后，生成的id_rsa和id_rsa.pub文件位于~/.ssh目录



##### 3.2.2Github配置

a)复制id_rsa.pub文件内的所有内容

b)打开Github -> settings -> SSH and GPG Keys -> New SSH key -> 填写一个Title，然后在下面框里粘贴刚才复制到内容，保存即可



##### 3.2.3测试Github连接

测试命令

> ssh -T git@github.com

在终端中执行该命令，只要能出现以下内容，就是说明Github连接成功

> Hi [your name]! You've successfully authenticated, but GitHub does not provide shell access.



> 可能是因为第一次执行，打印了以下内容
> The authenticity of host 'github.com (20.205.243.166)' can't be established.
> ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
> This key is not known by any other names
> Are you sure you want to continue connecting (yes/no/[fingerprint])?
> 直接输入yes
> 然后又打印出以下内容
> Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
> PTY allocation request failed on channel 0
> Hi heart14! You've successfully authenticated, but GitHub does not provide shell access.
> 同时发现yes之后，在~/.ssh目录下生成了一个known_hosts文件
>
> 说明已经成功配置好Github连接
> 再次执行ssh -T git@github.com命令，已经不会再问yes不yes了



> 参考：
> 优雅的卸载Mac默认的Xcode附带的Git https://ld246.com/article/1399808431515
> Git命令大全 https://www.runoob.com/note/56524
