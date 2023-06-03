---
title: 相对简单的git入门
date: 2023-06-04
description: 
categories:
  - 通识基础
---

在讨论git的基本使用之前，我们首先要知道一个问题的答案--那就是 git 是什么东西？

简单来说，git 是一个版本工作系统，负责记录你的每一次修改。当然，这里是很笼统的说法；但是在这篇入门文章中我们不会进行深入讨论。

![](img/assets/git.jpg)

## 建立版本库，了解工作区

在使用git之前我们首先需要建立一个 git 版本库

通过在你需要建立的 git 版本库的文件夹目录执行

```bash
git init
```

这样你的git版本库就建立好了。

这个时候你似乎看不出来文件有什么变化，但是如果你打开隐藏文件显示就会看到我们的目录里面多了一个名叫 .git 的文件夹。这里我们不需要知道太多，只需要知道我们的 git 版本库已经建立好了。

我们现在需要知道的是，当前我们所有在这个目录所做的修改（不包括 .git ）都会被存放上图中的工作区。

工作区一般用来存放我们当前正在执行的任务。

## 暂存区

需要清楚的是，当前我们所有做的修改并没有进行版本记录，所以我们需要执行下一步来把我们的修改放入版本记录之中。

首先我们需要执行一个命令（这个命令很有用，可以帮助我们知道当前 git 版本库的状态）

```bash
git stauts
```

如果不出意外，你可以看到命令行会会这样输出

```bash
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	LICENSE

no changes added to commit (use "git add" and/or "git commit -a")
```

似乎和你们的不太一样，但是没关系，我们当前需要注意的只有这个地方

```bash
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	LICENSE
```

这是在告诉我们尚未将文件提交到暂存区，也就是上图中的 stage 部分。

等等，我们是不是说到一个新的词语了，暂存区，这是个什么玩意？为什么要设计这么一个玩意而不直接提交到版本库里面呢？

如果要回答这些问题的话，那么这篇文章就不应该叫《相对简单的 git 入门》，而应该叫《深入浅出 git》；所以在这里我们只能简单说明一下暂存区的作用。

想象一下，如果你同时修改了 a 和 b 两个文件，但是你提交的时候只想提交 a ，那么这个时候你只需要选择单独将 a 放入暂存区然后提交就可以了。

回归正题，现在我们需要将 LICENSE 文件提交到暂存区，所以就可以执行命令

```bash
git add ./LICENSE
```

来将文件放入暂存区。

也许你会问那么，之前的

```bash
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt
```

是什么意思呢？在这里具体指的是我们修改了文件 readme.txt ，但是我们还没有将它放入暂存区；如果希望提交它的话，那么执行和LICENSE一样的流程就可以了

```bash
git add ./ readme.txt
```

顺便一提，如果打算提交当前目录的所有文件，可以通过执行 git add ./ 实现。

现在在让我们看一下 git 版本库的状态

```bash
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   LICENSE
	modified:   readme.txt
```

说明我们已经将所有的文件放入暂存区，接下来就需要我们进行提交到版本库了。

## 版本提交到

所以我们执行

```bash
git commit -m "understand how stage works"
```

这里解释一下，实际上执行的命令是 git commit ，而后面的 -m 是添加本次提交的备注，通常用来记录这次提交修改了什么，改正了那些方面的错误。

如果执行成功的话我们会被下面会有这样的输出

```bash
[master e43a48b] understand how stage works
 2 files changed, 2 insertions(+)
 create mode 100644 LICENSE
```

这代表我们已经将我们的修改放入到本地的版本管理仓库了。

至此，我们已经执行完一次 git 工作流了。

接下来让我们看一下我们 git 版本库的状态（输入 git status ）

```
On branch master
nothing to commit, working tree clean
```

这里不用说大家也能明白的吧。

如果你还是不明白的话，这里的意思是你当前的工作区和最后一次提交记录的版本库完全一样，赶快去工作吧（ bushi ）

到这里，我们的 git 入门教程就已经结束了；希望这篇教程能够帮助你在 git 的使用。
