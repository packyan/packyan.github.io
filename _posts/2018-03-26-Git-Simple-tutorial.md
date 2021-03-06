﻿---
layout: post
title: Git简易教程
date: 2018-3-26
categories: blog
tags: [Git,Github]
description: git能很好的进行版本控制，而且入门比较简单，只要掌握基本的语句就可以用命令行进行控制。之前在Robomaster队伍里，使用的基本是版本号来进行版本控制，导致了不同队员可能有自己的版本，但是却在团队内不能及时得到更新交流。如果使用git上传到github不同分支，能很好地在团队内传播，效率得以提到。（但是私有仓库要小钱钱啊~老师可以报销不？）
---
标签（空格分隔）： Git  github

git能很好的进行版本控制，而且入门比较简单，只要掌握基本的语句就可以用命令行进行控制。之前在Robomaster队伍里，使用的基本是版本号来进行版本控制，导致了不同队员可能有自己的版本，但是却在团队内不能及时得到更新交流。如果使用git上传到github不同分支，能很好地在团队内传播，效率得以提到。（但是私有仓库要小钱钱啊~老师可以报销不？）

---
[猴子都能懂的git入门](https://backlog.com/git-tutorial/cn/intro/intro1_1.html)
## 本地管理步骤
1. `cd` 进入目录
2. `git init` 把当前目录编程git可管理的仓库
3. 假设建立了一个`readme.md`文件，通过`git add readme.md`添加文件到GIT仓库。
4. `git commit` 把文件提交到仓库 eg. `git commit -m "wrote a readme file"`
5. 如果commit 错了，需要修改提交过的注释 git commit --amend 一次后悔药
6. 多次后悔注释 需要使用rebase

```
git rebase -i HEAD~3 
#修改哪个就把那行的pick改成edit, i insert
#保存wq
git commit -amend 
#vim 修改注释，wq退出
git rebase --continue 
#修改完后退回最新版本
git push -f
#强制推送
```


> 把文件添加到版本库
首先这里再明确一下，所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，Git也不例外。版本控制系统可以告诉你每次的改动，比如在第5行加了一个单词“Linux”，在第8行删了一个单词“Windows”。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道。
不幸的是，Microsoft的Word格式是二进制格式，因此，版本控制系统是没法跟踪Word文件的改动的，前面我们举的例子只是为了演示，如果要真正使用版本控制系统，就要以纯文本方式编写文件。
因为文本是有编码的，比如中文有常用的GBK编码，日文有Shift_JIS编码，如果没有历史遗留问题，强烈建议使用标准的UTF-8编码，所有语言使用同一种编码，既没有冲突，又被所有平台所支持。

**git commit 要写备注**
> 简单解释一下git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。
嫌麻烦不想输入-m "xxx"行不行？确实有办法可以这么干，但是强烈不建议你这么干，因为输入说明对自己对别人阅读都很重要。实在不想输入说明的童鞋请自行Google，我不告诉你这个参数。
git commit命令执行成功后会告诉你，1个文件被改动（我们新添加的readme.txt文件），插入了两行内容（readme.txt有两行内容）。
为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件，比如：

```
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```

小结

现在总结一下今天学的两点内容：

初始化一个Git仓库，使用`git init`命令。

添加文件到Git仓库，分两步：

>第一步，使用命令 `git add <file>`，注意，可反复多次使用，添加多个文件；
>第二步，使用命令`git commit`，完成。


## 1.基本命令
1. `cd` 进入目录
2. `git init` 把当前目录编程git可管理的仓库
3. `git add *.md` 添加单个文件
4. `git add -A`添加全部文件
5. `git commit -m "commit connent"` 先要提交修改 再push
6. `git status` 查看是否还有未提交
7. `git log` 日志
8. 版本退回操作
- `git reset -har HEAD^` 退回一个
- `git reset -har HEAD^^` 退回两个
- `git reset -har HEAD~100` 退回多个
9. 第一次连接 远程仓库提交 `git remote add origin 复制的地址`
10. 关联仓库 `git push -u origin master`
11. 第二次后 远程仓库提交 `git push`
12. 要上传的仓库 `git remote add origin git@github.com:yourName/yourRepo.git`

## 2. 基本教程
主要分为以下步骤
1. `sudo apt-get isntall git`
2. 配置git账号`$ git config --global user.name "Your Name"`
`$ git config --global user.email "email@example.com"`
3. 安装ssh
4. `ssh-keygen -t rsa -C "your_email@youremail.com"` 本地创建**ssh key** 并添加到github中 在**Account Settings**里
5. 验证是否添加成功 `ssh -T git@github.com`
6. 成功后就可以进行git操作了，参考第一章内容，初始化

[廖雪峰老师网站git入门](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013743858312764dca7ad6d0754f76aa562e3789478044000)

[简明入门教程](http://www.runoob.com/w3cnote/git-guide.html)

## 如何删除GIT中的.DS_Store
### .DS_Store 是什么
使用 Mac 的用户可能会注意到，系统经常会自动在每个目录生成一个隐藏的 .DS_Store 文件。.DS_Store(英文全称 Desktop Services Store)是一种由苹果公司的Mac OS X操作系统所创造的隐藏文件，目的在于存贮目录的自定义属性，例如文件们的图标位置或者是背景色的选择。相当于 Windows 下的 desktop.ini。

### 删除 .DS_Store
如果你的项目中还没有自动生成的 .DS_Store 文件，那么直接将 .DS_Store 加入到 .gitignore 文件就可以了。如果你的项目中已经存在 .DS_Store 文件，那就需要先从项目中将其删除，再将它加入到 .gitignore。如下：

> 删除项目中的所有.DS_Store。这会跳过不在项目中的 .DS_Store
1. `find . -name .DS_Store -print0 | xargs -0 git rm -f --ignore-unmatch`
将 .DS_Store 加入到 .gitignore
2. `echo .DS_Store >> ~/.gitignore`
更新项目
3. `git add --all`
4. `git commit -m '.DS_Store banished!'`

如果你只需要删除磁盘上的 .DS_Store，可以使用下面的命令来删除当前目录及其子目录下的所有.DS_Store 文件:

`find . -name '*.DS_Store' -type f -delete`

### 禁用或启用自动生成
禁止.DS_store生成：
`defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool TRUE`

恢复.DS_store生成：恢复.DS_store生成：
`defaults delete com.apple.desktopservices DSDontWriteNetworkStores`

参考: [mac显示隐藏文件](https://www.jianshu.com/p/a1c2495b02aa)
作者：iOSReverse
链接：https://www.jianshu.com/p/fdaa8be7f6c3
來源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

1. 先删除原有的.DS_Store：
`find . -name .DS_Store -print0 | xargs -0 git rm -f --ignore-unmatch`
命令解释：在当前文件夹以及当前文件夹的子文件夹中找到所有的.DS_Store文件，并将找到的文件通过管道传给xargs来处理。注意几个参数的理解： 
-print0：在find后不添加换行符（-print默认会添加换行符） 
-0：将管道送来的字符串当做普通的字符串，不做任何转义处理。

2. 建立.gitignore文件
vi .gitignore。然后添加.DS_Store作为忽略：
`.DS_Store`

3. 提交到git
`git add .gitignore`
`git commit -m 'delete .DS_Store'`

