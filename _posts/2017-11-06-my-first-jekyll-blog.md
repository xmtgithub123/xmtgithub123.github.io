---
layout: post
title: "使用github和jekyll设置自己的个人网站"
author: "Paul Le"
categories: journal
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
   1. [GitHub](#github搭建)
   2. [本地安装](#local-installation)
   3. [目录结构](#directory-structure)
   4. [从头开始](#starting-from-scratch)


### 什么是jekyll

Jekyll是一个简单的博客形态和静态站点生产机器.它有一个模版目录，其中包含原始文本格式的文档，通过一个转换器（如 Markdown）和我们的 Liquid 渲染器转化成一个完整的可发布的静态网站,你可以发布在任何你喜爱的服务器上.Jekyll 也可以运行在 GitHub Page 上,也就是说,你可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站,而且是完全免费的.你可以去[中文官网 To Lean Jekyll](http://jekyllcn.com)上学习它!

## 安装

### github搭建

要用github pages, 首先注册[github帐号](https://github.com/),
Head over to the `_posts` directory to view all the posts that are currently on the website, and to see examples of what post files generally look like. You can simply just duplicate the template post and start adding your own content.

### Local Installation

For a full local installation of Lagrange, [download your own copy of Lagrange](https://github.com/LeNPaul/Lagrange/archive/gh-pages.zip) and unzip it into it's own directory. From there, open up your favorite command line tool, and enter `jekyll serve`. Your site should be up and running locally at [http://localhost:4000](http://localhost:4000).

### Directory Structure

If you are familiar with Jekyll, then the Lagrange directory structure shouldn't be too difficult to navigate. The following some highlights of the differences you might notice between the default directory structure. More information on what these folders and files do can be found in the [Jekyll documentation site](https://jekyllrb.com/docs/structure/).

```bash
Lagrange

├── _data                      # Data files
|  └── authors.yml             # For managing multiple authors
|  └── settings.yml            # Theme settings and custom text
├── _includes                  # Theme includes
├── _layouts                   # Theme layouts (see below for details)
├── _posts                     # Where all your posts will go
├── assets                     # Style sheets and images are found here
|  ├── css
|  |  └── main.css
|  |  └── syntax.css
|  └── img
├── menu                       # Menu pages
├── _config.yml                # Site build settings
└── index.md                   # Home page
```

### Starting From Scratch

To completely start from scratch, simply delete all the files in the `_posts`, and `menu` folder, and add your own content. You may also replace the `README.md` file with your own README. Everything in the `_data` folder can be edited to suit your needs.
### Example Content

[Text and Formatting]({{ site.github.url }}{% post_url 2015-09-09-Text-Formatting %})

### Questions?

This theme is completely free and open source software. You may use it however you want, as it is distributed under the [MIT License](http://choosealicense.com/licenses/mit/). If you are having any problems, any questions or suggestions, feel free to [tweet at me](https://twitter.com/intent/tweet?text=My%question%about%Lagrange%is:%&amp;via=paululele), or [file a GitHub issue](https://github.com/lenpaul/lagrange/issues/new).
