---
title: Mac mini M2(macOS Ventura 13.3.1)安装Node.js
tags: [Mac, Mac mini, Apple Silicon, M2, macOS, Node]
categories:
  - macOS
date: 2023-04-18 15:54:30
image:
---





### 1.查看是否已安装Node.js

where node
node -v
两个命令分别返回
node not found
zsh: command not found: node
应该是没有安装过node的，而且我这新电脑



### 2.关于node.js和nvm

node.js
nvm是node的版本管理工具，它可以便捷的下载更新管理node的版本，所以安装这个就好了，使用它来下载安装node



### 3.安装nvm

nvm官方Github页面 https://github.com/nvm-sh/nvm
根据Readme里面的操作说明，使用下面的命令来安装nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash

但是终端打印错误
curl: (7) Failed to connect to raw.githubusercontent.com port 443 after 32 ms: Couldn't connect to server
又不行
意思是网络连不上服务器



通过修改hosts的方法解决这个问题
在https://site.ip138.com/raw.Githubusercontent.com/查询ip
然后在终端执行sudo vim /etc/hosts命令编辑hosts文件
在末尾添加上
185.199.108.133 raw.githubusercontent.com
185.199.109.133 raw.githubusercontent.com
185.199.110.133 raw.githubusercontent.com
185.199.111.133 raw.githubusercontent.com
182.43.124.6 raw.githubusercontent.com
20.205.243.166 github.com
保存退出



重新执行安装命令

这次倒是下载成功了，但是紧接着又来了一个git clone的错误
=> Cloning into '/Users/liwenfei/.nvm'...
fatal: unable to access 'https://github.com/nvm-sh/nvm.git/': HTTP/2 stream 1 was not closed cleanly before end of the underlying stream
Failed to clone nvm repo. Please report this!



网络不好，重试一次，依然报错
=> Cloning into '/Users/liwenfei/.nvm'...
fatal: unable to access 'https://github.com/nvm-sh/nvm.git/': Recv failure: Operation timed out
Failed to clone nvm repo. Please report this!

网络不好，继续重试



......



终于试成功了

```bash
liwenfei@macli Documents % curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 15916  100 15916    0     0    206      0  0:01:17  0:01:17 --:--:--  4483
=> Downloading nvm from git to '/Users/liwenfei/.nvm'
=> Cloning into '/Users/liwenfei/.nvm'...
remote: Enumerating objects: 359, done.
remote: Counting objects: 100% (359/359), done.
remote: Compressing objects: 100% (305/305), done.
remote: Total 359 (delta 40), reused 169 (delta 28), pack-reused 0
Receiving objects: 100% (359/359), 219.46 KiB | 881.00 KiB/s, done.
Resolving deltas: 100% (40/40), done.
remote: Enumerating objects: 54, done.
remote: Counting objects: 100% (54/54), done.
remote: Compressing objects: 100% (28/28), done.
remote: Total 28 (delta 26), reused 2 (delta 0), pack-reused 0
Unpacking objects: 100% (28/28), 4.38 KiB | 204.00 KiB/s, done.
From https://github.com/nvm-sh/nvm

 * tag               v0.39.3    -> FETCH_HEAD
* (HEAD detached at FETCH_HEAD)
  master
  => Compressing and cleaning up git repository

=> Appending nvm source string to /Users/liwenfei/.zshrc
=> Appending bash_completion source string to /Users/liwenfei/.zshrc
=> Close and reopen your terminal to start using nvm or run the following to use it now:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```




验证安装

要验证nvm是否已安装，请执行以下操作：

> command -v nvm

如果安装成功，它应该输出nvm。请注意，which nvm不起作用，因为nvm是一个源shell函数，而不是可执行的二进制文件。
注意：在Linux上，在运行安装脚本后，如果您在键入command -v nvm后获得nvm: command not found或没有看到来自终端的反馈，只需关闭当前终端，打开新终端，然后再次尝试验证。

如上面所说，在刚安装成功时，在终端执行command -v nvm命令，没有任何反应
退出终端重新打开终端再次执行command -v nvm命令，成功打印出nvm
说明已经安装成功

4.使用nvm安装node.js
nvm ls-remote 命令查看所有可安装的node版本
nvm install node 命令进行安装

```bash
liwenfei@macli ~ % nvm install node
Downloading and installing node v19.9.0...
Downloading https://nodejs.org/dist/v19.9.0/node-v19.9.0-darwin-arm64.tar.xz...
######################################################################################################################################### 100.0%
Computing checksum with shasum -a 256
Checksums matched!
Now using node v19.9.0 (npm v9.6.3)
Creating default alias: default -> node (-> v19.9.0)
liwenfei@macli ~ % 
liwenfei@macli ~ % node -v
v19.9.0
```

或者使用nvm install 16.20.0这样指定版本号的命令，来安装指定版本

然后node -v成功输出node版本信息
安装完成！
