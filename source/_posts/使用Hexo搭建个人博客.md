---
title: 使用Hexo搭建个人博客
tags: [Hexo, Blog, 博客]
categories: 
- 建站
image: 
date: 2018-02-12 13:32:24
top: 2
---



## Hexo简介

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。



## Hexo博客建站步骤

1.安装Git

2.安装Node.js

3.安装Hexo

4.Hexo建站

5.GitHub创建个人仓库

6.Hexo博客文件部署到GitHub

7.设置域名

8.跨设备更新发布博客



### 1.安装Git

Git是目前世界上最先进的分布式版本控制系统

- Windows：下载并安装 [git](https://git-scm.com/download/win).
- Mac：使用 [Homebrew](http://mxcl.github.com/homebrew/), [MacPorts](http://www.macports.org/) 或者下载 [安装程序](http://sourceforge.net/projects/git-osx-installer/)。
- Linux (Ubuntu, Debian)：`sudo apt-get install git-core`
- Linux (Fedora, Red Hat, CentOS)：`sudo yum install git-core`

> **Windows用户**
>
> 可以前往 [淘宝 Git for Windows 镜像](https://npm.taobao.org/mirrors/git-for-windows/) 下载 git 安装包

> **Mac用户**
>
> 如果在编译时可能会遇到问题，请先到 App Store 安装 Xcode，Xcode 完成后，启动并进入 **Preferences -> Download -> Command Line Tools -> Install** 安装命令行工具。



### 2.安装Node.js

Node.js 为大多数平台提供了官方的 [安装程序](https://nodejs.org/zh-cn/download/)。对于中国大陆地区用户，可以前往 [淘宝 Node.js 镜像](https://npm.taobao.org/mirrors/node) 下载。

其它的安装方法：

- Windows：通过 [nvs](https://github.com/jasongin/nvs/)（推荐）或者 [nvm](https://github.com/nvm-sh/nvm) 安装。
- Mac：使用 [Homebrew](https://brew.sh/) 或 [MacPorts](http://www.macports.org/) 安装。
- Linux（DEB/RPM-based）：从 [NodeSource](https://github.com/nodesource/distributions) 安装。
- 其它：使用相应的软件包管理器进行安装，可以参考由 Node.js 提供的 [指导](https://nodejs.org/zh-cn/download/package-manager/)。

对于 Mac 和 Linux 同样建议使用 nvs 或者 nvm，以避免可能会出现的权限问题。



> **Windows 用户**
>
> 使用 Node.js 官方安装程序时，请确保勾选 **Add to PATH** 选项（默认已勾选）

> **Mac / Linux 用户**
>
> 如果在尝试安装 Hexo 的过程中出现 `EACCES` 权限错误，请遵循 [由 npmjs 发布的指导](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally) 修复该问题。强烈建议 **不要** 使用 root、sudo 等方法覆盖权限

> **Linux**
>
> 如果您使用 Snap 来安装 Node.js，在 [初始化](https://hexo.io/zh-cn/docs/commands#init) 博客时您可能需要手动在目标文件夹中执行 `npm install`。



### 3.安装Hexo

Git与Node.js安装完成后，即可使用npm安装Hexo

```bash
$ npm install -g hexo-cli

# 查看hexo版本
$ hexo -v
```



### 4.Hexo建站

Hexo安装完成后，即可开始建站。

选定一个目录如/Hexoblog，打开命令行工具进入到该目录，执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。

```bash
$ hexo init Hexoblog
$ cd Hexoblog
$ npm install
```

新建完成后，Hexoblog文件夹的目录如下：

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

新建完成后，可在本地运行测试服务，在浏览器输入http://localhost:4000就可以看到生成的博客：

```bash
$ hexo server

# 停止服务
ctrl + c
```



### 5.GitHub创建个人仓库

在[GitHub](https://github.com/)上创建一个新的仓库，仓库名为：用户名.github.io，比如我的用户名为heart14，那么新建的仓库名就应该时heart14.github.io。



### 6.Hexo博客文件部署到GitHub

完成了本地博客网站的创建，也完成了GitHub个人仓库的创建，下一步就可以把网站部署到GitHub上，来进行网络访问了。

> 注：此步之前，应该已经完成本地git到GitHub的ssh key配置，保证可以从本地仓库到远程仓库之间的同步操作。



打开Hexo站点配置文件`_config.yml`，在文件内容的最下方找到`deploy:`，修改下方的配置内容：

```bash
deploy:
  type: git
  repo: https://github.com/用户名/用户名.github.io.git
  branch: master
```

然后需要安装[hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git)插件，才可以用命令一键把站点内容部署到GitHub

```bash
$ npm install hexo-deployer-git --save
```

插件安装完成后，就可以进行部署操作了

```bash
$ hexo clean
$ hexo generate
$ hexo deploy
```

- hexo clean 清除缓存文件 (`db.json`) 和已生成的静态文件 (`public`)。

  在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。

- hexo generate 生成静态文件。

- hexo deploy 部署网站。

以上三步命令也可简化为两步：

```bash 
$ hexo clean
$ hexo d -g
```

更多命令可参考[Hexo官方文档](https://hexo.io/zh-cn/docs/commands)



以上，即完成Hexo搭建博客网站并部署到GitHub上，通过GitHub page服务来实现站点发布。

通过访问：https://用户名.github.io 来看看博客效果吧。



### 7.设置域名

待更新


### 8.跨设备编辑发布博客

先占坑。

主要步骤：

1.在GitHub上yourname.github.io仓库上新建一个分支，分支名字可以随便起，比如我的叫做source，作用是用来上传博客源文件，而master分支不动，是用来上传hexo生成的静态博客文件的。

2.在yourname.github.io仓库上设置source分支为默认分支

3.将本地博客源文件上传到source分支，对于我来说也就是上面步骤中的/Hexoblog目录

4.在其它设备上要进行编辑发布博客:

  a)先安装git, node, npm, hexo以及hexo-deployer-git插件

  b)选定一处目录，将GitHub上yourname.github.io仓库克隆下来，由于设置了source分支为默认分支，所以克隆下来的默认分支就是source

  c)在本地仓库进行编辑文章等操作

  d)需要发布的时候，和之前一样，执行hexo clean && hexo g -d命令即可

  e)发布完成后，要将本地仓库上传到远程GitHub上，以便与下次如果换地方再进行发布的时候，远程仓库的博客源文件始终是最新的内容


## Hexo博客发布步骤

博客搭建完成了，那我想发布一篇文章该如何操作呢？

### 1.创建文章

```bash 
$ hexo new post 文章名
```

创建后的文件位于source/_post/目录下

### 2.编写文章

文章内容自由发挥了，注意文章头部front-matter的使用，可以设定文章发表时间、分类、标签等信息

```
---
title: 使用Hexo搭建个人博客
tags: [Hexo, Blog, 博客]
categories: 
- 建站
date: 2020-02-12 13:32:23
---
```

更多Front-matter参数可查看[Hexo文档](https://hexo.io/zh-cn/docs/front-matter)



### 3.发布部署

文章编写完成后，即可进行发布了。

```bash
$ hexo clean
$ hexo d -g
```

打开你的博客站点看看吧~
