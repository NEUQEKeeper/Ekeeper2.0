---
title: 如何快速搭建一个像这样的博客（二）
date: 2023-5-29
description: giiiiiiit nmsl
categories:
  - 培训
tags:
  - 博客
---

在上一篇中，你已经通过`fork`现成的仓库和`netlify`发布了你自己的博客，本篇将讲述如何通过工具 Git 修改自己博客

## 如何修改你的 GitHub 仓库

> 首先需要明确的是，每一个 GitHub 仓库，实际上都是一个 Git 仓库，Git 的 [wiki 百科](https://zh.wikipedia.org/wiki/Git)
>
> **git**是一个[分布式版本控制](https://zh.wikipedia.org/wiki/分散式版本控制)软件，最初由[林纳斯·托瓦兹](https://zh.wikipedia.org/wiki/林纳斯·托瓦兹)创作，于2005年以[GPL](https://zh.wikipedia.org/wiki/GPL)许可协议发布。最初目的是为了更好地管理[Linux内核](https://zh.wikipedia.org/wiki/Linux内核)开发而设计。应注意的是，这与GNU [Interactive Tools](https://zh.wikipedia.org/wiki/Git#cite_note-gnuit-4)（一个类似[Norton Commander](https://zh.wikipedia.org/w/index.php?title=Norton_Commander&action=edit&redlink=1)界面的[文件管理器](https://zh.wikipedia.org/wiki/软件包管理系统)）不同

管理 GitHub（Git 仓库），自然需要使用软件 Git

### 下载并配置 git

[Git - Downloads (git-scm.com)](https://git-scm.com/downloads)

选择 Windows 64 位  Setup 下载安装，没有特殊需求安装过程一路默认就行，使用 git -v 命令检查并查看 git 版本

```sh
C:\Users\northboat>git -v
git version 2.37.2.windows.2
```

绑定 GitHub 账号：请更换成自己的用户名和邮箱

```sh
git config --global user.name "northboat"
git config --global user.email "northboat@163.com"
```

生成 ssh 密钥，这个密钥用于绑定你的 GitHub，以提供安全可靠的传输（感兴趣的可以自行搜索 ssh 相关知识）（同样的请换成自己的邮箱）

```sh
ssh-keygen -t rsa -C "northboat@163.com"
```

如果是windows，你将在`C:\Users\northboat\.ssh`下找到一个名叫`id_rsa.pub`的文件，其中 northboat 是我的电脑用户名，另外如果你只看到两个`id_rsa`的同名文件，请在查看中把文件名后缀勾选，用记事本打开 id.rsa.pub 并复制其内容

点击 GitHub 右上角头像 Settings - SSH and GPG keys，点击绿色按钮 New SSH Key

随便给密钥取个名字，将密钥（刚刚复制的东西）粘贴到 Key 文本框内，点击 Add SSH key 即可完成添加

<img src="/img/assets/image-20230606020729142.png">

自此你的本地 Git 绑定了你的 GitHub 账号，可以开始远程仓库的拉取和推送

### 基本的 git 操作

#### git clone

将 Git 仓库从云上拉下来，从这个地方获取仓库 ssh 链接

<img src="/img/assets/image-20230606015802510.png">

```sh
git clone git@github.com:NEUQEKeeper/Ekeeper2.0.git
```

此时不出意外，你已经成功的将仓库下在了本地（在当前文件夹下）

#### git add

将变更提交到暂存区，如提交一个名叫 nmsl 的 markdown 文档，则键入命令

```sh
git add nmsl.md
```

此时这个文档就存到了仓库的暂存区，暂存区，怎么说，可以理解为一个购物车，提交之前的缓冲（避免你一下子给提交到仓库版本不好回退），在这个暂存区，文件的撤回和继续添加相对方便，当你确定好购买的商品后，再统一点击付款清空购物车里的内容

#### git commit

将暂存区的文件一股脑全部放入 Git 仓库（即清空购物车的操作），也就是所谓的更新一次仓库版本，比如你之前 git add nmsl.md，又了 git add wdnmd.md（此时这两个文件都在购物车内），此时 commit

```sh
git commit -m "这是一个注释记录，记录本次提交的更改，这一项是必须的，也是随意的"
```

则将上述两个文件都提交到了 Git 仓库（付款成功），此时仓库的版本向前迭代一轮，同时暂存区清空

就像买东西一样，即使付款成功后，也是可以选择退款的，只不过更繁琐一点（暂存区的存在在这就显得很有必要），感兴趣的可以搜一下，关键词：git 仓库回退

为什么要设置暂存区？从某方面来看，这和为什么要设置购物车其实是一样的

#### git push

最后一步，即在本地仓库根目标执行 git push 命令

```sh
git push
```

这条命令将把你的本地 Git 仓库推送到 GitHub 上，本地仓库根目录下的 .git 文件夹保存了诸如版本信息，远程仓库等数据，所以你只需要 push 就完事了

注意这里的三个命令`add、commit、push`都需要在仓库的根目录下执行

### 修改博客内容

在上一篇拉取的博客仓库中，你可以在 source - _post 文件夹里面发现一堆后缀为 .md 的文件，这实际上就是博客上的文章（被渲染之前的样子）

你可以任意的删除、增加或修改 .md 文件，Hexo 博客将自动识别并构建相应的 HTML 页面，你可以试试随便将一个文件修改成你的形状，然后 git 三步走`add - commit - push`到你的 GitHub 仓库

上一篇中，你已经用 Netlify 绑定了你的仓库，一旦仓库被修改（版本变更），Netlify 将自动为新版本的仓库重新启动部署，当你 push 成功之后，大概一分钟左右再次访问你的博客域名，将会看到你的修改

关于 .md 文件的编辑，笔者推荐使用 Typora，下载链接：

https://www.123pan.com/s/ODW8Vv-boGoA.html

提取码:mMJF

在安装成功后，请在 typora 的根目录下找到 resource 目录，将 app.asar 替换为我给你的下载链接里面的，然后启动 typora，再从 key.txt 里面随便找一个激活码激活即可完成破解

最后附上一份 Hexo Butterfly 主题的文档（爱来自台湾），以帮助你将自己的博客变成自己的形状

[Butterfly 安裝文檔(三) 主題配置-1 | Butterfly](https://butterfly.js.org/posts/4aa8abbe/)

