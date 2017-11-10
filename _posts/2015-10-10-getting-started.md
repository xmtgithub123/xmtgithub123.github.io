---
layout: post
title: "Git"
author: "MengTing Xu"
categories: git
tags: [documentation,sample]
image:
  feature: forest.jpg
  teaser: forest-teaser.jpg
  credit:
  creditlink:
---

# 简介

Git是一个分布式版本控制／软件配置管理软件，原是Linux内核开发者Linus Torvalds为更好地管理Linux内核开发而设计。其主要的目的是用来记录软件源代码的历史状态，类似于SVN或CVS。

你可以利用git来追踪项目中的文件，并设置保存点。你可以和合作伙伴共享这些保存点，将他们的工作和你的工作合并，或者可以将整个工程或某些文件和之前的任何保存点进行对比，甚至恢复到早期的某个保存点。

由于Git有Linux这个干爹，理论上它是不能在Windows平台上运行的。不过强大的程序员总是不会轻易放过任何展现自己的机会。比如Git Bash这个程序就提供了在Windows平台上使用Linux环境的一套工具。在Git Bash上的所有命令行操作，都与Linux环境完全相同。

当然Git绝非仅能使用命令行来使用，eclipse里有插件EGit（搜索Market Place即可安装），其他比较常用的Git GUI工具有：

1. TortoiseGit (Windows 平台)
2. GitX (Mac 平台)

# 工作流程

Git的分布式体现在代码仓库有两种：本地仓库和远端仓库。所有的commit（设置保存点）都只会对本地仓库产生影响，而只有在开发者使用push将修改推送到远端仓库后，其他人才可以通过pull或fetch获取到该开发者的修改内容。所以，git的推荐工作方式是：
__经常commit，定期push__
，也就是尽量将commit细致化，以便于后期回退或者对比，而按时（每天或每周）定期向服务器推送所有改动。

Git中，文件状态变化方式如下：

其中的动作及对应的命令：

* add the file：将文件添加到git追踪系统，命令为 git add .
* remove the file：将文件从git追踪系统中删除，后续修改将不再记录，命令为 git rm /path/to/file
* stage the file：将文件的修改暂存（如果既不想合并，又不希望丢失自己的修改），命令为 git stage
* commit：设置保存点，命令为 git commit -a -m '本次提交的改动描述'

**请协助我们在工作中做好流程标准化**

# 应用场景

## 克隆远端工程

    git clone git@***.com:/path/to/repo

## 查看项目状态

{% highlight bash %}
# 查看历史记录
git log

# 查看工程状态
git status

# 对比master分支和代号为eb3c2d的保存点
git diff master eb3c2d
{% endhighlight %}

## 替换本地改动

如果你想放弃本地的改动，让某一个文件恢复到之前的某个版本，你可以这么做

    git checkout -- 文件名

如果你想丢弃掉所有的本地改动和提交，用服务器上的最新版替换

    git fetch origin
    git reset --hard origin/master

或者只是想恢复到本地的最新修改

    git reset --hard HEAD

## 新建工程

1. 新建远端仓库（需要预先配置ssh key）
{% highlight bash %}
ssh git@***-it.com

# 初始化远端仓库，注意，需要替换/path/to/repo为有意义的路径
git init --bare /path/to/repo
{% endhighlight %}

2. 配置新项目并推送至远端
{% highlight bash %}
git init

# 增加远端服务器地址，注意，需要将/path/to/repo替换为实际地址
git remote add origin git@***-it.com:/path/to/repo
git add .
git commit -m '1st import'

# 推送至远端
git push origin master
{% endhighlight %}

## Fetch/Rebase

Fetch/Rebase是使用git的最佳实践，它可以确保时间轴一致，并在多人开发时提高合并效率。一个典型的Fetch/Rebase流程为：

{% highlight bash %}
# 添加所有文件至git追踪系统
git add .

# -a表示删除文件自动提交，-m表示此次提交的解释
git commit -a -m '本次提交的改动描述'

# 从origin这个远端仓库把所有其他人的改动下载到本地
git fetch origin master

# 将远端仓库中的所有修改在本地“重放”进行合并
git rebase origin/master master

# 如果提示冲突，直接修改冲突文件后，继续，否则结束流程
# 重新添加已改动的文件
git add .
# 继续“重放”合并
git rebase --continue
{% endhighlight %}

## 分支修正

默认状态下，git始终使用master分支，但是有些特殊情况会导致git分支异常，这样的情况下，需要重新手动恢复。该功能请慎用

{% highlight bash %}
# 查看历史记录
git log

# 移动现有指针到某一次提交（这次提交的commit id是以eb3c开头）
git checkout master

# 合并至commit id为a862的保存点
git merge a862
{% endhighlight %}
