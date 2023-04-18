---
title: Mac mini M2(macOS Ventura 13.3.1)安装Maven
tags: [Mac, Mac mini, Apple Silicon, M2, macOS, Maven]
categories:
  - macOS
date: 2023-04-18 15:54:15
image:
---





### 1.查看是否已安装了maven

打开终端，执行命令 mvn -v
显示zsh: command not found: mvn
说明还未安装



### 2.安装maven

#### 2.1.下载maven

官网地址https://maven.apache.org/download.cgi
历史版本归档地址https://archive.apache.org/dist/maven/maven-3/

下载3.6.0版本tar.gz文件



#### 2.2.解压

打开下载目录，双击apache-maven-3.6.0-bin.tar.gz文件，解压出来apache-maven-3.6.0目录，将它移动到你想放到位置



#### 2.3.配置环境变量

由于我新电脑没有.bash_profile文件，所以在终端中执行以下命令来创建和编辑.bash_profile文件

> touch ~/.bash_profile
> open -e ~/.bash_profile

在弹出的编辑窗口中粘贴以下内容：

> export MAVEN_HOME=/Users/liwenfei/Workspace/apache/apache-maven-3.6.0
> export PATH=$PATH:$MAVEN_HOME/bin

保存并退出

在终端中执行source ~/.bash_profile命令来使环境变量生效

再次验证：mvn -v
弹出两次“无法打开libjansi.jnilib”，均点取消（解决方案见参考列表）
然后终端打印出maven版本信息

> Apache Maven 3.6.0 (97c98ec64a1fdfee7767ce5ffb20918da4f719f3; 2018-10-25T02:41:47+08:00)
> Maven home: /Users/liwenfei/Workspace/apache/apache-maven-3.6.0
> Java version: 1.8.0_361, vendor: Oracle Corporation, runtime: /Library/Internet Plug-Ins/JavaAppletPlugin.plugin/Contents/Home
> Default locale: zh_CN, platform encoding: UTF-8
> OS name: "mac os x", version: "13.3.1", arch: "x86_64", family: "mac"



#### 2.4.配置永久生效

当按以上方法设置完成后，当时可以，但是重新打开终端应用或者电脑重启后，这个环境变量就失效了
zsh: command not found: mvn

在终端中执行以下命令来创建和编辑.zshrc文件
touch ~/.zshrc
open -e ~/.zshrc
在弹出的编辑窗口中写入source ~/.bash_profile并保存
然后再在终端中执行source ~/.zshrc命令来使环境变量生效
再次验证：mvn -v
成功打印出maven版本信息
重新打开终端再次验证
也成功打印出maven版本信息
ok



#### 2.5.修改配置文件

进入maven配置文件目录

> cd /Users/liwenfei/Workspace/apache/apache-maven-3.6.0/conf

备份配置文件

> cp settings.xml settings.xml.default

修改配置文件

> vim settings

a)修改repository目录

```xml
	<localRepository>/Users/liwenfei/Workspace/apache/apache-maven-repository</localRepository>
```

b)修改远程仓库镜像mirror为阿里云maven镜像

```xml
    <mirror>
      <id>aliyun</id>
      <mirrorOf>central</mirrorOf>
      <name>Nexus aliyun maven repo.</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>
```

保存并退出，即可正常使用Maven了



参考：
Maven安装 https://www.cnblogs.com/benjieqiang/p/17261978.html
Maven报错“无法打开libjansi.jnilib” https://www.cnblogs.com/shenxiaolin/p/15081246.html
