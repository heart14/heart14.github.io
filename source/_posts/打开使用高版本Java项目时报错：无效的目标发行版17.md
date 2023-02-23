---
title: 打开使用高版本Java项目时报错：无效的目标发行版17
tags: [Java, Java17]
categories:
  - 学习笔记
date: 2023-02-20 11:15:24
image:
---



不同项目使用不同jdk版本的配置。



#### 问题描述

1.本机安装的jdk版本为jdk1.8.0_241（已配置环境变量）

2.新项目A的pom.xml中配置了<java.version>17</java.version>属性，说明它使用的是jdk17版本

3.由于其它项目引入A项目依赖，所以要对A项目先进行 maven clean install

在点击maven install的时候报错：无效的目标发行版17



#### 问题原因

本机安装的jdk版本与项目使用的jdk版本不一致



#### 说明

由于需要在不更改项目java版本的情况下成功运行，所以网上一些教程里使用的“修改项目中的Java版本”并不适用我的情况



#### 解决方法

本机安装的jdk版本与项目使用的jdk版本不一致，那么我需要重新下载一个jdk17并安装

但两个版本的jdk在同一台机器上不能同时安装生效（因为不能同时配置两个目录不相同的JAVA_HOME环境变量）

jdk1.8是使用msi installer方式安装的，所以jdk17选择下载压缩文件，直接进行解压安装

然后不要配置jdk17的环境变量，保持本机环境生效的依然是jdk1.8

在使用java17的项目中进行手动引入jdk并选择使用



#### 解决过程

##### 下载jdk17

[Java Downloads | Oracle](https://www.oracle.com/java/technologies/downloads/)

在官方下载页面，选择windows - x64 Compressed Archive 下载压缩文件



##### 解压jdk17



##### 手动添加jdk

Idea打开项目，依次选择 File - Project Structure - Platform Settings - 加号，然后在弹窗里选择刚才解压目录，点击确定

点击Apply



##### 修改项目SDK

在File - Project Structure - Project Settings - Project 菜单中修改SDK选项配置为刚才添加的jdk17，同时Language level选项值为SDK default



##### 查看其它响应配置

上一步修改完成后，在File - Project Structure - Project Settings - Modules 里面的SDK版本应该也随之变为jdk17



##### 完成修改

点击OK



##### 验证

此时，再次执行maven clean install命令，可以成功执行

同时，cmd中查看Java -version可看到环境变量中的jdk版本依然为jdk1.8.0_241，也不会影响其它项目的jdk版本。（打开其它项目时最好看一下配置是否是自己原先的配置）

