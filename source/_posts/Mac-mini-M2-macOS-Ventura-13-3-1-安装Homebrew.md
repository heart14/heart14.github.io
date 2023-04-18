---
title: Mac mini M2(macOS Ventura 13.3.1)安装Homebrew
tags: [Mac, Mac mini, Apple Silicon, M2, macOS, Homebrew]
categories:
  - macOS
date: 2023-04-18 15:53:37
image:
---





### 1.查看是否已安装了homebrew

打开终端，执行命令 brew -v
显示zsh: command not found: brew
说明还未安装



### 2.安装homebrew

homebrew的Github主页 https://github.com/Homebrew/brew
homebrew的官方主页 https://brew.sh/index_zh-cn



按照homebrew官方主页中给的命令进行安装：
在终端中执行命令 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
但是打印了错误：
curl: (7) Failed to connect to raw.githubusercontent.com port 443 after 210 ms: Couldn't connect to server
意思是连接不上raw.githubusercontent.com服务器



改用科大源安装Homebrew
首先在命令行运行如下几条命令设置环境变量：
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles"
export HOMEBREW_API_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles/api"
注意 export命令是一次性设置环境变量



之后在命令行运行 Homebrew 安装脚本：
/bin/bash -c "$(curl -fsSL https://github.com/Homebrew/install/raw/HEAD/install.sh)"
但是又失败了
打印错误：
curl: (56) Recv failure: Operation timed out
操作超时，应该还是github的问题，国内网不太好连



然后看到科大源镜像使用帮助上写到：
初次安装 Homebrew / Linuxbrew 时，如果无法下载安装脚本， 可以使用我们每日同步的安装脚本文件。
/bin/bash -c "$(curl -fsSL https://mirrors.ustc.edu.cn/misc/brew-install.sh)"



在终端中执行这个命令
很快有了反应，让输入密码
输入开机密码，回车继续
然后又出现一行Press RETURN/ENTER to continue or any other key to abort
我们继续ENTER
然后终端会继续打印下载过程，等待...
安装顺利完成！



3.环境变量配置

安装完成时终端上打印了以下内容

```bash
......
Warning: /opt/homebrew/bin is not in your PATH.
  Instructions on how to configure your shell for Homebrew
  can be found in the 'Next steps' section below.
==> Installation successful!

==> Homebrew has enabled anonymous aggregate formulae and cask analytics.
Read the analytics documentation (and how to opt-out) here:
  https://docs.brew.sh/Analytics
No analytics data has been sent yet (nor will any be during this install run).

==> Homebrew is run entirely by unpaid volunteers. Please consider donating:
  https://github.com/Homebrew/brew#donations

==> Next steps:
- Run these two commands in your terminal to add Homebrew to your PATH:
    (echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/liwenfei/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
- Run these commands in your terminal to add the non-default Git remotes for Homebrew/brew and Homebrew/homebrew-core:
    echo '# Set PATH, MANPATH, etc., for Homebrew.' >> /Users/liwenfei/.zprofile
    echo 'export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"' >> /Users/liwenfei/.zprofile
    echo 'export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"' >> /Users/liwenfei/.zprofile
    export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"
    export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"
- Run brew help to get started
- Further documentation:
    https://docs.brew.sh
```



意思应该是让我配置环境变量
按照指示
在终端依次执行下面两条命令
(echo; echo 'eval "$(/opt/homebrew/bin/brew shellenv)"') >> /Users/liwenfei/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
然后执行brew -v
成功打印出版本号 Homebrew 4.0.13



继续按照指示配置环境变量
echo '# Set PATH, MANPATH, etc., for Homebrew.' >> /Users/liwenfei/.zprofile
echo 'export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"' >> /Users/liwenfei/.zprofile
echo 'export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"' >> /Users/liwenfei/.zprofile
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"



然后执行brew help成功打印出brew帮助信息

说明homebrew安装成功完成！





> 参考：
> https://www.funkyspacemonkey.com/how-to-install-homebrew-on-m1-macs-running-macos-monterey
> https://cloud.tencent.com/developer/article/1853162
> https://zhuanlan.zhihu.com/p/607620531
> https://www.jianshu.com/p/4d051b10af7e
> 科大镜像源相关
> https://mirrors.ustc.edu.cn/help/index.html#
> https://mirrors.ustc.edu.cn/help/brew.git.html







