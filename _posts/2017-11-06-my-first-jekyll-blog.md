---
layout: post
title: "使用github和jekyll设置自己的个人网站"
author: "Paul Le"
categories: git
tags: [documentation,sample]
image:
  feature: cutting.jpg
  teaser: cutting-teaser.jpg
  credit:
  creditlink: ""
---

一直想写个属于自己的博客，又由于自己平时太懒也没啥心思弄，所以一直搁置着，直到最近...突然好想搭建个个人主页网站，自己对个人主页的要求并不是很高，简洁大方清晰就行了，为了方便，所以最后选择了使用Github Page.

## 开始

### 目录

1. 介绍
   1. [什么是Jekyll](#什么是jekyll)
2. 安装
   1. [GitHub搭建](#github搭建)
   2. [Jekyll本地环境搭建](#jekyll本地环境搭建)
   3. [目录结构](#目录结构)
   4. [下载jekyll模板](#下载jekyll模板)


### 什么是jekyll

Jekyll是一个简单的博客形态和静态站点生产机器.它有一个模版目录，其中包含原始文本格式的文档，通过一个转换器（如 Markdown）和我们的 Liquid 渲染器转化成一个完整的可发布的静态网站,你可以发布在任何你喜爱的服务器上.Jekyll 也可以运行在 GitHub Page 上,也就是说,你可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站,而且是完全免费的.你可以去[中文官网 To Lean Jekyll](http://jekyllcn.com)上学习它!

## 安装

### github搭建

要用github pages, 首先注册[github帐号](https://github.com/),github是个非常优秀的代码仓库,里面有很多优秀的开源项目,你可以找到你想要知道的以及对你非常有益的代码,你可以分享也可以学习,这个我的Github:[https://github.com/xmtgithub123](https://github.com/xmtgithub123),欢迎大家fork && star,哈哈哈！

### (1).下面是github的操作流程图:

<img src="/assets/img/githubReg.png" alt="">

### jekyll本地环境搭建

#### (1).安装Ruby环境

网上教程蛮多的,大家可以参考网上的教程按步骤来实现,在这里放一个ruby的官网链接 [RubyInstaller](https://rubyinstaller.org/downloads/),下载->安装->检测,打开命令工具:

<pre>检测ruby是否已经安装完毕,输入:ruby -v  //若有版本号出现则说明已经安装成功,反之安装失败</pre>

#### (2).安装RubyDevKit环境

还是在个链接下,下载rubyDevkit安装包，安装在C:\RubyDevKit下,初始化:

<pre>ruby dk.rb init</pre>

<pre>ruby dk.rb install</pre>

#### (3).安装Jekyll环境

打开命令行输入命令如下：

<pre>gem install jekyll</pre>

而这个时候命令工具中会报错:

<pre>ERROR:  Could not find a valid gem 'jekyll' (>= 0), here is why:
        Unable to download data from http://ruby.taobao.org/ - bad response Not Found 404 (http://ruby.taobao.org/latest_specs.4.8.gz)
</pre>

这个错误说明你之前安装包的时候用了淘宝镜像,所以会报错;此时,你应该进行镜像更换:

<pre>gem source -r https://ruby.taobao.org/ (移除淘宝镜像)</pre>

<pre>gem source -a https://rubygems.org/ (添加新镜像)</pre>

安装成功,可以通过`gem source`来查看当前镜像:

<pre> *** CURRENT SOURCES ***
 https://rubygems.org/   //出现如下信息则说明安装成功</pre>

另一种情况是,安装rubygem 的镜像也有问题,也会提示如下信息:

<pre>
    ERROR:  Could not find a valid gem 'jekyll' (>= 0), here is why:
        Unable to download data from http://ruby.taobao.org/ - SSL_connetc retuned=1 errn=0 state=SSLv3 server certificate B:certificate verify failed(https://api.rubygems.org/specs.4.8.gz)
</pre>

解决方法如下:

<pre>删除原gem源: gem sources --remove https://rubygems.org/</pre>

<pre>添加国内源: gem sources -a http://gems.ruby-china.org/</pre>

安装成功,可以通过`gem source`来查看当前镜像:

<pre> *** CURRENT SOURCES ***
 http://gems.ruby-china.org/g/   //出现如下信息则说明安装成功</pre>

以上,安装完毕之后, 执行:

<pre>
> gem install jekyll bundler
> jekyll new my-awesome-site
> cd my-awesome-site
> bundle install
> bundle exec jekyll serve
</pre>

最后你可以通过打开浏览器 `http://localhost:4000` 来访问一个新的jekyll项目啦.

<img src="/assets/img/jekyll-pic.jpg" alt="">

### 目录结构

If you are familiar with Jekyll, then the Lagrange directory structure shouldn't be too difficult to navigate. The following some highlights of the differences you might notice between the default directory structure. More information on what these folders and files do can be found in the [Jekyll documentation site](https://jekyllrb.com/docs/structure/).

```bash

├── _data                      # 格式化好的网站数据
|  └── authors.yml             # authors的一些基础信息变量
|  └── settings.yml            
├── _includes                  # 加载这些包含部分到你的布局或文章中方便重用
├── _layouts                   # 布局-包裹外部的模板
├── _posts                     # 放置你的文章
├── assets                     # 样式或图片文件
|  ├── css
|  |  └── main.css
|  |  └── syntax.css
|  └── img
├── menu                       # Menu pages
├── _config.yml                # 网站配置数据
└── index.md                   
```
### 下载jekyll模板

jekyll 模板地址：[http://jekyllthemes.org/](http://jekyllthemes.org/)

下载自己喜欢的风格,拷贝到你的项目里,然后`git commit `到你的github上,`git push origin master`后,就可以在自己的github pages上看到效果啦.