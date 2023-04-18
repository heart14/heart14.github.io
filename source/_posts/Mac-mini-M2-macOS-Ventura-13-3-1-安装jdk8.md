---
title: Mac mini M2(macOS Ventura 13.3.1)安装jdk8
tags: [Mac, Mac mini, Apple Silicon, M2, macOS, Java, Jdk]
categories:
  - macOS
date: 2023-04-18 15:53:59
image:
---





这个安装非常简单，简单到我怀疑是不是有问题！

1.官网下载jdk-8u361-macosx-x64.dmg
2.在下载目录双击打开dmg,弹出的文件夹里面有个pkg
3.双击打开pkg文件，按照指示点击继续或者下一步，即可完成安装



ps
1.第一步打开dmg的时候，可能会提醒需要安装rosetta，安装即可
2.安装完打开终端执行java -version即可查看到版本号，竟然无需配置环境变量？为什么？
echo $JAVA_HOME命令打印为空！！！



where java命令打印结果为/usr/bin/java
ls -al /usr/bin | grep java命令打印结果
-rwxr-xr-x   52 root   wheel    168384  4  2 00:46 java
-rwxr-xr-x   52 root   wheel    168384  4  2 00:46 javac
-rwxr-xr-x   52 root   wheel    168384  4  2 00:46 javadoc
-rwxr-xr-x   52 root   wheel    168384  4  2 00:46 javah
-rwxr-xr-x   52 root   wheel    168384  4  2 00:46 javap
-rwxr-xr-x   52 root   wheel    168384  4  2 00:46 javapackager
-rwxr-xr-x   52 root   wheel    168384  4  2 00:46 javaws

所以jdk安装目录在哪里？那些bin lib jar什么的去哪了？
在这里：/Library/Java/JavaVirtualMachines/jdk1.8.0_361.jdk/Contents/Home
