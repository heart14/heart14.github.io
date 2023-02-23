---
title: 同时设置公司与个人的Git环境与Maven配置
tags: [Git, GitHub, GitLab, Maven]
categories:
  - 学习笔记
date: 2023-02-22 16:04:52
image:
---





## 需求说明

假如我的笔记本电脑，要在公司作为办公开发电脑，连接公司内网、内网Maven依赖仓库与内网Gitlab代码仓库，开发公司项目。同时要兼顾下班把笔记本带回家，连接阿里云Maven仓库和Github仓库，开发自己的项目。



### 解决方案

首先电脑上需要先安装完git。

在公司，通过https方式，进行公司项目代码从gitlab仓库上传下载，不需要配置git。第一次clone的时候弹出输入账号密码的登录框，登录后记住密码，之后即可在公司pull push 代码了。

在家里，通过ssh方式，进行个人项目代码从GitHub仓库上传下载。需要配置ssh key。

生成shh key

```bash
ssh-keygen -t rsa -C "your_email@example.com"
```

然后将生成的rsa.pub文件内容复制粘贴到github个人设置中的settings-->ssh key，即可进行代码上传下载。



#### git提交记录中的用户信息问题

公司gitlab账号是我的公司邮箱wfli@chinaums.com，GitHub是我的个人邮箱lwf14@qq.com。

由于主要是在公司使用，所以在git通用配置上，设置用户名和邮箱为公司：

```bash
git config --global user.name "wfli"  
git config --global user.email "wfli@chinaums.com"
```

这样在公司，什么都不用管，直接上传下载即可，代码提交记录上显示的就是公司里的个人信息了。



然后，在家里的时候，如果也要提交代码，GitHub上显示的提交记录，也是公司里的个人信息(wfli@chinaums.com)，这样其实也没问题，都是自己。但是如果想让它显示为GitHub的注册用户名和邮箱的话，可以在每个从GitHub下载下来的仓库目录下，单独设置user.name和email属性：

```bash
git config --local user.name "heart14"  
git config --local user.email "lwf14@qq.com"
```

这个--local参数使得这个配置只在这个目录生效，所以不会影响公司项目的提交。



这样配置之后，即可实现，公司项目通过HTTPS方式，以wfli/wfli@chinaums.com用户提交代码，个人项目通过SSH方式，以heart14/lwf14@qq.com用户提交代码。

> 参考：
> https://www.cnblogs.com/yanglang/p/9563496.html
> https://www.cnblogs.com/hezhi/p/10317642.html





#### maven依赖仓库问题

##### 问题说明

还有一个情况就是，

在公司用的是公司搭建的私域仓库，在家用的是阿里云公共仓库。

在公司连不了外网，用不了阿里云仓库，在家连不上内网，用不了公司仓库。

难道每次都要去修改maven/conf/settings.xml文件吗？



其实也不用。可以配置一下maven/conf/settings.xml，使得在不同环境，不同项目，可以成功下载自己所需的依赖。

##### 配置方法

在maven/conf/settings.xml的profile节点进行配置maven仓库镜像，这里可以配置多个仓库，他们会同时生效，也可以在Intellj idea上的maven插件上进行控制具体哪个仓库生效。

这里配置多个仓库后，会依次检索每个生效的仓库，如果连接失败或者没找到所需的依赖包，会去下一个生效的仓库查找。这样公司项目可以从公司maven仓库下载依赖，个人项目也可以从阿里云仓库下载依赖了。



而以往在mirrors里面配置，mirror相当于是对仓库的一个备份。即便配置了多个mirror，也只有在第一个仓库连接失败的时候才会进行连接下一个仓库，否则，只要连接成功，即使在仓库里没有找到依赖，也不会去下一个仓库找。这样就不能满足我们的需求了。







